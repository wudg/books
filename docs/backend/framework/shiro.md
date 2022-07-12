# Apache Shiro

**官网**：[进入](https://shiro.apache.org/index.html)

## 入门

### 1.1 介绍

Apache Shiro是一个强大且易用的Java安全框架,执行身份验证、授权、密码和会话管理。使用Shiro的易于理解的API,您可以快速、轻松地获得任何应用程序,从最小的移动应用程序到最大的网络和企业应用程序。

### 核心组件

![20220711155044](https://cdn.jsdelivr.net/gh/wudg/picgo@master/images/books/20220711155044.png)

三个核心组件：

1. Subject：当前操作用户（当前和系统交互的东西）
2. SecurityManager：管理所有用户的安全操作。`Shiro`通过`SecurityManager`管理内部组件实例，并通过它来提供安全管理的各种服务
3. Realms：充当了`Shiro`与应用安全数据间的“桥梁或者“连接器”。当用户执行认证和授权验证时，`Shiro`会从应用配置的`Realms`中查找用户及其权限信息

Realm实质上是一个安全相关的DAO，它封装了数据源的连接细节，并在需要时将相关数据源提供给Shiro。当配置Shiro时，必须至少指定一个Realm用于认证和授权，Realm可配置多个

## spring-boot 集成

**源码：**[进入](https://github.com/wudg/main-framework.git)