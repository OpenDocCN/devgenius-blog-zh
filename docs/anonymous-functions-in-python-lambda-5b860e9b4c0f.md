# python 中的匿名函数(Lambda)

> 原文：<https://blog.devgenius.io/anonymous-functions-in-python-lambda-5b860e9b4c0f?source=collection_archive---------18----------------------->

> *用 python 中的 lambda 关键字可以创建小型匿名函数*

![](img/f38edb1eeefde644cf4faab960b558ca.png)

凯文·Ku 在 [Unsplash](/s/photos/coding?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 上的照片

Python 和其他编程语言中的 Lambda 表达式源于 lambda 演算，这是阿隆佐·邱奇发明的一种计算模型

语义上，它们只是普通函数定义的语法糖。像嵌套函数定义一样，lambda 函数可以引用包含范围内的变量。

在 Python 中，我们一般把它作为一个高阶函数的参数(一个接受其他函数作为参数的函数)。Lambda 函数与 filter()、map()等内置函数一起使用。

(这里的匿名只是指无名函数)

# 语法:

*   **关键词:** `lambda`
*   **参数:**
*   **一个表达式**

> *lambda 参数:表达式*

*它们在语法上被限制为一个表达式*。也就是说，可以传递任意数量的参数，但是只能对它们执行一个操作

函数是 python 中的对象，Lambda 函数可以用在任何需要函数对象的地方。即。我们可以把它赋给变量，和对象一样使用，

*变量*=**λ***自变量:表达式*

*变量(自变量)*

# 普通 def 定义函数和 lambda 函数之间的差异

```
# Python code to illustrate cube of a number  
# showing difference between def() and lambda(). 
def square(y): 
    return y*y; 

g = lambda x: x*x
print(g(7)) 

print(cube(5))
```

*   **不使用 Lambda :** 这里两者都返回给定数的立方。但是，在使用 def 时，我们需要定义一个名为 cube 的函数，并需要向它传递一个值。执行之后，我们还需要使用 *return* 关键字从调用函数的地方返回结果。
*   **使用 Lambda :** Lambda 定义不包含“return”语句，它总是包含一个返回的表达式。我们也可以把 lambda 定义放在任何需要函数的地方，我们根本不需要把它赋给变量。这就是 lambda 函数的简单性。

# python 中 Lambda 函数的示例

**1。**

```
# Program to show the use of lambda functions
double = lambda x: x * 2print(double(5))
```

在上面的程序中，`lambda x: x * 2`是 lambda 函数。这里 x 是参数，`x * 2`是被求值并返回的表达式。

这个函数没有名字。它返回一个分配给标识符`double`的函数对象。我们现在可以称之为正常函数。该声明

```
double = lambda x: x * 2
```

几乎与以下内容相同:

```
def double(x):
   return x * 2
```

**2。**

```
>>> pairs = [(1, 'one'), (2, 'two'), (3, 'three'), (4, 'four')]
>>> pairs.sort(key=lambda pair: pair[1])
>>> pairs
[(4, 'four'), (1, 'one'), (3, 'three'), (2, 'two')]
```

# 与过滤器()一起使用的示例

Python 中的`filter()`函数接受一个函数和一个列表作为参数。

使用列表中的所有项目调用该函数，并返回一个新列表，其中包含该函数评估为`True`的项目。

下面是一个使用`filter()`函数从列表中过滤出偶数的例子。

```
# Program to filter out only the even items from a list
my_list = [1, 5, 4, 6, 8, 11, 3, 12]
new_list = list(filter(lambda x: (x%2 == 0) , my_list))
print(new_list)
```

# 使用 map()的示例

Python 中的`map()`函数接受一个函数和一个列表。

使用列表中的所有项目调用该函数，并返回一个新列表，该列表包含该函数为每个项目返回的项目。

```
# Program to double each item in a list using map()
my_list = [1, 5, 4, 6, 8, 11, 3, 12]
new_list = list(map(lambda x: x * 2 , my_list))
print(new_list)
```

# 使用 reduce 的示例

Python 中的`reduce()`函数接受一个函数和一个列表作为参数。使用 lambda 函数和列表调用该函数，并返回新的简化结果。这在列表的对上执行重复的操作。这是 functools 模块的一部分。示例:

```
from functools import reduce
li = [5, 8, 10, 20, 50, 100] 
sum = reduce((lambda x, y: x + y), li) 
print (sum)
```

# 使用 lambda 的多键排序

```
data = [[1,3,4],[1,5,6],[1,4,1],[2,4,5],[2,1,7],[2,1,2]]
data = sorted(data,key=lambda l:(l[1], l[2], l[3]), reverse=False)
print data
```

或者你可以使用`itemgetter`达到同样的效果(这样更快并且避免了 Python 函数调用)(不是 lambda 只是一个额外的代码):

```
import operator
data = sorted(data, key = operator.itemgetter(1, 2, 3))
```

注意这里你可以使用`sort`而不是`sorted`然后重新分配:

```
data.sort(key = operator.itemgetter(1, 2, 3))
```

***建议链接:***

[https://docs . python . org/3/tutorial/control flow . html # lambda-expressions](https://docs.python.org/3/tutorial/controlflow.html#lambda-expressions)

[https://realpython.com/python-lambda/](https://realpython.com/python-lambda/)

[https://www . geeks forgeeks . org/python-lambda-anonymous-functions-filter-map-reduce/](https://www.geeksforgeeks.org/python-lambda-anonymous-functions-filter-map-reduce/)

[https://www . programiz . com/python-programming/anonymous-function](https://www.programiz.com/python-programming/anonymous-function)