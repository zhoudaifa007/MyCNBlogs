 <h2>阿里 Java 面经 :memo: </h2> 
 
 > 牛客上扒的，具体出处不详。如有侵权，联系必删。赠人玫瑰，手留余香，向大佬致敬。
 
```html
1.Java 的四个基本特性(抽象、封装、继承、多态)，对多态的理解(多态的实现方式)以及在项目中哪些地方用到多态？
  1) Java 的四个基本特性:
   (1) 抽象：抽象是将一类对象的共同特征总结出来构造类的过程，包括数据抽象和行为抽象两方面。
抽象只关注对象有哪些属性和行为，并不关注这些行为的细节是什么。
   (2) 继承：继承是从已有类得到继承信息创建新类的过程。提供继承信息的类被称为父类(超类、基类)；
得到继承信息的类被称为子类(派生类)。继承让变化中的软件系统有了一定的延续性，同时继承也是封装程序中可变因素的重要手段。
   (3) 封装：通常认为封装是把数据和操作数据的方法绑定起来，对数据的访问只能通过已定义的接口。
面向对象的本质就是将现实世界描绘成一系列完全自治、封闭的对象。我们在类中编写的方法就是对实现细节的一种封装；
我们编写一个类就是对数据和数据操作的封装。可以说，封装就是隐藏一切可隐藏的东西，只向外界提供最简单的编程接口。
   (4) 多态性是指允许不同子类型的对象对同一消息作出不同的响应。

  2) 多态的理解(多态的实现方式)。
   (1) 方法重载(overload): 实现的是编译时的多态性(也称为前绑定)。
   (2) 方法重写(override): 实现的是运行时的多态性(也称为后绑定)。运行时的多态是面向对象最精髓的东西。
   (3) 要实现多态需要做两件事：
     ① 方法重写(子类继承父类并重写父类中已有的或抽象的方法)；
     ② 对象造型(用父类型引用引用子类型对象，这样同样的引用调用同样的方法就会根据子类对象的不同而表现出不同的行为)。

  3) 项目中对多态的应用。
   举一个简单的例子，在物流信息管理系统中，有两种用户：订购客户和卖房客户，两个客户都可以登录系统，他们有相
同的方法 Login，但登陆之后他们会进入到不同的页面，也就是在登录的时候会有不同的操作，两种客户都继承父类的
Login 方法，但对于不同的对象，拥有不同的操作。

2.面向对象和面向过程的区别？用面向过程可以实现面向对象吗？那是不是不能面向对象？
  面向对象和面向过程的区别：
   (1) 面向过程就像是一个细心的管家，事无具细的都要考虑到。而面向对象就像是个家用电器，你只需要知道他的功能，
不需要知道它的工作原理。
   (2) "面向过程" 是一种是事件为中心的编程思想。就是分析出解决问题所需的步骤，然后用函数把这些步骤实现，
并按顺序调用。面向对象是以“对象”为中心的编程思想。
   (3) 简单的举个例子：汽车发动、汽车到站。
    这对于“面向过程”来说，是两个事件，汽车启动是一个事件，汽车到站是另一个事件，面向过程编程的过程中我们关心的是事件，
而不是汽车本身。针对上述两个事件，形成两个函数，之后依次调用。
    然而这对于面向对象来说，我们关心的是汽车这类对象，两个事件只是这类对象所具有的行为。
而且对于这两个行为的顺序没有强制要求。

3.重载和重写，如何确定调用哪个函数？
   (1) 重载：重载发生在同一个类中，同名的方法如果有不同的参数列表(参数类型不同、参数个数不同或者二者都不同)则视为重载。
   (2) 重写：重写发生在子类与父类之间，重写要求子类被重写方法与父类被重写方法有相同的返回类型，比父类被重写方法更好
访问，不能比父类被重写方法声明更多的异常(里氏代换原则)。根据不同的子类对象确定调用的那个方法。

4.面向对象开发的六个基本原则(单一职责、开放封闭、里氏替换、依赖倒置、合成聚合复用、接口隔离)，迪米特法则。
在项目中用过哪些原则?
  1) 六个基本原则：
   (1) 单一职责：一个类只做它该做的事情(高内聚)。在面向对象中，如果只让一个类完成它该做的事，而不涉及与它无关的
领域就是践行了高内聚的原则，这个类就只有单一职责。
   (2) 开放封闭：软件实体应当对扩展开放，对修改关闭。要做到开闭有两个要点：
     ① 抽象是关键，一个系统中如果没有抽象类或接口系统就没有扩展点；
     ② 封装可变性，将系统中的各种可变因素封装到一个继承结构中，如果多个可变因素混杂在一起，系统将变得复杂而换乱。
   (3) 里氏替换：任何时候都可以用子类型替换掉父类型。子类一定是增加父类的能力而不是减少父类的能力，因为子类比父
类的能力更多，把能力多的对象当成能力少的对象来用当然没有任何问题。
   (4) 依赖倒置：面向接口编程。(该原则说得直白和具体一些就是声明方法的参数类型、方法的返回类型、变量的引用类型时，
尽可能使用抽象类型而不用具体类型，因为抽象类型可以被它的任何一个子类型所替代)。
   (5) 合成聚和复用：优先使用聚合或合成关系复用代码。
   (6) 接口隔离：接口要小而专，绝不能大而全。臃肿的接口是对接口的污染，既然接口表示能力，那么一个接口只应该描述
一种能力，接口也应该是高度内聚的。
  2) 迪米特法则：迪米特法则又叫最少知识原则，一个对象应当对其他对象有尽可能少的了解。
  3) 项目中用到的原则: 单一职责、开放封闭、合成聚合复用(最简单的例子就是 String 类)、接口隔离。

5.static 和 final 的区别和用途？
  (1) static
   修饰变量：静态变量随着类加载时被完成初始化，内存中只有一个，且 JVM 也只会为它分配一次内存，所有类共享静态变量。
   修饰方法：在类加载的时候就存在，不依赖任何实例；static 方法必须实现，不能用 abstract 修饰。
   修饰代码块：在类加载完之后就会执行代码块中的内容。
   父类静态代码块 -> 子类静态代码块 -> 父类非静态代码块 -> 父类构造方法 -> 子类非静态代码块 -> 子类构造方法
  (2) final
   修饰变量：
     编译期常量：类加载的过程完成初始化，编译后带入到任何计算式中。只能是基本类型。
     运行时常量：基本数据类型或引用数据类型。引用不可变，但引用的对象内容可变。
     修饰方法：不能被继承，不能被子类修改。
     修饰类：不能被继承。
     修饰形参：final 形参不可变。

6.Hash Map 和 Hash Table 的区别，Hash Map 中的 key 可以是任何对象或数据类型吗？HashTable 是线程安全的么？
   (1) Hash Map 和 Hash Table 的区别：
    Hashtable 的方法是同步的，HashMap 未经同步，所以在多线程场合要手动同步 HashMap 这个区别就像 Vector 和
ArrayList 一样。
    Hashtable 不允许 null 值( key 和 value 都不可以)，HashMap 允许 null 值( key 和 value 都可以)。
    两者的遍历方式大同小异，Hashtable 仅仅比 HashMap 多一个 elements 方法。
    Hashtable 和 HashMap 都能通过 values() 方法返回一个 Collection，然后进行遍历处理。
    两者也都可以通过 entrySet() 方法返回一个 Set， 然后进行遍历处理。
    HashTable 使用 Enumeration，HashMap 使用 Iterator。
    哈希值的使用不同，Hashtable 直接使用对象的 hashCode。而 HashMap 重新计算 hash 值，而且用于代替求模。
    Hashtable 中 hash 数组默认大小是 11，增加的方式是 old*2 + 1。HashMap 中 hash 数组的默认大小是 16，
而且一定是 2 的指数。
    HashTable 基于 Dictionary 类，而 HashMap 基于 AbstractMap 类。
   (2) Hash Map 中的 key 可以是任何对象或数据类型吗?
    可以为 null，但不能是可变对象，如果是可变对象的话，对象中的属性改变，则对象 HashCode 也进行相应的改变，
导致下次无法查找到已存在 Map 中的数据。
    如果可变对象在 HashMap 中被用作键，那就要小心在改变对象状态的时候，不要改变它的哈希值了。我们只需要保证
成员变量的改变能保证该对象的哈希值不变即可。
   (3) HashTable 是线程安全的么？
    HashTable 是线程安全的，其实现是在对应的方法上添加了 synchronized 关键字进行修饰，由于在执行此方法的时候
需要获得对象锁，则执行起来比较慢。所以现在如果为了保证线程安全的话，使用 ConcurrentHashMap。

7.HashMap 和 ConcurrentHashMap 的区别，ConcurrentHashMap 线程安全吗? ConcurrentHashMap 如何保证线程安全？
   (1) HashMap 和 ConcurrentHashMap 区别？
    HashMap 是非线程安全的，CurrentHashMap 是线程安全的。
    ConcurrentHashMap 将整个 Hash 桶进行了分段 Segment，也就是将这个大的数组分成了几个小的片段 Segment，
而且每个小的片段 Segment 上面都有锁存在，那么在插入元素的时候就需要先找到应该插入到哪一个片段 Segment，
然后再在这个片段上面进行插入，而且这里还需要获取 Segment 锁。
    ConcurrentHashMap 让锁的粒度更精细一些，并发性能更好。
   (2) ConcurrentHashMap 线程安全吗，ConcurrentHashMap 如何保证线程安全？
    HashTable 容器在竞争激烈的并发环境下表现出效率低下的原因是所有访问 HashTable 的线程都必须竞争同一把锁，
那假如容器里有多把锁，每一把锁用于锁容器其中一部分数据，那么当多线程访问容器里不同数据段的数据时，线程间就
不会存在锁竞争，从而可以有效的提高并发访问效率，这就是 ConcurrentHashMap 所使用的锁分段技术，首先将数据
分成一段一段的存储，然后给每一段数据配一把锁，当一个线程占用锁访问其中一个段数据的时候，其他段的数据也能
被其他线程访问。
    get 操作的高效之处在于整个 get 过程不需要加锁，除非读到的值是空的才会加锁重读。get 方法里将要使用的共享变量
都定义成 volatile，如用于统计当前 Segement 大小的 count 字段和用于存储值的 HashEntry 的 value。
定义成 volatile 的变量，能够在线程之间保持可见性，能够被多线程同时读，并且保证不会读到过期的值，
但是只能被单线程写(有一种情况可以被多线程写，就是写入的值不依赖于原值)，
在 get 操作里只需要读不需要写共享变量 count 和 value，所以可以不用加锁。
    put 方法首先定位到 Segment，然后在 Segment 里进行插入操作。插入操作需要经历两个步骤，第一步判断是否需要
对 Segment 里的 HashEntry 数组进行扩容，第二步定位添加元素的位置然后放在 HashEntry 数组里。

8.因为别人知道源码怎么实现的，故意构造相同 hash 的字符串进行攻击，怎么处理？那 jdk7 怎么办？
   1) 怎么处理构造相同 hash 的字符串进行攻击?
    当客户端提交一个请求并附带参数的时候，Web 应用服务器会把我们的参数转化成一个 HashMap 存储，
这个 HashMap 的逻辑结构如下：key1 --> value1;
    但是物理存储结构是不同的，key 值会被转化成 hashcode，这个 hashcode 会被转成数组的下标：0 --> value1；
    不同的 string 就会产生相同 hashcode 而导致碰撞，碰撞后的物理存储结构可能如下：0 --> value1 --> value2;
   2) 怎么处理？
     (1) 限制 Post 和 Get 的参数个数，越少越好;
     (2) 限制 Post 数据包的大小;
     (3) WAF。
   3) Jdk7 如何处理 hashcode 字符串攻击？
    HashMap 会动态的使用一个专门的 treemap 实现来替换掉它。

9.String、StringBuffer、StringBuilder 以及对 String 不变性的理解。
   (1) String、StringBuffer、StringBuilder。
    都是 final 类, 都不允许被继承;
    String 长度是不可变的, StringBuffer、StringBuilder 长度是可变的;
    StringBuffer 是线程安全的, StringBuilder 不是线程安全的，但它们两个中的所有方法都是相同的，
StringBuffer 在 StringBuilder 的方法之上添加了 synchronized 修饰，保证线程安全。
    StringBuilder 比 StringBuffer 拥有更好的性能。
    如果一个 String 类型的字符串，在编译时就可以确定是一个字符串常量，则编译完成之后，
字符串会自动拼接成一个常量。此时 String 的速度比 StringBuffer 和 StringBuilder 的性能好的多。
   (2) String 不变性的理解。
    String 类是被 final 进行修饰的，不能被继承。
    在用 + 号链接字符串的时候会创建新的字符串。
    String s = new String("Hello world"); 可能创建两个对象也可能创建一个对象。
    如果静态区中有 “Hello world” 字符串常量对象的话，则仅仅在堆中创建一个对象。
    如果静态区中没有 “Hello world” 对象，则堆上和静态区中都需要创建对象。
    在 java 中, 通过使用 "+" 符号来串联字符串的时候, 
实际上底层会转成通过 StringBuilder 实例的 append() 方法来实现。

10.String 有重写 Object 的 hashCode 和 toString 吗？如果重写 equals 不重写 hashCode 会出现什么问题？
   (1) String 有重写 Object 的 hashCode 和 toString 吗？
     String 重写了 Object 类的 hashCode 和 toString 方法。
     当方法 equals 被重写时，通常有必要重写 hashCode 方法，以维护 hashCode 方法的常规协定，
该协定相对等的两个对象必须有相同的 hashCode。
     object1.equals(object2) 为 true 时，object1.hashCode() == object2.hashSode() 为 true。
     object1.hashCode() == object2.hashCode() 为 false 时，object1.equals(object2) 必定为 false。
     object1.hashCode() == object2.hashCode() 为 true 时，object1.equals(object2) 不一定为 true。
   (2) 重写 equals 不重写 hashCode 会出现什么问题？
      在存储散列集合时(如 Set 类)，如果原对象.equals(新对象)，但没有对 hashCode 重写，即两个对象拥有
不同的 hashCode，则在集合中将存储两个值相同的对象，从而导致混淆。因此在重写 equals 方法时，必须重写 hashCode 方法。

11.Java 序列化，如何实现序列化和反序列化，常见的序列化协议有哪些？
   1) Java 序列化定义：将那些实现了 Serializable 接口的对象转换成一个字节序列，并能够在以后将这个字节序列
完全恢复为原来的对象，序列化可以弥补不同操作系统之间的差异。
   2) Java 序列化的作用：Java 远程方法调用 (RMI)；对 JavaBeans 进行序列化。
   3) 如何实现序列化和反序列化？
    (1) 实现序列化方法：
     实现 Serializable 接口。
     该接口只是一个可序列化的标志，并没有包含实际的属性和方法。
     如果不在改方法中添加 readObject() 和 writeObject() 方法，则采取默认的序列化机制。如果添加了这两个
方法之后还想利用 Java 默认的序列化机制，则在这两个方法中分别调用 defaultReadObject() 和
defaultWriteObject() 两个方法。
     为了保证安全性，可以使用 transient 关键字进行修饰不必序列化的属性。因为在反序列化时，private 修饰
的属性也能查看到。
     实现 ExternalSerializable 方法。
     自己对要序列化的内容进行控制，控制哪些属性能被序列化，哪些不能被序列化。
    (2) 反序列化：
    实现 Serializable 接口的对象在反序列化时不需要调用对象所在类的构造方法，完全基于字节。
    实现 ExternalSerializable 接口的方法在反序列化时会调用构造方法。
    (3) 注意事项：
    被 static 修饰的属性不会被序列化。
    对象的类名、属性都会被序列化，方法不会被序列化。
    要保证序列化对象所在类的属性也是可以序列化的。
    当通过网络、文件进行序列化时，必须按照写入的顺序读取对象。
    反序列化时必须有序列化对象的 class 文件。
    最好显示的声明 serializableID，因为在不同的 JVM 之间，默认生成 serializableID 可能不同，
可能造成反序列化失败。
   4) 常见的序列化协议有哪些？
    (1) COM 主要用于 Windows 平台，并没有真正实现跨平台，另外 COM 的序列化的原理利用了编译器中虚表，
使得其学习成本巨大。
    (2) CORBA 是早期比较好的实现了跨平台，跨语言的序列化协议。CORBA 的主要问题是参与方过多带来的版本
过多，版本之间的兼容性较差，以及使用复杂晦涩。
    (3) XML & SOAP：
    XML 是一种常用的序列化和反序列化协议，具有跨机器、跨语言等优点。
    SOAP(Simple Object Access Protocol) 是一种被广泛应用的，基于 XML 为序列化和反序列化协议的结构化
消息传递协议。SOAP 具有安全、可扩展、跨语言、跨平台并支持多种传输层协议。
    (4) JSON(JavaScript Object Notation):
    这种 Associative array 格式非常符合工程师对对象的理解。
    它保持了 XML 的人眼可读(Human-readable)的优点。
    相对于 XML 而言，序列化后的数据更加简洁。
    它具备 JavaScript 的先天性支持，所以被广泛应用于 Web Browser 的应用场景中，是 Ajax 的事实标准协议。
    与 XML 相比，其协议比较简单，解析速度比较快。
    松散的 Associative array 使得其具有良好的可扩展性和兼容性。
    (5) Thrift 是 Facebook 开源提供的一个高性能，轻量级 RPC 服务框架，其产生正是为了满足当前大数据量、
分布式、跨语言、跨平台数据通讯的需求。Thrift 在空间开销和解析性能上有了比较大的提升，对于性能要求比较高
的分布式系统，它是一个优秀的 RPC 解决方案。但是由于 Thrift 的序列化被嵌入到 Thrift 框架里面，Thrift 框架
本身并没有透出序列化和反序列化接口，这导致其很难和其他传输层协议共同使用。
    (6) Protobuf 具备了优秀的序列化协议的所需的众多典型特征。
    标准的 IDL 和 IDL 编译器，这使得其对工程师非常友好。
    序列化数据非常简洁，紧凑，与 XML 相比，其序列化之后的数据量约为 1/3 到 1/10。
    解析速度非常快，比对应的 XML 快约 20 - 100 倍。
    提供了非常友好的动态库，使用非常简洁，反序列化只需要一行代码。由于其解析性能高，序列化数据量相对少，
非常适合应用层对象的持久化场景。
    (7) Avro 的产生解决了 JSON 的冗长和没有 IDL 的问题，Avro 属于 Apache Hadoop 的一个子项目。
Avro 提供两种序列化格式：JSON 格式和 Binary 格式。Binary 格式在空间开销和解析性能方面可以和 Protobuf 媲美，
JSON 格式方便测试阶段的调试。适合于高性能的序列化服务。
   5) 几种协议的对比。
     XML 序列化 (Xstream) 无论在性能和简洁性上比较差;
     Thrift 与 Protobuf 相比在时空开销方面都有一定的劣势;
     Protobuf 和 Avro 在两方面表现都非常优越。

12.Java 实现多线程的方式及三种方式的区别？
   (1) 实现多线程的方式：
    继承 Thread 类，重写 run 函数。
    实现 Runnable 接口。
    实现 Callable 接口。
   (2) 三种方式的区别：
```
