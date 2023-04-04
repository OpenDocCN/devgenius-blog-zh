# 有效的 Java:抛出适合抽象的异常

> 原文：<https://blog.devgenius.io/effective-java-throw-exceptions-appropriate-to-the-abstraction-d085ca1ccba1?source=collection_archive---------3----------------------->

![](img/09e9694d30d9ba879bd3a019c15da4b3.png)

照片由[韦德·兰伯特](https://unsplash.com/@wade_lambert?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄

Effective Java*的大部分内容都集中在构建一个干净、可理解的 API，以及这是一个伟大库的基础。一个类的 API 的一部分是任何异常，它可能抛出检查(在那里它成为签名的一部分)或未检查的堆栈。作为代码的编写者，我们有责任确保这个 API 没有任何意外或令人震惊的地方。发生这种情况的方式之一是公开一个对我们正在编写的类没有意义的异常。*

不匹配异常的一个潜在例子是，如果您请求将两个数字相加，而方法抛出了一个`IOException`。如果这样一个方法抛出了一个`IOException`，我会有很多疑问，但问题仍然是抛出的异常与抽象不匹配。通过抛出这个低级别的异常，您向调用者公开了实现细节，这些实现细节将来可能会改变，但现在它是您的 API 的一部分，所以很难改变。那么有什么方法可以解释这一点呢？

解决这个问题的主要方法是进行所谓的*异常转换*。异常转换是指捕获较低级别的异常，并将其包装在与您正在处理的抽象相匹配的较高级别的异常中。我们可以在`AbstractSequentialList`类中找到这样的例子。

```
public E get(int index) {
  ListIterator<E> = listIterator(index);
  try {
    return i.next();
  } catch (NoSuchElementException e) {
    throw new IndexOutOfBoundsException("Index: " + index);
  }
}
```

在这种情况下，它正在实现的接口甚至告诉它`IndexOutOfBoundsException`是应该从这个方法抛出的异常。

异常转换的一种特殊形式是将较低级别的异常包装在较高级别的异常中，同时将较低级别的异常作为*原因*传递给较高级别的异常。许多方法公开了这个原因字段，并将它传递给 Throwable 类。如果一个特定的方法在其构造中没有暴露原因，你甚至可以在异常上调用`Throwable`的`initCause`。Throwable 提供了一个`getCause`方法，然后可以在堆栈的更高层使用该方法来检索底层问题。更重要的是，这个原因是通过堆栈跟踪暴露出来的，这对问题的调试有很大的帮助。这确实间接向调用方法公开了较低级别的细节。然而，它并不直接这样做，因此它不会强制调用者处理低级别的异常，相反，他们仍然可以处理高级别的异常，而不用担心低级别的实现细节。

最容易处理的异常是不会被抛出的异常。我们应该在所有代码中努力不抛出可避免的异常。有效的 Java 甚至建议我们有时可以绕过异常，简单地记录它们并继续前进。不过，我要警告不要使用这种模式。如果我们只是在异常情况下抛出异常，那么调用者可能需要知道发生了什么事情，简单地对调用者隐藏可能会有问题。

永远记住，任何抛出的异常都是 API 的一部分。因此，您应该考虑向调用者抛出什么异常。确保这些异常适合您的代码正在处理的抽象。如果抛出了较低级别的异常，使用*异常转换*模式将该异常转换为适合您的抽象的较高级别的异常类型。此外，考虑在这种被捕获和重新抛出的异常中使用 *cause* 字段。通过这样做，您可以更容易地阅读、维护和修改代码。