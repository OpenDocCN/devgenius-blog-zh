# Go 界面

> 原文：<https://blog.devgenius.io/interfaces-in-go-a0bab96f1027?source=collection_archive---------8----------------------->

如果你打算广泛使用 Go，你需要理解如何使用接口。界面并不是一个特别流行的东西，但是 Go 是这个特性的更广泛的用户之一。接口允许你编写可重用的代码。

# 什么是接口？

接口是将对象分组到它们的公共行为中的一种方式。接口由其名称和对象需要定义的方法来定义。任何定义了这些方法的对象都会“实现”该接口。

比如说甲、乙、丙三个学生都会做饭。他们试图给我提供美味的食物。然而，由于 A、B 和 C 都是在不同的厨师手下受训，所以他们烹饪的东西不同。尽管如此，他们都是会做饭的厨师。我们可以说 A、B、C 实现了“chef”接口，因为它们都可以用不同的方式烹饪。

让我们看看这是如何在 Go 中实现的。

# 在围棋中如何使用它？

下面是上面例子的代码。

```
package mainimport (
    "fmt"
    "math/rand"
)type cook interface {
    cookFood()
    getName() string
}type chef struct {
    name    string
    cuisine string
}func (c chef) cookFood() {
    result := fmt.Sprintf("%s the professional chef is cooking %s food", c.name, c.cuisine)
    fmt.Println(result)
}func (c chef) getName() string {
    return c.name
}type homeCook struct {
    name string
}func (h homeCook) cookFood() {
    result := fmt.Sprintf("%s the home cook is cooking food", h.name)
    fmt.Println(result)
}func (h homeCook) getName() string {
    return h.name
}func serve(c cook) {
    c.cookFood()
    result := fmt.Sprintf("%s: dinner is served!", c.getName())
    fmt.Println(result)
}func main() {
    chef1 := chef{"Brian", "Korean"}
    chef2 := chef{"Vincenzo", "Italian"}
    homeCook1 := homeCook{"Amara"}
    homeCook2 := homeCook{"Dana"} cooks := []cook{chef1, chef2, homeCook1, homeCook2} numCustomers := 100
    for i := 0; i < numCustomers; i++ {
        serve(cooks[rand.Intn(len(cooks))])
    }
}
```

我们定义了一个名为`cook`的接口。我们希望任何能够扮演`cookFood`和`getName`的人能够将自己视为`cook`。

下面，我们定义两个结构`chef`和`homeCook`。两者都有`name`字段，但是只有`chef`有一个`cuisine`字段来定义他或她擅长什么菜。

对于实现`cook`接口的`chef`和`homeCook`结构，它们都需要定义`cookFood`和`getName`方法。实现的细节很简单——只是一个简单的打印语句。

现在我们来看看`main`函数。我们定义了两个`chef`对象`chef1`和`chef2`。我们还定义了两个`homeCook`对象`homeCook1`和`homeCook2`。这四个对象存储在我们的`cooks`切片中，类型为`cook`，我们的接口。通常你不能在一个片上存储不同类型的对象，但是使用一个接口可以让我们做到这一点。

我们在 for 循环内部调用`serve`函数，它接受一个`cook`。这通常是行不通的——我们需要定义两个`serve`函数，一个用于`chef`类型，一个用于`homeCook`类型。然而，使用接口有助于我们避免这种重复。

当你的代码库很小的时候，很难理解接口。小项目不需要太多的结构，所以你可以不使用结构。然而，随着项目的增长，接口允许真正干净、可预测的代码。

# 它是如何在引擎盖下工作的？

界面不仅在设计代码时非常有用，而且在实现时也非常酷。接口可以描述为两个相连的指针块:一个指向类型定义，另一个指向其基础值。很困惑，对吧？看看上面例子中的这一行。

```
chef1 := chef{"Brian", "Korean"}
```

`chef1`实现了`cook`接口。如果我们以接口形式剖析`chef1`，第一个指针会指向`chef`结构体的类型定义，第二个指针会指向`chef1`的实际值。

# 什么是空接口？

现在我们可以考虑一个更高级，或者更简单的概念:空接口。我们在上面说过，接口通过特定的行为对对象进行分组。空界面会是什么样子？它没有任何类型可以实现的方法。这意味着任何类型的对象都可以实现接口。这就像把活着的人类归为一个类别，叫做“有机体”。年龄、身高、种族和性别并不重要——人类都是有机体。

看一下这个片段:

```
func main() {
    a := "hello"
    b := 100
    c := 3.14 objects := []interface{}{a, b, c}
}
```

这段代码可以编译，因为`objects`是一个存储任何实现空接口的对象的片。

# 技巧

在这篇文章的最后，我想和你分享我在短暂的围棋发展历程中积累的一些技巧。

*   请记住，虽然空接口提供了很大的灵活性，但是您有责任关注不同的可能类型。例如，假设您正在解组一个 JSON 对象。有时，您不知道传入的 JSON 的确切结构。Go 将巧妙地使用类型`map[string]interface{}`来解组 JSON 对象。JSON 键将被存储为`string`，值将被存储为`interface{}`。当操作这个值时，你必须考虑每一个可能的类型，否则你的代码将会失败。这是您在使用空接口时必须缴纳的税款。
*   一致地命名你的接口。当我把我的接口命名为`--er`时，它帮了我很大的忙。比如`copier`、`reader`、`parser`等。标准库中有一个明显的例外，那就是`builtin.Error`接口。我想这是可以的，因为它仍然与其他以`--er`结尾的接口押韵。
*   将接口和结构/结构方法定义保存在单独的文件中有助于提高可读性。这不是每个人都喜欢的，但我发现这在很多情况下都有帮助。
*   你不必重新发明轮子。如果你需要一个接口，看看标准库是否已经提供了。您可以实现的一些最常见的方法有`fmt.Stringer`、`io.Reader`、`io.Writer`、`builtin.Error`、`http.ResponseWriter`、`sort.Interface`等。
*   界面是一把强有力的锤子，但不是每个问题都是钉子。你不需要所有的东西都有一个接口。有时创建一个这样的系统的开销和努力是不值得的。试着在没有界面的情况下构建你的应用，你会遇到一种情况，你希望有一个界面。

# 结论

我希望这篇文章能帮助你澄清一些关于 Go 接口的不确定性。接口是 Go 编程语言必不可少的一部分，在你的旅途中你无疑会碰到它们。到时候，我相信你能处理好。继续努力，下周我会带着新的帖子来看你。

这篇文章也可以在 [Dev.to](https://dev.to/jpoly1219/interfaces-in-go-169i) 和 [my personal site](https://jpoly1219.github.io) 上看到。