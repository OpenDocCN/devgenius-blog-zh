# Bash 脚本和 Shell 编程基础

> 原文：<https://blog.devgenius.io/bash-scripting-and-shell-programming-basics-9837afb3cfde?source=collection_archive---------11----------------------->

![](img/c6e433fb0038eedf72d20a595827cf93.png)

你有没有浏览过 bash 脚本、shell 编程、bash 文件、shebang 之类的术语，或者听过有人谈论如何用 bash 编写代码，它甚至是一种编程语言吗？或者看到了奇怪的文件。sh 扩展，从来没有费心去研究他们？你知道这些很重要，但是如何使用它们呢？让我们试着深入了解它到底是什么，以及如何真正利用它！

# 外壳脚本简介

## 什么是剧本？

脚本是包含一系列命令的命令行程序，这些命令由解释器一个接一个地执行，对于 shell 脚本，shell 充当解释器。

您在终端(命令行)中运行的任何东西都可以放在 shell 脚本中，shell 脚本在自动化任务方面非常出色。如果您发现自己正在运行一系列命令来实现某些目标，那么将它们放在 shell 脚本中并每次都运行该脚本而不是所有这些命令是一个好主意。

让我们来看一个非常基本的 shell 脚本示例:

```
#!/bin/bash
echo “Hello World!”
```

以及如何运行这个脚本？

```
$ chmod 755 script.sh
$ ./script.sh
Hello World!
```

# 记住，每个 shell 脚本都以 shebang 开头。

## “舍邦”是什么鬼东西？

在上面的脚本示例中，第一行类似于 ***#！/bin/bash*** ，口头上你会说是***sharp-bang-forward slash-bin-forward slash***(有人参考！砰的一声。)于是，“ **#！**可以说是**锐棒**，已经慢慢变成了**射棒**。对，就是这样。

*注意:对于除 shebang 以外的每一行，如果它以井号(#)开头，则被视为注释。*

当脚本的第一行以 shebang 开头时，shebang 后面的代码将被用作脚本中列出的命令的解释器。上例中的“/bin/bash”。其他例子可以是:“bin/csh”、“bin/ksh/”、“bin/zsh”。

你听说过 python 是一种脚本语言吗？是的，你也可以用 python 作为解释器来运行脚本。看看下面的例子:

```
#!/bin/python
print "Hello World, Using a python script."
```

并运行它:

```
$ chmod 755 hello.py
$ ./hello.py
Hello World, Using a python script.
```

# 让我们掌握它的窍门

让我们做点什么，而不仅仅是印刷 Hello World！并在这里应用一些基本的编程概念，如将值存储到变量中等等。

## 为变量设置值并使用这些值

定义变量的规则:

*   shell 脚本中的变量区分大小写。
*   按照惯例，变量是大写的。
*   请记住，不要在=符号前后留任何空格。
*   变量名可以包含数字、字母和下划线。
*   一个很好的例子是:VARIABLE_NAME="some value "

```
#!/bin/bash
FAV_FRUIT="Apple"
echo "My garden is full of $FAV_FRUIT trees."
```

您也可以将变量值作为$ { FAV _ 水果}来传递，而不是$ FAV _ 水果。如果您想要连接两个字符串，您可能想要使用花括号语法，例如，如果您想要打印“I Love Apple **s** ”，那么您不能传递结尾带有“s”的值。您可以按如下方式传递该值:

```
echo "I Love ${FAV_FRUIT}s" 
```

## 算术运算符

比较两个数字之类的算术运算不是使用普通的运算符来完成的，比如等号、大于号或小于号，它在 shell 脚本中有一个特殊的语法。请看下面:

```
arg1 -eq arg2 True if arg1 is equal to arg2.
arg1 -ne arg2 True if arg1 is not equal to arg2.arg1 -lt arg2 True if arg1 is less than arg2.
arg1 -le arg2 True if arg1 is less than or equal to arg2.arg1 -gt arg2 True if arg1 is greater than arg2.
arg1 -ge arg2 True if arg1 is greater than or equal to arg2.
```

## 做决策——if 语句

现在，您对如何检查条件有了基本的了解，如果满足某个条件，您可以将结果与 if 语句结合起来运行一个命令(或一组命令)。

if 语句的语法如下:

```
if [condition-is-met]
then
  command 1
  command 2
  command N
fi
```

if 语句以单词“if”开头，后跟一个测试，下面一行包含单词“then”。接下来是一个命令，或者一系列的命令，如果满足条件，这些命令将被执行。最后，if 条件以单词“fi”结束，它只是“if”的倒拼。

您也可以将 else if 和 else 命令放在 if 命令之后，如下所示:

```
if [condition-is-met]
then
  command 1
  command 2
  command N
elif [condition-is-met]
then
  command X
else
  command Y
fi
```

## For 循环

您可以使用 for 循环对项目列表执行操作，shell 脚本中 for 循环的语法如下所示:

```
for VARIABLE_NAME in ITEM_1 ITEM_N
do
  command 1
  command 2
  command N
done
```

for 循环的第一行以单词“for”开头，后跟一个变量名，然后是一个项目列表。下一行包含单词“do ”,然后您应该放置您想要执行的语句，最后在下一行用单词“done”结束 for 循环。

在这种情况下，第一项被赋给变量，这组命令在该变量上执行，然后列表中的下一项被赋给该变量，这些命令再次在该变量上执行。这种情况会一直发生，直到循环到达列表中的最后一个元素。

## 接受用户输入(标准输入)

如果要接受 STDIN，使用 ***读取*** 命令。请记住，STDIN 通常来自键盘输入，但也可能来自其他来源，如另一个命令或一组命令的输出。 ***读作*** 的语法如下:

```
 read -p "PROMPT" VARIABLE
```

这里的 PROMPT 基本上就是你要向用户显示的内容。下面的例子:

```
read -p "Enter your name: " NAME
echo "You are registered, $NAME"
```

## 位置参数

位置参数是包含命令行内容的变量，变量有$0，$1，$2，$3……..$9.这个脚本本身存储在$0 中，第一个参数存储在$1 中，第二个存储在$2 中，依此类推。

看看这个例子:

```
$ script.sh parameter1 parameter2 parameter3
```

这里，变量及其值如下:

```
**$0 : "script.sh"
$1 : "parameter1"
$2 : "parameter2"
$3 : "parameter3"**
```

要访问命令行上的所有项目，请使用特殊变量 **$@。**

## 功能

每当您需要在脚本中多次执行相同的操作时，这是一个好迹象，表明您应该使用函数来代替。简单地说，函数是一个可重用的代码块，它执行一个动作并返回一个退出状态或返回代码。

有两种方法定义一个函数是一个 shell 脚本:

第一种方式:

```
function function-name() {
  #code goes here
}
```

第二种方式:

```
funtion-name() {
  #code goes here
}
```

要调用或执行函数，只需在脚本中的任何一行列出它的名字。一旦定义好，它就可以充当您可以在 shell 脚本中使用的任何其他命令。

*注意:调用函数时，不要使用括号。*

定义和调用函数的示例:

```
#!/bin/bash
function hello() {
  echo "Hello!"
}hello
```

当您运行上面的脚本时，屏幕上会显示单词 hello。

本文涵盖的主题是您编写自己的 bash 脚本所需的全部内容，并且肯定会引导您编写更复杂的脚本和自动化您的任务。

我希望这是有用的，请点击鼓掌按钮👏🏽如果你觉得有用/有见地。随意写评论。

编码快乐！