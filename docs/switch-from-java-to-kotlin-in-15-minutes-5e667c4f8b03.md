# 10 分钟内从 Java 切换到 Kotlin

> 原文：<https://blog.devgenius.io/switch-from-java-to-kotlin-in-15-minutes-5e667c4f8b03?source=collection_archive---------6----------------------->

# 简介:

当我在 Kotlin 进行第一次技术面试时，我对 Kotlin 一窍不通，但对 Java 已经足够好了。我在最终失败的面试前 15 分钟才意识到这个问题。我真的希望有一篇教程文章来帮助我在 15 分钟内快速学会从 Java 到 Kotlin 的上下文切换。

现在我已经在 Kotlin 工作了 3 年多。这篇文章总结了我在过去 3 年里学到的最关键的区别，以帮助你从 Java 转换到 Kotlin:

1.  句法
2.  类和接口
3.  希腊字母的第 11 个
4.  零安全
5.  范围功能:`let`和`apply`
6.  扩展ˌ扩张

# **1。语法:**

1.  类型总是跟在变量名后面

与 Java 不同，变量类型总是跟在变量名后面，用冒号`:`互相隔开:

## Java 语言(一种计算机语言，尤用于创建网站)

```
String text = "Hello world";
```

## **科特林**

```
var text: String = "Hello world"
```

没有类型`String`它也能工作，因为`"Hello world"`被称为`String`

```
var text = "Hello world"
```

请注意，在该行的末尾不再有`;`

对于函数声明，变量类型也在变量名之后，返回类型也在函数的末尾。另外，Kotlin 中的函数自带关键字`fun`。

## Java 语言(一种计算机语言，尤用于创建网站)

```
String displayAge(String name, int age) {
  return name + " is " + age " years old.";
}
```

## 科特林

```
fun displayAge(name : String, age : Int) : String {
  return name + " is " + age " years old."
}
```

*   `val`关键字将使你的变量不可变，类似于 Java 中的`final`
*   `var`关键字使变量可变，可以在初始化后重新赋值

```
var name = "Coding WeiShengSu"
name = "WeiShengSu" // can reassign value after initialization
val text = "this text cannot be updated after initialization"
```

# 2.类别和接口:

在 Java 中，我们通常会将参数传递给构造函数并保存为最终属性，或者根据给定的参数设置属性，例如:

```
class User {
    private final String firstName;
    private final String lastName;
    public final String fullName;
    public bool isAdmin; public User(String firstName, String lastName, bool isAdmin) {
        this.firstName = firstName;
        this.lastName = lastName;
        fullName = firstName + " " + lastName;
    }
}
```

Kotlin 只是通过`val`或`var`将属性保存在构造函数中。默认值也可以在构造函数中赋值。

```
class User(
  private val firstName:String, 
  private val lastName: String,
  val isAdmin: Bool = false
) {
  val fullName: String = firstName + " " + lastName
}
```

类声明非常类似于 Java。代替关键字`implements`和`extends`

```
class Administrator extends User {
  public Administrator(String firstName, String lastName) {
    super(firstName, lastName, true);
  }
}
```

Kotlin 只是简单地使用`:`来表示实现一个接口或扩展一个类:

```
class Administrator(
    firstName: String,
    lastName: String
) : User("Admin", "User", true)
```

为了实例化一个类，Java 使用了关键字`new`,这个关键字在 Kotlin 中被删除了

```
val user = User("Coding", "weishengsu", false)
```

为了实例化一个实现接口的对象，Kotlin 使用`object`和`:`

```
interface Callback {
  fun onSuccess(results : String)
}val callback = object : Callback {
  override fun onSuccess(results : String) {
    // handle results
  }
}
```

# 3.λ:

让我们举一个关于网络抓取的例子。通常网络请求函数接受上面定义的`Callback`，并发出网络请求。一旦结果回来，就会触发`Callback`。

**获取网络功能**

```
// Java
void fetchFromNetwork(Callback callback) {
  // make a network request
  callback.onSuccess(results);
}
```

如果我们没有定义接口`Callback`,只是想要一个一次性的函数作为参数，我们也可以在这里用

```
callback: (results: String) -> Unit)
```

这意味着函数接受类型为`String`的参数结果并返回`void`，在 Kotlin 中是`Unit`:

