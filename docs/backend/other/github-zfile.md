# 【Github开源项目体验】- ZFile 基于 Java 的在线网盘

> 在线云盘、网盘、OneDrive、云存储、私有云、对象存储、h5ai、上传、下载

```desc
date: 2022-08-02
address: NC
author: 吴第广
```

本文收录于：[个人博客](https://wudg.github.io/books/)

## 前言

自己动手搭建一个只属于自己的在线网盘😃😃😃

* 再也不用被网络限速（如某度云盘）
* 多端共享资源和在线浏览，图片、文件、视频一网打尽
* 代码开源，不用注册账号，我们就是管理员✨

### 地址

官方地址：[https://www.zfile.vip/](https://www.zfile.vip/)
Github地址：[https://github.com/zfile-dev/zfile](https://github.com/zfile-dev/zfile)

### 作用

云盘可以让您的照片，文档、音乐、视频、软件、应用等各种内容，随时随地触手可及，永不丢失。

多端共享资源和在线浏览，图片、文件、视频一网打尽

## 安装配置

### 安装

这里是基于 Docker 安装，也可以选择其他安装方式，文档地址：[https://docs.zfile.vip/#/install](https://docs.zfile.vip/#/install)

```shell
docker run -d --name=zfile --restart=always \
    -p 8080:8080 \
	-v /root/wudiguang/private/zfile/zfile-files:/root/zfile-files \
    -v /root/wudiguang/private/zfile/db:/root/.zfile-v4/db \
    -v /root/wudiguang/private/zfile/logs:/root/.zfile-v4/logs \
    -v /root/wudiguang/private/zfile/application.properties:/root/application.properties \
    zhaojun1998/zfile
```

这里服务是通过 `8080` 端口透出，zfile 主目录指定 `/root/wudiguang/private/zfile`
`-v /root/wudiguang/private/zfile/file:/data/file` 用于映射本地存储

#### 配置管理员用户名密码

访问：[http://IP:8080](http://IP:8080) 进行配置

IP 换成云服务器地址即可

### 基本配置

访问：[http://IP:8080](http://IP:8080)  进行基本信息设置，如下图：

![20220802165426](https://cdn.jsdelivr.net/gh/wudg/picgo@master/images/books/20220802165426.png)

✨后端站点域名：可以设置成云服务器的域名地址或者二级子域名（需要在nginx配置代理）,如 `http://zfile.wudg.work/`
👀前端站点域名：这里设置为 `后端站点域名/web`，即 `http://zfile.wudg.work/web`

### 存储源配置

配置存储位置信息，这里可以选择不同的`存储策略`，如阿里云OSS、七牛云和OneDriver等等，为了方便，我们这里选择`本地存储`，即存储在部署服务的机器磁盘上。

![20220802170016](https://cdn.jsdelivr.net/gh/wudg/picgo@master/images/books/20220802170016.png)

## 使用

访问：[http://IP:8080](http://IP:8080) 

![20220802171036](https://cdn.jsdelivr.net/gh/wudg/picgo@master/images/books/20220802171036.png)


### 创建文件夹/上传文件
![20220802170411](https://cdn.jsdelivr.net/gh/wudg/picgo@master/images/books/20220802170411.png)

### 画廊模式展示图片

![20220802170648](https://cdn.jsdelivr.net/gh/wudg/picgo@master/images/books/20220802170648.png)

### 在线播放音频

![20220802171800](https://cdn.jsdelivr.net/gh/wudg/picgo@master/images/books/20220802171800.png)

