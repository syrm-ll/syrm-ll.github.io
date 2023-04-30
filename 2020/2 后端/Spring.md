# Spring 简介



# 依赖,模块

Spring-context



# 选用日志框架

Spring 默认使用了 `commons-logging` 如果需要切换到 SLF4J + Logback, 需要排除 `commons-logging` 添加 `jcl-over-slf4j`

```groovy
configurations {
//    排除 spring 的 默认日志实现
    compile.exclude group:'commons-logging'
}

dependencies {
    implementation group: 'org.slf4j', name: 'jcl-over-slf4j', version: '2.0.0-alpha1'
    implementation group: 'org.slf4j', name: 'slf4j-api', version: '2.0.0-alpha1'
    implementation group: 'ch.qos.logback', name: 'logback-classic', version: '1.3.0-alpha5'
}
```



# 配置文件: Application.xml

## 组件: Bean

示例:

```xml
<!-- 注入方式有两种, 构造注入 或者set 注入, 写子标签即可 -->
<bean id="user" class="cn.bingdie.bean.User" scope="prototype" >
        <property name="id" value="10001"/>
</bean>
```

参数注入可以使用`property` 或 `constructor-arg` .

可以注入字符串,数值, 数组, Map等, 具体方法略

**C, P 命名空间注入:**

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       <!-- 导入明明空间 -->
       xmlns:p="http://www.springframework.org/schema/p"
       xmlns:context="http://www.springframework.org/schema/context"
       xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd http://www.springframework.org/schema/context https://www.springframework.org/schema/context/spring-context.xsd">

    ...

<bean id="user45" class="cn.bingdie.bean.User" p:id="100001" p:name="冰凝" p:age="188"/>
```

P: 属性注入, C: 构造注入


## 别名: alias

就是给 配置的 bean 取个别名. 没啥特殊. 注意作用范围: 单例, 原型

示例:

```xml
<alias name="user" alias="user2"/>
```



## 导入配置文件: import

导入其他配置文件

示例:

```xml
<import resource="Application.xml"/>
```

## 自动装配依赖

### 属性自动注入

#### XML 配置

```xml
<bean id="user" class="" scope="prototype"  autowire="byName | byType | default" />
```

#### 注解 配置

可用的注解:

- `@Resource`: `javax.annotation` 注解, 先按照 byName 寻找, 没有就 byType , 还没有报错.
- `@Autowired`: 自动注入, 按照byType 寻找, 如果没有报错 (可以指定参数, 允许为`NULL`).
- `@Qualifier`: 配合 `@Autowired` 使用, 在类型冲突时候指定唯一名字.
- `@Required` : 必须 (@sinece 5.1 之后启用)



> 属性的自动注入, 依赖  `context:annotation-config` 开启

---

### 组件的自动注入

Spring 组件

- `@Component` 组件

- `@Repository` 资料库: DAO

- `@Service` 服务: Service 层

- `@Controller` 控制器: Controller

上面几个注解作用完全相同,  不同在于 `@Service` 等提供了自解释的语义

> 以上注解生效依赖于注解扫描开启, 即:  `context:component-scan`
>
> `@Controller` 还需要 `mvc:annotation-driven`

## 依赖自动装配的设置

 `context:annotation-config`

> **注意:** 在拥有 `context:component-scan` 的情况下, 不需要这个配置项.

作用:  激活那些已经在spring容器里注册过的bean上面的注解，也就是显示的向Spring注册

```config
AutowiredAnnotationBeanPostProcessor
CommonAnnotationBeanPostProcessor
PersistenceAnnotationBeanPostProcessor
RequiredAnnotationBeanPostProcessor
```

这四个Processor，注册这4个BeanPostProcessor的作用，就是为了你的系统能够识别相应的注解。BeanPostProcessor就是处理注解的处理器。

> - 比如我们要使用@Autowired注解，那么就必须事先在 Spring 容器中声明 AutowiredAnnotationBeanPostProcessor Bean。传统声明方式如下
>
> ```xml
> <bean class="org.springframework.beans.factory.annotation.AutowiredAnnotationBeanPostProcessor "/>
> ```
>
> - 如果想使用@ Resource 、@ PostConstruct、@ PreDestroy等注解就必须声明CommonAnnotationBeanPostProcessor。传统声明方式如下
>
> ```xml
> <bean class="org.springframework.beans.factory.annotation.CommonAnnotationBeanPostProcessor"/> 
> ```
>
> - 如果想使用@PersistenceContext注解，就必须声明PersistenceAnnotationBeanPostProcessor的Bean。
>
> ```xml
> <bean class="org.springframework.beans.factory.annotation.PersistenceAnnotationBeanPostProcessor"/> 
> ```
>
> - 如果想使用 @Required的注解，就必须声明RequiredAnnotationBeanPostProcessor的Bean。
>
> 同样，传统的声明方式如下：
>
> 
>
> ```xml
> <bean class="org.springframework.beans.factory.annotation.RequiredAnnotationBeanPostProcessor"/> 
> ```
>
> 一般来说，像@ Resource 、@ PostConstruct、@Antowired这些注解在自动注入还是比较常用，所以如果总是需要按照传统的方式一条一条配置显得有些繁琐和没有必要，于是spring给我们提供< context:annotation-config/>的简化配置方式，自动帮你完成声明。
>
> 思考1：假如我们要使用如@Component、@Controller、@Service等这些注解，使用能否激活这些注解呢?
>
> 答案：单纯使用< context:annotation-config/>对上面这些注解无效，不能激活！



 `context:component-scan`

示例配置:

```xml
<context:component-scan base-package="">
        <context:include-filter type="" expression=""/>
        <context:exclude-filter type="" expression=""/>
