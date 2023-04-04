# golang in 60 seconds—计算字符串中出现的次数

> 原文：<https://blog.devgenius.io/golang-in-sixty-seconds-count-occurrences-in-a-string-984fe04dfa4f?source=collection_archive---------7----------------------->

![](img/39ac91e62ca0da6eaa8cf79eb5266f2f.png)

由[路易斯·阿里亚斯](https://unsplash.com/@luism_arias?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄

你有没有想过快速计算字母 s 在密西西比出现了多少次？我们会支持你的。

`strings.Count("Mississippi", "s") // 4`

我见过的最常见的代码高尔夫挑战之一是计算字符串中某个字母的出现次数，并对这些信息做一些处理。让我们把单词“Mississippi”中的每个字母换成它在单词中出现的次数:

```
word := “Mississippi”
newWord := “”
for _, l := range word {
    newWord = newWord + strconv.Itoa(strings.Count(word, string(l)))
}
fmt.Println(newWord) // "14444444224"
```

在这里，我们循环遍历单词中的每个符文(字符)，计算它出现的次数，将该数字转换为一个字符串，并将其追加到`newWord`字符串。你可以在这里尝试一下

更[六十秒后 Golang](https://richard-t-bell90.medium.com/list/golang-in-sixty-seconds-7a26c5131734)

[*获取无限制访问介质*](https://richard-t-bell90.medium.com/membership)

[*给我买杯咖啡*](https://ko-fi.com/richardtbell) *如果你喜欢这篇文章:)*

*更多内容请看*[*blog . dev genius . io*](http://blog.devgenius.io)*。*