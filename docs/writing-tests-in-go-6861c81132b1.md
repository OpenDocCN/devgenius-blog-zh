# 在 Go 中编写测试

> 原文：<https://blog.devgenius.io/writing-tests-in-go-6861c81132b1?source=collection_archive---------5----------------------->

在今天的帖子中，我们将探讨如何在 Go 中编写测试。测试驱动开发，也称为 TDD，是一种鼓励开发人员在开发过程中测试代码的范例。

# 什么是测试，我为什么要关心？

![](img/66097b408761c0984a4e5828673a0dac.png)

测试是一段用来验证您的代码行为是否正常的代码。起初，这可能听起来很乏味。

> *Meh。我完全了解我的代码。我知道我在做什么。考试真是浪费时间。*
> 
> *—你真正的遗言:’)*

我曾经认为编写测试也是浪费时间。我写了代码，所以我应该最清楚它是否有效，对吗？我总是可以打印调试并修复错误，对吗？当从其他地方调用我的函数时，它不可能会出错，对吗？…对吗？

编写测试之所以重要，有几个原因。当你在职业生涯或学校项目中取得进展时，有可能会和其他人一起工作。当你们一起做一个项目时，你可能要负责编写一段必须被其他人使用的代码。我们称之为*库*。当其他人依赖您的代码正常工作时，确保您处理边缘情况是至关重要的，否则他们需要等待，直到您修复它。这可能非常耗时，并且可以通过中途测试您的代码来避免。测试就像是你在独自编码时不知道自己需要的工具，但碰巧是团队项目中的救命稻草。

编写测试的第二个原因是，它通常会产生更干净的代码。当我们提到测试时，我们通常指的是*单元测试*，这意味着我们正在测试代码的每一个单独的组件。为了编写这些测试，您需要以模块化的方式编写代码。例如，我们可以把一个庞大臃肿的功能分解成能做好一件事的原子功能，而不是一个。然后，可以单独测试这些功能，以确保它们正常工作。

第三个原因更加主观。我认为 TDD 帮助我更好地将事情可视化。在 TDD 中，鼓励您在实际实现功能之前先编写测试。在我的情况下，通过首先编写测试，我更确定我的模块应该接受什么作为输入，应该输出什么。我也可以事先考虑边缘情况，这样我就不需要事后去修改我的实现。

# 在 Go 中编写测试

用 Go 编写测试非常容易。Go 在其标准库中包含了`testing`包，我认为这是该语言的开发者非常深思熟虑的决定。你真的不需要像其他语言那样的单独的测试框架或库，我很欣赏这一点。

Go 测试存储在`*_test.go`文件中。星号代表您的`.go`文件，其中包含您想要测试的函数。例如，假设我有一个`areas.go`文件，其中包含计算不同形状面积的函数。这些功能的测试将在`areas_test.go`中进行。

让我们从代码的高级概述开始。正如我前面提到的，`areas.go`将存储三个计算面积的函数:`findTriangleArea`、`findSquareArea`和`findCircleArea`。我们将在后面编写实现。我们应该首先编写输入和输出，否则当我们编写测试时，Go 会抱怨。

```
package areasfunc findTriangleArea(base, height float64) float64 {
    return 0.0
}func findSquareArea()func findCircleArea()
```

我们现在可以开始编写测试了。记住，我们希望在函数之前编写测试。创建一个名为`areas_test.go`的文件，并键入以下内容:

```
package areasimport (
    "testing"
)
```

我们从导入这些包开始。

```
func TestFindTriangleArea(t *testing.T) {
    base := 2.0
    height := 3.0
    expectedArea := 3.0
    actualArea := findTriangleArea(base, height) if actualArea != expectedArea {
        t.Fatalf("expected %v, got %v", expectedArea, actualArea)
    }
}
```

这是`findTriangleArea`的测试功能。所有 Go 测试函数的名称都采用了`TestXxx(t *testing.T)`的格式，其中`Xxx`是您想要测试的函数的名称。测试的第一个字符必须大写。`testing.T`是一个负责跟踪测试状态的变量。`T`可用于调用特定的方法，如`Errorf`或`Fatalf`来终止测试、返回错误、记录结果等等。

在函数内部，我们看到了测试的主要逻辑。简单地说，测试检查期望值和实际值是否匹配。在我们的例子中，我们调用底部为 2.0、高度为 3.0 的`findTriangleArea`。我们期望看到 3.0 的输出，因为`(base * height) / 2`找到了一个三角形的面积。如果对`findTriangleArea`的调用返回一个非零错误，或者如果返回的区域与预期的区域不匹配，我们将输出一个错误并停止测试。

