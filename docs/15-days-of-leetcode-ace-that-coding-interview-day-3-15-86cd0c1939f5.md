# 一周的 LeetCode(第 3/7 天)

> 原文：<https://blog.devgenius.io/15-days-of-leetcode-ace-that-coding-interview-day-3-15-86cd0c1939f5?source=collection_archive---------1----------------------->

![](img/b6e3e06d7ea25d5b1c92fbe008944384.png)

克里斯托夫·高尔在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄的照片

欢迎来到我们技术面试准备系列的第三集。我的目标是为你铺平道路，让你轻松获得你梦想中的工作，为你提供真正的解决问题的技能，涵盖很多话题。

准备好了吗？让我们直接跳进来吧！

第三天:[救人的船](https://leetcode.com/problems/boats-to-save-people/)

我假设你没有阅读任务描述，所以这里有一个摘要:

> 给定一个数组 w，其中 w[i]是第 I 个人的重量，你有无限多条船，每条船的容量为 L，最多可以载两个人。
> 
> 你需要承载所有人的最少船只数量是多少？

处理这个问题的一个好方法是把它分解成更小的子问题。让我们这样做吧！

**看下面这个稍微简单一点的问题:**

假设你已经分配了一些人到第一艘船上，还有一些剩余的空间 r，你能对这些额外的空间做什么最好的选择。

> 提示:想象一下，你只有两个未分配的人(体重不同)可以放入这个额外的空间。你应该选择哪一个？

显然，如果我们只需要容纳一个人，并且我们有一个空间 R，那么选择最大的一个总是更好的。最简单的推理方式是:

通过选择能装进 R 的最大的人，你比容纳一个体重较轻的人浪费更少的空间。

更严谨地说，如果你不选择能适合的最大的人(姑且称他为 X)而选择另一个重量更小的人(姑且称他为 Y)，那么你可以通过交换 X 和 Y 的位置找到至少一样好的解。

现在回到我们最初的问题:我们现在有了在每条船上选择第二个人的策略，但是我们如何选择第一个人呢？！

这是一个稍微困难的决定，但是我们仍然可以应用同样的逻辑，但是我们仍然需要简单的观察。

> 简单的观察:我们把人分配到船上的顺序并不重要，最终每个人都会在某条船上。

你可能认为这太明显了，观察对我们没有太大帮助，但让我们深入那个方向，挖掘更多有价值的观察。

1-每个人将被分配到一艘船上。

这意味着最重的人被分配到某条船上。

3-不知何故最重的人是要求最高的人，不给我们太多选择，所以从分配他开始是有意义的。

这种思维逻辑被称为贪婪思维，它说，如果你必须做出决定，那么选择目前看起来最方便的选项，而不用担心未来的决定，我知道这可能不是最好的生活课程，但它在解决算法任务时是有用的；)

![](img/eea0fa33db65cb5b369d48a32c5bc07b.png)

莎伦·麦卡琴在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上的照片

还记得我们如何从选择最重的人开始，当我们有一艘载有一个人的船时，我们通过选择最重的人来分配另一个人，我们有点想关于那艘船的最好的事情，而没有考虑我们未来可能需要的其他船。

让我们用一些伪代码来阐述一下:

```
answer = 0 
sort the people by weightsrepeat until all people are assigned to boats :- 

   1-pick the heaviest person and assign it to some boat.
   2-calculate the remaining space R on that boat.
   3-if there is some people with weight <= R who are not assigned.
   4-then pick the heaviest one of them and assign it to that boat. We have found a strategy for the task, let's write some code!
```

我的 C++实现:

**我们如何应对今天的任务，以及如何应对其他挑战？**

*   将你手中的问题分解成更小的子问题:这种技术非常有用，因为更小的问题往往有更简单、有时是众所周知的解决方案。
*   思考如何高效地解决更小的子问题，并将其组合成一个最终的快乐解决方案。
*   改变问题的设置，做出有助于你更好地解释面前问题的假设:注意，我们对原始数组进行了排序，以便运行我们的贪婪算法。
*   反复阅读这篇文章，直到你完全理解为止，尤其是如果这是你第一次看到贪婪的做法。

# 轮到你了:

恭喜你！如果你一口气做到了，我知道你能做到。

既然你有希望获得一些新技能，不要止步于此，将它们付诸实践。

去玩代码，自己从头开始写，挑战自己，想出可以用贪婪的方法解决的问题的例子。

**练习附加题:**

[使数组增加的最小运算量](https://leetcode.com/problems/minimum-operations-to-make-the-array-increasing/)

[分配 cookie](https://leetcode.com/problems/assign-cookies/)

[最小不相容性](https://leetcode.com/problems/minimum-incompatibility/)

[最大建筑高度](https://leetcode.com/problems/maximum-building-height/)

[任务调度器](https://leetcode.com/problems/task-scheduler/)

[非重叠区间](https://leetcode.com/problems/non-overlapping-intervals/)

如果你对我有任何问题，请在 [**LinkedIn**](https://www.linkedin.com/in/mohamed-sobhy-12181b165/) 上联系我，在 [**Medium**](https://medium.com/@mohamedsobhi777) 上关注我，获取更多有趣的文章。

今天到此为止。感谢阅读！