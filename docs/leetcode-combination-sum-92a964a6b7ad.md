# LeetCode —组合和

> 原文：<https://blog.devgenius.io/leetcode-combination-sum-92a964a6b7ad?source=collection_archive---------7----------------------->

# 问题陈述

给定一个由**个不同的**个整数*个候选数*和一个目标整数*个目标数*组成的数组，返回所有*个候选数*的**个唯一组合**的列表，其中所选数字的总和为目标数。您可以以任何顺序返回**中的组合**。

同一个**号码可以从*候选号码*和**中无限次选择**。如果至少一个所选数字的频率不同，则两个组合是唯一的。**

**保证**对于给定的输入，总计为*目标*的唯一组合的数量少于 *150* 个组合。

**例 1:**

```
Input: candidates = [2, 3, 6, 7], target = 7
Output: [[2, 2, 3], [7]]
Explanation:
2 and 3 are candidates, and 2 + 2 + 3 = 7\. Note that 2 can be used multiple times.
7 is a candidate, and 7 = 7.
These are the only two combinations.
```

**例二:**

```
Input: candidates = [2, 3, 5], target = 8
Output: [[2, 2, 2, 2], [2, 3, 3], [3, 5]]
```

**例 3:**

```
Input: candidates = [2], target = 1
Output: []
```

**约束:**

```
- 1 <= candidates.length <= 30
- 1 <= candidates[i] <= 200
- All elements of candidates are distinct.
- 1 <= target <= 500
```

# 说明

## 强力方法

强力方法是生成所有组合，并验证数组中的元素总和是否达到目标。

但上述程序的时间复杂度将为 **O(N )** 。

## 追踪

这类问题可以使用回溯来解决。我们编写了一个递归函数，其中我们一直将数组元素附加到一个临时数组(称为 current ),并一直跟踪这个临时数组中元素的总和。在递归函数中，我们保留了两种基本情况。

1.  第一个基本情况是检查临时数组中元素的总和是否等于目标值。如果是，我们返回并将临时数组附加到最终结果中。
2.  第二个基本情况是检查总和是否超过我们刚刚返回的目标元素。

让我们检查一下算法，以便清楚地理解上面的方法。

```
- initialize the result as a 2D array
  initialize current as an array

  // n = index, at start it will be 0.
  // sumTillNow = sum of the current elements in the array, at the start it will be 0
  // current = current list of elements in the array, at the start it will be an empty array []
- call combinationSumUtil(result, candidates, n, target, sumTillNow, current)

- return result

// combinationSumUtil function
- if sumTillNow == target
  // append current to result
  - result.push_back(current)

- if sumTillNow > target
  - return

- loop for i = n; i <= candidates.size() - 1; i++
  // append candidates array ith element to the current array
  - current.push_back(candidates[i])

  - sumTillNow += candidates[i]

  // call the function recursively
  - combinationSumUtil(result, candidates, i, target, sumTillNow, current)

  - sumTillNow -= current[current.size() - 1]

  // remove the last element from the array
  - current.pop_back()
```

让我们来看看我们在 **C++** 、 **Golang** 和 **Javascript** 中的解决方案。

## C++解决方案

```
class Solution {
public:
    void combinationSumUtil(vector<vector<int>>& result, vector<int>& candidates, int n, int target, int sumTillNow, vector<int>& current) {
        if(sumTillNow == target) {
            result.push_back(current);
        }

        if(sumTillNow > target) {
            return;
        }

        for(int i = n; i <= candidates.size() - 1; i++) {
            current.push_back(candidates[i]);
            sumTillNow += candidates[i];

            combinationSumUtil(result, candidates, i, target, sumTillNow, current);

            sumTillNow -= current[current.size() - 1];
            current.pop_back();
        }
    }

    vector<vector<int>> combinationSum(vector<int>& candidates, int target) {
        vector<vector<int>> result;
        vector<int> current;

        combinationSumUtil(result, candidates, 0, target, 0, current);
        return result;
    }
};
```

## 戈朗溶液

```
func combinationSumUtil(result *[][]int, candidates []int, n, target, sumTillNow int, current []int) {
    if sumTillNow == target {
        *result = append(*result, append([]int{}, current...))
        return
    }

    if sumTillNow > target {
        return
    }

    for i := n; i <= len(candidates) - 1; i++ {
        current = append(current, candidates[i])
        sumTillNow = sumTillNow + candidates[i]

        combinationSumUtil(result, candidates, i, target, sumTillNow, current)

        sumTillNow = sumTillNow - current[len(current) - 1]
        current = current[:len(current) - 1]
    }
}

func combinationSum(candidates []int, target int) [][]int {
    result := make([][]int, 0)

    combinationSumUtil(&result, candidates, 0, target, 0, []int{})

    return result
}
```

