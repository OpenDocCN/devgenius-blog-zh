# 数据结构练习:Java 中的数组支持集，带 TDD

> 原文：<https://blog.devgenius.io/data-structures-exercise-array-backed-set-in-java-with-tdd-9a54cc06e5e5?source=collection_archive---------0----------------------->

![](img/1c416e4c97401882537136b2bb06036e.png)

皮特·亚历克索普洛斯在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上的照片

改造现有的数据结构是一个很好的计算机编程练习。今天，我们将使用测试驱动开发(TDD)方法，在 Java 中创建一个数组支持的可变集合。

我希望你遵循你最喜欢的 Java 集成开发环境(IDE ),不管是 Eclipse、Apache NetBeans、IntelliJ IDEA 还是其他什么。

集合就像一个列表，但是有两个重要的区别:一个集合不能包含重复项，顺序无关紧要。在内部，元素必须有一定的顺序，但是这个顺序对用户来说不重要(如果顺序很重要，他们应该使用不同的数据结构)。

这使得移除集合中的元素比移除列表中的元素容易得多。假设我们想从一个数组支持的列表中移除一个元素。这可能需要向前推进几个要素。

对于数组支持的集合，移动末尾的内容来占据要移除的元素空出的位置可能是一件简单的事情。

数组支持的集合与数组支持的列表的共同之处在于，支持集合或列表的数组会在必要时自动扩展数组容量。我们很快就会谈到具体细节。

Java 开发工具包(JDK)中最著名的 set 实现可能是`HashSet`。对于较大的集合，set 实现比简单的数组支持的实现更有效，但它也比适合本练习的实现更复杂。

但是，如果您成功地完成了这个练习，那么您应该不会遇到使用散列表进行集合练习的问题。

除了没有哈希表，我们的数组支持的集合将非常类似于`HashSet`，但是我们不允许在我们的数组支持的集合中有 null 元素。

# 第一步

以下是我们阵列支持的 set 实现的框架:

注意，主构造函数需要一个初始容量参数。如果您像我一样，您更喜欢使用填充默认初始容量的辅助构造函数。

但是默认的初始容量应该是一个实现细节，一个不对外公开的值。出于这个原因，我们至少需要在一个测试中直接使用主构造函数。

接下来，让您的 IDE 为`ArrayBackedSet`启动一个测试类，使用 JUnit 4，如果您的系统上有 JUnit 5，最好使用 JUnit 5。

我们应该使用什么元素类型`E`进行测试？最好是一个具有定义良好的`equals()`函数的不可变类(来自`Object`的`equals()`不算)。如果`E`实现`Comparable`就好了。

我建议用`java.time.LocalDateTime`。将它的导入添加到测试类`ArrayBackedSetTest`。

我们也可以使用`String`，它也有一个`equals()`超驰并且是`Comparable`。或者我们不必局限于单一类型，我们可以有一些使用一种类型的测试和其他使用其他类型的测试。

我也开始喜欢在测试类中有一个私有的`java.util.Random`实例，但是如果你愿意的话，你可以一直使用`Math.random()`。

`add()`函数的工作方式应该是，如果元素被成功添加到集合中，它返回 true，如果它已经在集合中，则返回 false(因此没有必要再次添加它)。

这是第一个测试。如果您使用的是 JUnit 4，请务必添加“`public`”。

```
 @Test
    void testAdd() {
        ArrayBackedSet<LocalDateTime> set = new ArrayBackedSet<>();
        LocalDateTime dateTime = LocalDateTime.now();
        String msg = "Should be able to add " + dateTime.toString()
                + " to set of times";
        assert set.add(dateTime) : msg;
    }
```

它应该会失败，因为现在`add()`除了返回 false 之外什么也不做。我们简单地通过让`add()`返回 true 来通过这个测试，而不需要对内部`elements`数组做任何事情。

```
 public boolean add(E element) {
        return **true**;
    }
```

