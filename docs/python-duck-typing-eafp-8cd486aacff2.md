# 蟒蛇鸭打字& EAFP

> 原文：<https://blog.devgenius.io/python-duck-typing-eafp-8cd486aacff2?source=collection_archive---------12----------------------->

鸭子打字术语来源于一句谚语“如果它走路像鸭子，叫起来像鸭子，那么它一定是鸭子。”

Duck typing 是一个与动态类型相关的概念，在动态类型中，对象的类型或类没有它定义的方法重要。当您使用 duck 类型时，您根本不检查类型。相反，您检查给定方法或属性的存在。

在第一个例子中，我们没有使用 duck 类型化和检查类型。

```
class Duck:

    def quack(self):
        print('Quack, quack')

    def fly(self):
        print('Flap, Flap!')

class Person:

    def quack(self):
        print("I'm Quacking Like a Duck!")

    def fly(self):
        print("I'm Flapping my Arms!")

def quack_and_fly(thing):
    pass
    # Not Duck-Typed (Non-Pythonic)
    if isinstance(thing, Duck):
        thing.quack()
        thing.fly()
    else:
        print('This has to be a Duck!')

d = Duck()
quack_and_fly(d)
p = Person()
quack_and_fly(p)
Output-
Quack, quack
Flap, Flap!
This has to be a Duck!
```

在这个例子中，我们有函数——Duck()和 Person()，在函数 quack _ and _ fly()中，我们检查实例是否是 Duck()类型并执行操作。

在 Duck()函数对象的情况下，我们得到了一个庸医和苍蝇的方法，在 Person()对象的情况下，我们得到了一个类似这样的消息——这一定是一只鸭子。

所以检查类型和执行操作不是鸭子打字。

下面是一个例子，我们将相同的代码转换成 Duck typing。

```
class Duck:

    def quack(self):
        print('Quack, quack')

    def fly(self):
        print('Flap, Flap!')

class Person:

    def quack(self):
        print("I'm Quacking Like a Duck!")

    def fly(self):
        print("I'm Flapping my Arms!")

def quack_and_fly(thing):
    pass
    try:
        thing.quack()
        thing.fly()
    except AttributeError as e:
        print(e)

d = Duck()
quack_and_fly(d)
p = Person()
quack_and_fly(p)
Output-
Quack, quack
Flap, Flap!
I'm Quacking Like a Duck!
I'm Flapping my Arms!
```

在这种情况下，我们首先执行操作，如果它没有出现在方法中，那么我们将进行同样的处理。

这被称为 EAFP，或者在 Python 中请求原谅比请求许可更容易。它建议你马上去做你期望的工作。如果它不起作用并且发生了异常，那么只需捕捉异常并适当地处理它。

为了理解这一点，首先查看下面的例子，其中 **LBYL** 代表三思而后行。这是一种传统的编程方法，被认为是非 Pythonic 式的工作方式。

```
class Duck:

    def quack(self):
        print('Quack, quack')

    def fly(self):
        print('Flap, Flap!')

class Person:

    def quack(self):
        print("I'm Quacking Like a Duck!")

    def fly(self):
        print("I'm Flapping my Arms!")

def quack_and_fly(thing):
    pass
    # LBYL (Non-Pythonic)
    if hasattr(thing, 'quack'):
        if callable(thing.quack):
            thing.quack()

    if hasattr(thing, 'fly'):
        if callable(thing.fly):
            thing.fly()

d = Duck()
quack_and_fly(d)
p = Person()
quack_and_fly(p)
Output-
Quack, quack
Flap, Flap!
I'm Quacking Like a Duck!
I'm Flapping my Arms!
```

在这个例子中，首先，我们检查对象是否具有特定的属性，然后执行操作。这种解决问题的方法被称为 LBYL，因为它依赖于在执行期望的动作之前检查先决条件。

在 EAFP，我们首先执行操作，如果出现错误，我们将进行同样的处理。

```
class Duck:

    def quack(self):
        print('Quack, quack')

    def fly(self):
        print('Flap, Flap!')

class Person:

    def quack(self):
        print("I'm Quacking Like a Duck!")

    def fly(self):
        print("I'm Flapping my Arms!")

def quack_and_fly(thing):
    pass
    try:
        thing.quack()
        thing.fly()
        thing.dance()
    except AttributeError as e:
        print("Attribute Error !!")

d = Duck()
quack_and_fly(d)
p = Person()
quack_and_fly(p)
Output-
Quack, quack
Flap, Flap!
Attribute Error !!
I'm Quacking Like a Duck!
I'm Flapping my Arms!
Attribute Error !!
```

在这里，我调用了一个额外的方法 Dance()，这两个函数中都不存在，所以会出现 except block 和 printing 异常。