# 数据结构练习:使集合对 Java For Each 循环可用

> 原文：<https://blog.devgenius.io/data-structures-exercise-making-a-collection-available-to-javas-for-each-loops-126e6cf8a877?source=collection_archive---------1----------------------->

![](img/8ad5631acb3e2a94db1fcefe3fa0b8bc.png)

照片由[诺亚根岸](https://unsplash.com/@noah_negishi?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄

关于`java.util.ArrayList`的一个好处是，您可以使用一个“增强的”For 循环(称为“For Each”循环)遍历列表的内容。例如，鉴于

```
ArrayList<Transaction> trx = account.getTransactions();
```

而不是不得不写

```
CurrencyAmount bal = ZERO_DOLLARS;
for (i = 0; i < trx.size(); i++) {
    Transaction curr = trx.get(i);
    CurrencyAmount amt = curr.getAmount();
    bal = bal.plus(amt);
}
```

我们可以写

```
CurrencyAmount bal = ZERO_DOLLARS;
for (**Transaction trs : trx**) {
    CurrencyAmount amt = **trs**.getAmount();
    bal = bal.plus(amt);
}
```

您的集成开发环境(IDE)可能会为更少的代码行建议一个 lambda。

这不是一个只有在`java.util`中的职业才有的魔法能力。您可以简单地通过实现`Iterable<E>`接口赋予自己编写的集合类同样的能力。

嗯，可能没那么简单，因为你还得实现`Iterator<E>`接口。这可能看起来令人生畏，当我们考虑到我们可能会遇到职业角色转换问题时就更是如此。

每当任何编程任务看起来令人生畏时，测试驱动开发(TDD)通常会帮助我们克服阻碍我们开始任务的任何恐惧。

对于未检查的强制转换问题，我们可能会想出一个非常不优雅的解决方案，但是如果我们通过 TDD 得到这个不优雅的解决方案，我们可以自由地快速尝试潜在的更优雅的解决方案，并检查它们是否按下按钮就能工作。

如果我们认为可能更好的解决方案实际上不起作用，我们会知道，因为我们的一个或多个测试会失败。在这种情况下，我们可以回到不优雅的解决方案。

在这个练习中，我们需要一些可以实现`Iterable<E>`但还没有实现的集合类。我在之前的文章中描述的[中的数组支持的集合练习是这个练习的一个很好的候选。](/data-structures-exercise-array-backed-set-in-java-with-tdd-9a54cc06e5e5)

这个练习适用于数组支持的集合，但是除了缓存之外，您应该能够很容易地将它应用于其他集合类型。

这是一份非常粗略的草稿。请随意将它复制并粘贴到您最喜欢的 IDE 中，并使用它来阅读本文。

根据您的 IDE，这应该至少有两个警告，或者如果您使用默认设置的 IntelliJ IDEA，可能有十个警告。但是当我们完成的时候，我们会把它们减少到不超过一个。

我将`ArrayIterator`声明为私有包，因为我认为只有`collections`包中的类才能使用它。

我想到，我们可以通过使`ArrayIterator<E>`成为`ArrayBackedSet<E>`的私有非静态嵌套类来避免未检查的强制转换警告的各种问题。但是我不想把它重写为`ArrayBackedList<E>`的私有非静态类。

我的想法是，使用`ArrayIterator<E>`的类可以将它们的后备数组声明为`Object[]`或`E[]`。不管怎样，运行时类强制转换异常的幽灵仍然存在，因为它向后兼容泛型之前的 Java 版本。

我假设将使用`ArrayIterator<E>`的集合类通过在数组的第一个空闲位置添加新元素来工作，其余的都是空的，直到后备数组满了为止(在这个练习中，我们不担心扩展后备数组的容量)。

因此，当调用`ArrayIterator<E>`构造函数时，它接收一个数组和最后一个空闲索引作为其参数，并且它期望最后一个空闲索引之前的所有数组槽不为空。或者至少在我们编写了适当的测试并让它们通过之后会是这样。

不过，首先，我希望您编写测试来解决构造函数中的 To Do 注释中提到的三个问题(上面的第 32、33 和 34 行)。这三个很简单，所以我会很轻松地浏览一下。

让你的 IDE 从 JUnit 开始`ArrayIteratorTest`，最好是 JUnit 5，但是 JUnit 4 也不错。根据不同的设置，您可能会得到不同用途的测试过程存根。

我让你来决定是要保留这些存根，因为它们以后可能会有用，还是要从空的括号开始。

编写由第一个待办事项注释(第 32 行)指示的测试，看到它失败，然后做最少的必要工作使它通过。

在构造函数中重复第二个和第三个 To Do 注释。TDD 周期还包括一个重构步骤，但是在这个过程的早期阶段，这不太可能是必要的。

如果数组`elements`为空，那么`ArrayIterator<E>`构造函数应该抛出一个`NullPointerException`和一个有用的非空异常消息。

至于一个不好的`lastFreeIndex`参数(负值，或者正值但是太高)，我在想也许`IllegalArgumentException`有道理。如果您喜欢不同的异常，请相应地编写您的测试。如果你使用的是 JUnit 5，那就利用一下`assertThrows()`。

考虑到这一点，我们可以继续为`Iterator<E>`接口的“强制方法”编写测试。布尔`hasNext()`函数似乎是开始这部分过程的好地方。

```
 @Test
    void testHasNext() {
        String msg = "Non-empty iterator should have next element";
        String[] array = {msg};
        ArrayIterator<String> iterator 
                = new ArrayIterator<>(array, 1);
        assert iterator.hasNext() : msg;
    }
```

提醒:如果您使用的是 JUnit 4，测试过程必须是公共的(在 JUnit 5 中它们可能是公共的，但不一定是)。

这个测试应该会失败。我们简单地通过将`ranOut`字段的初始化改为 false 来让它通过。

```
 private boolean ranOut = **false**;
```

下一个测试实际上需要我们在`lastFreeIndex`为正数而不是 0 时让`ArrayIterator<E>`构造函数读入指定的数组元素。或者这可能超越了我们的想象。我们很快就会知道了。

下一次测试的类型`E`没有太大关系。我要用`javax.naming.ldap.Rdn`。如果您选择也使用`Rdn`，请导入它。我也喜欢导入`java.util.Random`来拥有一个简单的伪随机数发生器，而不是编写一堆容易出错的`Math.random()`调用。

```
 @Test
    void testDoesNotHaveNext() {
        int size = RANDOM.nextInt(256) + 1;
        Rdn[] array = new Rdn[size];
        ArrayIterator<Rdn> iterator = new ArrayIterator<>(array, 0);
        String msg = "Even though passed in array has " + size
                + " slots, 0-capped iter should not have next elem";
        assert !iterator.hasNext() : msg;
    }
```

为了通过这个测试，如果`lastFreeIndex`为 0，那么`ArrayIterator<E>`构造器将`ranOut`设置为真就足够了。

```
 ArrayIterator(Object[] elements, int lastFreeIndex) {
        // null check, index checks go here
        **if (lastFreeIndex == 0) {
            this.ranOut = true;
        }**
        E elem = (E) elements[0];
        this.current = new Node<>(elem);
    }
```

验证这是否通过。此时，实际上不需要读入任何指定的数组元素。

在我们编写一个实际读取数组元素的测试之前，我们应该编写一个测试来检查当所有元素都通过`next()`给出时`ranOut`字段实际上从假变为真。

这是一个`Iterator<E>`的核心概念。当`iterator.hasNext()`为真时，`iterator.next()`应该给出一个`E`类型的元素。但是一旦它为假，相同的函数调用应该导致`NoSuchElementException`(那是来自`java.util`，所以继续为它添加一个导入到`ArrayIteratorTest`)。

对于下一个测试，只有一个元素的数组也足够了。一旦单个元素由`next()`给出，`hasNext()`应该返回 false，而不是 true。

```
 @Test
    void testHasNextChangesToDoesNotHaveNext() {
        String[] array = {"Just one element for this test"};
        ArrayIterator<String> iterator 
                = new ArrayIterator<>(array, 1);
        String preMsg = "Iterator should have next element";
        assert iterator.hasNext() : preMsg;
        String element = iterator.next();
        String postMsg = "After giving single element \"" + element
                + "\", iterator should not have next anymore";
        assert !iterator.hasNext() : postMsg;
    }
```

请注意，我们没有对`next()`实际上给出了什么做出任何断言。那是因为我们还没有为`next()`本身编写任何测试。

下一步是重写`ArrayIterator<E>`构造函数，这样它实际上将数组元素读入`Node<E>`实例。本质上，这构建了一个单链表。

```
 ArrayIterator(Object[] elements, int lastFreeIndex) {
        // null check, index checks go here
        if (lastFreeIndex == 0) {
            this.ranOut = true;
        }
        E elem = (E) elements[0];
        this.current = new Node<>(elem);
        **Node<E> node = this.current;
        Node<E> follower;
        for (int i = 1; i < lastFreeIndex; i++) {
            elem = (E) elements[i];
            follower = new Node<>(elem);
            node.attachNext(follower);
            node = follower;
        }**
    }
```

然后我们重写`next()`来遍历单链表，一次遍历一个节点。但是在用完节点之前，它仍然会返回 null，当用完节点时，它会抛出`RuntimeException`而不是`NoSuchElementException`。

这是为了在我们着手为`next()`编写测试时保留最初的测试失败。

```
 @Override
    public E next() {
        **if (this.ranOut) {
            String excMsg = "No more elements to iterate through";
            throw new RuntimeException(excMsg);
        }
        if (this.current.nextElement == null) {
            this.ranOut = true;
        } else {
            this.current = this.current.nextElement;
        }**
        return null;
    }
```

有了这些改变，我们对`hasNext()`的最新测试现在应该通过了。

对于重构，我担心`ArrayIterator<E>`构造函数超过二十行。

然而，现在更紧迫的问题可能是，您的 IDE 现在至少向您显示了三个警告(至少 IntelliJ 是这样)。也许让接下来的几个测试通过会清除警告。

所以现在没有重构。我建议下一个测试要求`lastFreeIndex`之前的所有数组元素都为非空。我让你写这个。

这正是我为编写私有函数`ascertainNotNull()`的原因。像这样编辑构造函数:

```
 ArrayIterator(Object[] elements, int lastFreeIndex) {
        // null check, index checks go here
        **E elem;**
        if (lastFreeIndex == 0) {
            **elem = (E) elements[0];**
            this.ranOut = true;
        } **else {
            elem = this.ascertainNotNull((E) elements[0], 0);
        }**
        this.current = new Node<>(elem);
        Node<E> node = this.current;
        Node<E> follower;
        for (int i = 1; i < lastFreeIndex; i++) {
            elem = **this.ascertainNotNull(**elements[i]**, i)**;
            follower = new Node<>(elem);
            node.attachNext(follower);
            node = follower;
        }
    }
```

现在测试应该通过了。这一改变的额外好处是，除了一个警告之外，所有的警告都被消除了:在`ascertainNotNull()`中未检查的类型强制转换。可能没有简洁、简单的方法来消除这个警告。

现在我们先不要担心这个，先把它放在一边，然后继续测试，测试需要迭代器实际给出数组的元素。

对于这个测试，我使用`java.math.BigInteger`作为`E`类型，因为它很容易生成伪随机实例。被测类更难猜出伪随机数。

如果您还没有为`BigInteger`添加一个导入语句，请将其添加到测试类中。然后进行这个测试:

```
 @Test
    void testNext() {
        int size = RANDOM.nextInt(32) + 1;
        BigInteger[] array = new BigInteger[size];
        BigInteger number;
        for (int i = 0; i < size; i++) {
            number = new BigInteger(64 + i, RANDOM);
            array[i] = number;
        }
        ArrayIterator<BigInteger> iterator 
                = new ArrayIterator<>(array, size);
        int counter = 0;
        while (iterator.hasNext()) {
            number = iterator.next();
            assertEquals(array[counter], number);
            counter++;
        }
    }
```

当然，测试失败了，因为`next()`返回 null，即使它实际上正在遍历链表。我们只需要对`next()`做一些小的改动就能通过测试:

```
 @Override
    public E next() {
        if (this.ranOut) {
            String excMsg = "No more elements to iterate through";
            throw new RuntimeException(excMsg);
        }
        **E element = this.current.element;**
        if (this.current.nextElement == null) {
            this.ranOut = true;
        } else {
            this.current = this.current.nextElement;
        }
        return **element**;
    }
```

看起来我们现在唯一缺少的是确保迭代器在元素用完时抛出`NoSuchElementException`。

此时，您可能想简单地将`RuntimeException`改为`NoSuchElementException`，但是我们需要首先编写一个测试。我也把这个留给你。

如果您使用的是 JUnit 5，请使用`assertThrows()`。如果您使用的是 JUnit 4，您可以在`@Test`注释上使用`expected`属性，但是我会使用传统的 Try-Fail-Catch。

测试失败，因为当前`next()`抛出的是`RuntimeException`，而不是它的子类`NoSuchElementException`。校正非常容易。首先，为该异常添加一个导入。您的 IDE 可能已经猜到这就是您想要添加的内容。然后更改`next()`来抛出异常。测试应该会通过。

我们几乎准备好将它连接到一个数组支持的集合。如果没有明显的重构要做，我们应该看看我们是否需要实现任何有默认实现的东西。

从 Javadoc 的[中，我们看到`Iterator<E>`接口指定了两个函数和两个过程，但是实现类只需要`hasNext()`和`next()`函数。](https://docs.oracle.com/javase/8/docs/api/java/util/Iterator.html)

`forEachRemaining()`程序可能是每个循环的启动程序。它的默认实现归结为在`hasNext()`返回 true 时调用`next()`。我们应该让它保持原样。

`remove()`过程应该允许通过迭代器从底层集合中移除元素。这似乎是多线程环境中的一个麻烦。

`remove()`的默认实现只是抛出一个`UnsupportedOperationException`的实例，并给出无用的消息“remove”但是我还没有不喜欢到推翻`remove()`的地步。

现在将它连接到一个数组支持的集合，就像上一个练习中的`ArrayBackedSet<E>`。确保该课程的所有测试都通过。

然后将`Iterable<E>`添加到您选择的集合类的类声明中(不需要导入，因为该接口来自`java.lang`包)。

这应该会导致一个错误，因为`Iterable<E>`要求我们实现`iterator()`，一个返回`Iterator<E>`实例的函数。我建议放入下面的存根来清除这个错误:

```
 // TODO: Write a test for this
    @Override
    public Iterator<E> iterator() {
        return new ArrayIterator<>(this.elements, 0);
    }
```

也是`java.util.Iterator<E>`的一个导入。

然后，在相应的测试类中，编写一个测试。对于这个测试来说,`E`类型并不重要，重要的是集合应该有多于零个的非空元素，这样测试第一次就会失败。

我写了一个测试，开始时使用一个标准的 For 循环，用伪随机`BigInteger`实例填充一组由数组支持的`BigInteger`。我将测试的源代码列表分开，以便在中间给出解释。

```
 @Test
    void testIterator() {
        System.out.println("iterator");
        int size = RANDOM.nextInt(48) + 16;
        ArrayBackedSet<BigInteger> expected 
                = new ArrayBackedSet<>(size);
        BigInteger number;
        for (int i = 0; i < size; i++) {
            number = new BigInteger(i + 32, RANDOM);
            expected.add(number);
        }
```

然后，测试应该使用相同的`E`类型创建一个新的集合实例，这次使用 For Each 循环将第一个集合的元素添加到第二个集合中。

```
 ArrayBackedSet<BigInteger> actual 
                = new ArrayBackedSet<>(size);
        for (BigInteger n : expected) {
            actual.add(n);
        }
        assertEquals(expected, actual);
    }
```

如果您没有任何语法错误(比如由于不完整的复制和粘贴导致的错误粘贴)，您就可以运行测试了。

它应该会失败。在我的系统上，测试首先用以下伪随机数生成了一个数组支持的集合:2478834069，4237164324，3174102804，你明白了吧。在您的系统上，您可能会看到也可能看不到任何伪随机数。

因为`lastFreeIndex`为 0，迭代器的`hasNext()`函数在第一次从`forEachRemaining()`调用时返回 false，所以根本不给出任何元素。

我们简单地通过将`lastFreeIndex`参数更改为正确的界限来使测试通过。根据您如何进行数组支持的 set 练习，这可能是`this.nextUp`。

我们不需要编写对`iterator()`函数的直接调用，因为这是启用 For Each 循环的原因。如果您对此有丝毫怀疑，请从集合类中删除“`implements Iterable<E>`”，并验证测试类中显示的错误。

运行测试，应该会通过。现在您知道了如何让自己编写的 Java 集合对 For Each 循环可用。

# 重构

关于 TDD 的一个好处是它帮助我们防止过度工程化。不过，这并不能完全阻止它。我在这里提出的解决方案是过度设计的。

它应该是一个数组迭代器，然而它创建了一个单链表？那太多了。删除`Node<E>`类并相应地重构。你可以这样做，因为如果你怀疑你的更好的解决方案真的更好，你可以运行测试。

# 结论，现在

我想到，也许我们可以为数组支持的集合创建一个抽象超类。那么这个抽象超类包含`ArrayIterator<E>`作为私有的、非静态的内部类。

这样，所有的子类都不必实现`iterator()`。也许这将清除所有未检查的演员警告。但这是另一篇文章的主题。