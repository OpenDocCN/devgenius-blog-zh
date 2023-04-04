# 单调堆栈—算法模式

> 原文：<https://blog.devgenius.io/monotonic-stack-algorithm-pattern-7bfac59157c2?source=collection_archive---------3----------------------->

![](img/f1a1648765e4cf1c3c507704fdc848af.png)

图片来源 google.com

幸福的秘密在于找到相投的单调。

单调堆栈是堆栈数据结构的一种变体，其中的元素总是以特定的顺序排序。关键的区别仅在于推送操作:在我们将一个新元素推送到堆栈上之前，我们首先验证是否保持了单调性。如果是，那么我们从堆栈中弹出顶部的元素，直到推新元素不再打破单调性。

对于单调增加的**堆栈:**

*   我们按顺序遍历数组
*   在推动之前，我们继续弹出更大的元素。
*   因为我们不断弹出更大的元素，保留更小的元素，所以它会给我们一个尽可能小的结果。

> 当一个元素从单调堆栈中弹出时，它将不再被使用。

对于单调递减的**堆栈:**

*   我们以相反的顺序遍历数组
*   我们继续弹出**更小的**元素。
*   因为我们不断弹出较小的元素，保留较大的元素，所以它会给我们一个在字典上尽可能大于的结果。

```
def **monotonic_increasing_stack**(nums):
    n = len(nums)
    stack = []
    for i in range(n):
        while len(stack) > 0 and stack[-1] >= nums[i]:
            stack.pop()
        stack.append(nums[i])
    return stack# flip the array. 
def **monotonic_decreasing_stack**(nums):
    n = len(nums)
    stack = []
    for i in range(n):
        while len(stack) > 0 and stack[-1] < nums[i]:
            stack.pop()
        stack.append(nums[i])
    return stack

nums = [2, 3, 7, 11, 5, 17, 19]
print(monotonic_increasing_stack(nums)) 
# [2, 3, 5, 17, 19]
print(monotonic_decreasing_stack(nums))
# [19]
```

单调堆栈为数组问题中的多个 ***范围查询提供了最佳的时间复杂度解决方案。更准确地说，单调堆栈帮助我们维护范围内的最大和最小元素，并保持范围内元素的顺序。我们只根据添加的最新元素来更新堆栈。因为数组中的每个元素只能进入单调堆栈一次，所以时间复杂度为 O(N)。***

## 可以看到海景的建筑

一条线上有`n`栋建筑。给你一个大小为`n`的整数数组`heights`,它代表该行中建筑物的高度。

海洋在建筑的右边。如果一个建筑可以无障碍地看到大海，那么这个建筑就是海景。从形式上来说，如果一栋建筑右边的所有建筑都有一个更小的高度，那么它就有海景。

返回有海景的建筑的索引列表 **(0 索引)**，按升序排序。

在这个问题中，我们可以使用单调递减的堆栈。

```
def find_buildings(heights):
    n = len(heights)
    stack = []
    for i in range(n):
        curr_height = heights[i]
        while stack and heights[stack[-1]] <= curr_height:
            stack.pop()
        stack.append(i)
    return stack
```

## 删除重复的字母

给定一个字符串`s`，删除重复的字母，使每个字母出现一次，并且只出现一次。你必须确保你的结果是所有可能结果中按字典顺序最小的。

我们使用单调递增的堆栈概念。但是在推送任何项目之前，如果项目不在堆栈中，并且它小于堆栈中的前一个元素，并且这些元素是重复的，那么我们需要弹出这些元素。

```
def remove_duplicate_letters(s):
    stack = []
    seen = set()
    last_occurrence = {c: i for i, c in enumerate(s)}

    for i, c in enumerate(s):
        if c not in seen:
            while stack and stack [-1] > c and i < last_occurrence[stack[-1]]:
                seen.discard(stack.pop())
            seen.add(c)
            stack.append(c)
    return ''.join(stack)

s = "cbacdcbc"
print(remove_duplicate_letters(s))
# acdb
```

## 删除 k 位数字

给定代表非负整数`num`和整数`k`的字符串 num，返回从 `num`中移除 `k` *位后的最小可能整数*。**

在这个问题中，我们使用单调递增堆栈来删除大数字。当添加一个新的数字时，我们检查先前的数字是否大于当前的数字，并将其弹出。

```
def remove_k_digits(num, k):
    if k >= len(num):
        return '0'
    stack = []
    for digit in num:
        while(k and stack and stack[-1] > digit):
            stack.pop()
            k -= 1
        stack.append(digit)

    stack = stack[:-k] if k else stack

    return "".join(stack).lstrip('0') or '0'

num = "1432219"
k = 3
print(remove_k_digits(num, k))
# 1219
```

## 下一个更大的元素 0

数组中某元素`x`的**下一个更大的元素**是同一数组中`x`右边**的**第一个更大的**元素。获取每个数组元素的下一个更大的元素。**

