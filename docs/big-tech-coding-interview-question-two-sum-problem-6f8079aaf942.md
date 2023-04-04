# 大技术编码面试问题——两个和问题

> 原文：<https://blog.devgenius.io/big-tech-coding-interview-question-two-sum-problem-6f8079aaf942?source=collection_archive---------6----------------------->

## 获得大型科技公司软件工程师职位的旅程

## 作为软件工程师解决两个求和问题详细实现

# 概观

这是“大技术编码面试问题”系列中的一篇文章，这篇文章是关于两个和问题的。对于任何科技公司软件工程师来说，两个问题是一个非常常见的面试问题。这被归类为编码面试中的一个简单问题。一旦你很好地理解了这个两个和的问题，它对解决更困难的问题有很大的帮助，比如三个和，在某些方面是两个和问题的延续。

![](img/f89b0dfd4b8fbee8598ec38d1b599e2d.png)

马库斯·斯皮斯克在 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral) 上的照片

**问题**

> 给定一个整数数组“nums”和一个整数数组“target ”,返回这两个数字的索引，使它们加起来等于 target。您可以假设每个输入只有一个解决方案，并且不能两次使用同一个元素。可以任意顺序返回答案。

**例如**

```
 Input: nums = [2,7,11,15], target = 9
Output: [0,1]
Explanation: Because nums[0] + nums[1] == 9, we return [0, 1].
```

**约束条件**

```
2 <= nums.length <= 10⁴
-10⁹ <= nums[i] <= 10⁹
-10⁹ <= target <= 10⁹
Only one valid answer exists.
```

# 解决两个和问题

首先，让我们试着理解这个问题。给我们一个整数元素数组和一个目标和。我们的工作是编写一个函数，返回数组中两个元素的索引，当我们将这两个元素相加时，它应该等于给定的目标和。这个问题有一个注解:

> 如果有多个这样的对，我们需要返回从左边找到的第一个对的索引。

这意味着我们不需要关心输出的顺序。例如，我们可以从上面的示例中返回[0，1]或[1，0]。

## 定义测试用例

其次，我们为这个问题定义测试用例，以确保我们很好地理解这个问题。这是五个测试案例，包括快乐案例和边缘案例。以下测试案例涵盖了满意案例和边缘案例，如负值、正值、零值以及无法从输入数组中找到目标:

```
testCase1nums[] = {5, 7, 12,1};
testCase1Target = 12;
expectedResult1 = [0, 1]testCase2nums[] = {3, 5, 0};
testCase2Target = 6;
expectedResult2 = nulltestCase3nums[] = {0, 3};
testCase3Target = 3;
expectedResult3 = [0, 1]testCase4nums[] = {0, -1, 2, -3, 1};
testCase4Target = -2;
expectedResult4 = [3, 4]testCase5nums[] = {4, -11, 22, -23, 31};
testCase5Target = -20;
expectedResult5 = null
```

你应该在编码前把这一步当作一个好的实践，因为它有助于确保你和面试官对问题有相同的理解。通过定义测试案例和每个案例的预期结果，如果您有误解，面试官可以帮助您纠正您的理解以解决问题，避免浪费时间。

## 解决方案 1:暴力

接下来，如何先用一个直截了当的方法解决这个问题？面试官关心你的想法，所以大声告诉他们你的想法。

真正强力的方法是使用两个嵌套的 for 循环搜索所有可能的数字对。尽管这种解决方案很慢，但是为了问题的完整性，最好先尝试一下暴力解决方案。正是从这些强力解决方案中，您可以在以后进行优化。

*对于给定的输入数组 nums[]，我们需要做这些步骤:*

1.  *运行两个 for 循环，检查给定数组 nums[]中的每个组合。*
2.  *在特定的索引处按住外环，移动内环以获得所有可能的对。*
3.  *在内循环的每次迭代中，我们发现由外循环和内循环索引表示的数字相加是否达到目标和。*
4.  *如果 nums[outer index]+nums[inner index]等于输入目标变量，则返回数组{outerLoopIndex，innerLoopIndex}作为结果。否则，我们继续迭代以检查下一对。*
5.  *重复上述步骤，直到找到符合给定目标变量的组合。*

这是遵循上述步骤的强力方法的 Java 代码:

**findtosumsolution 1 函数:**

```
**static int**[] findTwoSumSolution1(**int** nums[], **int** target) {
    **int** length = nums.**length**;
    **for** (**int** i = 0; i < (length - 1); i++) {
      **for** (**int** j = (i + 1); j < length; j++) {
        **if** (nums[i] + nums[j] == target) {
          **int** result[] = { i, j };
          **return** result;
        }
      }
    }
    **return null**;
  }
```

在 main 方法中，您可以调用这个函数并用上面的测试用例来验证输出:

