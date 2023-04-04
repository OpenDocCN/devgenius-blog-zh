# 数据结构练习:Java 中的 LRU 缓存，带 TDD

> 原文：<https://blog.devgenius.io/data-structures-exercise-lru-cache-in-java-with-tdd-b61687da866?source=collection_archive---------3----------------------->

![](img/1dde8956275fd62c3bc472a9a95857bc.png)

也许这不是 LRU 藏身地的最佳视觉比喻...照片由[埃里克·温特](https://unsplash.com/@eric_w1?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄

改造现有的数据结构是一个很好的计算机编程练习。今天，我们将使用测试驱动开发(TDD)方法，在 Java 中制作一个最近最少使用的(LRU)缓存。

LRU 缓存本质上是一个固定容量的队列，保存名称-值对。当一个名称-值对被添加到缓存中时，它被添加到队列的前面(或者它被添加到末尾，然后立即移动到前面)。

名称是检索值的关键字。当使用名称从 LRU 缓存中检索值时，如果名称-值对不在队列的前面，则将其移动到队列的前面。

当缓存已满时，仍然可以添加新的名称-值对，即使容量不会像其他数据结构(如数组支持的列表)那样扩展。

无论队列末尾的名称-值对是什么，最近最少使用的名称-值对都被移除(或“驱逐”或“遗忘”)，以便为新的名称-值对腾出空间。

理想情况下，名称类型是一个引用类型，实例化和比较相等性的成本非常低(比如像`Integer`这样的原始包装)。

值类型是一种引用类型，重新计算起来应该非常昂贵，比如可能因为它需要数据库检索或互联网连接。如果重新计算会更快，那么从缓存中检索就不值得了。

尽管没有任何东西禁止我们对名称和值使用相同的类型。出于测试目的，我们可能会这样做。

举个例子，假设我们正在用 Java 编写一个 Web 浏览器。我们可能使用`java.net.URL`作为名称，我们可能使用某种文档对象类型作为值。

如果用户打开了一堆标签页，并且忽略了其中的一个足够长的时间，当他或她回到那个被遗忘的标签页时，网络浏览器必须重新连接到互联网以重新获得 HTML 内容。

但是最近使用的选项卡位于缓存的前端，当用户频繁地在这些选项卡上来回移动时，就可以提供这些选项卡了。这只是为了举例，因为网页浏览器可能不是 LRU 缓存的最佳用例。

根据您使用的 Java 开发工具包(JDK)的版本，您可能会发现`java.util.Scanner`使用`sun.misc.LRUCache`来缓存正则表达式模式。

给定的类可能会频繁调用`Scanner`实例的`nextInt()`和`nextDouble()`函数。那么适当的正则表达式应该在 LRU 缓存的前面，而`Scanner`实例可能需要的其他正则表达式在 LRU 缓存的后面，如果它们在那里的话。

如果你想在你自己的项目中使用`sun.misc.LRUCache`，你当然可以导入它。但是您会得到一个警告，让您知道这是一个专有类，将来可能会被删除。

这个警告可以被抑制，但是需要一个大的规避来抑制它。一般来说，你最好留意警告，而不是压制或忽视它们。

如果您的项目绝对必须有一个 LRU 缓存，您应该考虑获得一个第三方 LRU 缓存，这样您就知道它是可靠的、真实的，可能是专有的，但意味着由开发它的组织之外的人使用。

但是，作为一项编程练习，从头开始编写自己的程序肯定是值得的。尽管可以随意将`sun.misc.LRUCache`作为一个模型，或者如果您在自己的实现中遇到问题。

这里有一个骨架:

如果你愿意，你可以把这个包命名为“`org.example.collections`”或者类似的名字，而不仅仅是“`collections`”

此时，最小和最大容量常数仅供参考。如您所见，构造函数实际上并不强制约束。我们应该在某个时候为此编写测试。

显然，最小容量必须是一个正整数。但是 1 没有任何意义，因为这意味着一个名称-值对只保留在缓存中，直到添加下一个名称-值对。我认为 4 是有意义的最小容量。

至于最大容量，我不确定。在我查看的 JDK 源代码中，`Scanner`声明了一个容量为 7 的模式缓存。因此，我认为 128 人的最大容量应该足以满足大多数(如果不是所有)目的。

注意，这是一个抽象类。这里只有一个抽象子程序，抽象函数`create()`，就像`sun.misc.LRUCache`一样。具体的子类必须定义`create()`，但是这个函数可能只能从`LRUCache`调用。

即便如此，`create()`需要受保护的访问，而不是包私有访问，这样它就可以被`collections`包之外的类实现。

当一个不在缓存中的名字被调用时，`retrieve()`函数调用`create()`计算出相应的值放入队列中(在`sun.misc.LRUCache`中，`retrieve()`被称为`forName()`)。

但是，如果名称在缓存中，则只需从缓存中检索相应的值，并将其返回给调用者，但是需要在某个地方显示最近的检索。

我省略了抽象的布尔函数`hasName()`。对于想要使用我们的`LRUCache`的人来说，工作量减少了，但是对于我们来说，工作量增加了，而且将来可能会有性能损失。

布尔`has()`函数是我为了使测试更容易而添加的。它是包私有的，所以它可以被`LRUCacheTest`访问，但是不能被其他包中的类访问。其他包中的类不应该访问`LRUCache`的内部工作。

当我们实际编写`retrieve()`函数时(目前它只返回 null)，我们可以利用`has()`函数。也许是，也许不是，到了那里就知道了。

按照`sun.misc.LRUCache`模型，我们还需要一个私有的静态过程，将指定索引处的数组元素移动到数组的前面。但是我们需要首先编写测试。

这里是`LRUCacheTest`的骨架:

我用的是 JUnit 5。如果您正在使用 JUnit 4，您将需要更改导入语句(例如，import `org.junit.Test`)。

这个测试类包含了一个私有的`collections.LRUCache`实现，它很像`Scanner`中`sun.misc.LRUCache`的匿名实现，直到名字和值类型的选择。

尽管如此，还是有一些重要的区别。首先，我们需要能够调用不同大小的`LRUCacheImpl`构造函数(比如测试最小和最大容量约束)。这意味着我们必须写一个构造函数。

还要注意初始化为 0 的`createCallCount`字段。每次调用`create()`时，它都会增加`createCallCount`。因此，如果`LRUCache`实例调用`create()`来重新计算一个值，而它本应该从队列中检索这个值，那么测试可以查询`createCallCount`并看到它增加了。

我们几乎准备好开始编写测试了。我将从 Harsha Vardhan 的[最有用正则表达式的优秀列表](https://javascript.plainenglish.io/the-7-most-commonly-used-regular-expressions-in-javascript-bb4e98288ca6)中抽取一些正则表达式。

虽然那篇文章是为 JavaScript 程序员准备的，但是正则表达式语法是相同的。Java 和 JavaScript 中的正则表达式是有区别的，但是我们不应该太在意这些区别。

我也想出了一些自己的正则表达式。如果本文中的正则表达式不是最聪明或最有效的，它可能是我想到的一个。

我们不必使用真实世界的例子来进行测试，我们可以创建一些虚拟类来进行测试。但是我更喜欢让我的测试比那更真实一些。

这是第一个测试:

```
 @Test
    void testAddToCache() {
        LRUCacheImpl cache = new LRUCacheImpl(DEFAULT_SIZE);
        String expected = "^[a-zA-Z0–9+_.-]+@[a-zA-Z0–9.-]+$";
        String actual = cache.retrieve(expected).pattern();
        assertEquals(expected, actual);
    }
```

如果您使用的是 JUnit 4，您需要将测试公开，而不是将包私有。

正则表达式用于电子邮件地址验证(Harsha 列出的最有用的正则表达式中的第二个)。

由于`Pattern`没有从`Object`覆盖`equals()`，我们需要依靠`String`比较。

运行它，看到它失败，因为`retrieve()`返回 null 而不是我们想要的正则表达式。不，实际上这个测试会导致一个空指针异常。

这算不算测试失败？可能不是，因为`NullPointerException`不是`AssertionError`的实例。

但是如果你对这类事情不是超级教条主义者，那么继续让测试通过(我们会看到很多其他测试干净利落地失败)。

```
 public V retrieve(N name) {
        return **this.create(name);**
    }
```

应该可以了。但是当然`LRUCache`此时没有缓存任何东西。如果以相同的名称调用`retrieve()`两次，将重新计算该值，而不是从缓存中检索。因此这个测试:

```
 @Test
    void testRetrieve() {
        LRUCacheImpl cache = new LRUCacheImpl(DEFAULT_SIZE);
        String romanName = "^(?=[MDCLXVI])M*(C[MD]|D?C{0,3})" 
                + "(X[CL]|L?X{0,3})(I[XV]|V?I{0,3})$";
        Pattern expected = cache.retrieve(romanName);
        Pattern actual = cache.retrieve(romanName);
        assertEquals(expected, actual);
    }
```

这次的模式是验证罗马数字(这一个不是来自 Harsha 的列表)。

由于模式应该在第二次从缓存中检索，我们可以在这个测试中使用引用相等。我开始怀疑我们是否真的需要`createCallCount`，或者`has()`函数。

测试彻底失败了，因为模式每次都要重新编译。

> org . opentest4j . assertionfailederror:应为:Java . util . regex .**pattern @ 66480 DD 7**<^(？=[MDCLXVI])M*(C[MD]|D？C{0，3})(X[CL]|L？X{0，3})(I[XV]|V？I{0，3})$ >却被:Java . util . regex .**pattern @ 52a 86356**<^(？=[MDCLXVI])M*(C[MD]|D？C{0，3})(X[CL]|L？X{0，3})(I[XV]|V？我{0，3})$ >
> 期待:^(？=[MDCLXVI])M*(C[MD]|D？C{0，3})(X[CL]|L？X{0，3})(I[XV]|V？I{0，3})$
> 实际:^(？=[MDCLXVI])M*(C[MD]|D？C{0，3})(X[CL]|L？X{0，3})(I[XV]|V？I{0，3})$

在您的机器上，散列码几乎肯定会不同，但总体结果是相同的:模式是重新创建的，而不是从缓存中检索的。

因为我们已经看到这个测试因为正确的原因而失败，所以我们可以确信它只会在应该通过的时候通过。

我们需要添加一些东西来保存这些值。参照`sun.misc.LRUCache`，我们看到 Sun 团队简单地使用了一个数组。我将使用两个数组，一个用于名称，一个用于值。

如果你只想用一个阵，像太阳队，参考`sun.misc.LRUCache`。只有 Mark Reinhold 被列为作者，但我相信这是一个团队的努力。

将这些字段添加到`collections.LRUCache`:

```
 private final Object[] names;
    private final Object[] values; private final int capacity; private final int lastIndex; private int nextUp = 0;
```

我想分别制作`N[]`和`V[]`类型的`names`和`values`。如果我这样做，IntelliJ IDEA 会给出未检查的强制转换警告，而 Apache NetBeans 不会。不知道为什么。所以我决定继续把它们都做成类型`Object[]`。

接下来，修改构造函数如下:

```
 public LRUCache(int size) {
        // TODO: Throw exception if size < MINIMUM_CAPACITY
        // TODO: Throw exception if size > MAXIMUM_CAPACITY
        **this.capacity = size;
        this.names = new Object[this.capacity];
        this.values = new Object[this.capacity];
        this.lastIndex = this.capacity - 1;**
    }
```

并将`retrieve()`修改成这样:

```
 public V retrieve(N name) {
        **V value;
        if (this.nextUp > 0 && name.equals(this.names[0])) {
            value = (V) this.values[0];
        } else {
            value = this.create(name);
            this.names[0] = name;
            this.values[0] = value;
            this.nextUp++;
        }
        return value;**
    }
```

看来我只是推迟了未检查的演员警告。我确实在寻找适当的方式来处理那个警告时被转移了方向。到目前为止，我还没有找到一种优雅的方式来摆脱这种困境。我想我们必须小心这里的类型系统。

至少这通过了测试。但是缓存只能保存一个值。所以我们写下一个测试:

```
 @Test
    void testCacheRetainsValueWhileCapacityAvailable() {
        LRUCacheImpl cache 
                = new LRUCacheImpl(LRUCache.MINIMUM_CAPACITY);
        String timeName = "^(?:\\d|[01]\\d|2[0-3]):[0-5]\\d$";
        Pattern expected = cache.retrieve(timeName);
        for (int i = 1; i < LRUCache.MINIMUM_CAPACITY; i++) {
            String fillerName = "^" + i + "*$";
            cache.retrieve(fillerName);
        }
        Pattern actual = cache.retrieve(timeName);
        assertEquals(expected, actual);
    }
```

第一个正则表达式用于 24 小时格式的时间(例如，07:25，16:08)。放入缓存中。那么剩余的高速缓存容量被不太有用的模式填满。时间模式应该还在缓存中。

但这并没有发生，因为我们的缓存只使用了数组的第一个片段。我们可以简单地通过重写`retrieve()`来一次一个槽地填充数组来通过这个测试。现在考虑将名称-值对移到数组的前面还为时过早。

```
 public V retrieve(N name) {
        **Object currName;**
        V value;
        **boolean notFound = true;
        int index = 0;
        while (notFound && index < this.nextUp) {
            currName = this.names[index];
            if (currName.equals(name)) {
                notFound = false;
            }
            index++;
        }
        if (!notFound) {
            value = (V) this.values[index - 1];
        } else {
            value = this.create(name);
            this.names[this.nextUp] = name;
            this.values[this.nextUp] = value;
            this.nextUp++;
            if (this.nextUp == this.capacity) {
                this.nextUp--;
            }
        }**
        return value;
    }
```

这通过了测试，但我真的不喜欢它。这里有几个重构的机会。首先，它有二十多行。这不足以触发 IntelliJ IDEA 或 NetBeans 的建议，但足以引起问题。

所以让我们将数组搜索过程提取到一个私有函数中。

```
 private static int indexOf(Object obj, Object[] array, 
                               int endBound) {
        boolean found = false;
        int curr = 0;
        while (!found && curr < endBound) {
            found = obj.equals(array[curr]);
            curr++;
        }
        if (found) {
            return curr - 1;
        } else {
            return -1;
        }
    }
```

也许你的第一反应是让它成为一个可以访问`names`的实例函数，而不是一个需要`names`作为`Object[]`传入的静态函数。但是当我想为`values`做一个类似的功能时，我遇到了问题。所以我在两种情况下都使用了静态函数。

完成后，我们可以像这样缩短`retrieve()`:

```
 public V retrieve(N name) {
        V value;
        **int index = indexOf(name, this.names, this.nextUp);**
        if (**index > -1**) {
            value = (V) this.values[**index**];
        } else {
            value = this.create(name);
            this.names[this.nextUp] = name;
            this.values[this.nextUp] = value;
            this.nextUp++;
            if (this.nextUp == this.capacity) {
                this.nextUp--;
            }
        }
        return value;
    }
```

通过了测试，它应该也能通过我们写的另外两个测试。而且还不到二十行。我们可以进一步缩短它，将 Else 子句提取到另一个私有函数，也许叫做`add()`。

然而，更迫切的需求是编写一个测试，要求`LRUCache`实际跟踪名称-值的新近性，以便通过测试。

当达到容量时，我们的`LRUCache`的当前版本只是在数组的最后一个槽中添加新的名称-值对，而不移动任何其他的。现在，我们最近最少使用的缓存更像是最近创建的缓存。

如果最近没有检索到某个值，而添加了其他值，则旧值最终会被遗忘，这取决于容量。

当编写测试时，我们不应该关心任何给定值在数组中的位置。我们应该只关心一个值是否在缓存中。这就是`has()`函数的用途。目前是存根。为了唤起你的记忆:

```
 // TODO: Write a test for this
    boolean has(V value) {
        return false;
    }
```

现在我们开始写测试，然后删除它，在那里做注释。

```
 @Test
    void testHas() {
        System.out.println("has");
        LRUCacheImpl cache = new LRUCacheImpl(DEFAULT_SIZE);
        String ssnName = "\\d{3}-\\d{2}-\\d{4}";
        Pattern pattern = cache.retrieve(ssnName);
        String msg = "After adding pattern " + pattern.toString()
                + " to cache, cache should still have it";
        assert cache.has(pattern) : msg;
    }
```

正则表达式用于验证作为`String`实例的社会保险号(SSN)在正确的位置有破折号，除此之外别无它用。

测试失败，即使我们知道`ssnName`已经被添加到缓存中(我们知道这一点是因为`testAddToCache()`应该还在通过)。但是`has()`无论如何都返回 false。通过这个测试很容易:

```
 boolean has(V value) {
        return **true**;
    }
```

测试通过，没有什么需要重构的。这是那种让 TDD 觉得很傻的事情。但重要的是我们接下来要写的测试:

```
 @Test
    void testDoesNotHave() {
        LRUCacheImpl cache = new LRUCacheImpl(DEFAULT_SIZE);
        String numberName = "([-+]?\\d*)";
        Pattern pattern = Pattern.compile(numberName);
        String msg = "New cache should not have "
                + pattern.toString() + " or any other value";
        assert !cache.has(pattern) : msg;
    }
```

当然这失败了，因为现在`has()`总是返回 true。为了通过这个测试，我们可以利用私有的`indexOf()`函数。

```
 boolean has(V value) {
        return **indexOf(value, this.values, this.capacity) > -1**;
    }
```

测试是我使用`has()`功能的主要动机。应该是纯功能，意思是没有副作用，不像`retrieve()`。那个应该有副作用，这才是重点。

看起来我们已经准备好投入工作，使这个东西成为 LRU 缓存。

```
 @Test
    void testValueEventuallyForgotten() {
        LRUCacheImpl cache = new LRUCacheImpl(DEFAULT_SIZE);
        String romanName 
                = "^(?=[MDCLXVI])M*(C[MD]|D?C{0,3})(X[CL]|L?X{0,3})"
                + "(I[XV]|V?I{0,3})$";
        Pattern romanValue = cache.retrieve(romanName);
        for (int i = 0; i < DEFAULT_SIZE; i++) {
            assert cache.has(romanValue);
            String fillerName = "^" + i + "*$";
            cache.retrieve(fillerName);
        }
        String msg = "After filling in " + DEFAULT_SIZE
                + " other values, Roman pattern should be out";
        assert !cache.has(romanValue) : msg;
    }
```

这个测试会失败，因为无论什么值首先被添加到缓存中，它都会在缓存的生命周期内一直保留在缓存中。这是因为一旦达到容量，新的值总是在最后添加。

为了通过这个测试，让我们改变`retrieve()`，以便一旦达到容量，`nextUp`被重置为零，而不是`lastIndex`——这接近函数的结尾:

```
 if (this.nextUp == this.capacity) {
                this.nextUp **= 0**;
            }
```

为了确保其他测试也能通过，我们需要用`this.capacity`替换`retrieve()`的`indexOf()`调用中的`this.nextUp`。

好了，我们差不多完成了，我们只需要放入使这个东西成为最近最少使用的缓存的功能。

我们的下一个测试与上一个测试非常相似，但是关键的区别在于，这一次我们将在添加到缓存的第一个项目的 For 循环中使用`retrieve()`而不是`has()`。

这意味着`LRUCache`应该在每次检索时将第一个添加到缓存中的条目放在队列的前面。

```
 @Test
    void testFrequentlyRetrievedStaysCached() {
        LRUCacheImpl cache = new LRUCacheImpl(DEFAULT_SIZE);
        String dollarAmountName = "\\$\\d*\\.\\d{2}";
        Pattern expected = cache.retrieve(dollarAmountName);
        for (int i = 0; i < DEFAULT_SIZE; i++) {
            Pattern actual = cache.retrieve(dollarAmountName);
            assertEquals(expected, actual);
            String fillerName = "^" + (i + 1) + "*$";
            cache.retrieve(fillerName);
        }
        String msg = "Cache should still have " 
                + expected.toString()
                + " because it was retrieved repeatedly";
        assert cache.has(expected) : msg;
    }
```

但是测试当然会失败，因为此时我们实际拥有的是最近最少添加的缓存，而不是最近最少使用的缓存。

我们缺少的关键部分是一个私有过程，它将数组中指定位置的项移动到位置 0。我们实现`LRUCache`的方式使得将数组移动助手作为一个独立的单元变得更加重要:我们将在`names`和`values`中都需要它。

这是数组移动助手的存根:

```
 // STUB
    private static void moveToFront(Object[] objects, 
                                    int position) {
        // TODO: Implement this
    }
```

一旦我们实现了这一点，我们只需要对`retrieve()`函数做一些关键的修改。

```
 public V retrieve(N name) {
        V value;
        int index = indexOf(name, this.names, this.capacity);
        if (index > -1) {
            value = (V) this.values[index];
        } else {
            value = this.create(name);
            this.names[this.nextUp] = name;
            this.values[this.nextUp] = value;
            **index = this.nextUp;**
            this.nextUp++;
            if (this.nextUp == this.capacity) {
                **this.nextUp--;**
            }
        }
        **if (index > 0) {
            moveToFront(this.names, index);
            moveToFront(this.values, index);
        }**
        return value;
    }
```

我委托你负责`moveToFront()`的实际执行。提示:在 IntelliJ IDEA 中，如果你写了一个所谓的“手动数组拷贝”，你会得到一个建议，用一个`System.arraycopy()`来替换它。

记住:对于引用类型的数组，只有对象指针在数组中移动。除非运行时有自己的理由移动它们，否则对象本身会留在它们所在的地方，这个理由与我们的程序所做的事情无关。

因此，性能成本可能与在一个数字原语数组中移动值差不多。

如果你在编写`moveToFront()`时遇到任何问题，你可以参考`sun.misc.LRUCache`的来源(谷歌“sun misc LRUCache”)。

希望现在所有的单元测试都通过了。我忘了提醒你重构。这里当然有重构的机会。如果你不知道什么需要重构，看看警告和待办事项注释。

首先，这里有一个没有使用`MAXIMUM_CAPACITY`的警告。它有相应的 Do 注释，提醒我们让构造函数检查`size`参数没有超过`MAXIMUM_CAPACITY`。

所以编写一个测试，尝试实例化一个比`MAXIMUM_CAPACITY`大一号的`LRUCache`。确保不要在`LRUCacheTest`中硬编码该值(例如 129)，而是将其计算为`LRUCache.MAXIMUM_CAPACITY + 1`。

通过测试应该要求`LRUCache`构造函数抛出一个带有非空异常消息的`IllegalArgumentException`。同样的还有`MINIMUM_CAPACITY`，如果你还没有处理好的话。

嗯，原来我们不需要`lastIndex`做任何事。继续删除它(包括它的构造函数初始化)。我还有一个从未使用过的单参数`indexOf()`函数，所以我也删除了它。

这应该只留下一个警告，关于`retrieve()`中未检查强制转换的讨厌警告。IntelliJ 在这一点上没有太大的帮助，我不想隐藏任何警告。这很容易成为另一篇文章的主题。

我计划撰写关于从头实现数据结构的其他文章。接下来我考虑数组支持的集合和数组支持的列表。欢迎在评论中提出建议。

其他可用于编程练习的固定容量缓存包括最近使用的(MRU)缓存和最少使用的(LFU)缓存。

那么为`LRUCache`和其他人写一个抽象超类也是一个有价值的练习。