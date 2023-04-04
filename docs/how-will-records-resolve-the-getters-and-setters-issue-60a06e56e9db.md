# 记录将如何解决 Getters 和 Setters 问题

> 原文：<https://blog.devgenius.io/how-will-records-resolve-the-getters-and-setters-issue-60a06e56e9db?source=collection_archive---------4----------------------->

## 记录会让 getters 过时吗？

![](img/5b0c8d79396b21d474a381739a5fb0dc.png)

Brixiv 的照片:[https://www . pexels . com/photo/photo-a-border-collie-dog-catching-a-nettle-ball-at-beach-7316511/](https://www.pexels.com/photo/photo-of-a-border-collie-dog-catching-a-tennis-ball-at-the-beach-7316511/)

[吸气剂和设置剂是邪恶的](https://www.yegor256.com/2014/09/16/getters-and-setters-are-evil.html)。或者他们不是。

***无论哪种方式，你每天都在使用它们。***

***那么 Java 中的 getters 和 setters 会灭绝吗？未来什么会取代它们？除了 setters，我们还有其他选择吗？***

Lombok 已经着手解决 getters 问题。Lombok 为您生成了许多样板文件。Lombok 让生活变得更简单，但是 Java 会解决问题的根源。

***Java 将如何解决 getters 和 setters 的问题？让我们看看。***

# 您仍将使用 getters 来隐藏实现

大多数人认为 getters 公开了实现。结果是脆弱的代码。

***这是什么意思？如何用 getters 隐藏实现？***

如果您可以更改实现，但 getter 可以保持不变，那么您就处于有利位置。所以 getters 应该与实现无关。

是的，`getX`对于 x 变量来说并不是一个伟大的设计。稍后我们将详细讨论记录如何解决这个问题。

***那么有什么好的吸气剂例子呢？***

***Spring 的***[***oauth 2 user***](https://docs.spring.io/spring-security/site/docs/current/api/org/springframework/security/oauth2/core/user/OAuth2User.html)***接口就是一个很好的例子。*** 它公开了`getName`方法，在实现中可以有自己的东西。您的唯一名称可以是 JWT 解码的声明或用户属性。

只要你兑现承诺，外面的客户不会在意。这让我们回到为什么 getters 是一个好的实践。

***也随着我们前进，吸气剂帮助我们清理。***

随着我们的进一步发展，我们可以这样做。

如果我们直接访问这些属性，会是什么样子？

如果您更改为其他唯一标识符，您将需要更改类。

此外，如果你搬到一个不同的物业，你将很难逐步淘汰旧物业。假设我们添加了 userId，但留下了 jwtClaim。您需要某种方式将它标记为不推荐使用。

这里另一件重要的事情是不要将 getter 名称与底层实现捆绑在一起。正如我们在这里看到的，getter 是`getName`，而不是`getClaim`或`getId`。

***为什么 getter 名字很重要？***

这减少了脑力劳动，因为我们不需要知道下面是什么。只要我们有一个唯一的标识符，我们就没事了。

*使用 getters 我们能得到什么？*重构安全，API 不变，动作更快。

Getter 名称对于抽象级别来说应该是正确的。如果使用`getUserId`的话，依赖者是否需要知道课程提供了什么？最好将实现隐藏在一个`getUniqueId`后面。

***暴露一切会导致不良使用。***

比如`Unsafe`在模块系统之前就曝光了。如果您已经从 Java 8 迁移到 Java 11，您会看到这个类。这是因为某些图书馆使用它，即使它是不安全的。

这个类不是为公共使用而设计的，但是如果没有模块的话，Unsafe 是可以被访问的。当事情发生变化或者 Java 停止公开这个类时，您可能会失去支持。

# 您将使用 withers 创建不变的，而不是 setters

初始化记录或设置某种状态的唯一方法是通过规范的构造函数。

因为变异，Setters 不受欢迎。记录只希望通过构造函数来保存突变。当然，开发人员利用(或滥用)这一事实，创建定制的 withers 来设置记录的部分更新。

```
static Test fromA(double a) {  
    double b = calc(a);  
    double c = calc(a + b);  
    return new Test(a,b,c);  
}
```

如果您希望从单个字段中计算其他记录成员，则可以使用此示例。唯一惯用的方法是这样做。没有其他方法，我们仍然可以调用紧凑构造函数。

***解决这个问题的一个办法就是枯萎。***

Withers 可能会有一个用于添加 setter 逻辑的块。这样，您可以从一个参数中计算出一个成员。威瑟斯可以做的一个例子是:

今天我们已经有了类似的东西。那是龙目岛的`WithBy`。

`[***WithBy***](https://github.com/projectlombok/lombok/issues/2368)` ***会像琥珀提供的马肩隆。***

至少想法是一样的。您将在 withers 中编写任意代码。如上例所示。

`with { /* your init code here*/ }`

此外，如果你看一下由发表的关于 WithBy 的[评论，你会看到 Valhalla 和 Amber 将要解决的问题。比如自我参照型，这种原始瓦尔哈拉型是不会让发生的。这就是为什么这个问题需要一个语言解决方案。](https://github.com/projectlombok/lombok/issues/2368)

正如龙目岛撰稿人所说，这里的另一个问题是不变性的成本。

构建器对原始对象进行变异。所以如果你想改变 4 个属性，你总共有 2 个对象。对于 withers，将有 3 个中间对象。这将给 GC 带来压力，所以这也是 Java 应该解决的问题。

Withers 将为嵌套对象操作提供更好的语法。

如果我们研究一下 WithBy 文档，我们会看到它解决了什么问题。对嵌套对象字段的修改变得很麻烦，而用 By sets 来简化这一操作。

```
movie = movie.withDirector(movie.getDirector().withBirthDate(movie.getDirector().getBirthDate().plusDays(1)));
```

使用`WithBy`,这应该可以归结为以下代码:

```
movie = movie.withDirectorBy(d -> d.withBirthDateBy(bd -> bd.plusDays(1)));
```

对于 Java withers，将会是这样的[:](https://mail.openjdk.org/pipermail/amber-dev/2020-August/006485.html)

```
minorityReport = minorityReport with { director with { dateOfBirth with { year = 1956 }}};
```

# 不变量的 Getters 没有意义

记录模式已经在远离 getters 方面做了很多工作。

记录的另一个优点是快速重构。如果将记录与密封类型结合起来，重构会更容易。当您更改记录构造函数时，您也更改了每个 getter。

密封类型到底能为你做什么？您可以[将开关与记录及其类似元组的特性结合起来，以获得简洁的代码](https://mail.openjdk.org/pipermail/amber-dev/2022-October/007518.html)。下面是没有穷举开关的示例代码。

和详尽的开关:

如果使用密封类型，新的允许类型需要更新所有开关。穷举开关实现了这一特性。同样在未来，这个特性也会转移到任意职业。

目前，解构对自定义类不起作用。这是一个枚举解构的错误。

如果你有以下定义。

下面的代码会抛出一个错误。

***关于 getters 和 setters 将在哪里不复存在，你有其他想法吗？***