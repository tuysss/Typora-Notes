## 11.28&11.29

+ 静态加载的数据（大批量json）尽量动态从接口要，不要静态加载
+ 注意post方法是否要求登录及其他校验，ifso，要header里加料，cookie
+ 后端接口：表单提交多个字段，使用queryType，可以确定根据谁来查询
+ 能全局加载一次的数据，就不要放到函数中反复

## 11.30

+ 电子面单查询的效益：
  + for 研发：提升问题排查和修复效率
  + for 产品+运营：可以验证/查询电子面单数据，提高协同效率

+ 请求后端接口出现了POST401错误 —— Unorthorized

  + 一般是认证信息未携带或已失效/登录失效

  + strict-origin-when-cross-origin
  + 这个问题最终解决：连接接口的域名写错了，——跨域问题

## 12.1

+ 做这个《履约单查询》：
  + 有问题，找相应的人解决 —— 超过20min自己还解决不了的情况下
  + 提前对接需求、一定问明白了再开始做。  
    + *和自己开发最大的区别：需求是变动的、不明确的、甚至提需求的人自己也未必那么清楚
  + 一旦确定好需求，在自己的领域内，就要做出来、做好，不给别人（上下游）带来麻烦

## 12.6

#### 以前利用bug，现在被bug所害 ..

#### 企业开发多环境、多数据库

+ https://www.51cto.com/article/680426.html

#### 泳道是什么

+ bg：要求环境稳定 & 多服务/多版本同时测试的需求
+ 实现：对服务链路按需求进行分组复制，并实现逻辑、物理的隔离
+ 收益：解决抢占问题、提高开发效率
+ https://blog.csdn.net/terrychinaz/article/details/115346706
+ https://juejin.cn/post/7109705071151022087

#### 阅读blog

