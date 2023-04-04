# 数据结构练习:使用 TDD 的 Java 中的数组支持列表

> 原文：<https://blog.devgenius.io/data-structures-exercise-array-backed-list-in-java-with-tdd-e6cb031a0233?source=collection_archive---------4----------------------->

![](img/758c6dadba23661b040dcc9fd76edfef.png)

照片由[柳巴·比雷克](https://unsplash.com/@ibilyk?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄

最著名的 Java 类大概就是`String`。第二个最著名的是来自`java.util`包的`ArrayList<E>`。这个类结合了数组的速度和可靠性以及链表的大小灵活性。

没有必要重新发明`ArrayList<E>`，除了它是一个优秀的编程练习。如果您有一点点担心您的数据结构知识可能不足，那么重新发明一个现有的数据结构肯定有助于巩固您的知识，因为它让您获得了需要解决什么问题的第一手经验。

也许你已经完成了[数组支持的集合练习](/data-structures-exercise-array-backed-set-in-java-with-tdd-9a54cc06e5e5)。与数组支持的列表有一些相似之处。但是，我不会假设您已经完成或没有完成数组支持的 set 练习。

Java 虚拟机(JVM)级别支持数组，这意味着它们是“原始的”,这表明它们比我们用类构建的任何数据结构运行得都快。

当您确切知道需要多少数组元素时，数组就很有用，因为 Java 要求提前声明数组的确切大小。不允许调整现有数组的大小。

因此，扩展数组容量的唯一方法是将其内容复制到另一个容量更大的数组，然后丢弃原始数组。这比听起来容易，但每次都要写出来可能会很乏味。

更糟糕的是，它容易出错，可能会导致数组索引越界的偶然异常，这会分散我们对给定编程会话的关注。

最好有一个由数组支持的已定义类，但它会在必要时自动将支持数组复制到一个更大的数组中。这正是`ArrayList<E>`给我们的。

`ArrayList<E>`类有三个构造函数。其中一个构造函数用指定的初始容量初始化一个新实例。但是大多数程序员使用零参数构造函数，它只是用默认的初始容量初始化一个新的 list 实例，我相信这个容量是 10。

在这个练习中，我们只有两个构造函数，一个用于默认初始容量，另一个用于指定的初始容量。

多亏了泛型，你可以指定`ArrayList<E>`实例将包含的类型`E`，以在编译时获得额外的类型安全。举个例子，

```
ArrayList<Transaction> transactions = new ArrayList<>();
```

然后，当你写类似于“`transactions.add(someObject)`”的东西时，编译器可以检查`someObject`是否真的是一个`Transaction`实例，如果不是，就提醒你。

类型参数没有什么神奇的。我们可以为自己的类编写它们，就像我们自己版本的数组支持列表一样，我很快就会展示出来。

按照约定，类型参数由一个大写字母组成。字母`E`，大概是“元素”，广泛用于`java.util`包的集合类中(`K`和`V`用于键值对)。

我看不出有什么迫切的理由去质疑这个约定，我们的数组支持的 list 类将被称为`ArrayBackedList<E>`。

我将试着给你最基本的指导，让你知道如何使用测试驱动开发(TDD)过程自己编写`ArrayBackedList<E>`。

使用 TDD，您编写一个测试，确保它失败，然后更改测试中的类，使其通过测试。检查是否需要重构，如果确实需要，就重构。

要通过测试，您应该只做能通过测试的最少工作。这样，您就不会过度设计不必要的功能。

如果需要某个特定的特性，您最终会为它编写一个测试。但是如果不需要那个特性，你就永远不会为它编写测试。

有了 TDD，一个程序就逐渐脱离了不完美的状态。但是，除了最简单的程序，你不可能从一开始就写出一个完美的程序。TDD 将你肯定会犯的错误转化为阐明程序应该如何工作的优势。

我会给你一个非常粗略的`ArrayBackedList<E>`草稿，让你开始。请将它复制到您喜欢的 Java 集成开发环境(IDE)的项目中。制作一个`collections`包，或者`org.example.collections`，如果项目中还没有这样的包。

根据您的 IDE，这应该至少有一个警告，但是没有错误。如果有错误，可能是错误的包名或者是复制和粘贴时遗漏了右大括号。

无论我们先写哪个测试，这份草稿都会失败。从 JUnit 测试开始，让您的 IDE 用正确的导入为`ArrayBackedListTest`生成一个框架(根据您的 JUnit 版本)。

`ArrayBackedList<E>`构造函数应该通过抛出`IllegalArgumentException`来拒绝`initialCapacity`参数的负数。

如果你使用的是 JUnit 5，请使用`assertThrows()`。如果您使用 JUnit 4，您可以使用`@Test`注释的`expected`属性，但是在这种情况下，我会建议您用 JUnit 3 的方式编写测试，在尝试使用不正确的大小后，在 Try 块中使用`fail()`。

我开始喜欢在我的测试类中有一个`java.util.Random`实例，这样更容易得到随机数。我在`ArrayBackedListTest`中声明了一个名为`RANDOM`的实例。

我推荐使用`Random`的`nextInt()`函数得出一个伪负数，作为负容量测试中的初始容量传入。

当然，测试会失败，因为`ArrayBackedList<E>`的主构造函数实际上不做任何事情。要通过测试，我们需要做的就是在构造函数中添加一个 If 语句。类似于…的东西

```
if (initialCapacity < 0) throw new IllegalArgumentException();
```

尽管您编写的测试可能需要一个有用的异常消息。我写的那个至少需要一个非空的异常消息。

除了上面给出的非常粗略的草稿，我还会给你两到三个测试，从第一个测试`add()`函数开始:

```
 @Test
    void testAdd() {
        System.out.println("add");
        ArrayBackedList<String> list = new ArrayBackedList<>();
        String msg 
                = "Should be able to add this message to the list";
        boolean opResult = list.add(msg);
        assert opResult : msg;
    }
```

如果您使用的是 JUnit 4，请添加“`public`”；这适用于本文中的所有测试。

只需对`ArrayBackedList<E>`做一个改变就可以通过这个测试:

```
 public boolean add(E element) {
        return **true**;
    }
```

这太简单了，不需要任何重构。在过程的早期，通常不需要重构。除了可能删除待办事项注释。

我知道这感觉很蠢。现在`add()`实际上没有给列表添加任何东西，但是测试通过了。该测试实际上不需要向支持数组添加任何东西。

它不应该，因为那会要求我们在为那个函数编写正确的测试之前，使用`get()`超越自己，或者扩大后备数组的访问修饰符，我觉得这将违背正确封装的原则。

所以下一步可能是为`get()`函数编写一个测试，实际上需要`ArrayBackedList<E>`来存储传递给`add()`函数的元素。

首先，我们需要测试具有负索引或者过多索引的`get()`(例如，对于只有 28 个元素的列表，索引是 28——记住第一个索引是 0，而不是 1，所以在这个例子中最高有效索引应该是 27，而不是 28)。越界索引显然会导致`IndexOutOfBoundsException`。

编写这些测试，看它们失败(可能是因为空指针异常，而不是索引越界异常)。然后做最起码的必要工作让他们通过。

编写负指数测试并使其通过是很容易的。过度指数测试可能看起来更加困难。但是请记住，由于我们还没有提供一个从现有的 list 实例创建新的 list 实例的构造函数，所以新创建的 list 应该是空的。

因此，您的过度索引测试可以实例化一个新的`ArrayBackedList<E>`实例，然后立即对索引 0 或任何正索引调用`get()`，而无需向其添加任何元素。我认为你应该用`IndexOutOfBoundsException`来做这件事。

为了使`get()`过度索引测试通过，有必要添加某种计数器来跟踪到目前为止有多少元素被添加到列表中。大概是这样

```
 private int nextUp = 0;
```

然后每次调用`add()`时递增:

```
 public boolean add(E element) {
        **this.nextUp++;**
        return true;
    }
```

标识符"`nextUp`"对你有意义吗？如果没有，那么将它改为一个更有意义的名称可能是此时重构的唯一机会。

现在还没有理由对`elements`数组做任何事情，即使你的 IDE 可能警告你它没有被使用。如果你在用那个后备数组做任何事情，你已经超越了你自己。

好了，现在我们可以编写测试了，它实际上需要将元素写入后备数组。我在这里喜欢做的是想出或多或少随机混合的元素。它不必是完全随机的，只要足够随机以至于被测试的类不能提前猜到它们。

我编写了一个测试过程，将一些随机的日期(使用`java.time.LocalDateTime`)写入本地数组，并将这些日期添加到`ArrayBackedList<LocalDateTime>`的一个实例中。然后对照数组检查添加到列表中的每个日期。

```
 @Test
    void testGet() {
        System.out.println("get");
        int size = RANDOM.nextInt(128) + 1;
        ArrayBackedList<LocalDateTime> list 
                = new ArrayBackedList<>(size);
        LocalDateTime[] array = new LocalDateTime[size];
        LocalDateTime dateTime = LocalDateTime.now();
        int adjustment;
        for (int i = 0; i < size; i++) {
            adjustment = RANDOM.nextInt(512 + i);
            dateTime = dateTime.plusHours(adjustment);
            array[i] = dateTime;
            list.add(dateTime);
        }
        LocalDateTime expected, actual;
        for (int j = 0; j < size; j++) {
            expected = array[j];
            actual = list.get(j);
            assertEquals(expected, actual);
        }
    }
```

但是这个测试当然会失败，因为，只要`index`在适当的范围内，`get()`就会返回 null(当然假设您在通过之前的测试时没有超越自己)。

我们需要做一些改变来通过这个测试。首先，主构造函数需要初始化后备数组，根据指定的初始容量调整它的大小。下一段摘录中的 To Do 注释代表您为通过负初始容量测试所做的工作。

```
 public ArrayBackedList(int initialCapacity) {
        // TODO: Check initialCapacity is not negative
        **this.elements = new Object[initialCapacity];**
    }
```

然后`add()`需要将`element`添加到`elements`中。

```
 public boolean add(E element) {
        **this.elements[this.nextUp] = element;**
        this.nextUp++;
        return true;
    }
```

考虑到这一点，对`get()`的一个小改变将使测试通过:

```
 public E get(int index) {
        // TODO: Check index is within appropriate bounds
        **return (E) this.elements[index];**
    }
```

如果您使用 Eclipse 或 IntelliJ IDEA，您可能会得到一个未检查的强制转换警告。

对我来说，Eclipse 似乎过于急于用`@SuppressWarnings`注释来抑制警告。但是如果你这样做了，并且没有其他警告，Eclipse 不会给你一个绿色指示器。哦好吧。

如果您使用的是 Apache NetBeans，那么您可能会看到一个绿色的指示器，而不会抑制任何警告。也许 Apache 人认为未检查的 cast 警告与其说是帮助，不如说是麻烦。

你可能想知道为什么我将`elements`声明为`Object[]`而不是`E[]`。我确实试图将其声明为`E[]`，但这导致了编译错误。比起编译错误，我更喜欢未检查的强制转换警告。

现在，也许你认为这个练习最有趣的方面是:如何扩展列表的容量。是时候编写一个测试了，它将实例化一个具有一定初始容量的新列表，并继续填充列表到该容量。然后，测试将添加另一个元素。

我们不打算在这个问题上使用`assertThrows()`，因为扩展容量不应该导致任何异常(除非我们设法将容量增加到`Integer.MAX_VALUE`——尽管我怀疑在这种不太可能的情况下，某种虚拟机错误更有可能发生)。

但是如果你使用的是 JUnit 5，你也可以使用`assertDoesNotThrow()`。使用 JUnit 4 或更早的版本，您可以在 Catch 块中放置一个`fail()`。

编写一个测试，用特定的容量实例化`ArrayBackedList<E>`,并将列表填充到指定的容量。然后，测试试图再添加一个元素。不应出现任何异常。

因为我们当前的草案`add()`没有检查后备阵列是否已满，所以这个测试将触发`ArrayIndexOutOfBoundsException`，从而失败。

支持数组需要扩展至少一个元素。但是，如果添加的一个元素超出了原始容量，那么有理由认为可能会添加更多的元素。所以只增加一个阵列插槽可能太少了。但是将数组的大小翻倍可能有些过头了。

你们中的一些人可能对数组支持的 set 练习有似曾相识的感觉。对于这一点，我建议我们使用公式 floor(3 *n* /2)，其中 *n* 是后备数组的当前大小。

如果您查看`java.util.ArrayList<E>`的源代码，您会看到类似于`n + (n >> 1)`的内容，它利用逐位算术来计算 floor(3 *n* /2)可能会更快。

在 Java 1.2 时代，逐位算术可能在性能上有所不同，但现在这已经不重要了。使用你喜欢的任何公式，无论是`3 * n / 2`、`n + (n >> 1)`还是别的什么。

```
 public boolean add(E element) {
        **if (this.nextUp == this.elements.length) {
            int expandedSize = this.nextUp + (this.nextUp >> 1);
            Object[] largerArray = new Object[expandedSize];
            for (int i = 0; i < this.nextUp; i++) {
                largerArray[i] = this.elements[i];
            }
            this.elements = largerArray;
        }**
        this.elements[this.nextUp] = element;
        this.nextUp++;
        return true;
    }
```

现在测试应该通过了。

这里应该至少有两个重构的机会，你的 IDE 应该建议至少其中一个，也许是作为一个警告。

在 NetBeans 中，我收到“手动阵列复制”警告。建议的修复方法是用`System.arraycopy()`替换它(这可能是作为一个“本地方法”实现的)。继续进行更改，然后运行所有测试，以确保我们没有破坏任何东西。

这里重构的另一个机会是 If 语句的主体看起来应该是一个单独的私有过程。另一方面，整个单参数`add()`函数运行不到 15 行，所以也许减少它的长度现在不是优先考虑的事情。

我认为你可以用 TDD 过程自己解决剩下的问题，使用 To Do 注释作为路标。我会给你一些提示:

*   编写一个私有过程在后台数组中来回移动元素是一个好主意。如果相关的`add()`和`remove()`没有更新，两个私有程序都应该更新`nextUp`。
*   在对双参数`add()`进行操作之前，确保`get()`正常工作。
*   你可能想写一个`indexOf()`函数。这并不像 set 练习中那样重要，因为列表可以接受重复的元素，但是对于不止一个列表操作来说，这仍然非常重要。另一方面，如果您在重构步骤中编写`indexOf()`来消除“代码重复”，这并不是一件坏事

尽管可能会推迟对`iterator()`的测试，直到你阅读了[我关于迭代器的文章](/data-structures-exercise-making-a-collection-available-to-javas-for-each-loops-126e6cf8a877)，其中给出了更多的细节。

我们不能像草稿建议的那样，将迭代器实现为匿名类吗？我们当然可以。我将在另一篇文章中再次讨论这个想法。

我没有填写`equals()`或`hashCode()`的存根，但这些应该被视为练习的一部分。拥有一个正常运行的`equals()`可能会使其他一些测试更容易编写。

如果两个列表具有相同顺序的相同元素，则认为它们相等。因此，哈希代码应该是一个结果受元素顺序影响的函数。

例如，假设我们有一个包含整数 1、2 和 3 的列表和另一个包含整数 3、1 和 2 的列表。

如果`hashCode()`简单地将元素的散列码相加，那么例子中的两个列表将具有相同的散列码，6(记住`Integer`实例的散列码与其数值相同)。

假设我们让`hashCode()`取第一个元素的散列码，加倍它，加上第二个元素的散列码，加倍到目前为止的总和，等等。

因此，列表 1，2，3 将具有散列码 11，而列表 3，1，2 将具有散列码 16。散列码不必是完全唯一的，事实上，对于我们正在处理的列表类来说，这是不可能的。哈希码必须足够唯一。

在这种情况下，我会比`hashCode()`更彻底地测试`equals()`。这个例子中的列表不应该被认为是相等的，即使如果它们都是集合，它们将被认为是相等的。

我提到过关于集合，也许我们应该接受两个空集被认为是相等的，即使它们的`E`类型不同，因为`E`类型在运行时会被遗忘。列表也一样。所以也许不需要测试。

如果你也读过[我写的关于数组支持的集合的文章](/data-structures-exercise-array-backed-set-in-java-with-tdd-9a54cc06e5e5)，我感谢你。我为你在阅读这篇文章时的似曾相识的感觉道歉。

毫无疑问，`ArrayBackedList<E>`中来自`ArrayBackedSet<E>`的所有重复都可以用一个抽象类来消除，这个抽象类集合了我们现在拥有的两个类的所有公共功能。

也许我们还可以更好地封装迭代器类，使其优雅简洁。

这是我的下一篇数据结构文章。同时，我欢迎对其他数据结构主题的建议。