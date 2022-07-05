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

### 第1条：用静态工厂方法代替构造器

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

### 第2条：遇到多个构造器参数时要考虑使用构建器

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

### 第3条：用私有构造器或者枚举类型强化Singleton属性

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

### 第4条：通过私有构造器强化不可实例化的能力

工具类不希望被实例化，因为实例化对它没有任何意义。然而，在缺少显式构造器的情况下，编译器会自动提供一个公有的、无参的缺省构造器。

企图通过将类做成抽象类来强制该类不可被实例化时行不通的。

让这个类包含一个私有构造器，他就不能被实例化

```java
// Noninstantiable utility class
public class UtilityClass{
    private UtilityClass(){
        throw new AssertionError();
    }
    // Remainder omitted
}
```

### 第5条：优先考虑依赖注入来引入资源

有许多类会依赖一个或多个底层的资源。例如，拼写检查其需要依赖词典。因此，像下面这样把类实现为静态工具类的做法并不少见

1. 静态工具类
```java
public class SpellChecker{
    private static final Lexicon dictionary = "";
    private SpellChecker(){}
    public static boolean isValid(String word){...}
    public List<String> suggestions(String typo){}
}
```

2. 实现为Singleton

```java
public class SpellChecker{
    private final Lexicon dictionary = "";
    private SpellChecker(){}
    public static SpellChecker INSTANCE = new SpellChecker();
    public static boolean isValid(String word){...}
    public List<String> suggestions(String typo){}
}
```

以上两种方法都不理想，因为它们都是假定只有一本词典可用。实际上，每一种语言都有自己的词典，特殊词汇还要使用特殊的词典。

静态工具和Singleton类不适合于需要引用底层资源的类。

这里需要的是能够支持类的多个实例，每个实例都是用客户端指定的资源。满足该需求的最简单的模式是，当创建一个新的实例时，就将该资源传到构造器中。这就是依赖注入的一种形式：词典时拼写检查其的一个依赖，在创建拼写检查器时就将词典注入其中

```java
public class SpellChecker{
    private Lexicon dictionary;

    public SpellChecker(Lexicon dictionary){
        this.dictionary = Objects.requireNonNull(dictionary);
    }
    public static boolean isValid(String word){...}
    public List<String> suggestions(String typo){}
}
```

总而言之，不要用Singleton和静态工具来实现依赖一个或多个底层资源的类，且该资源的行为会影响到该类的行为；也不要直接用这个类来创建这些资源。而应该将这些资源或者工厂传给构造器（或者静态工厂，或者构建器），通过它们来创建类。这个实践就被称为依赖注入，它极大地提升了类的灵活性、可重用性和可测试性

### 第6条：避免创建不必要的对象

一般来说，最好能重用单个对象，而不是在每次需要的时候就创建一个相同功能地新对象。重用方式既快速，又流行。如果对象是不可变的，它就始终可以被重用。

作为一个极端的反面例子，看看下面地语句

```java
String s = new String("bikini");  // DO NOT DO THIS
```

该语句每次被执行的时候都会创建一个新的String实例，但是这些创建对象地动作全都是不必要的。传递给String构造器的参数“bikini”本身就是一个String实例，功能方面等同于构造器创建地所有对象。如果这种用法是在一个循环中，或者是在一个被频繁调用地方法中，就会创建出千万个不必要的String实例。

改进后的版本如下：

```java
String s = "Bikini";
```

对于同时提供了静态工厂方法和构造器的不可变类，通常优先使用静态工厂方法而不是构造器，以避免创建不必要的对象。

静态工厂方法Boolean.valueOf(String) 几乎总是优先于构造器Boolean(String)

有些对象创建地成本比其他对象要高得多。如果重复地需要这类“昂贵的对象”，建议将它缓存下来重用。遗憾的是，在创建这种对象的时候，并非总是那么显而易见。

假设想要编写一个方法，确定一个字符串是否为一个有效的罗马数字。下面介绍一种最容易的方法，使用一个正则表达式：

```java
static boolean isRomanNumeral(String s){
    return s.matches("...");
}
```

这个实现的问题在于它依赖String.matches()。虽然String.matches方法最容易于查看一个字符串是否与正则表达式相匹配，但并不适合在注重性能的情形中重复使用。问题在于，它在内部为正则表达式创建了一个Pattern实例，却只用了一次，之后就可以进行垃圾回收了。创建Pattern实例的成本很高，因为需要将正则表达式编译成一个有限状态机

为了提升性能，应该显式地将正则表达式编译成一个Pattern实例（不可变），让它变成类初始化的一部分，并将它缓存起来，每当调用isRomanNumeral方法的时候就重用同一个实例：

```java
public class RomanNumberals{
    private static final Pattern ROMAN = Pattern.compile("...");

    static boolean isRomanNumberal(String s){
        return Roman.matcher(s).matches();
    }
}
```

改进后的isRomanNumberal方法如果被频繁地使用，会显示出明显地性能优势。