此时不需要重构。

我之前提到过，out set 不允许 null，不像`java.util`中的集合。但是尝试添加 null 应该抛出 null 指针异常还是应该简单地返回 false？

你决定并相应地编写测试。称这个测试为“添加拒绝空值”如果您简单地返回 false，您可以通过这个小小的修改来通过测试:

```
 public boolean add(E element) {
        return **element != null**;
    }
```

我知道这感觉很傻。在每一个 TDD 周期，我们试图只做通过测试所必需的最少的工作。这不是因为我们懒，而是因为我们想找到最简单、最优雅的解决方案。

趁我还没忘记，你应该写一份否定能力的测试。主构造函数应该通过抛出适当的异常来拒绝负容量。如果您使用的是 JUnit 5，请使用`assertThrows()`。

# 检查元素是否已经在集合中

在这一点上，我们还不需要改变`add()`来实际添加任何东西到集合中，就像下一个测试不需要我们修改`contains()`来实际遍历后备数组寻找指定的元素。

我要你写下一个测试。它应该实例化一个新的`ArrayBackedSet<E>`(您选择类型`E`)，添加一个元素，然后断言集合包含该元素。

我们也可以通过改变布尔函数返回的内容来通过这个测试。

```
 public boolean contains(E element) {
        return **true**;
    }
```

下一个测试实际上需要我们重写`add()`来添加元素到集合中，重写`contains()`来检查指定的元素是否在集合中。

该测试应该向集合中添加一些元素，然后断言该集合不包含从未添加的元素。我认为这个测试在断言任何东西之前向集合中添加多个元素是很重要的。

例如，使用`LocalDate`作为我们的`E`类型，这个不包含测试可以将过去日期的`LocalDate`对象添加到一个集合中，然后断言该集合不包含明天日期的`LocalDate`实例。

我们现在要做的事情似乎很清楚了。但是我们不要急于求成:因为我们还没有对重复元素的测试，`add()`不应该检查要添加的元素是否已经在集合中。

我建议我们添加一个私有整数字段，`nextUp`，它将指向`elements`中准备好新值的下一个数组槽。它当然会被初始化为 0。我们这样重写`add()`:

```
 public boolean add(E element) {
        **this.elements[this.nextUp] = element;
        this.nextUp++;**
        return true;
    }
```

而`contains()`这样:

```
 public boolean contains(E element) {
        **boolean flag = false;
        int index = 0;
        while (!flag && index < this.nextUp) {
            flag = element.equals(this.elements[index]);
            index++;
        }**
        return **flag**;
    }
```

这应该通过不包含测试，以及先前的测试。这里至少有一个重构的机会:“`flag`”不是很有意义。也许应该是“`found`”…

`ArrayBackedSetTest`上可能有两个警告。一个可能是因为我们还没有使用来自`Assertions`的任何东西(或者`Assert`如果你正在使用 JUnit 4)。或者你在消极能力测试中。

第二个警告是因为我们在 Add 测试中的断言依赖于一个“不纯”的函数(`add()`现在改变了`elements`数组和`nextUp`字段)。

要消除该警告，只需将`set.add(dateTime)`提取到一个局部变量中，并对其进行断言。如果你想不出更好的名字，就叫它“T1”。

你们中的一些人可能想知道为什么我们的`contains()`函数有一个`E`类型的参数，而不是`Object`，就像我们在 JDK 的模型一样。

那只是因为向后兼容。在 Java 1.5 中增加泛型之前，Java 就有集合了。

由于我们正在为这个练习重新编写`ArrayBackedSet<E>`，我们不会受到向后兼容 Java 1.4 或更早版本的限制。

接下来，我想编写一个测试来尝试添加重复的元素。但是我认为，在我们编写测试之前，我们需要让`size()`正常工作。

所以首先编写一个测试，向集合中添加一些不同的元素，跟踪有多少，然后断言`set.size()`返回多少。

