# `Spring是什么`

> -- 轻量级： Spring是非入侵的
>
> -- 依赖注入  （DI依赖注入、IOC控制反转）
>
> -- 面向切面编程（AOP）（Aspect Oriented Programming）
>
> -- 容器
>
> -- 框架
>
> -- 一站式

Spring是一个企业级的开发框架，是软件设计层面的框架，优势在于可以将应用程序进行分层，开发者可以自足选择组件。

MVC ： Struct2， Spring MVC

ORMapping ： Hibernate、 MyBatis、 Spring Data

## `IOC和DI`

> IOC （inversion反转    of control） 思想是：  反转资源获取的方向。容器主动地将资源推送给它所管理的组件，组件所要做的仅是选择一种合适的方式来接受资源。    这种行为也被称为查找的被动形式。
>
> DI (Dependency依赖   Injection注入)   ---- IOC 的另一种表达方式(如setter方法)， 即：  组件以一些预先定义好的方式接受来自如容器的资源注入。

# `Spring中Bean`

Spring 支持3种依赖注入的方式：

- 属性注入
- 构造器注入
- 工厂方法注入（很少用）

> 属性注入即通过setter方法注入Bean的属性值或依赖的对象
>
> 属性注入是实际应用中最常见的注入方式



## `什么是控制反转`

在Spring框架中创建对象的工作不再由调用者来完成，而是交给IOC容器来创建，在推送给调用者，这个流程完成反转，所以称为反转控制。

### `配置文件`

- 通过配置bean标签来完成对象的管理

  - id： 对象名

  - class：对象的模版类。（所以交给IOC容器来管理的类必须有无参构造函数），因为底层是通过反射机制来创建呢对象，调用的是无参构造

- 对象的成员变量通过property标签来完成赋值。

  - name： 成员变量名
  - value：  成员变量值（基本数据类型，String可以直接赋值，如果是其他引用类型，不能通过value赋值）
  - ref：  将IOC中的另一个bean 赋给当前的成员变量 （DI）



### `IOC的底层原理`

- 读取配置文件，解析XML。
- 通过反射机制，实例化配置文件中所配置的所以的bean。
- IOC底层实现 https://www.bilibili.com/video/av62689841/?p=3