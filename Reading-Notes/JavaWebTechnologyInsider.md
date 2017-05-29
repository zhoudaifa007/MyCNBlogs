<h2>《深入分析 Java Web 技术内幕》 :books: </h2> 

> 许令波 著       电子工业出版社

```java
1.当一个用户在浏览器里输入 www.taobao.com 这个 URL，将会会发生什么？
  首先请求 DNS 把这个域名解析成对应的 IP 地址，然后根据这个 IP 地址在互联网上找到对应的服务器，向这个服务器发起
一个 get 请求，由这个服务器决定返回默认的数据资源给访问的用户。在服务器端实际上还有很多复杂的业务逻辑：服务器
可能有很多台，到底指定哪台服务器来处理请求，这需要一个负载均衡设备来平均分配所有用户请求；还有请求的数据是存储
在分布式缓存里还是一个静态文件中，或是在数据库里；当数据返回浏览器时，浏览器解析数据发现还有一些静态资源 
(如 CSS、JS 或者图片) 时又会发起另外的 HTTP 请求，而这些请求很可能会在 CDN 上，那么 CDN 服务器又会处理这个
用户的请求，大体上一个用户请求会涉及这么多的操作。
```

<a href="http://images.cnblogs.com/cnblogs_com/wp5719/936332/o_CDN.png"> B/S 网络架构 </a>

<a href="http://images.cnblogs.com/cnblogs_com/wp5719/936332/o_DNS.png"> DNS 域名解析 </a>

<a href="http://images.cnblogs.com/cnblogs_com/wp5719/936332/o_CDNs.png"> CDN 架构 </a>

```java
```