# Python 入门的 10 个基本练习|第 3 部分

> 原文：<https://blog.devgenius.io/10-basic-exercise-to-get-you-started-in-python-part-3-eda19d76ff7a?source=collection_archive---------10----------------------->

这些练习是本博客第 1、2 部分的延续。如果您还没有阅读第 1 部分，下面是链接:-[https://medium . com/@ arbaazkan 96/10-basic-exercise-to-get-you-started-in-python-4d 0 aa 6a 3b 185](https://medium.com/@arbaazkan96/10-basic-exercise-to-get-you-started-in-python-4d0aa6a3b185)

Part-2 下面是链接:-[https://medium . com/@ arbaazkan 96/10-basic-exercise-to-get-you-started-in-python-part-2-5e 7 ddf 83020 b](https://medium.com/@arbaazkan96/10-basic-exercise-to-get-you-started-in-python-part-2-5e7ddf83020b)

现在让我们从练习开始

**练习 1)解二次方程**

```
# The sqrt() function retrieves the square root
**from math import sqrt**# The quadratic equation has the form a*X*x+b*x+c = 0
# When the coefficients a,b,c are known, we are dealing with a 
# Specific quadratic equation. The root of the equation are the 
# values of the variable x for which a particular equation 
# Becomes true. What equation should be solved ?
**a = float(input("a = "))
b = float(input("b = "))
c = float(input("c = "))**# Calculating the discriminant
**d = b*b-4*a*c**# If it is greater than zero, then the equation has two roots
**if d > 0:
    x1 = (-b+sqrt(d))/(2*a)
    x2 = (-b-sqrt(d))/(2*a)
    print("x1 = %.2f; x2 = %.2f" %(x1,x2))**# If the discriminant is less than zero, then the equation has only
# root.
**elif d == 0:
    x1 = -b/(2*a)
    print("x1 = %.2f" % x1)**# Otherwise (if the discriminant is less then zero), the equation 
# Has no roots.
**else:
    print("No roots")**
```

**练习 2)猜随机数**

```
**from random import randint****secret_number = randint(1,100)
user_num = -1
try_count = 1****while secret_num != user_num:
    print("%d try:" % try_count, end="")
    user_num = int(input())
    if user_num < secret_num:
        print("Too less")
    elif user_num > secret_num:
        print("Too much")
    else:
        print("You guessed it!")
    try_count +=1**
```

**练习 ASCII 字符表的输出**

```
# Display in tabular form characters 32 through 127, inclusive.
**for i in range(32, 128):
    print(chr(i),end='')
    if (i-1)%10==0:
    print()
print()** 
```

**练习 4)展开类似“a-z”的字符串**

```
# Two letter of the English alphabet are given display a string
# That includes, in in order, all letters of the alphabet from the   # Given first letters to the last one inclusive.
**first = input("The first: ")
last = input("The last: ")****while first <= last
    print(first, end="")
    first = chr(ord(first)+1)
print()**
```

**练习 5)乘法表(while)**

```
# Using the "While" loop, print the multiplication table to the 
# screen
**i = 1****while i<10:
    j = 1
    while j < 10:
        print("%4d"%(i*j),end='')
    print()
    i += 1**
```

**练习 6)乘法表(for)**

```
# Using the "for" loop, print the multiplication table to the screen
**for i in range(1,10):
    for j in range(1,10):
        print("%4d" %(i*j),end='')
    print()**
```

**练习 7)将一个数字从十进制转换为基数不超过 9 的任何数字系统**

```
# A decimal number and base of another number system (up to and 
# including 9) are given. Write a program that converts the decimal 
# number to the specified numeral system.
**num = int(input())
base = int(input("Base(2-9): "))****if not (2<=base<=9):
    quit()****new_num = ''****while num > 0:
    new_num = str(nun % base) + new_num
    num //= base
print(new_num)**
```

**练习 8)阶乘**

```
# Write a program that calculates the factorial of an integer
**n = int(input())
factorial = 1****while n>1:
    factorial *= n
    n -= 1****print(factorial)**
```

**练习 9)数列 1，-0.5，0.25，-0.125，…**

```
# The user enter the number of elements of the sequence to be 
# Displayed on the screen. Print specified number of elements 
# of the sequence 1, -0.5, 0.25, 0.125,...(each next element is 
# twice less in absolute value than the previous one and has the 
# opposite sign)
**n = int(input("Number of items: ")
item = 1****while n>0:
    print(item, end='')
    item = item / -2
    n -=1
print()** 
```

**练习 10)计算数字的偶数和奇数位**

```
# A natural number is entered. The program Should determine how many
# Numbers are contained in this number and how many are odd
**a = int(input()) 
even = 0
odd = 0****while a>0:
    if a % 2 == 0:
        even +=1
    else:
        odd += 1
    a = a//10****print("Even: %d" %even)
print("Odd: %d" %Odd)**
```