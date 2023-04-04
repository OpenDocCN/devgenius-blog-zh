# 堆栈第 2 部分—Go 中后缀的中缀

> 原文：<https://blog.devgenius.io/stacks-part-2-infix-to-postfix-in-go-610da8776be?source=collection_archive---------12----------------------->

欢迎回到*围棋数据结构简介*！正如我所承诺的，我们将研究堆栈的一个特殊应用。具体来说，我们将探讨如何将中缀表达式转换成它们的后缀等价物。后缀符号可能一开始看起来不直观，但是在阅读了这篇文章之后，它会变得更加清晰。

# 简明入门

“Jacob，距离你上次发帖已经整整一周了。我不记得这些是什么了。”好的，让我们从解释这些是什么开始。

我们在数学课上学到了表达式是术语和运算符的组合，通常以这种格式编写。

```
a + b
```

这就是所谓的*中缀*符号。这是因为运算符是在操作数之间的中的**。**

我们还用另外两种符号来计算表达式。它们是*前缀*和*后缀*符号。以下是三种表达方式的对比。

```
Infix: <operand><operator><operand>
Prefix: <operator><operand><operand>
Postfix: <operand><operand><operator>
```

上面的表达式`a + b`，也可以这样写:

```
Infix: a+b
Prefix: +ab
Postfix: ab+
```

我们使用中缀符号是因为它易于理解。但是通常对我们人类来说容易的事情对计算机来说却很难。想象一下，写一个程序来计算一个表达式。像`a + b`这样简单的表达足够简单。然而，想象一下如果表达式变得更复杂。

```
{(a*b) + (c*d)} - e
```

当有很多括号和运算符时，你可以看到难度增加得有多快。我们不仅需要处理操作，还需要跟踪操作的顺序。因为有嵌套的括号，我们必须先前后跳转来计算括号内的表达式。跟踪我们在哪里将是一场噩梦。

现在看看这个。

```
ab*cd*+e-
```

后缀符号更容易理解，因为我们只需要从左到右扫描表达式就可以得到结果。

# 我们如何评估一个后缀表达式？

我们来看看这个表情:

```
ab*cd*+e-
a = 2
b = 5
c = 3
d = 1
e = 4
```

我们首先从左向右扫描。当我们看到一个操作员时，我们会停下来。

```
2 5 * 3 1 * + 4 -
    ^
```

一旦我们看到操作员，我们就后退两步。每次我们回去的时候，我们都会记下数字。然后，我们评估表达式。在这种情况下，我们在乘法运算符`*`处停止，然后后退两步。我们跟踪这些数字，它们是`2`和`5`。要进行评估，我们需要做的就是`2 * 5`，这就产生了`10`。应用操作的顺序很重要。例如:如果我们的操作符是`-`而不是`*`，那么`2 - 5`应该会产生`-3`，但是做`5 - 2`会产生`3`。

```
2 5 * 3 1 * + 4 -
    ^
Numbers: 5, 2
Evaluation: 2 * 5 = 10Result:
10 3 1 * + 4 -
```

太好了。我们现在需要做的就是重复上述步骤，直到完成。

```
10 3 1 * + 4 -
       ^
Numbers: 1, 3
Evaluation: 3 * 1 = 3Result:
10 3 + 4 -10 3 + 4 -
     ^
Numbers: 3, 10
Evaluation: 3 + 10 = 13Result:
13 4 -13 4 -
     ^
Numbers: 4, 13
Evaluation: 13 - 4 = 9Result:
9
```

我们最终得到了`9`。为了仔细检查，让我们计算一下中缀表达式。

```
Evaluate {(a*b) + (c*d)} - e, given these conditions:
a = 2, b = 5, c = 3, d = 1, e = 4{(2 * 5) + (3 * 1)} - 4 
= {10 + 3} - 4 
= 13 - 4 
= 9
```

瞧。我们已经成功评估了后缀表达式。

注意我们跟踪的`Numbers`是如何像堆栈一样运行的吗？当我们看到一个运算符时，我们回溯两次，并将这些数字推入`Numbers`堆栈。然后，我们弹出这些数字并应用操作。

# 将其转换为手动方式

我们可以使用堆栈将中缀符号转换成后缀等价符号。在我们讨论如何将其写入 Go 之前，理解该过程背后的逻辑很重要。

假设我们需要转换这个表达式:

```
a+b*c
```

我们应该做的是跟踪运算符，类似于我们在前面的例子中跟踪操作数的方式。

首先，我们从左向右扫描。当我们看到一个操作数时，我们把它写到边上。当我们看到一个运算符时，我们将其添加到我们的运算符列表中。

```
a + b * c
^
Operators:
Result: aa + b * c
  ^
Operators: +
Result: aa + b * c
    ^
Operators: +
Result: aba + b * c
      ^
Operators: +, *
Result: ab
```

这是最重要的部分。

