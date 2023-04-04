# DP 基本问题第 1 部分

> 原文：<https://blog.devgenius.io/dp-basic-problems-part-1-5d21e1b2c213?source=collection_archive---------3----------------------->

![](img/f8482768a7ff1f8f169ba2899f1adb4b.png)

[来源](https://unsplash.com/photos/hHL08lF7Ikc)

# (1)[0–1 背包问题](https://practice.geeksforgeeks.org/problems/0-1-knapsack-problem0945/1)

给你 **N** 件物品的重量和价值，将这些物品放入一个容量为 **W** 的背包中，以获得背包中最大的总价值。请注意，每种商品我们只有**一个数量。
换句话说，给定两个整数数组 **val[0..N-1]** 和 **wt[0..N-1]** ，分别代表与 **N** 项相关联的值和权重。同样给定一个表示背包容量的整数 W，找出 **val[]** 的最大值子集，使得这个子集的权重之和小于等于 **W.** 不能分解一个物品，**要么选择完整的物品，要么不选择(0–1 属性)**。**

![](img/82157a876d38d5299c538fb66ef7ec19.png)

***解决方案:***

![](img/48e8cb0f70300d240ef9772d827cae12.png)

# [(2)计算到达第 N 级楼梯的方法](https://www.codingninjas.com/codestudio/problem-details/count-ways-to-reach-nth-stairs_798650)

你已经有了几级楼梯。最初，你在第 0 级楼梯，你需要到达第 n 级楼梯。每次你要么爬一步，要么爬两步。你应该返回从第 0 步爬到第 n 步的不同方式的数量。

**例如:**

```
N=3
```

![](img/dabb333b3ea26f5f0475a79a1b472553.png)

```
We can climb one step at a time i.e. {(0, 1) ,(1, 2),(2,3)} or we can climb the first two-step and then one step i.e. {(0,2),(1, 3)} or we can climb first one step and then two step i.e. {(0,1), (1,3)}.
```

![](img/cb2a8d2e78a1b91d2a34c013e0fc5979.png)

解决方案:

![](img/fe31f6ea5ea774994ab81dc7804cab1e.png)

# [(3)蛙跳](https://www.codingninjas.com/codestudio/problem-details/frog-jump_3621012)

N 级长楼梯的第一级台阶上有一只青蛙。青蛙想到达第 n 级楼梯。HEIGHT[i]是第(i+1)级楼梯的高度。如果青蛙从第 I 级跳到第 j 级楼梯，那么跳跃中损失的能量由|HEIGHT[i-1] — HEIGHT[j-1] |给出。青蛙在第 I 层楼梯上，他可以跳到第(i+1)层或第(i+2)层。你的任务是找出青蛙从第一级楼梯到达第 n 级楼梯所用的最小总能量。

**例如**

```
If the given ‘HEIGHT’ array is [10,20,30,10], the answer 20 as the frog can jump from 1st stair to 2nd stair (|20-10| = 10 energy lost) and then a jump from 2nd stair to last stair (|10-20| = 10 energy lost). So, the total energy lost is 20.
```

![](img/8142802a2eb8dd080b3bff68c2c1e1d7.png)

```
Recurrence Relation:f(n)if n==0:return 0left = f(n-1) + abs(arr[n]-arr[n-1])if n>1: 
      right = f(n-2) + abs([arr[n]-arr[n-2])return min(left,right)
```

解决方案:

![](img/b1451361df204fe9b750a0e7a0dc9a3d.png)

***空间优化代码***

![](img/a07672d4eb6f765f0b86f9053fece177.png)![](img/322117ed211758e76a3e085658ebe422.png)

# [(4)粘人小偷](https://practice.geeksforgeeks.org/problems/stickler-theif-1587115621/1?utm_source=gfg&utm_medium=article&utm_campaign=bottom_sticky_on_article)

小偷想从一个拥有排成一行的房子的社会中抢劫钱财。他是一个古怪的人，在抢劫房屋时遵循一定的规则。按照规则，他将**永远不会连续打劫两栋房子**。同时，他希望**最大化**他**抢劫**的金额。小偷知道哪所房子里有多少钱，但却想不出一个最佳的抢劫策略。他请求你帮助他找到最大的钱，如果他严格遵守规则的话。每家都有一定数量的钱。

```
**Input:** n = 6
a[] = {5,5,10,100,10,5}
**Output:** 110
**Explanation:** 5+100+5=110
```

![](img/c4a49494aee84bbb65d7db852dcc5b97.png)

解决方案:

![](img/457712f4bb34d71081a203e2771ea223.png)

制表方法:

![](img/4c8d31be2cec934d63e403094a9db4d5.png)

空间优化代码

![](img/e069ce711810cb634cc942caddafca30.png)![](img/22f7a7fd183ff499a0b05400dd3ddf9d.png)

# [**(5)入室抢劫者 II**](https://leetcode.com/problems/house-robber-ii/)

你是一个专业的强盗，计划沿街抢劫房屋。每所房子都藏了一定数量的钱。这个地方所有的房子都排成一圈。这意味着第一栋房子是最后一栋的邻居。同时，相邻的房子都有一个安全系统连接，如果两个相邻的房子在同一个晚上被闯入**它会自动联系警察。**

给定一个整数数组`nums`代表每套房子的钱数，返回*你今晚可以抢劫的最大金额* ***而不报警*** 。

```
**Input:** nums = [2,3,2]
**Output:** 3
**Explanation:** You cannot rob house 1 (money = 2) and then rob house 3 (money = 2), because they are adjacent houses.
```

![](img/5b5dbc70a604a2e2c10b16ed65cdefc2.png)

解决方案:

![](img/e1f65ded91142ed1e428f8b145a95855.png)

# [(6)忍者的训练](https://www.codingninjas.com/codestudio/problems/ninja-s-training_3621003)

![](img/5c7a85aa8ed1e7d1b643617451e5ac2c.png)![](img/7189d72c7487b4e25291bcd0694f8e51.png)![](img/69bc2ecf45dfdadca59f0dd1b29f7203.png)

解决方案:

![](img/64d32e7b66a77d8ab45a1ad67d6a82b9.png)

# [(7)独特路径](https://leetcode.com/problems/unique-paths/)

一个`m x n`网格上有一个机器人。机器人初始位于**左上角**(即`grid[0][0]`)。机器人试图移动到**右下角**(即`grid[m - 1][n - 1]`)。机器人在任何时候只能向下或向右移动。

给定两个整数`m`和`n`，返回*机器人可以到达右下角*的可能唯一路径的数量。

测试用例的生成使得答案将小于或等于`2 * 109`。

![](img/30f3382bc68685ed917a640f0c90a0e0.png)

解决方案:

![](img/9cb06e425d3c80a93a196c7e81f062a5.png)

# [(8)独特的路径二](https://leetcode.com/problems/unique-paths-ii/)

给你一个`m x n`整数数组`grid`。有一个机器人最初位于**左上角**(即`grid[0][0]`)。机器人试图移动到**右下角**(即`grid[m-1][n-1]`)。机器人在任何时候只能向下或向右移动。

在`grid`中，障碍物和空间分别标记为`1`或`0`。机器人走的路径不能包含任何作为障碍物的正方形。

返回*机器人到达右下角*可能采用的唯一路径的数量。

测试用例的生成使得答案将小于或等于`2 * 109`。

![](img/8661d48c709a7cf4c7aa1ccf9ffe21da.png)

```
**Input:** obstacleGrid = [[0,0,0],[0,1,0],[0,0,0]]
**Output:** 2
**Explanation:** There is one obstacle in the middle of the 3x3 grid above.
There are two ways to reach the bottom-right corner:
1\. Right -> Right -> Down -> Down
2\. Down -> Down -> Right -> Right
```

![](img/e7e8584cf433697b73b5d7414d8fa651.png)![](img/c36d94f1d22cb990b11907b6674abbd7.png)

解决方案:

![](img/fc51d76a5aac0b847f30e4aa22010d6b.png)

# [(9)最小路径和](https://leetcode.com/problems/minimum-path-sum/)

给定一个填充了非负数的`m x n` `grid`，找出一条从左上到右下的路径，使沿其路径的所有数字之和最小。

**注意:**在任何时间点，您只能向下或向右移动。

```
**Input:** grid = [[1,3,1],[1,5,1],[4,2,1]]
**Output:** 7
**Explanation:** Because the path 1 → 3 → 1 → 1 → 1 minimizes the sum.
```

![](img/6791433c6783318b9487f82fc5056fee.png)

解决方案:

![](img/7874e1405081ef9733f3106fd0b2d9ff.png)

# [***【10】目标总和***](https://leetcode.com/problems/target-sum/)

给你一个整数数组`nums`和一个整数`target`。

您希望通过在 nums 中的每个整数前添加符号`'+'`和`'-'`中的一个来构建 nums 的**表达式**，然后连接所有的整数。

*   比如说`nums = [2, 1]`，你可以在`2`前加一个`'+'`，在`1`前加一个`'-'`，串联起来构建表达式`"+2-1"`。

返回您可以构建的不同**表达式**的数量，其计算结果为`target`。

```
**Input:** nums = [1,1,1,1,1], target = 3
**Output:** 5
**Explanation:** There are 5 ways to assign symbols to make the sum of nums be target 3.
-1 + 1 + 1 + 1 + 1 = 3
+1 - 1 + 1 + 1 + 1 = 3
+1 + 1 - 1 + 1 + 1 = 3
+1 + 1 + 1 - 1 + 1 = 3
+1 + 1 + 1 + 1 - 1 = 3
```

![](img/7e0c4bda285905c2c61c8bd18ab35738.png)

***解决方案:***

![](img/78f731f9a1f9a287a519b666a8cde9e8.png)

查看类似的博客:

[](/10-daily-practice-problems-day-12-619934ae6e98) [## 10 个日常练习题~第 12 天

### 1.除自身以外的数组乘积

blog.devgenius.io](/10-daily-practice-problems-day-12-619934ae6e98) [](https://medium.com/@Mr.DataScientist/stacks-problems-2cac2f450422) [## 堆栈问题…！！！！

### 让我们来解决一些著名的问题，这些问题可以通过堆栈轻松解决。

medium.com](https://medium.com/@Mr.DataScientist/stacks-problems-2cac2f450422) [](/10-daily-practice-problems-day-8-c303db2bd1ef) [## 10 个日常练习题~第 8 天

### 1.同一棵树

blog.devgenius.io](/10-daily-practice-problems-day-8-c303db2bd1ef) [](https://medium.com/@Mr.DataScientist/subarray-problems-d515e1f0fbbf) [## 子阵列问题

### 1.最大乘积子阵列

medium.com](https://medium.com/@Mr.DataScientist/subarray-problems-d515e1f0fbbf) [](https://medium.com/@Mr.DataScientist/10-daily-practice-problems-day-3-6470449e5cd5) [## 10 个日常练习题~第三天

### 1.K-组中的反向节点

medium.com](https://medium.com/@Mr.DataScientist/10-daily-practice-problems-day-3-6470449e5cd5) 

# ***参考文献:***

[](https://www.youtube.com/c/NeetCode) [## 尼特码

### 以前的 Neet 和现在的 SWE @ Google，也是我热爱教学！(不在教育、就业或培训领域)…

www.youtube.com](https://www.youtube.com/c/NeetCode)