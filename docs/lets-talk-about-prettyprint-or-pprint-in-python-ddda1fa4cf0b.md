# 我们来谈谈 python 中的“prettyprint”或 pprint

> 原文：<https://blog.devgenius.io/lets-talk-about-prettyprint-or-pprint-in-python-ddda1fa4cf0b?source=collection_archive---------5----------------------->

![](img/0956b97d7a1eca693a4efc07d553e00e.png)

照片由[希特什·乔杜里](https://unsplash.com/@hiteshchoudhary?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 拍摄

**开始学习 python 编程，你做过的第一件事是什么？**
我很确定我们都是从“ **Hello World** ”程序开始我们的 python 之旅的。在 python 中，这可以在一行代码中完成:

`print(“Hello World”)`

但是，在使用 ***print()*** 函数打印字典、列表或任何其他复杂数据类型时，您是否曾经面临过这种痛苦？由于不正确的缩进问题，我们经常在 python 嵌套数据结构的输出中遇到可读性方面的困难。

**还是没明白我在说哪个问题？**
让我们在这里试试代码:

```
 coordinates = [
 {
 “name”: “Location 1”,
 “gps”: (29.008966, 111.573724)
 },
 {
 “name”: “Location 2”,
 “gps”: (40.1632626, 44.2935926)
 },
 {
 “name”: “Location 3”,
 “gps”: (29.476705, 121.869339)
 }
]
print(coordinates)
```

上面代码的输出:

```
[{‘gps’: (29.008966, 111.573724), ‘name’: ‘Location 1’}, {‘gps’: (40.1632626, 44.2935926), ‘name’: ‘Location 2’}, {‘gps’: (29.476705, 121.869339), ‘name’: ‘Location 3’}]
```

我们可以看到，上面的代码不具有视觉可读性和美感。

难怪 python 被认为是对用户最友好的语言！

**怎么琢磨？**

![](img/100b4dedd727f0344ea0a4db134e31e8.png)

这真的可能吗？

Python 附带了一个 **pprint** 模块，它以一种更可读的方式提供了**漂亮打印**任意数据结构的能力。

# 什么是 pprint？

python 中的 pprint 模块负责以适当的格式打印行块，以吸引读者。它使用换行符和缩进以明确的方式打印数据。

# pprint 和 python 中的 print 有什么不同？

print()是 python 中的一个简单函数，用于在屏幕上向用户显示指定的消息。但是，如果我们使用 python 打印字典、列表或任何其他复杂的函数，我们经常会发现阅读打印出来的语句有歧义。它包括不可表示为 Python 常量的内置对象、文件、套接字、类或实例。
**“pprint”模块随后就来救你了。**

![](img/e0fc613d01810d91b0d2151e34db14b5.png)

“pprint”，救援者

它以可读的形式格式化对象，每行有适当的宽度。它带有可调整的宽度限制，以便于用户使用。它将所有元素转换成可以用 Python 常量表示的字符串，并以视觉上美观的格式打印出来。pprint 函数返回可以在解释器中作为输入运行的输出。而且，通过解析字符串，更容易将其转换回类型。

***那么，让我们潜入 pprint…***

## 在 python 文件的顶部导入库 pprint

```
import pprint
```

现在，我们可以使用 ***。pprint()*** 对象或者实例化我们自己的 pprint 对象 ***PrettyPrinter()*** 。

```
pprint.pprint(['Radha', 1, 'Hari', 'Simesh', 25, 847])# Instantiating pprint object
my_pprint = pprint.PrettyPrinter()
my_pprint.pprint(['Radha', 1, 'Hari', 'Simesh', 25, 847])
```

两者将给出相同的输出

```
['Radha', 1, 'Hari', 'Simesh', 25, 847] 
['Radha', 1, 'Hari', 'Simesh', 25, 847]
```

## 那么，pprint()和 PrettyPrinter()有什么区别呢？

如果我们需要调整宽度约束或其他参数，我们显式地构造`[PrettyPrinter](https://docs.python.org/3/library/pprint.html#pprint.PrettyPrinter)`对象。

**语法:**

```
*class* pprint.PrettyPrinter(*indent=1*, *width=80*, *depth=None*, *stream=None*, ***, *compact=False*, *sort_dicts=True*)
```

pprint()方法使用库的默认设置，而在创建 PrettyPrinter()对象时，我们可以更改库的默认配置。

让我们用几个例子来理解:

1.  深度参数决定了 Python PrettyPrinter 在嵌套结构中向下递归的程度

```
tuple1 = ('spam', ('eggs', ('lumberjack', ('knights', ('ni', ('dead',('parrot', ('fresh fruit',))))))))# Using PrettyPrinter
pp = pprint.PrettyPrinter(depth=6) # default configuration of depth # being none is changed to depth = 6
# Now it will print till depth of six brackets
pp.pprint(tuple1)#Using only pprint() object
pprint.pprint(pprint.pprint(tuple1, depth=6))
pprint.pprint(tuple1)
```

上面代码的输出:

```
('spam', ('eggs', ('lumberjack', ('knights', ('ni', ('dead', (...))))))) 
('spam', ('eggs', ('lumberjack', ('knights', ('ni', ('dead', (...)))))))
('spam',
  ('eggs',
   ('lumberjack', ('knights', ('ni', ('dead', ('parrot', ('fresh fruit',))))))))
```

你能注意到不同吗？

我们看到，当使用 depth = 6 打印 tuple1 时，在六个省略号之后不再显示数据，而当只使用 pprint.pprint()打印时，所有数据都显示出来。

设置不存储在 ***中。pprint()*** ，即默认设置保持不变，而在 PrettyPrinter()中保存设置或更改。这里存储深度= 6。

2.使用宽度参数，我们可以选择输出将打印多少列。默认宽度为 80，即 80 个字符，但您可以更改。

现在，让我们看看在几个内置函数中使用 pprint()比 print()函数的主要区别和好处。

```
from pprint import pprint # We can directly call the method 
# pprint() using it
coordinates = [
 {
 “name”: “Location 1”,
 “gps”: (29.008966, 111.573724)
 },
 {
 “name”: “Location 2”,
 “gps”: (40.1632626, 44.2935926)
 },
 {
 “name”: “Location 3”,
 “gps”: (29.476705, 121.869339)
 }
]
pprint(coordinates)
```

以上输出的输出:

```
[{'gps': (29.008966, 111.573724), 'name': 'Location 1'},
 {'gps': (40.1632626, 44.2935926), 'name': 'Location 2'},
 {'gps': (29.476705, 121.869339), 'name': 'Location 3'}]
```

现在，参考本文开头使用 print()的第一个示例。哪个输出在视觉上更吸引人？使用 ***print()*** 还是 ***pprint()*** ？

这就是本文中关于 python 中 pprint 的全部内容。关注我，获取更多关于 python 和机器学习的文章。

**加分:**python 中还有一个模块叫 pprintpp，用于漂亮打印也叫 pprint++。

***快乐学习，快乐编码！***