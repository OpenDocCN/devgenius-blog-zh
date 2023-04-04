# 访问私有方法？强大的 Java 黑魔法

> 原文：<https://blog.devgenius.io/accessing-private-methods-powerful-black-magic-with-java-f7bc0e8972fb?source=collection_archive---------4----------------------->

## 用反射访问私有字段和调用私有类

# 什么是反射？

Reflect 是一个鲜为人知的 java 库，它允许您编辑字段和调用类的给定对象的方法。也许你在想

> "这有什么酷的，我以为我已经可以调用类方法并引用它们的变量了？"

如果我告诉你这个库也允许你访问私有的字段和方法呢😲！！！

是的，你可以在一个类之外访问这个类的私有方法！！！这就是为什么我认为它是黑魔法🧙‍♂️.

![](img/0b9ca6379d89ac43ed550e2013fc04a2.png)

约书亚·牛顿在 [unspalsh](https://unsplash.com/) 上的照片

# 你什么时候使用反射？

好吧，但是你为什么要用这个呢？如果你想访问一个私有字段，你可以改变方法存根，使之成为公共的。是的，这是真的，但如果你不想这样做，但仍然想暂时访问它以外的类呢？也许您会在一些 Junit 测试中使用它，这样您就可以直接测试私有 helper 方法，而不是通过使用它们的公开的公共方法进行间接测试。

# 一个例子

## 神秘课堂

我们得到一个神秘的类，它有一个私有字段、一个公共方法和一个私有方法。现在我们将在课堂之外调用它们🤯。

## 反映示例程序

## 输出:

```
Executing public method 10
Changed field name
Executing private method
```

要使用 reflect 调用方法和字段，我们需要遵循几个步骤…

1.  使用对象上的 **getClass()** 获取类。
2.  使用**getDeclaredMethod()/getDeclaredField()**获取方法/字段，并传入名称和它需要的参数类型(如果是方法的话)。
3.  如果是私有的字段或方法， **setAccessible(true)** 将其临时公开。
4.  使用字段或方法，使用: **Invoke()** 作为传入对象的方法，使用任何 params 或 **set()** 和 **get()** 作为字段。

恭喜你！您已经开始了学习 java 库的黑暗魔法的旅程——负责任地使用这些知识……或者不使用😝。