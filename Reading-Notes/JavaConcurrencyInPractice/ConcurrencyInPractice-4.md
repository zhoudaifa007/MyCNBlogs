<h2>《Java 并发编程实战》 :books: </h2> 
> Brian Goetz、 Tim Peierls、 Joshua Bloch、 Joseph Bowbeer、 David Holmes、 Doug Lea 著   

```html
 49.显式锁：
   与内置锁相比，显式的 Lock 提供了一些扩展功能，在处理锁的不可用性方面有着更高的灵活性，并且对队列行有着更好的控制。
但 ReentrantLock 不能完全替代 synchronized，只有在 synchronized 无法满足需求时，才应该使用它。
   读-写锁允许多个读线程并发地访问被保护的对象，当访问以读取操作为主的数据结构时，它能提高程序的可伸缩性。
   
 50.Lock 与 ReentrantLock：
   (1) Lock 接口：
   public interface Lock {
      void lock();
      void lockInterruptibly() throws InterruptedException;
      boolean tryLock();
      boolean tryLock(long timeout, TimeUnit unit) throws InterruptedException;
      void unlock();
      Condition newCondition();
   }
   
   (2) 轮询锁与定时锁：可定时的与可轮询的锁获取模式是由 tryLock 方法实现的，与无条件的锁获取模式相比，它具有更完善的
  错误恢复机制。
    通过 tryLock 来避免锁顺序死锁：
    public boolean transferMoney(Account fromAcct, Account toAcct, DollarAmount amount, 
                  long timeout, TimeUnit unit) throws InsufficientFundsException, InterruptedException {
        long fixedDelay = getFixedDelayComponentNanos(timeout, unit);
        long randMod = getRandomDelayModulusNanos(timeout, unit);   
        long stopTime = System.nanoTime() + unit.toNanos(timeout);

        while (true) {
           if (fromAcct.lock.tryLock()) {
              try {
                 if (toAcct.lock.tryLock()) {
                    try {
                       if (fromAcct.getBalance().compareTo(amount) < 0)
                          throw new InsufficientFundsException();
                       else {
                          fromAcct.debit(amount);
                          toAcct.credit(amount);
                          return true;
                       }
                    } finally {
                       toAcct.lock.unlock();
                    }
                 }
              } finally {
                 fromAcct.lock.unlock();
              }
           }
           if(System.nanoTime() < stopTime)
              return false;
           NANOSECONDS.sleep(fixedDelay + rnd.nextLong() % randMod);
        }
    }

  (3) 可中断的锁获取操作：
    public boolean sendOnSharedLine(String message) throws InterruptedException {
       lock.lockInterruptibly();
       try {
          return cancellableSendOnSharedLine(message);
       } finally {
          lock.unlock();
       }
    }

    private boolean cancellableSendOnSharedLine(String message) throws InterruptedException { ... }

 (4) 非块结构的加锁。
 
 51.性能考虑因素：性能是一个不断变化的指标，如果在昨天的测试基准中发现 X 比 Y 更快，那么在今天就可能已经过时了。
  公平性：在 ReentrantLock 的构造函数中提供了两种公平性选择：创建一个非公平的锁(默认)或者一个公平的锁。
  在 synchronized 和 ReentrantLock 之间进行选择：在一些内置锁无法满足需求的情况下，ReentrantLock 可以作为
一种高级工具。当需要一些高级功能时才应该使用 ReentrantLock，这些功能包括：可定时的、可轮询的与可中断的锁获取
操作，公平队列，以及非块结构的锁。否则，还是应该优先使用 synchronized。

 52.读-写锁：
  ReadWriteLock 接口：
  public interface ReadWriteLock {
     Lock readLock();
     Lock writeLock();
  }
  ReadWriteLock 中的一些可选实现包括：释放优先；读线程插队；重入性；降级；升级。
  用读—写锁来包装 Map:
  public class ReadWriteMap<K, V> {
     private final Map<K, V> map;
     private final ReadWriteLock lock = new ReentrantReadWriteLock();
     private final Lock r = lock.readLock();
     private final Lock w = lock.writeLock();

     public ReadWriteMap(Map<K, V> map) {
        this.map = map;
     }

     public V put(K key, V value) {
        w.lock();
        try {
           return map.put(key, value);
        } finally {
           w.unlock();
        }
     }
     // 对 remove(), putAll(), clear() 等方法执行相同的操作

     public V get(Object key) {
        r.lock();
        try {
           return map.get(key);
        } finally {
           r.unlock();
        }
     }
     // 对其他只读的 Map 方法执行相同的操作
  }
  
 53.构建自定义的同步工具：
  要实现一个依赖状态的类——如果没有满足依赖状态的前提条件，那么这个类的方法必须阻塞，那么最好的方式是基于现有的
库类来构建，例如 Semaphore.BlockingQueue 或 CountDownLatch。然而，有时候现有的库类不能提供足够的功能，在这种
情况下，可以使用内置的条件队列、显示的 Condition 对象或者 AbstractQueuedSynchronizer 来构建自己的同步器。
内置条件队列与内置锁是紧密绑定在一起的，这是因为管理状态依赖性的机制必须与确保状态一致性的机制关联起来。同样，
显示的 Condition 与显示的 Lock 也是紧密地绑定到一起的，并且与内置条件队列相比，还提供了一个扩展的功能集，包括
每个锁对应于多个等待线程集，可中断或不可中断的条件等待，公平或非公平的队列操作，以及基于时限的等待。

 54.原子变量与非阻塞同步机制：
  非阻塞算法通过底层的并发原语(例如比较并交换而不是锁)来维持线程的安全性。这些底层的原语通过原子变量类向外公开，
这些类也用做一种“更好的 volatile 变量”，从而为整数和对象引用提供原子的更新操作。
  非阻塞算法在设计和实现时非常困难，但通常能够提供更高的伸缩性，并能够更好的防止活跃性故障的发生。在 JVM 从一个
版本升级到下一个版本的过程中，并发性能的主要提升都来自于(在 JVM 内部以及平台类库中)对阻塞算法的使用。

  

```