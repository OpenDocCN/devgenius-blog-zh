# 在 Python 中导入模块

> 原文：<https://blog.devgenius.io/importing-modules-in-python-ef4a514870bf?source=collection_archive---------0----------------------->

## 让我们了解一下在 python 中导入模块的不同方法。

![](img/52f67734545a4abba7e590d4659dcac0.png)

来自 [Pexels](https://www.pexels.com/photo/birds-eye-view-photo-of-freight-containers-2226458/?utm_content=attributionCopyText&utm_medium=referral&utm_source=pexels) 的[汤姆·菲斯克](https://www.pexels.com/@tomfisk?utm_content=attributionCopyText&utm_medium=referral&utm_source=pexels)的照片

## 在 Python 中导入模块

## 模块

模块是包含 Python 定义和语句的文件。文件名是模块名加上后缀`.py`。模块提供了代码的可重用性。通过使用模块，我们可以对相关的代码进行分组。— [python 文档](https://docs.python.org/3/tutorial/modules.html)

## **本故事涵盖的主题**

1.  **导入模块的不同方式。**

*   使用 import 语句
*   使用 from 子句
*   使用 from 子句和*

2.**模块搜索路径**

**3。出现的错误**

*   ModuleNotFoundError
*   属性错误
*   导入错误
*   名称错误

## 导入模块的不同方式。

1.  **使用导入语句**

`import modulename`

导入语句分两步执行:

1.  找到一个模块，加载并初始化它(如果需要的话)
2.  在本地命名空间中为 import 语句出现的范围定义一个或多个名称。

导入的模块名放在导入模块的全局符号表中。只放置模块名，而不放置模块中的函数、变量或类名。所以，如果我们想从导入的模块中访问函数或变量，就必须通过提及
`modname.fnname`-用于访问函数
`modname.varname`-用于访问变量

**例如:**

*   我已经创建了一个文件**“add . py”**。它包含变量 `a`、`b`和函数`a1`。

```
x=5
y=10
**def** a1(a,b):
    **return** (a+b)
```

*   现在，必须将这个模块导入到另一个文件 **"calc.py"**

导入的模块名(add.py)只放在导入模块的(calc.py)全局符号表中。

> **导入添加→导入** `**add**` **并有界** `**locally**`

因此，访问函数 a1 为
`add.a1()`

访问变量如
`add.x`
`add.y`

```
**import** add
print (add.a1(4,5))
*#Output:9* print (add.x)*#Output:5* print (add.y)*#Output:10*
```

## 分配一个本地名称

我们可以为导入的模块或`modname.fnname` 指定一个本地名称，并在程序中使用它

为导入的模块指定本地名称，如

`import add as a`

> **导入添加为 a →导入** `**add**` **并有界为** `**a**`

示例:

在这个程序中，我们可以使用本地名称来引用导入的模块

```
**import** add **as** a
print (a.a1(4,5))
*#Output:9* print (a.x)*#Output:5*
```

为 modname.fnname 指定本地名称，如

b=add.a1

我们可以使用本地名称从导入的模块中访问函数。

```
**import** add
n=add.a1
print (n(3,4))
*#Output:7*
```

**2。使用 from 子句**

`from modname import fn1,fn2,var1`

1.  首先，它将找到在 `from` 子句中指定的模块，加载它，并在必要时初始化它。
2.  对于在`import`子句
    *中指定的每个标识符，检查导入的模块是否具有该名称的属性
    *如果没有，尝试导入具有该名称的子模块，然后再次检查导入的模块的属性
    *如果没有找到该属性，则引发`ImportError`。
    *否则，对该值的引用存储在本地名称空间中，使用`as`子句中的名称(如果存在的话)，否则使用属性名称

**例子**

此 import 语句将模块中的名称直接导入到导入模块的符号表中。

所以我们可以直接访问函数和变量，而不用提到模块名。

> 从添加导入 a1，x→导入`add`和`add.a1`有界为`a1`，`add.x`有界为 `x`

```
**from** add **import** a1,x
print (a1(2,3))*#Output:5* print (x)*#Output:5*
```

**3。使用 from 子句和***

它将导入模块定义的所有名称。我们可以直接访问导入模块中的所有名称。

```
**from** add **import** *
print (a1(2,3))*#Output:5* print (x)*#Output:5*
```

**注意:**这将导入除下划线(`_`)开头的名称之外的所有名称。在大多数情况下，Python 程序员不使用这个工具，因为它向解释器中引入了一组未知的名称，可能隐藏了一些您已经定义的内容。

## 模块搜索路径

第一次导入模块时

1.  解释器首先搜索内置模块
2.  然后，它将在变量 sys.path 给出的目录列表中搜索该文件。

`sys.path`从这些位置初始化

1.  当前目录(包含输入脚本的目录)
2.  带有目录列表的环境变量
3.  安装相关目录

```
**import** sys
print (sys.path)
```

它将列出所有模块的搜索路径

如果我们想在搜索路径中添加任何目录，我们可以使用`sys.path.append()`来添加

```
**import** sys
sys.path.append(**"C:\indhuProject"**)
```

现在，导入的模块也将在这个路径中搜索。像这样，我们也可以从不同的目录导入模块。

## 引发的错误

1.  ModuleNotFoundError
2.  属性错误
3.  导入错误
4.  名称错误

1. **ModuleNotFoundError**

如果导入的模块不存在，意味着它将引发`ModuleNotFoundError`

**例 1**

```
**from** sub **import** a1,x
print (x)
*#Output:ModuleNotFoundError: No module named 'sub'*
```

示例 2

```
**import** sub
print (x)
*#Output:ModuleNotFoundError: No module named 'sub'*
```

**2。属性错误**

当从导入的模块中访问函数/变量时，如果它不存在意味着，它将引发`AttributeError`。

```
**import** add
print (add.a2(1,2))
*#Output:AttributeError: module 'add' has no attribute 'a2'*
```

**3。导入错误**

使用 from 子句，当我们正在导入时，如果属性不可用意味着它将引发`ImportError`

```
**from** add **import** s2,x
print (x)
*#Output:ImportError: cannot import name 's2' from 'add'*
```

**4。名称错误**

如果我们使用 import 语句，如果我们直接访问函数名和变量名，就会引发`NameError.`

我们应该像`modname.varname`一样访问

**例 1:**

```
**import** add
print (a1(4,5))
*#Output:NameError: name 'a1' is not defined*print (x)*#Output:NameError: name 'x' is not defined*
```

**例二:**

如果我们为模块创建一个本地名称，那么我们必须使用这个本地名称来引用导入的模块。否则它将引发 NameError

```
**import** add **as** a
print (add.x)
*#Output:NameError: name 'add' is not defined*
```

**例 3:**

如果我们使用 from 子句导入，它将把名字从模块直接导入到导入模块的符号表中。所以我们可以直接访问 import 语句中提到的函数名/变量名。

如果我们提到 modname.fnname()，就会引发 **NameError** 。

```
**from** add **import** a1
print (add.a1(3,4))
*#Output:NameError: name 'add' is not defined*
```

## 结论:

import 语句将在`sys.path`提到的路径中寻找模块。如果我们想从任何其他路径导入，使用命令`sys.path.append()`将该路径包含在 sys.path 中

```
import add →Imported *add* and bounded *locally* import add as a → Imported *add* and bounded as *a* from add import a1,x→ Imported *add* and *add.a1* bounded as *a1*,*add.x* bounded as *x*
```

## **资源(Python 文档):**

[模块](https://docs.python.org/3/tutorial/modules.html)

[导入声明](https://docs.python.org/3/reference/simple_stmts.html#import)

## 延伸阅读:

[](https://medium.com/dev-genius/importing-packages-in-python-fb3f4a64ed14) [## 在 Python 中导入包

### 探索用 python 导入包的不同方法

medium.com](https://medium.com/dev-genius/importing-packages-in-python-fb3f4a64ed14) 

请关注此空间，了解更多关于 Python 和数据科学的文章。如果你喜欢看我的更多教程，就关注我的 [***中***](https://medium.com/@IndhumathyChelliah)[***LinkedIn***](https://www.linkedin.com/in/indhumathy-chelliah/)*[***Twitter***](https://twitter.com/IndhuChelliah)***。****

*感谢阅读！*