为了通过测试，我们只需将`size()`中的 1 改为`this.nextUp`。

```
 public int size() {
        return **this.nextUp**;
    }
```

我认为现在我们已经拥有了编写重复元素测试所需的一切。如果一个元素已经在集合中，`add()`应该返回 false，而`size()`应该返回与调用`add()`之前相同的数字。

```
 @Test
    void testNoDuplicateElements() {
        ArrayBackedSet<String> set = new ArrayBackedSet<>();
        String msg = "Should not be able to add same element twice";
        set.add(msg);
        int expected = set.size();
        boolean reAddFlag = set.add(msg);
        assert !reAddFlag : msg;
        int actual = set.size();
        assertEquals(expected, actual);
    }
```

让这个测试通过的最简单的方法是通过 If 语句给`add()`添加一个`contains()`检查。

```
 public boolean add(E element) {
        **if (this.contains(element)) {
            return false;
        }**
        this.elements[this.nextUp] = element;
        this.nextUp++;
        return true;
    }
```

你们中的一些人可能更喜欢用 If-Else，这很好。对于那些不记得重新格式化键盘快捷键的人来说，敲几次空格键来修复缩进并不太麻烦。

也许有一些聪明的方法可以在没有 If 语句的情况下通过这个测试。如果你知道这样做的方法，而且不是一个愚蠢的解决方法，请在评论中告诉我。

# 根据需要扩展容量

假设后备数组有 20 个槽，都是满的，并且有一个调用来添加第 21 个元素。将数组大小加倍可能太多了。只添加一个阵列插槽可能太少了。

这表明折衷为 3/2，根据需要进行整数舍入。例如，如果后备阵列有 16 个插槽，而需要第 17 个，则将阵列容量增加到 24 个，而不是 32 个。

如果以后需要，阵列容量将增加到 36、54、81，然后是 121 或 122，具体取决于 243/2 是向上取整还是向下取整，以此类推。

对于下一个测试，使用主构造函数来指定初始容量。那么测试应该添加足够的不同元素来填充初始容量。接下来是关键时刻:测试为这个集合增加了另一个元素。

为测试失败的`ArrayIndexOutOfBoundsException`编写一个 Catch 子句。或者，如果您使用 JUnit 5 或 TestNG，请使用`assertDoesNotThrow()`。这个测试将会失败，因为`add()`将会导致一个异常被抛出。

如果这是 C++，IDE 可能会警告我们，但是试图访问最后一个索引之外的数组的确切效果将是不可预测的。

如你所知，在 Java 中，数组的大小在初始化后是固定的，不能扩展。但是，将一个数组的内容复制到另一个容量更大的数组，然后将原来的数组指针改为指向新的数组，这是完全有效的。

我建议我们在一个名为`expandCapacity()`的私人程序中进行。然后`add()`检查容量并在必要时调用`expandCapacity()`。

```
 public boolean add(E element) {
        if (this.contains(element)) {
            return false;
        }
        **if (this.nextUp == this.elements.length) {
            this.expandCapacity();
        }**
        this.elements[this.nextUp] = element;
        this.nextUp++;
        return true;
    }
```

我把`expandCapacity()`的写作留给你。提示:IntelliJ IDEA 可能会建议您使用`System.arraycopy()`。NetBeans 可能会给你同样的提示。

一旦你让`expandCapactiy()`工作，我们所有的测试都应该通过，并且`ArrayBackedSetTest`应该显示所有的绿色。

但是`ArrayBackedSet`可能会显示四个警告，如果你使用 IntelliJ 的话，因为我们还没有编写像`remove()`这样的函数(NetBeans 不会警告你未使用的公共函数)。

# 从集合中移除元素

我继续编写了一个测试来删除集合中实际存在的元素。但后来我意识到我有点超前了。

如果你小心确保它正确地工作，而不是走得太远，删除功能实际上是相当复杂的。

