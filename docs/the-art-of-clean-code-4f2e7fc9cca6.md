# (干净)代码的艺术

> 原文：<https://blog.devgenius.io/the-art-of-clean-code-4f2e7fc9cca6?source=collection_archive---------6----------------------->

![](img/edfd8890a2f63546d935ec9d4663eeb7.png)

照片由[克莱门特·H](https://unsplash.com/@clemhlrdt?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)在 [Unsplash](https://unsplash.com/s/photos/code?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 上拍摄

我和我的同事最近就**清洁代码**进行了一次颇有见地的讨论。虽然这个主题的根源仍然是技术，但它有哲学的本质，不同的开发人员有不同的主观看法。尽管标题崇高，但我绝不是在宣称这篇文章的内容是上帝的话。我只是试图传递一些我从导师和我自己的经历中收集到的见解。

最简单的干净代码是既有意义又好看的代码。对我来说，有意义的代码是所有伟大架构模式的根源。这些模式都可以归结为一个原则——一段代码应该只做它应该做的事情。好看的代码就是 ***易读的代码*** 。

让我们来看看一些可以帮助你保持代码整洁的东西。

## **保持你的代码格式良好**

大多数 ide 会给你一个工具和快捷方式来自动格式化你的文件。用它！在 Visual Studio 上，快捷方式是 Ctrl+K+D。我不能夸大在现有文件上使用 Ctrl+K+D 而看不到任何更改的喜悦。格式化通过确保所有的缩进都是正确的，并且所有的括号都在正确的位置来保持代码的可读性。避免代码行之间出现多行空白。避免没有任何线间隙。我建议在逻辑上不同的语句之间留一行空白。

专业提示:避免尾随空白。如果你使用的是 Visual Studio，进入工具->选项->文本编辑器->常规，选中选项*查看空白*。启用此选项后，请清理尾随空格。自动格式化也会清除尾随空格。

## 使用有意义的名称

你的代码可以也应该讲述一个故事。名字很重要。尽可能使用赋予变量意义的名称。如果你的类被恰当地命名，用你正在实例化的类来命名你的变量通常是完全可以接受的。对于简单类型，如 int、bool 或 string，请使用明确定义该变量用途的名称。

```
// bad names
var a = 100;
var x = "Administrator";
var y = false;// good names
var maxScore = 100;
var adminRole = "Administrator";
var hasAccess = false;
```

## 遵循编码标准

尽管有“标准”这个词，但您会发现这些标准会随着组织的不同而变化，并且会随着时间的推移而变化。找到贵公司采用的标准。坚持下去。如果没有标准，就去争取。这里有一篇文章定义了一些我认为适合 C#开发的标准。

## 利用语言

跟上您选择的编程语言提供的提示和技巧。使用 ternaries，在可用和适当的地方使用空条件和空合并操作符。话虽如此，请确保您的结果代码不会损害可读性。

```
// using if-else
var firstName = String.Empty;
var lastName = String.Empty;if (person != null) 
{
  firstName = person.FirstName;
  lastName = person.LastName;
}
else
{
  firstName = "Unknown";
  lastName = "Person";
}// using null-conditional and null-coalescing operators
var firstName = person?.FirstName ?? "Unknown";
var lastName = person?.LastName ?? "Person";
```

## 对重复代码使用方法

当我看到同一行代码重复不止一次时，我怀疑它是否可以移到一个方法中。可重用性是一个关键原则，当你处理需要改变的大特性时，你会明白这一点。当变化不可避免地到来时，您希望尽可能少地修改。在方法中保留重复的代码允许您只更改那些方法，而不影响其余的方法。你的代码很珍贵。不要写多余的代码。

## 避免评论

这是有争议的，虽然我不明白为什么。如果你的代码有意义，你可以避免写注释来描述你在做什么。您可以将这些详细的描述保存到您的拉取请求和文档中。当你重构代码时，不要把前面的代码留在注释中。自信一点，摆脱那些死代码。你害怕抹掉另一个开发者的工作吗？这就是我们进行版本控制的目的。[当童子军](https://jasonmccreary.me/articles/are-you-a-boy-scout/)。

## 给你的代码添加语义

我重视语义和句法的准确性。让我举个例子来说明我的意思。假设您有一个汽车数据集，您想使用主键 CarId 来查找一辆汽车。你会在这里使用 single 或 Default 还是 FirstOrDefault？不要在这里使用 FirstOrDefault，即使它会给你正确的结果。通过使用 SingleOrDefault，您告诉读者，对于该查询，您最多只能获得一辆汽车，这在语义上是正确的，因为在查询中使用主键应该只能得到一个结果。当使用 SingleOrDefault/FirstOrDefault 或任何可能返回空值的方法时，不要忘记添加一个空值检查。引入一个没有被设置为对象实例的*对象引用*异常应该会让你流泪。

这是另一个例子。如果不在方法内部进行异步操作，就不要将方法标记为 async。您的代码将会运行得非常好，但是它会使读者对您将该方法标记为 async 的意图感到困惑。

## 了解原因

听着，我们都这么做。我们都是从栈溢出，从 C#角，从同一个项目的其他文件中拷贝代码。没关系，但是试着花点时间去理解为什么代码是那样写的，当你理解了，试着写得更好。理解为什么这个类从另一个类继承。它真的使用了基类的方法吗？提问。进行辩论。寻找新的视角。

试着让写干净的代码成为一种习惯。不要成为先“完成任务”然后回来清理的开发人员。痴迷于干净的代码。