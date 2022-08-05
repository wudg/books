# Go 语言学习基础例子

## 变量

```go
// variable.go
package main
import "fmt"
func main() {
	// 变量定义
	// var identifier type

	// 标准声明变量
	var basic string = "wudiguang"

	// 基础变量默认值
	// 数值 ---> 0
	// bool ---> false
	// 字符串 ---> 空串
	// 其他 ---> nil
	var mynumber int
	var mybool bool
	var mystring string
	// wudiguang 0 false
	fmt.Println(basic, mynumber, mybool, mystring)

	// 其他变量默认值
	var mypoint *int
	var myarray []int
	var mymap map[string]int
	var mychan chan int
	var myfunc func(string) int
	var myerror error
	// <nil> [] map[] <nil> <nil> <nil>
	fmt.Println(mypoint, myarray, mymap, mychan, myfunc, myerror)

	// 根据值执行判断变量类型
	var specialbool = true
	fmt.Println(specialbool)

	// 变量声明后，不允许使用 := 赋值
	// 下面代码会产生编译错误
	// var intVal int
	// intVal := 3
	// fmt.Println(intVal)

	// 多变量声明
	var multu1, multi2 = 1, 4
	fmt.Println(multu1, multi2)

	// 初始化并赋值，只能在函数体中出现
	transfer1, transfer2 := multi2, multu1
	fmt.Println(transfer1, transfer2)
}
```

## 常量

```go
// constant.go
package main

import (
	"fmt"
)

func main() {
	// 常量定义(显式和隐式)
	// const identifier [type] = value

	const LENGTH int = 10
	const WIDTH int = 5

	fmt.Printf("面积为：%d", LENGTH*WIDTH)

	// 常量还可以用作枚举
	const (
		UNKNOWN = 0
		FEMALE  = 1
		MALE    = 2
	)
	fmt.Println(UNKNOWN, FEMALE, MALE)

	// iota：特殊的常量，可以被认为是一个可以被编译器修改的常量。const中每新增一行常量声明就使iota计数一次
	const (
		fitst  = iota // first = 0
		second = iota // second = first + 1
		three  = iota // three = second + 1
	)

	const (
		iota_a = iota
		iota_b
		iota_c
		iota_d = "ha"
		iota_e
		iota_f = 100
		iota_g
		iota_h = iota
		iota_i
	)
	fmt.Println(iota_a, iota_b, iota_c, iota_d, iota_e, iota_f, iota_g, iota_h, iota_i)
}
```

## 条件

```go
// condition.go
package main

import "fmt"

func main() {
	// 描述：条件语句控制代码执行流程

	// if
	var number1 int = 10
	fmt.Println("number1 的值为", number1)
	if number1 < 20 {
		fmt.Println("number1 小于 20")
	}
	// if...else
	if number1 < 20 {
		fmt.Println("number1 小于 20")
	} else {
		fmt.Println("number1 大于等于 20")
	}
	// if 嵌套
	if number1 < 20 {
		if number1 < 15 {
			fmt.Println("number1 小于 15")
		}
	}
	// switch
	switch number1 {
	case 10:
		fmt.Println("number1 为 10")
	case 15:
		fmt.Println("number1 为 10")
	case 20:
		fmt.Println("number1 为 10")
	default:
		fmt.Println("都不是")
	}
	// select
	var c1, c2, c3 chan int
	var i1, i2 int
	select {
	case i1 = <-c1:
		fmt.Println("received ", i1, " from c1")
	case c2 <- i2:
		fmt.Println("sent ", i2, " to c2")
	case i3, ok := (<-c3): // same as: i3, ok := <-c3
		if ok {
			fmt.Println("received ", i3, " from c3")
		} else {
			fmt.Printf("c3 is closed\n")
		}
	default:
		fmt.Printf("no communication\n")
	}
}
```

## 数据类型