因此，我希望您编写的下一个测试是实例化一个新集合，但不向该集合添加任何元素的测试。然后，测试试图删除适当类型的一些元素，这应该是不可能的，因为集合中根本没有元素，所以`remove()`返回 false。

这个测试应该会失败，因为现在它返回 true。让它通过只需要我们对`ArrayBackedSet`做一点小小的改变:

```
 public boolean remove(E element) {
        return **false**;
    }
```

现在，我们编写测试来删除集合中实际存在的元素。我也把这个留给你。只要确保第一次失败是因为正确的原因。

我写的测试感觉有点长。这将是一个好主意，使一个单独的私有函数，使一个集合与元素被删除添加在一些伪随机位置。

更好的是，使用`Integer`作为`E`类型，包含从 0 到某个伪随机 *m* 的整数。然后选择一个至少为 0 但不大于 *m* 的伪随机 *n* ，去掉 *n* 。断言`remove(` *n* `)`返回真，`contains(` *n* `)`返回假，`size()`返回比之前少一。

如果你不喜欢在一个测试中有三个断言，那就继续做三个独立的测试……实际上，我越想越觉得这个测试应该分成三个独立的测试:

1.  首先编写一个测试，当对集合中存在的元素调用时，断言`remove()`返回 true。通过让`remove()`简单地返回`this.contains(element)`来通过这个测试。
2.  接下来编写一个测试来断言，在对存在于集合中的元素调用`remove()`之后，该元素不再存在于集合中，因此`contains()`返回 false。通过用 null 替换后备数组中的元素来通过这个测试(但是要注意:根据您编写`contains()`的方式，这可能会导致 null 指针异常)。
3.  最后，编写一个测试来断言成功移除一个元素会将集合的大小减少 1。只需在适当的位置添加行“`this.nextUp--;`”即可通过该测试。

现在我意识到了另一种超越自己的方法:不需要在后备数组中移动任何元素就可以通过上述三个测试。

这可能导致这样的问题，即删除一个元素会无意中导致另一个元素的意外删除。

这意味着我们需要编写一个测试，断言在删除一个元素之后，添加到集合中的所有其他元素到目前为止仍然在集合中。

将该测试称为“仅删除目标元素”最简单的`E`类型也可能是`Integer`。

如果我们没有超越自己，这个新的测试第一次应该会失败，如果我们按照我上面概述的三个步骤编写了`remove()`。

通过这个测试的一个方法是将被移除元素之后的所有元素在后备数组中向前移动一个槽。这对于数组支持的列表来说是合适的。

但是请记住，顺序与集合中的调用方无关。调用者关心的是一个元素是否在集合中，而不是这个元素在后备数组中的位置。

因此，通过这个测试最直接的方法就是用后备数组中的最后一个元素覆盖要删除的元素的数组槽。

例如，如果要从{1，2，3，4，5}中删除 3，可以将后备数组改为{1，2，5，4，null}，当然`this.nextUp`调整为指向 4 而不是 null。

实际上，后备数组可以改为{1，2，5，4，5}，并且仍然通过所有测试，只要`this.nextUp`被调整为指向 4 而不是第二个 5。这是一个测试无法检查的实现细节，因为`this.elements`拥有私有访问权限。

现在我已经为`remove()`准备了大约四十行测试。如果我在一次测试中完成`remove()`的所有测试，可能会有 30 行代码。

然而，一般来说，我们应该更担心被测试的类中的长单元，而不是进行测试的类中的长单元。

我的第一份`remove()`工作草案大概有二十行。

```
 public boolean remove(E element) {
        **boolean found = false;
        int index = 0;
        while (!found && index < this.nextUp) {
            found = element.equals(this.elements[index]);
            index++;
        }
        if (found) {
            index--;
            int lastIndex = --this.nextUp;
            if (index < lastIndex) {
                this.elements[index] = this.elements[lastIndex];
            }
            this.elements[lastIndex] = null;
            return true;
        } else {**
            return false;
        **}**
    }
```

