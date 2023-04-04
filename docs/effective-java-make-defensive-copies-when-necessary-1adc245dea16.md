# 有效的 Java！必要时制作防御性副本

> 原文：<https://blog.devgenius.io/effective-java-make-defensive-copies-when-necessary-1adc245dea16?source=collection_archive---------1----------------------->

Java 作为一种语言的好处之一是，它可以提供一些其他低级语言无法提供的安全性。避免诸如缓冲区溢出、放错位置的指针以及 C 和 C++等语言的其他内存损坏问题。话虽如此，Java 也不能幸免于安全问题，我们仍然必须保持警惕。

让我们来看一个例子，看看这会给我们带来什么样的伤害。让我们看看一个名为`Period`的类，它努力成为一个不可变的类，保存一个开始和结束的`Date`对象。

```
public final class Period {
  private final Date start;
  private final Date end; public Period(Date start, Date end) {
    if (start.compareTo(end) > 0) {
      throw new IllegalArgumentException("Start is after end");
    }
    this.start = start;
    this.end = end;
  } public Date start() {
    return start;
  } public Date end() {
    return end;
  }
}
```

乍一看，这似乎是一个合理的实现，并且它加强了 start before end 的不变性。尽管如此，打破这个不变量还是很容易的。下面的代码可以做到这一点:

```
Date start = new Date()
Date end = new Date();
Period p = new Period(start, end);
end.setYear(50);
```

代码的最后一行修改了我们的`Period`对象的内部。那么我们有什么方法可以解决这个问题呢？在 Java 8 及更高版本中，最好的方式可能是使用在该版本中引入的改进的日期对象之一，比如`Instant`或`LocalDateTime`。那将是我们最好的选择。如果我们没有这种选择呢？让我们来看一个构造函数，它可以帮助我们解决问题。

```
public Period(Date start, Date end) {
    this.start = new Date(start.getTime());
    this.end = new Date(end.getTime());
    if (start.compareTo(end) > 0) {
      throw new IllegalArgumentException("Start is after end");
    }
  }
```

有了这段代码，前面的攻击就被挫败了。值得注意的是，我们在构造函数中直接进行复制，而不读取值，以防止值在检查和存储之间发生变化的计时攻击。最后，我们不使用`clone`方法来避免通过覆盖`clone`方法并给它一个不安全的实现来规避隐含的安全性。

还有最后一个地方我们需要保护，那就是我们暴露的 getters。通过它们是如何实现的，对象的接收者可以改变内部状态。

```
public Date start() {
    return new Date(start.getTime());
  } public Date end() {
    return new Date(end.getTime());
  }
```

通过这个简单的改变，我们现在真正地使这个类不可变了。

那么制作这些防御性副本有什么坏处呢？主要问题可能是由此带来的内存压力。如果这些副本是在一个紧凑的大循环中制作的，这会对代码产生重大影响。这不是避免使用防御性副本的理由，但这是需要记住的事情。

这一切真的值得吗？人们真的这么想得到我们的代码吗？像 Effective Java 的许多部分一样，我发现防止攻击的防御性副本主要适用于库作者，而不适用于我们这些编写内部消耗代码的人。除了防范攻击之外，防御性副本还能防止我们内部状态的意外变化。当我们没有制作防御性副本时，很容易意外地改变这种状态，并且这些错误非常难以追踪。

总之，防御性副本可以保护我们的代码免受不良行为者以及我们自己的错误的影响，而无需太多额外的代码和工作。