# spring-boot整合arthas

## 使用步骤

* 本地运行`arthas`服务端
* 修改`spring-boot`项目
* `arthas`连接`spring-boot`项目

## 本地运行

### 下载arthas服务端jar

**下载地址:**[进入](https://github.com/alibaba/arthas/releases)

![20220708112543](https://cdn.jsdelivr.net/gh/wudg/picgo@master/images/books/20220708112543.png)

> 最新版本为：`3.6.3`


### 本地运行arthas服务端

```shell
java -jar arthas-tunnel-server-3.6.2-fatjar.jar
```

![20220708112458](https://cdn.jsdelivr.net/gh/wudg/picgo@master/images/books/20220708112458.png)

看到上面打印日志则表示启动成功。同时记录下如截图中的内容（登录密码）


### 浏览器访问arthas web端


**arthas服务信息地址：** http://localhost:8080/actuator/arthas

![20220708113151](https://cdn.jsdelivr.net/gh/wudg/picgo@master/images/books/20220708113151.png)

`登录账号：`
- username：arthas
- password：上述日志中打印的密码

**arthas交互平台地址：** http://localhost:8080/



## 修改spring-boot项目

### 引入arthas maven依赖

其中`${arthas.version}`需使用最新版依赖版本号替换

```xml
<dependency>
    <groupId>com.taobao.arthas</groupId>
    <artifactId>arthas-spring-boot-starter</artifactId>
    <version>${arthas.version}</version>
</dependency>
```

### application.properties中加入以下配置

下面配置中`arthas.agent-id`对应的值后面连接`arthas-web`客户端需要

```properties
arthas.agent-id=springboot-arthas
arthas.tunnel-server=ws://127.0.0.1:7777/ws
```

## arthas连接spring-boot项目

### 访问arthas-web客户端

查看服务`agent`连接`arthas`是否成功

地址：http://localhost:8080/actuator/arthas

```json
{
	"clientConnections": {
		"CE76AUYU58ORZQYAQ12X": {
			"host": "0:0:0:0:0:0:0:1",
			"port": 9157
		}
	},
	"version": "3.6.3",
	"properties": {
		"server": {
			"host": "0.0.0.0",
			"port": 7777,
			"ssl": false,
			"path": "/ws",
			"clientConnectHost": "192.168.9.119"
		},
		"embeddedRedis": null,
		"enableDetailPages": true,
		"enableIframeSupport": true
	},
	"agents": {
		"springboot-arthas": {
			"host": "127.0.0.1",
			"port": 8993,
			"arthasVersion": "3.6.3"
		}
	}
}
```

页面`json`数据中`agents`对应的`value`就是当前连接到`arthas`的`java`服务

地址：http://localhost:8080

![20220708114035](https://cdn.jsdelivr.net/gh/wudg/picgo@master/images/books/20220708114035.png)

### 使用arthas

接下来就可以愉快地使用`arthas`啦~