`t.Fatalf`就像`fmt.Sprintf`一样，我们可以根据自己的喜好格式化输出。我喜欢输出期望值，我们得到的实际值，以及任何错误，如果它存在的话。

让我们进行测试。为了在 Go 中运行一个测试，我们进入终端，并且`cd`进入测试文件所在的目录。然后，我们运行`go test -v`。您将得到如下输出:

```
$ go test -v
=== RUN   TestFindTriangleArea
    areas_test.go:15: expected 3, got 0
--- FAIL: TestFindTriangleArea (0.00s)
FAIL
exit status 1
FAIL    example.com/areas     0.002s
```

现在我们可以写出`findTriangleArea`的实现了。

```
func findTriangleArea(base, height float64) float64 {
    area := base * height / 2
    return area
}
```

简单吧？我也会为其他函数写一个。这是我们的最终结果。

```
// areas.go
package areasimport "math"func findTriangleArea(base, height float64) float64 {
    area := base * height / 2
    return area, nil
}func findSquareArea(side float64) float64 {
    area := side * side
    return area
}func findCircleArea(radius float64) float64 {
    area := math.Pi * radius * radius // round to the 3rd decimal place
    roundedArea := math.Round(area*1000) / 1000
    return roundedArea
}package areasimport (
    "testing"
)func TestFindTriangleArea(t *testing.T) {
    base := 2.0
    height := 3.0
    expectedArea := 3.0
    actualArea := findTriangleArea(base, height) if actualArea != expectedArea {
        t.Fatalf("expected %v, got %v", expectedArea, actualArea)
    }
}func TestFindSquareArea(t *testing.T) {
    side := 2.0
    expectedArea := 4.0
    actualArea := findSquareArea(side) if actualArea != expectedArea {
        t.Fatalf("expected %v, got %v", expectedArea, actualArea)
    }
}func TestFindCircleArea(t *testing.T) {
    radius := 2.0
    expectedArea := 12.566
    actualArea := findTriangleArea(radius) if actualArea != expectedArea {
        t.Fatalf("expected %v, got %v", expectedArea, actualArea)
    }
}
```

如果我们现在运行`go test -v`，我们将得到以下结果:

```
$ go test -v
=== RUN   TestFindTriangleArea
--- PASS: TestFindTriangleArea (0.00s)
=== RUN   TestFindSquareArea
--- PASS: TestFindSquareArea (0.00s)
=== RUN   TestFindCircleArea
--- PASS: TestFindCircleArea (0.00s)
PASS
ok    example.com/areas     0.001s
```

厉害！我们所有的测试都通过了。

# 多个案例的测试

这都是阳光和彩虹，但如果我们要测试多个案例呢？您可能希望对多个案例进行测试。你是对的。

凭直觉，我们可以这样写:

```
func TestFindSquareArea(t *testing.T) {
    side := 2.0
    expectedArea := 4.0
    actualArea := findSquareArea(side) if actualArea != expectedArea {
        t.Fatalf("expected %v, got %v", expectedArea, actualArea)
    } side = 3.0
    expectedArea = 4.0
    actualArea := findSquareArea(side) if actualArea != expectedArea {
        t.Fatalf("expected %v, got %v", expectedArea, actualArea)
    } // repeat...
}
```

因为这段代码会很快变得难看，所以让我们试试别的。

```
func TestFindSquareArea(t *testing.T) {
    type findSquareAreaTest struct {
        side float64
        expected float64
    } findSquareAreaTests := []findSquareAreaTest{
        {2.0, 4.0},
        {3.0, 9.0},
        {4.0, 16.0},
        {0.0, 0.0},
    } for _, test := range findSquareAreaTests {
        output := findSquareArea(test.side)
        if output != test.expected {
            t.Fatalf("expected %v, got %v", test.expected, output)
        }
    }
}
```

这个看起来好多了。我们声明了一个保存`side`值和`expected`值的结构`findSquareAreaTest`。然后，我们创建该结构的一个片段，它保存了我们所有的测试用例。我们只需要使用一个 for 循环来遍历每个测试用例。这种类型的方法被称为*表驱动测试*，因为我们使用了一个包含许多测试用例的表。

# 结论

感谢你阅读这篇文章。希望你觉得有用。当然还有比我在这里列出的更高级的测试策略。我强烈建议您查看 `[testing](https://pkg.go.dev/testing)` [包](https://pkg.go.dev/testing)的[文档，以便深入了解。然而，这篇文章将带你开始测试驱动开发。尝试在你自己的个人项目中应用它！下次见，有更多有趣的话题。再见！](https://pkg.go.dev/testing)

你也可以在 [Dev.to](https://dev.to/jpoly1219/writing-tests-in-go-fd8) 和 [my personal site](https://jpoly1219.github.io) 上阅读这篇文章。