```
**public static void** main(String [] args) {
    **int** testCase1nums[] = {5, 7, 12,1};
    **int** testCase1Target = 12;

    **int** testCase2nums[] = {3, 5, 0};
    **int** testCase2Target = 6;

    **int** testCase3nums[] = {0, 3};
    **int** testCase3Target = 3;

    **int** testCase4nums[] = {0, -1, 2, -3, 1};
    **int** testCase4Target = -2;

    **int** testCase5nums[] = {4, -11, 22, -23, 31};
    **int** testCase5Target = -20;
    System.***out***.println(Arrays.*toString*(*findTwoSumSolution1*(testCase1nums,testCase1Target)));
    System.***out***.println(Arrays.*toString*(*findTwoSumSolution1*(testCase2nums,testCase2Target)));
    System.***out***.println(Arrays.*toString*(*findTwoSumSolution1*(testCase3nums,testCase3Target)));
    System.***out***.println(Arrays.*toString*(*findTwoSumSolution1*(testCase4nums,testCase4Target)));
    System.***out***.println(Arrays.*toString*(*findTwoSumSolution1*(testCase5nums,testCase5Target)));
  }
```

**时间复杂度:** O(n )
**空间复杂度:** O(1)

在最坏的情况下，上述函数的运行时间复杂度为 O(n)。最坏的情况什么时候会发生？当所需的组合是嵌套循环要检查的最后一个组合时，会出现最坏的情况。

你可以在 [Github](https://gist.github.com/techisbeautiful/1379f488ed471c3fd3ca8b53b3d96b5e) 看到这个解决方案的完整代码。

## 解决方案 2: HashMap

在通过第一种方法解决问题后，你需要思考如何改进你的逻辑，使它能够更快更有效地运行，如果你想破解你的编码面试，尤其是当你面试 FAANG 公司时，这一点很重要。

使用哈希方法可以更好地解决这个问题。这个想法是使用 HashMap 来存储我们已经访问过的元素的索引:

*   HashMap 的键是给定输入数组中的数字。
*   HashMap 的值是由 HashMap 键表示的数组中数字的索引。

*以下是实现这一逻辑的步骤:*

1.  为键和值初始化一个整数散列表。
2.  从第一个元素开始，遍历给定数组 nums[]中的每个元素。
3.  在每一次迭代中，我们发现临时号码是否出现在散列表中。temp number =(target-nums[I])，I 为迭代的当前索引。
4.  *如果存在，返回{所需编号索引，当前编号索引}作为函数的结果。*
5.  *否则，将当前迭代编号作为键，将其索引作为值添加到 HashMap 中。*
6.  *重复这个逻辑，直到找到要返回的结果。如果找不到结果，返回 null*

**findtosumsolution 2 函数:**

```
**static int**[] findTwoSumSolution2(**int**[] nums, **int** target) {
    HashMap<Integer,Integer> map = **new** HashMap<>();
    **for**(**int** i = 0; i < nums.**length**; i++){
      Integer temp = (target - nums[i]);
      **if**(map.containsKey(temp)){
        **int** result [] = { map.get(temp), i };
        **return** result;
      }
      map.put(nums[i], i);
    }
    **return null**;
  }
```

调用 main 方法中的函数:

```
**public static void** main(String [] args) {
    **int** testCase1nums[] = {5, 7, 12,1};
    **int** testCase1Target = 12;

    **int** testCase2nums[] = {3, 5, 0};
    **int** testCase2Target = 6;

    **int** testCase3nums[] = {0, 3};
    **int** testCase3Target = 3;

    **int** testCase4nums[] = {0, -1, 2, -3, 1};
    **int** testCase4Target = -2;

    **int** testCase5nums[] = {4, -11, 22, -23, 31};
    **int** testCase5Target = -20;
    System.***out***.println(Arrays.*toString*(*findTwoSumSolution2*(testCase1nums,testCase1Target)));
    System.***out***.println(Arrays.*toString*(*findTwoSumSolution2*(testCase2nums,testCase2Target)));
    System.***out***.println(Arrays.*toString*(*findTwoSumSolution2*(testCase3nums,testCase3Target)));
    System.***out***.println(Arrays.*toString*(*findTwoSumSolution2*(testCase4nums,testCase4Target)));
    System.***out***.println(Arrays.*toString*(*findTwoSumSolution2*(testCase5nums,testCase5Target)));
  }
}
```

**时间复杂度:** O(n)
**空间复杂度:** O(n)

由于我们必须访问数组 nums[]的所有元素，这种方法的时间复杂度是 O(n)。最糟糕的情况是当所需的组合是 for 循环中要检查的最后一个组合时。

你可以在 [Github](https://gist.github.com/techisbeautiful/f25b0451d30fd7258cb964af05be4eb9) 看到这个解决方案的完整代码。

# 摘要

在这篇文章中，我分享了一个软件工程师编码面试中的常见问题，以及如何用残酷的力量和最优的解决方案来解决它。感谢您的宝贵时间！

如果你有任何挑战问题需要帮助，请在评论中告诉我。

# 参考

本文的问题来自 Leetcode。

如果你喜欢这个故事，你可以[关注](https://medium.com/@techisbeautiful)，[订阅我](https://medium.com/subscribe/@techisbeautiful)，这样你将是第一个收到我下一篇文章的人！

[你可以在这里](https://medium.com/@techisbeautiful/membership)成为媒介会员，拥有**无限访问**媒介平台上的每一个故事。如果你使用上面的链接，它也支持我，因为我有一个来自 Medium 的小佣金。谢谢大家的支持！