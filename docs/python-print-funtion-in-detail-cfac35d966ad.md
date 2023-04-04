# Python Print()函数的细节

> 原文：<https://blog.devgenius.io/python-print-funtion-in-detail-cfac35d966ad?source=collection_archive---------1----------------------->

所有程序员都通过在控制台上打印结果来测试他们的代码输出，分析一段代码、函数、类、脚本的结果是很重要的。所有编程语言都提供了一些特殊的神奇功能来检查输出，这有助于人们理解代码块的性质。我们来说说 python 的打印函数。我将主要谈论 python 3+版本。

# 你知道 python 是如何处理函数的吗？

让我们调用一个函数，并尝试分析 python 是如何执行的。

```
def say_hello():
    print("Hello")

say_hello()
```

1.  Python 将检查 ***say_hello*** 是否合法，它将在内部搜索同名的现有函数，如果失败，它将中止并抛出一个错误。
2.  现在，它将检查所需参数的数量(例如，一个函数需要两个参数，在这种情况下只提供一个参数，它将中止执行并抛出一个错误)。
3.  现在 Python 跳转到带有所需参数的函数 ***say_hello()*** 中，并执行代码，得到想要的结果。
4.  它返回代码(在函数调用处)。

同样的事情也发生在 ***print()*** 函数上。print()函数获取其参数，并在需要时将其转换为人类可读的格式，然后**将结果数据发送到输出设备**(控制台)。

# print()接受的参数个数？

print 函数接受任意数量的参数。它可以小于 1。

```
print("Hello World")
print()
```

任何类型的数据都可以通过 print()函数传递，它可以是字符串、数字、字符、逻辑值、对象。

```
print("Hello",2,"P")
```

**`print()`**函数返回什么值？****

**它不返回任何值**

*****print()*** 每次开始执行时，从新的一行开始输出。指令以相同的顺序执行。**

```
print("Hello",2,"P")
```

**以上打印将打印`Hello`然后`2`然后`P`顺序不变。**

****Empty print()输出的是空行。****

**\ '，在字符串内部使用时，称为**转义**字符**字符**它表示字符串中的一系列字符暂时转义以引入特殊包含。如果你想在一个字符串中加上反斜线，你需要加上两个反斜线。**

```
print("Hello \\ there")Output - Hello \ there
```

**print()函数调用多个参数，并在一行中输出它们。**

```
print("This","is","python","print","function")
Output - This is python print function
```

**`print()`函数**主动在输出的参数**之间放置一个空格。**

**要在**新行中显示输出，您可以使用\n****

```
print("this is python print \n function")Output - this is python print 
 function
```

**对于多行，可以使用**三个单引号或双引号****

```
print('''This is
python 
print
function''')

print("""This 
is 
python
 print
  function""")
```

****注意-p*rint()****既接受**位置**也接受**关键字参数**。***

```
*print("Hello",2,"P")*
```

***上面的打印是位置参数的示例，其中每个参数将以相同的顺序打印。***

*****打印关键字参数*****

***print 函数接受两个关键字参数。***

1.  *****结束*****
2.  *****九月*****
3.  *****冲水*****
4.  *****文件*****

******end*** 关键字显示了它在参数结束时的行为。***

```
*print("this","is","print","function",end=" ")Output - this is print function* 
```

***看到上面的输出中没有**换行**。***

***默认行为反映了`end`关键字参数为**隐式**的情况，使用方式如下:`end="\n"`。***

***我们已经看到 print with multiple arguments 将输出用空格分隔的所有参数，这种行为是由于关键字 argument ***sep*** 造成的。***

***默认行为反映了`sep`关键字参数为**的情况，以如下方式隐式使用**:`sep=" "`。我们可以改变这种行为。***

```
*print("this","is","python","print","function",sep='-')Output - this-is-python-print-function*
```

******sep 关键字*** 将自变量与其值分开。它的值可以是空字符串。***

```
*print("this","is","python","print","function",sep="")Output - thisispythonprintfunction*
```

******flush 关键字*** 用于停止打印功能缓冲数据。如果 *flush* 参数设置为 True，函数将不会缓冲数据以提高效率，并将在每次调用时刷新一次。***

***来自`print()`的输出进入缓冲区。当您更改`end`参数时，缓冲器不再刷新。为了确保一调用`print()`就得到输出，还需要使用`flush=True`参数。***

```
*print("hello",flush=True)*
```

******文件*** 参数指定打印函数应将给定对象写入何处。默认情况下，它写入 *sys.stdout****

```
*import sys

print("This is python print function", file=sys.stderr)*
```

*****在特定文件中打印*****

```
*my_file = open("/home/Desktop/code.txt","w")
print("This is python print function", file=my_file)
my_file.close()*
```

***这就是 Python 打印函数。如果我错过了什么，请在评论中告诉我。***

***谢谢！对于阅读，希望会有帮助。***