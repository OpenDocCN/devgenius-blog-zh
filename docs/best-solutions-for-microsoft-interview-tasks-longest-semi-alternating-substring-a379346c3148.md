# 微软面试任务的最佳解决方案。最长半交替子串

> 原文：<https://blog.devgenius.io/best-solutions-for-microsoft-interview-tasks-longest-semi-alternating-substring-a379346c3148?source=collection_archive---------8----------------------->

![](img/bf5875794727fcdf7e3f0a290ac4769d.png)

# 描述:

[](https://leetcode.com/discuss/interview-question/398037/) [## 微软| OA 2019 |最长半交替子串- LeetCode 讨论

### 提高你的编码技能，迅速找到工作。这是扩展你的知识和做好准备的最好地方…

leetcode.com](https://leetcode.com/discuss/interview-question/398037/) 

# 解决方案:

这个任务类似于没有 3 个相同连续字母的任务[字符串，但是这里我们需要找到一个不包含 3 个相同连续字符的最长子字符串，并返回它的长度。](https://medium.com/@molchevsky/best-solutions-for-microsoft-interview-tasks-string-without-3-identical-consecutive-letters-88ceac458572)

为了做到这一点，让两个计数器 ***计数*** 和 ***结果。*** ***计数*** 将保持我们处理的当前子串的长度，而 ***结果*** 将保持我们找到的最长子串的长度。然后我们需要遍历给定的字符串，并在 ***count 中计算它的字符数。*** 如果我们遇到三个相同的连续字符，我们必须将当前处理的子串的长度保存在 ***结果*** 中作为最长的子串，以防 ***计数*** 大于 ***结果*** 。然后我们必须重置 ***计数*** 并从 1 开始计算当前子串的长度。

当整个字符串被处理后，只需返回保存的最长子字符串的计数器。

C++代码:

```
int solution(const string & s) {
    const int MAX_COUNT = 3;
    int s_len = s.length();
    if(s_len < MAX_COUNT) { return s_len; }
    int count = 1, result = 1;
    // Scan whole string and count it's characters.
    for(int i = 1; i < s_len - 1; ++i) {
        // If we meet 3 consecutive characters
        if((s[i-1] == s[i]) && (s[i+1] == s[i])) {
            // save the counter as result if it is 
            // bigger than the previous result
            result = max(result,count+1);
            // and drop the counter to 1
            count = 1;
        }
        else { count++; }
    }
    // return maximal result
    return max(result,count+1);
}
```

您可以在这里找到包含完整项目的资源库:[https://github . com/jolly-fellow/Microsoft/tree/master/longest _ semi-alternating _ substring](https://github.com/jolly-fellow/microsoft/tree/master/longest_semi-alternating_substring)

返回到[目录。](https://medium.com/@molchevsky/best-solutions-for-microsoft-interview-tasks-cae6b0f3ff86)