```go
package main

func main() {
	// 描述：数据类型的出现是为了把数据分成所需内存大小不同的数据，编程的时候要用大数据的时候才需要申请大内存，就可以充分利用内存

	// 布尔型：false、true

	// 数字类型：int和浮点型
	// int：uint*、uint16、uint32、uint64、int8、int16、int32、int64
	// 浮点型：float32、float64、complex64、complex128
	// 其他：byte、rune、uint、uintptr

	// 字符串类型：string

	// 派生类型：指针、数组、结构化、Channel、函数、切片、接口、Map

}
```

## 方法

```go
package main

import (
	"fmt"
	"math"
)

/* 定义结构体 */
type Circle struct {
	radius float64
}

// func function_name( [parameter list] ) [return_types] {
// 	  函数体
//  }

func max(num1, num2 int) int {
	var result int

	if num1 > num2 {
		result = num1
	} else {
		result = num2
	}
	return result
}

// 函数返回多个值

func swap(x, y string) (string, string) {
	return y, x
}

// 值传递：指在调用函数时将实际参数复制一份传递到函数中，这样在函数中如果对参数进行修改，将不会影响到实际参数
// 引用传递：指在调用函数时将实际参数的地址传递到函数中，那么在函数中对参数所进行的修改将影响实际参数

// 函数闭包，创建函数getSequence()，返回另外一个函数
func getSequence() func() int {
	i := 0
	return func() int {
		i += 1
		return i
	}
}

func main() {
	fmt.Println("最大值是：", max(10, 15))

	a, b := swap("world", "hello")
	fmt.Println("a=", a)
	fmt.Println("b=", b)

	// 函数作为实参
	getSquareRoot := func(x float64) float64 {
		return math.Sqrt(x)
	}

	// 闭包调用（类匿名方法）
	fmt.Println(getSquareRoot(9))

	nextNumber := getSequence()
	fmt.Println(nextNumber())
	fmt.Println(nextNumber())
	fmt.Println(nextNumber())

	nextNumber1 := getSequence()
	fmt.Println(nextNumber1())
	fmt.Println(nextNumber1())
	fmt.Println(nextNumber1())

	// 调用结构体的函数
	var c1 Circle
	c1.radius = 10.00
	fmt.Println("圆的面积=", c1.getArea())
}

// 定义结构体的内部函数
func (c Circle) getArea() float64 {
	return 3.14 * c.radius * c.radius
}
```

## 循环

```go
package main

import "fmt"

func main() {
	// 重复遍历

	// for
	// for init; condition; post { }
	// for condition { }
	// for { }

	sum := 0
	for i := 0; i < 10; i++ {
		sum += i
	}
	fmt.Println("sum=", sum)

	sum2 := 1
	for sum2 <= 10 {
		sum2 += sum2
	}
	fmt.Println("sum2=", sum2)

	// sum3 := 0
	// for {
	// 	sum3++
	// }

	// For-each range 循环:可以对字符串、数组、切片等进行迭代输出元素

	strings := []string{"google", "runoob"}
	for i, s := range strings {
		fmt.Println(i, s)
	}

	numbers := [6]int{1, 2, 3, 5}
	for i, x := range numbers {
		fmt.Printf("第 %d 位 x 的值 = %d\n", i, x)
	}

	map1 := make(map[int]float32)
	map1[1] = 1.0
	map1[2] = 2.0
	map1[3] = 3.0
	map1[4] = 4.0

	// 读取 key 和 value
	for key, value := range map1 {
		fmt.Printf("key is: %d - value is: %f\n", key, value)
	}

	// 读取 key
	for key := range map1 {
		fmt.Printf("key is: %d\n", key)
	}

	// 读取 value
	for _, value := range map1 {
		fmt.Printf("value is: %f\n", value)
	}

	// for嵌套：同上

	// break

	// continue

	// goto

	// goto label;
	// ..
	// .
	// label: statement;
}
```