# Python 继承

> 原文：<https://blog.devgenius.io/python-inheritance-7e84be118c60?source=collection_archive---------14----------------------->

# 概观

继承是面向对象编程(OOP)的四大支柱之一。继承允许类从另一个类继承所有方法和属性。父类是被继承的类，也称为基类。子类是从另一个类继承的类。

![](img/bfddb7820b4821d68738e80b9d544316.png)

马库斯·斯皮斯克在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上的照片

# 父类

父类是一个典型的类，它只需要有将被继承的方法和属性。

```
class Parent:
  def __init__(self, name):
    self.name = name

  def printName(self):
    print(self.name)
```

# 子类别

子类可以是一个典型的类，但是你只需要把父类放在子类后面的括号中。

```
class Child(Parent):
  passx = Child("Mike")
x.printName()>> Mike
```

当类没有任何方法或属性时，使用 *pass* 关键字。

# __init__()

每次创建类时都会调用 *__init__()* 函数。当子类从父类继承时，父类 *__init__()* 函数被子类 __init__()函数覆盖。这意味着只有孩子的 *__init__()* 函数会被调用，而不是父母或两者。

```
class Parent:
  def __init__(self, name):
    print("Parent")
    self.name = name

  def printName(self):
    print(self.name)class Child(Parent):
  def __init__(self, name):
    print("Child")
    self.name = namex = Child("Mike")
x.printName()>> Child
>> Mike
```

# 超级()

*super()* 函数引用了从继承而来的父类，这使您可以访问父类的属性和方法。

```
class Parent:
  def __init__(self, name):
    print("Parent")
    self.name = name

  def printName(self):
    print(self.name)class Child(Parent):
  def __init__(self, name):
    print("Child")
    self.name = namex = Child("Mike")
x.printName()>> Parent
>> Mike
```

# 新属性

当子类从父类继承时，可以向子类添加父类没有的属性。

```
class Parent:
  def __init__(self, name):
    print("Parent")
    self.name = namedef printName(self):
    print(self.name)class Child(Parent):
  def __init__(self, name, age):
    print("Child")
    self.name = name
    self.age = age
  def getAge(self):
    print(self.age)x = Child("Mike", 9)
x.printName()
print(x.age)>> Parent
>> Mike
>> 9
```

# 新方法

子类继承父类时可以有自己的方法。

```
class Parent:
  def __init__(self, name):
    print("Parent")
    self.name = name
  def printName(self):
    print(self.name)

class Child(Parent):
  def __init__(self, name, age):
    print("Child")
    self.name = name
    self.age = age
  def getAge(self):
    print(self.age)

x = Child("Mike", 9)
x.printName()
x.getAge()>> Child
>> Mike
>> 9
```

如果子类与父类有相同的方法名，它将覆盖父类的方法。

```
class Parent:
  def __init__(self, name):
    print("Parent")
    self.name = name
  def printName(self):
    print(self.name)

class Child(Parent):
  def __init__(self, name, age):
    print("Child")
    self.name = name
    self.age = age
  def getAge(self):
    print(self.age)
  def printName(self):
    print("Child " + self.name)

x = Child("Mike", 9)
x.printName()
x.getAge()>> Child
>> Child Mike
>> 9
```

要调用被覆盖的父类方法，您必须使用 *super()* 方法。

```
class Parent:
  def __init__(self, name):
    print("Parent")
    self.name = name
  def printName(self):
    print("Parent " + self.name)

class Child(Parent):
  def __init__(self, name, age):
    print("Child")
    self.name = name
    self.age = age
  def getAge(self):
    print(self.age)
  def printName(self):
    super().printName()

x = Child("Mike", 9)
x.printName()
x.getAge()>> Child
>> Parent Mike
>> 9
```

# 结论

继承有助于表示真实世界的关系。它通过减少代码来帮助提高可重用性。如果多个类将使用相同的属性和方法，您可以有一个父类和多个从父类继承的子类。继承也是可传递的，所以如果一个类从父类继承的子类继承，那么这个类从子类和父类获得方法和属性。