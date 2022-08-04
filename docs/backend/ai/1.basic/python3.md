# python3

```desc
date: 2022-08-04
address: NC
author: 吴第广
```

参考资料：[廖雪峰python教程](https://www.liaoxuefeng.com/wiki/1016959663602400)

吴第广-【人工智能学习路线】-【人工智能开发入门】-【Python编程】

简介：编程语言。提供了非常完善的基础代码库，覆盖了网络、文件、GUI、数据库、文本等大量内容。除了内置的库外，还有大量的第三方库

许多大型网站都有python的身影，如YouTuBe，Instagram，还有国内的豆瓣。还有大公司在使用，其中包括Yahoo、NASA等

定位：优雅、明确、简单

适合哪些类型应用开发：网络应用：网站、后台服务，日常小工具：各类脚本

缺点：运行速度慢、不能加密

## 安装python

略

### 解释器
> 用于执行 .py 代码的环境

* CPython：官网内置解释器，C语言开发，最广
* IPython：基于CPython之上的一个交互式解释器，增强了交互方式
* PyPy：采用JIT技术，目标是执行速度。虽然绝大部分Python代码都可以再PyPy下运行，但是仍存在一定的不同，可能导致运行结果和CPython不太一样
* JPython：运行再Java平台的Python解释器，直接把Python代码编译成Java字节码
* IronPython：和JPython类似，只不过是运行再微软.Net平台上的解释器，把Python代码编译成.Net字节码

<!--more-->

### 小结

Python解释器很多，但使用最广泛的还是CPython

## 第一个python程序

命令行模式和Python交互模式

命令模式：可以执行输入`python`纪进入交互式环境，也可以执行`python hello.py`运行一个`.py`文件

```python
>>> print("hello, world")
hello, world
>>> print('hello, world')
hello, world
```

```python
# 文件名：calc.py
# 文件目录下cmd执行：python calc.py
# 打印：600
print(100 + 200 + 300)
```

### 使用文本编辑器-Visual Studio Code

下载并使用，替换自带的文本编辑器

### 直接运行 .py 文件

windows 上是不行的，但是，在Mac和Linux上是可以的，方法就是在 .py 文件第一行加上一个特殊的注释：

```python
#!/usr/bin/env python3

print('hello, world')
```

然后给 .py 文件执行权限，就可以直接运行 `hello.py` 了
```shell
chmod a+x hello.py
```

### 在线python代码运行

[进入](https://c.runoob.com/compile/9/)


### 输出和输入

输出：

```python
print('The quick brown fox', 'jumps over', 'the lazy dog')
```

输入：
```python
>>> name = input('please enter your name: ')
please enter your name: wudiguang
>>> print('my name is:', name)
my name is: wudiguang
```


## python基础

任何一种编程语言都有自己的一套语法，编译器或者解释器负责把服务语法的程序代码转换成CPU能够执行的机器码再执行

Python语法比较简单，采用缩进方式，如下格式：

```python
# print absolute value of an integer:
a = int(input('input a number：'))
if a >= 0:
    print(a)
else:
    print(-a)
```

缩进有利有弊

利：强迫我们写出格式化的代码，强迫我们写出缩进较少的代码
弊：复制-粘贴失效

小结： Python采用缩进来组织代码块，请务必遵守约定俗成的习惯，坚持使用4个空格的缩进。另外，Python程序是大小写敏感的


### 数据类型和变量

* 整数
* 浮点数
* 字符串：转义符：`\`，字符串不转移：`r''`，多行保持格式：`'''...'''`
* 布尔值：True、False，支持`and`、`or`和`not`逻辑运算
* 空值：None
* 常量：通常用全部大写的变量名表示，如 `PI`

在Python中，可以把任意数据类型数据赋值给同一个变量，这种变量类型不固定的语言称为`动态语言`，与之对应的是`静态语言`。静态语言在定义时必须指定变量类型，如Java

注意：Python的整数没有大小限制，浮点也没有大小限制，但是超出一定范围就直接表示为`INF`

### 字符串和编码

字符编码：由于计算机只能识别`0` `1`组成的数据，需要将文本数据转成计算机能识别的数据格式才行，索引产生了字符编码来转换文本到计算机能直接的格式

多语言混合的文本会乱码，因此产生了Unicode字符集，把所有语言都统一到一套编码里，这样就不会有乱码问题了

ASCII：1个字节，最早只有127个字符被编码到计算机，即大小写英文字母、数字和一些符号
GB2312：中文编码
Shift_JIS：日文编码
Euc-kr：韩文编码
Unicode：通常使用两个字节来表示字符
UTF-8：可变长的编码

统一Unicode后又产生了新问题，用Unicode编码比ASCII编码多一倍的存储空间，在存储和传输上就十分不划算，于是产生了可变长的`UTF-8`编码，`UTF-8`编码把Unicode字符根据不同的数字大小编码成1-6个字节，通常使用的英文字母被编码成1个字节，汉字通常是3个字节，只有很生僻的字符才会被编码成4-6个字节

* 使用记事本编辑时：从文件读取`UTF-8`字符被转换为`Unicode`字符到内存里，编辑完成后，保存的时候再把`Unicode`转换为`UTF-8`保存到文件
* 流量网页时：服务器会把动态生成的`Unicode`内容转换为`UTF-8`再传输到浏览器


在最新的Python 3版本中，字符串是以`Unicode`编码，即Python字符串支持多语言
```shell
>>> print('包含中文的str')
包含中文的str
```

Python提供`ord()`函数获取字符的整数表示

```shell
>>> ord('A')
65
>>> ord('中')
20013
>>> chr(66)
'B'
>>> chr(25991)
'文'
```

Python对`bytes`类型的数据用带`b`前缀的单引号或双引号表示：
```python
x = b'ABC'
```

`Unicode`表示的字符串通过`encode()`方法可以编码为指定的`bytes`，如：

```shell
>>> 'ABC'.encode('ascii')
b'ABC'
>>> '中文'.encode('utf-8')
b'\xe4\xb8\xad\xe6\x96\x87'
```

反过来，如果我们从网络或磁盘上读取字节流，那么读到的数据就是`bytes`。要把`bytes`变为字符串，就需要用`decode()`方法

```shell
>>> b'ABC'.decode('ascii')
'ABC'
>>> b'\xe4\xb8\xad\xe6\x96\x87'.decode('utf-8')
'中文'
```

`len('ABC')` 计算字符串包含多少个字符

当我们源代码中包含中文时，需要指定保存为UTF-8编码。当Python解释器读取源代码时，为了让它按UTF-8编码读取，通常在文件开头添加下面两行注释：

```python
#!/usr/bin/env python3
# -*- coding: utf-8 -*-
```

* 第一行：告诉Linux/MAC系统这是一个Python可执行程序
* 第二行：告诉Python解释器，按照UTF-8编码读取源代码，否则源代码中中文输出可能乱码

`格式化输出`

1. 使用`%`实现：

```shell
>>> 'Hello, %s' % 'world'
'Hello, world'
>>> 'Hi, %s, you have $%d.' % ('Michael', 1000000)
'Hi, Michael, you have $1000000.'
```

占位符：

%d：整数
%f：浮点数
%s：字符串（如果不确定数据类型，%s永远起作用，把任何数据类型转换为字符串）
%x：十六进制整数

2. format()

使用字符串的`format()`方法，使用{0}、{1}...占位符

```shell
>>> 'Hello, {0}, 成绩提升了 {1:.1f}%'.format('小明', 17.125)
'Hello, 小明, 成绩提升了 17.1%'
```

3. f-string

以`f`开头的字符串，自动替换变量

```shell
>>> r = 2.5
>>> s = 3.14 * r ** 2
>>> print(f'The area of a circle with radius {r} is {s:.2f}')
The area of a circle with radius 2.5 is 19.62
```

### list和tuple

list：不定长有序集合，里面元素数据类型可以不同，也可以嵌套list

```shell
>>> classmates = ['Michael', 'Bob', 'Tracy']
>>> classmates
['Michael', 'Bob', 'Tracy']
>>> L = []
>>> len(L)
0
```

* len(list)
* append('xxx')
* insert(index, 'xxx')
* pop()：删除末尾元素
* pop(index)：删除指定位置元素

tuple：有序list，一旦初始化就不能修改，不可变，所以代码更安全

```shell
>>> t = (1, 2)
>>> t
(1, 2)
>>> t = ()
>>> t
()
>>> t = (1,)
>>> t
(1,)
```

tuple不可变是指每个元素的指向对象不变，但是指向的对象内容可以变，即指针地址不变，地址中的内容可变
```shell
>>> t = ('a', 'b', ['A', 'B'])
>>> t[2][0] = 'X'
>>> t[2][1] = 'Y'
>>> t
('a', 'b', ['X', 'Y'])
```

### 条件判断

例子：

```python
age = 3
if age >= 18:
    print('adult')
# `elif`是`else if`的简写
elif age >= 6:
    print('teenager')
else:
    print('kid')
```

`if`判断条件还可以简写，如：
```python
if x:
    print('True')
```

只要 `x` 是非零数值、非空字符串、非空list等，就判断为`True`，否则为`False`


### 循环

1. `for...in`

```python
names = ['Michael', 'Bob', 'Tracy']
for name in names:
    print(name)
```

`range()`函数生成一个整数序列，再通过`list()`函数可以转换为list

```shell
>>> list(range(5))
[0, 1, 2, 3, 4]
```

2. while

```python
sum = 0
n = 99
while n > 0:
    sum = sum + n
    n = n - 2
print(sum)
```

`break`:提前结束循环
`continue`：跳过当前这次循环，直接开始下一次循环

尽量避免使用`break`和`continue`，滥用会造成代码执行逻辑分叉过多，容易出错


### dict和set

1. dict：键值对，同`Java`中的`HashMap`，无序,key是不可变对象

```shell
>>> d = {'Michael': 95, 'Bob': 75, 'Tracy': 85}
>>> d['Michael']
95
```

使用`dict['key']`时，如果`key`值不存在，则会报错，避免报错的方法有两种:
* 通过`in`判断`key`是否存在
```shell
>>> 'Thomas' in d
False
```
* 通过`get()`方法获取，如果`key`不存在，则返回`None`或者自己指定的`value`
```shell
>>> d.get('Thomas')
>>> d.get('Thomas', -1)
-1
```

`pop('key')`：删除一个元素

2. set：无序，不重复，类似list，可以做数学意义上的交、并等操作

```shell
>>> s = set([1, 2, 3])
>>> s
{1, 2, 3}
```

* add(key)：添加元素
* remove(key)：删除元素

学习记录：2022-08-04 18:28:00

## 函数

## 高级特性

## 函数式编程

## 模块

## 面向对象编程

## 面向对象高级编程

## 错误、调试和测试

## IO编程

## 进程和线程

## 正则表达式

## 常用内建模块

## venv

## 图形界面

## 网络编程

## 电子邮件

## 访问数据库

## web开发

## 异步IO

## 使用MicroPython

## 实战

## 总结