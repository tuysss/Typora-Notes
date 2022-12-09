11.28&11.29

+ 静态加载的数据（大批量json）尽量动态从接口要，不要静态加载
+ 注意post方法是否要求登录及其他校验，ifso，要header里加料，cookie
+ 后端接口：表单提交多个字段，使用queryType，可以确定根据谁来查询
+ 能全局加载一次的数据，就不要放到函数中反复

11.30

+ 电子面单查询的效益：
  + for 研发：提升问题排查和修复效率
  + for 产品+运营：可以验证/查询电子面单数据，提高协同效率

+ 请求后端接口出现了POST401错误 —— Unorthorized

  + 一般是认证信息未携带或已失效/登录失效

  + strict-origin-when-cross-origin
  + 这个问题最终解决：连接接口的域名写错了，——跨域问题

12.1

+ 做这个《履约单查询》：
  + 有问题，找相应的人解决 —— 超过20min自己还解决不了的情况下
  + 提前对接需求、一定问明白了再开始做。  
    + *和自己开发最大的区别：需求是变动的、不明确的、甚至提需求的人自己也未必那么清楚
  + 一旦确定好需求，在自己的领域内，就要做出来、做好，不给别人（上下游）带来麻烦

12.6

##### 以前利用bug，现在被bug所害 ..

##### 企业开发多环境、多数据库

+ https://www.51cto.com/article/680426.html

##### 泳道是什么

+ bg：要求环境稳定 & 多服务/多版本同时测试的需求
+ 实现：对服务链路按需求进行分组复制，并实现逻辑、物理的隔离
+ 收益：解决抢占问题、提高开发效率
+ https://blog.csdn.net/terrychinaz/article/details/115346706
+ https://juejin.cn/post/7109705071151022087

##### 阅读blog