+  [程序员如何学习和构建网络知识体系](https://plantegg.github.io/2020/05/24/%E7%A8%8B%E5%BA%8F%E5%91%98%E5%A6%82%E4%BD%95%E5%AD%A6%E4%B9%A0%E5%92%8C%E6%9E%84%E5%BB%BA%E7%BD%91%E7%BB%9C%E7%9F%A5%E8%AF%86%E4%BD%93%E7%B3%BB/)

+  Blog [如何在工作中学习](https://plantegg.github.io/2018/05/24/%E5%A6%82%E4%BD%95%E5%9C%A8%E5%B7%A5%E4%BD%9C%E4%B8%AD%E5%AD%A6%E4%B9%A0V1.1/)
   + 学习本身：慢就是快“学习不是走斜坡，更像是走阶梯，每一阶有每一阶的难点”，学了《java Concurrency Practice》深有感触。1.甄别「值得学习的知识」：通用的、能打通学习链路的 2.看见它，抓住它，做好要花费浮光掠影式学习10倍以上时间和精力的准备，并且“学会”之后不断应用和总结
     + “钉子式的学习方式，就是在一大片知识中打入几个桩，反复演练将这个桩不停地夯实，夯温，做到在这个知识点上用通俗的语言跟小白都能讲明白，然后在这几个桩中间发散像星星之火燎原一样把整个一片知识都掌握下来。这种学习方法的缺点就是很难找到一片知识点的这个点，然后没有很好整合的话知识过于零散。”
   + 向同学提问的技巧：**这段值得全文反复看** 直奔主题、描述清晰、帮对方把除了解决我的问题以外的所有困难都解决。
   + 业务问题解决：“我最不喜欢的一句话就是我的程序没问题，因为我的逻辑是这样的，不会错的。你当然不会写你知道的错误逻辑，程序之所以有错误都是在你的逻辑、意料之外的东西。”
   + [在工作中学习ver2.0](https://plantegg.github.io/2020/11/11/%E5%A6%82%E4%BD%95%E5%9C%A8%E5%B7%A5%E4%BD%9C%E4%B8%AD%E5%AD%A6%E4%B9%A0--V2.0/) 哪些品质能够决定一个人学习好坏——排在第一名的是复盘、总结能力

#### 跨域资源共享CROS - 再探

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





## 12.7

#### <font color='#008080'>daily work achieve</font>

+ 今天终于把手头的两个都上线了prt环境，现在定位问题、解决问题（无论是部署的、还是js代码的）感觉都上了一个台阶







## 12.8

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
  + 2023/1/10又遇到这个问题，确实是token过期释放的问题。这次设置了90天过期时间。需要本周期内github token的话，去picgo配置文件里复制。
  
  

**阅读 性能优化七步法 + 美团技术团队 大众点评订单分库分表实战**





## 12.9

#### 昨天那个问题延续，怎么用云搭建cdn？

+ 不是搭建，而是从CDN服务商处购买



## 12.12

### 笔试题启示录

首先，必须要过笔试关。

+ 现在的问题是：

​	一，基础知识的，也不够牢靠

​	二，算法题的，1. 简单的，一眼就会的，不能十分钟ac，有的甚至耗时30-40min；2. 稍难的，要么真不会，要么有思路但是畏难不敢下手，要么是因为前面简单题耗时太多时间来不及直接放弃。

+ 要做的：
  1. 基础知识&简单题，每天坚持做着，就像每天睡前坚持玩四小时手机一样。要求：只要坚持就好。
  
  2. 找今年秋招的笔试题，限时做，然后去牛客学习别人的思路。要求：一做一进步。
  
     

### 今日阅读清单：

[Spring解决了什么问题](https://blog.csdn.net/qq_41723615/article/details/89088901)

https://github.com/DerekYRC/mini-spring



### 解决：端口占用 on mac

`lsof -i:8080`

`kill 65383`



### 从@Component到Spring bean

1. 是什么？

   + @controller用于标注控制层，其中将被注入service的实现
   + @service用于标注服务层，其中将被注入dao/mapper的实现
   + @repository用于标注数据访问层  p.s.@Mapper是MyBatis的注解，和Spring没有关系
   + **@Component**泛指各种组件，当该类不属于易守难攻三种类的时候，就标注@Component。

2. 作用？

   + 所有以上这些注解，用于**声明一个Bean**——也就是说，当Spring启动时，~~它们将被示例化，保存到容器中。~~ 这些类将被扫描，纳入spring容器中进行管理
     + <context:component-scan base-package=”com.*”>
   + 在需要用到的时候，@autowired，Spring将从容器中取到这个已被示例化的类，~~注入到@autowired标注的方法的入参~~，实际工作中，很多对象会依赖其他对象完成任务。这时候就需要能够**将组件扫描得到的bean**和他们依赖装配在一起，即——***装配（wiring）**，whichis 依赖注入的本质。*
   + 总结，Spring Bean管理：@Component及其衍生注解用于**注册Bean**；@Autowired/@Resource注解用于**装配Bean** 
     + ps.此条总结的bg是管理bean的方案一——自动装配

   

   ## 扩展：Spring Bean

   1. 是什么？

      Spring Bean是*被实例的，组装的及被Spring 容器管理的* **Java对象**。

      Spring 容器会自动完成@bean对象的实例化。

      创建应用对象之间的协作关系的行为称为：**装配(wiring)**，这就是依赖注入的本质。

   2. Spring帮助我们通过两种方式管理bean，一种是注册Bean，另一种是装配Bean。完成管理动作的方式有如下三种：

      1. 使用自动配置。@Component注解及其衍生注解@RestController、@Controller、@Service和@Repository等都是组件注册注解。使用组件注册注解告诉Spring，我是一个bean，你要来管理我，然后使用@AutoWired注解去装配Bean；也可以使用
         由javax.annotation.Resource提供@Resource去装配Bean。所谓装配，就是管理各个对象之间的协作关系。
      2. 使用JavaConfig。使用@Configuration告诉Spring，IoC容器要怎么配置，即怎么去注册bean，怎么去处理bean之间的关系（装配）。
      3. 使用XML配置。标签就是告诉spring怎么获取这个bean，各种就是手动的配置bean之间的关系。

+ 参考：

  + https://www.cnblogs.com/strivelearn/p/6096128.html
  + https://www.cnblogs.com/east7/p/14390862.html

  

### 面试prepare

#### 实习

+ 实习期间主要的工作是？

  参与快手供应链的中台搭建。目前完成两个需求是，履约单管理，电子面单查询。内容主要是是追踪商品下单后，供应链的商品信息、订单状态变更、履约轨迹这些。这两个需求都是面向b端的，一方面是对研发，问题排查，另一方面主要是给运营侧，验证/查询相关数据，提高协同效率

  中台：业务需求和功能需求高度相似、通用化程度很高。现在把这些业务逻辑抽出来，提高复用、提高效率。

+ 主要收获是？

  1. 体会到工作和自己写代码的区别.={1.规范的开发流程：需求上来，和产品argue协同。开发过程中，解决问题的思路也相比之前有了扭转。最后提测、交付、上线  2.技术分享会和公司、尤其是组内同学的技术、业务文档。}

     解决问题={1. 定位问题、分析这个问题是否是自己能解决的，mentor20min 2. 解决问题：自己写代码可能就畏难略过，但是工作任务必须完成，咬咬牙做也就做成了，给自己正反馈}

  2. 团队内沟通。a.前面提及，技术问题  b.工作交接。第一个接的需求，做完了就做完了，mentor说对接了吗？原来还有这一步，还要评审。  ->一方面，是不懂就问。另一方面，这个不懂应该是在自己翻阅过公司文档、历史群聊之后都找不到答案的问题，然后去问别人。同时，问完自己实践并记录，同一个问题做到不打扰别人第二遍。

+ 面临的最大问题和解决？

  之前的工作，主要是内部乐高中台，我觉得除了第一周对开发工具、以及内部中台上手有点儿磕磕绊绊，后面就比较顺利了。每接一个需求，都要求自己在这个项目中优化上个项目中的代码缺陷，比如之前review又提出我的代码静态加载会比较慢，但是那个项目已经要上线了，我就会在下个项目中有意识地规避、改正。

  另外就还是沟通、提问的意识。

+ 工作和自己写代码的区别？（同上）

  

##### 算法：做个二分、做个树和链表





### git 拉取远程的指定分支

参考：https://blog.51cto.com/u_15688254/5391692

bg：`git clone <远程仓库地址>` clone且仅clone main这一个分支 (通过命令`git branch --list`查看)

+ 方案一： 

  `git clone -b <指定分支名> <远程仓库地址>`

  问题：实质上和之前一样，只是有且仅有了<指定分支>这一个。因为我计划从要学习的repo`mini-spring`拉取多个分支，因此选用方案二。

+ 方案二：

  1. 创建空文件夹，并 `git init`
  2. `git remote add origin <远程仓库地址>` 建立本地仓库与远程仓库的连接
  3. `git fetch origin <远程仓库指定分支名>`
  4. `git checkout -b <本地分支名，随便起> origin/<远程仓库指定分支名>` 在本地创建分支并切换到该分支
  5. `git pull origin <远程仓库指定分支名>`

  

#### 问题：已知fetch+merge==pull。fetch是干嘛的？

+ Git Fetch is the command that tells the local repository that there are changes available in the remote repository **without bringing the changes into the local repository.** 
+ Git Pull on the other hand brings the copy of the remote directory changes into the local repository.



## 12.13

### <font color='#008080'>daily work achieve</font>

昨天上午接到了要写代码的任务，下午有事情，到晚上拉下来代码，留了一堆飘红走了

今天开始正式进这个项目



### git config两个账号

https://www.cnblogs.com/cangqinglang/p/12462272.html

注意：

恭喜你！完成以上配置后，其实你已经基本完成了所有配置。分别进入附属于 github 和 gitlab 的仓库，此时都可以进行 git 操作了。但是别急，如果你此时提交仓库修改后，你会发现提交的用户名变成了你的系统主机名。

这是因为 git 的配置分为三级别，System —> Global —>Local。System 即系统级别，Global 为配置的全局，Local 为仓库级别，优先级是 Local > Global > System。

因为我们并没有给仓库配置用户名，又在一开始清除了全局的用户名，因此此时你提交的话，就会使用 System 级别的用户名，也就是你的系统主机名了。

因此我们需要为每个仓库单独配置用户名信息，假设我们要配置 github 的某个仓库，进入该仓库后，执行：

```
git config --local user.name "jitwxs"
git config --local user.email "jitwxs@foxmail.com" 
```

执行完毕后，通过以下命令查看本仓库的所有配置信息：

git config --local --list



## 12.14

### maven导包成功但import飘红

问题描述：

1. 第一阶段，左边全灰，即，不能识别子模块。

   + 解决：尝试清除缓存重启，等，最后莫名其妙好了

2. 第二阶段，能够识别子模块，但是pom文件橙色

   + 解决：[idea在克隆Maven项目时pom文件无法识别子模块，依赖导入不进来](https://blog.csdn.net/qq_42956376/article/details/111450758)

3. 第三阶段，还剩这个import不进来

   ![image-20221214110243633](https://raw.githubusercontent.com/tuysss/cloudimg/main/Typora-Notes-images/2022/12/14/44fd43805851b58b40beca069153e9ba-20221214110245-82f917.png)

   + ~~一个思路：含.proto文件的上一级目录应该被marked as sources root~~
   
   + 问题实质之一：<font color='red'>**import不是从project中的.java文件中import，而是从target的.class文件中**</font>
   
     + 需要先maven编译：`mvn clean compile -DCHECKSTYLE.SKIP=TRUE  //-D：传入properties属性参数`
   
   + 问题实质之二：idea识别不了target包中的类/maven项目无法识别jar包里的class
   
     + 最终解决：删除项目中的.idea文件，重启。
   
       （.Idea存放项目的配置信息，包括描述、编码、历史记录，版本控制信息。in own word，保存一些过去的配置信息）。

+ **扩展： 很多情况，诸如.class导入失败/.class不能生成 /project接口乱套**，都可以尝试使用删除.idea文件夹，然后关闭打开，.idea文件夹会自动生成
+ **扩展之二： 许多build可以成功，但是飘红报错的通用思路：**
  1. `mvn clean` + `mvn install -Dcheskstyle.skip=true`
  2. 尝试invalidate and restart
  3. 尝试删除.idea文件夹



### gRPC

https://appmaster.io/zh/blog/shi-yao-shi-grpc

//todo：笔记



### 微服务

+ 工程项目下多模块+模块之间建立依赖关系

  

### IDEA全局设置不能走preference（快捷键#，）



### 脑中模拟开发流程 -> 见停发区域查询/

![image-20221214204746727](https://raw.githubusercontent.com/tuysss/cloudimg/main/Typora-Notes-images/2022/12/14/c305e9162b7ad0285328e647d232ae08-20221214204748-403363.png)





## 12.15

### <font color='#008080'>daily work achieve</font>

1. 学习宁，如何细致地做一件事，从头开始，那些在地基上省下的时间之后要加倍奉还的。比如上面这张架构图，我收到就“以为自己看不懂”，然后就不看了。其实，只要耐下性子来，半个小时的事情。后面琐碎的，多花了多少时间来来回回的看。差不多从11月初入职前发现的这个问题，再加上最近面试回答起基础问题，常常有这朵乌云在头顶——那些该成为基础的，我踏得很飘忽。不过也不要太着急，宝想要，宝努力，宝得到。从生活和工作的每一处中可以训练就好。
2. debug：今天观察别人帮我debug的心路历程（by搜索记录），发现也没什么不一样，就是经验+排查+搜索+落实。不过我想较而言欠缺的——相信自己一定能够解决的信心。常常半途而废。
3. 今天下午开始吧，拿到这个大项目，有点吓到，也有点平静。（因为通过之前的经验和正反馈相信这个问题是困难的，但是是我可以解决的。）从两点半起吧，现在是十点整，学习protocal buffer加上梳理一条业务链路，可以说能看懂这一条七八分了。爽爽的，加油～



### git reset 

+ git reset --hard [版本哈希]    //用于回退版本

+ 不带[版本哈希]     //回到第一次pull或者clone
+ 不带--hard           //用来从暂存恢复到工作区，场景：不小心add多了文件

### git分支管理再学习

https://www.runoob.com/git/git-branch.html



### maven常用指令 

`mvn clean`	清理项目生产的临时文件,一般是模块下的target目录
`mvn compile`	编译源代码，一般编译模块下的src/main/java目录





### Protobuf学习

#### 1. overview

https://developers.google.com/protocol-buffers/docs/overview

+ 是什么？
  + 语言/平台无关的，前后兼容的，像json但更小更快--产生原生语言binding
  + ”Protocol buffer“语义：是一门语言；是proto编译器将处理的代码；特定语言的运行时库；可写进文件的序列化数据
+ 解决了什么问题？
  + 给‘包’中的结构化数据提供了提供了序列化格式，以极小的size（megabytes）
  + 对即时网络传输和长期数据存储都通用
  + 结构：{message+service}都是在`.proto`文件中自定义
  + proto编译器在**build**时被调用

#### 2. 语法指南

+ 根据您的 `.proto` 生成的内容

  + 对于 **Java**，编译器会生成 `.java` 文件，其中包含每种消息类型的类，以及用于创建消息类实例的特殊 `Builder` 类。

+ 导入定义

  https://developers.google.com/protocol-buffers/docs/proto3#importing_definitions

+ 定义服务
  + 如果要将消息类型与 RPC（远程过程调用）系统配合使用，您可以在 `.proto` 文件中定义 RPC 服务接口，协议缓冲区编译器会生成以您选择的语言编写的服务接口代码和存根。

#### 3. proto java教程

一个[实例](https://developers.google.com/protocol-buffers/docs/javatutorial), 特别好。讲了：`java_package` ,`java_multiple_files`, `java_package`, and `java_outer_classname`.

1. 关于.proto文件的选项：

（因为weava标注在google页面不生效，摘录重要如下：）

+ 栗子中：The `.proto` file starts with a package declaration, which helps to prevent naming conflicts between different projects. In Java, the package name is used as the Java package unless you have explicitly specified a `java_package`, 
  + 语法中：`java_package` (file option): The package you want to use for your generated Java/Kotlin classes. 
  +  ！！java_package中的路径是target中生成的.class文件的路径，而非java文件路径。而这个java_package又与import关联着（抬头看上面红字import）
  + [关于protobuf文件几个参数的含义](https://blog.csdn.net/u010900754/article/details/105984161 )（package、java_package）——“包名的含义与平台语言无关，这个package仅仅被用在proto文件中用于区分同名的message类型。可以理解为message全名的前缀”

#### 4. personal questions

1. protobuf和grpc的关系？
   - ProtoBuf是一种序列化数据结构的协议
   - [protoc](https://www.zhihu.com/search?q=protoc&search_source=Entity&hybrid_search_source=Entity&hybrid_search_extra={"sourceType"%3A"answer"%2C"sourceId"%3A2522919604})是一个能把proto数据结构转换为各种语言代码的工具
   - RPC是一种通信协议
   - gRPC是一种使用ProtoBuf作为接口描述语言的一个RPC实现方案



### @resource和@autowired

<img src="https://raw.githubusercontent.com/tuysss/cloudimg/main/Typora-Notes-images/2022/12/15/e6b6974fea4ed820e3b66439dbad0324-20221215211150-d119ce.png" alt="image-20221215211148948" style="zoom:50%;" />

+ 二者都是做bean的注入时使用。@Autowired是spring提供的注解；@Resource是javax.annotation.Resource包中的，需要导入，但是spring支持。
+ @Autowired只能按照类型（byType）装配依赖对象。若想byType，需要结合@Quailfier注解使用
+ @Resource默认byName。可以通过设置name或者type属性=type，按type注入`@Resource(name="userDao")`

参考：https://www.cnblogs.com/think-in-java/p/5474740.html





## 12.16

### <font color='#008080'>daily work achieve</font>



### git

##### git fetch 切换远程分支到本地

https://www.jianshu.com/p/1958c55722a7

##### git stash

保存当前工作进度，会把暂存区和工作区的改动保存起来。执行完这个命令后，在运行`git status`命令，就会发现当前是一个干净的工作区，没有任何改动。

应用场景：开发到一半，有新需求要切换到另一个分支，但是现在还不想提交，使用stash保存当前的工作。之后可以`stash pop`恢复

git reset

### 领域模型

+ 针对业务，OO的
+ 相对于“数据模型”而言的



## es文档

### 一基础入门 

+  通过隐藏 Lucene 的复杂性，取而代之的提供一套简单一致的 RESTful API。
+ es的语义包括：一个分布式的实时文档**存储**，每个字段可以**被索引**和搜索；一个分布式实时**分析**搜索引擎；能胜任上百个服务节点的扩展。

#### 1.你知道的，为了搜索...

##### 和Elasticsearch交互

+ java中，使用es内置的客户端，通过9300端口并使用 Elasticsearch 的原生 *传输* 协议和集群交互
+ 所有其他语言，使用restful api通过9200端口，实际使用和任何http请求相同，by curl

##### 面向文档

+ 和关系型数据库对比：行和列存储不适合表现力丰富的大对象，且存储时需要将对象压缩成行，查询时又要重新构造成对象。
+ ES是面向文档的，即存储整个对象或文档。使用JSON作为文档的序列化格式。

#### 一个示例：索引员工文档

##### 存储

概念上的：

+ “索引”/index的语义：
  + 作名词，一个索引类似于关系型数据库的一个数据库
  + 作动词，类“存储”，类INSERT
+ *倒排索引*
  + <font color='red'>区别于B+树的</font>
  + 默认的，一个文档中的每一个属性都是 *被索引* 的（有一个倒排索引）和可搜索的

实际操作：

```json
PUT /megacorp/employee/1     //megacorp：索引(数据库),有关联的文档集合[_index] 
{                             //employee：类型,index下的逻辑分区     [_type]
    "first_name" : "John",    //1：雇员的ID                         [_id]
    "last_name" :  "Smith",
    "age" :        25,
    "about" :      "I love to go rock climbing",
    "interests": [ "sports", "music" ]
}
PUT /megacorp/employee/2
{
    "first_name" :  "Jane",
    "last_name" :   "Smith",
    "age" :         32,
    "about" :       "I like to collect rock albums",
    "interests":  [ "music" ]
}
```

##### 检索

1. 简单地执行 一个 HTTP `GET` 请求并指定文档的地址——索引库、类型和ID。 使用这三个信息可以*返回原始的 JSON 文档*：

```js
GET /megacorp/employee/1
```

2. 进入正式的搜索之轻量搜索：——搜索所有的雇员 ： _serach

```js
GET /megacorp/employee/_search
```

​	返回结果not原始文档，还要加上查询的范围结果分析

3. 查询表达式，Query_String，使用DSL，此处是JSon

   有点儿类似curl

4. 更复杂的搜索：<font color='brown'>结构化查询</font>、使用match+过滤器filter

5. 全文搜索：`match`。 按照相关性得分排序

6. 短语搜索：`match_phrase` 完全匹配的

7. 高亮搜索：在原来的查询基础上加一个``highlight`参数即可

8. **分析**

   + <font color='red'>聚合</font>功能aggregations。类似SQL中的`GROUP BY`但是更强大

   + e.g. 挖掘出员工中最受欢迎的兴趣爱好

     ```js
     GET /megacorp/employee/_search
     {
       "aggs": {
         "all_interests": {
           "terms": { "field": "interests" }
         }
       }
     }
     ```

     返回结果：

     ```json
     {
        ...
        "hits": { ... },
        "aggregations": {
           "all_interests": {
              "buckets": [
                 {
                    "key":       "music",
                    "doc_count": 2
                 },
                 {
                    "key":       "forestry",
                    "doc_count": 1
                 },
                 {
                    "key":       "sports",
                    "doc_count": 1
                 }
              ]
           }
        }
     }
     ```

#### 数据输入和输出

+ 在 Elasticsearch 中，术语 ***文档*** 有着特定的含义。它是指最顶层或者根**对象**。



### 深入搜索

##### 结构化搜索

+ 精确值查找——使用过滤器

  + 优势：
    + 执行快，cas不会计算相关度（跳过了评分阶段）
    + 很容易被缓存

+ 指令：

  + `term`:它接受一个字段名以及我们希望查找的数值

  + `constant_score`当查找一个精确值的时候，我们不希望对查询进行评分计算。只希望对文档进行包括或排除的计算

    ```sense
    GET /my_store/products/_search
    {
        "query" : {
            "constant_score" : { 
                "filter" : {
                    "term" : { 
                        "price" : 20
                    }
                }
            }
        }
    }
    ```

  



### 聚合

##### 高阶概念

***桶（Buckets）***

满足特定条件的文档的集合   相当于`GROUP BY color`

***指标（Metrics）***

对桶内的文档进行统计计算     相当于 `SELECT COUNT(color)` 、 `SUM()` 、 `MAX()` 等统计方法 

##### [一个聚合实例](https://www.elastic.co/guide/cn/elasticsearch/guide/current/_aggregation_test_drive.html),非常好！

```sense
GET /cars/transactions/_search
{
    "size" : 0,
    "aggs" : { 
        "popular_colors" : { 
            "terms" : { 
              "field" : "color"
            }
        }
    }
}
```

|      | 聚合操作被置于顶层参数 `aggs` 之下（如果你愿意，完整形式 `aggregations` 同样有效）。 |
| ---- | ------------------------------------------------------------ |
|      | 然后，可以为聚合指定一个我们想要名称，本例中是： `popular_colors` 。 |
|      | 最后，定义单个桶的类型 `terms` 。                            |



##### 聚合和搜索请求同时执行

##### 过滤——filter桶

+ better：在返回结果中过滤than对查询条件限定

##### Doc Values数据结构

+ bg：倒排索引只对某些操作是高效的
  + 倒排索引的优势——查找包含某个项的文档
+ Doc Values 通过序列化把数据结构持久化到磁盘，我们可以充分利用操作系统的内存，而不是 JVM 的 Heap

### 数据建模

##### 关联关系处理

+ 关系型数据库的局限性：
  + 对全文检索有限的支持能力
  + 实例关联查询的时间消耗很昂贵。特别是跨服务器进行实体关联时成本极其昂贵，基本不可用。
+ ES和大多数NoSQL数据库类似，是扁平化的。扁平化优势：
  + 索引过程是快速和无锁的。
    + ps.单个文档中的数据变更是ACID的，而涉及多个文档的事务不是。当一个事务部分失败时，无法回滚索引数据到前一个状态。
  + 搜索过程是快速和无锁的。
  + 因为每个文档相互都是独立的，大规模数据可以在多个节点上进行分布。
+ 模拟关系数据库：

### 分页

https://www.lidihuo.com/elasticsearch/elasticsearch-pagination.html



## 12.19

### 区分数据模型：DO、DTO、VO

<img src="https://raw.githubusercontent.com/tuysss/cloudimg/main/Typora-Notes-images/2022/12/19/d0bef0f8120db87fd6cb27b92a43d8fa-20221219191137-ac41ba.webp" alt="img" style="zoom:50%;" />

+ DTO：Data Transfer Object数据传输对象。
  + 横跨前端后端。在后端，是controller的java对象，在前端是js对象（json
+ VO：View Objec值对象。
  + 展示用的数据。纯前端。json
+ PO：Persistant object持久对象。
  + 数据库中的记录。一个PO的数据结构对应着库中表的结构。==Entity
+ BO：Business Object业务对象。
  + Bo就是PO的组合。有可能在业务层拼装，也可能框架跳过PO直接生成BO。
+ DO：在阿里巴巴开发手册中的定义，Data Object==PO





## 12.29

#### :: in Java

1. 是什么？

   如果Lambda要表达的函数方案已经存在于某个方法的实现中，那么则可以通过双冒号来引用该方法作为Lambda的替代者。也就是说，方法引用实际上是返回一个方法，而不是该方法的执行结果。

2. 基本语法

​	**构造器引用：**它的语法是Class::new，或者更一般的Class< T >::new实例如下：

```
final Car car = Car.create( Car::new );final List< Car > cars = Arrays.asList( car );
```

​	**静态方法引用：**它的语法是Class::static_method，实例如下：

```
cars.forEach( Car::collide );
```

​	**特定类的任意对象的方法引用：**它的语法是Class::method实例如下：

```
cars.forEach( Car::repair );
```

​	**特定对象的方法引用：**它的语法是instance::method实例如下：

```
final Car police = Car.create( Car::new );cars.forEach( police::follow ); 
```

3. 一个实例：

```java
  //获取合并后的停发管控区间
  Long mergedStartTime = unreachableOriginalAreaList.stream().filter(Objects::nonNull)
    .map(UnreachableAreaCheckDO::getStartTime)
    .filter(Objects::nonNull)
    .min(Long::compare)
    .orElseGet(System::currentTimeMillis);
```



## 1.3

#### Git rebase

[git实用技巧：将多次commit合并为一次](https://blog.csdn.net/vxzhg/article/details/105448190)

注意：使用git rebase之前必须先git stash保存当前的工作进度（包括暂存区和工作区）

#### Git chekout 丢弃修改

具体：当前工作区的所有修改，尚未暂存，丢弃

`git checkout .     //清除当前目录下所有没add的修改`

https://segmentfault.com/q/1010000005850900



## 1.4

#### 所有的“class not found”问题 -> 编译问题



#### Java继承 底层实现

https://www.cnblogs.com/swiftma/p/5537665.html

***实质：类加载***

参考：《深入理解JVM》第七章：类加载机制：

“Java类型的加载、链接、初始化都是在程序运行期间完成的，使得Java语言提前编译困难，但是提高了扩展性”——多态{实现接口方法、继承抽象类重写抽象方法、继承重写覆盖父类方法} 就体现在这里。

<img src="https://raw.githubusercontent.com/tuysss/cloudimg/main/Typora-Notes-images/2023/01/04/8a4efb09a3469128d62bfb439fcdcde9-20230104142759-84d476.png" alt="image-20230104142757629" style="zoom:50%;" />

（pps，类加载器在虚拟机外部，作用是图中第一步“加载”中的其中第一个动作——通过一个类的全限定名来获取描述该类的二进制字节 

流”）



#### 并发场景下的幂等问题 & 分布式锁

https://www.sohu.com/a/505277466_612370

“锁就是共享资源的互斥访问”





## 1.9

#### about中台

![image-20230109150954179](../../../Library/Application%20Support/typora-user-images/image-20230109150954179.png)

#### goland破解✅

https://www.bilibili.com/read/cv11179149

#### Git 待处理🅿️

https://blog.csdn.net/HeatDeath/article/details/79181025

still：搞清楚两个配置

#### Go学到：

https://go.dev/tour/flowcontrol/1



## 1.11

#### debug `NoSuchBeanDefinitionException`

`Neither @ContextConfiguration nor @ContextHierarchy found for test class`

栈底错误：

```bash
Caused by: org.springframework.beans.factory.NoSuchBeanDefinitionException: No qualifying bean of type 'com.kuaishou.kwaishop.scm.address.rich.client.AddressMetaRichReadClient' available: expected at least 1 bean which qualifies as autowire candidate. Dependency annotations: {@org.springframework.beans.factory.annotation.Autowired(required=true)}
```

test运行不起来。

解决：本服务中的Application类中添加包扫描。

<img src="https://raw.githubusercontent.com/tuysss/cloudimg/main/Typora-Notes-images/2023/01/11/f4cd44036d2f6bece605616a5bedeb59-20230111155113-e8386d.png" alt="image-20230111155111803" style="zoom:50%;" />

![image-20230111155143367](https://raw.githubusercontent.com/tuysss/cloudimg/main/Typora-Notes-images/2023/01/11/c4dad9ad9b2b1a9ee9c271e2b9890ceb-20230111155144-47e94b.png)

**总结`NoSuchBeanDefinitionException`解决思路：**

1. 检查：要注入bean容器的类（之后将被@autowired的）是否添加了@Component注解
2. 检查：@Service注解要加在实现类上，是否错误地添加到了接口上
3. 最后，检查是否添加了包扫描路径



#### 缓存预热

https://zq99299.github.io/note-book/cache-pdp/069.html#%E7%BC%93%E5%AD%98%E9%A2%84%E7%83%AD%E5%9F%BA%E6%9C%AC%E6%80%9D%E8%B7%AF



#### debug： git提交远程仓库被拒绝

最近遇到两次。

报错信息：

```bash
git@github.com: Permission denied (publickey).
fatal: Could not read from remote repository.

Please make sure you have the correct access rights
and the repository exists.
```

解决：

1.是否能连接到远程仓库

```bash
ssh -T git@git.corp.kuaishou.com
```

或者

```bash
ssh -vT git@github.com
```

2. 分情况
   1. 连接不上（即本次github场景）：因为之前重新生成了ssh key，可能被本机判断为全局的，覆盖了之前rsa_github.pub，把这个key填到github设置中即可
   2. 能连接上（场景：clone、pull都可以，提交业务代码至gitlab仓库中失败）：developer权限到期。



#### 写一下实习项目部分的简历：

https://mp.weixin.qq.com/s/wfabzdpOPdq89faFFpZ4NA



## 1.12

#### debug 调试接口 502gateway错误

原因：没有在request header中配置测试用泳道，因为主干中还没有这个grpc服务。

参考：https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/download%2Fpdf%2F149549%2F%25E6%259C%2580%25E4%25BD%25B3%25E5%25AE%259E%25E8%25B7%25B5_cn_zh-CN.pdf



#### git冲突：从远程fecth后，要和本地代码merge，出现merge conflict问题

分情况，

1. 确定远程分支是需要的，那么丢弃本地分支内容

   `git reset --hard origin/master`

2. 不能丢弃本地修改，那么需要对unmerged的文件进行手动修改，删掉其中冲突的部分，然后运行如下命令

   `$:git add filename
   $:git commit -m "message"`

参考：https://blog.csdn.net/nonfuxinyang/article/details/77206486

