# 鸟、鸡和企鹅:如何实现类层次结构

> 原文：<https://blog.devgenius.io/birds-chickens-and-penguins-how-to-implement-the-class-hierarchy-f2ef50ab80ef?source=collection_archive---------10----------------------->

## Python 中的面向对象范例、类层次结构、继承和覆盖

![](img/a6d8d9272da60a6fdc5d21dfda245b93.png)

伊恩·帕克在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄的照片

最近，有人问我实现类层次结构的最正确的方法是什么，使用了一个具体的例子:Bird 的超类，三个子类派生自这个超类:

*   **Generic_Bird** ，是后面要实现的通用鸟，
*   **鸡**，是一种飞行能力有限的鸟，
*   **企鹅**，根本不准飞。

我认为这将是一个有趣的话题，也是一个原始的案例研究，与 OOP 手册中常见的案例相比，可以用作教育材料。所以我实现了类的层次结构，现在我在这里，与你分享我的想法。

首先，让我们定义一下`**Bird**`类:

```
class Bird:

    __max_altitude__ = 0.3  # kilometers  
    __max_distance__ = 5000 # kilometers
    __ability_to_fly__ = True 

    def __init__(self):
        self.__max_altitude__ = Bird.__max_altitude__
        self.__max_distance__ = Bird.__max_distance__
        self.__ability_to_fly__ = Bird.__ability_to_fly__

    def introduction(self):
        print("\nI introduce myself:")
        print(f"I am an object of the '{type(self).__name__}' class")

        if self.__ability_to_fly__ == True:
            print("I can fly. Yippee!")
        else:
            print("I can't fly. What a pity...")

        print(f"I can reach the max altitude of {self.__max_altitude__ * 1000} meters e travel by air for {self.__max_distance__} km")

    def fly(self, altitude, distance):    # altitude and distance in km
        if (altitude <= self.__max_altitude__):
            if (distance <=self.__max_distance__):
                # If the input values are compliant, proceed
                self.__take_flight__(altitude)
                self.__travel_by_air__(distance)
                self.__land__()
            else:
                raise Exception("OutOfRange: the input value for 'altitude' exceeds the allowed range.")
        else:
            raise Exception("OutOfRange: the input value for 'altitude' exceeds the allowed range.")

    def __take_flight__(self, altitude):
        # the __take_flight__ method will be valid for all the birds with the ability to fly
        print(f"I take fly and reach {altitude*1000} meters altitude.")

    def __travel_by_air__(self, distance):
        # the __travel_by_air__ method will be valid for all the birds with the ability to fly
        print(f"I travel by air for {distance} km.")

    def __land__(self):
        # the __land__ method is valid for all sublcasses (I will not override it)
        print("I land.") 
```

在上面的代码中，我们定义了一个名为`**Bird**`的类，它代表一个通用的 bird。这个类有三个私有类常量:`**__max_altitude__**`、`**__max_distance__**`、`**__ability_to_fly__**`，分别代表一只鸟能达到的最大高度、能飞的最大距离(都用公里表示)、是否有飞行能力。

方法`**__init__()**`是 Python 中的一个特殊方法，它被称为**构造器**，它创建一个对象实例并初始化对象的内部状态。在`**Bird**`类的特定情况下，该方法将对象的`**__max_altitude__**`、`**__max_distance__**`和`**__ability_to_fly__**`属性的值设置为相应的类常量。

方法打印关于物体的信息，包括它的类名和它是否有飞行的能力。这是一个公共方法，因为它前面没有两个下划线。可以从类外部调用公共方法。

公共方法`**fly()**`有两个参数:`**altitude**`和`**distance**`，代表鸟要飞的高度和距离。如果`**altitude**`和`**distance**`的输入值在特定鸟的允许范围内，该方法调用`**__take_flight__()**`、`**__travel_by_air__()**`和`**__land__()**`方法。如果输入值不在允许的范围内，该方法将引发异常。

分别模拟鸟起飞、飞行和降落的`**__take_flight__()**`、`**__travel_by_air__()**`和`**__land__()**`私有方法。它们都是私有方法，因为它们前面有两个下划线。私有方法是只能在类内部调用，而不能从类外部调用的方法。**私有方法通常用于实现与类用户无关的类细节**。

