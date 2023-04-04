# 两个总和 leetcode 问题解决方案快 1000 倍

> 原文：<https://blog.devgenius.io/two-sum-leetcode-problem-solution-1000x-faster-cdb8332dcc34?source=collection_archive---------8----------------------->

![](img/e9af9f2bdc7bc2b40a96fb18adf357a9.png)

在这篇文章中，你将学习解决两个和的 leetcode 问题的方法，在这篇文章中，我们将看到两个和的 leetcode 问题的多种解决方案，你也可以根据你的要求更新下面的代码。

# 两个总和 leetcode 问题陈述:

给定一个整数数组 *nums* 和一个整数 *target* ，返回这两个数字的索引*，这样它们加起来就是 target* 。

你可以假设每个输入都有*恰好*一个解，并且你不能使用*相同的*元素两次。

可以任意顺序返回答案。

示例 1:

```
Input: nums = [2,7,11,15], target = 9
Output: [0,1]
Explanation: Because nums[0] + nums[1] == 9, we return [0, 1].
```

示例 2:

```
Input: nums = [3,2,4], target = 6
Output: [1,2]
```

示例 3:

```
Input: nums = [3,3], target = 6
Output: [0,1]
```

我想你们现在已经明白我们要做什么了，如果你还不明白，我在这里，让我为你解释一下。

