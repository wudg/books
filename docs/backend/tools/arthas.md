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

https://arthas.aliyun.com/doc/advanced-use.html#id2

### class/classloader相关

### monitor/watch/trace相关

### profiler/火焰图

### 鉴权

### options

### 管道

### 后台异步任务

