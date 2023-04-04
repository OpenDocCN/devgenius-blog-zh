# 使用 Java 8 Lambda 实现设计模式

> 原文：<https://blog.devgenius.io/implementing-design-patterns-using-java-8-lambda-c8a95ef66115?source=collection_archive---------6----------------------->

![](img/22f435494bbaee098184f743bf2aa8dc.png)

Java 8 引入了 lambda 表达式的概念，它是匿名函数，可用于实现函数接口(具有单一抽象方法的接口)。这允许使用更简洁和功能性的编程风格来实现设计模式。

例如，**策略模式**定义了一系列算法并支持它们的可互换性，可以使用 Java 8 中的 lambda 表达式实现，如下所示:

```
public interface SortStrategy {
    List<Integer> sort(List<Integer> list);
}

public class QuickSortStrategy implements SortStrategy {
    @Override
    public List<Integer> sort(List<Integer> list) {
        // Implementation of quick sort algorithm
    }
}

public class MergeSortStrategy implements SortStrategy {
    @Override
    public List<Integer> sort(List<Integer> list) {
        // Implementation of merge sort algorithm
    }
}

public class Sorter {
    private SortStrategy strategy;

    public Sorter(SortStrategy strategy) {
        this.strategy = strategy;
    }

    public void setStrategy(SortStrategy strategy) {
        this.strategy = strategy;
    }

    public List<Integer> sort(List<Integer> list) {
        return strategy.sort(list);
    }
}

// Usage:
Sorter sorter = new Sorter(new QuickSortStrategy());
List<Integer> sortedList = sorter.sort(unsortedList);

// Using lambda expressions:
Sorter sorter = new Sorter(list -> {
    // Implementation of quick sort algorithm using lambda expression
});
List<Integer> sortedList = sorter.sort(unsortedList);
```

在上面的例子中，`SortStrategy`接口及其实现(`QuickSortStrategy`和`MergeSortStrategy`)定义了不同的排序算法。`Sorter`类使用`SortStrategy`接口对整数列表进行排序。使用 lambda 表达式，排序算法可以内联实现，从而允许更简洁和功能性的编程风格。

**观察者模式**，用于在对象之间建立一对多的依赖关系，可以通过以下方式使用 lambda 表达式实现:

```
// Define the interface for the strategy
public interface Strategy {
    public int doOperation(int a, int b);
}

// Define the context that uses the strategy
public class Context {
    private Strategy strategy;

    public Context(Strategy strategy) {
        this.strategy = strategy;
    }

    public int executeStrategy(int a, int b) {
        return strategy.doOperation(a, b);
    }
}

// Use the context and lambda expressions to define the strategies
public class Main {
    public static void main(String[] args) {
        Context context = new Context((a, b) -> a + b);
        System.out.println(context.executeStrategy(3, 4)); // 7

        context = new Context((a, b) -> a - b);
        System.out.println(context.executeStrategy(3, 4)); // -1

        context = new Context((a, b) -> a * b);
        System.out.println(context.executeStrategy(3, 4)); // 12
    }
}
```

在这个例子中，`Strategy`接口定义了所有策略都必须实现的方法。`Context`类使用一个`Strategy`对象来执行特定的操作。在`main()`方法中，lambda 表达式用于定义不同的策略，然后传递给`Context`执行。这允许您在运行时轻松地在不同策略之间切换，使您的代码更加灵活和可重用。

```
// Create an observable sequence of data
Observable<Data> observable = Observable.create(emitter -> {
  while (!emitter.isDisposed()) {
    Data data = getNextData();
    emitter.onNext(data);
  }
});

// Subscribe to the observable sequence
observable.subscribe(data -> {
  // Handle the data as it is received
});
```

在这个例子中，我们使用一个 lambda 表达式来创建一个可观察的数据序列，该序列可以被程序中的其他组件订阅。lambda 表达式用于生成数据并将其发送给订阅的组件。这允许我们使用 Java 8 的 lambda 表达式以简洁和功能性的方式实现观察者模式。