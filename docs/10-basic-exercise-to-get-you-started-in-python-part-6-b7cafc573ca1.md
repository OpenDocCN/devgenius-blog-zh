# Python 入门的 10 个基本练习|第 6 部分

> 原文：<https://blog.devgenius.io/10-basic-exercise-to-get-you-started-in-python-part-6-b7cafc573ca1?source=collection_archive---------13----------------------->

这些练习是本博客第 3、4、5 部分的延续。如果你还没有阅读第 3 部分，这里是链接:-[https://medium . com/@ arbaazkan 96/10-basic-exercise-to-get-you-started-in-python-Part-3-EDA 19d 76 ff 7 a](https://medium.com/@arbaazkan96/10-basic-exercise-to-get-you-started-in-python-part-3-eda19d76ff7a)

Part-4 下面是链接:-[https://medium . com/@ arbaazkan 96/10-basic-exercise-to-get-you-started-in-python-part-4-aa 61691 c0e F2](https://medium.com/@arbaazkan96/10-basic-exercise-to-get-you-started-in-python-part-4-aa61691c0ef2)

Part-5 这里是链接:-[https://medium . com/@ arbaazkan 96/10-基本-锻炼-让你开始-在 python 中-part-5-c42bb82aca9c](https://medium.com/@arbaazkan96/10-basic-exercise-to-get-you-started-in-python-part-5-c42bb82aca9c)

现在让我们从练习开始

**练习 1)数值按范围分布**

```
# A list is given that contains values from I to 9
# inclusive. The program should count how many
# elements in this list are in the ranges 1 to 3, 4 to 6, 7
# to 9.

a = [3, 5, 7, 3, 8, 1, 8, 1, 7, 3, 2,
     4, 6, 8, 5, 4, 3, 3, 6, 5, 7, 8,
     9, 5, 3, 2, 3]

count_1_3 = 0
count_4_6 = 0
count_7_9 = 0

for i in a:
    if 1 <= i <= 3:
        count_1_3 += 1
    elif 4 <= i <= 6:
        count_4_6 += 1
    elif 7 <= i <= 9:
        count_7_9 += 1

print("Range 1-3:", count_1_3, "items")
print("Range 4-6:", count_4_6, "items")
print("Range 7-9:", count_7_9, "items")
```

**练习 2)替换列表项**

```
# A list of integers is given. Replace positive elements
# in it with the value 1, negative ones with the value -1,
# leave zero unchanged.

array = [10, -15, 3, 8, 0, 9, -6]
print(array)

for i in range(len(array)):
    if array[i] > 0:
        array[i] = 1
    elif array [i] < O:
        array[i] = -1

print(array)
```

**练习 3)检查文件扩展名**

```
# The user enters the name of the file. Check if the file
# extension is in the list of valid ones.

extensions = ['png', 'jpg', 'jpeg', 'gif']

fname = input().split('.')

if len(fname) >= 2:
    fname_ext = fname[-l].lower()
    if fname_ext in extensions:
        print("Yes")
    else:
        print("No")

else:
    print("The fname has no extension")
```

**练习 4)找出字符串中最长的单词**

```
# A string of words is given. Find the longest word in it 
# and display it on the screen.

string = "python java c c++ javascript pascal php"
print(string)

words = string.split()

id_longest = 0

for i in range(1, len(words)):
    if len(words[id_longest]) < len(words[i]):
        id_longest = i

print(words[id_longest]) 
```

**练习 5)将文本转换为去掉标点符号的单词列表**

```
# A string consisting of words and punctuation marks
# is entered. Put words from this string into the list.

string = input("Write down a text:\n")

signs = ['.', ',', ':', ';',
         '!', '?', '(', ')']

words = string.split()

i = 0
for word in words:
    if word[-1] in signs:
        words[i] = word[:-1]
        word = words[i]
    if word[0] in signs:
        words[i] = word[1:]
    i += 1

for i in words:
    print(i, end=' ')
print()
```

**练习 6)列表的交集。从两个列表中找到相同的项目**

```
# Two lists are given. Create a third list from the
# elements that are in both the first list and the second.

a = [5, [1, 2], 2, 'r', 4, 'ee']
b = [4, 'we', 'ee', 3, [1, 2]]
c = []

for i in a:
    if i in c:
        continue
    for j in b:
        if i == j:
            c.append(i)
            break

print(c)
```

**练习 7)找出列表中最常出现的值**

```
# A list of integers is given. Determine which value
# occurs most often in it. Display it on the screen.

from random import random

a = [int(random()*5) for i in range(15)]
print(a)

a_set = set(a)

most_common = None
qty_most_common = 0

for item in a_set:
    qty = a.count(item)
    if qty > qty_most_common:
        qty_most_common = qty
        most_common = item

print(most_common)
```

**练习 8)冒泡排序**

```
# Fill the list with random numbers and display it on
# the screen. Sort the list in ascending order using the
# bubble method. Display the sorted list.

from random import randint

N = 7
a = []

for i in range(N):
    a.append(randint(1, 20))
print(a)

for i in range(N-1):
    for j in range(N-i-1):
        if a[j] > a[j+1]:
            b = a[j]
            a[j] = a[j+1]
            a[j+1] = b

print(a)
```

**练习 9)选择排序**

```
# Fill the list with random numbers and display it on
# the screen. Sort the list in ascending order using the
# selection sort method. Display the sorted list.

from random import randint

N = 7
a = []
for i in range(N):
    a.append(randint(1, 20))
print(a)

j = N -1
while j > 0:
    ind = 0
    for i in range(1, j+1):
        if a[i] > a[ind]:
            ind = i
    b = a[ind]
    a[ind] = a[j]
    a[j] = b
    j -= 1

print(a)
```

**练习题 10)二分搜索法。在有序数组中寻找一个数。**

```
# Fill the array with random numbers, sort it using the
# "sort" method, display the sorted list on the screen.
# Ask the user for the number which he wants to find in
# this array. Using the binary search method, find this
# element and display its index.

from random import random

N = 20
array = []
for i in range(N):
    array.append(int(random()*100))
array.sort()
print(array)

number = int(input())

low = 0
high = N-1
while low <= high:
    mid = (low + high) // 2
    if number < array[mid]:
        high = mid - 1
    elif number > array[mid]:
        low = mid + 1
    else:
        print("ID =",mid)
        break
else:
    print("No the number") 
```