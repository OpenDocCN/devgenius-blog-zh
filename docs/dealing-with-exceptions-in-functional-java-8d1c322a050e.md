# 在函数式 Java 中处理异常

> 原文：<https://blog.devgenius.io/dealing-with-exceptions-in-functional-java-8d1c322a050e?source=collection_archive---------0----------------------->

## 像专家一样抛出异常

![](img/c5885e6377012c0a5650792696695a6b.png)

图片来自 [Pixabay](https://pixabay.com/?utm_source=link-attribution&utm_medium=referral&utm_campaign=image&utm_content=1615665) 的[基思·约翰斯顿](https://pixabay.com/users/keithjj-2328014/?utm_source=link-attribution&utm_medium=referral&utm_campaign=image&utm_content=1615665)

异常曾经非常方便。当您的应用程序通过功能分解被分解时，将异常冒泡到可以合理处理它们的某个级别是很重要的。功能分解可能导致非常深的嵌套，在每一层处理问题是一个主要问题和错误的来源。

在函数式编程中，函数式分解经常被基于动作的分解所取代。你有特定的动作模式，比如链接在一起的`map`和`filter`，你的函数提供了每个动作的细节。也许你的函数是进一步的动作链，特别是当你的数据本质上是分层的时候，但是不再有基于“直觉”的任意分割函数，并且让你的函数嵌套十层或更多层。

但是异常处理在像 streams 这样的模式中是不被处理的，所以你的 lambdas 经常不得不处理异常，这使得它们比它们需要的要难看得多。在我的文章[中，所有的循环都是代码味道](https://medium.com/swlh/all-loops-are-a-code-smell-6416ac4865d6)，我抱怨不得不像这样处理 lambda 函数中的异常:

```
InputStream nis =
    LoopElimination.class.getResourceAsStream("/testfile.txt");
// Exception handling makes functional programming ugly
Stream.generate(() -> {
   try {
      return nis.read();
   } catch (IOException ex) {
      throw new RuntimeException("Error reading from file", ex);
   }
})
 .takeWhile(ch -> ch != -1)
 .forEach(ch -> System.out.print((char)(int)ch));
```

如果我能写，理解起来会容易得多:

```
Stream.generate(() -> nis.read())
  .takeWhile(ch -> ch != -1)
  .forEach(ch -> System.out.print((char)(int)ch));
```

基于动作的链接库通常采用由标准功能接口集组成的 lambdas，`Function`，`BiFunction`，`UnaryOperator`，`Supplier`，`Consumer`等。这些都不允许抛出检查过的异常，而且理由充分。如果它们被抛出，系统会如何处理？检查异常通常是我们可以通过用默认值替换或者记录日志而不返回任何内容来处理的异常。无论哪种方式，流或函数链都应该尝试继续。

我想看看能不能想出一些代码，可以通用地包装这些函数，以便它们可以返回类型或异常。Scala 程序员可能已经在上蹿下跳地说“为什么不直接用 Scala 的`Either`类型呢？”Scala 程序员会如此兴奋。

我从来不喜欢`Either`这个名字，因为它没有足够强烈地暗示它的正常用法。它的正常用法是一个可以有错误或类型的容器，以便函数可以指示其返回类型中是否发生了错误。因为 Java 没有这样的东西，如果我们想类似地处理异常，我们将需要这样的东西。我将把它命名为`Possibly`,以表明它可能有类型或异常。下面是一个简单的实现:

```
public class Possibly<T> {
    final T value;
    final Exception exception;
    private Possibly(T value) {
        if(value == null) 
          throw new RuntimeException(
              "value of Possibly cannot be null");
        this.value = value;
        this.exception = null;
    }
    private Possibly(Exception exception) {
        if(exception == null) 
          throw new RuntimeException(
              "exception of Possibly cannot be null");
        this.value = null;
        this.exception = exception;
    }
    public static <T> Possibly<T> of(T value ) {
        return new Possibly(value);
    }
    public static <T> Possibly<T> of(Exception exception ) {
        return new Possibly(exception);
    }
    public boolean is() {
        return value != null;
    }
    public Possibly<T> doIfException(Consumer<Exception> action) {
        if(exception != null) {
            action.accept(exception);
        }
        return this;
    }
    public Optional<T> getValue() {
        return Optional.ofNullable(value);
    }
    public Optional<Exception> getException() {
        return Optional.ofNullable(exception);
    }
}
```

这个类型有静态的`of`函数，它基于参数的类型构造一个新的`Possibly`类型。它有一个谓词函数`is`，如果值存在(即没有异常)，它将返回。它有一个副作用函数`doIfException`，可用于记录异常，但可以进一步链接。它还有`getValue`和`getException`，它们返回所请求类型的`Optional`值。我可以给这个类添加更多的东西，但是我会让它变得简单，只使用我们将要使用的方法。

现在我们有了`Possibly`类型，我们可以创建一个`PossiblyFunction`来包装一个可以抛出异常的函数。

```
public class PossiblyFunction<V, R> implements 
                         Function<V, Possibly<R>> {
    private final ExceptionFunction<V, R> f;
    private PossiblyFunction(final ExceptionFunction<V, R> f) {
        this.f = f;
    }
    static <V, R> PossiblyFunction<V, R> of(
            final ExceptionFunction<V, R> f) {
        return new PossiblyFunction<>(f);
    }
    [@Override](http://twitter.com/Override)
    public Possibly<R> apply(V value) {
        try {
            return Possibly.of(f.apply(value));
        } catch (Exception e) {
            return Possibly.of(e);
        }
    }
}
interface ExceptionFunction<V, R> {
    R apply(V value) throws Exception;
}
```

这是一个扩展了`Function`接口的函数类，但是它返回的是`Possibly<T>`类型，而不是普通的`T`类型。同样，我使用静态的`of`函数返回一个新的`PossiblyFunction`类型用于构造，并且我覆盖了`apply`函数，以便它捕捉一个可能的`Exception`并创建适当的`Possibly`类型。为了创建其中一个，我们需要一个看起来很像`Function<V, R>`接口的`ExceptionFunction<V, R>`接口。但是它不能只是扩展它，因为`apply`函数必须能够抛出一个`Exception`，这会破坏`Function.apply`方法的签名。

现在我们有了它，我们可以为我们将使用的所有其他功能接口复制它。只有少数几个被流库使用，所以这不是一个艰巨的任务。但是现在，我只列出了`PossiblyFunction`,因为这是我目前要使用的全部。

为了进行试验，我将解决一个我经常遇到的问题，用 Jackson 库解析 JSON。通常情况下，`ObjectMapper.readTree`可能会抛出异常，但是通过使用上面的包装方法，我们可以避免在操作符链中使用 try/catch:

```
public class Main {
  public static ObjectMapper objectMapper = new ObjectMapper(); public static void main(String [] argv) {
    System.out.println("/good.json = "
      + parseFileAndPrettyPrint("/good.json"));
    System.out.println("/bad.json = " 
      + parseFileAndPrettyPrint("/bad.json"));
  }

  public static String parseFileAndPrettyPrint(String streamName) {
    var r = new BufferedReader(
            new InputStreamReader(
                    Main.class.getResourceAsStream(streamName)));
    return Optional.of(r.lines()
            .collect(StringBuilder::new, 
                    StringBuilder::append,
                    StringBuilder::append))
            .map(StringBuilder::toString)
            .map(PossiblyFunction.of(s -> objectMapper.readTree(s)))
            .orElse(Possibly.of(new Exception("no value exists")))
            .doIfException(e -> e.printStackTrace(System.out))
            .getValue()
            .map(n -> n.toPrettyString())
            .orElse("unparsable");

    }
}
```

`parseFileAndPrettyPrint`函数将打开类路径中的一个文件，读取所有行并将它们收集到一个字符串中，然后使用`PossiblyFunction.of(s -> objectMapper.readTree(s))`函数将流映射到`Possibly<JsonNode>`。因为我们的`Optional.of`返回一个`Optional<String>`，我们必须提供一个默认值，这个值就是`PossiblyFunction.of(new Exception(“no value exists”))`。有了这个实现，那里应该不可能有空的，所以实际上不应该调用它。但是我们把它放在那里是为了让编译器高兴。

`doIfException`方法会导致记录堆栈跟踪以进行诊断的副作用。我们使用`getValue`得到一个`Optional<JsonNode>`类型，并把它打印回一个字符串。如果解析 JSON 时出现异常，`getValue`将返回`Optional.empty`，我们可以使用`orElse`返回它是不可解析的。堆栈跟踪应该已经在前面打印出来，指出它不可解析的原因。运行它会导致:

```
/good.json = {
  "name" : "good json",
  "value" : "this is some good json"
}
com.fasterxml.jackson.core.JsonParseException: 
    Unexpected character ('}' (code 125)): 
    was expecting a colon to separate field name and value at 
    [Source: (String)"{    "name": "good json",    "value"}";
    line: 1, column: 38]
   at ...Stack trace goes here
/bad.json = unparsable
```

因此，我们获取诊断日志，并获得默认值。在一个真实的程序中，如果 JSON 不可解析，我们可能想要采取不同的行动，我们可以使用谓词函数`Possibly.is()`来决定做什么。

那么，我们如何解决文章开头出现的第一个问题，即一个字符一个字符地读取字符流？抛出异常的 lambda 属于类型`Supplier`，它不接受任何参数，但返回一个`int`值(它使用一个`int`值来区分-1 和任何合法字符)。所以我们需要制造一个`PossiblySupplier`型号，就像我们制造`PossiblyFunction`型号一样。我会继续做那件事。你可以在我的 GitHub 页面上找到代码。但是现在从顶部开始的代码看起来像这样:

```
var nis = Main.class.getResourceAsStream("/good.json");
Stream.generate(PossiblySupplier.of(() -> nis.read()))
      .takeWhile(ch -> ch.getValue().orElse(-1) != -1)
      .filter(ch -> ch.is())
      .map(ch -> (char)(int)ch.getValue().get())
      .forEach(ch -> System.out.print(ch));
```

干净多了！

这是混合旧 java 和新 java 的一个常见问题，我确信如果我足够努力地寻找，我可以找到这样做的库。但这似乎是一个很好的编码练习，也是一个很好的方式，可以让你更熟悉像 Streams 和 Project Reactor 这样的链式库所使用的函数接口。我希望你在研究这个编码问题时得到了乐趣，并受到启发去写你自己的，或者寻找一个已经写好的(我的还没有完成，可能永远也不会完成，但是如果你想完成它，请随时发送一个 PR)

我的 GitHub 上的代码:

[](https://github.com/rkamradt/FunctionalExceptions/tree/v0.1) [## rkamradt/功能异常

### 此时您不能执行该操作。您已使用另一个标签页或窗口登录。您已在另一个选项卡中注销，或者…

github.com](https://github.com/rkamradt/FunctionalExceptions/tree/v0.1) 

提到的其他文章:

[](https://medium.com/swlh/all-loops-are-a-code-smell-6416ac4865d6) [## 所有的循环都是代码味道

### for、while 及其同类之死。

medium.com](https://medium.com/swlh/all-loops-are-a-code-smell-6416ac4865d6)