# PHP — P73:错误

> 原文：<https://blog.devgenius.io/php-p73-errors-7c8abc9c6a13?source=collection_archive---------11----------------------->

![](img/daed65ca87767c55c578955ecb3e0d9c.png)

在过去的 72 篇文章中，我们生活在乐观的一面。我们从来没有停下来想过可能会发生错误。我们是程序员，我们非常擅长我们的工作。为什么我们会认为我们应该担心错误处理呢？哦，使用我们应用程序的用户。谁在乎呢。他们应该知道如何在提示时输入有效数据。当我们说他们应该输入他们的年龄时，显然我们的意思是这应该是一个数字(整数)而不是一个字符串。如果他们不知道如何遵循指示，那是他们的责任。你说他们可能会破坏数据库？好吧，这才是问题所在。

尽管大声说出来很可怕，但是把你的应用程序的每个用户都当成一个绝对的白痴。不要当着他们的面说，而是在幕后想一想。我不会责怪你，即使你笑了，因为你允许自己在脑海中尖叫“白痴”。你不会离真相太远的。人们会有意或无意地找到破坏你的应用程序的方法。全靠你全力守护。

[](/php-p72-errors-intro-83e7b58700d5) [## PHP — P72:错误介绍

### 在我们开始研究如何处理 PHP 中的错误之前，我想先谈一些初级程序员…

blog.devgenius.io](/php-p72-errors-intro-83e7b58700d5) 

# 认真对待错误

从 PHP 7 开始，通过抛出`Error`异常来报告错误。我们可能会遇到哪些类型的错误？

*   `ArithmeticError`是数学运算失败时抛出的错误。
*   `DivisionByZeroError`是一种当你试图被零除时出现的`ArithmeticError`。
*   `AssertionError`断言失败时抛出。
*   `CompileError`因编译错误而抛出。
*   `ParseError`是一种在解析 PHP 代码时出现的`CompileError`。例如，在使用`eval()`函数时，您会看到这个错误。
*   `TypeError`在某些情况下，当数据类型不正确时会发生，例如类属性不匹配，参数类型与声明的参数类型不匹配，或者返回类型与声明不同。
*   `ArgumentCountError`是`TypeError`的一种。当一个方法有一组预定义的参数，但传递给它的实参太少时，就会发生这种情况。
*   `ValueError`是一个棘手的问题。当数据类型正确，但预期值不正确时，就会出现这种情况。例如，期望正整数，但收到负整数。
*   `UnhandledMatchError`当匹配表达式没有手臂时发生。
*   对`Fiber`函数的无效操作会引发`FiberError`。

# 没有捕捉到错误

我们可以在 PHP 中快速创建一个错误进行测试，看看会发生什么。创建一个新的 php 文件并编写:`echo 10/0;`

你会得到一个错误。

```
Fatal error: Uncaught DivisionByZeroError: Division by zero in /home/user/scripts/code.php:3Stack trace:
#0 {main}
  thrown in /home/user/scripts/code.php on line 3
```

这是我们之前看到的`DivisionByZero`错误。有趣的是，上面写着`Uncaught`。我们没有`catch`它。但是我们如何捕捉错误呢？用`try/catch`块。

# 尝试/抓住

try/catch 块代表了一种测试方法，如果它不起作用，就在错误显示在屏幕上之前捕捉它，并向用户提供负 UX。

在我们的`try`块中我们可以尝试什么？我们可以尝试执行我们的`echo 10/0;`语句。将它放入你的`try { }`身体。

我们知道，在这种情况下，将抛出的错误是`DivisionByZero`错误。我们看到，我们之前尝试了代码，PHP 返回了一个`Uncaught`错误。一个`DivisionByZero`只是一个数据类型。它是由 PHP 开发人员创建的一种数据类型。把它想象成一个`string`或`int`数据类型。如果你读过我以前的文章，你会记得你可以声明参数的数据类型。

[](/php-p48-type-declarations-47114a7a0e0b) [## PHP — P48:类型声明

### 如果您来自 PHP 的旧时代，您可能已经被 PHP 自动为您声明类型所宠坏了…

blog.devgenius.io](/php-p48-type-declarations-47114a7a0e0b) 

让我们抓住我们的`DivisionByZero`错误。我们将在我们的`catch()`语句中捕获它。

运行代码。应该会出现一个空白屏幕。我们发现了错误，并阻止它出现在屏幕上！它现在存储在我们的`$error`变量中。我们可以优雅地处理它，就像发送一封电子邮件，声明发生了一个错误，或者将其记录到我们的日志文件中。我们也可以重复一些更加用户友好的东西。

再次运行代码，您将看到屏幕上显示消息`Dear idiot user. You cannot divide by zero`。

要查看$error 对象包含什么，只需使用`var_dump`并将其转储到屏幕上。

