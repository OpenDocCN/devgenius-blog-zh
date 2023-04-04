# 微软面试任务的最佳解决方案。K 天后的一周中的某一天。

> 原文：<https://blog.devgenius.io/best-solutions-for-microsoft-interview-tasks-day-of-week-a8e931e21266?source=collection_archive---------5----------------------->

![](img/bf5875794727fcdf7e3f0a290ac4769d.png)

# 描述:

[](https://leetcode.com/discuss/interview-question/703151/) [## Microsoft | OA 2020 |天后的星期几- LeetCode 讨论

### 提高你的编码技能，迅速找到工作。这是扩展你的知识和做好准备的最好地方…

leetcode.com](https://leetcode.com/discuss/interview-question/703151/) 

# 解决方案:

根据描述，我们有输入参数:字符串与星期几的名称和距离，直到一周的另一天。我们必须找到一周的第二天的名称，并返回一个带有其名称的字符串。要解决这个问题，最好是将一周中给定日期从字符串转换为天数。让我们从 0 开始数日子。也就是周日是 0，周一是 1 …周六是 6。

那么我们就有了一周中两天之间的距离。我们需要找出一周中的哪一天是这段距离的最后一天。或者换句话说，一周中的第几天。

每周包含 7 天，因此为了找到一周中的某几天，我们需要将这些天之间的距离从天转换为周。如果我们有给定日期之间的整数周数，就很容易计算出以周为单位的距离。例如，如果我们有 S =“太阳”和 K = 7，7 天中的一天也是“太阳”。但是如果距离没有被整除，我们可以用除以 7 后的余数来计算一周内的天数。例如，假设 S = Tue，K = 10。星期二是一周的两天。我们应该把 10 加到 2。10+2=12 并将 12 除以 7。我们得到余数= 5。

因此，一周的最后一天是星期五，因为一周的第五天是星期五，在本周的星期二和下周的星期五之间有 10 天。

C++代码:

```
string solution(const string &day, int k) {
    vector<string> days = {"Sun", "Mon", "Tue", "Wed", 
                           "Thu", "Fri", "Sat"};
    unordered_map<string, int> week_map = {{"Sun", 0},
                                           {"Mon", 1},
                                           {"Tue", 2},
                                           {"Wed", 3},
                                           {"Thu", 4},
                                           {"Fri", 5},
                                           {"Sat", 6}};
    return days[(week_map[day] + k) % 7];
}
```

您可以在这里找到包含完整项目的资源库:[https://github . com/jolly-fellow/Microsoft/tree/master/day _ of _ week](https://github.com/jolly-fellow/microsoft/tree/master/day_of_week)

返回到[目录。](https://medium.com/@molchevsky/best-solutions-for-microsoft-interview-tasks-cae6b0f3ff86)