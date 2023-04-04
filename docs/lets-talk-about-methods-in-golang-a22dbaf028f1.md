# [Golang]我们来谈谈 Golang 中的方法

> 原文：<https://blog.devgenius.io/lets-talk-about-methods-in-golang-a22dbaf028f1?source=collection_archive---------6----------------------->

让我们来谈谈 **Golang** 中的`***methods***`，我们为什么要使用它们，它们在 **Golang 中的意义是什么。**

![](img/fc93d7efd48e3b599b4e4a3da2ec14c4.png)

Go 不是一种纯面向对象的编程语言，它不支持类。因此，类型上的方法是实现类似于类的行为的一种方式。方法允许对与类似于类的类型相关的行为进行逻辑分组。

一个*方法*声明类似于一个 ***函数声明*** ，但是它有一个额外的参数声明部分。额外参数部分可以包含且只能包含*方法*的接收器类型的一个参数。唯一的一个参数叫做*方法*声明的接收器参数。接收器参数必须包含在`()`中，并在`func`关键字和*方法*名称之间声明。

**例如:**

下面我将**自定义**类型**定义为**员工。我也有*方法*调用`accountDetails()`，接收者类型`a`作为账号

```
package mainimport "fmt"type account struct {
    accountName   string
    accountType   string
    accountValue  int
}func (a account) accountDetails() {
    fmt.Printf("Account Name: %s\n", a.accountName)
    fmt.Printf("Account Type: %s\n", a.accountType)}func (a account) getAccountValue() int {
    return a.accountValue
}

func main() {
    acc := account{accountName: "Chase Account", accountType: "saving", accountValue: 21000}
    acc.accountDetails()
    fmt.Printf("Account Value Updated after calling by reference: %d\n", acc.getAccountValue())
}
```

方法`accountDetails()`附加到接收器类型`account`上，并提供访问字段`accountName`和`accountType`的方法。类似地,`getAccountValue()`方法返回接收者`account`类型的`accountValue`。

现在假设您试图在一个*方法*中修改接收器的字段值。假设您想用一个名为`updateAccountValue()`的额外的*方法*来更改银行账户的`accountValue`

```
package mainimport "fmt"type account struct {
    accountName   string
    accountType   string
    accountValue  int
}func (a account) accountDetails() {
    fmt.Printf("Account Name: %s\n", a.accountName)
    fmt.Printf("Account Type: %s\n", a.accountType)
    fmt.Printf("Account Value: %d\n", a.accountValue)
}func (a account) getAccountValue() int {
    return a.accountValue
}func (a account) updateAccountValue(updatedAccountValue int) int {
    a.accountValue = updatedAccountValue
    return a.accountValue
}func main() {
    acc := account{accountName: "Chase Account", accountType: "saving", accountValue: 21000}
    acc.accountDetails()
    acc.updateAccountValue(22000)
    if acc.getAccountValue() == 21000 {
        fmt.Printf("Did Account value updated after calling acc.updateAccountValue method: [%d] - No\n", acc.getAccountValue())
    } else if acc.getAccountValue() == 22000 {
        fmt.Printf("Did Account value updated after calling acc.updateAccountValue method: [%d] - Yes\n", acc.getAccountValue())
    }
}
```

**输出:**

```
Account Name: Chase Account
Account Type: saving
Account Value: 21000
Did Account value updated after calling acc.updateAccountValue method: [21000] - No
```

这里发生了什么，当方法`updateAccountValue()`被调用时，接收器的副本被制作，并且接收器的副本在方法内部是可用的。因为它是一个副本，所以对值接收者的任何更改对调用者都是不可见的。为了完成这项工作，我们需要为*方法使用一个 ***指针接收器*** 。对指针接收器所做的任何更改都将被调用者看到。*

```
package mainimport "fmt"type account struct {
    accountName   string
    accountType   string
    accountValue  int
}func (a account) accountDetails() {
    fmt.Printf("Account Name: %s\n", a.accountName)
    fmt.Printf("Account Type: %s\n", a.accountType)
    fmt.Printf("Account Value: %d\n", a.accountValue)
}func (a account) getAccountValue() int {
    return a.accountValue
}**func (a *account) updateAccountValue(updatedAccountValue int) int {
    a.accountValue = updatedAccountValue
    return a.accountValue
}**func main() {
    acc := account{accountName: "Chase Account", accountType: "saving", accountValue: 21000}
    acc.accountDetails()
    acc.updateAccountValue(22000)
    if acc.getAccountValue() == 21000 {
        fmt.Printf("Did Account value updated after calling acc.updateAccountValue method: [%d] - No\n", acc.getAccountValue())
    } else if acc.getAccountValue() == 22000 {
        fmt.Printf("Did Account value updated after calling acc.updateAccountValue method: [%d] - Yes\n", acc.getAccountValue())
    }
}
```

**输出:**

```
Account Name: Chase Account
Account Type: saving
Account Value: 21000
Did Account value updated after calling acc.updateAccountValue method: [22000] - Yes
```

所以有必要为方法定义 ***指针接收器*** 来修改原始的调用方字段**……不，不一定。**我们可以很好的调用实际的`acc` 实例中的方法并修改字段。为此我们不得不像这样使用`&acc`

```
(&acc).updateAccountValue(22000)
```

**方法**和**接收器**都需要在同一个包中定义，否则无效。方法可以通过将**大写**而在包之间共享，也称为**导出**，即

```
***💡 Will only be available within the package***

func (a *account) **updateAccountValue**(updatedAccountValue int) int {
    a.accountValue = updatedAccountValue
    return a.accountValue
}***💡 Will be available outside of the package***func (a *account) **UpdateAccountValue**(updatedAccountValue int) int {
    a.accountValue = updatedAccountValue
    return a.accountValue
}
```

`updateAccountValue`只能在定义的包内访问，在我们的例子中是`**main**`，但是`UpdateAccountValue` 可以在包外访问，因为`Go`会将它们导出到模块。

`**structs**`和`**struct fields**`也是如此

**例如:** `account`结构不是**导出的**，但是`AccountValue`字段中的`account`结构是**导出的，**表示`account`结构在包外不可访问，但是`AccountValue`字段将被导出并且在包外可用。

`**AccountValue**` 结构为**大写**，因此将被导出并在包外提供

```
type account struct {
    accountName   string
    accountType   string
    AccountValue  int
}type Money struct {
    available   int
    interest    int
    percentage  int
}
```

到目前为止，我们已经了解到:

*   *方法*是在`Golang`中表现为 OOPs 的方式
*   *方法*需附式
*   *方法* 可以通过指针和数值两种方式访问接收方字段
*   *方法*需与受体同包定义
*   *方法通过**大写**可以将*导出到包装外使用

还有一些类似*方法链接*等的东西，我会在以后尝试覆盖…希望这会有所帮助！！

# 快乐编码！！