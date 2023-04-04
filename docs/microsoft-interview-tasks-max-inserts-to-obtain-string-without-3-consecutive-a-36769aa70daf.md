# 微软面试任务的最佳解决方案。获得没有 3 个连续“a”的字符串的最大插入次数。

> 原文：<https://blog.devgenius.io/microsoft-interview-tasks-max-inserts-to-obtain-string-without-3-consecutive-a-36769aa70daf?source=collection_archive---------7----------------------->

![](img/bf5875794727fcdf7e3f0a290ac4769d.png)

# 描述:

[](https://stackoverflow.com/questions/58125024/how-many-characters-x-can-you-add-in-a-string-so-that-no-three-consecutive-chara) [## 你可以在一个字符串中添加多少个字符 x，这样就不会有三个连续的字符在…

### 这是一个 leetcode 式的面试问题，我曾经尝试过，但没有成功，现在又尝试了一次。给定…

stackoverflow.com](https://stackoverflow.com/questions/58125024/how-many-characters-x-can-you-add-in-a-string-so-that-no-three-consecutive-chara) 

# 解决方案:

如果 N 是给定字符串中的字符数，并且所有字符都不是 A，我们可以在这个字符串中插入 2 * (N + 1)个字符，因为我们可以在字符串中的每两个字符之间插入两个 As，在字符串的开头和结尾插入两个 As。因此，为了解决这个问题，我们需要计算所有的非 A 字符。当我们知道这个数字时，就很容易计算出我们可以插入多少。这个数字将是 2 *(可能插入的位置数+ 1) —找到的位置数。

换句话说，2 *(N+1)——(N-As 的数量)；

C++代码:

```
int solution(const string & s) {
    int count_As = 0, count_others = 0, s_len = s.length();
    for (int i = 0; i < s_len; ++i) {
        if (s[i] == 'a') {
            count_As++;
        }
        else {
            count_others++;
            count_As = 0;
        }
        if (count_As == 3) {
            return -1;
        }
    }
    return 2 * (count_others + 1) - (s_len - count_others);
}
```

您可以在这里找到包含完整项目的存储库:[https://github . com/jolly-fellow/Microsoft/tree/master/max _ inserts _ to _ get _ string _ without _ 3 _ continuous _ a](https://github.com/jolly-fellow/microsoft/tree/master/max_inserts_to_obtain_string_without_3_consecutive_a)

返回到[目录。](https://medium.com/@molchevsky/best-solutions-for-microsoft-interview-tasks-cae6b0f3ff86)