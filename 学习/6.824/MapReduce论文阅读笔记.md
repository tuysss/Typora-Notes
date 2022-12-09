**Abstract**

+ map k-v pair + reduce function
+ mapreduce适用于并行系统。运行时系统关注并行任务/数据的调度。包装了分布式系统，使程序员可以把mapreduce当作黑箱来使用。
+ 本mapreduce的实现：大量主机数（千级），已在谷歌集群中广泛使用。

## **1. Introduction**

+ 作者处理大规模、原始数据的经验{爬取到的文档、网络请求日志}并计算它们的派生数据。由于这些数据往往跨越非常多主机，对于这些数据的计算、分发、失败处理往往代码量很大。
+ 抽象出一个library可以「并行-容错-数据分发-负载均衡」map & reduce 两个原语受lisp等函数式语言启发。map用于逻辑上的数据记录以便于计算，reduce将多个相同key的value合并。map可使用者定制，reduce提高了多任务并发效率&容灾之重试。
+ 最妙的是提供了接口with方法——自动并行化，大规模计算——for高性能大规模集群
+ 本论文梗概
  + 第二部分：编程模型+举例。
  + S3:  mapreduce的一个实现，tailored towards基于集群的环境
  + S4:  一些编程模型的优化
  + S5: measurements for our imple for various tasks
  + S6:  探索google中的mapreduce实战经验：作为重写生产索引系统的基础
  + S7:  future work



## **2. Programming Model**

+ 输入是k-v对，输出是k-v对
+ map接收user传入的k-v对，MapReduce把它们和中间key I关联起来，传递给Reduce模块。
+ Reduce，也是user写的，接收中间键key I和它的一组value。它将这些values组织成一组更小的values，每次Reduce调用往往只输出0/1个value。中间态的values通过iterator逐个传递给user的reduce。这利于我们处理难以与存储器适配的大批数据。

2.1  **举例**

+  大规模词汇背景下的“词出现次数统计”
+ 该场景下，map给每个词+1，reduce将每个词附带的（map到的）1加起来

2.3 **更多例子** 

+ 分布式正则表达式统计
+ 网址访问频率统计
+ 网页链接图表反转
+ 主机检索词矢量
+ 索引反转
+ 分布式排序



## **3. implementation** 

​	3.1**执行总览**

+  map的分布式体现在并发地处理输入数据，将它们分割成m，reduce再用partation函数，比如hash将m个key收敛到r个。

流程图：当user调用mapreduce时，一系列行为会发生：

<img src="/Users/tianyanning/Library/Application Support/typora-user-images/image-20221203144658479.png" alt="image-20221203144658479" style="zoom:50%;" />



1. 用户程序中的MR库split to M，然后复制多份给集群
1. 其中一份copy时独特的——master。共有m个map任务和r个reduce任务，master主节点会指派空闲的从节点来完成工作。
1. 处理map任务的worker读取k-v对，把它们buffer到主存，
1. 周期性的，buffered paired被写到本地磁盘，分区成r块，从节点将在本地磁盘中的位置发送给主节点，之后master会负责把这些位置发给负责reduce工作的从节点（workers）
1. 负责reduce工作的worker收到master的以上位置通知之后，就通过远程调用拿到这些buffered data，全部读到之后，将data按keysort，然后group。如果intermediate data的量太大了，将使用外部的sorted
1. reduce worker(*节点*)遇到unique key之后，会把key和相关数据发送给user的reduce*函数*，reduce函数最终的output发给output file作为最终的输出结果
1. 最后，master唤醒用户程序

最终会得到R个output文件，用户无需将它们合并，这些文件通常会用做下一次MapReduce调用的input。

​	3.2 **Master数据结构**

​	存储每个map/reduce任务的state、identity

​	作为map&reduce任务之间的管道

​	3.3**容错**

+  Worker Failure

  master经常ping所有worker，ping不通即视为失败，task被置为空闲状态。已完成的map任务需要重新执行，reduce不需要（因为已经被存储在了全局文件系统中，whichmeans 分布一致性的）。

+ Master Failure

  周期性checkpoint，失败就copy一份上次的快照

+ faliures的语义

​	3.4**局部性/本地性**

​	尽量节省网络带宽

​	3.5**任务颗粒度**

​	类比：并发线程池&线程之间的关系

​	3.6 **Backup Tasks**

​	造成mapreduce耗时过长，往往是个别特别耗时的任务影响

​	解决：任务被标记为completed，备份下来。



## **4. 改进**

4.1 分割函数

​	特殊的hash

4.2 排序保证

4.3 合并函数

​	用户可以自己定制

4.4 输入输出类型

4.6跳过失败记录

4.7本地调试

4.8状态信息

4.9计数器



 ## 5. 性能



## 6. 实战

已被用于：大规模机器学习、谷歌新闻的集群、热门查询、网页的提取、大规模图计算。

应用快速增长因为：加速了集群的开发和原型测试，对于对分布式和并发0经验的程序员，也能轻松获取大批量资源。

6.1 **大规模标引**

用于谷歌网页搜索设备

the indexing system将爬取回来的大批文件，以GFS文件的形式作为输入。

优点1. 标引的code小而简单，因为关于分布式、并发、容错的部分都隐藏在了MapReduce库中。2.MapReduce库的表现很好，可以将计算和分布式解耦 3.改变简单



## 7. 相关工作

 MapReduce编程模型各个模块的灵感来源。。。



## 8. 结论

它的成功在于：

1. 易用。它封装了并发和分布式系统的细节。
2. 应用广泛。可以解决许多问题见6
3. 已经有可用的实现

团队在工作中的收获：

1. 限制编程模型有利于并发、分布式计算和容错的实现
2. 网络带宽是稀缺资源，因此我们致力于减少网络请求by局部性优化、本地复制
3. 冗余执行可以用于减少慢机器的影响、处理任务失败和数据丢失。



















