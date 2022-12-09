

[尚硅谷笔记](https://blog.csdn.net/lyyrhf/article/details/120159288)

[整理成面试题类型](https://zhangc233.github.io/2021/07/30/RabbitMQ%E9%9D%A2%E8%AF%95%E9%A2%98/)



## rabbitmq概念部分

+ 工作原理part，connection-channel的关系类似线程池-线程，e.g.JDBC连接池



## 安装总结

+ 安装在linux服务器中（我的版本centOS 8）
+ 安装内容：
  1. erlang（rabbitmq底层通信需要，mq编译需要） 
  2. rabbitmq及其key和sock
  3. 安装完毕，设置mq开机启动等配置
+ mq后台管理面板
  1. 在命令行安装management插件
  2. 防火墙开启端口15672 or 5672.  
  3. !! 注意：管理面板的ip就是linux服务器的公网ip！脑子挖塌啦
  4. 第一次登陆，发现guest的权限都没有开启，按照步骤在rabbitmq中添加用户即可
+ 参考：
  + [从安装到管理，全流程适用](https://blog.csdn.net/weixin_42601001/article/details/12393122)
  + [centOS8安装，版本新。错误总结比较全，只看前半段到安装即可](https://www.jianshu.com/p/608885293351)


+ ps.  macOS上的ssh连接，因为xshell不提供mac上的安装，适用了一圈之后发现用terminal+terminal上的{Shell->新建远程连接->安全文件传输sftp}这样全命令行就不错。
  + 参考：[Mac上传文件到Linux服务器](https://blog.51cto.com/u_15183212/2737240)

## demo问题解决

+ connection confused

  <img src="/Users/tianyanning/Library/Application Support/typora-user-images/image-20221125153731680.png" alt="image-20221125153731680" style="zoom:50%;" />

  检索得知是端口映射问题

  + [rabbit的配置文件的位置？如何查看和修改端口号？](https://www.helloworld.net/p/4250587856)——通过打印日志看出。（rabbitmq通常不需要更改配置）
  + 最终解决：是防火墙5672端口没有打开<img src="/Users/tianyanning/Library/Application Support/typora-user-images/image-20221125155250702.png" alt="image-20221125155250702" style="zoom:50%;" />
    + 参考：看到受启发
    + <img src="/Users/tianyanning/Library/Application Support/typora-user-images/image-20221125155403940.png" alt="image-20221125155403940" style="zoom:50%;" />
  + ps. 客户端Connection redused排错：host-port写没写对，port端口有没有防火墙，服务器的服务有没有启动





# RabbitMQ tutorial笔记

### “Hello World!”

+ rabbitmq是消息代理。以送信打比方，rabbitmq同时是信箱(queue)、邮局()和快递员()。
+ 



# 问题

### channel、queue、exchange的关系？

https://stackoverflow.com/questions/18418936/rabbitmq-and-relationship-between-channel-and-connection/18419417#18419417

+ Connection是真实的TCP连接to消息代理商，channel是其中的一条虚拟连接专供AMQP使用。一个connection中可以建立多个channel
+ channel和queue之间没有直接关联。channel是真正向message broker发消息的，queue也许灵感来自channel

https://www.rabbitmq.com/tutorials/amqp-concepts.html

+ A queue binds to the exchange with a routing key K
+ in AMQP 0-9-1, messages are load balanced between consumers and not between queues.
+ **Queue:**To instruct an exchange E to route messages to a queue Q, Q has to be *bound* to E. Bindings may have an optional *routing key* attribute used by some exchange types
+ **Consumers:**Storing messages in queues is useless unless applications can consume them
+ 
