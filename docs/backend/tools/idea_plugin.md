# 推荐这几款好用的IDEA插件，一定不要错过

## 前言

古人云：工欲善其事必先利其器

作为一个`JAVA`开发攻城狮(又叫`新时代农民工`)，我们每天接触到最多的软件，那必然是`IDEA`（如果你是用的是`Eclipse`，当我没说hhhh）

好了，接下来开始正文，下面的`IDEA`插件都是我一直在使用的，而且确实在日常开发中起到了很大作用

## 插件集合

### lombok

我相信这个插件应该是`JAVA`开发必备的一个插件了吧(除了个别公司有强制要求外)，可以说几乎没有人不爱它的，但是，我们真的能使用好这个插件吗？恐怕不一定

**优点：** Lombok项目是一个Java库，它会自动插入编辑器和构建工具中，Lombok提供了一组有用的注释，用来消除Java类中的大量样板代码。仅五个字符(@Data)就可以替换数百行代码从而产生干净，简洁且易于维护的Java类。

**缺点：** Lombok也存在一定风险，在一些开发工具商店中没有Project Lombok支持选择。 IDE和JDK升级存在破裂的风险，并且围绕项目的目标和实施存在争议。

**示例如下：**

```java
import lombok.AllArgsConstructor;
import lombok.Builder;
import lombok.Data;
import lombok.NoArgsConstructor;

import java.io.Serializable;

/**
 *  @description 用户实体类
 *
 *  @author wudiguang
 *  @date 2022/8/23 18:03
 */
@Data
@Builder
@NoArgsConstructor
@AllArgsConstructor
public class User implements Serializable {
    /**
     * 用户名
     */
    private String username;

    /**
     * 密码
     */
    private String password;

    /**
     * 昵称
     */
    private String nickname;

    /**
     * @Setter：注解在类或字段，注解在类时为所有字段生成setter方法，注解在字段上时只为该字段生成setter方法。
     * @Getter：使用方法同上，区别在于生成的是getter方法。
     * @ToString：注解在类，添加toString方法。
     * @Build：支持链式构建对象。`User.builder().username("wudiguang").password("5555").nickname("wdg").build();`
     * @EqualsAndHashCode：注解在类，生成hashCode和equals方法。
     * @NoArgsConstructor：注解在类，生成无参的构造方法。
     * @RequiredArgsConstructor：注解在类，为类中需要特殊处理的字段生成构造方法，比如final和被@NonNull注解的字段。
     * @AllArgsConstructor：注解在类，生成包含类中所有字段的构造方法。
     * @Data：注解在类，生成setter/getter、equals、canEqual、hashCode、toString方法，如为final属性，则不会为该属性生成setter方法。
     * @Slf4j：注解在类，生成log变量，严格意义来说是常量。
     */
}
```

### EasyCode

这是一个能将我们从无脑的单表`CURD`中解放出来的必备插件！！！一键生成所有表的基础MVC代码，包括`controller`,`service`,`mapper`,`xml`,`entity`等文件，其中生成的数据库类型与java实体类的映射关系十分方便，使用EasyCode可以大大节省我们在这些简单重复操作上耗费的大量时间，使我们有更多的时间去关注业务和架构上的问题，有利于软件快速开发。

另外，EasyCode还提供了自定义模板规则的功能，也就是说，我们可以自己去DIY更加符合我们项目的各个用于生成代码的模块文件！！！真的太爱了，有没有一种相见恨晚的感觉？

todo 插入图片

todo 插入图片

todo 插入图片

### Git Commit Template

这是一个用于规范我们日常开发中`Git`提交内容的插件，这里的插件message规范采用的是`Angular`规范，我们可以去`Angular`官方仓库看看，看看人家是如何将提交信息写的那么精致完善,[在线地址](https://github.com/angular/angular)

可能开发者并没有意识到`Git`提交日志的重要性，或许你现在打开`IDEA`就能发现项目的提交信息全是`update xxx`的提交日志

我们可以参照[【阮一峰】老师写的提交规范](http://www.ruanyifeng.com/blog/2016/01/commit_message_change_log.html)

todo 插入图片

### Maven Helper

在写Java代码的时候，我们可能会出现Jar包的冲突的问题，这时候就需要我们去解决依赖冲突了，而解决依赖冲突就需要先找到是那些依赖发生了冲突，当项目比较小的时候，还可以依靠IEDA的【Diagrams】查看依赖关系，当项目比较大依赖比较多后就比较难找了，这时候就`Maven Helper`插件就能实现快速解决依赖冲突了。

todo图片

**其中三个选项分别表示如下:**

* Conflicts：查看冲突
* All Dependencies as List：列表形式查看所有依赖
* All Dependencies as Tree：树形式查看所有依赖

当出现冲突需要解决时，下面会显示冲突的信息，我们可以选择冲突的依赖 Exclude它。

### JRebel

IDEA 热部署插件

JRebel是一款JVM插件，它使得Java代码修改后不用重启系统，立即生效。IDEA上原生是不支持热部署的，一般更新了 Java 文件后要手动重启 Tomcat 服务器，才能生效。目前对于idea热部署最好的解决方案就是安装JRebel插件。

[IDEA热部署配置使用](https://blog.csdn.net/liuming690452074/article/details/125236755)

## 总结

`IDEA`作为`JAVA`开发者使用最多的开发软件，我们可以使其最大化的节省我们的开发时间，节省出来的时间我们可以用来摸鱼，也可以多看看技术博客来提升我们的技术水平。

推荐摸鱼网站：[今日热榜](https://tophub.today/) 和 [程序员导航](https://tooool.org/)