# Python 入门的 10 个基本练习|第 5 部分

> 原文：<https://blog.devgenius.io/10-basic-exercise-to-get-you-started-in-python-part-5-c42bb82aca9c?source=collection_archive---------23----------------------->

这些练习是本博客第 2、3、4 部分的延续。如果你还没有阅读第 2 部分，这里是链接:-[https://medium . com/@ arbaazkan 96/10-basic-exercise-to-get-you-started-in-python-Part-2-5e 7 ddf 83020 b](https://medium.com/@arbaazkan96/10-basic-exercise-to-get-you-started-in-python-part-2-5e7ddf83020b)

Part-3 下面是链接:-[https://medium . com/@ arbaazkan 96/10-基础-练习-让你入门-python-part-3-eda19d76ff7a](https://medium.com/@arbaazkan96/10-basic-exercise-to-get-you-started-in-python-part-3-eda19d76ff7a)

Part-4 下面是链接:-[https://medium . com/@ arbaazkan 96/10-basic-exercise-to-get-you-started-in-python-part-4-aa 61691 c0e F2](https://medium.com/@arbaazkan96/10-basic-exercise-to-get-you-started-in-python-part-4-aa61691c0ef2)

现在让我们从练习开始

**练习 1)从字符串中选择整数**

```
# A string consisting of letters and digits is given extract number
# (not digits) from a string and display them on the screen.**s = input()
l = len(s)****i = 0
while i < 1:
    num = ''
    symbol = s[i]
    while symbol.isdigit():
        num += symbol
        i += 1
        if i < l:
            symbol = s[i]
        else:
            break
    if num != '':
        print(num)
    i += 1**
```

**练习 2)找出浮点数的最大位数**

```
# A random float number is generated find the maximum digit in it.**from random import random****num = round(random() * 1000, 3)
print(num)****str_num = str(num)****max_digit = -1****for i in range(len(str_num)):
    if str_num[i] == '.':
        continue
    elif max_digit < int(str_num[i]):
        max_digit = int(str_num[i])****print(max_digit)**
```

**练习 3)确定字符串中小写字母和大写字母的百分比**

```
# A string is entered. Determine how many lowercase and how many
# uppercase letters are in it. Express the result as a percentage**string = input()
length = len(string)
lower = upper = 0****for i in string:
    if i.islower():
        lower += 1
    elif i.isupper():
        upper += 1****per_lower = lower / length * 100
per_upper = upper / length * 100
print("Lower: %.2f%%" % per_lower)
print("Upper: %.2f%%" % per_upper)**
```

**练习 4)字符串是回文吗？**

```
# A string is entered. Determine if it is a palindrome (reads the
# same from left to right and right to left).**s = input()
l = len(s)****for i in range(l//2):
    if s[i] != s[-1-i]:
        print("It's not palindrome")
        guit()****print("It's palindrome")**
```

**练习 5)用随机数填充数组**

```
# Fill the list with random integers and display it.**from random import randint****N = 10
array = []****for i in range(N):
    array.append(randint(1, 99))****for i in array:
    print(i, end='')****print()**
```

**练习 6)寻找一个数组的最小和最大元素**

```
# Fill the list with random integers. Find the elements
# with the minimum and maximum values in the list.**from random import randint****N = 7
a = [0] * n****for i in range(N):
    a[i] = randint(0, 100)
    print(a[i], end=' ')
print()****maximun = -1
minimum = 101
for i in a:
    if i > maximum:
        maximum = i
    if i < minimum:
        minimum = i****# or use functions of Python
# maximum = max(a)
# minimum = min(a)****print("Maximum:", maximum)
print("Minimum:", minimum)**
```

**练习 7)列表中有多少个奇数和偶数？**

```
# Fill in the list with random natural numbers. Count
# how many even values are in it, and how many are odd.**from random import random****a = []
for i in range(7):
    n = int(random() * 100)
    a. append(n)****print(a)****even = O
odd = O****for i in a:
    if i % 2 == 0:
        even += 1
    else:
        odd += 1****print( "Even:", even)
print( "Odd:", odd)**
```

练习 8)把正数和负数分开

```
# There is a list of positive and negative numbers. Put
# negative items in one list and positive items in
# another.**from random import random****a =[]
for i in range(7):
    n = int(random() * 20) - 10
    a.append(n)****print(a)****neg = []
pos = []
for i in a:
    if i < 0:
        neg.append(i)
    elif i > O:
        pos.append(i)****print(neg)
print(pos)**
```

**练习 9)输出大于数组算术平均值的项目**

```
# Fill the list with random integers. Find the arithmetic
# mean of the values of the elements in this list
# Display list items with values greater than the
# arithmetic mean.**from random import randint
N = 10
a = []
suma = O****for i in range(N):
    a. append(randint(O, 9))
    print(a[i]), end=' ')
    suma += a[i]
print()****average = suma / N
print("The average: %.2f" % average)****for i in a:
    if i > average:
        print(i, end='')
print()**
```

**练习 10)值属于指定范围的元素的数量**

```
# The user enters the minimum and maximum range
# limits. The program should display the number of
# matrix elements whose values belong to the
# specified range.**import random****arr = []
for i in range(30):
    x = int(random.random() * 100)
    arr.append(x)
    print("%3d" % x, end=' ')
    if (i + 1) % 5 == 0:
        print()****minimum = int(input('Min: '))
maximum = int(input('Max: '))****qty = O
for i in arr:
    if minimum <= i <= maximum:
        qty += 1****print(qtY)**
```