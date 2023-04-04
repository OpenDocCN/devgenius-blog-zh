# Python 中的 f 字符串

> 原文：<https://blog.devgenius.io/f-string-in-python-5f5056c94601?source=collection_archive---------18----------------------->

![](img/e066e331728772ad30d3e8b7a6c5e283.png)

照片由[斯文·布兰德斯马](https://unsplash.com/@seffen99?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)在 [Unsplash](https://unsplash.com/s/photos/words?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 上拍摄

*在 Python 中，f-string 是一个文字字符串,‘f’表示格式，它通过用值替换大括号中任何变量的名称来格式化字符串。例如*

## 一个变量和一行

当我们只有一个输入变量时，在这个例子中变量是年龄。

```
age=25
f'My age year is {age}'
```

输出 **:**

```
'My age year is 25'
```

## 两个变量和一条线

当我们在字符串 **:** 中输入多个变量时

```
city="Tokyo"
year=2010
f'I am living in {city} since {year}'
```

输出 **:**

```
'I am living in Tokyo since 2010'
```

## 多线

*   我们对每一行单独使用`f`，你需要在多行字符串 **:** 的每一行前放置一个`f`

```
city="Tokyo"
year=2010
profession='statistician'
programming_language='python'
a=(f'I am a {profession}.' 
f'I am living in {city} since {year}.' 
f'I like {programming_language}.')
print(a)
```

输出 **:**

```
I am a statistician. I am living in Tokyo since 2010\. I like python.
```

*   如果在第一行的开头只用一个`f`只有 **:** 呢

```
city="Tokyo"
year=2010
profession='statistician'
programming_language='python'
a=(f'I am a {profession}. ' 
'I am living in {city} since {year}. ' 
'I like {programming_language}.')
print(a)
```

输出 **:**

```
I am a statistician. I am living in {city} since {year}. I like {programming_language}
```

*   只在一行中使用上面的代码并且只有一个`f`:**:**

```
city="Tokyo"
year=2010
profession='statistician'
programming_language='python'
a=f'I am a {profession}. I am living in {city} since {year}. I like {programming_language}.'
print(a)
```

输出 **:**

```
I am a statistician. I am living in Tokyo since 2010\. I like python.
```

## 日期时间

打印当前时间 **:**

```
import datetime
current_time = datetime.datetime.now().strftime('%I:%M:%S %p')

print(f"Current Time: { current_time }")
```

输出 **:**

```
Current Time: 09:34:16 AM
```

## 任意值 **:**

```
value=3*7
f'i read total of {value} pages of the book each weak'
```

输出 **:**

```
'i read total of 21 pages of the book each weak'
```

如果你欣赏这个帖子，请给我买一个 [ko-fi](https://ko-fi.com/haseeba) 。