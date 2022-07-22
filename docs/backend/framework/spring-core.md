# spring 核心

```desc
date: 2022-07-04
address: NC
author: 吴第广
```

## IoC 容器

### IoC容器和 Beans
>IoC 也叫依赖注入（DI）。这是一个对象仅通过构造函数参数、工厂方法的参数或在对象实例构造或工厂方法返回后在对象实例上设置的属性来定义依赖关系（即，它们处理的其他对象）的过程。然后，容器在创建bean时注入这些依赖项。这个过程基本上是bean本身的逆过程（因此称为控制反转），通过使用类的直接构造或服务定位器模式等机制来控制其依赖项的实例化或者位置

`org.springframework.bean` 和`org.springframework.context` 包是 Spring `IoC` 容器的基础`BeanFactory` 接口提供一种高级配置机制，能够管理任何的对象。`ApplicationContext` 是`BeanFactory` 的子接口。它补充了：

1. 更容易与 AOP 功能集成
2. 消息资源处理（用于国际化）
3. 事件发布
4. 应用层特定上下文，如 `WebApplicationContext`

简单来说，BeanFactory 提供了配置框架和基本功能，而ApplicationContext 添加了更多特定于企业的功能。ApplicationContext是BeanFactory的完整超集。

在Spring中，构成应用程序主干并由Spring IoC容器管理的对象称为Bean。Bean是由Spring IoC容器实例化、组装和管理的对象，否则，Bean只是应用程序中许多对象中的一个。Bean以及它们之间的依赖关系反映在容器使用的配置元数据中。

### 1.2. 容器概览

`org.springframework.context.ApplicationContext`接口表示Spring IoC 容器，并负责实例化、配置和组装Bean。容器通过读取配置元数据获取关于实例化、配置和组装哪些对象的指令。

配置元数据以XML、Java注解或Java代码表示。它可用让我们表达组成应用程序的对象以及这些对象之间丰富的相互依赖关系。`Spring` 提供了`ApplicationContext` 接口的几个实现。在独立应用程序中，创建`ClassPathXmlApplicationContext` 或者`FileSystemXmlApplicationContext`。虽然XML一直是定义配置元数据的传统格式，但是我们可以通过提供少量XML配置来声明支持这些额外的元数据格式，从而指示容器使用Java注解或代码作为元数据格式。

![20220722163329](https://cdn.jsdelivr.net/gh/wudg/picgo@master/images/books/20220722163329.png)

#### 1.2.1. 配置元数据

P4


## 资源

## 验证、数据绑定和类型转换

## Spring表达式语言 -- SpEL

## 面向切面编程

## AOP 的详细

## 零安全

## 数据缓冲区和编解码器

## 日志

## 附录