假设我们有一个名为*arr =【2，7，11，15】*的未排序数组和一个目标值 *target=9* ，如果你将索引 *0* 值(即 *2* 和索引 *1* 值(即 arr 的 *7* )相加，你将得到答案 *9* ，它与*目标*完全相同 因为我们不需要两个和的所有可能对，我们可以返回我们得到的索引，即 *[0，1]* ，所以这将是我们根据给定问题的答案。索引的顺序可以从 *[0，1]* 改变为 *[1，0]* ，这在两个和的 leetcode 问题中是可以接受的。

```
Runtime: 2 ms, faster than 99.24% of Go online submissions for Two Sum.
Memory Usage: 4.3 MB, less than 54.46% of Go online submissions for Two Sum.
```

# 解决两个和问题的方法；

下面是解决围棋中两个和问题的所有可能的方法

*   强力(使用两个循环)
*   使用 Go 地图
*   使用左右指针
*   所有可能的配对

这些是您将在下面的操作中看到的所有可能的实现，所以不用花费任何额外的时间，让我们一个接一个地看这些实现。

# 1.蛮力或天真的方法

蛮力或天真的方法是得到结果的最简单的方法，我们要做的是，在这个方法中我们需要两个循环，这样我们可以依次迭代每个索引，以检查总和是否等于给定的目标值，如果匹配，那么我们将立即返回两个元素的索引，正如我之前提到的，返回的索引的顺序在这里并不重要。

## 算法:

*   步骤 1:创建一个函数，该函数将有两个参数:第一个*未排序数组*，第二个参数将是*目标*值，该函数还将返回索引的数组/切片。
*   步骤 2:创建一个循环 *i=0* 并遍历未排序的数组长度
*   步骤 3:创建第二个循环，从 *j = i+1* 开始迭代到未排序数组的长度。
*   第四步:在第二个循环中，对每次迭代的 *arr[i]* 和 *arr[j]* 的值求和，并存储在一个名为 *sum = arr[i] + arr[j]* 的变量中。
*   第五步:在得到每个指标的总和后的第二次循环中，写一个 if 条件并检查， *sum == target* 。
*   步骤 6:如果步骤 5 为真，则返回索引 *return []int{i，j}* ，您可以更改 *i，j* 的位置。

现在我们有了一个算法。下面就按照顺序来实现吧。

## 代码:

下面是简单方法的代码

```
package main
import (
    "fmt"
)
func TwoSumNaive(slice []int, target int) []int { // step 1
    for i := 0; i < len(slice)-1; i++ { // step 2
        for j := i + 1; j < len(slice); j++ { // step 3
            sum := slice[i] + slice[j] // step 4
            if sum == target { // step 5
                return []int{i, j} // step 6
            }
            continue
        }
    }
    return []int{}
}
func main() {
    slice := []int{2, 7, 2, 11, 15, 6}
    target := 9
    sum := TwoSumNaive(slice, target)
    fmt.Println(sum)
}
```

## 解释:

正如你在代码中看到的，我们有一个未排序的数组/切片 *slice := []int{2，7，2，11，15，6}* 和一个目标值 *target := 9* ，然后我们调用 *TwoSumNaive()* 函数，并将 *slice* 和 *target* 变量作为参数传递给该函数。在 *TwoSumNaive()* 内部，我们首先在未排序的数组/片上开始从 *0 到 len(arr)-1* 的循环，并且在第一次循环之后，我们开始从 i+1 到未排序数组的最后一个元素的另一次循环，因此执行将像这样发生，例如 *i=0* 和 *j=0+1* 即 *1* ，以及索引值 *0* 对于索引 *1* 它是 *7* ，那么和值将是 *2 + 7 = 9* ， *sum = 9* ，接下来我们将比较 sum 和 target，因为我们有 sum *9* 并且 target 也是 *9* ，然后它将返回索引 *i* 和 *j* ，所以我们的答案将是

*下面是 leetcode 和本地系统执行结果:*

```
*$ go run .\main.go
[0 1]*
```

```
*Runtime: 46 ms, faster than 22.60% of Go online submissions for Two Sum.
Memory Usage: 3.6 MB, less than 74.60% of Go online submissions for Two Sum.*
```

*Leetcode 的结果看起来不太好，因为这比我在文章中提到的要花更多的时间。*

# *2.使用散列表*

*天真的方法是程序员能想到的最简单的方法，但对于大长度数组，天真的方法并没有像我们需要的那样快，因为你可以为这个数组 *{2，7，2，11，15，6}* 花费几乎 *46 ms* 这并不是很快，所以，为了使它更快，我们首先需要消除我们的第二个循环，我们还需要新的数据结构 HashMap， 在 Go 中，这是一个*映射*，所以我们要做的不是将我们的值相加，而是检查*diff = target–arr[index]*，例如*diff =**9–2*和 *diff* 将是 *7* ，因为我们有剩余值，所以我们将查看 *7 的映射**

## *算法*

*   *步骤 1:创建一个函数，该函数将有两个参数:第一个*未排序数组*，第二个参数将是*目标*值，该函数还将返回数组/索引片。*
*   *步骤 2:声明一个 map 类型的变量，这样我们就可以查找剩余的值。*
*   *步骤 3:创建一个循环，遍历未排序的数组/切片。*
*   *第四步。从数组/切片中减去*索引*的*目标*值，例如*diff = target–slice[idx]*。*
*   *步骤 5:将差值作为键传递给 map，检查这个键是否存在于我们的 hashmap 中。*
*   *步骤 6:如果键存在于我们的 hashmap 中，提取键的值并返回当前的索引和值。*
*   *步骤 7:如果键不存在，那么将当前索引的值作为键和值添加到我们的 HashMap 中。*

*我们有我们的算法，让我们实现它，看看它的结果，下面是算法的实现:*

## *密码*

```
*package main

import (
    "fmt"
)

// using HashMap
func TwoSumHashmap(slice []int, target int) []int {
    hashMap := map[int]int{}
    for idx := 0; idx < len(slice); idx++ {
        pmatch := target - slice[idx]
        if v, exists := hashMap[pmatch]; exists {
            return []int{idx, v}
        }
        hashMap[slice[idx]] = idx
    }
    return nil
}

func main() {
    slice := []int{2, 7, 2, 11, 15, 6}
    target := 9
    sum := TwoSumHashmap(slice, target)
    fmt.Println(sum)
}*
```

## *解释:*

*我们在这个代码示例中使用 map，你可以看到我们有一个名为 *TwoSumHashmap* 的函数，它有两个参数:第一个是未排序的数组，第二个是我们的目标值，它也返回 *[]int* 。*

*在我们的函数中，我们已经声明了一个名为 *hashMap* 的映射，因为我们都知道 Go 的映射保存了键和值对信息，所以我们最好使用它，而不是像在*方法 1* 中那样使用另一个循环来迭代所有值。*

*在声明一个映射之后，我们在给定的片/数组上运行我们的循环，直到数组的最后一个元素。在循环中，我们在每次迭代中减去 target-slice[idx ],在下一步中，我们直接将差值作为映射的键传递。在 Go 中，你可以像这样检查所传递的键是否存在于映射中，因为第二个返回值返回一个 *Boolean* 值，如果所提供的键存在于键中，则该值为真，否则它将返回一个假值，因此如果我们的键存在于映射中，则我们将直接返回数组的当前索引和从键中获得的值，该键也是一个索引， 如果为假，那么我们将进入下一步，我们将添加当前索引数组/片值作为键，添加当前索引作为它的值，这样它将一直工作，直到找到键，并将返回索引，如果没有找到，函数将返回 *nil* 。*

*下面是 Leetcode 和本地系统的代码输出:*

```
*$ go run .\main.go
[1 0]*
```

```
*Runtime: 2 ms, faster than 99.24% of Go online submissions for Two Sum.
Memory Usage: 4.3 MB, less than 54.46% of Go online submissions for Two Sum.*
```

*与强力方法相比，HashMap 要好得多。Hashmap 几乎比暴力破解快 2300%。*

*下面是得到一对两个和的另一种方法，让我们看看*

# *3.左右指针*

**免责声明:*这种方法可能在 leetcode 中不起作用，因为这种方法只对排序数组起作用，而且通过排序索引可能会改变，所以你的答案。*

*在这种方法中，我们将首先对数组进行排序，然后我们将创建一个 for 循环(无限),并添加一些条件，基于这些条件，我们的左右指针将向前和向后移动。让我们来看看这个算法:*

## *算法*

*   *步骤 1:创建一个函数，该函数将有两个参数:第一个*未排序数组*，第二个参数将是*目标*值，该函数还将返回数组/索引片。*
*   *第二步:声明两个变量 left pointer 和 right pointer，并初始化它们的值 *left = 0* ， *right=len(arr)* 。*
*   *第 3 步:首先对你的数组进行排序，在 go 中你可以使用 *Sort* 包进行这个*排序。Ints()* 。*
*   *第四步。创建一个 *for ever 循环*for start！=结束。*
*   *步骤 5:对左右数组索引值求和。*
*   *步骤 6:如果 sum > target，则添加条件:如果为真，则*right = right–1*向后移动指针。*
*   *步骤 7:else if sum< target: if true then left = left + 1, move pointer forward.*
*   *Step 8: else: return []int{left, right}.*

## *Code*

```
*package main

import (
    "fmt"
    "sort"
)

func TwoSumSortedArray(slice []int, target int) []int {
    if len(slice) < 2 {
        fmt.Println("can't process")
        return nil
    }
    sort.Ints(slice)
    start := 0
    end := len(slice) - 1
    fmt.Println("After Sorting:", slice)

    for start != end {
        sum := slice[start] + slice[end]
        if sum > target {
            end = end - 1
        } else if sum < target {
            start = start + 1
        } else {
            return []int{start, end}
        }
    }
    return nil
}

func main() {
    slice := []int{2, 7, 2, 11, 15, 6}
    target := 9
    fmt.Println("Before Sorting:", slice)
    sum := TwoSumSortedArray(slice, target)
    fmt.Println(sum)
}*
```

## *Explanation*

*As you can see in the *TwoSumSortedArray*函数首先，我们添加了一个条件来检查数组/片是否有足够的数据长度，因为我们不想让我们的程序崩溃:)，然后我们对数组/片进行了排序，现在数组的顺序已经改变，所以现在的结果不会像前面的例子那样。接下来我们声明了我们的指针并赋值。*