```
/app/72 Errors/72 Errors Intro.php:10:
**object**(*DivisionByZeroError*)[*1*]
  *protected* 'message' => string 'Division by zero' *(length=16)*
  *private* 'string' (Error) => string '' *(length=0)*
  *protected* 'code' => int 0
  *protected* 'file' => string '/app/72 Errors/72 Errors Intro.php' *(length=34)*
  *protected* 'line' => int 5
  *private* *array* 'trace' (Error) => 
    **array** *(size=0)*
      *empty*
  *private* *?Throwable* 'previous' (Error) => null
  *public* 'xdebug_message' => string '<tr><th align='left' bgcolor='#f57900' colspan="5"><span style='background-color: #cc0000; color: #fce94f; font-size: x-large;'>( ! )</span> DivisionByZeroError: Division by zero in /app/72 Errors/72 Errors Intro.php on line <i>5</i></th></tr>
<tr><th align='left' bgcolor='#e9b96e' colspan='5'>Call Stack</th></tr>
<tr><th align='center' bgcolor='#eeeeec'>#</th><th align='left' bgcolor='#eeeeec'>Time</th><th align='left' bgcolor='#eeeeec'>Memory</th><th align='left' bgcolor='#eeeeec'>Function</th><th align=''... *(length=842)*
```

# 为什么要测试这种东西？

我们为什么要测试这种东西？我们知道`10 / 0`总会把`DivisionByZeroError`扔出去，所以何必呢？用户输入。我们可以从用户那里得到输入，取而代之的是这样的内容:

```
echo 10 / $_POST['user_number'];
```

然后，用户可以在表单中键入他们想要的任何内容，当他们单击 submit 时，信息就会被发送过来进行处理。如果他们输入 0，错误将被捕获。

# 链接 Catch 语句

如果他们输入的不是数字，那该怎么办？如果用户输入一个字母呢？我们得到了一种新的错误。

```
Fatal error: Uncaught TypeError: Unsupported operand types: int / string in /home/user/scripts/code.php:3Stack trace:
#0 {main}
  thrown in /home/user/scripts/code.php on line 3
```

收到的错误是一个`TypeError`。我们如何抓住它？我们已经赶上了我们的`DivisionByZeroError`。我们在哪里放置`TypeError`的捕获物。你会紧紧抓住它。

# 一个更普通的捕捉

还会出现其他错误吗？当然了。如果我们想确保我们所有的错误都被处理，在 try/catch/catch/…/catch 语句的末尾，我们需要添加一个更通用的 catch 语句来捕获任何可能被遗漏的内容。就像您可以用`DivisionByZeroError`和`ArithmeticError`处理程序捕捉被零除的错误一样(因为 division by zero error 是算术错误类型)，您也可以用错误处理程序捕捉任何错误。我们上面看到的所有东西，比如`TypeError`，都是`Error`类型的。

# 最终声明

如果您想每次都执行一段代码，而不管是否捕捉到错误，该怎么办？您可以使用`finally`语句来实现。

如果您运行上面的代码，您将得到:

```
Dear idiot user. You cannot divide by zero
Dear idiot user. This code always executes.
```

# 结论

我认为，在这篇文章中，我们陷入了我所希望的最深的错误之中。他们很直接。我见过开发人员纠结于尝试/捕捉概念。就把它想象成一个 if/else 语句，其中 if = try，else = catch。如果有多个条件，就开始引入 if/elseif/else 语句。类似于 try/catch/catch 的概念。

[](https://github.com/dinocajic/php-youtube-tutorials) [## GitHub-dinocajic/PHP-YouTube-tutorials:PHP YouTube 教程的代码

### PHP YouTube 教程的代码确保你已经安装了 Docker。克隆回购。运行以下命令…

github.com](https://github.com/dinocajic/php-youtube-tutorials) ![](img/6b4f6c11928bbc970ba326e49ee4975b.png)

迪诺·卡伊奇目前是 [LSBio(生命周期生物科学公司)](https://www.lsbio.com/)、[绝对抗体](https://absoluteantibody.com/)、 [Kerafast](https://www.kerafast.com/) 、[珠穆朗玛生物](https://everestbiotech.com/)、[北欧 MUbio](https://www.nordicmubio.com/) 和 [Exalpha](https://www.exalpha.com/) 的 IT 主管。他还担任我的自动系统的首席执行官。他有十多年的软件工程经验。他拥有计算机科学学士学位，辅修生物学。他的背景包括创建企业级电子商务应用程序、执行基于研究的软件开发，以及通过写作促进知识的传播。

你可以在 [LinkedIn](https://www.linkedin.com/in/dinocajic/) 上联系他，在 [Instagram](https://instagram.com/think.dino) 上关注他，或者[订阅他的媒体出版物](https://dinocajic.medium.com/subscribe)。

[*阅读迪诺·卡吉克(以及媒体上成千上万其他作家)的每一个故事。你的会员费直接支持迪诺·卡吉克和你阅读的其他作家。你也可以在媒体上看到所有的故事。*](https://dinocajic.medium.com/membership)