# Java 中的重载

> 原文：<https://blog.devgenius.io/overloading-in-java-a256401970d7?source=collection_archive---------25----------------------->

![](img/2b59d5695bb812d9d656903c936e781b.png)

马克西米利安·魏斯贝克尔在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上的照片

想象你自己在执行“谈话”的功能，在这个功能中，你要讲述一个关于你一天是如何度过的故事。当你和一个陌生人说话时，你会很快忽略一些细节(参数)。但是现在让我们想象一下，你正在向一个你爱的人讲述你的一天是如何度过的，你会比向陌生人讲述时对细节(参数)更透彻。在这里，您只是执行了相同的功能“交谈”,但具有不同的参数“详细”和“非详细”。这里的大脑是编译器，它知道在给定必要参数的情况下讲什么故事。这就是重载背后的概念。

现在谈谈技术定义，重载是 Java 中的一个特性，它允许具有相同名称但不同方法签名的方法具有灵活性；其中签名可以因输入参数的数量或输入参数的类型或两者而不同。重载也称为静态多态。重载有助于解决必须用不同的名称编写方法来解决同一问题的问题。在本文中，我们将看到一个通过重载构造函数和方法来解决问题的典型例子。

你可能想知道，如果两个方法有相同的名字，程序如何知道调用哪个方法。给定必要的参数，编译器知道选择哪个方法，因为每个方法都有不同的方法签名。

```
boolean updateProfile (int newId){}
boolean updateProfile (int newId, char genderId){}
boolean updateProfile (int charId, int newid){}
```

若要重载方法，必须遵循以下规则:

1.  这些方法必须具有相同的名称。
2.  参数列表必须改变(即参数或参数类型或两者都必须改变)。
3.  仅更改返回类型并不能使其适合重载。
4.  更改参数的顺序对于重载是有效的。

在本文中，我们将浏览构造函数和方法中重载的例子。在我们讨论重载的例子之前，有必要理解为什么重载是有益的。

> **超载的好处**

1.  它有助于你编写干净的代码。
2.  它们可以在构造函数上实现，允许用不同的方法初始化一个类的对象。
3.  重载增加了程序的可读性

> **动作中的过载**

假设我们有一个名为 User 的类，其中有两种类型的用户，即学生和教师，他们都有不同的注册屏幕。学生注册屏幕不要求工资信息，但教师注册屏幕要求工资。因此，在这种情况下，我们可以使用构造函数重载，下面是如何做。

```
class User{
    int id;
    String name;
    int salary; User(int userId, String userName){
        id = userId;
        name = userName;
    }

    User(int userId, String userName, int userSalary){
        id = userId;
        name = userName;
        salary = userSalary;
    }
}
```

从上面的代码中，我们有两个名为 User 的构造函数。当一个名叫 John 的学生注册时，我们可以通过调用第一个构造函数来创建一个用户对象。

```
User student = new User(101, "John");
```

如果一个名为 George 的教师注册了，我们就通过调用第二个构造函数来创建一个用户对象。

```
User instructor = new User(102, "George", 40000);
```

上面的例子是在 Java 中如何执行构造函数重载的典型例子。

现在我们已经定义了构造函数，让我们向类中添加一些方法。我们将创建一个方法来更新用户信息，并因此重载该方法来查看方法重载是如何完成的。

该类引入了两个具有不同参数的新方法。一种方法只更新用户的姓名，而另一种方法更新用户的姓名和薪水。

我们创建另一个名为 UserTest 的类，调用构造函数和方法来查看工作中的重载。

这段代码的结果显示如下:

```
Name of the Student User John
Name of the Instructor User George
Salary of the Instructor User 40000
Updated Student Name Benjamin 
Updated instructor Name Chris 
Updated instructor Salary 100000
```

因此，您可以看到 Java 中重载的美妙之处，因此，为了上面列出的好处，建议将重载的概念引入到您的 Java 程序中。