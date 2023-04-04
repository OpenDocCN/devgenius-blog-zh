# C++构造函数析构函数

> 原文：<https://blog.devgenius.io/c-constructor-destructor-d1ba42e03f33?source=collection_archive---------10----------------------->

![](img/97bbba197edd8dad4a9bc94134b1e856.png)

Garvin St. Villier 摄

在本文中，我们将探讨面向对象编程中构造函数和析构函数的概念。这些特殊的方法分别用于初始化和清理对象，在确保程序正常运行方面起着至关重要的作用。我们将深入研究构造函数和析构函数是如何工作的，并提供它们在 Java 和 C++等常见编程语言中的用法示例。到本文结束时，您将对这些重要的概念有一个基本的了解，并且能够自信地将它们结合到您自己的程序中。

**什么是构造函数？**

在面向对象编程中，构造函数是一种特殊的方法，在创建对象时调用。它用于初始化对象并为其属性分配内存。构造函数通常与类同名，并且没有返回类型。

**构造函数可以是公共的吗？**

是的，构造函数可以被声明为 public。这意味着可以从类外部调用构造函数，从而允许其他代码创建该类的实例。当类打算用作独立对象而不是用作继承的基类时，通常使用公共构造函数。

```
#include<iostream>
using namespace std;

class A{
    public:
        A(){
            cout << "constructor called";
        }
};

int main() {
    A a;
    return 0;
}
```

**构造函数可以私有吗？**

是的，构造函数可以被声明为私有的。这意味着构造函数只能从类本身内部调用。当类不打算用作独立对象而是用作继承的基类时，通常使用私有构造函数。在这种情况下，派生类可以使用`base`关键字调用基类的私有构造函数来创建基类的实例。这允许派生类控制基类如何以及何时被实例化，并且可以用于实现单例模式，其中只允许创建一个类的一个实例。

```
#include <iostream>
using namespace std;

class Singleton {
   private:
   static Singleton *instance;
   int data;

   Singleton() {
      data = 0;
   }

   public:
   static Singleton *getInstance() {
      if (!instance)
      instance = new Singleton;
      return instance;
   }

   int getData() {
      return this -> data;
   }
   void setData(int data) {
      this -> data = data;
   }
};

Singleton *Singleton::instance = 0;

int main(){
   Singleton *s = s->getInstance();
   cout << s->getData() << endl;
   s->setData(100);
   cout << s->getData() << endl;
   return 0;
}
```

我们还可以使用 friend 关键字声明一个私有构造函数。friend 关键字允许函数或类访问类的私有成员。在私有构造函数的情况下，这意味着友元函数或类可以调用私有构造函数，即使该类之外的其他代码无法访问它。

```
#include<iostream>
using namespace std;

class A{
    private:
        A(){
            cout << "private constructor called";
        }
    friend int main();
};

int main() {
    A a;
    return 0;
}
```

**构造函数可以被保护吗？**

是的，在 C++中，构造函数可以被标记为 protected。受保护的构造函数只能由类本身和从它继承的任何类访问。这意味着类及其派生类之外的代码不能直接调用受保护的构造函数。

```
#include <iostream>
using namespace std;

class Base {
    protected:
       Base() {
           cout << "Protected constructor called";
       }
};

class Derived: public Base {
    public:
       Derived() : Base() {}
};

int main(){
   Derived d;
   return 0;
}
```

**什么是析构函数？**

析构函数是一个类的特殊成员函数，当该类的一个对象被销毁或超出范围时会被调用。析构函数与类同名，但前面有一个波浪号(~)。

例如，如果一个类被命名为“Foo”，那么这个类的析构函数将被声明为“~Foo()”

析构函数的主要目的是释放类在其生存期内分配的所有资源。这可能包括内存、文件句柄和其他资源。析构函数是由语言运行库自动调用的，所以您不需要在代码中直接调用它们。

析构函数可以是公共的吗？

是的，析构函数在 C++中可以被标记为 public。任何引用该类的对象的代码都可以访问公共析构函数，并且可以调用它来销毁该对象。

但是，需要注意的是，在大多数情况下，没有必要直接调用析构函数。当对象超出范围或被销毁时，语言运行库将自动调用析构函数。

一般来说，只有在有特定原因的情况下，才应该将析构函数设置为公共的，比如需要为对象实现自定义的清理逻辑。在大多数情况下，最好将析构函数保留为默认值(它是私有的)，并让语言运行时在对象被销毁时处理其自动调用。

```
#include <iostream>
using namespace std;

class A{
    public:
        ~A(){
            cout << "destructor is called";
        }
};

int main(){
   A a;
   return 0;
}
```

**析构函数可以是私有的吗？**

是的，析构函数在 C++中可以被标记为私有的。私有析构函数只能由类本身访问，不能由类外部的代码调用。

在大多数情况下，最好将析构函数保留为私有，并让语言运行时在对象被销毁时处理其自动调用。这确保了只有在安全的情况下才调用析构函数，并且外部代码不能干扰对象的生存期。

通常没有必要将析构函数设置为私有的，除非有特殊的原因，例如需要防止外部代码调用析构函数并干扰对象的生存期。

**析构函数能被保护吗？**

是的，析构函数在 C++中可以被标记为 protected。受保护的析构函数只能由类本身和从它继承的任何类访问。这意味着类及其派生类之外的代码不能直接调用受保护的析构函数。

在大多数情况下，最好将析构函数保留为私有，并让语言运行时在对象被销毁时处理其自动调用。这确保了只有在安全的情况下才调用析构函数，并且外部代码不能干扰对象的生存期。

只有在有特定原因的情况下，才应该保护析构函数，例如，如果需要为对象及其派生类实现自定义清理逻辑。在大多数情况下，最好将析构函数保留为默认值(它是私有的)，并让语言运行时在对象被销毁时处理其自动调用。

这是关于 c++的基础，构造函数和析构函数是一个很大的主题，不可能在一篇文章中完成。

亲自加入我的 [**不和谐服务器与我互动。**](https://discord.gg/UUXs24qtRz)

*如果你觉得这篇文章很有帮助，记得去*👉 ***跟着*******拍手👏*** *和* ***合用*** 👐和你的朋友一起吧！*