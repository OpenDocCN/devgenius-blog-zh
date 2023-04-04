# 有效的 Java:避免过度同步

> 原文：<https://blog.devgenius.io/effective-java-avoid-excessive-synchronization-3eb32734a1fb?source=collection_archive---------0----------------------->

![](img/f2c10ea49c052e3538b299d862372967.png)

照片由 [Iwona Castiello d'Antonio](https://unsplash.com/@aquadrata?utm_source=medium&utm_medium=referral) 在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 拍摄

鉴于在 *Effective Java* 中讨论的最后一项涵盖了我们需要使用同步的情况，本章涵盖了对过度同步的关注。缺乏同步会导致活性问题和安全问题，而过度同步会导致性能降低、死锁和不确定性。

要遵循的一个重要规则是始终保持对你的`synchronized`块的控制。这意味着在一个`synchronized`块中不要将控制权交给外部代码。这种控制权的传递可能来自于调用方法，这些方法旨在覆盖或调用客户端提供的函数。原因是我们无法控制这些区块中发生的事情，可能会导致我们的程序出现严重问题。

让我们考虑一个`Set`的例子，当项目被添加到`Set`时，它向客户端提供通知。这是一个简化的、不完整的实现，但它功能齐全，具有指导意义。

```
public class Observable<E> extends ForwardingSet<E> {
  public ObservableSet(Set<E> set) {
    super(set);
  } private final List<SetObservable<E>> observers = new ArrayList<>(); public void addObserver(SetObserver<E> observer) {
    synchronized(observers) {
      observers.add(observer);
    }
  } public boolean removeObserver(SetObserver<E> observer) {
    synchronized(observers) {
      return observers.remove(observer);
    }
  } private void notifyElementAdded(E element) {
    synchronized(observers) {
      for (SetObserver<E> observer : observers) {
        observer.added(this, element);
      }
    }
  } @Override 
  public boolean add(E element) {
    boolean added = super.add(element);
    if (added) {
      notifyElementAdded(element);
    }
    return added;
  } @Override 
  public boolean addAll(Collection<? extends E> c) {
    boolean result = false;
    for (E element : c) {
      result |= add(element);
    }
    return result;
  }
}
```

这不是一个超级小的代码量，而是类的概念，即客户端可以通过提供一个观察者来订阅对集合的更改。让我们快速看一下`SetObserver`界面。

```
@FunctionalInterface
public interface SetObserver<E> {
  void added(ObservableSet<E> set, E element);
}
```

通过提供类似于:

```
set.addObserver((s, e) -> System.out.println(e));
```

我们可以输出添加到集合中的所有元素。这很好。让我们看一些更高级的东西。让我们来看一个尝试，让一个观察器输出每个元素，然后将自己作为观察器移除。

```
set.addObserver(new SetObserver<>() {
  public void added(ObservableSet<Integer> s, Integer e) {
    System.out.println(e);
    if (e == 23) {
      s.removeObserver(this);
    }
  }
});
```

想象一下，将 1 到 100 号元素添加到这个集合中。我们可能会想象它会输出 1–23，然后将自己作为观察者移除。实际发生的情况是，我们有 1–23 个输出，然后抛出一个`ConcurrentModificationException`。发生这种情况的原因是我们的`Set`代码在这一点上循环了观察器，然后我们请求取出观察器。尽管`synchronized`块保护观察者集合不被更改，但它并不阻止它被循环。

让我们看一个不同的应用程序，它说明了您可能遇到的不同问题。让我们举一个例子，不管出于什么原因，观察者决定使用一个后台线程来尝试将自己从观察者列表中移除，可能错误地认为这可能有助于尝试绕过`ConcurrentModificationException`问题。

```
set.addObserver(new SetObserver<>() {
  public void added(ObservableSet<Integer> s, Integer e) {
    System.out.println(e);
    if (e == 23) {
      ExecutorService executor = Executors.newSingleThreadExecutor();
      try {
        executor.submit(() -> s.removeObserver(this)).get();
      } catch (Exception e) {
        throw new AssertionError(e);
      } finally {
        executor.shutdown();
      }
    }
  }
});
```

这个程序对于它试图做的事情来说过于复杂，并且没有必要使用线程。那么在这个程序中会发生什么呢？当我们试图运行这个程序时，我们会陷入死锁。这是因为当后台线程调用`removeObserver`时，它在`synchronized`块等待轮到它执行，而`synchronized`块正在等待上面的`added`方法完成。虽然这个例子是这个问题的一个虚构实例，但是如果您不小心的话，这些事情可能会在生产代码中发生。

一件有趣的事情是，为什么第一个例子被允许通过`synchronized`块，而第二个例子在`synchronized`块等待？原因是`synchronized`锁是*重入*。这意味着，如果一个特定的线程获得了锁，它就被允许进入由同一个锁保护的任何其他临界区。可重入锁使多线程编程变得更简单，但是会导致活跃度和安全性问题。

那么，在一个`synchronized`块中，放弃程序执行控制的替代方法是什么呢？简单地说，最小化您的同步部分。这不仅有利于安全，也有利于性能。所以让我们来看一下临界区最小化的函数。

```
private void notifyElementAdded(E element) {
  List<SetObserver<E>> snapshot = null;
  synchronized(observers) {
    snapshot = new ArrayList<>(observers);
  }
  for (SetObserver<E> observer : snapshot) {
    observer.added(this, element);
  }
}
```

这段代码做的是复制我们的观察者列表，然后对该快照进行操作。这使得 synchronized 块非常短，不会将执行控制传递给 synchronized 块中的外部方法，从而使程序更加安全。

有一种更简单的方法来解决这个问题。某些数据类型是专门设计用于并发场景的。这些集合之一是`CopyOnWriteArrayList`。这个`List`实现正如它所说的那样。每次写入时，该实现都会对其内容进行完整复制。因为内部数据不会被修改，所以在多线程环境中使用是安全的。这样做的代价是写入开销很大，而读取和迭代非常快，而且没有同步。在错误的情况下，这将是一个可怕的选择，但在其他情况下，如我们的观察者的情况下，这可能是一个很好的选择，因为我们很少写，并不断迭代的内容和阅读。让我们看看这个变化后我们的类会是什么样子。

```
private final List<SetObserver<E>> observers = new CopyOnWriteArrayList<>();public void addObserver(SetObserver<E> observer) {
  observers.add(observer);
}public boolean removeObserver(SetObserver<E> observer) {
  observers.remove(observer);
}private void notifyElementAdded(E element) {
  for (SetObserver<E> observer : observers) {
    observer.added(this, element);
  }
}
```

这段代码更容易推理，也更简单，同时仍然是线程安全的。

与本主题相关的最后一个讨论主题是我们在构建可变类时面临的决定。当涉及到线程安全时，我们有两种选择，要么要求在类外部提供同步，要么在类内部添加线程安全。只有当您可以通过内部同步实现更高的并发性时，才应该使用内部同步。没有什么是没有代价的。许多早期的 Java 核心类在内部同步方面出错，因此即使在单线程用例中使用，也要付出同步代价。为了解决这个问题，已经建立了许多现代的替代方案来取代最初的线程安全实现。例如用`StringBuilder`代替`StringBuffer`，用`HashMap`代替`Hashtable`，用`ThreadLocalRandom`代替`Random`。如果有疑问，请遵循这些最新的类所建议的指导，不要在类内同步，不要将类记录为线程安全的。

如果你的类同步是有意义的，那么你将会走上一条更复杂的道路。编写高性能且安全的线程安全代码可能很困难。促进它的模式和实践超出了这篇博客的范围，但是有许多很好的资源可以学习。

综上所述，不要从临界区内调用提供的函数。这样做的话，您的代码将面临死锁和数据损坏。保持你的关键部分小。使用线程安全的数据类型是向代码中添加线程安全的一种有用方法。当构建一个具有可变性的类时，考虑它是否应该提供同步。