*我们开始了循环，并添加了一个条件，所以只要我们的指针值相等，循环就会结束。*

*在循环中，我们添加左和右索引值，并将其传递给我们所拥有的条件，并根据总和和目标值将指针向左和向右移动。如果目标值大于左和右指针数组索引值的联合值，那么我们将把结束指针向后移动一步，对于其他条件，我们将把*开始(左)*指针向前移动，如果两个条件都不满足，那么我们将返回开始(左)和结束(右)值。*

```
*$ go run .\main.go
Before Sorting: [2 7 2 11 15 6]
After Sorting: [2 2 6 7 11 15]
[0 3]*
```

# *4.所有可能的配对程序*

*如果我们想要所有可能的两个和对，是的，你可以在修改一点代码后，用上面的两种方法得到所有可能的和对。*

*下面是所有可能的两个和对的代码:*

## *蛮力:*

```
*func TwoSumNaive(slice []int, target int) [][]int {
    s := [][]int{}
    for i := 0; i < len(slice)-1; i++ {
        for j := i + 1; j < len(slice); j++ {
            sum := slice[i] + slice[j]
            if sum == target {
                s = append(s, []int{i, j})
            }
            continue
        }
    }
    return s
}*
```

## *散列表:*

```
*func TwoSumHashmap(slice []int, target int) [][]int {
    s := [][]int{}
    hashMap := map[int]int{}
    for idx := 0; idx < len(slice)-1; idx++ {
        pmatch := target - slice[idx]
        if v, exists := hashMap[pmatch]; exists {
            s = append(s, []int{idx, v})
            delete(hashMap, pmatch)
        }
        hashMap[slice[idx]] = idx
    }
    return s
}*
```

**本文原贴于*[*programmingeeksclub.com*](https://programmingeeksclub.com/two-sum-leetcode-1000x-faster/)*

**我的个人博客网站:* [*编程极客俱乐部*](https://programmingeeksclub.com/)
*我的脸书页面:* [*编程极客俱乐部*](https://www.facebook.com/profile.php?id=100086258693659)
*我的电报频道:* [*编程极客俱乐部*](https://t.me/dpgcl)
*我的推特账号:* [*库尔迪普辛格*](https://twitter.com/kusinghofficial)
*我的 Youtube 频道:【T28**