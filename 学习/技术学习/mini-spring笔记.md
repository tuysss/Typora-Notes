#  IoC

### 1-1 bean容器：beanFactory保存bean

一个简单的bean容器——beanFactory，内部包含一个map用以保存bean，只有注册bean和获取bean两个方法

从模型上看，Bean-Container是个近似Map<name,object>

### 1-2 定义bean & 在注册表（map中）注册它

注册表是一个Map<beanName,BeanDefination>,其中BeanDefination类记录了bean的基本信息，包括class类型、方法构造参数、是否为单例等

+ **综合来说步骤就两步：**
  1. <font color='brown'>**在注册表中注册这个bean：registerBeanDefinition(beanName, beanDefination)**</font>
  2. <font color='brown'>**需要用的时候，getBean() -> createBean() ->  getBeanDefination() -> 从map中get** </font>
+ ps bean和class的区别？
  + Java Bean 实际是就是一个普通的 Java Class，但是需要满足三个要求，使得这个类必须是具体的和公共的
    - 所有属性为 private，只允许通过 setXXX, getXXX 进行操作
    - 一个不需要初始化参数的 constructor
    - 实现了 Serializable

### 1-3 示例化策略

1. 简单的。反射，根据bean的无参构造函数实例化对象
2. 使用CGLIB动态生成子类

##### 	CGLIB？

+ （背景）由于JDK的动态代理只能对接口进行代理，CGLIB弥补了这个缺陷（原理）by：动态生成一个要代理类的子类并重写方法，在子类中使用方法拦截拦截父类方法的调用，顺势植入横切逻辑。（底层）使用字节码处理框架ASM，（优势）thereby：比java反射的JDK代理更快。



### 1-4 为bean填充属性

1. 在BeanDefination中添加PropertiesValue，记录属性列表
2. 在`AbstractAutowireCapableBeanFactory`中添加`applyPropertyValues`方法，通过反射设置属性。



### 1-5 为bean注入bean

不考虑循环依赖，

如果beanA依赖beanB，则在为beanA注入属性的时候，先示例化beanB，再通过反射设置属性



### 1-6 资源和资源加载器

+ Resource是资源的抽象和访问接口，简单写了三个实现类{classpath+文件系统+url}
+ ResourceLoader接口则是资源查找定位策略的抽象，DefaultResourceLoader是其默认实现类

为下一part做准备



### 1-7 在xml文件中定义bean

这一步的意义是，在实际应用（测试类）中，不再手动注册beanDefinition，而是从xml文件中读（和解析、注册）



### 1-8 BeanFactoryPostProcessor和BeanPostProcessor

`BeanFactoryPostProcessor`是spring提供的容器扩展机制，允许我们在bean实例化之前修改bean的定义信息即BeanDefinition的信息

`BeanPostProcessor`在bean实例化后修改bean或替换bean。BeanPostProcessor是后面实现AOP的关键。



### 1-9 应用上下文ApplicationContext

+ BeanFactory是spring的基础设施，面向spring本身；而ApplicationContext面向spring的使用者

+ 从bean的角度看，目前的生命周期：

  <img src="https://raw.githubusercontent.com/tuysss/cloudimg/main/Typora-Notes-images/2023/01/05/661f4926a4870e9d632aff1680583296-20230105115312-8997f7.png" alt="img" style="zoom:100%;" />