所有的测试现在都应该通过了。这是做一些重构的最佳时机。重构的机会应该很明显。如果不明显，复习`contains()`。

`contains()`和`remove()`都遍历`elements`数组，寻找特定的元素。这种重复并不完全是一字不差的，因为差异是非常表面的。

我们应该创建一个私有函数，称之为类似于`indexOf()`的东西，它的工作方式很像`String`的`indexOf()`函数。如果在后备数组中找到指定的元素，`indexOf()`返回索引。否则，它返回 1。

然后我们重写`contains()`和`remove()`来使用`indexOf()`。重写后的`contains()`可以放在一行中，但是三行更清晰也更简洁。

使用私有的`indexOf()`也会使`remove()`变得更短，但不会更短。

```
 public boolean remove(E element) {
        **int index = this.indexOf(element);** if **(index == -1)** {return false;} else {int lastIndex = --this.nextUp;
            if (index < lastIndex) {
                this.elements[index] = this.elements[lastIndex];
            }
            this.elements[lastIndex] = null;
            return true; **}**
    }
```

调用私有函数而不是内联可能会花费我们几分之一毫秒的时间。在我看来，清晰的收益和随后进行更改的便利是值得的。

虽然也许我们可以通过确保`indexOf()`只迭代到`nextUp - 1`，而不是`elements.length - 1`来重新获得那一毫秒的分数。

现在运行所有测试，确保重构没有破坏任何东西。

# 检查集合是否为空

数学家处理的大多是无限集合和空集。作为程序员，我们主要处理有限的集合，但是我们也处理空集，比如当搜索没有结果时。

`java.util.HashSet`有`isEmpty()`功能是有道理的，`ArrayBackedSet<E>`也是，只是我们的还不行。会的，在我们为它写了一个测试之后。

```
 @Test
    void testIsEmpty() {
        System.out.println("isEmpty");
        ArrayBackedSet<java.sql.Clob> set = new ArrayBackedSet<>();
        String msg = "Newly created set should be empty";
        assert set.isEmpty() : msg;
    }
```

元素类型`E`对于这个特定的测试并不重要，因为集合将从空开始并保持为空。所以我用了`Clob`。

这个测试应该会失败。你知道让它通过的诀窍。写下一个测试的诀窍，不是空测试，我也留给你们。

选择`E`类型。添加一个`E`的实例就足以使集合不为空。如果你使用`assert`而不是 JUnit 的`assertFalse()`，确保在断言中否定`set.isEmpty()`。

测试失败了，你已经知道如何让它通过。如果没有，可以很快搞清楚。

# 清理布景

现在我们在`ArrayBackedSet`中应该只有一个警告，警告告诉我们不使用`clear()`(至少在 IntelliJ IDEA 中是这样)，只有一个做注释，也涉及`clear()`。

我把为`clear()`写测试的任务交给你了。基本思想是实例化一个集合，用一些元素填充它，断言它不为空，然后调用`clear()`并断言该集合现在为空，其大小为 0。

到目前为止，我们编写的方法是，为了通过这个测试，将`nextUp`重置为 0 就足够了。

```
 public void clear() {
        **this.nextUp = 0;**
    }
```

但是这样做有一个潜在的严重问题:如果后备数组没有被新元素完全覆盖，我们的 set 实现可能会保留对对象的引用，否则垃圾收集器会丢弃这些对象，因为没有对这些对象的其他引用。

清除操作应该将后备数组中的每个非 null 元素设置为 null，这样就可以安全地丢弃不再需要的对象，从而释放可能对程序的其余部分有用的内存。

我仍然相信`elements`应该是`ArrayBackedSet`的私有物。尽管如此，还是有办法为`ArrayBackedSetTest`提供查询后备数组的方法，比如某种包私有函数，它可以揭示后备数组是否有任何非空值。

