Distributed System 

+ #### Infructure:

  + Storage + Communication + Computation
  + Storage ---- most important
  + Communication ---- we use as tool, 更多内容详见6.829
  + Compuattion ---- MapReduce talks about

+ #### Abstraction:

   暴露interface，让它像file system一样易用

+ #### Implementation：

  + RPC - Remote Procedure Call 

    <font color='brown'>" whose goal was to mask that we are communicating over an unreliable network"</font>

  + Threads

    "which are a programming technique that allows us to harness multi-core computers"

    "more important for this class, threads are a way of structuring concurrent operations in a way that hopefully simplifies the programmer view of yhose concurrent "

  + Concurrency Control:  locks...

+ #### Perforamnce & Scalability

   + 目标：Fault Tolerant & Availability & Recovering 
   +  实现by：Non-volatile Storage & Replication

+ **Consistency**

   + All about  (k-v)  put/get

   + Guarantee 的程度区分了-- strong/week consistency 

      

------

### MapReduce

+ problems:  to build an index of web ,most is "sort" work, with terabytes, is expensive. So Google was running gaint computations with giant data on giant amout of computers.

  + E.g. PageRank 

+ in high level: map+reduce

  ![image-20230106182423411](https://raw.githubusercontent.com/tuysss/cloudimg/main/Typora-Notes-images/2023/01/06/86ea30134a051464f6033c7bdec9a4e7-20230106182425-14cf48.png)

+ GFS: spllit terabytes big file into chucks of MB files













