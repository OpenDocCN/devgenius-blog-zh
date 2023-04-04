# 围棋中的大整数:处理大数很容易

> 原文：<https://blog.devgenius.io/big-int-in-go-handling-large-numbers-is-easy-157cb272dd4f?source=collection_archive---------3----------------------->

![](img/05db112ddd6b7b967f086bdd6576be27.png)

有时，我们希望进行数学运算，这涉及到非常大的整数计算，超出了所有可用的原始数据类型的限制。例如，100 的阶乘包含 158 位数字，因此我们不能将其存储在任何可用的原始数据类型中。Golang 不会隐式检查溢出，因此当 int64 中存储的位数超过 64 位时，这可能会导致意外的结果。

为了解决这个问题，Go 提供了实现任意精度算术(大数)的包“big”。

# 描述

支持以下数值类型:

```
Int    signed integers
Rat    rational numbers
Float  floating-point numbers
```

Int、Rat 或 Float 的零值对应于 0。因此，新值可以以通常的方式声明，并表示为 0，而无需进一步初始化:

```
var x Int        // &x is an *Int of value 0
var r = &Rat{}   // r is a *Rat of value 0
y := new(Float)  // y is a *Float of value 0
```

## 工厂功能

或者，可以使用以下形式的工厂函数来分配和初始化新值:

```
func NewT(v V) *T
```

例如，NewInt(x)返回一个设置为 int64 参数 x 的值的*Int，NewRat(a，b)返回一个设置为分数 a/b 的*Rat，其中 a 和 b 是 int64 值，NewFloat(f)返回一个初始化为 float64 参数 f 的*Float。

```
var z1 Int
z1.SetUint64(123)                 // z1 := 123
z2 := new(Rat).SetFloat64(1.25)   // z2 := 5/4
z3 := new(Float).SetInt(z1)       // z3 := 123.0
```

# 用法:斐波那契数

现在我们来看一个例子

这个例子演示了如何使用 big.Int 计算具有 100 位小数的最小斐波那契数，并测试它是否是质数。这概述了大包装的使用。

# 与其他类型相互转换

我在使用大软件包时面临的另一个困难是类型转换。类型转换是我们在编写代码开发应用程序时经常要做的事情。

可供我们用作大包输入的数据通常是原始数据类型。最直接的方法是以字符串格式传输这些数据，通常作为某个 JSON 对象的属性。以下是一些将值与`big.Int`相互转换的例子。

## 从 int 转换

将一个`int`转换成`big.Int`很简单，但是必须通过`int64`，就像这样:

```
newBigInt := big.newInt(int64(someInt))
```

## 转换为整数

要将大整数转换为 uint64，请运行以下代码:

```
var smallnum, _ = new(big.Int).SetString("2188824200011112223", 10)
num := smallnum.Uint64()
```

请注意，Go 不执行边界检查——如果大整数的值不适合 uint64，您将不会得到任何警告，并且 **num** 将溢出并保存不正确的值。

要转换成带符号的 64 位整数，请使用`Int64()`而不是`Uint64()`。

## 从字符串转换

假设你持有一个大整数作为`string`，你用它创建一个`big.Int`，如下所示:

```
**var** bignum, ok = new(big.Int).SetString("218882428714186575617", 0)
```

如果输入到`setString()`的字符串以“0x”开始，将使用基数 16(十六进制)。如果字符串以“0”开头，将使用基数 8(八进制)。否则，它将使用基数 10(十进制)。您也可以手动指定基数(最多 62)。返回值的类型为`*big.Int`。如果 Go 未能创建 big.Int，则将 **ok** 设置为 *false* 。

## 转换为字符串

转换成字符串可以让你序列化为 JSON，打印到控制台，等等，所以它非常有用。

要将一个`big.Int`转换成十六进制符号的`string`，使用:

```
str1 := prime1.Text(16) // or: str1 := fmt.Sprintf("0x%x", bigInt)
```

要转换成十进制记数法，请使用:

```
str1 := prime1.Text(10) // fmt.Sprintf("%v", bigInt)
```

# 结论

我把这个留给你，让你积累经验。

就是这样！我希望这篇关于 Go 大整数的短文。有用！当然，欢迎各种评论。

# 参考

[](https://golang.org/pkg/math/big/) [## big-Go 编程语言

### Package big 实现任意精度的算术(大数)。支持以下数值类型:Int…

golang.org](https://golang.org/pkg/math/big/)