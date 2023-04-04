# Python 中的字符串

> 原文：<https://blog.devgenius.io/string-in-python-3b0ce4458c76?source=collection_archive---------17----------------------->

可以通过将字符括在单引号或双引号中来创建字符串。Python 中甚至可以使用三重引号，但通常用于表示多行字符串和文档字符串。

```
# defining strings in Python
message = 'Hello World'
print(message)
```

**字符串中的转义引用:-**

在下面的字符串中，字符串之间有一个单引号。如果我们运行同样的代码，我们会得到一个无效的语法错误

```
Print('Hello, I don't like single quote')
                   ^
  SyntaxError: invalid syntax
```

有两种解决方案，

1.  将字符串放在双引号中，而不是单引号中。

```
print("hello I don't like single quote"
```

2.将转义字符放在字符串中的单引号之前。

```
print('hello I don\'t like single quote')
```

**Python 中的 len()函数**

Python 中的 len()函数返回对象的长度。它可以返回列表中的项目数。您可以将函数用于许多不同的数据类型。

```
greeting = "Good Day!"
len(greeting)Output -9 (Total 9 chars in the String)
```

**字符串索引**

在 Python 中，字符串是字符数据的有序序列，因此可以用这种方式进行索引。字符串中的单个字符可以通过指定字符串名称后跟方括号中的数字(`[]`)来访问。

```
s = 'welcome'

s[0]
'w'
s[1]
'e'
```

**字符串切片**

Python 还允许一种从字符串中提取子字符串的索引语法，称为字符串切片。如果`s`是一个字符串，`s[m:n]`形式的表达式返回从位置`m`开始到位置`n`的`s`部分:

```
s = 'welcome'
s[2:5]
'lco'
```

**Python 中的计数**

该方法返回指定值在字符串中出现的次数。

```
# define string
string = "Python is awesome, isn't it?"
substring = "is"count = string.count(substring)# print count
print("The count is:", count)Output - The count is: 2
```

**在 Python 中查找**

find()接受一个字符串作为输入。它用于在给定的字符串中查找子字符串第一次出现的**的索引。如果子串不存在，那么它返回 **-1** ，但不会抛出异常。**

```
paragraph = 'I am learning python and it is a great language'
print(paragraph.find('I'))
print(paragraph.find('q'))Output- 
0
1
```

**在 Python 中替换**

Python 字符串方法 **replace()** 返回一个字符串的副本，其中出现的旧*被替换为新*新*，可选地将替换数量限制为最大*和最大*。*

```
str = "this is string example...."
print str.replace("is", "was")Output - this was string example....
```

**Python 中的字符串格式**

示例中使用的字符串有多个空占位符，每个占位符将引用格式()中的一个值。第一个值将被第一个占位符替换，依此类推。

```
print ("{} is new kind of {} experience!".format("Python","learning"))Output - Python is new kind of learning experience!
```

**Python 中的 f 字符串**

Python f-string 是用于字符串格式化的最新 Python 语法。从 Python 3.6 开始就有了。Python f-strings 提供了一种在 Python 中格式化字符串的更快、更可读、更简洁、更不容易出错的方法。

```
name = 'Peter'
age = 23

print(f'{name} is {age} years old')Output - Peter is 23 years old
```