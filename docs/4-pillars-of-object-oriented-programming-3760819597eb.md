# 面向对象编程的 4 个支柱

> 原文：<https://blog.devgenius.io/4-pillars-of-object-oriented-programming-3760819597eb?source=collection_archive---------10----------------------->

面向对象编程有 4 个支柱:

*   抽象
*   多态性
*   遗产
*   包装

或者派。

# 抽象

抽象是只显示相关信息的过程。例如，在 Youtube 上，你可以在搜索栏中搜索视频，但你不知道它实际上是如何搜索视频的。在编程中，某些语言有抽象类，如 Java 和 Python。抽象类声明了至少一个未实现但将由子类实现的方法。

```
**class** Polygon(ABC):
    # Abstract Method   
    **def** sides(self):
        **pass
class** Triangle(Polygon): 
    # Implement method
    **def** sides(self):
   **      print**("3 sides")
**class** Square(Polygon):
    # Implement method
    **def** sides(self):
   **      print**("4 sides")
```

在大型团队中工作时，抽象很有用。一个类可能有不同实现，但有一个共同的方法。您可以将抽象类视为用户和类之间的一种契约形式。该类将返回相同类型的值，但获取的方式可能不同。

# 多态性

多态性意味着有不同的形式。在编程中，它是指在不同的实现中拥有相同方法的能力。在 python 中，多态性的一个例子是 len()方法，它可以接受一个字符串或数组。

```
puts(len("string")
>> 6
puts(len[1,2,3])
>> 3
```

另一个例子是当不同的类有相同的方法和不同的实现时。

```
**class** Triangle(Polygon): 
    **def** sides(self):
   **      print**("3 sides")
**class** Square(Polygon):
    **def** sides(self):
   **      print**("4 sides")
```

# 遗产

继承是指子类获得其父类的所有方法和属性。

```
class Animal:
  def __init__(self, name, species): 
    self.species = species
    self.name = namedef printInfo(self):
    print(self.name, self.species)class Pet(Animal):
  pass

x = Pet("Spot", "Dog")
x.printInfo()
>> Spot Dog
```

在上面的例子中，动物类是父类，子类是宠物。类继承传递给它的任何类，并且可以传递多个类。

# 包装

封装是将数据和方法绑定到一个单元或类，以防止外部来源的干扰。这意味着类中的某些数据和方法在类外是不可访问的。在 python 中，私有方法和变量的名字以下划线开头。

```
class Account:
  def __init__(self, balance):
    self.__balance = balance
  def withdraw(self, amount):
    self.__balance -= amount
  def deposit(self, amount):
    self.__balance += amount
  def amount(self):
    self.__bankrupt()
    print(self.__balance)
  def __bankrupt(self):
    if self.__balance <= 0:
      print("bankrupt")

x = Account(0)
x.amount()
>>bankrupt
>> 0
```

如果你试图访问私有方法或变量，它会抛出一个错误。