在这个问题中，我们使用单调递减堆栈。

**代码实现**

```
def get_next_greater_element(nums):
    n = len(nums)
    stack = []
    result = [-1] * n

    for i in range(n):
        while len(stack) > 0 and nums[stack[-1]] < nums[i]:
            result[stack[-1]] = nums[i]
            stack.pop()
        stack.append(i)
    return result

nums = [2, 7, 3, 5, 4, 6, 8]print(get_next_greater_element(nums))
# [7, 8, 5, 6, 6, 8, -1]
```

## 下一个更大的元素 1

给定一个循环整数数组`nums`(即`nums[nums.length - 1]`的下一个元素是`nums[0]`)，为 `nums`中的每个元素返回****下一个更大的数字*** *。**

*数字`x`的**下一个更大的数字**是数组中下一个遍历顺序的第一个更大的数字，这意味着您可以循环搜索以找到它的下一个更大的数字。如果不存在，返回该号码的`-1`。*

> *一个**循环**列表的问题，我们需要遍历列表**两次***

***代码实现***

```
*def next_greater_elements_cycle(nums):
    n = len(nums)
    result = [-1] * n
    stack = []

    for idx, val in enumerate(nums): 
        while stack and val > nums[stack[-1]]:
            result[stack.pop()] = val   
        stack.append(idx)

    for idx, val in enumerate(nums):  
        while stack and val > nums[stack[-1]]:
            result[stack.pop()] = valreturn result

nums = [1,2,3,4,3]
print(next_greater_elements_cycle(nums)) 
# [2, 3, 4, -1, 4]*
```

## *下一个更大的元素 2*

*数组中某个元素`x`的下一个更大的元素是同一数组中`x`右边的第一个更大的元素。给你两个不同的 0 索引整数数组`nums1`和`nums2`，其中`nums1`是`nums2`的子集。*

*对于每个`0 <= i < nums1.length`，找到索引`j`使得`nums1[i] == nums2[j]`和确定`nums2[j]`在`nums2`中的下一个更大的元素。如果没有下一个更大的元素，那么这个查询的答案是`-1`。*

*返回长度为 `nums1.length` *的*数组* `ans` *使得* `ans[i]` *是如上所述的* ***下一个更大的元素*** *。***

***代码实现***

```
*def next_greater_elements_2(nums1, nums2):
    mapping = {}
    result = []

    for item in nums2:
        while result and item > result[-1]:
            mapping[result.pop()] = item
        result.append(item)

    while result:
        mapping[result.pop()] = -1

    for item in nums1:
        result.append(mapping[item])

    return result

nums1 = [4,1,2]
nums2 = [1,3,4,2]
print(next_greater_elements_2(nums1, nums2))*
```

## *要排序的最大块*

*给定一个长度为`n`的整数数组`nums`，它表示范围`[0, n - 1]`中整数的排列。我们将`nums`分割成一定数量的**块**(即分区)，并单独对每个块进行排序。将它们连接起来后，结果应该等于排序后的数组。*

*返回我们可以对数组进行排序的最大块数。*

*对于给定的数组，最大数量的块发生在它增加并且每个元素都已经在正确的位置的时候。因此，只要我们看到一个顺序递增的数组，我们就继续。当我们看到一个元素的位置混乱时，我们希望找到它的正确位置，因为在当前位置和应该位置之间的任何东西都需要排序，因此会成为它自己的一部分。*

***代码实现***

```
*def max_chunks_to_sorted(nums):
    stack = []
    for num in arr:
        largest = num
        while stack and stack[-1] > num:
            largest = max(largest, stack.pop())
        stack.append(largest)

    return len(stack)

nums = [1,0,2,3,4]
print(max_chunks_to_sorted(nums))
# 4*
```

## *使数组非递减的步骤*

*给你一个 **0 索引**整数数组`nums`。在一个步骤中，**移除**所有元件`nums[i]`，其中`nums[i - 1] > nums[i]`用于所有`0 < i < nums.length`。*

*返回*执行的步数，直到* `nums` *变成* ***非递减*** *数组*。*

*从右到左反向扫描，保持递增堆叠。*

***代码实现***

```
*def total_steps(nums):
    ans = 0
    nums.reverse()
    stack = [[nums[0], 0]]
    for i in range(1, len(nums)):
        cnt = 0
        while stack and stack[-1][0] < nums[i]:
            cnt = max(cnt + 1, stack[-1][1])
            stack.pop()
        stack.append([nums[i], cnt])
        ans = max(ans, cnt)
    return ans

nums = [5,3,4,4,7,3,6,11,8,5,11]
print(total_steps(nums))*
```

## *子阵列最小值之和*

*给定一个整数数组 arr，求`min(b)`的和，其中`b`覆盖`arr`的每个(相邻)子数组。由于答案可能较大，返回答案**模**。*

***代码实现***

