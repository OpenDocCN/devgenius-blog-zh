# 用 Pydantic 反序列化子类，有用吗？！

> 原文：<https://blog.devgenius.io/deserialize-child-classes-with-pydantic-that-gonna-work-784230e1cf83?source=collection_archive---------2----------------------->

用 Pydantic 反序列化子类的问题，以及如何解决。

![](img/2b24299bf62fb201b66e32f0901fefb0.png)

Pydantic 是一个很棒的包，用于在 Python 中序列化和反序列化数据类。
当我们试图使用内置的 JSON 库进行反序列化时，它不会像预期的那样处理类。Python 是一种动态类型语言，因此不支持指定加载到什么类型。在 Pydantic 中，每个属性的类的提示类型以及序列化和反序列化的魔力将开箱即用。(欲了解更多信息，请阅读[文章](https://motcke.medium.com/is-c-superior-to-python-in-serializing-data-5c122dfbd57c))

但是，当处理类继承时，Pydatic 将无法反序列化对象。让我们看看:

我们暗示`ducks`是一个`list[DuckBase]`——这意味着`ducks`可以包含`DuckBase`的任何子类。序列化`disney_movie`时，一切都将按预期进行:

然而，当将这些原始数据反序列化回它的类时，我们在 python 对象中只得到部分数据——所有的`ducks`都被解析为`DuckBase`而不是正确的类！

我的课在哪里？！我要他们回来！

# **为什么解析成正确的类如此重要？**

在 pydantic 中，当加载特定类的数据时，有一些强大的实用程序。例如:类型注释、默认值、数据无效时的用户友好错误、验证器等。

当加载到错误的类时，会丢失那些强实用程序。例如，我们可以添加一个验证器来验证`DaisyDuck`中的`voice_by_years`值必须是 1945 年到 2022 年之间的一组年份。当我们加载到`DuckBase`类时，这个验证将不会运行。

让我们看看这个有问题的场景:

当我们试图用错误的`voice_by_years`反序列化一个`DaisyDuck`到`DaisyDuck`类时。我们不会得到预期的`ValueError`:

然而，当我们试图用错误的`voice_by_years`反序列化一个`DaisyDuck`到`DuckBase`类而不是`DaisyDuck`时。我们不会得到预期的异常:

# **提示可能的子类**

于是我找到了一个解决办法，不提示`ducks`为`list[DuckBase]`，我会提示`ducks`为`list[DonaldDuck|DaisyDuck]`

让我们看看 JSON 会是什么样子:

哦！突然我看到`DaisyDuck`物体有一个额外的场——`nick_name`。我不想要。为什么会这样？

Pydantic 看到鸭子中的每只鸭子都有两个选项作为其类型。它试图序列化为第一种类型`DonaldDuck`。属性`nick_name`在`DaisyDuck’s`对象中缺失。但是，有一个缺省值 None，所以 pydantic 解析为带有`nick_name = None`的`DonaldDuck` JSON。

如果我们将改变`ducks`类型列表的顺序(首先是`DaisyDuck`，然后是`DonaldDuck`)，那么在序列化 JSON 时，将包括来自`DaisyDuck`的所有属性的默认值。

***一定有更好的办法！*** *不包括额外属性。*

让我们删除默认值，看看 JSON:

哦！现在我已经有了我想要的字段，**，没有了额外的默认值字段**:)

当我们反序列化这些原始数据时:

根据 JSON 中的原始数据将`ducks`的每个 duck 解析为正确的子类。

但是我想保留我的默认值。 ***一定有更好的办法！***

# **注释元数据字段提示**

让我们将新类型的`Duck`声明为`[DonaldDuck, DaisyDuck]`，并为每个子类添加一个名为`duck_type`的元数据属性，该属性将描述 duck 类型(`‘donald’` / `’daisy’`)。并使用这种类型的`Duck`作为鸭子列表的类型。让我们来看看:

在 JSON 中，我们可以看到`duck_type`元数据也被保存了:

让我们反序列化这些原始数据:

耶，一切都像预期的那样解析！

但是当我们将添加一个新的`DuckBase`的孩子时，我们将需要处理类型`Duck`，我不想每次都更新这个类型。 ***一定有更好的办法！*** 我能做什么？

# **元数据字段动态提示**

我可以在`DuckBase`类中实现`__**init_subclass__**`函数。这个函数将在创建`DuckBase`的子类时调用。在这个函数中，我将在`subclass_registry`中自动保存 Duck 类的所有子类。

现在，当初始化`DuckMovie`类时，`**__init__**`函数将:

1.  通过属性`duck_type`检查每只鸭子的类型。
2.  在`subclass_registry`中寻找与`duck_type`相同的子类。
3.  用鸭子的原始数据创建匹配子类的实例:

我们来看看迪士尼 _ 电影的 JSON:

在反序列化的结果中:

太棒了！我有正确的课程。

但是，我不想存储每个孩子的类的元数据！
***一定有更好的办法！***

# **通过属性名称动态提示**

我可以通过查看类中存在哪些属性来了解类的类型，而不是将元数据存储在原始数据中。

> 如果它看起来像只鸭子，游泳像只鸭子，叫声像只鸭子，那么它很可能是只鸭子。

让我们在将鸭子解析到正确的类时使用鸭子类型的概念！当初始化`DuckMovie`类时，`**__init__**`函数将:

1.  检查每只鸭子的属性名称。
2.  在`subclass_registry`中寻找具有相同属性名称的子类。
3.  用鸭子的原始数据创建匹配子类的实例:

让我们来看看 JSON:

太好了！**没有多余字段**！

反序列化的结果看起来如何？

精彩！！！

**结论:**

我们看到 Pydantic 没有将子类反序列化为正确的类。在本文中，我们试图帮助 Pydantic 理解反序列化到哪个孩子的类中。

首先，我们通过暗示所有可能的子类来做到这一点(使用`Union`)。如果我们不需要默认值，这可能会很棒。然而，当我们使用默认值时，Pydantic 从第一个可能的子类中序列化实例的额外默认值。

其次，我们向每个子类添加了子类类型的元数据，这些元数据也被保存到 JSON 中。这样 Pydantic 就能明白哪个子类被反序列化了。然而，我们不想将元数据保存到 JSON 中。

最后，我们找到了告诉 Pydantic 如何确定子类类型的方法，方法是将现有的 JSON 属性与子类属性进行匹配。