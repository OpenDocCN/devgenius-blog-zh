# Python:面向对象编程

> 原文：<https://blog.devgenius.io/python-object-oriented-programming-120080bc6a5e?source=collection_archive---------15----------------------->

*作者:加利赫*

![](img/606e5f2aa21592dd6d17028cabe080f6.png)

在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上由 [Hitesh Choudhary](https://unsplash.com/@hiteshchoudhary?utm_source=medium&utm_medium=referral) 拍摄的照片

> **目录** :
> ●构造器&析构器
> ●封装
> ●设置器&获取器
> ●继承
> ●重写&重载方法(多态)
> ●抽象类
> ●接口类
> ●静态方法
> ●魔法方法
> ●运算符重载
> ● Super()
> ●多重继承
> ●方法解析顺序

# ●构造函数和析构函数

构造函数和析构函数实际上是面向对象编程范式中的一种方法，这不仅仅是在 python 中。这个方法可能是类创建结构中必须存在的东西。

当你创建一个类时，你可以在对象创建过程本身的同时做一些事情。构造函数是一种特殊的方法，在创建对象(实例)时会自动执行。而析构函数是在对象被销毁时调用的函数。

**构造函数**通常用于进行对象准备的初始过程，比如给属性赋值，调用内部方法，以及其他几个被认为必要的过程。在 Python 中， **__init__()** 方法被称为控制器，并且总是在创建对象时被调用。

```
def __init__(self):
 # body of the constructor
```

**析构函数**用于在对象从内存中完全移除之前进行最后的清理。例如，关闭数据库连接，关闭当前打开的文件，或者在应用程序运行时清除缓存。

移除它的方法是使用 **del** 命令。python 中的析构函数名为 __del__()，这是一个神奇的方法，其调用过程是由系统自动完成的。即使实际上没有调用 **del** 函数，如果这个类有一个析构函数，那么在没有程序运行之后，所有的变量都会被自动销毁。

# ●封装

**封装**是一种像包裹在胶囊里一样包裹信息的机制。我们可以在一个类中设置属性和函数的权限。这个概念通常也被称为访问修饰符。

python 中有 3 种可用的访问修饰符:

1.公共

具有公共访问权限的变量或属性可以从任何地方访问，无论是从类外部还是从类内部。考虑下面的例子:

2.保护

具有受保护访问权限的变量或属性只能以有限的方式由它们自己访问(即在内部类中)，也可以从它们的派生类中访问。

要定义具有受保护特权的属性，我们必须在变量名前使用下划线前缀\\。

```
class Modifiers:
 def __init__(self, name):
 self._protected_member = name 

 m = Modifiers("Galih")
 print(m._protected_member)
 m._protected_member = "Ngekho"
 print(m._protected_member)
```

3.私人的

下一个修饰符是私有的。类中的每个变量都有私有访问权限，它只能在该类中被访问。它不能从外部访问，甚至不能从继承它的类访问。要使属性私有，我们必须添加两个下划线作为属性名前缀。

**名芒林**

如果在属性名前加上双下划线(__)，如下所示:

_ _ 属性

Python 会自动将 _ _ 属性的名称更改为:

_ class _ _ 属性

这样，您就不能从类的外部直接访问 _ _ 属性，如:

实例。_ _ 属性

但是，您仍然可以使用 _ class _ _ 属性名称来访问它:

实例。_ class _ _ 属性

# ● Setter & Getter

Setters 和 getters 是用于检索数据并将其填充到对象中的两种方法。在封装中，数据可以用私有修饰符包装，这样就不能从类外部直接访问它们。所以 setter 和 getter 方法将帮助我们访问数据。创建 setter 和 getter 方法的方法通常与创建方法相同。

考虑下面的例子:

```
class User {
 private String username;
 private String password;
   // setter method
   public void setUsername(String username){
     this.username = username;
   }
   public void setPassword(String password){
     this.password = password;
   }
   // getter method
   public String getUsername(){
     return this.username;
   }
   public String getPassword(){
     return this.password;
   }
 }
```

setter 和 getter 方法必须被赋予一个公共修饰符，因为这些方法将从类的外部被访问。setter 和 getter 方法的区别在于返回值、参数和方法内容。setter 方法没有返回值。因为任务只是将数据填充到属性中。而 getter 方法根据要检索的数据类型返回一个值。

# ●继承

一个类或对象可以与其他类相关。这种关系的一种形式是继承。这种关系就像父母和孩子的家庭关系。

一个类可以有一个或多个后代或子类。子类将继承父类的属性和方法。

在继承中，超类可以将它拥有的属性传递给每个子类，但不是超类拥有的所有属性都可以被子类继承。

可以从超类继承到子类的权限提示是受保护的和公共的，而私有的不会传递到子类。

创建父类

```
class Person:
 def __init__(self, fname, lname):
 self.firstname = fname
 self.lastname = lname

 def printname(self):
 print(self.firstname, self.lastname)

 #Use the Person class to create an object, and then execute the printname method:

 x = Person("John", "Doe")
 x.printname()
```

创建子类

```
class Student(Person):
 pass
```

现在，学生类与 Person 类具有相同的属性和方法。

```
x = Student("Mike", "Olsen")
x.printname()
```

# ●覆盖和重载方法(多态性)

python 中的多态性定义了子类中与父类中的方法同名的方法。在继承中，子类从父类继承方法。此外，还可以修改子类中从父类继承的方法。

这主要用于从父类继承的方法不适合子类的情况。这个在子类中重新实现方法的过程被称为[方法覆盖](https://www.edureka.co/blog/method-overloading-and-overriding-in-java/)。下面的例子展示了继承的多态性:

方法重写是在子类或子类的超类中重新创建现有方法。使用这种方法是为了使子类具有更具体的功能。

```
class Parent():

 # Constructor
 def __init__(self):
   self.value = "Inside Parent"

 # Parent's show method
 def show(self):
   print(self.value)

 # Defining child class
 class Child(Parent):

 # Constructor
 def __init__(self):
   self.value = "Inside Child"

 # Child's show method
 def show(self):
   print(self.value)

 # Driver's code
 obj1 = Parent()
 obj2 = Child()

 obj1.show()
 obj2.show()
```

重载是一种方法，可以在一个类中创建两个或两个以上同名的方法，但参数的类型和个数必须互不相同。

# ●抽象类

如果一个类包含一个或多个抽象方法，那么这个类就称为抽象类。抽象方法是声明的方法，但不包含实现。抽象类不能被实例化，它的抽象方法必须由它的子类实现。

一个抽象类可以被认为是其他类的蓝图。它允许您创建一组方法，这些方法必须在从抽象类构建的任何子类中创建。包含一个或多个抽象方法的类称为抽象类。抽象方法是有声明但没有实现的方法。当我们设计大型功能单元时，我们使用一个抽象类。当我们想为一个组件的不同实现提供一个公共接口时，我们使用一个抽象类。

```
from abc import ABC, abstractmethod

 class Polygon(ABC):

 @abstractmethod
 def noofsides(self):
   pass

 class Triangle(Polygon):

 # overriding abstract method
 def noofsides(self):
   print("I have 3 sides")

 class Pentagon(Polygon):

 # overriding abstract method
 def noofsides(self):
   print("I have 5 sides")

 class Hexagon(Polygon):

 # overriding abstract method
 def noofsides(self):
   print("I have 6 sides")

 class Quadrilateral(Polygon):

 # overriding abstract method
 def noofsides(self):
   print("I have 4 sides")

 # Driver code
 R = Triangle()
 R.noofsides()

 K = Quadrilateral()
 K.noofsides()

 R = Pentagon()
 R.noofsides()

 K = Hexagon()
 K.noofsides()

//result
I have 3 sides
I have 4 sides
I have 5 sides
I have 6 sides
```

# ●接口类别

在 Python 这样的面向对象语言中，接口是应该由实现类提供的方法签名的集合。实现接口是编写有组织的代码和实现抽象的一种方式。

包 **zope.interface** 为 Python 提供了“对象接口”的实现。它由 Zope 工具包项目维护。该包直接导出两个对象，“接口”和“属性”。它还导出了几个助手方法。它旨在提供比 Python 内置的 abc 模块更严格的语义和更好的错误消息。

```
import zope.interface

class MyInterface(zope.interface.Interface):
  x = zope.interface.Attribute("foo")
  def method1(self, x):
    pass
  def method2(self):
    pass

print(type(MyInterface))
print(MyInterface.__module__)
print(MyInterface.__name__)

# get attribute
x = MyInterface['x']
print(x)
print(type(x))
```

**实现接口**

接口充当设计类的蓝图，所以接口是使用类上的**实现器**装饰器实现的。如果一个类实现了一个接口，那么这个类的实例就提供了这个接口。除了类实现的内容之外，对象还可以直接提供接口。

```
class MyInterface(zope.interface.Interface):
 x = zope.interface.Attribute("foo")
 def method1(self, x):
   pass
 def method2(self):
   pass

@zope.interface.implementer(MyInterface)
class MyClass:
 def method1(self, x):
   return x**2
 def method2(self):
   return "foo"
```

我们声明 MyClass 实现了 MyInterface。这意味着 MyClass 的实例提供了我的接口。

# ●静态方法

与实例方法不同，静态方法不绑定到对象。换句话说，静态方法不能访问和修改对象的状态。

此外，Python 不会将 cls 参数(或 self 参数)隐式传递给静态方法。因此，静态方法不能访问和修改类的状态。

在实践中，您使用静态方法来定义在类中有一些逻辑关系的实用方法或组[函数](https://www.pythontutorial.net/python-basics/python-functions/)。

要定义静态方法，可以使用@staticmethod 装饰器:

```
class className:
 @staticmethod
 def static_method_name(param_list):
   pass
```

要调用静态方法，可以使用以下语法:

className.static_method_name()

# ●魔术方法

python 中的魔法方法是一些特殊的方法，它们的名字以双下划线开始和结束。神奇的方法并不是设计给我们直接调用的，而是会在特定的时间被系统内部调用。

**__str__**

当相应类的实例被转换为字符串或用作 print()函数的参数时，将调用 __str__()方法。

```
class Segitiga:

 def __init__(self, a, t):
   self.alas = a
   self.tinggi = t

 def __str__(self):
   luas = 0.5 * self.alas * self.tinggi
   return f'segitiga (alas={self.alas} tinggi={self.tinggi} luas={luas})'

# create an instance
a = Segitiga(10, 10)
b = Segitiga(20, 10)
print(a)
print(b)
segitiga (alas=10 tinggi=10 luas=50.0)
segitiga (alas=20 tinggi=10 luas=100.0)
```

# ●操作员过载

操作符重载是一种技术，在这种技术中，我们将管理或定义一个类的行为，以及它将如何与各种操作符交互。

要查看 Python 的操作符重载，请启动 Python 终端并运行以下命令:

```
class Angka:
 def __init__(self, angka):
   self.angka = angka

 def __add__(self, objek):
   return self.angka + objek.angka

x1 = Angka(10)
x2 = Angka(20)

print(x1 + x2)

// result
30
```

# ●超级()

Python 还有一个 super()函数，它将使子类继承其父类的所有方法和属性:

```
class Student(Person):
 def __init__(self, fname, lname):
 super().__init__(fname, lname)
```

通过使用 super()函数，您不必使用父元素的名称，它将自动从其父元素继承方法和属性。

**添加属性**

```
class Student(Person):
 def __init__(self, fname, lname):
   super().__init__(fname, lname)
   self.graduationyear = 2019
```

在下面的示例中，2019 年应该是一个变量，并在创建学生对象时传递给学生类。

# ●多重继承

在 python 编程语言中，一个类可以继承多个不同的父类。继承多个类的能力称为多重继承。

我们简单地定义每个父类，用括号()括起来，用逗号(，)隔开。

```
class Parent1:
 pass

class Parent2:
 pass

class Turunan(Induk1, Induk2):
 pass
```

在上面的例子中，子类成为了 Parent1 类和 Parent2 类的“子”类。

# ●方法解析顺序

所以在多重或多层次继承的场景中，第一个被调用的属性/函数来自它自己的类。如果没有找到，就从第一个父类开始。如果没有找到，则从第二个父类开始，依此类推。

所以在多重或多层次继承的场景中，第一个被调用的属性/函数来自它自己的类。如果没有找到，就从第一个父类开始。如果没有找到，则从第二个父类开始，依此类推。

特别是对于多重继承，父类的顺序是从左到右的。所以派生的第一个类是最左边的类，然后是右边的类，依此类推。

此方法的顺序称为方法解析顺序(MRO)。