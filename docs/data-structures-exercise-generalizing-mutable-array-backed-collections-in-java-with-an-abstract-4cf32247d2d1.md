# 数据结构练习:用抽象超类概括 Java 中可变数组支持的集合

> 原文：<https://blog.devgenius.io/data-structures-exercise-generalizing-mutable-array-backed-collections-in-java-with-an-abstract-4cf32247d2d1?source=collection_archive---------12----------------------->

![](img/122ef1a4a873e11a8b5ba0b857afa8ad.png)

[Ariel](https://unsplash.com/@arielbesagar?utm_source=medium&utm_medium=referral) 在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄的照片

如果你做了[数组支持集合的练习](/data-structures-exercise-array-backed-set-in-java-with-tdd-9a54cc06e5e5)和[数组支持列表的练习](/data-structures-exercise-array-backed-list-in-java-with-tdd-e6cb031a0233)，你可能会注意到它们有很多共同点。也许你甚至从一个复制粘贴到另一个，尽管有一种挥之不去的感觉，这种复制粘贴绝对不是计算机编程的最佳实践之一。

我们将在本文中通过一些重构来缓解这种感觉。

这是一篇很长的文章，我希望您能在自己的计算机上使用自己喜欢的 Java 集成开发环境(IDE)来学习。同时，我也希望你做了我上面提到的两个数据结构练习中的一个，或者两个都做了。

列表和集合是不同的，但是，当作为数组支持的数据结构实现时，它们有许多共同的重要特征。

具体来说，

*   集合的后备数组应该根据需要扩展容量，就像列表的后备数组一样。
*   呼叫者可以指定初始容量，但该初始容量决不能是负数。
*   或者调用方可以使用提供默认初始容量的辅助构造函数。列表和集合之间的值可能没有什么不同。
*   size 函数的工作方式是一样的，它应该有相同的标识符。
*   这同样适用于 remove 函数、布尔值为空的函数和 remove 过程。
*   无论数据结构是集合还是列表，在后备数组中搜索元素都是一样的(唯一的区别是集合中的元素没有公开索引)。
*   set 类和 list 类都应该提供迭代器，以支持 Each 循环(我将在另一篇文章中解释如何实现)。

因此，有必要创建一个抽象类，将这些集合共有的所有功能集中到一个方便的地方。

然后，如果我们想到一个更好的方法来实现这些共享功能之一，我们只需在一个地方进行必要的更改。您将不得不运行所有的测试，但是 IDE 让这变得足够简单。

我们的抽象类将从 list 和 set 类中复制几行。一旦我们让抽象类正常工作，我们就可以重构列表并设置类来消除大部分(如果不是全部)的重复。

至于名称不同但相同的函数或过程，我们可以在抽象类中声明它们是抽象的，或者我们可以提供默认的实现，可以根据需要覆盖这些实现。

然而，我们不会从任何测试类中删除任何内容。最好有多余的测试。在任何情况下，子类错误地覆盖超类肯定是可能的。

在您的 IDE 中，打开带有`ArrayBackedSet<E>`和/或`ArrayBackedList<E>`类的项目。在`ArrayBackedSetTest`和`ArrayBackedListTest`中的所有测试都应该通过，并且在测试中的类中没有错误或警告，除了可能对`E`的未检查的强制转换。

我假设你已经知道`E`是什么了。快速复习:它只是集合元素类型的替身。

调用者将用`String`、`LocalDate`、`SQLPermission`或调用者需要的任何引用类型替换`E`。如果省略了类型规范，`E`将被理解为`Object`，尽管这是不被允许的。

# 起草概述

就像集合和列表练习一样，我会给你一个有警告(但没有错误)的草稿，它应该不会通过我们的第一次测试。

这是`ArrayBackedCollection<E>`的草稿，我想让你把它放到你最喜欢的 Java 集成开发环境(IDE)中:

注意这个类实现了来自`java.lang`包的`Iterable<E>`，而不是来自`java.util`的`Iterator<E>`。

您的 IDE 可能会显示一些警告，比如在 IntelliJ IDEA 中可能有十几个警告，但是应该没有错误。

不要急着给`ArrayBackedSet<E>`或`ArrayBackedList<E>`加`extends ArrayBackedCollection<E>`。

由于`ArrayBackedCollection`是抽象的，我们的测试需要一个具体的子类。它应该只提供实现所需的最低限度，在这种情况下只是一个构造函数。

嗯，零参数构造函数可能是一种可以接受的奢侈，因为对于某些测试来说，初始容量并不重要。

下面是嵌入在 JUnit 5 测试类框架中的实现:

NetBeans 足够聪明，可以在测试类中的抽象类的实现中创建填充，但是您可能仍然喜欢从上面复制并粘贴框架。

如果您正在使用 JUnit 4，或者一个完全不同的测试框架，JUnit 5 的 import 语句将需要修改。

您可能已经知道，Java 禁止抽象类也被标记为 final。试图对一个类同时使用两个修饰符会导致编译错误。人们的想法可能是这是一种矛盾修饰法。

然而，抽象类有最终成员并不矛盾，就像上面显示的`expandCapacity()`过程。

这里的“最终”当然不是指“最终版本”，而是指那个特定过程的终结。

我的想法是，如果我们决定不喜欢选择新容量的公式(3 *n* /2，其中 *n* 是当前容量)，我们只需要在一个地方改变它，而不用担心检查覆盖。

当然，这是假设我们没有创建一些其他的过程来做几乎相同的事情，但是有不同的标识符。当我们开始重构时，我们必须记住这一点。

# 编写测试

我认为我们已经准备好了开始测试驱动开发(TDD)过程的一切:我们写一个测试，看它失败，改变被测试的类，使它通过测试，然后如果必要的话重构。

如果您做了集合和列表练习，您会记得编写测试来确保这些类通过抛出`IllegalArgumentException`来拒绝初始容量参数的负数。

现在我想让你为`ArrayBackedCollection<E>`做同样的事情，写一个测试，试图创建一个新的`ArrayBackedCollectionImpl<E>`，用一些伪随机数作为初始容量。选择任何你喜欢的`E`类型。如果您使用 JUnit 5 或 TestNG，请使用`assertThrows()`。

测试应该会给我们第一次`ArrayBackedCollectionTest`的失败。整数参数构造函数的当前版本没有出现预期的异常，而是简单地忽略了该参数，并创建了一个新的后备数组，其大小为硬编码的 25。

或者，如果您提前将硬编码的 25 更改为`initialCapacity`参数，测试将导致负数组大小异常，而不是`IllegalArgumentException`。

您知道如何通过这个测试:将一个 If 语句和适当的 Throw 语句放入相关的构造函数中。检查需要重构的东西，即使在这一点上我们可能没有什么可以重构的，除非超越我们自己。

在 list 和 set 练习中，除了 class private 之外，我没有给后备数组`elements`任何访问修饰符。我认为测试类窥视后备数组破坏了正确的封装。

现在有了抽象超类，如果没有保护的话，有必要使后备数组包成为私有的(尽管我们仍然应该停止使用 public)。子类需要为自己访问后备数组。

在这种情况下，测试类也可以访问后备数组。但最好只在绝对必要时才这样做，并确保这些访问是只读的。

我不确定测试整数参数构造函数是否正确地调整了支持数组的大小是否合理，但是我们可以编写这个测试。`E`类型现在实际上并不重要。

```
 @Test
    void testConstructorBackingArrayAllocation() {
        int expected = 2 
                * ArrayBackedCollection.DEFAULT_INITIAL_CAPACITY
                + RANDOM.nextInt(20) + 1;
        ArrayBackedCollection<java.sql.Clob> collection
                = new ArrayBackedCollectionImpl<>(expected);
        int actual = collection.elements.length;
        assertEquals(expected, actual);
    }
```

如果您使用的是 JUnit 4，一定要公开测试。

我们只需在构造函数中修改适当的行就可以让它通过。

在为复制构造函数编写测试之前，我们需要通过许多其他测试。

虽然我们可以把`add()`抽象化，但我认为让它表现得像`ArrayBackedList<E>`中的`add()`更有意义。也就是说，元素应该简单地添加到后备数组中的下一个可用位置，而不检查集合中是否已经有该元素。

但是我们对`add()`的第一个测试应该只是期望函数返回 true，而不需要对后台数组做任何事情。

```
 @Test
    void testAdd() {
        ArrayBackedCollection<String> collection
                = new ArrayBackedCollectionImpl<>();
        String msg = "Should be able to add this to the collection";
        boolean opResult = collection.add(msg);
        assert opResult : msg;
    }
```

我们使用中间变量`opResult`是为了以后不会出现不纯的函数警告，因为`add()`会有修改后备数组的“副作用”,一旦我们让它正常工作。

测试当然会失败。一个简单的改变就让它过去了。

```
 public boolean add(E element) {
        return **true**;
    }
```

此时不需要重构。

但是在我们对`contains()`进行测试之前，我们不会对它进行任何修改。

我想你已经知道接下来的两个测试是什么了。首先，对`contains()`进行测试，实例化一个新的集合，添加一个元素，然后断言列表包含该元素。不要像前面的测试那样担心布尔结果的中间结果。

看到测试失败，只需通过让`contains()`返回 true 而不是 false 就可以让它通过。

当我开始进行不包含测试时，我想我可能走得太远了。我认为我们应该为`indexOf()`放一个存根，并为它写一个测试。

```
 // TODO: Write tests for this
    int indexOf(E element) {
        return Integer.MAX_VALUE;
    }
```

当然，这个想法是`indexOf()`返回`element`在后备数组中的索引，或者如果`element`不在后备数组中，返回 1。或者任何负整数都可以。

我对`indexOf()`的访问级别非常谨慎。list 类可以将其扩展为 public，set 类可以将其保持为 package private。

我建议对`indexOf()`进行一个非常简单的测试:一组整数(使用`Integer`包装器)按顺序排列，0 在索引 0 处，1 在索引 1 处，2 在索引 2 处，以此类推，直到某个合理的小阈值。然后测试简单地断言这些整数中的每一个的索引都等于它自己。

这个测试当然会失败。我们需要更改`add()`来实际添加元素到支持数组:

```
 public boolean add(E element) {
        **this.elements[this.nextUp] = element;
        this.nextUp++;**
        return true;
    }
```

接下来，我们把`indexOf()`改成这样:

```
 int indexOf(E element) {
        **boolean found = false;
        int counter = 0;
        while (!found && counter < this.nextUp) {
            found = element.equals(this.elements[counter]);
            counter++;
        }
        if (found) {
            return counter - 1;
        } else {**
            return Integer.MAX_VALUE;
        **}**
    }
```

我认为当`element`不在后备数组中时，我们需要对`indexOf()`进行单独的测试。

此外，我认为单独的测试应该实例化一个新的集合，向其中添加某种随机元素，然后尝试获取保证不在集合中的元素的索引。

例如，测试可以向集合中添加一些伪偶数，比如说`2 * i * RANDOM.nextInt()`，其中`i`是 For 循环变量，然后查询奇数伪随机数的索引。

测试应该会失败，因为`indexOf()`正在为未找到的分支返回`Integer.MAX_VALUE`。简单替换未找到的分支将使测试通过。

我不认为无索引测试应该要求 1，确切地说，它应该只要求一个小于 0 的数字。测试不是文档。

一旦我们将测试从失败变成通过，我们就可以进行不包含测试了。测试实例化了一个新的集合，但是没有添加任何东西。那么它断言集合不包含任意元素。

我们看到测试失败了。有了我们用`indexOf()`打下的基础，正确版本的`contains()`实际上是自己写的。

```
 public boolean contains(E element) {
        return **this.indexOf(element) > -1**;
    }
```

测试通过，此时所有测试都应该通过。你记住重构了吗？我忘了。有时候会这样。没什么大不了的。

尽管此时唯一的重构机会可能是确保没有过时的待办事项注释(也就是说，项目中唯一的待办事项注释是针对我们尚未编写的测试)。

你认为`ArrayBackedCollection<E>`应该接受空元素吗？我不知道。如果你同意我的观点，那么`contains(null)`应该总是返回 false，它不应该在任何时候导致空指针异常。

编写一个测试，断言`contains(null)`返回 false，或者如果立即出现空指针异常，则测试失败。在查询`contains(null)`之前，确保测试至少添加了一个非空元素——这是为了确保至少可以进行一次元素比较。

当然，一开始就添加一个空元素也是不可能的。但是我反复思考调用`add(null)`是应该返回 false 还是导致空指针异常。

然而，如果 list 子类有一个 add at index 过程(与 add at index 布尔函数相反)，那么为了一致性，add 函数和 add at index 过程都应该抛出一个空指针异常，以尝试添加一个空元素。

或者说这样的一致性值得吗？也许更重要的是文档清楚地解释了区别。

我想我没有在数组支持的列表练习中涉及这一点，我试图保持文章简短。值得重温。但是现在我将继续讨论抽象集合超类:编写关于尝试向集合中添加空元素的测试，从通过到失败。

一旦非空元素填满了后备数组的初始容量，仍然可以添加更多的非空元素，但是后备数组的容量需要扩展，而不会触发数组索引越界异常。

我已经在草稿中放了一个工作的`expandCapacity()`程序，但是没有任何调用。

在我们实际编写对它的调用之前，我们需要编写一个测试。该测试指定集合的初始容量，然后在尝试再添加一个元素之前将集合填充到该容量。

为`ArrayIndexOutOfBoundsException`编写一个 Catch 块，如果它发生，测试将失败。或者测试框架可能有某种我们可以使用的“断言不抛出”。

我们通过在必要时调用`expandCapacity()`的`nextUp`上添加边界检查来使测试通过。

```
 public boolean add(E element) {
        // Null check goes here
        **if (this.nextUp == this.elements.length) {
            this.expandCapacity();
        }**
        this.elements[this.nextUp] = element;
        this.nextUp++;
        return true;
    }
```

边界检查是在空值检查之前还是之后有关系吗？这可能是一个重构的机会。

我现在才想到，0 应该像负数一样禁止作为初始容量参数。如果 *n* = 0，那么 3 *n* /2 也= 0。编写一个测试，尝试实例化一个初始容量为 0 的集合，并要求抛出一个`IllegalArgumentException`来通过，使其从失败变为通过。

有时有必要从数组支持的集合中移除元素。将数组中的指针改为空指针就足够了。

但是后备数组需要将所有非空元素连续放置，不管集合是列表、集合还是其他。

当顺序无关紧要时，比如在集合中，最简单的方法是让`remove()`获取后备数组中最后一个非空元素，并将其移动到要删除的元素当前所在的位置。

然后，像 set 类这样的类可以简单地继承`remove()`，而像 list 类这样的类必须覆盖`remove()`，以便保持其余元素的顺序。

即使我们选择这样做，而不是简单地在`ArrayBackedCollection<E>`中将`remove()`抽象化，相应测试类中的测试应该只断言被移除的元素不再包含在集合中，并且集合的大小已经减少了一个元素。

虽然我们还没有测试过`size()`函数。另一方面，测试可以窥视`nextUp`，因为我们已经把它变成了包的私有字段。即便如此，如果没有令人信服的理由，我还是不愿意这么做。

所以现在把`remove()`放到一边，继续做`size()`。`size()`的测试应该实例化一个新的集合，但是不要填充到初始容量。大概是这样的:

```
 @Test
    void testSize() {
        System.out.println("size");
        int capacity = RANDOM.nextInt(128) + 32;
        ArrayBackedCollection<**SomeClass**> collection 
                = new ArrayBackedCollectionImpl<>(capacity);
        int expected = RANDOM.nextInt(capacity - 8) + 4;
        for (int i = 0; i < expected; i++) {
            **SomeClass** element = **???** ;
            collection.add(element);
        }
        int actual = collection.size();
        assertEquals(expected, actual);
    }
```

我希望你能自己选择用什么做`SomeClass`。

如果测试失败，我们只需将`size()`转换成`nextUp`的 getter 就可以通过测试:

```
public int size() {
        return **this.nextUp**;
    }
```

好了，现在我们准备测试从集合中移除一个元素。

但是，首先编写一个测试来尝试移除集合中不存在的元素。优选地，集合不为空，它只是不包含指定要移除的元素。在这种情况下，`remove()`应该返回 false，而不对后备数组进行任何更改。

写测试，看它失败，让它通过。然后为`remove()`编写前面讨论过的测试，删除单个元素，断言该元素不再在集合中，并且集合的大小已经减少了一个元素。

确保第一次测试失败。我重写了`remove()`以通过测试:

```
 public boolean remove(E element) {
        **int index = this.indexOf(element);
        if (index < 0) {**
            return false;
        **} else {
            this.nextUp--;
            if (index < this.nextUp) {
                this.elements[index] = this.elements[this.nextUp];
            }
            this.elements[this.nextUp] = null;
            return true;
        }**
    }
```

假设您的测试不期望所有在`index`和`nextUp`之间的元素都上移，那么这应该会通过您的测试。

重构的唯一机会可能是这两个 If 语句，一个嵌套在另一个的 Else 子句中。如果你能在通过所有测试的时候不使用任何 If 语句，并且不显得愚蠢，那就太好了。

你可以自己解决`isEmpty()`的考试。记住:一个元素足够让一个集合不为空。

对`clear()`的测试可以只是填充一个集合，断言它不为空，调用`clear()`，然后断言它为空。那么这就足以通过这样一个考验:

```
 public void clear() {
        **this.nextUp = 0;**
    }
```

这样做的唯一问题是，它会保留对其他任何目的都不需要的对象的引用，从而阻碍垃圾收集。

由于对`nextUp`和`elements`的访问范围更广，我们可以编写一个测试来检查后备数组槽是否都被转换为空指针。我仍然觉得这有问题，但我也发现这个理由比之前的`remove()`更有说服力。

```
 @Test
    void testClearNullsOutBackingArraySlots() {
        int capacity = RANDOM.nextInt(64) + 16;
        ArrayBackedCollection<**SomeClass**> collection
                = new ArrayBackedCollectionImpl<>(capacity);
        **// TODO: Fill collection**
        collection.clear();
        for (int j = 0; j < collection.elements.length; j++) {
            String msg = "Backing array slot " + j 
                    + " should be null";
            assert collection.elements[j] == null : msg;
        }
    }
```

一定要把收藏品装满。

我现在想到，实现清除过程的更好方法可能是实例化一个与`elements`大小相同的新数组，并将`elements`指针重置为该新数组。

这样，如果有任何对象仅仅因为来自`elements`的引用而被挂起，它们占用的内存现在可以被垃圾收集器回收。

```
 public void clear() {
        **Object[] emptyArray = new Object[this.elements.length];
        this.elements = emptyArray;**
        this.nextUp = 0;
    }
```

这里至少有一个重构的机会。如果你的 IDE 不建议，你能搞清楚吗？

# 等式和散列码

提醒:由于 Java 类型系统的工作方式，`equals()`需要检查 null 并检查参数的类型。事情就是这样，四分之一世纪以来一直如此，现在抱怨也没有用。

无论您选择自己编写`equals()`和`hashCode()`重写还是让您的 IDE 来做，`equals()`都需要在不导致意外的空指针或类强制转换异常的情况下工作。

另一个期望或“契约”是两个相等的集合具有相同的哈希代码。为不同的集合保证不同的散列码是可取的，但在数学上通常是不可能的。

所以对`hashCode()`的测试应该只检查两个相等的集合是否有相同的散列码，而不是两个不同的集合是否得到不同的散列码。

同样，如果我们在抽象类中编写一个列表应该有的`equals()`，那么我们也应该编写一个列表应该有的`hashCode()`。那么 list 子类不需要覆盖任何一个，set 子类必须覆盖两个。

这意味着抽象类`hashCode()`应该受到元素顺序的影响。列表{1，2，3}的哈希应该不同于列表{2，1，3}。

如果您选择通过 TDD 过程自己创建`equals()`和`hashCode()`，放入这些存根以确保您得到失败的第一个测试:

```
 // TODO: Write tests for this
    @Override
    public boolean equals(Object obj) {
        return false;
    } // TODO: Write tests for this
    @Override
    public int hashCode() {
        return 0;
    }
```

并确保您有以下测试来指导您的`equals()`函数的编写:

*   参照相等测试:`x.equals(x)`为真
*   空不等式测试:`x.equals(null)`为假
*   类相等测试:如果`y`与`x`不是同一个类，则`x.equals(y)`为假
*   集合大小测试:如果`x`没有`y`那么多的元素，`x.equals(y)`为假
*   集合顺序测试:如果`x`和`y`都有相同数量的元素但是顺序不同，`x.equals(y)`为假，但是如果元素顺序相同，`x.equals(y)`为真(这是列表行为而不是集合行为，集合类可以覆盖)。
*   最后是对两个集合的测试，这两个集合除了指针之外都是相同的(这意味着它们在运行时是两个不同的对象)。

因为`ArrayBackedCollection<E>`是一个抽象类，我们希望`equals()`使用`getClass()`而不是`instanceof`。对于类不等式测试，我推荐使用`ArrayBackedCollectionImpl<E>`和一个匿名类实现，具有相同的`E`类型和相同的元素。

有时候，我认为引用等式优化对于我们的计算机来说是不必要的，因为我们的计算机比 Java 在 25 年前第一次出现时要快得多。

但是如果一个集合包含数百甚至数千个元素，如果不是绝对必要的话，我们当然希望`equals()`函数不必遍历集合中的元素。

对于集合大小测试，用完全相同的元素以相同的顺序创建两个相同具体类的集合。然后只在一个集合中再添加一个元素，这样该集合正好比另一个集合多一个元素。

收集顺序测试应该相当简单。我将对一个集合使用字符 A、B、C(在它们适当的对象包装器中),对另一个集合使用 A、C、B。或者一个是 1，2，3，另一个是 3，1，2。类似的东西。

这就只剩下测试两个集合之间除了引用相等之外的所有东西都相等。无论您选择使用哪种对象进行测试，只要确保在断言相等之前将相同的元素添加到两个集合中即可。

此时，如果您使用 IntelliJ，您应该在测试类中只有一个警告:静态嵌套类的复制构造函数没有用于任何事情。在我们研究这个之前，我认为我们应该先研究迭代器。

IntelliJ 可能会对测试中的类给出更多的警告。暂时忽略它们，因为我们稍后会清除它们。

# 启用迭代器

`Iterator<E>`接口允许我们的 Java 程序通过 For Each 循环遍历 Java 集合框架中集合的任何实例。我们可以将这种能力添加到我们自己的集合中。

去年，我写了一篇关于迭代器的文章。我在那篇文章中给出的解决方案太长了。我只能用比更简洁的解决方案更容易理解的希望来证明它。

最简洁的解决方案是使用匿名类。我真的不喜欢匿名类，我认为它们被过度使用到了严重降低可重用性的程度。

使用非静态嵌套类可能是拥有一个单独的类和拥有一个匿名类之间的折中方案。这是我在上面的草稿里写的。

当然，我们仍然需要编写测试。只是不要从空集合迭代器测试开始，因为它可能会通过。对于非空集合，单个元素可能就足够了。大概是这样的:

```
 @Test
    void testIteratorHasNext() {
        ArrayBackedCollection<LocalDateTime> collection
                = new ArrayBackedCollectionImpl<>();
        **collection.add(**LocalDateTime.now()**);**
        **Iterator<**LocalDateTime**> iterator = collection.iterator();**
        String msg = "Iterator for non-empty should have next";
        **assert iterator.hasNext() : msg;**
    }
```

在`ArrayBackedIterator<E>`中的一个简单改变就可以通过测试。

那么接下来的测试显然是`testIteratorDoesNotHaveNext()`。该测试的`E`类型实际上是不相关的，因为该类型的元素实际上不会被添加到集合中。

让那个测试通过，同时也有先前的测试通过，这就有点复杂了。首先，我们需要在内部类中添加一个私有字段。

```
 private int index = 0;
```

然后将`hasNext()`改为对照外部类的`nextUp`字段实际检查`index`:

```
 @Override
        public boolean hasNext() {
            return **this.index < ArrayBackedCollection.this.nextUp**;
        }
```

还不需要改变`next()`中的任何东西。有了这个改变，现在所有的测试都应该通过了。

下一个测试是针对`next()`的，我们只是想看看一个集合的迭代器是否可以返回这个元素。测试将失败，因为`next()`返回 null，而不是集合的第一个元素。

要通过测试，只需让`next()`给出集合的元素 0。

```
 @Override
        public E next() {
            return **(E) ArrayBackedCollection.this.elements[0]**;
        }
```

我们现在在 IntelliJ 中有五个警告(在 NetBeans 中只有一个)，但是所有的测试都应该通过。单元素集合测试还不需要`next()`对`index`做任何事情。

为此，我们需要一个测试，该测试可能会产生两个或更多伪随机元素，将每个元素逐个放入一个数组和一个集合中。然后测试使用迭代器逐个检索元素，并断言由`next()`给出的元素是测试期望的元素。

我的第一个想法是使用迭代器创建一个单独的集合，其元素与第一个集合相同，但这有无限循环的风险。我不认为这是一次失败的测试或通过的测试，我认为这是一次不测试。

而是用一个数组写一个测试，在一个有界循环中逐个断言，这样当`next()`连续两次给出第一个元素，而不是第一个元素后面跟着第二个元素时，测试就会失败；不会陷入死循环。

看到它失败，然后进行以下更改:

```
 @Override
        public E next() {
            return **(E) ArrayBackedCollection
                   .this.elements[this.index++]**;
        }
```

测试通过。所有的测试都应该通过，被测试的类应该只剩下三个警告。

我们几乎完成了迭代器。如果`hasNext()`为假，无论如何再次调用`next()`都会导致`NoSuchElementException`(来自`java.util`包)，而不是数组索引越界异常。我把写测试的任务交给你了。

还有一个问题是是否取消对从`Object`到`E`的未检查演员的警告。我想这次我们不得不这么做了。但至少，因为它是一个抽象类，我希望我们不必在子类中处理这个问题。

# 启用复制构造函数

我故意将复制构造函数排除在前面的练习之外。我认为这个练习现在是讨论它的适当地方。

拥有复制构造函数的特定类是有用的，但是我认为，通过泛化，复制构造函数变得更加有用。有了复制构造函数，我们也许能够做一些事情，比如将列表转换成集合，从而丢弃重复的元素。

不过，对于我们的测试，我们将使用同一个类的两个实例，这样我们就可以使用一个简单的等式断言。第一个实例应该有一些元素，类型`E`并不重要。

然后，对复制构造函数的调用应该创建一个新的实例，该实例具有相同顺序的相同元素。但是当然第一次测试会失败。`ArrayBackedCollectionImpl<E>`复制构造函数只是遵从`ArrayBackedCollection<E>`复制构造函数，这是应该的。

并且该复制构造器当前是仅创建大小为 20 的新集合的草稿。在我们改变之前，让我们运行测试，确保第一次确实失败了。我刚想起来我还没用`toString()`做过任何事。但是多亏了散列码，测试结果是令人放心的:

> **预期:**集合。arraybackdcollectiontest $ arraybackdcollectionimpl @ 62 faaa FAA
> **实际:**集合。arraybackdcollectiontest $ arraybackdcollectionimpl @ 0

十六进制数字 62FAAFAA 告诉我们预期的集合有一些元素，而数字 0 告诉我们实际的集合几乎肯定没有元素。

我试图使用迭代器从一个集合复制到副本，但是这太有问题了。所以我选了更简单的。

```
 public ArrayBackedCollection(ArrayBackedCollection<?
            extends E> collection) {
        **this.elements = new Object[collection.elements.length];
        this.nextUp = collection.nextUp;
        System.arraycopy(collection.elements, 0, this.elements, 0,
                this.nextUp);**
    }
```

在这里，我还首先编写了一个 For 循环来复制后台数组，并让 IDE 将它改为一个`arraycopy()`调用。

显然，这在 set 类中是行不通的。我想我们只能为 set 类提供一个从头开始编写的复制构造函数。

# 将一般化应用于特定的类

在继续之前，对您想要使之成为`ArrayBackedCollection<E>`的子类的类运行测试。

所有的测试都应该通过，这样我们就可以确定在引入抽象超类之前，这些测试都工作正常。如果任何一个测试失败了，回顾那个测试并相应地处理它。

我建议你不要删除你对预期子类的任何测试，如果你没有得到它们的任何错误或警告，并且它们没有互相矛盾。

另外，我希望得到一些行数，但不要担心太精确。我得到了:

*   `ArrayBackedList`运行 197 行
*   `ArrayBackedSet`运行 158 行
*   `ArrayBackedCollection`运行 181 条线路

您的行数可能会略有不同。

另外，如果您取消了 list 类或 set 类中的任何警告，我希望您不要取消它们，至少现在不要。我对`ArrayBackedList`有三个警告，对`ArrayBackedSet`没有错误或警告。

现在让我们将抽象超类应用于具体的类。我先用`ArrayBackedList<E>`试了一下。我把“`implements Iterable<E>`”换成了“`extends ArrayBackedCollection<E>`”(记住接口是继承的)。

这在 IntelliJ 中导致了三个错误和三个警告(在 NetBeans 中还有更多警告)。

第一个错误是针对`expandCapacity()`程序的。NetBeans 希望您添加一个`@Override`注释，同时确认它在超类中被标记为 final。我想我在集合课上把它叫做别的东西了。我只是从子类中删除了整个过程。

这就解决了两个错误，现在我们只剩下主构造函数的错误了，初始容量是一个整数，它需要调用超类构造函数。

还要简化主构造函数以使用超类构造函数。因为`ArrayBackedCollection<E>`构造函数检查`initialCapacity`不是负数，所以`ArrayBackedList<E>`构造函数没有理由也这样做。

现在 IntelliJ 中有四个警告，NetBeans 中有九个。首先，`ArrayBackedList<E>`不再需要定义自己的`elements`数组，所以删除它。还要删除`nextUp`，即使没有警告(NetBeans 会警告字段隐藏，但 IntelliJ 不会)。

NetBeans 会警告您缺少覆盖注释，而 IntelliJ 不会。在这种重构的情况下，拥有那些丢失覆盖注释的警告非常有帮助。

除了没有索引参数的`add()`和 remove by element，我们可以删除 NetBeans 告诉我们需要在`ArrayBackedList<E>`中覆盖注释的所有内容。这包括`equals()`和`hashCode()`。

但是确保不要删除调用`moveElementsForward()`的`remove()`。相反，添加`@Override`注释来清除 NetBeans 警告。

IntelliJ 中有两个未检查的强制转换警告，分别针对 get by index 函数和 remove by index 函数。这些函数应该返回`E`的实例，但是`elements`的类型是`Object[]`。镇压可能是唯一的解决办法。

NetBeans 并没有对我弃用`ArrayIterator<E>`而选择`ArrayBackedIterator<E>`给出警告，但它确实展示了早期的带删除线的迭代器实现。

只需从子类中完全删除`iterator()`，因为抽象超类会处理它。这可能会对未使用的导入产生一两个警告，这很容易解决。

检查未使用的私有函数和私有过程。这些是 IntelliJ 中的警告，而不是 NetBeans 中的警告。

最后，删除`DEFAULT_STARTING_CAPACITY`常量，代之以使用超类中的`DEFAULT_INITIAL_CAPACITY`常量。

运行`ArrayBackedListTest`中的所有测试，确保一切正常。

现在我已经把`ArrayBackedList<E>`减少到了 97 行。我已经把它的行数减半了:以前有将近 200 行，现在只有将近 100 行。我不认为我能从`ArrayBackedSet<E>`中切掉那么多。

与`ArrayBackedList<E>`一样，确保`ArrayBackedSet<E>`的所有测试都通过。

在`ArrayBackedSet<E>`类声明中增加`extends ArrayBackedCollection<E>`，替换`implements Iterable<E>`这一次，让我们继续删除子类的`elements`和`nextUp`字段，以及默认的初始容量常数。

与 list 类一样，set 类中的扩展容量覆盖现在不再是不必要的，而且也是无效的，因此我们将其删除。

接下来，简化主构造函数，只调用带有`initialCapacity`参数的超类构造函数，然后它不应该做任何其他事情。

但是现在我们还有一个错误要处理:因为`indexOf()`在超类中是包私有的，所以我们不能让它在子类中是私有的。我们可以扩大子类中的访问范围，但不能缩小。

所以删除子类`indexOf()`就行了。除了测试类，只有来自其他包的类应该实例化`ArrayBackedSet<E>`。因此，正确的封装仍然是强制性的。

此时，我意识到我忘记了在预期子类中添加拒绝 null 的测试。所以我做了一点题外话来处理一下。

好了，回到让`ArrayBackedSet<E>`成为我们新设计的`ArrayBackedCollection<E>`的子类。

使用`ArrayBackedList<E>`，我们能够删除单参数`add()`，但是必须保留单参数`remove()`覆盖。对于`ArrayBackedSet<E>`,情况正好相反:我们丢弃了在创建抽象超类之前编写的`remove()`,但是我们不应该保留`add()`,我们应该调整它。

除了检查预期的新元素是否已经在集合中，集合的`add()`本质上与超类`add()`相同:它执行空检查，并检查后备数组是否足够大以容纳新元素。

所以我们把它简化成

```
 **@Override**
    public boolean add(E element) {
        if (this.contains(element)) {
            return false;
        } **else {
            return super.add(element);
        }**
    }
```

我们完全可以查克纳`contains()`、`size()`、`isEmpty()`、`clear()`、`iterator()`。

现在重新运行测试，以确保一切仍然正常工作。大多数测试都通过了，但是还有几个问题。

一个是，当初始容量参数为负时，set 测试类预期负数组大小异常，但是超类的测试类预期非法参数异常。

我们不应该有矛盾的测试。所以我简单地修改了 set test 类，以符合超类的功能。如果我是这类事情的绝对坚持者，我也会倒回超类，让子类测试失败，然后从那里开始，但是我不会那样做。

解决了这个问题之后，还有一个问题，我让 set class 抛出一个空指针异常，试图向 set 中添加 null，而不是让它返回 false。我再次选择只改变子类和测试类，以符合超类。

有了这些改变，我已经把`ArrayBackedSet<E>`从 158 行减少到 91 行。没有《T1》那么戏剧化，但仍然比我预期的要多得多。

# 从头开始子类化

我考虑过使用队列和排序列表作为集合类的例子，从一开始就从`ArrayBackedCollection<E>`子类化，但是两者都不令人满意。

然后我从 Apache Commons collections 框架中了解到了`Bag<E>`。它不是 Java 开发工具包(JDK)的一部分，但它是`java.util.Collection<E>`的一个子接口……但它“违反”了该接口的不少“契约”。

更糟糕的是，他们计划重写该接口，以便未来的版本符合`java.util.Collection<E>`“契约”，这当然有可能破坏向后兼容性。

这里我们没有从`java.util.Collection<E>`继承子类，我们可以自由地改变`ArrayBackedCollection<E>`可能附加的任何显式或隐式的“契约”。

一个包很像一个列表。箱包和清单都允许重复元素。但是 Apache Commons `Bag<E>`的特别之处在于它提供了函数`getCount()`和`uniqueSet()`。

`getCount()`功能给出袋子中特定元件的计数。`uniqueSet()`功能提供了一个`java.util.Set<E>`，它具有与包相同的所有元素，但当然没有任何重复。

Apache Commons `Bag<E>`接口有三个官方实现:`DefaultMapBag<E>`、`HashBag<E>`和`TreeBag<E>`。这三个都是地图支持的，没有一个是数组支持的。

我们要制作`ArrayBackedBag<E>`。我们当然不会把这个发给 Apache Commons，这只是一个练习。

在任何情况下，我们都没有实现 Apache Commons `Bag<E>`接口，因此我们将尽可能使用`E`而不是`Object`，因为我们不受 1.5 版之前 Java 兼容性的限制。

我可以想到两种不同的方法来编写测试类`ArrayBackedBagTest`。

我们可以在`ArrayBackedBag<E>`中放置许多可能导致测试失败的存根，然后我们编写`ArrayBackedBagTest`，就好像`ArrayBackedCollection<E>`不存在一样。

然后，我们通过从`ArrayBackedBag<E>`中删除存根来通过大部分测试。

或者我们可以让我们的`ArrayBackedBag<E>`初稿只包含 IDE 给我们开绿灯或黄灯所需的最少内容。那么我们的测试只针对特定于`ArrayBackedBag<E>`的功能。

我打算采用后一种方法，至少是为了不让这篇文章变得比现在更长。如果你更喜欢前一种方法，你可以自由地这样做，只是要预先警告，所有的重复可能会让你紧张。

下面是测试中的类`ArrayBackedBag<E>`的框架:

IntelliJ 给了我六个警告，但是没有错误，所以我们可以继续了。

对于测试类，我为我将要使用的`E`类型添加了导入，也许还有`java.util.Random`有一个伪随机数发生器。

我们的第一个测试是针对`getCount()`而不是`add()`的，因为我们相信超类已经覆盖了这一点。至少在我们进行一项要求我们超越`add()`的测试之前。

如果包是空的，`getCount()`应该为调用者可能查询的任何元素返回 0(元素的类型是`E`，而不是`Object`)。

```
 @Test
    void testGetCountOfAbsentElement() {
        ArrayBackedBag<LocalDateTime> bag = new ArrayBackedBag<>();
        assertEquals(0, bag.getCount(LocalDateTime.now()));
    }
```

测试失败是因为`getCount()`返回一个负数而不是 0。在这一点上，如何让它通过应该是显而易见的。

如果一个元素只被添加到一个包中一次，它的计数应该是 1。

```
 @Test
    void testGetCount() {
        ArrayBackedBag<BigInteger> bag = new ArrayBackedBag<>();
        BigInteger number = new BigInteger(72, RANDOM);
        bag.add(number);
        String msg = "Since " + number.toString()
                + " was added to the bag, its count should be 1";
        assertEquals(1, bag.getCount(number), msg);
    }
```

暂时不需要覆盖`add()`。

```
 public int getCount(E element) {
        return **this.contains(element) ? 1 : 0**;
    }
```

到目前为止应该能通过两项测试。

所以现在我们需要一个测试，它实际上要求`getCount()`遍历后备数组寻找`element`以通过测试。或者，更好的是，元素计数随着元素的添加而更新。

我能想到的最简单的测试是，将某种`E`类型的某种元素添加到一个袋子中，进行几次伪随机 *n* 次。那么`expected`将类似于`RANDOM.nextInt(100) + 10`。

或者我们可以使用`Integer`作为`E`类型，然后测试从 0 到 *n* 循环，将 0 添加到包中 0 次，仅添加 1 次，添加 2 次，以此类推，直到 *n* 。或者可能只是从 2 到 *n* ，因为情况 0 和 1 已经被前面的测试覆盖了。

好吧，我会把它留在那里。

# 概括起来

集合是一个很好的例子，说明了面向对象的多态性如何帮助我们减少不必要的重复，提高程序组件的可重用性。

在可变数组支持集合的情况下，我们看到它们共享许多功能，这些功能可以方便地放在一个公共超类中。

这个练习显示了抽象类非常有用，无论它是对现有类层次结构的追溯添加，还是用于简化新的实现，或者两者兼而有之。