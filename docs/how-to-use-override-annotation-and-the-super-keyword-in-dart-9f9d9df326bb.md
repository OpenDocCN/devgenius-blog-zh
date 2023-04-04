# 如何在 Dart 中使用 Override 注释和 super 关键字。

> 原文：<https://blog.devgenius.io/how-to-use-override-annotation-and-the-super-keyword-in-dart-9f9d9df326bb?source=collection_archive---------1----------------------->

## Dart 为您提供了额外的功能，使您的代码更加高效。

![](img/d5f1a464e496c8cd03205b4a1cf5d6aa.png)

埃米尔·佩龙在 [Unsplash](https://unsplash.com/s/photos/coding?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 上的照片

[**Dart**](https://dart.dev/) 是一种开源的面向对象编程语言，如果你熟悉 OOP 概念，Dart 将是一种学习和构建 [**Flutter**](https://flutter.dev/) 应用程序的绝佳语言。如果你用过 Java 等类似的编程语言，那么 Dart 对你来说并不难。

这是一个简短的教程，在这里，我将讨论如何在 Dart 编程语言中使用**覆盖注释**，以及如何在 Dart 程序中使用 **super** 关键字。

# 覆盖注释

让我们通过下面的例子来理解它们。

```
void main() { WebDeveloper webDev = WebDeveloper();
  print(webDev.location);
  print(webDev.salary); webDev.develop();}class Developer {
  String location = 'Colombo';
  int salary = 33000; void develop() {
    print('develops an applications');
  }
}class WebDeveloper extends Developer {}
```

这里，我们有`Developer`超类和`WebDeveloper`子类，子类可以访问其超类的所有属性和方法。因此，当您运行该命令时，`webDev` 对象将打印出`Developer` 类中的所有值。同样，当我们需要在`WebDeveloper`类中构建我们自己的`develop()`方法时，我们可以使用`override`注释。

让我们看看下面的代码片段。

```
void main() {
  WebDeveloper webDev = WebDeveloper();
  print(webDev.location);
  print(webDev.salary);
  webDev.develop();
}class Developer {
  String location = 'Colombo';
  int salary = 33000; void develop() {
    print('develops an applications');
  }
}class WebDeveloper extends Developer {
  @override
  void develop() {
    print('develops web applications');
  }
}
```

当您运行这段代码时，它将打印出一个字符串`develops web applications`而不是`develops an applications`。因此，这个`override annotation`有助于覆盖父类中的`develop()`方法。

# 超级关键词

在 Dart 中，我们可以使用`super`关键字向父类方法添加新值。让我们创建另一个名为`MobileDeveloper`的子类，看看如何使用 super 关键字向父类`develop()`方法添加新值。

让我们看看下面的代码片段。

```
void main() { //   WebDeveloper webDev = WebDeveloper();
  //   print(webDev.location);
  //   print(webDev.salary);
  //   webDev.develop(); MobileDeveloper mobDev = MobileDeveloper('ios'); mobDev.develop();
} class Developer {
  String location = 'Colombo';
  int salary = 33000; void develop() {
    print('develops an applications');
  }
}class WebDeveloper extends Developer {
  @override
  void develop() {
    print('develops web applications');
  }
}class MobileDeveloper extends Developer {
  String operatingSystem;
  MobileDeveloper(String os) {
    operatingSystem = os;
  }

  @override
  void develop() {
    super.develop();
    print('applications will work on $operatingSystem');
   }
}
```

输出将是:

```
develops an applicationsapplications will work on ios
```

如前所述，super 关键字可用于向父类方法添加新值。那么该方法将输出这两个值。

# 结论

恭喜你！您对如何在 Dart 编程语言中使用`override annotation` 和`super`关键字有了基本的了解。这实际上是一个初学者友好的教程，适合那些喜欢学习 Dart 来构建漂亮的颤振应用程序的人。

感谢阅读。我希望本文中的信息对您有用。