*   当添加运算符时，我们需要检查我们所指向的运算符是否比列表顶部的运算符具有更高的优先级。这里，我们所指向的运算符`*`优先于列表顶部的运算符`-`。乘法优先于减法。
*   如果它确实优先，我们只需要将指向的运算符添加到我们的运算符列表中。
*   但是，如果它没有优先，我们弹出列表，并将列表中的运算符附加到结果字符串中。我们继续这样做，直到我们指向的运算符优先于列表顶部的运算符。在所有这些之后，我们将运算符添加到列表中。

让我们继续。

```
a + b * c
        ^
Operators: +, *
Result: abc
```

一旦我们到达最后，我们弹出操作符列表，并将项目附加到结果字符串中，如下所示:

```
Result: abc*+
```

让我们再看一个例子。

```
a * b + c
^
Operators:
Result: a
---a * b + c
  ^
Operators: *
Results: a---a * b + c
    ^
Operators: *
Results: ab---a * b + c
      ^
Operators: * // check for precedence before you add +!
Results: ab
```

这里，我们以第二种情况结束。由于`+`不优先于`*`，我们需要弹出操作符列表，直到`+`优先。在这种情况下，我们需要弹出它，直到列表中没有任何内容。

```
a * b + c
      ^
Operators: + // + is pushed in after * is popped out
Results: ab* // * is popped and appended into the result string
```

剩下的就简单了。

```
a * b + c
        ^
Operators: +
Results: ab*c---Results: ab*c+
```

让我们看最后一个例子。这一次，我们将转换一个带括号的表达式。

```
{(a*b) + (c*d)} - e
```

别担心，我们只需要再添加一条规则。

*   如果它是一个左括号，我们把它添加到我们的操作符列表中。
*   如果是右括号，我们弹出列表，直到弹出相应的括号。
*   我们不需要在结果字符串后面加上括号。后缀符号不需要。

让我们一步一步来！

```
{ ( a * b ) * c - d } - e
    ^
Operators: {, (
Result: a---{ ( a * b ) * c - d } - e
      ^
Operators: {, (, *
Result: a---{ ( a * b ) * c - d } - e
        ^
Operators: {, (, *
Result: ab---{ ( a * b ) * c - d } - e
          ^
Operators: { // pop until the corresponding parenthesis is popped
Result: ab* // * is appended---{ ( a * b ) * c - d } - e
            ^
Opeartors: {, *
Result: ab*---{ ( a * b ) * c - d } - e
              ^
Operators: {, *
Result: ab*c---{ ( a * b ) * c - d } - e
                ^
Operators: {, - // - doesn't take precedence over *
Result: ab*c* // * is popped and appended---{ ( a * b ) * c - d } - e
                  ^
Operators: {, -
Result: ab*c*d---{ ( a * b ) * c - d } - e
                    ^
Operators: 
Result: ab*c*d----{ ( a * b ) * c - d } - e
                      ^
Operators: -
Result: ab*c*d----{ ( a * b ) * c - d } - e
                        ^
Operators: -
Result: ab*c*d-e---Result: ab*c*d-e-
```

它看起来很复杂，只是因为我们一步一步慢慢来。逻辑本身非常简单。

同样，操作符列表的作用就像一个堆栈，因为我们不断地从堆栈中推出和取出。

# 去把它写在 Go 里

太好了！现在我们已经理解了高级逻辑是如何工作的，我们可以开始在 Go 中实现它了。

```
type Stack struct {
    items []float64
}func (s *Stack) Push(data float64) {
    s.items = append(s.items, data)
}func (s *Stack) Pop() {
    if s.IsEmpty() {
        return
    }
    s.items = s.items[:len(s.items)-1]
}func (s *Stack) Top() (float64, error) {
    if s.IsEmpty() {
        return 0.0, fmt.Errorf("stack is empty")
    }
    return s.items[len(s.items)-1], nil
}func (s *Stack) IsEmpty() bool {
    if len(s.items) == 0 {
        return true
    }
    return false
}func (s *Stack) Print() {
    for _, item := range s.items {
        fmt.Print(item, " ")
    }
}
```

我们将使用上一篇文章中的堆栈实现。这个堆栈可以容纳`float64`个类型。

让我们先来看看如何计算一个后缀表达式。

## 评估后缀表达式

```
func evaluatePostfix(exp string) (float64, error) {
    operands := new(Stack)
    chars := strings.Split(exp, " ") for _, char := range chars {
        if !isOperator(char) {
            op, err := strconv.ParseFloat(char, 64)
            if err != nil {
                return 0.0, err
            }
            operands.Push(op)
        } else {
            operand2, err := operands.Top()
            if err != nil {
                return 0.0, err
            }
            operands.Pop() operand1, err := operands.Top()
            if err != nil {
                return 0.0, err
            }
            operands.Pop() calculated, err := calculate(char, operand1, operand2)
            if err != nil {
                return 0.0, err
            } operands.Push(calculated)
        }
    }
    result, err := operands.Top()
    if err != nil {
        return 0.0, err
    } return result, nil
}
```

