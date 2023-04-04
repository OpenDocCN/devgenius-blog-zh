# 失败没关系:预期 Go 中的测试失败

> 原文：<https://blog.devgenius.io/its-ok-to-fail-expecting-test-failures-in-go-5b359a04415c?source=collection_archive---------5----------------------->

*当感觉不对劲的时候*

最近，我发现自己处于一种奇怪的情况，需要 Golang 中的一个单元测试失败才能认为它成功。虽然这对于语言来说是非常罕见的情况，但它给了我们一个机会来更好地理解测试框架是如何运行的，以及我们如何可能实现这一点。像往常一样…系好安全带，开始阅读。

![](img/dd2779fd9b44b8ef8ccd7f3ecb88a537.png)

照片由 [Cindy Tang](https://unsplash.com/@tangcindy?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 在 [Unsplash](http://localhost:3000/collections/4492909/fail-faster?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 上拍摄

# 问题是

其他编程语言中的测试框架为开发人员提供了通知框架*测试失败是“没问题的”*的方法，这确实是测试被认为成功的预期行为。在 Golang 不是这样的。归根结底，这是由于该语言管理错误的方式，并期望开发人员来处理它们。为了更好地理解这个问题，下面是其他编程语言中的一些例子:

```
// Language: Java, Framework: JUnit 4,5import org.junit.Test
....@Test(**expected = NullPointerException.class**) 
public void TestMethodWithNull() {     
    String fullName = null;     
    NameExtractor testCandidate = new NameExtractor();
    String firstName = testCandidate.getFirstName(fullName)
}...// Language: Python 2,3, Framework: unittestimport unittestclass TestFailures(unittest.TestCase) def test_method_with_error_1(self):
   candidate = NameExtractor() 
   **self.assertRaises(SomeError, candidate.get_first_name, None)**:

 **@unittest.expectFailure** 
 def test_method_with_error_2(self):
   candidate = NameExtractor()
   candidate.get_last_name(None)
```

为了说明这个例子，我们使用了一个虚构的类`NameExtractor`,它的实现意味着从表示全名的给定字符串中提取名字和姓氏。在这两种语言中，该类的约定是在参数无效的情况下抛出异常或引发错误。

这些条款本质上是语法上的糖，两种语言中有等效的方法通过使用 catch 块来实现相同的目标，但上面是一种更优雅的处理方式，因为它不会污染您的测试。此外，如果您正在评估您的单元测试的覆盖率，而不仅仅是被测试的代码，那么使用 catch 块通常会影响这个度量。

`NameExtractor`的 Golang 等效实现使用显式错误管理，如下所示:

```
// Language: Gotype NameExtractor interface { GetFirstName(fullName string) (string, error)
  GetLastName(fullName string) (string, error)
}type SimpleExtractor struct {
  NameExtractor 
  ....
}func (e *SimpleExtractor) GetFirstName(f string) (string, error) {

   if **len(f) == 0** {
     return "", errors.New("Full name cannot be an empty string")
   }
   ....
   return firstName, nil
}func (e *SimpleExtractor) GetLastName(f string) (string, error) {

   if **len(f) == 0** {
     return "", errors.New("Full name cannot be an empty string")
   }
   ....
   return lastName, nil
}// Testing codeimport "testing"
import "github.com/stretchr/testify/assert"func TestGetFirstNameWithEmpty(t *testing.T) { extractor := &SimpleExtractor{ ... }
    **firstName, err := extractor.GetFirstName("")
    assert.NotNil(t, err, "Error not returned on empty string")**
    assert.Equal(t, 0, len(firstName), "First name should be empty")
}func TestGetLastNameWithEmpty(t *testing.T) { extractor := &SimpleExtractor{ ... }
    **lastName, err := extractor.GetLastName("")
    assert.NotNil(t, err, "Error not returned on empty string")**
    assert.Equal(t, 0, len(lastName), "Last name should be empty")
}
```

不管个人对异常作为一种语言特性有什么看法，我们总是可以将带有异常的代码翻译成具有显式错误管理的代码。因此，我们的函数将总是返回，并且通过良好的设计，我们将总是能够访问正常执行流程中的错误。因为错误管理在 Golang 中是显式的，所以没有必要将测试失败表示为积极的结果。…真的吗？

# 单元测试库的奇特案例

除了例外，似乎没有必要将测试失败建模为成功的测试。事实证明，异常管理——虽然是最突出的——只是我们可以预期测试失败的原因之一。

另一个原因发生在支持单元测试开发的库的开发中。在这里，我们可以开发在特定条件下触发单元测试失败的能力。这些库的一个例子是**stretchr/evident/require**。这公开了 twin 库*stretchr/evident/assert*的相同方法，但是它没有为测试记录失败，而是导致测试立即*失败*。按照 Golang 测试的说法，这意味着**要求**方法最终在`testing.T`实现上调用`FailNow()`而不是`Fail()`。根据库文档( [FailNow()](https://pkg.go.dev/testing#T.FailNow) 和 [Fail()](https://pkg.go.dev/testing#T.Fail) ):

*"FailNow 将该函数标记为失败，* ***通过调用 runtime 停止*** *该函数的执行。Goexit(然后运行当前 goroutine 中的所有延迟调用)。”*

*“失败”将功能标记为失败，但* ***继续*** *执行*

虽然对`Fail()`的调用仍然是可管理的，但是对`FailNow()`的调用会导致当前 go 例程的中断，从而产生类似于异常的行为和类似的结果。

因此，如果我们开发最终调用`FailNow()`的测试库(如果你的库使用 *require* ，这是肯定的)，我们如何测试正确的行为被实现了？这就需要将测试失败建模为成功的条件，或者类似的东西。

## FailNow()并且失败得更快！

在我们进入细节之前，让我们先花点时间解释一下为什么我们会在单元测试中使用像 *require* 这样的东西。如果你的测试只包含一个断言，那么使用*require*vs .*assert*不会产生任何功能上的差异。无论如何，断言可能在方法体的末尾。当您的测试使用*多个断言*时，这种差异就开始发挥作用，其中一个断言是下面的必要前提。让我们看看下面的单元测试:

```
import (
  "testing" "github.com/stretchr/testify/require"
  "github.com/stretchr/testify/assert"
)func TestWithMultipleAssertions(t *testing.T) { candidate := NewTestCandidate() entity := candidate.GetEntity("something")
  require.NotNil(t, entity, "Returned entity should not be nil")
  assert.Equal(t, "expected", entity.GetQuery(), "....")
} 
```

在上面的场景中，带有 require 的第一个断言也验证了带有 assert 的第二个断言的前提条件，并使对实体的`GetQuery()`调用安全返回。如果实体为`nil`，在最后一次断言之前，测试以失败而终止。

如果我们将上面清单中的 *require* 替换为 *assert* ，并且调用`GetEntity()`返回`nil`，那么结果将是一个紧急错误，而不是测试失败。虽然我们认为这是可以接受的，但这更可能被解释为单元测试逻辑中的错误，而不是失败的测试。运行这种测试的自动化工具就是这种情况。

# 实施失败管理:管理你的期望

预期会调用`T.FailNow()`的被测代码提出了一个有趣的问题，即与普通测试相反:检测对`T.FailNow()`的调用，并将测试标记为成功；如果没有调用`T.FailNow()`，则触发失败。

后者相当容易实现:我们可以简单地在预计会导致失败的方法后添加一个对`T.FailNow()`的显式调用，如果这是 invoke，我们就知道测试失败了。真正的挑战是成功测试的检测，因为`T.FailNow()`调用`runtime.Goexit()`，这终止了测试中的代码，让我们没有钩子。

可以帮助我们实现这个目标的是隔离和控制被测试代码的执行，这样我们就可以“看到”它的失败，并且将正在执行的测试标记为成功(这仅仅意味着测试方法成功退出)。只有在与代表主测试方法的 go 例程不同的*go 例程*中执行测试代码时，这才是可能的。

这正是 go testing 库执行测试并防止运行测试的主程序因调用`runtime.Goexit()`而意外终止的原因。如果你对细节感兴趣可以看看测试包的实现，特别是 [func (t *T) Run(name string，f(t *T)) bool](https://github.com/golang/go/blob/master/src/testing/testing.go#L1460) 和 [func tRunner(t *T，fn func (t *T))](https://github.com/golang/go/blob/master/src/testing/testing.go#L1306) 。

我们可以重用这个概念，并添加一些附加功能来改善开发人员的体验。

## 实现测试气泡

我们希望尽可能少地破坏现有的开发人员编写单元测试的经验，并最终实现如下内容:

```
func TestMethodWithFailNow(t *testing.T) { candidate := StructUnderTest{}

  **ExpectFailure**(t, func(**tt TestingT**) { // this is expected to invoke T.FailNow() eventually
     candidate.CallFailNow(tt, nil)
  }) // if we get here this means that ExpectFailure has
  // execute correctly and the test exits with success.
}
```

这里有三点需要注意:

*   期望调用的代码部分被隔离到一个函数中；
*   该函数作为参数传递给`ExpectFailure`函数，该函数作为控制保护来验证故障；和
*   这个函数有`TestingT`参数，顾名思义，它充当`testing.T`的代理，这样我们习惯的所有行为对我们都是可用的。

我们放什么在`ExpectFailure`里变魔术，`TestingT`从哪里来？

`TestingT`是一个包装`testing.T`结构的接口，提供的实现接口的结构需要能够注册`FailNow()`已被调用的事实，将该信息放在某个地方，然后调用真正的`T.FailNow()`来保存预期的行为。这是通过定义如下的`MockTestingT`结构来实现的:

```
import (
   "runtime"
   "testing" "github.com/stretchr/testify/require"
)type MockTestingT struct {
  FailNowCalled bool
  t *testing.T TestingT
}func (m *MockTestingT) FailNow() {
  // register the method is called
  **m.FailNowCalled = true**
  // exit, as normal behaviour
  **runtime.Goexit()**
}func (m *MockTestingT) Errorf(format string, args ...interface{}) {
  t.Errorf(format, args)
}
```

`ExpectFailure`需要隔离作为参数传递的函数的执行，以防止对`runtime.Goexit()`的调用过早终止执行，然后验证函数的执行是否有预期的结果。

```
import "sync"// include the listing abovefunc ExpectFailure(t TestingT, func f(tt TestingT)) { var wg sync.WaitGroup

  // create a mock structure for TestingT
  mockT := &MockTestingT{t: t} // setup the barrier
  **wg.Add(1)**
  // start a co-routine to execute the test function f
  // and release the barrier at its end
  **go func() {
     defer wg.Done()
     f(mockT)
  }**

  // wait for the barrier.
  **wg.Wait()** // verify fail now is invoked
  **require.True(t, mockT.FailNowCalled)**
}
```

上面的两个清单让我们实现了我们所需要的。`MockTestingT`嵌入[要求。测试](https://github.com/stretchr/testify/blob/master/require/requirements.go#L4)(因此提供`testing.T`提供的方法)。模拟对象拦截对`FailNow()`的调用，并在失败前跟踪它的调用。

`ExpectFailure`函数使用一个屏障来等待被测试函数的执行完成，然后验证它的终止是否是因为调用了`FailNow()`而发生的，从而给出了我们想要实现的反向行为。

# 包裹

虽然 Golang 中预期测试失败的情况非常罕见，但启用这一功能让我们有机会了解标准测试框架实现的某些方面以及依赖它的一些库，如`require`和`assert`。通过理解我们想要解决的问题的本质，以及 Golang 提供给我们的并发性抽象的最少知识，我们已经演示了如何实现一种模式，这种模式尽可能接近 Java 或 Python 等语言中用于测试异常的模式。

单元测试快乐！