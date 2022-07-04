# 《Effiective Java》

```desc
date: 2022-07-01
address: NC
author: 吴第广
```

## 第一章 引言

目的：帮助读者更有效地使用 Java 及其类库（util、io、concurrent、function）等

本书大部分内容都不是讨论性能的，二十关心如何编写出清晰、正确、可用、健壮、灵活和可维护的程序来。

## 第二章 创建和销毁对象

### 第一条：用静态工厂方法代替构造器

* 提供一个共有的构造器
* 提供一个共有的静态工厂方法：返回类的实例

例子：
```java
public static Boolean valueOf(boolean b) {
    return (b ? TRUE : FALSE);
}
```

优点：

* 静态工厂方法有更加具体的名称。如BigInteger.probablePrime() 显然比BigInteger(int, int, Random) 返回的更加清楚
* 不必在每次调用它们的时候都创建一个新对象。可以使用预先构建好的实例，或者将构建好的实例缓存起来。
* 可以返回原返回类型的任何子类型的对象
* 所返回的对象的类可以随着每次调用而发生变化。这取决于静态工厂方法的参数值。
* 方法返回的对象所属的类，在编写包含该静态工厂方法的类时可以不存在

缺点：

1. 类如果不含共有的或者受保护的构造器，就不能被子类化

- from：类型转换方法
- of：聚合方法
- valaueOf：比from和of更繁琐的一种替换
- instance或者getInstance：返回的实例是同过方法参数来描述的
* create或者newInstance：能够每次确保调用同一个实例
* get Type

### 第二条：遇到多个构造器参数时要考虑使用构建器

静态功能和构造器有个共同的局限性：它们都不能很好地扩展到大量的可选参数。

对于参数较多时，程序员经常采用重叠构造器模式。提供的第一个构造器只有必要的参数，第二个构造器有一个可选参数，第三个构造器有两个可选参数，以此类推，最后一个构造器包含所有可选的参数。

简而言之，重叠构造器模式可行，但是当有许多参数的时候，客户端代码会很难编写，并且仍然难以阅读。

`建造者模式`

```java
package com.wdg.effectivejava.chapter1;
import com.alibaba.fastjson.JSON;
import java.io.Serializable;

/**
 * @author wudiguang
 * @Description 建造者组装对象
 * @Date 2022/7/1 17:55
 */
public class NutritionFacts implements Serializable {
    // ignore getter&setter
    private Integer servingSize;
    private Integer servings;
    private Integer calories;
    private Integer fat;
    private Integer sodium;
    private Integer carbohydrate;

    public static class Builder {
        // 必填参数
        private Integer servingSize;
        private Integer servings;
        //可选参数 -- 使用默认值初始化
        private Integer calories = 0;
        private Integer fat = 0;
        private Integer sodium = 0;
        private Integer carbohydrate = 0;

        public Builder(Integer servingSize, Integer servings) {
            this.servingSize = servingSize;
            this.servings = servings;
        }

        public Builder calories(Integer var) {
            this.calories = var;
            return this;
        }

        public Builder fat(Integer var) {
            this.fat = var;
            return this;
        }

        public Builder sodium(Integer var) {
            this.sodium = var;
            return this;
        }

        public Builder carbohydrate(Integer var) {
            this.carbohydrate = var;
            return this;
        }

        public NutritionFacts build() {
            return new NutritionFacts(this);
        }
    }

    private NutritionFacts(Builder builder) {
        servingSize = builder.servingSize;
        servings = builder.servings;
        calories = builder.calories;
        fat = builder.fat;
        sodium = builder.sodium;
        carbohydrate = builder.carbohydrate;
    }

    public static void main(String[] args) {
        NutritionFacts nutritionFacts = new Builder(240, 8).calories(100).sodium(35).fat(0).carbohydrate(27).build();
        System.out.println(JSON.toJSONString(nutritionFacts));
    }
}
```

Builder 模式也适用于类层次结构。

<!-- P23 -->
与构造器相比，builder的微略优势在于，它可以有多个可变参数。因为builder是利用单独额方法来设置每一个参数。

Builder模式十分灵活，可以利用单个builder构建多个对象。builder的参数可以在调用build方法来创建对象期间进行调整，也可以随着不同的对象而改变，builder可以自动填充某些域，例如每次创建对象时自动增加序列号

Builder模式的确也有它自身的不足。为了创建对象，必须先创建它的构建器。虽然创建这个构建器的开销在实践中可能不那么明显，但在某些十分注重性能的情况下，可能就成问题了。

如果类的构造器或者静态工厂中具有多个参数，设计这种类时，Builder模式就是一种不错的选择。

### 第三条：用私有构造器或者枚举类型强化Singleton属性

Singleton是指仅仅被实例化一次的类。Singleton通常被用来代表一个无状态的对象，或者那些本质上唯一的系统组件。使类成为Singleton会使它的客户端测试变得十分空难。

实现Singleton有两种常见的方法（都要保持构造器为私有的），并导出公有的静态成员，以便允许客户端能够访问该类的唯一实例、

1. 公有静态成员是一个final域
```java
public class Elvis{
    public static final Elvis INSTANCE = new Elvis();
    private Elvis(){}
    public void leaveTheBuilding(){}
}
```

2. 公有的成员是一个静态工厂方法
```java
public class Elvis{
    private static final Elvis INSTANCE = new Elvis();
    private Elvis(){}
    public static Elvis getInstance(){return INSTANCE}
    public void leaveTheBuilding(){}
}
```

为了维护并保证Singleton，必须声明所有实例域都是瞬时的，并提供一个readResolve方法。否则，每次反序列化一个序列号的实例时，都会创建要给新的实例。

3. 声明一个包含单个元素的枚举类型
```java
public enum Elvis{
    INSTANCE;

    public void leaveTheBuilding(){}
}
```

### 第四条：通过私有构造器强化不可实例化的能力



## 第三章 对于所有对象都通用的方法

## 第四章 类和接口

## 第五章 泛型

## 第六章 枚举和注解

## 第七章 Lambda 和 Stream

## 第八章 方法

## 第九章 通用编程

## 第十章 异常

## 第十一章 并发

## 第十二章 序列化