我们首先通过空格将表达式分割成单独的字符。表达式应该是操作数和运算符的组合，每个都用空格分隔。我们现在可以遍历得到的字符片段。

对于每个字符，我们想通过使用`isOperator`辅助函数来检查它是操作数还是运算符。如果一个字符是一个操作数，我们把它压入堆栈。

如果角色是一个操作符，我们弹出堆栈中最上面的两个项目，将它们保存为`operand2`和`operand1`。然后，我们使用`calculate`辅助函数对表达式求值。一旦计算完成，我们就把它推到堆栈中。

迭代完表达式后，我们从堆栈中获取最顶端的项，这是我们的最终结果。

以下是助手函数的定义。

```
func isOperator(char string) bool {
    switch char {
    case "+":
        return true
    case "-":
        return true
    case "*":
        return true
    case "/":
        return true
    default:
        return false
    }
}func calculate(operator string, operand1, operand2 float64) (float64, error) {
    result := 0.0
    switch operator {
    case "+":
        result = operand1 + operand2
    case "-":
        result = operand1 - operand2
    case "*":
        result = operand1 * operand2
    case "/":
        result = operand1 / operand2
    default:
        return 0.0, fmt.Errorf("invalid operator")
    } return result, nil
}
```

现在我们可以看看如何编写中缀到后缀的转换函数。

## 将中缀转换为后缀

我们将改变堆栈，使其存储`string`类型，而不是`float64`类型。

```
func infixToPostfix(exp string) (string, error) {
    operators := new(StringStack)
    chars := strings.Fields(exp)
    result := "" for _, char := range chars {
        if isOpeningParenthesis(char) {
            operators.Push(char)
        } else if isClosingParenthesis(char) {
            for !operators.IsEmpty() && !isMatchingParenthesis(ignoreError(operators.Top()), char) {
                top, err := operators.Top()
                if err != nil {
                    return "", err
                }
                result += top
                operators.Pop()
            }
            operators.Pop()
        } else if !isOperator(char) {
            result += char
        } else if isOperator(char) {
            for !operators.IsEmpty() && hasHigherPrecedence(ignoreError(operators.Top()), char) && !isOpeningParenthesis(ignoreError(operators.Top())) {
                top, err := operators.Top()
                if err != nil {
                    return "", err
                }
                result += top
                operators.Pop()
            }
            operators.Push(char)
        }
    } for !operators.IsEmpty() {
        top, err := operators.Top()
        if err != nil {
            return "", err
        }
        result += top operators.Pop()
    } return result, nil
}
```

开头也差不多。我们创建一个字符串片段来保存所有拆分的字符。这里的新内容是我们需要处理四个案例。

*   当字符是一个左括号时，我们把它压入堆栈。
*   当字符是右括号时，我们弹出所有的操作符，直到弹出一个匹配的左括号。
*   当字符是一个操作数时，我们把它附加到结果字符串中。
*   当字符是一个操作符时，我们检查优先级。如果栈中最顶端的操作符的优先级高于字符，我们就弹出直到找到左括号。然后，我们将字符推入堆栈。

最后，我们将堆栈中剩余的内容刷新到结果字符串中。

以下是助手函数的定义。

```
func isOpeningParenthesis(char string) bool {
    switch char {
    case "(":
        return true
    case "{":
        return true
    case "[":
        return true
    default:
        return false
    }
}func isClosingParenthesis(char string) bool {
    switch char {
    case ")":
        return true
    case "}":
        return true
    case "]":
        return true
    default:
        return false
    }
}func isMatchingParenthesis(opening, closing string) bool {
    switch opening {
    case "(":
        if closing == ")" {
            return true
        }
    case "{":
        if closing == "}" {
            return true
        }
    case "[":
        if closing == "]" {
            return true
        }
    default:
        return false
    }
    return false
}func hasHigherPrecedence(target, source string) bool {
    if (target == "*" || target == "/") && (source == "+" || source == "-") {
        return true
    } else {
        return false
    }
}func ignoreError(val string, err error) string {
    return val
}
```

请注意，忽略错误不是一个好习惯。我用它来处理恼人的错误返回，因为我不能将`operators.Top()`传递给`hasHigherPrecedence`或`isMatchingParenthesis`，因为它返回两个值，其中一个是错误。如果您必须忽略这样的错误，您也可以定义一个类似于`stack.TopNoError()`的 struct 方法，它将返回没有任何错误变量的最上面的值。

# 结论

感谢您的阅读！那是一篇很长的文章，但是我为你能忍受我而感到骄傲。您通常不会遇到必须将中缀表达式显式转换为后缀表达式的情况，因为编译器会为您处理它。然而，这是一个学习栈如何工作的好方法，我认为这也是一个非常有趣的话题。下周我会带着一份关于排队的报告回来。到时候见！

你也可以在[发展到](https://dev.to/jpoly1219/stacks-part-2-infix-to-postfix-in-go-2oam)和[我的个人网站](https://jpoly1219.github.io)上阅读这篇文章。