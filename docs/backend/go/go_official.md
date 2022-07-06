# 《go官方文档》

> Go 编程语言是一个开源项目，旨在提高程序员的工作效率

> Go表达能力强、简洁、高效。它的并发机制使得编写多线程程序变得容易，从而最大限度地利用多核和网络机器，而其新型系统支持灵活和模块化的程序构造。Go可以快速编译为机器代码，同时还具有垃圾收集的便利性和运行时反射的能力。它是一种快速、静态类型的编译语言，感觉就像一种动态类型的解释语言。

[文档地址](https://go.dev/doc/)

## 安装

### 下载

[下载地址](https://go.dev/dl/)

### 安装

1. 打开下载的 MSI 文件，根据提示进行安装
2. 验证go语言环境是否安装成功：命令行输入 `go version`，确认命令行中是否打印go版本信息

## 快速上手

### 编写代码

1. 创建文件夹 `hello`
2. 从命令行进入上面新建的文件夹中，并运行 `go mod init example/hello`

```shell
go mod init example/hello
go: creating new go.mod: module example/hello
```

3. 进入文本编辑器中，新建`hello.go`文件并编写代码
```go
// 声明一个 main 包（包是对函数进行分组的一种方式，它由同一个目录中的所有文件组成）
package main
// 导入 fmt 包，该包包含格式化文本的功能，包括打印到控制台。此软件包是安装Go时获得的标准库软件之一。
import "fmt"
// 实现将消息打印到控制台的功能。默认情况下，在运行 main 包时会执行 main 函数
func main() {
	fmt.Println("Hello, World");
}
```

4. 运行代码

```shell
go run .
Hello, World
```

### 调用一个扩展包

当你需要你的代码去做一些其他人可能已经实现的功能时，你可以寻找一个包，其中包含你可以在代码中使用的函数。

1. 使用外部模块的功能，使打印的消息更有趣
    a. 访问 `https://pkg.go.dev/` 并搜索 `quote` 包
    b. 在搜索结果中定位后点击 `rsc.io/quote` 包
    c. 在文档中`Index`可以看到可供使用的函数说明
2. 在我们的代码中导入`rsc.io/quote`包并且增加一个Go方法调用

```go
package main

import (
	"fmt"

	"rsc.io/quote"
)

func main() {
	fmt.Println("Hello, World")
	fmt.Println(quote.Go())
}
```

3. 添加新的模块要求和sums：Go将添加 `quote` 模块作为必须依赖，以及Go用于验证模块的sum文件。

```go
go mod tidy
go: finding module for package rsc.io/quote
go: found rsc.io/quote in rsc.io/quote v1.5.2
```

4. 运行代码并查看调用函数返回的消息

```shell
go run .
Hello, World
Don't communicate by sharing memory, share memory by communicating.
```

`注意：`当你运行`go mod tidy` 时，它找到并下载了你导入的`rsc.io/quote`模块。默认情况下，它下载了最新版本`v1.5.2`

## 创建一个模块

### 先决条件

* 一些编程经验：目前的样例代码非常简单，但它有助于了解函数、循环和数组
* 编译代码的工具：VSCode
* 命令终端：cmd

### 开始编写其他人可以使用的模块

从创建Go模块开始。在一个模块中，收集一组离散且有用的函数到一个或多个包。例如，你可以创建一个模块，其中的包具有进行财务分析的功能，以便其他编写财务应用程序的人可以使用我们编写的代码。

Go代码被分组到包中，包被分组到模块中。你的模块指定运行代码所需的依赖项，包括Go语言版本及其所需要的其他模块集

当你在模块中添加或改进功能时，你将发布模块的新版本。编写调用模块中函数的代码的开发人员可以导入模块的更新包，并在将其投入生产使用之前使用新版本进行测试。

1. 创建 `greetings` 文件夹
2. 使用 Go 初始化命令 `go mod init example.com/greetings`

```shell
go mod init example.com/greetings
go: creating new go.mod: module example.com/greetings
```

3. 在文本编辑器中编写程序代码 `greetings.go`

```go
package main

import "fmt"

// Hello 为指定的人返回问候语
func Hello(name string) string {
	// 返回在消息中嵌入姓名的问候语
	message := fmt.Sprintf("Hi,%v. Welcome!", name)
	return message
}
```
    a. 声明一个 `greetings` 包来收集相关的函数
    b. 实现一个 `Hello` 函数来返回问候语
    c. 此函数接受类型为 `string` 的 `name` 参数并返回一个字符串。在Go中，名称以大写字母开头的函数可以被不在同一个包中的函数调用，这在Go中被称为导出名称。
    d. 声明一个 `message` 变量来接收问候语。在Go中， `:=` 运算符是在一行中声明和初始化变量的快捷方式，和下面方式作用相同

    ```go
    var message string
    message = fmt.Sprintf("Hi,%v. Welcome!", name)
    ```

    e. 使用 `fmt` 包中的 `Sprintf` 函数组装问候语。`%v` 是字符串占位符
    f. 将格式化的问候语文本返回给调用者

## 多模块工作区入门

### 先决条件

* 安装 Go 1.18 或更新的版本
* 编写代码的开发工具
* 命令终端

### 创建一个模块

1. 创建文件夹 `workspace`
2. 初始化模块

```shell
$ mkdir hello
$ cd hello
$ go mod init example.com/hello
go: creating new go.mod: module example.com/hello
```
通过`go get`添加一个依赖 `golang.org/x/example`

```shell
go get golang.org/x/example
```

```go
package main

import (
	"fmt"

	"golang.org/x/example/stringutil"
)

func main() {
	fmt.Println(stringutil.Reverse("Hello"))
}
```

运行 `hello` 程序：

```shell
go run example.com/hello       
olleH
```

### 创建工作区

1. 初始化工作区：在workspace目录运行 `go work init ./hello`，在workspace目录下生成包含看hello模块的`go.work`文件
2. 在workspace目录下运行程序

```shell
go run example.com/hello
olleH
```

Go 命令将工作区中所有模块作为主模块包括在内，这允许我们在模块中引用包，甚至是模块外的包。在模块或者工作区之外运行`go run` 命令将导致报错，因为Go命令不知道使用哪些模块

### 下载并且修改模块

1. 克隆仓库：`git clone https://go.googlesource.com/example`
2. 添加模块到工作区：`go work use ./example`，这个命令将添加一个新的模块到`go.work`文件中
现在包含`example.com/hello`模块和`golang.org/x/example` 模块
这将允许我们使用我们在stringutil模块副本中编写的新代码，而不是使用 `go get` 命令下载的模块缓存中的模块版本
3. 添加新的函数：我们向`golang.org/x/example/stringutil`包中添加一个“将参数字符串转换成对应的大写，并返回”

```go


```

4. 修改 `hello` 程序来使用新的函数

```go
package main

import (
	"fmt"

	"golang.org/x/example/stringutil"
)

func main() {
	fmt.Println(stringutil.ToUpper("Hello"))
}
```

运行工作区代码

```shell
go run example.com/hello
HELLO
```

## 使用Go和Gin开发一个RESTFUL API

### 先决条件

* 本地安装Go 1.16版本或更新版本
* 开发工具
* 命令行终端
* curl 工具

### 设计API端点

你将构建一个API，该API提供对销售乙烯基记录商店的访问，因此，客户端可以通过这些端点为用户获取和添加相册

下面是将要创建的端点

/albums
    * GET-查询相册集合，返回json
	* POST-接收一个json数据来增加一个相册

/albums/:id
    * GET-根据ID查询相册，返回json格式的相册数据

### 创建代码目录

```shell
mkdir web-service-gin
cd web-service-gin
go mod init example/web-service-gin
go: creating new go.mod: module example/web-service-gin
```

### 编写代码

```go
// main.go
package main

import (
    "net/http"

    "github.com/gin-gonic/gin"
)

// album represents data about a record album.
type album struct {
    ID     string  `json:"id"`
    Title  string  `json:"title"`
    Artist string  `json:"artist"`
    Price  float64 `json:"price"`
}

// albums slice to seed record album data.
var albums = []album{
    {ID: "1", Title: "Blue Train", Artist: "John Coltrane", Price: 56.99},
    {ID: "2", Title: "Jeru", Artist: "Gerry Mulligan", Price: 17.99},
    {ID: "3", Title: "Sarah Vaughan and Clifford Brown", Artist: "Sarah Vaughan", Price: 39.99},
}

func main() {
    router := gin.Default()
    router.GET("/albums", getAlbums)
    router.GET("/albums/:id", getAlbumByID)
    router.POST("/albums", postAlbums)

    router.Run("localhost:8080")
}

// getAlbums responds with the list of all albums as JSON.
func getAlbums(c *gin.Context) {
    c.IndentedJSON(http.StatusOK, albums)
}

// postAlbums adds an album from JSON received in the request body.
func postAlbums(c *gin.Context) {
    var newAlbum album

    // Call BindJSON to bind the received JSON to
    // newAlbum.
    if err := c.BindJSON(&newAlbum); err != nil {
        return
    }

    // Add the new album to the slice.
    albums = append(albums, newAlbum)
    c.IndentedJSON(http.StatusCreated, newAlbum)
}

// getAlbumByID locates the album whose ID value matches the id
// parameter sent by the client, then returns that album as a response.
func getAlbumByID(c *gin.Context) {
    id := c.Param("id")

    // Loop through the list of albums, looking for
    // an album whose ID value matches the parameter.
    for _, a := range albums {
        if a.ID == id {
            c.IndentedJSON(http.StatusOK, a)
            return
        }
    }
    c.IndentedJSON(http.StatusNotFound, gin.H{"message": "album not found"})
}
```