但是我对它的感觉还不够强烈，不能真正写它，所以我要继续写下去，加入一些行来清除后备数组。

```
 public void clear() {
        **for (int i = 0; i < this.nextUp; i++) {
            this.elements[i] = null;
        }**
        this.nextUp = 0;
    }
```

清理后备阵刚好到`nextUp`就停下来就够好了。Java 的一个优点是 Java 虚拟机会将对象数组初始化为空。

但是，如果您决定编写包私有后备数组查询函数，它可能会遍历整个后备数组。无论容量是否扩大，所需的数量始终可用，如`elements.length`。

# 比较两组

也许扩展阵列容量这件事一开始看起来令人生畏。但是现在，多亏了 TDD，它变得非常简单，并不那么有趣。

至少在我看来，更有趣的是平等测试。但是也许有了 TDD，这也会变得非常容易。

如果两个集合具有相同的元素，则这两个集合是相等的，而不管支持数组中元素的顺序如何，并且两个集合中的任何元素都不包含在另一个集合中。

例如，假设一组颜色由红色、绿色和蓝色组成，另一组颜色由蓝色、绿色和红色组成。支持数组肯定是不同的，但它们包含相同的元素。

这是 Java，我们还必须处理这样的可能性，即`equals()`的`obj`参数可能是不同的类型，或者甚至可能是 null。现在不要向我抱怨，向二十五年前的太阳微系统公司抱怨吧。

至于引用等式优化(`this == obj`)，你可能觉得对于今天更快的计算机来说没有必要。即便如此，对它进行测试也无妨，即使您的实现并没有使用它。

我希望您在`ArrayBackedSet`中放入一个总是返回 false 的`equals()`存根，以及一个总是为所有`ArrayBackedSet`实例返回 0 或其他常量的`hashCode()`覆盖。

然后，我希望你编写引用相等(`assert x.equals(x)`)、不等于 null ( `assert x != null`)和不等于另一个类(`assert x != y`，其中`y`是任何其他类的实例)的测试。

我知道你的 IDE 可以生成`equals()`和`hashCode()`覆盖(至少如果它是主要的 IDE 之一的话)。

这样一个生成的覆盖对于引用相等测试、不等于空测试和不等于另一个类测试来说应该很好。但是它可能需要被编辑以通过我们接下来要写的测试。

我重申，支持数组中的顺序对于集合相等比较来说并不重要。集合{1，3，5，7，9，2，4，6，8，10}与集合{1，2，3，4，5，6，7，8，9，10}相同。

但是如果一个集合比另一个集合多一个元素，这两个集合就不可能相等，因为一个集合至少有一个元素是另一个集合没有的。

这表明在逐个比较元素之前，`equals()`应该检查集合大小。编写一个测试，创建两个集合，一个比另一个稍大，并断言两个集合不相等。

如果 ide 生成的`equals()`覆盖比较了`this`和`obj`的`nextUp`字段，那么它应该通过不同大小集合测试。

它还可能通过一个测试，创建两个相同大小和相同类型的集合，但是具有不同的元素，并断言这两个集合不相等，尽管它们大小相同。

如果您没有使用 IDE 生成的覆盖，一定要进行相同大小但不同的元素测试。例如，你可以用一百个随机整数组成集合 *A* ，用另外一百个整数组成集合 *B* 。

就像，可能在一个循环中，测试添加数字 *n* 来设置 *A* 和数字 *n* 来设置 *B* ，例如，第一次迭代可能会将 508 添加到 *A* ，将 508 添加到 *B* 。然后，当循环完成时， *A* 和 *B* 具有相同的大小，但是它们应该具有不同的元素(当然，除了在伪随机数发生器只给出零的极不可能的情况下)。

使相同大小但不同元素通过测试的最简单方法可能是使用`java.util.Arrays`中的静态`equals()`。这可能就是 IDE 生成的覆盖所使用的。

IDE 生成的覆盖可能无法通过下一个测试，因为支持数组会有所不同:

