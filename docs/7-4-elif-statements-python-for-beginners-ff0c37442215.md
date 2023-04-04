# # 7.4 elif-语句— Python 初学者

> 原文：<https://blog.devgenius.io/7-4-elif-statements-python-for-beginners-ff0c37442215?source=collection_archive---------8----------------------->

## 另一个如果

然而，我们只能使用 if 和 else。所以我们可以检查一个条件。如果为真，我们运行 if-code 块，如果不是 else-code 块，不管其他条件如何。现在我们还想检查其他条件。为此，我们使用 elif 语句，我将在本文中向您解释。

![](img/13b4be6b2c9c0313025b19a7ac98acbd.png)

照片由[简·kopřiva](https://www.pexels.com/@koprivakart?utm_content=attributionCopyText&utm_medium=referral&utm_source=pexels)从[派克斯](https://www.pexels.com/photo/photo-of-a-red-snake-3280908/?utm_content=attributionCopyText&utm_medium=referral&utm_source=pexels)拍摄

## elif 语句

elif 语句由单词 *if* 和*else*(*else if*=*elif*)混合而成。从它的名字你就能猜到它要做什么。没错。这就像一个带有 if 语句行为的 else 语句。用在 if 之后，else 之前。语法由关键字 *elif* 、条件(评估为真或假)、冒号和缩进代码块组成。像这样:

```
age = 20
if age < 21: # Is True
  print('You are young!') # Gets printed out
elif age < 50: # No check because if-statement is already True (however, is also True)
  print('You are normal age!')
else: # if-statement is already True, so no else
  print('You are old!')
```

我们得到了输出*你还年轻！。*if 语句被评估为真。此外，我们可以有多个 elif 语句，如果我们愿意，我们可以检查 10 个以上的条件。需要记住的一件重要事情是，if 语句或 elif 语句中只有一个在被求值为 True 时运行。因此，当一个 if 语句或 elif 语句被评估为真时，其中的代码块将被运行，而其后的其他 elif 语句(以及 else 语句)将被跳过。为了更清楚起见，我们稍微改变一下上面的例子:

```
age = 20
if age < 50: # Is True
  print('You are normal age!') # Gets printed out
elif age < 21: # No check because if-statement is already True (however, is also True)
  print('You are young!') # We would expect this to get printed out, but it was not...
else: # if-statement is already True, so no else
  print('You are old!')
```

我们希望你还年轻！像上次一样打印出来，因为 20 岁是个年轻的年龄。但是*你是正常年龄！*打印出来。从逻辑上来看我们的程序。20 比 50 小。当这被评估为真时，我们从这个代码块获得输出，并且跳过另一个 elif 语句和 else 语句。现在，如果你想让它以正确的方式进行，你可以选择第一个选项(第一个例子)或者这样做(我推荐这样做，它让其他开发者和你未来的自己更清楚):

```
age = 20
if age > 20 and age < 50: # Is False as age is smaller than 20 (and not greater)
  print('You are normal age!')
elif age < 21: # True and checked as if-statement above is False
  print('You are young!') # We get the expected output
else: # elif-statement is already True, so no else
  print('You are old!')
```

现在，正如我们所料，我们得到了结果:你还年轻！。精彩！如果我们现在将可变年龄改为 30，我们将得到*你是正常年龄！*。一切正常！

## 要记住什么

请记住，在 if-elif-statement 块中，最多有一条语句被求值为 True，并且其中的代码块将被运行。下面的 elif 语句将被跳过。要完成 if-elif 语句块，请使用 else 语句，以便在没有结果为 True 时有一个“默认”情况。

现在我们有了完整的 if-elif-else 语句块。现在，您可以检查代码中的各种条件，并在给定条件为真时决定运行代码。 *下一篇文章我们将创建一个示例程序来使用学到的东西！*

和往常一样，如果你有任何关于 Python 或编码的问题，请在下面的评论中提出。

**直到那时！**

*l0ckD2wN*