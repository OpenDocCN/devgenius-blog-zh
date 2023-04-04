# 带有参数和命名元组的 Python 装饰器

> 原文：<https://blog.devgenius.io/python-decorators-with-arguments-and-named-tuple-9ac58f4392ad?source=collection_archive---------19----------------------->

在之前的装饰者教程中，我没有涉及一个主题——带参数的装饰者。

带参数的装饰器是一个接受自定义参数并返回实际装饰器(将应用于被装饰函数)的函数。

要了解更多，请查看下面的普通装饰者示例。

```
def decorator_Function(org_function):
    def inner_Function():
        print("Printing from Decorators")
        return org_function()
    return inner_Function

@decorator_Function
def display():
    print('Print Display')

display()
Output-
Printing from Decorators
Print Display
```

这里，我们在装饰器内部传递 display()函数，并向它添加一条消息。

假设我们想在调用装饰器时添加一些自定义消息，而不是硬编码的消息，那么我们可以使用带参数的装饰器。

下面是一个相同的例子。

```
def outer_example(message):
    def decorator_Function(org_function):
        def inner_Function():
            print(message,"Printing from Decorators")
            return org_function()
        return inner_Function
    return  decorator_Function

@outer_example('Testing - ')
def display():
    print('Print Display')

display()
Output-
Testing -  Printing from Decorators
Print Display
```

在上面的例子中，我们在装饰器上添加了一个 outer_example()并传递了参数，我们还引入了一个嵌套层作为 outer_example，并需要添加一个返回装饰器的 return 语句。

现在我们可以传递带有 display()函数的' @outer_example '作为装饰器，并传递参数。在这里，我们将“测试”作为一条消息添加并打印出来。

**命名元组:-**

命名元组是集合模块下的另一个类。像字典类型的对象一样，它包含映射到一些值的键。在这种情况下，我们可以使用键和索引来访问元素。

这里是一个普通元组的例子，我们使用索引来访问值。

```
color = [55,67,123]
print(color[0])
Output-
55
```

假设在上面的例子中，如果“55”表示红色，67 表示“绿色”，依此类推，我们就无法将数值与数字联系起来。因此，如果我们想做同样的事情，我们也可以使用字典，并将其标记为键-值对。

```
color = {'red':55,'green':67,'white':123}
print(color['red'])
```

但是 Dictionary 是一个可变的对象，而 tuple 是不可变的，所以如果我们需要使用 tuple 执行相同的操作，那么我们可以使用一个命名的 tuple。

```
from collections import namedtuple
#Creating Named tuple
Color = namedtuple('Color',['red','green','white'])
# Mapping the value
color = Color(55,67,123)
# accessing the value
print(color.red)
Output-
55
```