现在我们已经定义了*超类*，让我们开始寻找*子类*。

第一个子类`**Generic_Bird**`没有任何额外的属性或方法。该类的行为类似于一个属性为默认值的`**Bird**`对象。

```
class Generic_Bird(Bird):
    def __init__(self):
        super().__init__()
```

现在让我们看看如何实现`**Chicken**`子类，尽管有飞行的能力，但不能执行适当的飞行，它将宁愿是一个跳远(1.5 米高，2 米远)。

`**Chicken**`重新定义了自己的私有类常量:`**__max_altitude__**`、`**__max_distance__**`(也用公里表示)和`**__ability_to_fly__**`。

虽然`**Chicken**`类可以从`**Bird,**` 继承`**__ability_to_fly__**`，因为它仍然被设置为值 **True，**我更喜欢再次声明它，以获得一个清晰易读的代码。

`**Chicken**`有自己的`**__init__()**`方法，将对象的`**__max_altitude__**`、`**__max_distance__**`和`**__ability_to_fly__**`属性设置为相应的`**Chicken**`类常量。

如上所述，`**Chicken**`类重新定义了它的方法:`**fly()**`、`**__take_flight__()**`和`**__travel_by_air__()**`方法，反映了一只鸡有限的飞行能力。`**__land__()**`方法不需要改变，只是简单的继承。

```
class Chicken(Bird):
    __max_altitude__ = 0.0015  # kilometers  
    __max_distance__ = 0.002 # kilometers
    __ability_to_fly__ = True 

    def __init__(self):
        super().__init__()
        self.__max_altitude__ = Chicken.__max_altitude__   
        self.__max_distance__ = Chicken.__max_distance__    
        self.__ability_to_fly__ = Chicken.__ability_to_fly__   

    def fly(self):        # override the fly method of Bird superclass 
        print("... Low-flying of the chicken:")
        self.__take_flight__()
        self.__travel_by_air__()
        self.__land__()

    def __take_flight__(self):
        print(f"... I can only jump {self.__max_altitude__*1000} meters height.")

    def __travel_by_air__(self):
        print(f"... I can only jump {self.__max_distance__*1000} meters long.")
```

最后，让我们来看看最可爱的:`**Penguin**`类。

让我们再次假设`**Penguin**`没有飞行能力。常量`**__ability_to_fly__**`被设置为**假**并且`**fly()**`方法被重新定义以告知它不能被执行。

```
class Penguin(Bird):
    __max_altitude__ = 0.0005 # Let's assume that Penguin can jump only 0.5 meters height
    __max_distance__ = 0 
    __ability_to_fly__ = False

    def __init__(self):    
        super().__init__()
        self.__max_altitude__ = Penguin.__max_altitude__ 
        self.__max_distance__ = Penguin.__max_distance__    
        self.__ability_to_fly__ = Penguin.__ability_to_fly__

    def fly(self):
        print("Fly: Action not allowed") 
```

现在，让我们实例化这些对象，看看它们的性能。

```
generic_Bird = Generic_Bird()
generic_Bird.introduction()
generic_Bird.fly(0.1, 2000)

chicken = Chicken()
chicken.introduction()
chicken.fly()

penguin = Penguin()
penguin.introduction()
penguin.fly()
```

这是生成的输出:

```
I introduce myself:
I am an object of the 'Generic_Bird' class
I can fly. Yippee!
I can reach the max altitude of 300.0 meters e travel by air for 5000 km
I take fly and reach 100.0 meters altitude.
I travel by air for 2000 km.
I land.

I introduce myself:
I am an object of the 'Chicken' class
I can fly. Yippee!
I can reach the max altitude of 1.5 meters e travel by air for 0.002 km
... Low-flying of the chicken:
... I can only jump 1.5 meters height.
... I can only jump 2.0 meters long.
I land.

I introduce myself:
I am an object of the 'Penguin' class
I can't fly. What a pity...
I can reach the max altitude of 0.5 meters e travel by air for 0 km
Fly: Action not allowed
```

和往常一样，并不只有一种编写代码的方法。在我看来，这种结构更整洁，更易于维护。我从编写这段代码中获得了很多乐趣。希望你也能享受阅读的乐趣！

如果你喜欢这篇文章，请鼓掌并分享！