</context:component-scan>
```

该配置项其实也包含了自动注入上述 四个processor 的功能，因此当使用 `< context:component-scan/>` 后，就可以将` < context:annotation-config/>` 移除了。

通过对**base-package**配置，就可以把controller包下 service包下 dao包下的注解全部扫描到了！ 

> 包含过滤器, 排除过滤器 (include-filter 和 exclude-filter) 同时只能配置一个



 `mvc:annotation-driven`

Spirng MVC 专用,` < mvc:annotation-driven /> `是一种简写形式，完全可以手动配置替代这种简写形式，简写形式可以让初学都快速应用默认配置方案。 会自动注册`DefaultAnnotationHandlerMapping`与`AnnotationMethodHandlerAdapter` 两个bean,是spring MVC为`@Controllers`分发请求所必须的。 
并提供了：数据绑定支持，`@NumberFormatannotation` 支持，`@DateTimeFormat`支持，`@Valid`支持，读写XML的支持（JAXB），读写JSON的支持（Jackson）。




## 配置 AOP

### 注意事项

添加依赖

```shell
# gradle
implementation "org.aspectj:aspectjweaver:1.9.4"
```



> 注意:
>
> SSM项目中, 因为 Spring 容器的上下级关系, Controller 的 AOP配置必须放置在MVC的配置文件中



### 使用Spring API

添加实现 Spring 的接口的"通知"

```java
@Component
public class Log implements MethodBeforeAdvice, AfterReturningAdvice {}
```

> MethodBeforeAdvice: 方法执行之前
>
> AfterReturningAdvice: 方法返回之后
>
> 更多参考 `aop` 包内其他类

配置 `ApplicationContext.xml` 

```xml
<aop:config>
    <!-- 切入点 -->
    <aop:pointcut id="logPointcut" expression="execution(void cn.bingdie.service.UserServiceImpl.*())"/>
    <!-- 通知 -->
    <aop:advisor advice-ref="<引用切面类>" pointcut-ref="logPointcut"/>
</aop:config>
```







### 使用自定义类, 定义切面

```java
public class DivPoint {}
```

配置 `ApplicationContext.xml`

```xml
<!-- 使用自定义的切面类 -->
<aop:config>
    <!-- 切面 -->
    <aop:aspect id="" ref="div">
        <!-- 切入点 -->
        <aop:pointcut id="divLog" expression="execution(* cn.bingdie.service..*.*(..))"/>
        <!-- 通知 -->
        <aop:before method="之前" pointcut-ref="divLog"/>
        <aop:after method="之后" pointcut-ref="divLog" />
    </aop:aspect>
</aop:config>
```

### 使用注解

开启注解支持:

```xml
<!-- ApplicationContext.xml -->
<!-- proxy-target-class 为 true 使用cglib, 默认为false 使用jdk -->
<aop:aspectj-autoproxy proxy-target-class=" false (默认) | true" />
```

使用注解定义切面, 可用注解有:

```java
@Before
@After
@AfterReturning
@AfterThrowing
@Around // 环绕 执行顺序很奇怪, 在所有 before 之前, 在所有 after 之前
```

示例: 

```java
@Aspect
@Component
@Slf4j
public class AnnotationPoint {

    @Pointcut("execution(* cn.bingdie.service.*.*(..))")
    public void defaultPoint() {}

    @Before("defaultPoint()")
    public void before () {
        log.error("使用注解, 方法之前");
    }

    @After("defaultPoint()")
    public void after() {
        log.error("使用注解, 方法之后");
    }
}
```



## 配置声明式事务: XML(略)

​	



# 使用注解开发

```java
@Configuration
@Import(MybatisSpringConfig.class)
@ComponentScan(value = "cn.bing.die.test",
        excludeFilters = { @ComponentScan.Filter(Controller.class), @ComponentScan.Filter(ControllerAdvice.class)
})
@EnableTransactionManagement
```

等

## 配置声明式事务: 注解 (TODO)





# 启动

## XML配置文件启动

传入的配置文件可以是多个.

```java
ApplicationContext context = new ClassPathXmlApplicationContext("Application.xml");
context.getBean();
```

## 纯注解配置启动

```java
ApplicationContext context = new AnnotationConfigApplicationContext(SpringConfig.class);
```

## 跟随Web容器启动

```xml
<!-- web.xml -->

<listener>
    <listener-class>org.springframework.web.context.ContextLoaderListener</listener-class>
</listener>

<context-param>
	<param-name>contextConfigLocation</param-name>
	<param-value>classpath:Application.xml</param-value>
</context-param>
```