```
@Test
    void testEquals() {
        System.out.println("equals");
        ArrayBackedSet<Character> alphabetSet 
                = new ArrayBackedSet<>();
        ArrayBackedSet<Character> pangramSet 
                = new ArrayBackedSet<>();
        char[] alphabet = "abcdefghijklmnopqrstuvwxyz"
                .toCharArray();
        for (char letter : alphabet) {
            alphabetSet.add(letter);
        }
        char[] pangram 
                = "The quick brown fox jumps over the lazy dog"
                .replace(**" "**, **""**).toLowerCase().toCharArray();
        for (char gram : pangram) {
            pangramSet.add(gram);
        }
        assertEquals(alphabetSet, pangramSet);
    }
```

`alphabetSet`的支持数组应该有 ASCII (Unicode Latin-1)顺序的字母。但是`pangramSet`的支持数组应该有相同的字母，但是顺序不同:t，h，e，q，u，I，c，k，b，r，o，w，n，f，x，j，m，p，s，v，l，a，z，y，d，g。

在考虑了几种通过测试的不同方法后，我决定创建一个私有的后备数组匹配函数。该函数只是遍历一个数组，检查给定的元素是否包含在另一个后备数组中，一旦在另一个数组中找不到匹配的元素就停止。

通过将它标记为 private，我们可以确保只有当两个集合的大小匹配时才调用它。这可能不是最优雅或最有效的解决方案。也许在重构过程中，或者在代码审查时，我会想到一个更好的主意。

两个空集应该被认为相等吗？也许只有当一个的类型`E`与另一个的类型`E`匹配时。但是由于类型`E`在运行时不可用，似乎没有办法检查这一点。我想知道他们在 Scala 中是如何做到的？我得调查一下…

# 其他集合操作

我决定在这个练习中省略三个重要的集合运算:并集、交集和补集，只是因为类型系统的问题。

两个或多个集合的联合就是将集合合并成一个集合(当然，两个或多个集合中的元素在联合的集合中只出现一次)。例如{G，B-平坦，D}、{C，E，G，B-平坦}和{F，A，C}的并集是{A，B-平坦，C，D，E，F，G}。

两个集合的交集是两个集合中出现的所有元素的集合。比如{G，B-平，D}和{C，E，G，B-平}的交集是{ B-平，G}。而{B-flat，G}和{F，A，C}的交集就是空集(因为这两个集合没有共同的元素)。

交集是一种交换操作，这意味着无论在哪个 set 实例上调用它，结果都是相同的(或者，如果您选择将它实现为静态函数，则参数的给定顺序也是相同的)。

一个集合和另一个集合的补集是另一个集合中不在第一个集合中的所有元素的集合。这是*而不是*一个交换运算。比如来自{G，B-flat，D}的{C，E，G，B-flat}的补码是{D}，来自{C，E，G，B-flat}的{G，B-flat，D}的补码是{C，E}(也可能是反过来，我对那种东西很困惑)。

因为类型`E`在运行时不可用，所以我们可能不得不满足于 union 是`ArrayBackedSet<Object>`的实例而不是最窄的适用类型这样的操作结果。

所以把这些集合操作看作是额外的奖励，不管你是否能找到一种方法使结果的`E`类型尽可能的窄。

# 结论，现在

看起来我们就要结束了。如果你遇到任何问题，你可以查阅你的 IDE 中的`java.util.HashSet`源码，或者[我的玩具实例 GitHub 库](https://github.com/Alonso-del-Arte/toy-examples)中的`collections`包。

但是我们没有提供遍历集合的方法。这实际上是一个完全不同的练习，在我写完这篇文章后，我写了另一篇关于的文章[。](/data-structures-exercise-making-a-collection-available-to-javas-for-each-loops-126e6cf8a877)

我要多做这些练习。也许我接下来应该做一个不可变的数据结构。我欢迎评论中的建议。