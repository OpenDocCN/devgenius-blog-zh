# 科特林的测试工具。快速回顾 2022 年。

> 原文：<https://blog.devgenius.io/testing-tools-for-kotlin-quick-recap-in-2022-1c2b0d4e179a?source=collection_archive---------6----------------------->

![](img/0899f81dbdd8796199d713862dd71932.png)

我们可以将测试工具分为三大类:

1.  **框架**
2.  **嘲笑**
3.  **断言**

# 1)框架

## [6 月 4 日](https://github.com/junit-team/junit4/wiki/Getting-started)

刚刚好，足以工作，没有任何酷的功能。在 Android 项目中仍然是默认的，因为下一个版本(JUnit5)需要 Java8，由于编译时间的原因，Java 8 仍然不是 Android 的默认版本。

## [6 月 5 日](https://junit.org/junit5/docs/current/user-guide/)

这就是未来。可以与旧的 JUnit4 测试一起工作。这里提到的所有其他框架都需要这样才能工作。你可以用这个实现所有其他的行为。所有您知道的，所有新功能都在一个地方。
我个人不需要比这更多的东西。

主要特性:

**嵌套测试**——您可以构建漂亮的测试层次结构，并根据需要对它们进行分组。例如，您可以按照 BDD 风格将它们分组，如下所示。

```
*@Nested* inner **class** `GIVEN (...)`{
    *@Nested* inner **class** `WHEN (...)`{
        *@Test* **fun** `THEN (...)`() {
        *@Test* **fun** `THEN (...)`() {
```

**assertAll** —在抛出错误之前断言所有断言。您不必重新运行测试来检查其他断言是否有效！

```
assertAll( 
    { assertTrue(...) }, 
    { assertFalse(..) } 
)
```

现在你可以像人一样在测试结束时测试异常。

```
assertThrows<NullPointerException> { nullPointerFunc() }
```

**@ParameterizedTest** —不同场景不用复制&粘贴测试。在测试中感受数据类参数的力量！

```
@ParameterizedTest 
@ValueSource(strings = ["racecar", "radar", "able was I ere I saw elba"]) 
fun palindromes(candidate: String?) {
    assertTrue(StringUtils.isPalindrome(candidate)) 
}
```

**@ DisplayName**——我知道在 Kotlin 中你可以用句子作为名字，但是对于 display name 你可以用表情符号。是的，很可能你永远也不会用到它。

```
@Test 
@DisplayName("Test name with emoji: 😅 😂 🤣 ") 
fun testWithDisplayNames() {
```

