# 一个问题，五种解决方案——素性

> 原文：<https://blog.devgenius.io/one-problem-five-solutions-primality-734078141bc3?source=collection_archive---------3----------------------->

读者们好，这是本系列的又一集，我们将深入研究一个计算机科学问题，并用五种编程语言对其进行分析，这可能是今年本系列的最后一集，所以就当是我的圣诞礼物吧。

我们今天要解决的问题是一个经典的素性问题，我从 hackerrank 上摘下来的，让我们来看看问题描述:

一个 ***质数是一个大于 1 的自然数，它除了 1 和它本身之外没有其他的正整数。给定一个 p 整数，确定每个整数的素性，并在新的一行返回素或不素。***

所以如果数是质数我们返回 ***【质数】*** 如果不是那么 ***【非质数】*** 返回

有了上面的介绍，我想我们已经足够舒服了，可以开始寻找针对这个问题的解决方案了

# 朱莉娅

第一个解决方案来自本系列中的一种语言，Julia 语言的特性和简单性让我非常惊讶，即使你没有使用这种语言的经验，用 Julia 编写代码的感觉也是如此自然和甜美

```
function primality(n)
    if n <= 1
        return "Not prime"
    end

    for x in 2:n-1
        if n%x == 0
            return "Not prime"
        end
    end
    "Prime"
end
```

Julia 自动返回最后一条语句的方式非常有趣和有用，因为一旦 Julia 返回，我们只需验证它不是素数，如果它小于 1，它就不是素数

如果 n 大于 1，则创建一个从 2 到 n-1 的范围，我们设置一个循环来遍历它，如果 n 可以被这个范围内的任何值整除，我们只需返回***【Not prime】***。

# Vlang

在本文的所有解决方案中，我认为这个解决方案是最冗长的，因为 V 更接近于系统语言或者更明确的语言类型，比如 GO

```
fn primality(n int) string {
 if n <= 1 {
  return "Not prime"
 }

 arr_size := n-2

 entries := []int{len: arr_size, init: it+2}

 for i in entries {
  if n%i == 0 {
   return "Not prime"
  }
 }

 return "Prime"
}
```

这个过程有点类似于 Julia 解决方案，我们检查 n 是否大于 1，如果不是，那么我们定义可能的整除值的大小，然后，我们填充一个可能是除数的条目数组

然后在条目中执行一个循环，检查它是否可分，如果是，那么我们返回 ***【非素数】*** ，否则我们只需返回 ***【素数】*** 。

# Java Script 语言

下一个解决方案是 JavaScript 解决方案，对于命令式语言来说，这个解决方案非常简短，我不喜欢的唯一一点是我不得不使用 for 循环，因为我认为它比手动改变状态的 while 循环更合适

```
const primality = (n) => {
    if(n<=1) 
        return "Not prime"
    for (let divisor = 2; divisor < n-1; divisor++) {
        if(n%divisor == 0)
            return "Not prime"
    }
    return "Prime"
}
```

# 长生不老药

函数式语言在解决方案的格式上会有更多的分歧

```
defmodule Primality do 
  def primality(n) when n <= 1 do
    "Not prime"
  end
  def primality(2), do: "Prime"
  def primality(n) do 
    any_divisible = Enum.any? 2..n-1, &(rem(n, &1)==0)
    case any_divisible do 
      true -> "Not prime"
      _ -> "Prime"
    end
  end
end
```

在解决方案中使用模式匹配，我们多次声明函数，在第一次声明中验证它是否小于 1，在第二次声明中验证它是否为 2，在最后一次声明中我们使用一个***“any？”*** 验证 n 是否可被从 2 到 n-1 的区间中的任何条目整除的函数，如果是，我们返回 ***【非素数】*** ，否则我们只返回 case 结构中的 ***【素数】*** 。

# 哈斯克尔

第二种函数式语言也有一个不同于之前所有语言的解决方案

```
primality n 
    | n <= 1 || anyDivisible = "Not prime"
    | otherwise = "Prime"
    where
        interval = [2..n-1]
        anyDivisible = length (filter (\v -> n`rem`v==0) interval) > 0 
```

在 Haskell 中，我选择使用 guards 来验证条件，在 where 作用域上，我定义了一个从 2 到 n-1 的区间，并应用了一个过滤器来获取所有可能的除数。

在第一个保护子句中，我们检查它是否小于 1，或者是否找到了任何约数，如果是，那么它不是素数

而在最后一个子句上，我们只需返回***“Prime”。***

起初，我认为这个解决方案足够好，但后来我想起 Haskell 还有一个***【any】***函数，所以我对这个解决方案做了一些改进，使用了***【any】***函数，并计算了我在函数组合***【function 2】中使用的布尔值。function1"*** 在这里，我执行除法的剩余部分，并将结果传递给第二个函数，比较它是否等于零。

```
primality n 
    | n <= 1 || anyDivisible = "Not prime"
    | otherwise = "Prime"
    where
        interval = [2..n-1]
        anyDivisible = any ((0 ==) . (n`rem`)) interval
```

就这样，我们在这个系列中又做了一集，在这个系列中尝试新语言和推出新语言非常好，当然，明年会有更多集，所以请继续关注
祝你假期愉快，明年见！！
感谢阅读！