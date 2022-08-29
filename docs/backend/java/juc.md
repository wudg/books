# 《并发编程》

## 前言

### 为什么要看源码

做项目遇到下面问题：

1. 不知道如何去设计
2. 设计时，问题考虑步骤

经验不足，而经验主要从项目实践中积累。工作经验的积累来自于年限与实践，然后看源码开源扩展我们的思路，变相增加我们的经验。虽然不能在短时间内荣国实践积累经验，但是可以通过学习开源框架、开源项目来获取经验

进入职场一般都要先熟悉现有系统

使用框架或者工具做开发时，如果对它足够了解，就能最大化地减少出故障的可能。如ArrayBlockingQueue里面关于元素入队有offer()和put()
* offer()：当队列满了就会丢弃要入队地元素，之后offer方法会返回false
* put()：当队列满了，则会挂起当前线程，之道队列有空闲，元素入队成功后才返回

看源码最大的好处是可以开阔思路，提升架构设计能力。有些东西仅靠书本和自己思考是很难学到的，必须通过看源码，看别人如何设计，然后思考为何这样设计。

能力的提升不在于写了多少代码，做了多少项目，而在于给你一个业务场景时，是否能拿出几种靠谱的解决方案，并且说出各自的优缺点。而如何才能拿出来，一来靠经验，二来靠归纳总结，而看源码可以快速增加经验

### 如何看源码

先 Google 查找这个开源框架的官方介绍，通过资料了解该框架有几个模块，各个模块是做什么的，之间有什么联系，每个模块都有那些核心类，在阅读源码时开源着重看这些类

然后对哪个模块感兴趣就去写个小demo，先了解一下这个模块地具体作用，然后再debug进入看具体实现。在debug过程中，第一遍走马观花，简略看一下调用逻辑，都用了哪些类。第二遍需要重点地debug，看看这些类担任了架构图里的哪些功能，使用了哪些设计模式。如果第二遍有感觉了，便大致知道了整体代码地功能实现，但是对整个代码结构还不是很清晰，毕竟代码里面多个类来回调用，很容易遗忘当前断电地来处。那么开源进行第三遍debug，这时最好把主要类地调用时序图以及类图结构画出来，等画好后，再对着时序图分析调用流程，就可以清楚地之道类之间的调用关系，而通过类图可以之道类地功能以及它们相互之间的依赖关系。

另外，开源框架里面每个功能类或者方法一般都有注释，这些注释是一手资料，比如JUC包里的一些并发组件地注释，就已经说明了它们地设计原理和使用场景

在阅读源码时，最好画出时序图和类图，因为人总是善忘地。如果隔一段时间你再去看之前看过的源码，虽然有些印象，但当你想去看某个模块地逻辑时，又需根据demo再从头debug。而如果有个这两张图，就可以从这两张图里面直接找，并且看一眼时序图就知道整个模块地脉络了。

此外，查框架使用说明最好去官网查

当然，研究代码时不一定非要debug三遍，其实这里说的是三种掌握程度，如果一遍就能掌握，那自然更好

## 第一章 并发编程线程基础

* 并发编程线程基础
* 并发编程的其他概念与原理解析

### 什么是线程

线程是进程中的一个实体，线程本身是不会独立存在的。

* 进程：代码在数据集合上的一个运行活动，是系统进行资源分配和调度的基本单位
* 线程：进程的一个执行路径，一个进程中至少有一个线程，进程中的多个线程共享进程的资源

操作系统在分配资源时是把资源分配给进程的，但是CPU资源比较特殊，它是被分配到线程的，因为真正要占用CPU运行的是线程，所以也说线程是CPU分配的基本单位

在Java中，当我们启动main函数时其实就启动了一个JVM进程，而ain函数所在的线程就是这个进程的一个线程，也叫主线程



## 第二章 并发编程的其他基础知识

## 第三章 Java并发包中ThreadLocalRandom类原理剖析

## 第四章 Java并发包中原子操作类原理剖析

## 第五章 Java并发包中List源码剖析

## 第六章 Java并发包中锁原理剖析

## 第七章 Java并发包中并发队列原理剖析

## 第八章 Java并发包中线程池ThreadPollExecutor原理探究

## 第九章 Java并发包中ScheduledThreadPoolExecutor原理探究

## 第十章 Java并发包中线程同步器原理剖析

## 第十一章 ArrayBlockingQueue的使用