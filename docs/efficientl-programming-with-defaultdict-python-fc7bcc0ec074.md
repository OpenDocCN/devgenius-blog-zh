# 使用 defaultdict() Python 高效编程

> 原文：<https://blog.devgenius.io/efficientl-programming-with-defaultdict-python-fc7bcc0ec074?source=collection_archive---------4----------------------->

![](img/3467c4cfc90efd8f78598b37b7a0afe0.png)

在这篇小文章中，我想向您展示一个让您的开发过程更加高效的小特性！

我会用例子给你看什么是 defaultdict()以及 dict()和 defaultdict()的区别。

## 在这里可以观看关于 defaultdict()的视频:

# dict() vs defaultdict()

**Defaultdict** 是一个类似容器的字典，存在于模块**集合**中。Defaultdict 是 dictionary 类的一个子类，它返回一个类似字典的对象。除了 **defaultdict 从不引发 KeyError** 之外，字典和 defaultdict 的功能几乎相同。它为不存在的键提供默认值。

**语法:**default dict(default _ factory)
**参数:**

*   **default_factory:** 返回定义的字典的默认值的函数。如果这个参数不存在，那么字典会产生一个 KeyError。

实际上，你知道如果你试图把一个值追加到缺少键的字典中，你会得到一个 KeyError。

```
certificates = {}
students = [“Michael”, “George”, “Jesica”]
grades = [
    {“History”: 50, “Mathematics”: 90, “Physics”: 70},
    {“History”: 70, “Mathematics”: 85, “Physics”: 30},
    {“History”: 60, “Mathematics”: 50, “Physics”: 60}
]for index, value in enumerate(grades):
    certificates[students[index]].append(value)
>>> KeyError: ‘Michael’
```

为了避免这种错误，您可以构建以下“if”语句:

```
for index, value in enumerate(grades):
    if students[index] not in certificates:
        certificates[students[index]] = []
    certificates[students[index]].append(value)
>>> {
    'Michael': [{'History': 50, 'Mathematics': 90, 'Physics': 70}],
    'George': [{'History': 70, 'Mathematics': 85, 'Physics': 30}], 
    'Jesica': [{'History': 60, 'Mathematics': 50, 'Physics': 60}]
}
```

但是您可以使用 **defaultdict()** 让您的代码更加清晰:

```
from collections import defaultdict certificates = defaultdict(list)
for index, value in enumerate(grades):
    certificates[students[index]].append(value)
>>> defaultdict(<class ‘list’>, {‘Michael’: [{‘History’: 50, ‘Mathematics’: 90, ‘Physics’: 70}], ‘George’: [{‘History’: 70, ‘Mathematics’: 85, ‘Physics’: 30}], ‘Jesica’: [{‘History’: 60, ‘Mathematics’: 50, ‘Physics’: 60}]})
```

在上面的例子中，我们将 **default_factory** 设置为**列表**，如果我们调用 missing key，我们将得到一个空列表 **[]** 作为 missing key 的默认值。我们也可以设置任何数据类型: **int，str，dict，float，set，tuple，list** 。

让我们尝试所有这些数据类型并检查结果:

```
test_dict = defaultdict(**int**)
>>> 0test_dict = defaultdict(**float**)
>>> 0.0test_dict = defaultdict(**dict**)
>>> {}test_dict = defaultdict(**list**)
>>> []test_dict = defaultdict(**tuple**)
>>> ()test_dict = defaultdict(**set**)
>>> set()test_dict = defaultdict(**str**)
>>> ‘’
```

此外，您可以设置自己的默认值。default_factory 必须是可调用的，这样我们就可以做下一步:

```
test_dict = defaultdict(lambda: “Default value by lambda”)
test_dict[missing_key]
>>> ‘Default value by lambda’
```

运筹学

```
def test_function():
    return “Default value by usual function”test_dict = defaultdict(test_function)
>>> ‘Default value by usual function’
```

但是如果你不设置 default_factory，它将会是 **None** ，在这种情况下，如果你试图调用一个丢失的键，你会得到一个 **KeyError**

```
test_dict = defaultdict()
test_dict[“missing_key”]
>>> KeyError: missing_key
```

留下你的评论，比如这个视频。

谢谢观看和良好的发展！回头见！