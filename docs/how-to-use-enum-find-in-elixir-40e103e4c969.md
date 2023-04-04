# 如何在 Elixir 中使用 Enum.find

> 原文：<https://blog.devgenius.io/how-to-use-enum-find-in-elixir-40e103e4c969?source=collection_archive---------8----------------------->

在 Elixir 的集合中查找值的最佳方法是什么？如果您习惯于 C 或 JavaScript 等非函数式语言，您可能会考虑使用 for 循环。但是 Elixir 的功能性质意味着我们必须采取不同的方法:函数 *Enum.find* 。

![](img/5cf2e5c6484565eef712051f065c9bed.png)

由[马库斯·温克勒](https://unsplash.com/@markuswinkler?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄

函数 *Enum.find* (以及它的朋友 *Enum.find_index* 和 *Enum.find_value* )适用于任何实现了 [*可枚举*](https://hexdocs.pm/elixir/1.14.1/Enumerable.html) 协议的类型，最显著的是 [*List*](https://hexdocs.pm/elixir/1.14.1/List.html) 、 [*Map*](https://hexdocs.pm/elixir/1.14.1/Map.html) 和 [*Range*](https://hexdocs.pm/elixir/1.14.1/Range.html) *。对于本文中的例子，我们将坚持使用简单的列表。*

# 寻找简单的值

为了在可枚举的中找到一个值，我们使用可枚举的数据和一个查找函数调用 *Enum.find* :

```
e = [5, 10, 15]
Enum.find(e, fn v -> v == 10 end)=> 10
```

如果找不到我们要找的值，我们将得到 *nil* :

```
e = [5, 10, 15]
Enum.find(e, &(&1 == 11))=> nil
```

# 查找索引:Enum.find_index

如果我们感兴趣的是*一个条目出现在可枚举的什么位置*，我们可以使用 *Enum.find_index:*

```
e = [5, 10, 15]
Enum.find_index(e, fn v -> v == 10 end)=> 1
```

# 使用地图查找

当我们用一个*映射调用 *Enum.find* 时，*我们得到一个元组，其中包含一个键和一个传递给我们的 finder 函数的值:

```
e = %{a: 5, b: 10, c: 15}
Enum.find(e, fn {k, v} -> k == :b end)=> {:b, 10}
```

(旁注，因为我们没有使用元组中的值 *v* ，我们可能会得到一个未使用的变量警告。我们真的应该把它命名为 *_v* ，但是为了便于说明，我们把它放在一边。

返回的值当然也是包含键和值的元组。这可能不是你想要发生的，但是下一节关于 *Enum.find_value* 的内容将帮助我们解决这个问题。

# 查找和转换值:Enum.find_value

在前面的例子中，我们得到了一个表示键和值的元组，但是如果我们实际上只想要元组中的一个元素呢？我们可以使用 *Enum.find_value* :

```
e = %{a: 5, b: 10, c: 15}
Enum.find_value(e, fn {k, v} -> if k == :b, do: v, else: nil end)=> v
```

# 使用捕获运算符(&)查找

在我们到目前为止使用的所有示例中，我们都是这样编写测试函数的:

```
fn v -> some_test(v) end
```

但是我们可以用捕捉操作符(&)来简化。我们可以这样重写上面的函数:

```
&some_test(&1)
```

这里，函数参数被捕获为 *& 1、& 2、*等，尽管和*enum . find*finder 函数只接受一个参数。所以改写一下我们之前的一个例子:

```
e = [5, 10, 15]
Enum.find(e, &(&1 == 10))=> 10
```

# 额外收获:如何从头开始构建 Find

我们一开始提到一些语言会使用 for 循环来实现这个功能，我们应该使用 *Enum.find* 来代替。但是我们不想让人觉得 *Enum.find* 是某种神奇的黑盒。让我们看看我们如何在 Elixir 中实现这个功能。

我们下面的实现使用了一种常见的函数模式*递归*，而不是一个在*列表中查找值的循环。*为了简单起见，我们将忽略本例中的其他*枚举数*。

它还使用了另一种常见的灵丹妙药习语，即函数输入上的*模式匹配*:如果列表为空，则答案始终为零。否则，我们对列表的第一项(v)和列表的其余部分进行模式匹配。如果对值 *v* 的测试匹配，我们就找到了答案——否则，我们递归调用:

```
defmodule ListUtil do
  def find([], _), do
    nil
  end def find([v | rest], f) do
    # invoke the test function on the first value in the list
    if f.(v),  do: v, else: find(rest, f)
  end
end# test our implementation
ListUtil.find([1,2,3], fn i -> i == 2 end)
```

就是这样！虽然大多数时候您可以使用 *Enum.find* 做您需要做的事情，但是如果需要迭代其他类型的数据结构，了解如何在 Elixir 中处理这类问题是很好的。

*原载于*[*Blixtdev*](https://blixtdev.com/how-to-use-enum-find-in-elixir/)。

Jonathan 在大型和小型创业公司中拥有超过 20 年的工程领导经验。如果你喜欢这篇文章，请考虑加入 Medium 来支持 [*乔纳森和其他成千上万的作者*](https://medium.com/@jonnystartup/membership) *。*