## Javascript 解决方案

```
var combinationSum = function(candidates, target) {
    let result = [];

    const combinationSumUtil = (candidates, n, target, sumTillNow, current) => {
        if(sumTillNow === target) {
            result.push([...current]);
        }

        if(sumTillNow > target) {
            return;
        }

        for(let i = n; i <= candidates.length - 1; i++){
            current.push(candidates[i]);
            sumTillNow += candidates[i];

            combinationSumUtil(candidates, i, target, sumTillNow, current);

            sumTillNow -= current[current.length - 1];
            current.pop();
        }
    }

    combinationSumUtil(candidates, 0, target, 0, []);

    return result;
};
```

让我们试运行一下我们的算法。

```
Input: candidates = [2, 3, 6, 7]
       target = 7

// combinationSum function
Step 1: vector<vector<int>> result
        vector<int> current

Step 2: combinationSumUtil(result, candidates, 0, target, 0, current)

// combinationSumUtil function
Step 3: if sumTillNow == target
           0 == 7
           false

        if sumTillNow > target
           0 > 7
           false

        loop for int i = n; i <= candidates.size() - 1
             i = 0
             i <= 4 - 1
             0 <= 3
             true

             current.push_back(candidates[i])
             current.push_back(candidates[0])
             current.push_back(2)
             current = [2]

             sumTillNow += candidates[i]
                         = sumTillNow + candidates[0]
                         = 0 + 2
                         = 2

             combinationSumUtil(result, candidates, i, target, sumTillNow, current)
             combinationSumUtil([][], [2, 3, 6, 7], 0, 7, 2, [2])

Step 4: if sumTillNow == target
           2 == 7
           false

        if sumTillNow > target
           2 > 7
           false

        loop for int i = n; i <= candidates.size() - 1
             i = 0
             i <= 4 - 1
             0 <= 3
             true

             current.push_back(candidates[i])
             current.push_back(candidates[0])
             current.push_back(2)
             current = [2, 2]

             sumTillNow += candidates[i]
                         = sumTillNow + candidates[0]
                         = 2 + 2
                         = 4

             combinationSumUtil(result, candidates, i, target, sumTillNow, current)
             combinationSumUtil([][], [2, 3, 6, 7], 0, 7, 4, [2, 2])

Step 5: if sumTillNow == target
           4 == 7
           false

        if sumTillNow > target
           4 > 7
           false

        loop for int i = n; i <= candidates.size() - 1
             i = 0
             i <= 4 - 1
             0 <= 3
             true

             current.push_back(candidates[i])
             current.push_back(candidates[0])
             current.push_back(2)
             current = [2, 2, 2]

             sumTillNow += candidates[i]
                         = sumTillNow + candidates[0]
                         = 4 + 2
                         = 6

             combinationSumUtil(result, candidates, i, target, sumTillNow, current)
             combinationSumUtil([][], [2, 3, 6, 7], 0, 7, 6, [2, 2, 2])

Step 6: if sumTillNow == target
           6 == 7
           false

        if sumTillNow > target
           6 > 7
           false

        loop for int i = n; i <= candidates.size() - 1
             i = 0
             i <= 4 - 1
             0 <= 3
             true

             current.push_back(candidates[i])
             current.push_back(candidates[0])
             current.push_back(2)
             current = [2, 2, 2, 2]

             sumTillNow += candidates[i]
                         = sumTillNow + candidates[0]
                         = 6 + 2
                         = 8

             combinationSumUtil(result, candidates, i, target, sumTillNow, current)
             combinationSumUtil([][], [2, 3, 6, 7], 0, 7, 8, [2, 2, 2, 2])

Step 7: if sumTillNow == target
           8 == 7
           false

        if sumTillNow > target
           8 > 7
           true
           return

           we backtrack to step 6 and continue

Step 8: combinationSumUtil([][], [2, 3, 6, 7], 0, 7, 8, [2, 2, 2, 2])
        sumTillNow = sumTillNow - current[len(current) - 1]
                   = 8 - current[4 - 1]
                   = 8 - current[3]
                   = 8 - 2
                   = 6

        current.pop_back()
        current = [2, 2, 2]

        i++
        i = 1

        i = 1
        i <= 4 - 1
        1 <= 3
        true

        current.push_back(candidates[i])
        current.push_back(candidates[1])
        current.push_back(3)
        current = [2, 2, 2, 3]

        sumTillNow += candidates[i]
                    = 6 + candidates[1]
                    = 6 + 3
                    = 9

        combinationSumUtil(result, candidates, i, target, sumTillNow, current)
        combinationSumUtil([][], [2, 3, 6, 7], 0, 7, 9, [2, 2, 2, 3])

Step 9: if sumTillNow == target
           9 == 7
           false

        if sumTillNow > target
           9 > 7
           true
           return

           we backtrack to step 8 and continue

Step 10:combinationSumUtil([][], [2, 3, 6, 7], 0, 7, 9, [2, 2, 2, 3])
        sumTillNow = sumTillNow - current[len(current) - 1]
                   = 9 - current[4 - 1]
                   = 9 - current[3]
                   = 9 - 3
                   = 6

        current.pop_back()
        current = [2, 2, 2]

        i++
        i = 2

        i = 2
        i <= 4 - 1
        2 <= 3
        true

        current.push_back(candidates[i])
        current.push_back(candidates[2])
        current.push_back(6)
        current = [2, 2, 2, 6]

        sumTillNow += candidates[i]
                    = 6 + candidates[2]
                    = 6 + 6
                    = 12

        combinationSumUtil(result, candidates, i, target, sumTillNow, current)
        combinationSumUtil([][], [2, 3, 6, 7], 0, 7, 12, [2, 2, 2, 6])

Step 11: if sumTillNow == target
           12 == 7
           false

         if sumTillNow > target
           12 > 7
           true
           return

           we backtrack to step 10 and continue

Step 12:combinationSumUtil([][], [2, 3, 6, 7], 0, 7, 9, [2, 2, 2, 3])
        sumTillNow = sumTillNow - current[len(current) - 1]
                   = 12 - current[4 - 1]
                   = 12 - current[3]
                   = 12 - 6
                   = 6

        current.pop_back()
        current = [2, 2, 2]

        i++
        i = 3

        i = 3
        i <= 4 - 1
        3 <= 3
        true

        current.push_back(candidates[i])
        current.push_back(candidates[3])
        current.push_back(7)
        current = [2, 2, 2, 7]

        sumTillNow += candidates[i]
                    = 6 + candidates[3]
                    = 6 + 7
                    = 13

        combinationSumUtil(result, candidates, i, target, sumTillNow, current)
        combinationSumUtil([][], [2, 3, 6, 7], 0, 7, 13, [2, 2, 2, 7])

Step 13: if sumTillNow == target
           13 == 7
           false

         if sumTillNow > target
           13 > 7
           true
           return

           we backtrack to step 12 and continue

Step 14:combinationSumUtil([][], [2, 3, 6, 7], 0, 7, 9, [2, 2, 2, 3])
        sumTillNow = sumTillNow - current[len(current) - 1]
                   = 13 - current[4 - 1]
                   = 13 - current[3]
                   = 13 - 7
                   = 6

        current.pop_back()
        current = [2, 2, 2]

        i++
        i = 4

        i = 3
        i <= 4 - 1
        4 <= 3
        false

        We return to Step 5 directly

Step 15:combinationSumUtil([][], [2, 3, 6, 7], 0, 7, 6, [2, 2, 2])
        sumTillNow = sumTillNow - current[len(current) - 1]
                   = 6 - current[3 - 1]
                   = 6 - current[2]
                   = 6 - 2
                   = 4

        current.pop_back()
        current = [2, 2]

        i++
        i = 1

        i = 1
        i <= 4 - 1
        1 <= 3
        true

        current.push_back(candidates[i])
        current.push_back(candidates[1])
        current.push_back(3)
        current = [2, 2, 3]

        sumTillNow += candidates[i]
                    = 4 + candidates[1]
                    = 4 + 3
                    = 7

        combinationSumUtil(result, candidates, i, target, sumTillNow, current)
        combinationSumUtil([][], [2, 3, 6, 7], 0, 7, 7, [2, 2, 3])

Step 16:if sumTillNow == target
           7 == 7
           true

           result.push_back(current)
           result.push_back([2, 2, 3])

           result = [[2, 2, 3]]

Similarly, we iterate over all other elements and get the result as
[[2, 2, 3], [7]]
```

*原载于*[*https://alkeshghorpade . me*](https://alkeshghorpade.me/post/leetcode-combination-sum)*。*