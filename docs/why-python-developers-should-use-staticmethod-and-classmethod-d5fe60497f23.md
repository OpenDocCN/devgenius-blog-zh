# 为什么 Python 开发人员应该使用@staticmethod 和@classmethod

> 原文：<https://blog.devgenius.io/why-python-developers-should-use-staticmethod-and-classmethod-d5fe60497f23?source=collection_archive---------1----------------------->

![](img/3e333c7943fc51ac5c1e19581e930785.png)

在本文中，我将向您展示什么是`@staticmethod`和`@classcmethod`，以及为什么以及何时应该使用它们！让我们开始吧。

> 如果你还不知道什么是装饰器，以及它是如何工作的，那么首先阅读 [**这篇文章**](https://medium.com/@vlad.bashtannyk/what-the-decorators-in-plain-words-python-b600623ea497) 。

# 定义:

## @静态方法

`@staticmethod`是一个内置的装饰器，它在 Python 中定义了类中的静态方法。静态方法既不接受类参数，也不接受实例参数，不管它是由类的实例调用还是由类本身调用。

## 特征

*   在类中声明一个静态方法。
*   它不能有`cls`或`self`参数。
*   静态方法不能访问类属性或实例属性。
*   静态方法可以使用`ClassName.MethodName()`调用，也可以使用`object.MethodName()`调用。
*   它可以返回类的对象。

下面的示例声明了一个静态方法:

```
class Supermarket:
    product = "Milk"  # class attribute

    def __init__(self, product, best_before):
        self.best_before = best_before  # instance attribute
        self.product = product @staticmethod    
    def normalize_product_name(product):
        product = product.capitalize().strip()        
        return product
```

上面，`Supermarket`类使用`@staticmethod`装饰器将`normalize_product_name()`方法声明为静态方法。注意，它不能有`self`或`cls`参数。

可以使用`ClassName.MethodName()`或`object.MethodName()`调用静态方法。

```
>>> norm_product = Supermarket.normalize_product_name("milk  ")
'Milk'
>>> obj = Supermarket("Bread", "2022-05-18")
>>> obj.normalize_product_name("milk  ")
'Milk'
```

## @classmethod

`@classmethod`是一个内置的装饰器，在 Python 中定义了类中的类方法。类方法只接收类参数。可以使用`ClassName.MethodName()`或`object.MethodName()`调用类方法。

`@classmethod`是`classmethod()`功能的替代。建议使用`@classmethod` decorator 代替函数，因为它只是语法糖。

## 特征

*   声明一个类方法。
*   第一个参数必须是`cls`，可以用来访问类属性。
*   class 方法只能访问类属性，而不能访问实例属性。
*   可以使用`ClassName.MethodName()`和`object.MethodName()`调用类方法。
*   它可以返回类的对象。

下面的示例声明了一个类方法:

```
class Supermarket:
    product = "Milk"  # class attribute

    def __init__(self, product, best_before):
        self.best_before = best_before  # instance attribute
        self.product = product @classmethod    
    def get_product(cls):
        print("product=" + cls.product)
```

上面的`Supermarket`类包含一个类属性`product`和一个实例属性`best_before`。`get_product()`方法用`@classmethod`装饰器装饰，使其成为一个类方法，可以使用`Supermarket.get_product()`调用。注意，任何类方法的第一个参数必须是`cls`，它可以用来访问类的属性。您可以给第一个参数取任何名称，而不是`cls`。

可以使用`ClassName.MethodName()`或`object.MethodName()`调用类方法。

```
>>> Supermarket.get_product()
'product=Milk'
>>> obj = Supermarket()
>>> obj.get_product()
'product=Milk'
```

但是在类方法中你不能使用实例属性:

```
class Supermarket:
    product = "Milk"  # class attribute

    def __init__(self, product, best_before):
        self.best_before = best_before  # instance attribute
        self.product = product @classmethod    
    def get_product(cls):
        print(f"product={cls.product}, age={cls.best_before}") >>> Supermarket.get_product()
AttributeError: type object 'Supermarket' has no attribute 'best_before'
```

# 什么时候应该使用静态方法？

## 1.将效用函数分组到一个类中

静态方法的用例有限，因为像类方法或类中的任何其他方法一样，它们不能访问类本身的属性。

然而，当你需要一个不访问一个类的任何属性，但有意义的是它属于这个类的效用函数时，我们使用静态函数。

例如，您添加了更改“保质期”格式的功能:

```
from datetime import datetime class Supermarket:    
    def __init__(self, product, best_before):
        self.best_before = "2022-05-18"
        self.product = "Milk"

    @staticmethod    
    def change_date_format(best_before):
        best_before = datetime.strptime(best_before, "%Y-%m-%d")
        best_before = best_before.strftime("%d-%m-%Y")
        return best_before >>> Supermarket.change_date_format("2022-08-06")
'06-08-2022'
```

它是一个静态方法，因为它不需要访问`Supermarket`本身的任何属性，只需要参数。

## 2.具有单一实现

```
from datetime import datetime class Supermarket:    
    def __init__(self, product, best_before):
        self.best_before = best_before
        self.product = product

    def get_best_before_date(self):
        return self.best_before

    [@staticmethod](http://twitter.com/staticmethod)    
    def change_date_format(best_before):
        best_before = datetime.strptime(best_before, "%Y-%m-%d")
        best_before = best_before.strftime("%d-%m-%Y")
        return best_before class GroceryStore(Supermarket):
    def get_best_before_date(self):
        return Supermarket.change_date_format(self.best_before)

>>> supermarket = Supermarket("Milk", "2022-05-18")
>>> grocery = GroceryStore("Milk", "2022-05-18")
>>> supermarket.get_best_before_date()
'2022-05-18'
>>> grocery.get_best_before_date()
'18-05-2022'
```

# 什么时候应该使用类方法？

您可以将类方法用于未绑定到特定实例但绑定到类的任何方法。实际上，对于创建类实例的方法，通常使用类方法。

## 1.工厂方法

工厂方法是那些为不同用例返回类对象的方法。例如:

```
class Supermarket:    
    def __init__(self, product, best_before):
        self.best_before = "2022-05-18"
        self.product = "Milk"

    @classmethod    
    def add_product(cls):
        return cls("Bread", "2022-05-29")>>> obj = Supermarket.add_product()
>>> obj.product
'Milk'
>>> obj.best_before
'2022-05-18'
```

在这种情况下，add_product()函数只创建一个新的类对象(一个新产品及其保质期)。

## 2.更正继承中的实例创建

每当您通过将工厂方法实现为类方法来派生一个类时，它都会确保正确创建派生类的实例。

您可以创建一个静态方法，但是它创建的对象将总是被硬编码为基类。

但是，当您使用类方法时，它会创建派生类的正确实例。

```
class Supermarket:
    product_price = {"Milk": 1} def __init__(self, product, best_before):
        self.best_before = "2022-05-18"
        self.product = "Milk" @staticmethod
    def add_import_product(product, best_before):
        return Supermarket(product, best_before) @classmethod    
    def add_product(cls, product, best_before):
        return cls(product, best_before) class GroceryStore(Supermarket):
    product_price = {"Milk": 2}grocery1 = GroceryStore.add_import_product("Bread", "2022-06-05")
isinstance(grocery1, GroceryStore)
>>> False
grocery2 = GroceryStore.add_product("Apple", "2022-06-10")
isinstance(grocery2, GroceryStore)
>>> True
```

这里，使用静态方法创建一个类实例，希望我们在创建过程中硬编码实例类型。

这显然会在继承`Supermarket`到`GroceryStore`时产生问题。

`add_import_product`方法不返回`Grocerystore`对象，而是返回其基类`Supermarkets`的对象。

这违反了 OOP 范式。使用类方法作为`add_product()`可以确保代码的面向对象性，因为它将第一个参数作为类本身，并调用它的工厂方法。

# 结论:

在本文中，我们分析了什么是`@staticmethod`和`@classcmethod`，并了解了如何、在哪里以及为什么使用它们:

*   我们一般使用@classmethod 来创建工厂方法。工厂方法返回不同用例的类对象。
*   我们通常使用静态方法来创建和分组效用函数。

在评论中分享你的想法和观点，如果这篇文章对你有用和有趣，点击**“鼓掌”**。点击**“关注”**总能获得有用的文章！