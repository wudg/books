# Arthas

[官方文档](https://arthas.aliyun.com/doc/quick-start.html)

[用户案例](https://github.com/alibaba/arthas/issues?q=label%3Auser-case)

使用场景
* 接口性能优化：使用trace命令输出方法路径上的每个节点上耗时
* 多线程WeakHashMap引起的死循环cpu跑满：sc -d 查看当前类来自哪个jar包

## 快速入门

### 启动math-game

```shell
curl -O https://arthas.aliyun.com/math-game.jar
java -jar math-game.jar
```

math-game是一个简单的程序，每隔一秒生成一个随机数，再执行质因数分解，并打印出分解结果。

math-game源代码：[查看](https://github.com/alibaba/arthas/blob/master/math-game/src/main/java/demo/MathGame.java)

### 启动arthas

```shell
curl -O https://arthas.aliyun.com/arthas-boot.jar
java -jar arthas-boot.jar
```

选择应用Java进程：

```shell
java -jar arthas-boot.jar
[INFO] arthas-boot version: 3.6.2
[INFO] Found existing java process, please choose one and input the serial number of the process, eg : 1. Then hit ENTER.
* [1]: 11840 org/netbeans/Main
  [2]: 12496 org.jetbrains.jps.cmdline.Launcher
  [3]: 16784 com.wudg.springbootarthas.SpringbootArthasApplication
  [4]: 18136 org.jetbrains.idea.maven.server.RemoteMavenServer36
  [5]: 8056 math-game.jar
  [6]: 17804
5
[INFO] Start download arthas from remote server: https://arthas.aliyun.com/download/3.6.2?mirror=aliyun
[INFO] File size: 12.88 MB, downloaded size: 8.88 MB, downloading ...
[INFO] Download arthas success.
[INFO] arthas home: C:\Users\admin\.arthas\lib\3.6.2\arthas
[INFO] Try to attach process 8056
[INFO] Attach process 8056 success.
[INFO] arthas-client connect 127.0.0.1 3658
  ,---.  ,------. ,--------.,--.  ,--.  ,---.   ,---.
 /  O  \ |  .--. ''--.  .--'|  '--'  | /  O  \ '   .-'
|  .-.  ||  '--'.'   |  |   |  .--.  ||  .-.  |`.  `-.
|  | |  ||  |\  \    |  |   |  |  |  ||  | |  |.-'    |
`--' `--'`--' '--'   `--'   `--'  `--'`--' `--'`-----'

wiki       https://arthas.aliyun.com/doc
tutorials  https://arthas.aliyun.com/doc/arthas-tutorials.html
version    3.6.2
main_class
pid        8056
time       2022-07-06 11:35:18

[arthas@8056]$
```

### 查看dashboard

输入`dashboard`命令回车，会展示当前进程的信息

```shell
[arthas@8056]$ dashboard
ID   NAME                          GROUP          PRIORITY  STATE    %CPU      DELTA_TIM TIME      INTERRUPT DAEMON
-1   C1 CompilerThread3            -              -1        -        0.0       0.000     0:0.421   false     true
-1   C2 CompilerThread1            -              -1        -        0.0       0.000     0:0.218   false     true
-1   C2 CompilerThread2            -              -1        -        0.0       0.000     0:0.156   false     true
-1   C2 CompilerThread0            -              -1        -        0.0       0.000     0:0.140   false     true
1    main                          main           5         TIMED_WA 0.0       0.000     0:0.078   false     false
22   arthas-NettyHttpTelnetBootstr system         5         RUNNABLE 0.0       0.000     0:0.062   false     true
-1   GC task thread#8 (ParallelGC) -              -1        -        0.0       0.000     0:0.031   false     true
-1   GC task thread#7 (ParallelGC) -              -1        -        0.0       0.000     0:0.031   false     true
-1   GC task thread#6 (ParallelGC) -              -1        -        0.0       0.000     0:0.031   false     true
-1   GC task thread#0 (ParallelGC) -              -1        -        0.0       0.000     0:0.031   false     true
-1   GC task thread#9 (ParallelGC) -              -1        -        0.0       0.000     0:0.031   false     true
Memory                    used    total    max     usage    GC
heap                      17M     163M     3604M   0.49%    gc.ps_scavenge.count          2
ps_eden_space             542K    65024K           0.04%    gc.ps_scavenge.time(ms)       14
ps_survivor_space         0K      10752K   10752K  0.00%    gc.ps_marksweep.count         1
ps_old_gen                17M     89M      2703M   0.63%    gc.ps_marksweep.time(ms)      21
nonheap                   28M     29M      -1      97.03%
code_cache                5M      5M       240M    2.19%
metaspace                 20M     21M      -1      96.82%
compressed_class_space    2M      2M       1024M   0.25%
Runtime
os.name                                                     Windows 10
os.version                                                  10.0
java.version                                                1.8.0_301
java.home                                                   E:\env\jdk\jre
systemload.average                                          -1.00
processors                                                  12
timestamp/uptime                                            Wed Jul 06 11:38:04 CST 2022/182s
```

### 通过thread命令来获取math-game进程的Main Class

`thread 1`会打印线程ID 为 1的栈，通常是main函数的线程

```shell
thread 1 | grep 'main('
    at demo.MathGame.main(MathGame.java:17)
```

### 通过jad来反编译Main Clas

```shell
jad demo.MathGame

ClassLoader:
+-sun.misc.Launcher$AppClassLoader@5c647e05
  +-sun.misc.Launcher$ExtClassLoader@28d93b30

Location:
/C:/Users/admin/Downloads/math-game.jar

       /*
        * Decompiled with CFR.
        */
       package demo;

       import java.util.ArrayList;
       import java.util.List;
       import java.util.Random;
       import java.util.concurrent.TimeUnit;

       public class MathGame {
           private static Random random = new Random();
           private int illegalArgumentCount = 0;

           public static void main(String[] args) throws InterruptedException {
               MathGame game = new MathGame();
               while (true) {
/*16*/             game.run();
/*17*/             TimeUnit.SECONDS.sleep(1L);
               }
           }

           public void run() throws InterruptedException {
               try {
/*23*/             int number = random.nextInt() / 10000;
/*24*/             List<Integer> primeFactors = this.primeFactors(number);
/*25*/             MathGame.print(number, primeFactors);
               }
               catch (Exception e) {
/*28*/             System.out.println(String.format("illegalArgumentCount:%3d, ", this.illegalArgumentCount) + e.getMessage());
               }
           }

           public static void print(int number, List<Integer> primeFactors) {
               StringBuffer sb = new StringBuffer(number + "=");
/*34*/         for (int factor : primeFactors) {
/*35*/             sb.append(factor).append('*');
               }
/*37*/         if (sb.charAt(sb.length() - 1) == '*') {
/*38*/             sb.deleteCharAt(sb.length() - 1);
               }
/*40*/         System.out.println(sb);
           }

           public List<Integer> primeFactors(int number) {
/*44*/         if (number < 2) {
/*45*/             ++this.illegalArgumentCount;
                   throw new IllegalArgumentException("number is: " + number + ", need >= 2");
               }
               ArrayList<Integer> result = new ArrayList<Integer>();
/*50*/         int i = 2;
/*51*/         while (i <= number) {
/*52*/             if (number % i == 0) {
/*53*/                 result.add(i);
/*54*/                 number /= i;
/*55*/                 i = 2;
                       continue;
                   }
/*57*/             ++i;
               }
/*61*/         return result;
           }
       }

Affect(row-cnt:1) cost in 375 ms.
```

### watch

通过`watch`命令来查看`demo.MathGame#primeFactors`函数返回值

```shell
 watch demo.MathGame primeFactors returnObj
Press Q or Ctrl+C to abort.
Affect(class count: 1 , method count: 1) cost in 78 ms, listenerId: 1
method=demo.MathGame.primeFactors location=AtExit
ts=2022-07-06 11:42:20; [cost=0.653ms] result=@ArrayList[
    @Integer[2],
    @Integer[5],
    @Integer[11],
    @Integer[19],
    @Integer[29],
]
method=demo.MathGame.primeFactors location=AtExit
ts=2022-07-06 11:42:21; [cost=0.1287ms] result=@ArrayList[
    @Integer[2],
    @Integer[2],
    @Integer[2],
    @Integer[5519],
]
method=demo.MathGame.primeFactors location=AtExit
ts=2022-07-06 11:42:22; [cost=0.0179ms] result=@ArrayList[
    @Integer[37],
    @Integer[59],
    @Integer[79],
]
method=demo.MathGame.primeFactors location=AtExit
ts=2022-07-06 11:42:23; [cost=0.052ms] result=@ArrayList[
    @Integer[2],
    @Integer[3],
    @Integer[7],
    @Integer[13],
    @Integer[59],
]
method=demo.MathGame.primeFactors location=AtExit
ts=2022-07-06 11:42:24; [cost=0.0494ms] result=@ArrayList[
    @Integer[3],
    @Integer[7],
    @Integer[7],
    @Integer[7],
    @Integer[11],
    @Integer[11],
]
```

### 退出arthas

退出连接：`quit`或`exit`
退出arthas：stop

## 进阶使用

### 基础命令

* help：查看命令帮助信息
* cat：打印文件内容
* echo：打印参数
* grep：匹配查找
* base64：base64编码转换
* tee：复制标准输入到标准输出和指定的文件
* pwd：返回当前的工作目录
* cls：清空当前屏幕区域
* session：查看当前会话信息
* version：输出当前arthas版本
* history：打印命令历史
* quit：退出当前连接
* stop：关闭arthas服务端
* keymap：arthas快捷键列表及自定义快捷键

### jvm相关

* dashboard：查看当前系统实时数据面板

```
参数名称	参数说明
[i:]	刷新实时数据的时间间隔 (ms)，默认5000ms
[n:]	刷新实时数据的次数
```


```shell
[arthas@12796]$ dashboard
ID      NAME                                            GROUP                   PRIORITY        STATE           %CPU            DELTA_TIME      TIME            INTERRUPTED     DAEMON
51      DestroyJavaVM                                   main                    5               RUNNABLE        0.0             0.000           0:2.312         false           false
-1      C1 CompilerThread3                              -                       -1              -               0.0             0.000           0:0.703         false           true
59      arthas-NettyWebsocketTtyBootstrap-5-3           main                    5               RUNNABLE        0.0             0.000           0:0.109         false           true
-1      VM Thread                                       -                       -1              -               0.0             0.000           0:0.109         false           true
15      RMI TCP Accept-0                                system                  5               RUNNABLE        0.0             0.000           0:0.062         false           true
27      arthas-TunnelClient-1-1                         main                    5               RUNNABLE        0.0             0.000           0:0.031         false           true
32      arthas-NettyHttpTelnetBootstrap-4-1             main                    5               RUNNABLE        0.0             0.000           0:0.015         false           true
58      arthas-forward-client-connect-local-7-1         main                    5               RUNNABLE        0.0             0.000           0:0.015         false           true
-1      C2 CompilerThread2                              -                       -1              -               0.0             0.000           0:0.015         false           true
-1      C2 CompilerThread0                              -                       -1              -               0.0             0.000           0:0.015         false           true
2       Reference Handler                               system                  10              WAITING         0.0             0.000           0:0.000         false           true
3       Finalizer                                       system                  8               WAITING         0.0             0.000           0:0.000         false           true
4       Signal Dispatcher                               system                  9               RUNNABLE        0.0             0.000           0:0.000         false           true
5       Attach Listener                                 system                  5               RUNNABLE        0.0             0.000           0:0.000         false           true
20      RMI Scheduler(0)                                system                  5               TIMED_WAITING   0.0             0.000           0:0.000         false           true
62      Keep-Alive-Timer                                system                  8               TIMED_WAITING   0.0             0.000           0:0.000         false           true
22      Catalina-utility-1                              main                    1               TIMED_WAITING   0.0             0.000           0:0.000         false           false
23      Catalina-utility-2                              main                    1               WAITING         0.0             0.000           0:0.000         false           false
24      container-0                                     main                    5               TIMED_WAITING   0.0             0.000           0:0.000         false           false
26      arthas-timer                                    main                    5               WAITING         0.0             0.000           0:0.000         false           true
33      arthas-NettyWebsocketTtyBootstrap-5-1           main                    5               RUNNABLE        0.0             0.000           0:0.000         false           true
34      arthas-NettyWebsocketTtyBootstrap-5-2           main                    5               RUNNABLE        0.0             0.000           0:0.000         false           true
35      arthas-shell-server                             main                    5               TIMED_WAITING   0.0             0.000           0:0.000         false           true
36      arthas-session-manager                          main                    5               TIMED_WAITING   0.0             0.000           0:0.000         false           true
37      arthas-UserStat                                 main                    5               WAITING         0.0             0.000           0:0.000         false           true
39      http-nio-8085-exec-1                            main                    5               WAITING         0.0             0.000           0:0.000         false           true
40      http-nio-8085-exec-2                            main                    5               WAITING         0.0             0.000           0:0.000         false           true
41      http-nio-8085-exec-3                            main                    5               WAITING         0.0             0.000           0:0.000         false           true
42      http-nio-8085-exec-4                            main                    5               WAITING         0.0             0.000           0:0.000         false           true
43      http-nio-8085-exec-5                            main                    5               WAITING         0.0             0.000           0:0.000         false           true
44      http-nio-8085-exec-6                            main                    5               WAITING         0.0             0.000           0:0.000         false           true
Memory                                   used          total         max          usage         GC
heap                                     65M           273M          3604M        1.83%         gc.ps_scavenge.count                            5
ps_eden_space                            21M           95M           1316M        1.65%         gc.ps_scavenge.time(ms)                         37
ps_survivor_space                        10M           10M           10M          99.70%        gc.ps_marksweep.count                           2
ps_old_gen                               33M           167M          2703M        1.24%         gc.ps_marksweep.time(ms)                        48
nonheap                                  56M           59M           -1           95.15%
code_cache                               7M            7M            240M         3.16%
metaspace                                43M           45M           -1           94.78%
compressed_class_space                   5M            6M            1024M        0.55%
direct                                   0K            0K            -            100.13%
mapped                                   0K            0K            -            0.00%
Runtime
os.name                                                                                         Windows 10
os.version                                                                                      10.0
java.version                                                                                    1.8.0_301
java.home                                                                                       E:\env\jdk\jre
systemload.average                                                                              -1.00
processors                                                                                      12
timestamp/uptime                                                                                Fri Jul 08 14:53:22 CST 2022/370s
```

* thread

> 查看当前线程信息，查看线程的堆栈

```
参数名称	参数说明
id	线程id
[n:]	指定最忙的前N个线程并打印堆栈
[b]	找出当前阻塞其他线程的线程
[i <value>]	指定cpu使用率统计的采样间隔，单位为毫秒，默认值为200
[--all]	显示所有匹配的线程
```

```
[arthas@12796]$ thread -n 3
"Reference Handler" Id=2 cpuUsage=0.0% deltaTime=0ms time=0ms WAITING on java.lang.ref.Reference$Lock@5517b80f
    at java.lang.Object.wait(Native Method)
    -  waiting on java.lang.ref.Reference$Lock@5517b80f
    at java.lang.Object.wait(Object.java:502)
    at java.lang.ref.Reference.tryHandlePending(Reference.java:191)
    at java.lang.ref.Reference$ReferenceHandler.run(Reference.java:153)


"Finalizer" Id=3 cpuUsage=0.0% deltaTime=0ms time=0ms WAITING on java.lang.ref.ReferenceQueue$Lock@7beda1ad
    at java.lang.Object.wait(Native Method)
    -  waiting on java.lang.ref.ReferenceQueue$Lock@7beda1ad
    at java.lang.ref.ReferenceQueue.remove(ReferenceQueue.java:144)
    at java.lang.ref.ReferenceQueue.remove(ReferenceQueue.java:165)
    at java.lang.ref.Finalizer$FinalizerThread.run(Finalizer.java:216)


"Signal Dispatcher" Id=4 cpuUsage=0.0% deltaTime=0ms time=0ms RUNNABLE
```

* jvm

> 查看当前JVM信息

* sysprop

> 查看当前JVM的系统属性(System Property)

```
sysprop [-h] [property-name] [property-value]

OPTIONS:
 -h, --help                                  this help
 <property-name>                             property name
 <property-value>                            property value
```

查看所有属性：sysprop
查看单个属性：sysprop java.version
修改单个属性：sysprop user.country CN

* sysenv

> 查看当前JVM的环境属性(System Environment Variables)

```
sysenv [-h] [env-name]

 OPTIONS:
 -h, --help                                                 this help
 <env-name>                                                 env name
```

* vmoption

> 查看，更新VM诊断相关的参数

查看所有的option：vmoption
查看指定option：vmoption PrintGC
更新指定option：vmoption PrintGC true

* perfcounter

> 查看当前JVM的 Perf Counter信息


* logger

> 查看logger信息，更新logger level


```
[arthas@12796]$ logger
 name                            ROOT
 class                           ch.qos.logback.classic.Logger
 classLoader                     sun.misc.Launcher$AppClassLoader@18b4aac2
 classLoaderHash                 18b4aac2
 level                           INFO
 effectiveLevel                  INFO
 additivity                      true
 codeSource                      file:/E:/env/maven/repository/ch/qos/logback/logback-classic/1.2.11/logback-classic-1.2.11.jar
 appenders                       name            CONSOLE
                                 class           ch.qos.logback.core.ConsoleAppender
                                 classLoader     sun.misc.Launcher$AppClassLoader@18b4aac2
                                 classLoaderHash 18b4aac2
                                 target          System.out
```

* mbean

> 这个命令可以便捷的查看或监控 Mbean 的属性信息。

```
参数名称	参数说明
name-pattern	名称表达式匹配
attribute-pattern	属性名表达式匹配
[m]	查看元信息
[i:]	刷新属性值的时间间隔 (ms)
[n:]	刷新属性值的次数
[E]	开启正则表达式匹配，默认为通配符匹配。仅对属性名有效
```

* getstatic

> 通过getstatic命令可以方便的查看类的静态属性

* ognl

> 执行ognl表达式

```
参数名称	参数说明
express	执行的表达式
[c:]	执行表达式的 ClassLoader 的 hashcode，默认值是SystemClassLoader
[classLoaderClass:]	指定执行表达式的 ClassLoader 的 class name
[x]	结果对象的展开层次，默认值1
```

### class/classloader相关

* sc

> 查看JVM已加载的类信息


```
参数名称	参数说明
class-pattern	类名表达式匹配
method-pattern	方法名表达式匹配
[d]	输出当前类的详细信息，包括这个类所加载的原始文件来源、类的声明、加载的ClassLoader等详细信息。
如果一个类被多个ClassLoader所加载，则会出现多次
[E]	开启正则表达式匹配，默认为通配符匹配
[f]	输出当前类的成员变量信息（需要配合参数-d一起使用）
[x:]	指定输出静态变量时属性的遍历深度，默认为 0，即直接使用 toString 输出
[c:]	指定class的 ClassLoader 的 hashcode
[classLoaderClass:]	指定执行表达式的 ClassLoader 的 class name
[n:]	具有详细信息的匹配类的最大数量（默认为100）
```

* sm

> 查看已加载类的方法信息

```
参数名称	参数说明
class-pattern	类名表达式匹配
method-pattern	方法名表达式匹配
[d]	展示每个方法的详细信息
[E]	开启正则表达式匹配，默认为通配符匹配
[c:]	指定class的 ClassLoader 的 hashcode
[classLoaderClass:]	指定执行表达式的 ClassLoader 的 class name
[n:]	具有详细信息的匹配类的最大数量（默认为100）
```

```shell
[arthas@12796]$ sm -d java.lang.String toString
 declaring-class  java.lang.String
 method-name      toString
 modifier         public
 annotation
 parameters
 return           java.lang.String
 exceptions
 classLoaderHash  null

Affect(row-cnt:1) cost in 6 ms.
```

* dump

> dump 已加载类的 bytecode 到特定目录

```
参数名称	参数说明
class-pattern	类名表达式匹配
[c:]	类所属 ClassLoader 的 hashcode
[classLoaderClass:]	指定执行表达式的 ClassLoader 的 class name
[d:]	设置类文件的目标目录
[E]	开启正则表达式匹配，默认为通配符匹配
```

```shell
[arthas@12796]$ dump com.wudiguang.arthas.*
 HASHCODE  CLASSLOADER                                    LOCATION
 18b4aac2  +-sun.misc.Launcher$AppClassLoader@18b4aac2    C:\Users\admin\logs\arthas\classdump\sun.misc.Launcher$AppClassLoader-18b4aac2\com\wudiguang\arthas\ArthasApplication$$EnhancerBySpri
             +-sun.misc.Launcher$ExtClassLoader@55040f2f  ngCGLIB$$1070f778.class
 18b4aac2  +-sun.misc.Launcher$AppClassLoader@18b4aac2    C:\Users\admin\logs\arthas\classdump\sun.misc.Launcher$AppClassLoader-18b4aac2\com\wudiguang\arthas\ArthasApplication.class
             +-sun.misc.Launcher$ExtClassLoader@55040f2f
Affect(row-cnt:2) cost in 213 ms.
```

* heapdump

> dump java heap, 类似jmap命令的heap dump功能。

dump到指定文件：heapdump /tmp/dump.hprof
只dump live对象：heapdump --live /tmp/dump.hprof
dump到临时文件：heapdump

* vmtool

> vmtool 利用JVMTI接口，实现查询内存对象，强制GC等功能。

强制GC：vmtool --action forceGc

* jad

> 反编译指定已加载类的源码

```
参数名称	参数说明
class-pattern	类名表达式匹配
[c:]	类所属 ClassLoader 的 hashcode
[classLoaderClass:]	指定执行表达式的 ClassLoader 的 class name
[E]	开启正则表达式匹配，默认为通配符匹配
```

只打印源码

```java
[arthas@12796]$ jad --source-only com.wudiguang.arthas.ArthasApplication
/*
* Decompiled with CFR.
*/
package com.wudiguang.arthas;

import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;

@SpringBootApplication
public class ArthasApplication {
    public static void main(String[] args) {
/*10*/     SpringApplication.run(ArthasApplication.class, args);
    }
}
```

反编译指定的函数：jad --source-only com.wudiguang.arthas.ArthasApplication main
不显示行号：jad --source-only com.wudiguang.arthas.ArthasApplication main --lineNumber false

* classloader

> 查看classloader的继承树，urls，类加载信息

```
参数名称	参数说明
[l]	按类加载实例进行统计
[t]	打印所有ClassLoader的继承树
[a]	列出所有ClassLoader加载的类，请谨慎使用
[c:]	ClassLoader的hashcode
[classLoaderClass:]	指定执行表达式的 ClassLoader 的 class name
[c: r:]	用ClassLoader去查找resource
[c: load:]	用ClassLoader去加载指定的类
```

查看ClassLoader的继承树：classloader -t

* mc

> Memory Compiler/内存编译器，编译.java文件生成.class。

* retransform

> 加载外部的.class文件，retransform jvm已加载的类。

### monitor/watch/trace相关

* monitor

> 方法执行监控

参数名称	参数说明
class-pattern	类名表达式匹配
method-pattern	方法名表达式匹配
condition-express	条件表达式
[E]	开启正则表达式匹配，默认为通配符匹配
[c:]	统计周期，默认值为120秒
[b]	在方法调用之前计算condition-express

监控的维度说明
```
监控项	说明
timestamp	时间戳
class	Java类
method	方法（构造方法、普通方法）
total	调用次数
success	成功次数
fail	失败次数
rt	平均RT
fail-rate	失败率
```

* watch

> 函数执行数据观测

```
class-pattern	类名表达式匹配
method-pattern	函数名表达式匹配
express	观察表达式，默认值：{params, target, returnObj}
condition-express	条件表达式
[b]	在函数调用之前观察
[e]	在函数异常之后观察
[s]	在函数返回之后观察
[f]	在函数结束之后(正常返回和异常返回)观察
[E]	开启正则表达式匹配，默认为通配符匹配
[x:]	指定输出结果的属性遍历深度，默认为 1，最大值是4
```

观察函数调用返回时的参数、this对象和返回值：watch demo.MathGame primeFactors -x 2
观察函数调用入口的参数和返回值：watch demo.MathGame primeFactors "{params,returnObj}" -x 2 -b
同时观察函数调用前和函数返回后：watch demo.MathGame primeFactors "{params,target,returnObj}" -x 2 -b -s -n 2
调整-x的值，观察具体的函数参数值：watch demo.MathGame primeFactors "{params,target}" -x 3
条件表达式的例子：watch demo.MathGame primeFactors "{params[0],target}" "params[0]<0"
观察异常信息的例子：watch demo.MathGame primeFactors "{params[0],throwExp}" -e -x 2
按照耗时进行过滤：watch demo.MathGame primeFactors '{params, returnObj}' '#cost>200' -x 2
观察当前对象中的属性：watch demo.MathGame primeFactors 'target'


* trace

> 方法内部调用路径，并输出方法路径上的每个节点上耗时

```
参数名称	参数说明
class-pattern	类名表达式匹配
method-pattern	方法名表达式匹配
condition-express	条件表达式
[E]	开启正则表达式匹配，默认为通配符匹配
[n:]	命令执行次数
#cost	方法执行耗时
```

`trace 能方便的帮助你定位和发现因 RT 高而导致的性能问题缺陷，但其每次只能跟踪一级方法的调用链路。`

trace函数：trace demo.MathGame run
trace次数限制：trace demo.MathGame run -n 1


* stack

> 输出当前方法被调用的调用路径

```
参数名称	参数说明
class-pattern	类名表达式匹配
method-pattern	方法名表达式匹配
condition-express	条件表达式
[E]	开启正则表达式匹配，默认为通配符匹配
[n:]	执行次数限制
```

stack：stack demo.MathGame primeFactors
据条件表达式来过滤：stack demo.MathGame primeFactors 'params[0]<0' -n 2
据执行时间来过滤：stack demo.MathGame primeFactors '#cost>5'

* tt

略

* cat

> cat /tmp/a.txt

### profiler/火焰图

* profiler

> 使用async-profiler生成火焰图

```
profiler action [actionArg]

参数名称	参数说明
action	要执行的操作
actionArg	属性名模式
[i:]	采样间隔（单位：ns）（默认值：10'000'000，即10 ms）
[f:]	将输出转储到指定路径
[d:]	运行评测指定秒
[e:]	要跟踪哪个事件（cpu, alloc, lock, cache-misses等），默认是cpu
```

### 鉴权

### options

### 管道

### 后台异步任务

