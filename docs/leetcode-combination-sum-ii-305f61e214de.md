# LeetCode —组合和 II

> 原文：<https://blog.devgenius.io/leetcode-combination-sum-ii-305f61e214de?source=collection_archive---------16----------------------->

# 问题陈述

给定一组候选号码*(候选号码)*和一个目标号码*(目标号码)*，找出*候选号码*中所有唯一的组合，其中候选号码总和为*目标号码*。

*候选*中的每个数字在组合中只能使用**一次**。

**注意**:解决方案集合不能包含重复的组合。

**例 1:**

```
Input: candidates = [10, 1, 2, 7, 6, 1, 5], target = 8
Output:
[
[1, 1, 6],
[1, 2, 5],
[1, 7],
[2, 6]
]
```

**例 2:**

```
Input: candidates = [2, 5, 2, 1, 2], target = 5
Output:
[
[1, 2, 2],
[5]
]
```

**约束:**

```
- 1 <= candidates.length <= 100
- 1 <= candidates[i] <= 50
- 1 <= target <= 30
```

# 说明

## 追踪

这个问题可以用我们在上一篇博客[组合求和](https://alkeshghorpade.me/post/leetcode-combination-sum)中使用的类似方法来解决。唯一不同的是*候选*中的每个元素只能用**一次**。每个元素只使用一次，可以通过递归调用函数时跳到下一个索引来实现。

另一个区别是，我们的解决方案集不能包含重复的组合。问题陈述没有提到数组是排序的。为了避免重复的结果集，我们需要对数组进行排序，并避免使用相同的元素。

我们先检查一下算法。

```
- initialize the result as a 2D array
  initialize current as an array // n = index, at start it will be 0.
  // sumTillNow = sum of the current elements in the array, at the start it will be 0
  // current = current list of elements in the array, at the start it will be an empty array []
- call combinationSum2Util(result, candidates, current, n, sumTillNow, target)- return result// combinationSum2Util function
- if sumTillNow == target
  // append current to result
  - result.push_back(current)- if sumTillNow > target
  - return- set prev = -1- loop for i = n; i <= candidates.size() - 1; i++
  - if prev != candidates[i]
    // append candidates array ith element to the current array
    - current.push_back(candidates[i]) - sumTillNow += candidates[i] // call the function recursively
    - combinationSumUtil(result, candidates, i, target, sumTillNow, current) - sumTillNow -= current[current.size() - 1] // remove the last element from the array
    - current.pop_back() - prev = candidates[i]
  - end of if
```

让我们来看看我们在 **C++** 、 **Golang** 和 **Javascript** 中的解决方案。

## C++解决方案

```
class Solution {
public:
    void combinationSum2Util(vector<vector<int>> &result, vector<int>& candidates, vector<int>& current, int index, int sumTillNow, int target) {
        if(sumTillNow == target) {
            result.push_back(current);
            return;
        } if(sumTillNow > target) {
            return;
        } int prev = -1; for(int i = index; i <= candidates.size() - 1; i++) {
            if(prev != candidates[i]) {
                current.push_back(candidates[i]);
                sumTillNow += candidates[i]; combinationSum2Util(result, candidates, current, i + 1, sumTillNow, target);
                sumTillNow -= current[current.size() - 1]; current.pop_back();
                prev = candidates[i];
            }
        }
    } vector<vector<int>> combinationSum2(vector<int>& candidates, int target) {
        vector<vector<int>> result;
        vector<int> current; sort(candidates.begin(), candidates.end());
        combinationSum2Util(result, candidates, current, 0, 0, target);
        return result;
    }
};
```

## 戈朗溶液

```
func combinationSum2Util(result *[][]int, candidates, current []int, index, sumTillNow, target int) {
    if sumTillNow == target {
        *result = append(*result, append([]int{}, current...))
        return
    } if sumTillNow > target {
        return
    } prev := -1 for i := index; i < len(candidates); i++ {
        if prev != candidates[i] {
            current = append(current, candidates[i])
            sumTillNow += candidates[i] combinationSum2Util(result, candidates, current, i + 1, sumTillNow, target)
            sumTillNow -= current[len(current) - 1]
            current = current[:len(current) - 1]
            prev = candidates[i]
        }
    }
}func combinationSum2(candidates []int, target int) [][]int {
    result := make([][]int, 0) sort.Ints(candidates)
    combinationSum2Util(&result, candidates, []int{}, 0, 0, target) return result
}
```

## Javascript 解决方案

```
var combinationSum2 = function(candidates, target) {
    let result = [];
    candidates.sort(function(a, b){ return a - b }); const combinationSum2Util = (candidates, current, index, sumTillNow, target) => {
        if( sumTillNow == target ) {
            result.push([...current]);
            return;
        } if( sumTillNow > target ) {
           return;
        } let prev = -1; for(let i = index; i < candidates.length; i++) {
            if(prev != candidates[i]) {
                current.push(candidates[i]);
                sumTillNow += candidates[i]; combinationSum2Util(candidates, current, i + 1, sumTillNow, target); sumTillNow -= current[current.length - 1];
                current.pop(); prev = candidates[i];
            }
        }
    } combinationSum2Util(candidates, [], 0, 0, target); return result;
};
```

让我们试运行一下我们的算法。

```
Input: candidates = [2, 5, 2, 1, 2]
       target = 5// combinationSum function
Step 1: vector<vector<int>> result
        vector<int> currentStep 2: sort(candidates.begin(), candidates.end()
        [1, 2, 2, 2, 5]Step 3: combinationSum2Util(result, candidates, current, 0, 0, target)// combinationSum2Util function
Step 4: if sumTillNow == target
           0 == 5
           false if sumTillNow > target
           0 > 5
           false prev = -1 loop for int i = index; i <= candidates.size() - 1
          i = 0
          0 <= 5 - 1
          true - if prev != candidates[i]
               -1 != 1
               true current.push_back(candidates[i])
            current.push_back(candidates[0])
            current.push_back(1)
            current = [1] sumTillNow += candidates[i]
                        = sumTillNow + candidates[0]
                        = 0 + 1
                        = 1 combinationSum2Util(result, candidates, current, i + 1, sumTillNow, target)
            combinationSum2Util([][], [1, 2, 2, 2, 5], [1], 1, 1, 5)Step 5: if sumTillNow == target
           1 == 5
           false if sumTillNow > target
           1 > 5
           false prev = -1 loop for int i = index; i <= candidates.size() - 1
          i = 1
          1 <= 5 - 1
          true - if prev != candidates[i]
               -1 != 2
               true current.push_back(candidates[i])
            current.push_back(candidates[1])
            current.push_back(2)
            current = [1, 2] sumTillNow += candidates[i]
                        = sumTillNow + candidates[1]
                        = 1 + 2
                        = 3 combinationSum2Util(result, candidates, current, i + 1, sumTillNow, target)
            combinationSum2Util([][], [1, 2, 2, 2, 5], [1, 2], 3, 3, 5)Step 6: if sumTillNow == target
           3 == 5
           false if sumTillNow > target
           3 > 5
           false prev = -1 loop for int i = index; i <= candidates.size() - 1
          i = 2
          2 <= 5 - 1
          true - if prev != candidates[i]
               -1 != 2
               true current.push_back(candidates[i])
            current.push_back(candidates[1])
            current.push_back(2)
            current = [1, 2, 2] sumTillNow += candidates[i]
                        = sumTillNow + candidates[1]
                        = 3 + 2
                        = 5 combinationSum2Util(result, candidates, current, i + 1, sumTillNow, target)
            combinationSum2Util([][], [1, 2, 2, 2, 5], [1, 2, 2], 3, 5, 5)Step 7: if sumTillNow == target
           5 == 5
           true result.push_back(current)
           result = [[1, 2, 2]] return we backtrack to step 6 and continueStep 8: combinationSumUtil([][], [1, 2, 2, 2, 5], [1, 2, 2], 3, 5, 5)
        sumTillNow = sumTillNow - current[current.size() - 1]
                   = 5 - current[3 - 1]
                   = 5 - current[2]
                   = 5 - 2
                   = 3 current.pop_back()
        current = [1, 2] prev = candidates[i]
             = candidates[2]
             = 2 i++
        i = 3 loop for int i = index; i <= candidates.size() - 1
          i = 3
          3 <= 5 - 1
          true - if prev != candidates[3]
               2 != 2
               false i++
        i = 4 loop for int i = index; i <= candidates.size() - 1
          i = 3
          4 <= 5 - 1
          true - if prev != candidates[4]
               2 != 5
               true current.push_back(candidates[i])
            current.push_back(candidates[4])
            current.push_back(5)
            current = [1, 2, 5] sumTillNow += candidates[i]
                        = sumTillNow + candidates[4]
                        = 3 + 5
                        = 8 combinationSum2Util(result, candidates, current, i + 1, sumTillNow, target)
            combinationSum2Util([][], [1, 2, 2, 2, 5], [5], 5, 8, 5)Step 9: if sumTillNow == target
           8 == 5
           false if sumTillNow > target
           8 > 5
           true
           return we backtrack to step 5 and continueStep 10: combinationSum2Util([][], [1, 2, 2, 2, 5], [1, 2], 3, 3, 5)
         sumTillNow = sumTillNow - current[current.size() - 1]
                   = 3 - current[2 - 1]
                   = 3 - current[1]
                   = 3 - 2
                   = 1 current.pop_back()
         current = [1] prev = candidates[i]
              = candidates[1]
              = 2 i++
         i = 2 loop for int i = index; i <= candidates.size() - 1
          i = 2
          2 <= 5 - 1
          true - if prev != candidates[3]
               2 != 2
               false i++
         i = 3 loop for int i = index; i <= candidates.size() - 1
          i = 3
          3 <= 5 - 1
          true - if prev != candidates[3]
               2 != 2
               false i++
         i = 4 loop for int i = index; i <= candidates.size() - 1
          i = 4
          4 <= 5 - 1
          true - if prev != candidates[4]
               2 != 5
               true current.push_back(candidates[i])
            current.push_back(candidates[4])
            current.push_back(5)
            current = [1, 5] sumTillNow += candidates[i]
                        = sumTillNow + candidates[4]
                        = 1 + 5
                        = 6 combinationSum2Util(result, candidates, current, i + 1, sumTillNow, target)
            combinationSum2Util([][], [1, 2, 2, 2, 5], [1, 5], 5, 6, 5)Step 11: if sumTillNow == target
           6 == 5
           false if sumTillNow > target
            6 > 5
            true
            return we backtrack to step 4 and continueWe similarly iterate from index 2, backtrack, and then reach the last index.We return the answer as
[
  [1, 2, 2],
  [5]
]
```

*原载于*[*https://alkeshghorpade . me*](https://alkeshghorpade.me/post/leetcode-combination-sum-ii)*。*