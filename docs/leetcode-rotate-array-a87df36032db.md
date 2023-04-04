# LeetCode —旋转阵列

> 原文：<https://blog.devgenius.io/leetcode-rotate-array-a87df36032db?source=collection_archive---------8----------------------->

# 问题陈述

给定一个数组，将数组向右旋转 **k** 步，其中 **k** 为非负。

**例一:**

```
Input: nums = [1, 2, 3, 4, 5, 6, 7], k = 3
Output: [5, 6, 7, 1, 2, 3, 4]

Explanation:
rotate 1 steps to the right: [7, 1, 2, 3, 4, 5, 6]
rotate 2 steps to the right: [6, 7, 1, 2, 3, 4, 5]
rotate 3 steps to the right: [5, 6, 7, 1, 2, 3, 4]
```

**例 2:**

```
Input: nums = [-1, -100, 3, 99], k = 2
Output: [3, 99, -1, -100]

Explanation:
rotate 1 steps to the right: [99, -1, -100, 3]
rotate 2 steps to the right: [3, 99, -1, -100]
```

**约束:**

```
- 1 <= nums.length <= 10^5 
- -2^31 <= nums[i] <= 2^31 
- 1 - 0 <= k <= 10^5
```

# 说明

## 强力解决方案

强力方法是创建一个临时(temp)数组，并将前 k 个元素存储在 temp 数组中。然后，我们将数组的其余部分移动 k 个位置，并将 tmp 数组追加回原始数组。

流程将如下所示:

```
Input nums[] = [1, 2, 3, 4, 5, 6, 7], k = 2

1) Store the first k elements in a temp array
   temp[] = [1, 2]

2) Shift rest of the nums[]
   nums[] = [3, 4, 5, 6, 7, 6, 7]

3) Store back the k elements
   nums[] = [3, 4, 5, 6, 7, 1, 2]
```

上述方法的时间复杂度为 **O(N)** ，空间复杂度为 **O(k)** 。

## 一个一个旋转

如果我们想避免使用额外的空间，我们可以一个接一个地旋转数组元素。

我们将数组的第一个元素存储在 temp 变量中，并将 nums[1]移位到 nums[0]，nums[2]移位到 nums[1]…直到最后一个元素 nums[n — 1]被移位到 nums[n — 2]。

我们重复上述程序 k 次。

上述方法的时间复杂度为 **O(N*k)** ，空间复杂度为 **O(1)** 。

## 反转数组

问题可以在 **O(N)** 时间内通过部分反转数组解决。我们将数组分为两部分，part1 = nums[0..k-1]和 part2 = nums[k..n — 1]。

我们反转 part1 数组中的元素。然后我们反转第二部分的数组元素。最后，我们反转整个数组。

让我们检查算法，看看这个解决方案是如何工作的。

```
// rotate(nums, k)
- set n = nums.size()

- return if n == 1 or k == 0

- update k = k % n
  This is done when k is greater than n.
  eg n = 8, k = 12
  k = k % n
    = 12 % 8
    = 4

- call reverseArray(nums, 0, n - 1)
- call reverseArray(nums, 0, k - 1)
- call reverseArray(nums, k, n - 1)

// reverseArray(nums, start, end)
- while start < end
  - swap(nums[start], nums[end])
  - start++
  - end--
```

让我们看看我们在 **C++** 、 **Golang** 和 **Javascript** 中的解决方案。

## C++解决方案

```
class Solution {
public:
    void reverseArray(vector<int>& nums, int start, int end) {
        while(start < end) {
            std::swap(nums[start], nums[end]);
            start++;
            end--;
        }
    }

    void rotate(vector<int>& nums, int k) {
        int n = nums.size();
        if(n == 1 || k == 0) {
            return;
        }

        k = k % n;
        reverseArray(nums, 0, n - 1);
        reverseArray(nums, 0, k - 1);
        reverseArray(nums, k, n - 1);
    }
};
```

## 戈朗溶液

```
func reverseArray(nums []int, start, end int) {
    for start < end {
        tmp := nums[start]
        nums[start] = nums[end]
        nums[end] = tmp
        start++
        end--
    }
}

func rotate(nums []int, k int)  {
    n := len(nums)

    if n == 1 || k == 0 {
        return
    }

    k = k % n

    reverseArray(nums, 0, n - 1)
    reverseArray(nums, 0, k - 1)
    reverseArray(nums, k, n - 1)
}
```

## Javascript 解决方案

```
var reverseArray = function(nums, start, end) {
    while(start < end) {
        let tmp = nums[start];
        nums[start] = nums[end];
        nums[end] = tmp;
        start++;
        end--;
    }
}

var rotate = function(nums, k) {
    let n = nums.length;

    if( n == 1 || k == 0 ) {
        return;
    }

    k = k % n;
    reverseArray(nums, 0, n - 1);
    reverseArray(nums, 0, k - 1);
    reverseArray(nums, k, n - 1);
};
```

让我们试运行一下我们的算法，看看解决方案是如何工作的。

```
Input: nums = [1, 2, 3, 4, 5, 6, 7]
       k = 3

// rotate function
Step 1: n = nums.size()
          = 7

Step 2: n == 1 || k == 0
        7 == 1 || 3 == 0
        false

Step 3: k = k % n
          = 3 % 7
          = 3

Step 4: reverseArray(nums, 0, n - 1)
        reverseArray(nums, 0, 6)

// reverseArray(nums, start, end)
Step 5: while start < end
        0 < 6
        true

        swap(nums[0], nums[6])
        nums = [7, 2, 3, 4, 5, 6, 1]

        start++
        start = 1
        end--
        end = 5

Step 6: while start < end
        1 < 5
        true

        swap(nums[1], nums[5])
        nums = [7, 6, 3, 4, 5, 2, 1]

        start++
        start = 2
        end--
        end = 4

Step 7: while start < end
        2 < 4
        true

        swap(nums[2], nums[4])
        nums = [7, 6, 5, 4, 3, 2, 1]

        start++
        start = 3
        end--
        end = 3

Step 8: while start < end
        3 < 3
        false

// rotate function
Step 9: reverseArray(nums, 0, k - 1)
        reverseArray(nums, 0, 2)

// reverseArray(nums, start, end)
Step 10: while start < end
         0 < 2
         true

         swap(nums[0], nums[2])
         nums = [5, 6, 7, 4, 3, 2, 1]

         start++
         start = 1
         end--
         end = 1

Step 11: while start < end
         1 < 1
         false

// rotate function
Step 12: reverseArray(nums, k, n - 1)
         reverseArray(nums, 3, 6)

// reverseArray(nums, start, end)
Step 13: while start < end
         3 < 6
         true

         swap(nums[3], nums[6])
         nums = [5, 6, 7, 1, 3, 2, 4]

         start++
         start = 4
         end--
         end = 5

Step 14: while start < end
         4 < 5
         true

         swap(nums[4], nums[5])
         nums = [5, 6, 7, 1, 2, 3, 4]

         start++
         start = 5
         end--
         end = 4

Step 15: while start < end
         5 < 4
         false

// rotate function
no more steps to execute

We return the final array as [5, 6, 7, 1, 2, 3, 4].
```

*原载于*[*https://alkeshghorpade . me*](https://alkeshghorpade.me/post/leetcode-rotate-array)*。*