# 零拷贝

```desc
date: 2022-07-04
address: NC
author: 吴第广
```

## 案例

```java
File f = new File("/data.txt");
RandomAccessFile file = new RandomAccessFile(f, "r");

byte[] buf = new byte[(int)f.length()];
file.read(buf);

Socket socket = ...;
socket.getOutputStream().write(buf);
```

流程：
1. 将文件内容读取到byte数组中
2. 通过socket，将byte数组发送给客户端

结果：三次内核态-用户态系统切换，四次数据拷贝

![20220704181411](https://cdn.jsdelivr.net/gh/wudg/picgo@master/images/books/20220704181411.png)

由于Java无法直接读取系统文件，实际的做法是先将文件读取到内核缓存区，再将内核缓存区的数据读取到用户缓冲区；同样，Java也无法直接写数据到系统文件，需要先将数据存储到socket缓冲区，再将数据拷贝到网卡进行数据的发送。

## NIO

将上面的代码逻辑调整为：使用FileChannel读 -> 写入到ByteBuffer -> 从ByteBuffer读 -> 写入到SocketChannel

ByteBuffer：ByteBuffer.allocateDirect(16)

这里ByteBuffer申请操作的是操作系统的内存，这块内存操作系统可以直接操作，Java语言也可以直接操作，相当于文件数据从磁盘拷贝到这块内存

![20220704182917](https://cdn.jsdelivr.net/gh/wudg/picgo@master/images/books/20220704182917.png)

## Linux2.1 -> sendFile()

在Java中FileChannel中有transferTo/transferFrom方法拷贝数据

![20220705090525](https://cdn.jsdelivr.net/gh/wudg/picgo@master/images/books/20220705090525.png)

1. Java调用transferTo -> 用户态到内核态 -> 使用DMA将数据读入内核模块缓冲区（不使用CPU）
2. 数据从内核缓冲区传输到socket缓冲区，CPU会参与拷贝
3. 使用DMA将socket缓冲区的数据写入网卡（不使用CPU）

## Linux2.4 

![20220705090800](https://cdn.jsdelivr.net/gh/wudg/picgo@master/images/books/20220705090800.png)

1. Java调用transferTo -> 用户态到内核态 -> 使用DMA将数据读入内核模块缓冲区（不使用CPU）
2. 将一些offset和length信息拷贝到socket缓冲区
3. 使用DMA将socket缓冲区的数据写入网卡（不使用CPU）

结果：Java到操作系统的切换只有一次，数据拷贝发生两次

最后两种方式都可以叫做零拷贝。零拷贝是指：不用在Java内存之间进行拷贝操作

零拷贝优点：

1. 用户态和内核态的切换少
2. 不利用CPU计算，减少CPU缓存伪共享
3. 零拷贝适合小文件传输