+  [程序员如何学习和构建网络知识体系](https://plantegg.github.io/2020/05/24/%E7%A8%8B%E5%BA%8F%E5%91%98%E5%A6%82%E4%BD%95%E5%AD%A6%E4%B9%A0%E5%92%8C%E6%9E%84%E5%BB%BA%E7%BD%91%E7%BB%9C%E7%9F%A5%E8%AF%86%E4%BD%93%E7%B3%BB/)

+  Blog [如何在工作中学习](https://plantegg.github.io/2018/05/24/%E5%A6%82%E4%BD%95%E5%9C%A8%E5%B7%A5%E4%BD%9C%E4%B8%AD%E5%AD%A6%E4%B9%A0V1.1/)
   + 学习本身：慢就是快“学习不是走斜坡，更像是走阶梯，每一阶有每一阶的难点”，学了《java Concurrency Practice》深有感触。1.甄别「值得学习的知识」：通用的、能打通学习链路的 2.看见它，抓住它，做好要花费浮光掠影式学习10倍以上时间和精力的准备，并且“学会”之后不断应用和总结
     + “钉子式的学习方式，就是在一大片知识中打入几个桩，反复演练将这个桩不停地夯实，夯温，做到在这个知识点上用通俗的语言跟小白都能讲明白，然后在这几个桩中间发散像星星之火燎原一样把整个一片知识都掌握下来。这种学习方法的缺点就是很难找到一片知识点的这个点，然后没有很好整合的话知识过于零散。”
   + 向同学提问的技巧：**这段值得全文反复看** 直奔主题、描述清晰、帮对方把除了解决我的问题以外的所有困难都解决。
   + 业务问题解决：“我最不喜欢的一句话就是我的程序没问题，因为我的逻辑是这样的，不会错的。你当然不会写你知道的错误逻辑，程序之所以有错误都是在你的逻辑、意料之外的东西。”
   + [在工作中学习ver2.0](https://plantegg.github.io/2020/11/11/%E5%A6%82%E4%BD%95%E5%9C%A8%E5%B7%A5%E4%BD%9C%E4%B8%AD%E5%AD%A6%E4%B9%A0--V2.0/) 哪些品质能够决定一个人学习好坏——排在第一名的是复盘、总结能力

##### 跨域资源共享CROS - 再探

+ 是什么？

  + CORS是一个W3C标准，全称是"跨域资源共享"（Cross-origin resource sharing）

    + W3C？：万维网联盟，负责对web进行标准化，创建并维护www标准

  + 它允许浏览器向跨源服务器，发出[`XMLHttpRequest`](https://www.ruanyifeng.com/blog/2012/09/xmlhttprequest_level_2.html)请求，从而克服了AJAX只能[同源](https://www.ruanyifeng.com/blog/2016/04/same-origin-policy.html)使用的限制

    + 同源？：

      + 是什么：A网页设置的 Cookie，B网页不能打开，除非这两个网页"同源"。——协议域名+端口相同

      + 目的：保护用户信息安全，防止恶意的网站窃取数据

      + <font color='#a31515'>注意：跨域请求被拒绝，其实是浏览器已经拿到了不同源服务器响应的数据，浏览器对非同源请求返回的结果做了拦截，而不是服务器拒绝浏览器的请求。</font>

      + 后果：太严格了，主要三种限制：1.Cookie无法读取 2.DOM无法获得 3.AJAX请求不能发送

        + 规避方式：
          1. for Cookie——浏览器设置document.domain可以只要一级域名相同就共享
          2. for DOM（iframe窗口和window.open方法打开的窗口）——片段识别符、window.name、跨文档通信API
          3. for AJAX——一，架设服务器代理（浏览器请求同源服务器，再由后者请求外部服务。二，3wayto规避：JSONP、WebSocket、CORS
             + JSONP：网页动态插入`<script>`元素，只适用于`get`
             + WebSocket是一种通信协议，使用`ws://`（非加密）和`wss://`（加密）作为协议前缀。该协议不实行同源政策，只要服务器支持，就可以通过它进行跨源通信。
             + CORS是跨源AJAX请求的根本解决方法。详见下

        

+ 简介：

  1. CORS需要浏览器和服务器同时支持，目前所有浏览器都支持，因此实现CORS通信的关键是服务器只要实现了CORS接口，就可以跨源通信 
  2. 对于开发者来说，CORS通信与同源的AJAX通信没有差别

+ 实现：针对简单请求&非简单请求分类讨论

  + 分类：

    <img src="https://raw.githubusercontent.com/tuysss/cloudimg/main/Typora-Notes-images/2022/12/09/c7c2b56e3c81f62ddd215959825b6686-20221209142941-ad3baa.png" alt="114def096f0b9aad56d04ce6b2a63464-20221209105144-61fdcd.png" style="zoom:50%;" />

  + 简单请求

    + 对于简单请求，浏览器直接发出CORS请求。具体来说，就是在头信息之中，增加一个`Origin`字段。`Origin`字段用来说明，本次请求来自哪个源（协议 + 域名 + 端口）

    + 服务器根据这个值，决定是否同意这次请求。

      如果`Origin`指定的源，不在许可范围内，服务器会返回一个正常的HTTP回应。浏览器发现<u>，这个回应的头信息没有包含`Access-Control-Allow-Origin`字段</u>，就知道出错了，从而抛出一个错误，被`XMLHttpRequest`的`onerror`回调函数捕获。*注意，这种错误无法通过状态码识别，因为<u>HTTP回应的状态码有可能是200</u>。*

  + 非简单请求

    + 非简单请求是那种对服务器有特殊要求的请求，比如请求方法是`PUT`或`DELETE`，或者<u>`Content-Type`字段的类型是`application/json`。</u>
      + why application/json对服务器有特殊要求？：
    + 非简单请求的CORS请求，会<u>在正式通信之前，增加一次HTTP查询请求</u>，称为**"预检"请求**（preflight）。
    + 浏览器先询问服务器，当前网页所在的域名是否在服务器的许可名单之中，以及可以使用哪些HTTP动词和头信息字段。只有得到肯定答复，浏览器才会发出正式的`XMLHttpRequest`请求，否则就报错。
    + 一旦服务器通过了"预检"请求，以后每次浏览器正常的CORS请求，就都跟简单请求一样，会有一个`Origin`头信息字段。服务器的回应，也都会有一个`Access-Control-Allow-Origin`头信息字段。

+ 参考：[阮一峰：跨域资源共享 CORS 详解](https://www.ruanyifeng.com/blog/2016/04/cors.html)

+ ps. Axios是不允许跨域的，可能会遇到这个问题，遇到再说吧

#### git再探

+ 今天解决的问题：

  把Typora-notes推到github的远程仓库中，先git status发现有提示

  <img src="https://raw.githubusercontent.com/tuysss/cloudimg/main/Typora-Notes-images/2022/12/09/a928a866850da7310d0cc2edc96376a9-20221209142806-eec33d.png" alt="image-20221209142805865" style="zoom:50%;" />

  没有理，add-commit后，试图直接push到origin/main，被拒绝：

  <img src="https://raw.githubusercontent.com/tuysss/cloudimg/main/Typora-Notes-images/2022/12/09/c4dd202b1c46bb475b16276bec95346a-20221209142828-91fbff.png" alt="image-20221209142827430" style="zoom:50%;" />

  解决：先从远程相应分支origin/main中pull（fetch+merge），再次push

  <img src="../../../Library/Application%20Support/typora-user-images/image-20221209142859858.png" alt="image-20221209142859858" style="zoom:50%;" />

  

+ 总结：

  + 建立远程仓库与本地关联：1.git clone 2.git add remote 3.本地的add-commit 4. git push 5.之后每次push之前，注意要先pull
  + push origin main || pull origin main  这个main指的都是远程分支名，动脑子想一想，本地的默认就是现在的branch HEAD了呀

+ 参考：[Git User Manual](https://mirrors.edge.kernel.org/pub/software/scm/git/docs/user-manual.html#sharing-development)

  

#### 序列化与反序列化

#### wireshark使用入门





12.7

#### <font color='#008080'>daily work achieve</font>

+ 今天终于把手头的两个都上线了prt环境，现在定位问题、解决问题（无论是部署的、还是js代码的）感觉都上了一个台阶







12.8

#### <font color='#008080'>daily work achieve</font>

+ 今天把手头两个项目都发布了正式页面，总共耗时20min+

+ 进步之处，like mentor said，解决问题的时间就是20min，现在我把它控制在20-30min。清晰地辨识自己能力的边界在哪里。

+ 有待进步，解决问题的过程中，发现自己十分钟之前问出了愚蠢的问题，是文档中写的明明白白的。so，能力的边界之信息搜索和整理和标记-有待突破！

  

#### git 

1. 不能直接push因为“one commit ahead of origin/main”?
2. 为解决上述问题，需要先pull -> “merge by recursive strategy”？



#### 发现typora-notes上传github后，图片url访问404的解决——用github仓库做picgo上传图片的图床

+ 参考：

  1. [Typora + PicGo-Core + Github 实现图片上传到Github](https://www.cnblogs.com/xiaowj/p/13934555.html)
  2. [也是参考上面博客，作了补充](https://www.jianshu.com/p/33e7da24ee36)
  3. [获取github的token](https://www.cnblogs.com/yongdaimi/p/16300462.html)

+ 配置好了，question：必须右键图片->上传 才可以吗？能不能批量上传through命令行or else？

  + 解决：在Typora处设置		

    <img src="https://raw.githubusercontent.com/tuysss/cloudimg/main/Typora-Notes-images/2022/12/09/a1d287cd569c93b721cbb06e835714b9-20221209142727-608b93.png" alt="image-20221209142725856" style="zoom:50%;" />
    
    

+ 问题：自动上传时经常image load failed

  + https://blog.51cto.com/vmuu/4914267

  + 现在的解决是：通过`vim ~/.picgo/config.json ` 配置picgo中github图床位置访问，可以在github上访问到图片位置，但是本地暂时还是不能显示。很不方面浏览。

    ``````bash
    {
      "picBed": {
        "uploader": "githubPlus",
        "current": "githubPlus",
        "githubPlus": {
          "branch": "main", // 仓库分支
          "customUrl": "https://github.com/tuysss/cloudimg/tree/main", // 访问的自定义url
          "origin": "github", // 存放的图片类型
          "repo": "tuysss/cloudimg", // 存放图片的仓库
          "path": "Typora-Notes-images", // 存放图片的仓库目录下的文件夹
          "token": "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" // 访问github的仓库的token, 不知道怎么
    设置的自行百度
        }
      },
      "picgoPlugins": {
        "picgo-plugin-github-plus": true, // 启用github-plus插件
        "picgo-plugin-rename-file": true,
        "picgo-plugin-watermark": true
      },
      "picgo-plugin-github-plus": {
        "lastSync": "2022-12-09 10:51:46"
      },
      "picgo-plugin-rename-file": {
        "format": "{y}/{m}/{d}/{hash}-{origin}-{rand:6}"
      },
      "picgo-plugin-watermark": { // 以下配置信息参考插件地址说明
        "text": "tuysss", // 水印名称
        "fontSize": 18, // 水印字体大小
        "position": "rm" // 水印位置
      }
    }

  + 最终解决：问题在于——*picgo里面图床设置的github图床自定义域名有问题*

    + 参考：[【错误解决】 PicGo +typora + GitHub 搭建个人图床工具：typora 中的图片image load failed](https://blog.csdn.net/weixin_42072280/article/details/120568487?spm=1001.2101.3001.6650.3&utm_medium=distribute.pc_relevant.none-task-blog-2%7Edefault%7ECTRLIST%7ERate-3-120568487-blog-115803675.pc_relevant_3mothn_strategy_recovery&depth_1-utm_source=distribute.pc_relevant.none-task-blog-2%7Edefault%7ECTRLIST%7ERate-3-120568487-blog-115803675.pc_relevant_3mothn_strategy_recovery&utm_relevant_index=4)

    ```bash
    {
      "picBed": {
        "uploader": "githubPlus",
        "current": "githubPlus",
        "githubPlus": {
          "branch": "main", 
          //不用设置访问自定义的url，因为我这里没有用到cdn，自定义没必要还会出错
          "customUrl": "https://github.com/tuysss/cloudimg/tree/main", // 访问的自定义url
          "origin": "github", 
          "repo": "tuysss/cloudimg", 
          "path": "Typora-Notes-images", 
          "token": "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" 
        }
      }
    
    }
    ```

+ 有一个问题，上传图片报错，报错日志看不懂

  + 解决：去github-setting重新生成了token，替换到picgo配置中

  

#### 阅读 性能优化七步法 + 美团技术团队 大众点评订单分库分表





#### 12.9

#### 昨天那个问题延续，怎么用云搭建cdn？

+ 不是搭建，而是从CDN服务商处购买




