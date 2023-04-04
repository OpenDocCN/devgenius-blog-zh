# Python 异常处理

> 原文：<https://blog.devgenius.io/python-exception-handling-bbaea543785b?source=collection_archive---------10----------------------->

# 概观

在编程中有错误和例外。错误是程序不能处理的问题，但异常可以。错误的例子有语法错误、内存不足错误和递归错误。异常是在程序运行时检测到并可以处理的错误。错误处理的主要模块是 try、except、else 和 finally。

# 尝试

在 try 块中，您将代码放在您认为会发生异常的地方。如果没有异常，那么将运行 try 块中的整个代码，并跳过 except 块。如果 try 块中出现异常，则跳过剩余的代码。

```
**try**:
    x = int(input("Please enter a number: "))
```

# 除...之外

except 块是定义预期发生的异常的地方。如果 except 块不处理特定的异常，它将尝试将异常传递给外部 try 块。如果该异常未被处理，则它是一个未处理的异常，程序会停止并打印一条异常消息。

```
try:
    x = int(input("Please enter a number: "))
except ValueError:
    print("Oops!  That was no valid number.  Try again...")
```

# 其他

else 块是没有异常时执行代码的地方。可以在一个块中处理多个异常。

```
try:
    x = int(input("Please enter a number: "))
except ValueError:
    print("Oops!  That was no valid number.  Try again...")
else:
    print("Valid number")
```

# 最后

finally 块是不管是否有异常都要执行的代码。如果有一个异常没有被处理，那么在异常被传递之前，finally 块首先被执行。如果在错误处理中有中断、继续或返回，那么 finally 代码块就在它们之前执行。如果 finally 有 return 语句，那么 finally 块将是返回值，而不是其他块。

```
try:
    x = int(input("Please enter a number: "))
except ValueError:
    print("Oops!  That was no valid number.  Try again...")
else:
    print("Valid number")
finally:
    print("Always executed")
```

# 结论

当您的项目正在开发中时，您希望避免任何中断。为了避免这些中断，你应该尝试自己处理错误，而不是让程序暂停。在尝试处理异常时，您可以使用父异常类来处理更一般的异常，也可以使用特定的类来处理某些异常。也可以创建自己的异常并处理它。