**边注** s:
1。要在 Android 上使用以上所有的好处，你必须使用一个外部插件:[https://github.com/mannodermaus/android-junit5](https://github.com/mannodermaus/android-junit5)
2。JUnit5 不使用`@Rules`，你需要使用扩展，但它们非常简单:

```
@ExtendWith(
    MockitoExtension::class, //Mockito have extensions
    InstantExecutorExtension::class 
//Android have only rule, but rewrite it as extension is dead simply
)//Here you have extension
class InstantExecutorExtension : 
BeforeEachCallback, AfterEachCallback {
    override fun beforeEach(context: ExtensionContext?) {
        ArchTaskExecutor.getInstance()
            .setDelegate(
                object : TaskExecutor() {
                    override fun executeOnDiskIO(runnable: Runnable
                         = runnable.run()
                    override fun postToMainThread(runnable:Runnable)                         
                         = runnable.run()
                    override fun isMainThread(): Boolean = true
                }
            )
    } override fun afterEach(context: ExtensionContext?) {
        ArchTaskExecutor.getInstance().setDelegate(null)
    }
}
```

## [Spek](https://www.spekframework.org/)

核心概念是将测试写成嵌套的 lambdas。他们强制进行 BDD 风格的测试，这很好，但是你不需要他们来实现。

```
object MyTest: Spek({
    group("a group") {
        test("a test") {
            ...
        }

        group("a nested group") {
            test("another test") {
                ...
            }
        }
    }
})object SetFeature: Spek({
    Feature("Set") {
        val set by memoized { mutableSetOf<String>() }

        Scenario("adding items") {
            When("adding foo") {
                set.add("foo")
            }

            Then("it should have a size of 1") {
                assertEquals(1, set.size)
            }

            Then("it should contain foo") {
                assertTrue(set.contains("foo"))
            }
        }
```

## [Kotest](https://kotest.io/)

与上面的非常相似，但是 Kotest 给了你更多的可能性。

```
class MyTests : StringSpec({
    "strings.length should return size of string" {
        "hello".length shouldBe 5
    }
})class MyTests : BehaviorSpec({
    given("a broomstick") {
        `when`("I sit on it") {
            then("I should be able to fly") {
                // test code
            }
        }
        `when`("I throw it away") {
            then("it should come back") {
                // test code
            }
        }
    }
})
```

可能的测试样式列表:
* Fun Spec:ScalaTest
* Describe Spec:Javascript frameworks 和 RSpec
* Should Spec:A Kotest original
* String Spec:A Kotest original
* Behavior Spec:BDD frameworks
* Free Spec Spec:ScalaTest
* Word Spec:ScalaTest
* Feature Spec:cumber
* Expect Spec Spec:A Kotest original
* Annotation Spec:JUnit

# 2)模拟

## [莫奇托](https://site.mockito.org/)

最流行的嘲笑事物的工具。**用 Java** 编写，但仍然是一个非常好的工具，即使是在 Koltin 项目中。对于 Kotlin，你也有一个小助手库 Mockito-Kotlin，它允许你用`whenever`:
[https://github.com/nhaarman/mockito-kotlin](https://github.com/nhaarman/mockito-kotlin) 替换你的``when`,它有你需要的一切。差不多了。这只是 Java Libray 的一个覆盖，它不支持 Kotlin 多平台。前一段时间，我在使用`suspend`功能时也遇到了一些问题。如果你想和协程一起使用，请查看 GitHub 上的[问题。](https://github.com/mockito/mockito-kotlin/issues)

现在，当您在项目中使用协程时，如果您想使用 Kotlim 多平台，最好选择 MockK。

## [莫克](https://mockk.io/)

这也是一个非常好的嘲讽工具，但是纯粹是用 Kotlin 语言写的。与 Mockito 相比，MockK 有两个主要优势:

1.  100%支持`suspend`功能支持
2.  使用 Kotlin 多平台

由于以上原因，我选择它作为新 Kotlin 项目中的默认嘲讽工具。

```
val mock = mockk<ObjectToMock>()
every **{** mock.function() **}** returns value //simple mock
coEvery **{** mock.suspendFunction() **}** returns value //suspend mock
```

# 3)断言

可能的库列表:

[Kotest](https://kotest.io/docs/assertions/assertions.html)——是的，这个来自框架，也有一个独立的断言库。这是最受欢迎的。

```
name shouldBe "sam"
user.name shouldNotBe **null**
```

[Kluent](https://markusamshove.github.io/Kluent/)——和上面那个很像，第二受欢迎。

```
"hello" shouldBeEqualTo "hello"
"hello" shouldNotBeEqualTo "world"
```

[Strikt](https://strikt.io/)

```
expectThat(subject)
    .hasLength(35)
    .matches(Regex("[\\w\\s]+"))
    .startsWith("T")
```

[中庭](https://github.com/robstoll/atrium)

```
expect(x).toBe(9)
```

[火腿蛋糕](https://github.com/npryce/hamkrest)

```
assertThat("xyzzy", 
    startsWith("x") and endsWith("y") and !containsSubstring("a")
)
```

[Expekt](http://winterbe.github.io/expekt/)

```
23.should.equal(23)
“Kotlin”.should.not.contain(“Scala”)
listOf(1, 2, 3).should.have.size.above(1)
```

[资产者](https://www.kotlinresources.com/library/assertk/)

```
assertThat(person.name).isEqualTo(“Alice”)
```

在这里，我最大的问题是选择什么。就我个人而言，我喜欢 Kotest 的方法，它看起来很好，也很受欢迎。依我看，所有这些都是个人喜好的问题。

# 摘要

我希望我帮助您快速浏览了 Kotlin 中可能的测试工具。我使用 JUnit5 和带有 Kotest 的 MockK 作为断言库。
你在项目中使用什么或者你想尝试什么？

加入我的**代码端**！
万事如意，
阿图尔

*最初发表于 2020 年的*[](https://www.thecodeside.com/2020/10/13/testing-tools-for-kotlin-quick-recap-in-2020/)*，但这次没有太大变化。*