## Day 0

需求：

需要两个乐高RPC接口
1.分页查询仓主数据； 查询条件，货主ID（必填），仓编码，仓外部编码；
2.查询仓销售范围；根据当前仓查询

before开发，Q&A:

​	第一个接口，

	1. 给的字段和乐高页面对应？
	1. protobuf？：warehouse.service？
	1. 返回如图？{仓库编码+仓库名称+别名+地址}

​	第二个接口，

1. 查询使用当前仓的哪些字段？
2. 返回：list of销售范围？



大致流程：

rpc层：

接收request，构建查询类型`xxxParam`，传入Service层 -> 通过converter，从service返回的`pageResult`中build出response

service层：

```java
ScmPageResult<WarehouseDO> search(SearchWarehouseParam searchWarehouseParam);
```

具体的，传入`xxxParam`类型到dao中查，构建`pageResult`，返回。

dao层：

如有需要，处理查询条件。调用mapper层处理具体查询逻辑。



## Day 1

todo：

1. 找一下两个接口有无现成的mapper可以用

   ```java
       /**
        * 分页查询仓信息
        */
       ScmPageResult<WarehouseDO> search(SearchWarehouseParam searchWarehouseParam);
   ```

2. 怎么构建Condition。Condition是？



参考学习链路：rpc DeleteWarehouse (DeleteWarehouseRequest) returns (DeleteWarehouseResponse);











参考：

接口一：

```java
    @SSOWebLoginRequired(sids = {ApiConstants.SSO_WEB_LOGIN_REQUIRED_SID})
    @RequestMapping(value = "/search", method = RequestMethod.POST)
    @ScmLog(action = ScmLogConstants.ACTION_SEARCH_NAME, itemId = "#sellerId", itemType =
            ScmLogConstants.ITEM_TYPE_WAREHOUSE, param = "#searchWarehouseParam", sellerId = "#sellerId")
    public Object search(@Visitor long sellerId, @JsonParam(value = "searchWarehouseParam")
            SearchWarehouseParam searchWarehouseParam) {
```

接口二：

```java
    @SSOWebLoginRequired(sids = {ApiConstants.SSO_WEB_LOGIN_REQUIRED_SID})
    @RequestMapping(value = "/list", method = RequestMethod.POST)
    @ScmLog(action = ScmLogConstants.ACTION_LIST_NAME, itemId = "#sellerId", itemType =
            ScmLogConstants.ITEM_TYPE_WAREHOUSE_SALE_SCOPE_TEMPLATE, param = "#id", sellerId = "#sellerId")
    public Object list(@Visitor long sellerId, @JsonParam(value = "id")
            Long id) {
```



- [ ] proto定义
- [ ] 参考api，定义dto，写service，写rpc





Q:

1. rpcservice里的@override？

2. 货主id（ownerId）和商家id（merchantId）？

   结论：目前在供应链端的数值是一致的。但是在业务上，商家id是全电商领域；货主id是供应链领域。设计是为了以后的业务中，有的owner只供货，而不直接面向用户销售。





## Day 2 

#### try块的return、finally块中的return、try-catch-fianlly块外部的return

https://segmentfault.com/a/1190000021229894

几个结论：

1. 即使try中return了，finally也会被执行 -- 因此在finally块中进行相关资源的关闭清理一直是最佳实践

2. try中有retrun，之后还有return。又分为两种情况：

   1. 第二个return在try-catch-finally的外部：哪怕final中改变了try中要return的值，也不受影响。

      + 因为JVM规范：先把try中将要return的值先存到一个本地变量中，即本例中的x=2将会被保存下来。接下来去执行finally语句，最后返回的是存在本地变量中的值，即返回x=2.

        <img src="https://raw.githubusercontent.com/tuysss/cloudimg/main/Typora-Notes-images/2023/01/11/d139cde2b1134bc6bca56ed65b2bb2fe-20230111111625-f9e3ca.png" alt="image-20230111111624354" style="zoom:50%;" />

   2. 第二个return在finally块中，try中的return值会受影响。

      + 因为规范规定了，当try和finally里都有return时，会忽略try的return，而使用finally的return。

问题场景：

![image-20230111112022473](https://raw.githubusercontent.com/tuysss/cloudimg/main/Typora-Notes-images/2023/01/11/b26b88fb6cafbb40cbe269648a42e3d1-20230111112023-2a23ad.png)