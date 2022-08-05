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

函数是最基本的一种代码抽象方式

### 调用函数

官方函数网站：[进入](http://docs.python.org/3/library/functions.html)

可以使用 `help(abs)`查看`abs`函数的帮助信息

```shell
>>> abs(100)
100
>>> abs(-20)
20
>>> abs(12.34)
12.34
```

* max(x1, x2...)：去最大值
* int('123')：强制转换为整数
* float('12.45')：强制转换为浮点型
* str(1.23)：强制转换为字符串
* bool(1)：强制转换为布尔型
* hex(int)：转为十六进制数值

调用Python函数需要根据定义，传入正确的参数。如果函数调用出错，需要会看错误信息

### 定义函数

```python
# abstest.py
# -*- coding: utf-8 -*-
def my_abs(x):
    if x >= 0:
        return x
    else:
        return -x    
```

把上述的`my_abs()`函数定义保存到`abstest.py`文件中，然后可以使用`from abstest import my_abs` 来导入`my_abs()`函数，注意`abstest`是文件名

空函数：空逻辑

```python
def nop():
    pass
```

参数检查：如只允许整数和浮点，当传入了不恰当的参数时，打印错误信息

```python
# -*- coding: utf-8 -*-
def my_abs(x):
    if not isinstance(x, (int, float)):
        raise TypeError('bad operand type')
    if x >= 0:
        return x
    else:
        return -x
```

添加了参数检查后，如果传入错误的参数类型，函数就可以抛出一个错误：

```python
Traceback (most recent call last):
  File "client.py", line 2, in <module>
    print(my_abs('A'))
  File "abstest.py", line 4, in my_abs
    raise TypeError('bad operand type')
TypeError: bad operand type
```

返回多个值：

```python
import math

def move(x, y, step, angle=0):
    nx = x + step * math.cos(angle)
    ny = y - step * math.sin(angle)
    return nx, ny
```

`import math`语句表示导入`math`包，并允许后续代码引用`math`包里的`sin`，`cos`等函数

```python
>>> x, y = move(100, 100, 60, math.pi / 6)
>>> print(x, y)
151.96152422706632 70.0
```

但其实这是一种假象，Python函数返回的仍然是单一值：

```python
>>> r = move(100, 100, 60, math.pi / 6)
>>> print(r)
(151.96152422706632, 70.0)
```

原来返回值是一个tuple！但是，在语法上，返回一个tuple可以省略括号，而多个变量可以同事接收一个tuple，按位置赋给对应的值，所以Python函数返回值其实就是返回一个tuple。但写起来更方便


### 函数的参数

必选参数、默认参数、可变参数、关键字参数和命名关键字参数

默认参数值：

```python
def power(x, n=2):
    s = 1
    while n > 0:
        n = n - 1
        s = s* x
    return s
```


可变参数：
1. 组装tuple或list
2. 变长

```python
# 传入tuple或list
# 调用：calc(1, 2, 3)
def calc(numbers):
    sum = 0
    for n in numbers:
        sum = sum + n * n
    return sum
```

```python
# 变长参数
# 调用：calc(1,2,3)
def calc(*numbers):
    sum = 0
    for n in numbers:
        sum = sum + n * n
    return sum
```

关键字参数：允许传入0个或任意个含参数名的参数，这些关键字参数在函数内部自动组装为一个dict

```python
def person(name, age, **kw):
    print('name:', name, 'age:', age, 'other:', kw)
```

函数`person`除了必选参数`name`和`age`外，还接受关键字参数`kw`

```shell
# 只传入必选参数
>>> person('Michael', 30)
name: Michael age: 30 other: {}
```

```python
# 传入任意个数的关键字参数
>>> person('Bob', 35, city='Beijing')
name: Bob age: 35 other: {'city': 'Beijing'}
>>> person('Adam', 45, gender='M', job='Engineer')
name: Adam age: 45 other: {'gender': 'M', 'job': 'Engineer'}
```

```python
# 包含各类参数的函数定义
def f1(a, b, c=0, *args, **kw):
    print('a =', a, 'b =', b, 'c =', c, 'args =', args, 'kw =', kw)

def f2(a, b, c=0, *, d, **kw):
    print('a =', a, 'b =', b, 'c =', c, 'd =', d, 'kw =', kw)
```

```python
# 函数调用
>>> f1(1, 2)
a = 1 b = 2 c = 0 args = () kw = {}
>>> f1(1, 2, c=3)
a = 1 b = 2 c = 3 args = () kw = {}
>>> f1(1, 2, 3, 'a', 'b')
a = 1 b = 2 c = 3 args = ('a', 'b') kw = {}
>>> f1(1, 2, 3, 'a', 'b', x=99)
a = 1 b = 2 c = 3 args = ('a', 'b') kw = {'x': 99}
>>> f2(1, 2, d=99, ext=None)
a = 1 b = 2 c = 0 d = 99 kw = {'ext': None}
```

### 递归

```python
def fact(n):
    if n==1:
        return 1
    return n * fact(n - 1)
```

使用递归函数的优点是逻辑简单清晰，缺点是过深的调用会导致栈溢出。

针对尾递归优化的语言可以通过尾递归防止栈溢出。尾递归事实上和循环是等价的，没有循环语句的编程语言只能通过尾递归实现循环。

Python标准的解释器没有针对尾递归做优化，任何递归函数都存在栈溢出的问题。

## 高级特性

代码越少，开发效率越高

### 切片

取一个list或tuple的部分元素，如下list：

```python
L = ['Michael', 'Sarah', 'Tracy', 'Bob', 'Jack']
```

* L[0:3] or L[:3]：取前三个元素
* L[-2:]：倒数第二个到最后一个

### 迭代

`for...in`

可以使用`list`、`tuple`、`dict`等

```python
# list或tuple
for (i=0; i<length; i++) {
    n = list[i];
}
```

```python
d = {'a': 1, 'b': 2, 'c': 3}
# 默认迭代key
for key in d:
    print(key)
# 迭代value
for value in d.values()
# 同时迭代key和value
for k, v in d.items()
```

判断一个对象是否可迭代：

```python
from collections.abc import Iterable
isinstance('abc', Iterable)
# True
isinstance([1,2,3], Iterable)
# True
isinstance(123, Iterable)
# False
```

获取list每个元素下标：内置的`enumerate`函数把`list`变成索引-元素树

```python
for i, value in enumerate(['A', 'B', 'C']):
    print(i, value)
```

任何可迭代对象都可以作用于`for`循环，包括我们自定义的数据类型，只要符合迭代条件，就可以使用`for`循环

### 列表生成式

运用列表生成式，可以快速生成list，可以通过一个list推导出另一个list，而代码却十分简洁。

```python
# [1*1, 2*2, 3*3..., 10*10]
[x * x for x in range(1, 11)]
# [1, 4, 9, 16, 25, 36, 49, 64, 81, 100]

# 筛选出仅偶数的平方
[x * x for x in range(1, 11) if x % 2 == 0]
# [4, 16, 36, 64, 100]

# 使用两层循环，生成全排列
[m + n for m in 'ABC' for n in 'XYZ']
```

### 生成器-generator

> 一边循环一边计算的机制

```python
# 定义generator
g = (x * x for x in range(10))
# 依次打印
next(g)
```

定义斐波拉契数列：

```python
def fib(max):
    n, a, b = 0, 0, 1
    while n < max:
        print(b)
        a, b = b, a + b
        n = n + 1
    return 'done'
```

上述的函数和generator很类似，要把`fib`函数变成generator函数，只需要把`print(b)`改为`yield b`就可以了

```python
def fib(max):
    n, a, b = 0, 0, 1
    while n < max:
        yield b
        a, b = b, a + b
        n = n + 1
    return 'done'
```

generator函数执行逻辑：每次调用`next()`时执行，遇到`yield`语句返回，再次执行从上次返回的`yield`语句处继续执行

generator是非常强大的工具，在Python中，可以简单地把列表生成式改成generator，也可以通过函数实现复杂逻辑的generator

### 迭代器

凡是可作用于for循环的对象都是Iterable类型；

凡是可作用于next()函数的对象都是Iterator类型，它们表示一个惰性计算的序列；

集合数据类型如list、dict、str等是Iterable但不是Iterator，不过可以通过iter()函数获得一个Iterator对象。

## 函数式编程

函数式编程是一种抽象程度很高的编程范式，纯粹的函数式编程语言编写的函数没有变量，因此，任意一个函数，只要输入是正确的，输出就是确定的

特点：允许把函数本身作为参数传入另一个函数，还允许返回一个函数

### 高阶函数

变量可以指向函数名

```python
f = abs
f(-10)
```

### 传入函数

一个函数可以接收另一个函数作为参数--高阶函数

```python
def add(x, y, f):
    return f(x) + f(y)
```

```python
x = -5
y = 6
f = abs
f(x) + f(y) ==> abs(-5) + abs(6) ==> 11
return 11
```

编写高阶函数，就是让函数的参数能够接收别的函数。

#### map/reduce

* map()：接收两个参数，一个是函数，一个是`Iterator`，`map`将传入的函数一次作用到序列的每个元素，并把结果作为新的`Iterator`返回

```shell
>>> def f(x):
...     return x * x
...
>>> r = map(f, [1, 2, 3, 4, 5, 6, 7, 8, 9])
>>> list(r)
[1, 4, 9, 16, 25, 36, 49, 64, 81]
```

`map()`传入的第一个参数是`f`，即函数对象本身。由于结果`r`是一个`Iterator`，`Iterator`是惰性序列，因此通过`list()`函数让它把整个序列都计算出来并返回一个list

```python
>>> list(map(str, [1, 2, 3, 4, 5, 6, 7, 8, 9]))
['1', '2', '3', '4', '5', '6', '7', '8', '9']
```

* reduce()：把一个函数作用在一个序列`[x1, x2, x3,...]`上，这个函数必须接收两个参数，`reduce`把结果继续和序列的下一个元素做累积计算，其效果就是：

```python
reduce(f, [x1, x2, x3, x4]) = f(f(f(x1, x2), x3), x4)
```

#### filter

### sorted



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