要优先使用基本类型，而不是装箱基本类型，要当心无意识的自动装箱。

### 第7条：消除过期地对象引用

由于Java语言具有垃圾回收功能，使得程序员的工作变得更加容易，因为当你用完了对象之后，它们会被回收。

```java
public class Stack{
    private Object[] elements;
    private int size = 0;
    private static final int DEFAULT_INITIAL_CAPACITY = 16;

    public Stack(){
        elements = new Object[DEFAULT_INITIAL_CAPACITY];
    }

    public void push(Object e){
        ensureCapacity();
        elements[size++] = e;
    }

    public Object pop(){
        if(size == 0){
            throw new EmptyStackException();
        }
        return elements[--size];
    }

    private void ensureCapacity(){
        if(elements.length == size){
            elements = Arrays.copyOf(elements, 2 * size + 1);
        }
    }
}
```

这个程序隐藏着一个问题：随着垃圾回收器活动增加，或者由于内存占用的不断增加，程序性能地降低会逐渐表现出来。极端情况下，这种内存泄漏会导致磁盘交换，甚至导致程序失败（OutOfMemoryError错误），但是这种失败情形相对比较少见。

如果一个栈先是增长，然后再收缩，那么，从栈中弹出地对象将不会被当作垃圾回收，即使使用栈地程序不再引用这些对象，它们也不会被回收。这是因为栈内部维护着对这些对象地过期引用。所谓的过期引用，是指永远也不会再被解除地引用。

如果一个对象引用被无意识地保存起来了，那么垃圾回收机制不仅不会处理这个对象，而且也不会处理这个对象所引用地所有其他对象。

这类问题修复方法很简单：一旦对象引用已经过期，只需清除这些引用即可。

```java
public Object pop(){
        if(size == 0){
            throw new EmptyStackException();
        }
        Object element = elements[--size];
        elements[size] = null;
        return element;
    }
```

清空过期引用地另一个好处是，如果它们以后又被错误地解除引用，程序就会立即抛出NullPointerException，而不是悄悄地错误运行下去。

内存泄露的另一个常见来源是缓存。

一旦你把对象引用放到缓存中，它就很容易被遗忘，从而使得它不再有用之后很长一段时间仍然留在缓存中。

只要在缓存之外存在对某个项的键的引用，该项就有意义，那么就可以用WeakHashMao代表缓存。

内存泄漏的第三个常见来源是监听器和其他回调。

注册回调后却没有显式地取消注册

### 第8条：避免使用终结方法和清除方法

终结方法（finalizer）通常是不可预测地，也是很危险地，一般情况下是不必要的。

在Java中，当一个对象变得不可达时，垃圾回收器会回收与该对象相关联地存储空间，并不需要程序员做专门的工作。

永远不应该依赖终结方法或者清除方法来更新重要的持久状态。

使用终结方法和清除方法有一个非常重要的性能损失

终结方法有一个严重的安全问题

### 第9条：try-with-resources 优先于 try-finally

Java类库中包括许多必须通过调用 close 方法来手工关闭的资源。如InputStream、OutputStream和java.sql.Connection等。客户端经常会忽略资源的关闭，造成严重的性能后果也就可想而知了

根据经验，try-finally语句是确保资源会被适当关闭的最佳方法，就算发生异常或者返回也一样，但是如果再添加第二个资源，就会一团糟了。

```java
static String firstLineOfFile(String path) throws IOException{
    try(BufferedReader br = new BuffereReader(
        new FileReader(path))){
        return br.readLine();
    }
}
```

使用try-with-resources 不仅使代码变得更简洁易懂，也更容易进行诊断。

结论：在处理必须关闭的资源时，始终要优先考虑使用try-with-resources，而不是try-finally


## 第三章 对于所有对象都通用的方法

Object 非final 方法（equals、hashCode、toString、clone和finalize）都有明确的约定，因为它们设计成是要被覆盖的。

如果不能做到这一点，其他依赖于这些约定的类（HashMap、HashSet）就无法结合该类一起正常运作

### 第10条：覆盖equals时请遵守通用约定

* 类的每个实例本质上都是唯一的
* 类没有必要提供“逻辑相等”的测试功能
* 超类已经覆盖了equals，超类的行为对于这个类也是合适的
* 类时私有的，或者包级私有的，可以确定它的equals方法永远不会被调用

如果类具有自己特有的“逻辑相等”概念，而且超类还没有覆盖equals。如Integer，String等

* 自反性：对于任何非null的引用值x，x.equals(x) 必须返回true
* 对称性
* 传递性
* 一致性
* 对于任何非null的引用值，x.equals(null) 必须返回true

<!-- P41 -->

## 第四章 类和接口

## 第五章 泛型

## 第六章 枚举和注解

## 第七章 Lambda 和 Stream

## 第八章 方法

## 第九章 通用编程

## 第十章 异常

## 第十一章 并发

## 第十二章 序列化