```
// Kotlin
fun fetchFromNetwork(callback: Callback) {
  // make a network request
  callback.onSuccess(results)
}// or fun fetchFromNetwork(callback: (results: String) -> Unit)) {
  // make a network request
  callback.onSuccess(results)
}
```

客户端用一个新的`Callback`对象调用函数`fetchFromNetwork`:

```
// Java:
fetchFromNetwork(new Callback() {
    @Override
    public void onSuccess(String results) {
        // handle the results, maybe update the UI with the data
        updateUi(results);
    }
});
```

1.  Kotlin 中的等效方法调用非常相似，但是将实例化调用`new Callback() { ... }`替换为`object : Callback { ... }`

```
// Kotlin
fetchFromNetwork(object : Callback {
    @Override
    fun onSuccess(results : String) {
        // handle the results, maybe update the UI with the data
        updateUi(results)
    }
})
```

2.Lambda 替换回调接口

因为回调接口中只有一个函数，所以我们可以将`object : Callback { ... }`及其覆盖方法`onSuccess`简化为一个 lamba，只用于`onSuccess`的实现:

`{ arguments -> body of the function }`

```
// Kotlin
fetchFromNetwork({ results : String ->
     // handle the results, maybe update the UI with the data
     updateUi(results)
})
```

3.`it`是默认参数

如果不指定参数名`results`，参数默认为`it`

```
// Kotlin
fetchFromNetwork({ 
     // handle the results, maybe update the UI with the data
     updateUi(it)
})
```

4.删除括号

更简单的是，lambda 可以移出括号:

```
fetchFromNetwork {
     // handle the results, maybe update the UI with the data
     updateUi(it)
}
```

这种模式在科特林很常见。举几个例子:

**forEachIndexed**

参数`{ index : Int, name : String -> iteration body }`是一个函数`action`，它接受索引和类型`T`并返回`Unit`:

```
val names = listOf("Coding", "WeiShengSu", "John", "Susan")names.forEachIndexed { index, name ->
  println(name + " is at position " + index)
}
```

**过滤器**

参数`{ name: String -> iteration body returns Boolean }`

是一个接受类型`T`并返回`Boolean`的谓词，指示对象是否需要过滤。`filter`函数将返回列表中匹配谓词条件的所有对象:

```
fun <T> Iterable<T>.filter(predicate: (T) -> Boolean): List<T>
```

下面是调用`filter`函数的例子。

```
val names = listOf("Coding", "WeiShengSu", "John", "", "Susan")val notEmptyNames = names.filter { name ->
  return@filter name.isNotEmpty()
}
```

因为 lambda 在另一个函数中被调用，我们需要`@filter`标签来指定 lambda 的返回，而不是其他函数的返回。

**最后一行是 lambda** 中的返回值

因为默认情况下 lambda 的最后一行是返回值，我们可以简单地删除`return@filter`来使之超级干净

```
val notEmptyNames = names.filter { name ->
  name.isNotEmpty()
}
```

## 4.空、可空、非空和空安全

![](img/b371cab3eccfb550fe12ba078ec961cf.png)

在 Java 中，有这么多的`object != null`检查、`@Nullable`、`@Nonnull`来防止 lint、编译器或者单元测试的空指针异常。

关键字`?`能够处理各种空的情况

**可空**

任何带有`?`的变量类型都意味着可以为空，例如:`String?` `Int?` `User?`

```
var user: User? = null // variable user can be null
```

对于函数，参数或返回类型也可以为空

```
fun getUserName(user: User?) : String?
```

对于`< NullableType? >`中的类型，它也可以为空

*   `List<String?>`列表不可为空，但是列表的元素可以为空
*   `List<String>?`列表可以为空，但列表的元素不可为空

**空检查**

为了访问可空变量的属性，我们使用安全的调用操作符`?.`，这在调用链中非常有用

```
// Java
final Result result = fetchForResults();if (result != null &&
    result.status != null &&
    result.status.code != null ) {
    println(result.status.code);
}
```

我们可以用`?.`链接所有可空的属性

```
if (result?.status?.code? != null) {
  println(result?.status?.code)
}
```

**猫王操作员**

如果变量为空，那么我们给变量赋予默认值，否则使用变量值:

```
// Javaint len;
if (list != null) {
  len = list.size();
} else {
  len = 0;
}
```

`?:`如果为空，帮助您指定默认值:

```
// Kotlinval len = list?.size() ?: 0
```

`?:`对于函数中的提前返回或者为空时抛出异常非常有用

```
fun foo(user: User?): String? {
    val parent = user?.getParent() ?: return null
    val name = user?.getName() ?: throw IllegalArgumentException("name expected")
    // handle non-null parent and name
}
```

**安全施放**

`as?`帮你投给给定的职业。如果对象不是给定类的实例，它就返回 null

```
fun getUserName(obj: Any): String? {
  val user = obj as? User
  if (user == null) {
    return null  // handle when the object is not instance of User
  } else {
    return user.name
  }
}
```

或者通过`?.`简化

```
fun getUserName(obj: Any): String? {
  return (obj as? User)?.name
}
```

# 5.范围函数 let 和`apply`

我发现大多数情况下可以通过`let`和`apply`来实现。如果您对其他范围功能感兴趣，如`run`、`with`和`also`，您可以在这里找到更多详细信息

**让**

`let{ variableName -> }`对非空对象执行 lambda 并返回最后一行，以上例为结果状态:

```
// Java
final Result result = fetchForResults();if (result != null &&
    result.status != null &&
    result.status.code != null ) {
    println(result.status.code);
}
```

我们实际上可以用 lambda `let`来链接:

```
result?.status?.code?.let { code -> 
  println(code)
}
```

不指定变量名`code`:

```
result?.status?.code?.let { println(it) }
```

**应用**

`apply`也执行 lambda，但是使用`this`来访问对象。它还返回了对象`this`。`apply`当我们试图在同一个对象上设置一堆属性或方法时，这非常有用:

```
// Java
User user = new User();
user.age = 20;
user.name = "WeiShengSu"
user.grantPermission();
```

其中在同一个对象上设置了几个属性`user`:

```
val user = User().apply {
  this.age = 20 
  this.name = "WeiShengSu"
  this.grantPermission()
}
```

或者我们甚至可以简化没有`this`

```
val user = User().apply {
  age = 20 
  name = "WeiShengSu"
  grantPermission()
}
```

# 6.扩展:

Kotlin 能够通过称为*扩展的特殊修饰来扩展类/接口的功能。*要声明一个扩展函数，在它的类名或接口名前面加上`.`和方法名:

```
// Java
// This method will return the last Element of given list
// return null if the list is emptypublic static String getLastElementOrNull(List<String> texts) {
  if (texts.size() == 0) {
    return null;
  }
  return texts.get(texts.size() - 1);
}// To call the function:
String lastElement = getLastElementOrNull(texts);
```

在扩展函数中，我们可以通过可选关键字`this`直接访问公共属性或方法:

```
fun List<String>.getLastElementOrNull() : String? {
  if (this.size() == 0) {
    return null
  }
  return get(this.size() - 1)
}
```

一旦我们声明了扩展函数，所有的`List<String>`都有我们刚刚在上面声明的这个函数:

```
val texts: List<String> = listOf("WeiShengSu")// To cal the function:
val lastElement = texts.getLastElementOrNull()
```

更酷的是，类类型可以为空，例如:

```
// Java, if it is a null number, just return string "null"
public static String convertToString(@Nullable Integer number) {
  if (number == null) return "null";
  return number.toString();
}
```

在这种情况下，用`Int?`声明扩展函数:

```
fun Int?.convertToString() {
  if (this == null) return "null"
  return this.toString()
}
```

或者更简单:

```
fun Int?.convertToString() {
  return this?.toString() ?: "null"
}
```

在这种情况下，我们甚至可以在一个可空的 Int ( `Int?`)上调用这个扩展，而不需要`?`检查:

```
val number : Int? = getNumberSomewhere() // maybe nullable number
val textNumber : String = number.convertToString()
```

# 7.结论

希望这些主题能帮助您快速从 Java 切换到 Kotlin。

还有几个话题和 Java 不一样。但是如果你读到这里，你应该能够理解，阅读和用 Kotlin 写一些代码。其余的差异应该很简单，或者可以通过专门的谷歌搜索快速发现。

请跟随，鼓掌并在下面留下你的评论