```
*def get_previous_large_element(nums):
    n = len(nums)
    left, stack = [-1] * n, []
    for i in range(n):
        while stack and nums[i] < nums[stack[-1]]: 
            stack.pop()
        left[i] = stack[-1] if stack else -1
        stack.append(i)
    for i in range(n):
        left[i] =  i+1 if left[i] == -1 else i - left[i]
    return leftdef get_next_large_element(nums):
    n = len(nums)
    right, stack = [-1] * n, []
    for i in range(n):
        while stack and nums[i] < nums[stack[-1]]: 
            right[stack.pop()] = i
        stack.append(i)
    for i in range(n):
        right[i] =  n - i if right[i] == -1 else right[i] - i
    return rightdef sum_subarray_mins(nums):
    mod = (10**9) + 7
    left = get_previous_large_element(nums)
    right = get_next_large_element(nums)

    return sum(a*l*r for a, l, r in zip(nums, left, right)) % modnums = [3,1,2,4]
print(sum_subarray_mins(nums))*
```

***优化的代码实现***

*我们使用一个单调的非递减堆栈来存储左边界和右边界，其中一个数是子数组中的最小数*

*一个数作为最小值出现的次数是`|left_bounday-indexof(num)| * |right_bounday-indexof(num)|`。总和是`sum([n * |left_bounday - indexof(n)| * |right_bounday - indexof(n)| for n in array])`*

```
*def sum_subarray_mins(nums):
    n = len(nums)
    nums.append(0)
    res = 0
    mod = 1000000007
    stack = [-1]

    for i, num in enumerate(nums):
        while stack and nums[stack[-1]] > num: # (i)
            idx = stack.pop()
            res += nums[idx] * (i - idx) * (idx - stack[-1])

        stack.append(i)

    return res % (mod)

nums = [3,1,2,4]print(sum_subarray_mins(nums))*
```

## *用全 1 计数子矩阵*

*给定一个`m x n`二进制矩阵`mat`，*返回具有全 1*的 ***子矩阵*** *的个数。**

*在第一步中，我们逐行排列`mat`以获得 1 的直方图。然后，我们对每一行使用一个单调堆栈，以找到每个 I，j 位置上全为 1 的子矩阵的最大数量。堆栈存储非递减高度的索引。每当新的高度出现时，弹出堆栈中高于新高度的高度，同时删除由额外高度贡献的配额(在弹出高度和新高度之间)。*

***代码实现***

```
*def num_sub_matrices(mat):
    m, n = len(mat), len(mat[0]) for i in range(m):
        for j in range(n):
            if mat[i][j] and i > 0: 
                mat[i][j] += mat[i-1][j]

    res = 0
    for i in range(m):
        stack = []
        cnt = 0
        for j in range(n):
            while stack and mat[i][stack[-1]] > mat[i][j]: 
                jj = stack.pop()
                kk = stack[-1] if stack else -1
                cnt -= (mat[i][jj] - mat[i][j])*(jj - kk) cnt += mat[i][j]
            res += cnt
            stack.append(j) return res*
```

## *直方图中最大的矩形*

*给定一个整数数组`heights`表示直方图的条形高度，其中每个条形的宽度为`1`，返回*直方图*中最大矩形的面积。*

*堆栈以递增的高度维护条形的索引。在添加新的条形之前，我们弹出比新条形高的条形。弹出的条代表一个矩形的高度，新条作为右边界，当前栈顶作为左边界。正如我们所知，一旦弹出，该元素将永远不会被再次使用，所以当弹出一个项目时，我们计算它可以生成的最大可能矩形，并更新`max_area`值。*

***代码实现***

```
*def largest_rectangle_area(heights):
    max_area = 0
    stack = []
    for idx, ht in enumerate(heights):
        start = idx
        while stack and stack[-1][1]> ht:
            i, h = stack.pop()
            max_area = max(max_area, h * (idx-i))
            start = i
        stack.append((start, ht))

    for i, h in stack:
        max_area = max(max_area, h*(len(heights)-i))

    return max_area

heights = [2,1,5,6,2,3]print(largest_rectangle_area(heights)) # 10*
```

## *收集雨水*

*给定代表高程图的非负整数`n`，其中每个条形的宽度为`1`，计算雨后它可以收集多少水。*

*高度 x 处的长度将是 x 的(下一个较小的索引-前一个较小的索引-1)*

***代码实现***

```
*def trap_rain_water(height):
    stack = []
    total = 0

    for i in range(len(height)):
        while len(stack) > 0 and height[stack[-1]] < height[i]:
            poppedIdx = stack.pop()

            if len(stack) == 0:
                break

            heightVal = min(height[stack[-1]], height[i]) - height[poppedIdx]
            length = i - stack[-1] - 1
            total += heightVal * length

        stack.append(i)

    return total

height = [0,1,0,2,1,0,1,3,2,1,2,1]print(trap_rain_water(height))
# 6*
```

*编码快乐！！*