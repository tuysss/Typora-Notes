## Java NIO 和 Netty

<https://www.zhihu.com/question/24322387/answer/282001188>

+ NIO：

  + 定义：None Blocking IO,非阻塞IO。

    + 区别于BIO，Blocking IO.传统的多线程服务器是BIO：

      <img src="https://raw.githubusercontent.com/user/repo/main/Typora-Notes-images/image-20221118112610659.png" alt="image-20221118112610659" style="zoom:50%;" />

      1. Accept是阻塞的，只有新连接来了，Accept才会返回，主线程才能继
      2. Read是阻塞的，只有请求消息来了，Read才能返回，子线程才能继续处理
      3. Write是阻塞的，只有客户端把消息收了，Write才能返回，子线程才能继续读取下一个请求

  + NIO怎么做到非阻塞？—— 事件机制（一个线程把accepy、读、写全做了）

    伪代码：

    ```java
    while true {
        events = takeEvents(fds)  // 获取事件，如果没有事件，线程就休眠
        for event in events {
            if event.isAcceptable {
                doAccept() // 新链接来了
            } elif event.isReadable {
                request = doRead() // 读消息
                if request.isComplete() {
                    doProcess()
                }
            } elif event.isWriteable {
                doWrite()  // 写消息
            }
        }
    }
    ```

+ **Netty**

  + 建立在NIO基础之上，提供了更高层次的抽象

    + accept可以用单独的献策会给你吃，读写用一个线程池；也可以accept和读写操作共享一个线程池。--可组装，定制化
    + 提供了一些列生命周期回调接口 -- 异步的
    + Netty可以同时管理多个端口，可以使用NIO客户端模型 --对RPC是必要的
    + 除了可以处理TCP socket，还可以处理UDP socket

  + Netty是什么？

    1）本质：JBoss做的一个Jar包

    2）目的：快速开发高性能、高可靠性的网络服务器和[客户端程序](https://www.zhihu.com/search?q=客户端程序&search_source=Entity&hybrid_search_source=Entity&hybrid_search_extra={"sourceType"%3A"answer"%2C"sourceId"%3A78947405})

    3）优点：提供异步的、事件驱动的网络应用程序框架和工具

[美团技术团队 Java NIO浅析](https://tech.meituan.com/2016/11/04/nio.html)



## Java8函数式接口 和 lambda

函数式接口(Functional Interface)就是一个有且仅有一个抽象方法，但是可以有多个非抽象方法的接口。

函数式接口可以被隐式转换为 lambda 表达式。

e.g.接口的定义：

```java
@FunctionalInterface
interface GreetingService 
{
    void sayMessage(String message);
}
```

那么就可以使用Lambda表达式来表示该接口的一个实现(注：JAVA 8 之前一般是用匿名类实现的)：

```java
GreetingService greetService1 = message -> System.out.println("Hello " + message);
```

or(我会这样写)

```java
GreetingService greetService1 = (message) -> {
  System.out.println("Hello " + message)
};
```









+ 使用过的：Runnable、Callable、FileFilter、Comparator、



## 惰性求值与Supplier

https://segmentfault.com/a/1190000040924267
https://www.jianshu.com/p/67c1ac9f93b5

1. Java和C一样是严格的(Strict)语言，Supplier引入了惰性计算(Lazy)/懒加载的机制，即：在变量使用时才完成计算
2. Lazy implements Supplier,是一个supplier实例
3. Lazy重写get方法
4. Spring源码中单例模式的使用和volatile  https://www.jianshu.com/p/123dfa087d65

