# Python 入门的 10 个基本练习|第 2 部分

> 原文：<https://blog.devgenius.io/10-basic-exercise-to-get-you-started-in-python-part-2-5e7ddf83020b?source=collection_archive---------7----------------------->

这些练习是本博客第一部分的延续。如果您还没有阅读第 1 部分，下面是链接:-[https://medium . com/@ arbaazkan 96/10-basic-exercise-to-get-you-started-in-python-4d 0 aa 6a 3b 185](https://medium.com/@arbaazkan96/10-basic-exercise-to-get-you-started-in-python-4d0aa6a3b185)

现在让我们从练习开始

**习题 1)数字是负数还是正数？**

```
# user enters a number 
**a = input()**# converting it to a floating point number
**a = float(a)**# if the number is greater than zero,
**if a > 0:
   print(1)**# then output is 1# if the number is less than zero,
**elif a < 0:
    print(-1)**# then output is -1# In all other cases
**else:
    print(0)**# then output is 0# Note. "All other cases" is when the number 
# is zero, it is no more and no less then zero
```

**练习 2)这个数字是偶数还是奇数？**

```
# user enter a number 
**a = input()**# convert it to integer type
**a = int(a)**# The % (modulus) operator return the residue 
# of the division. Even numbers are completely
# divisible by 2, the remainder is 0\. Odd numbers
# have a residue equal to 1\. If the remainder of the 
# division is zero,
**if a%2 == 0:
    print("Even")**# The number is even 
# Otherwise,
**else:
    print("Odd")**# The number is odd.
```

**练习 3)确定三个的最大整数**

```
**a = int(input())
b = int(input())
c = int(input())
print("The maximum is",end="")**# If A is greater than B and greater than C, then
# it is the maximum
**if b <= a >= c:
    print(a)**# Otherwise, we check the number B, that it is greater
# than both A and C
**elif a <= b >= c:
    print(b)**# Otherwise, we check the number C, that it is greater
# than both A and B
**elif a <= c >= b:
    print(c)**# Note 1.The last elif can be replaced by a branch without
# a condition (else)
# Note 2\. In conditional expressions, the sign = is necessary
# for the case when two numbers have the same maximum value.
```

**练习 4)检查一个整数被另一个整数整除的情况**

```
# The check of divisibility makes sense only for integer. Therefore,
# we convert the string returned by input() function to an integer
# type using the int() function.
**a = int(input())
b = int(input())**# The % (modulus) operator return the residue of the division.
# If it is zero, there is no remainder, then the first number is 
# divided completely into the second.
**if a%b == 0:
    print("Yes")**# In all other cases, there is the remainder and it is not zero.
# Hence the first number is not divided completely into second.
**else:
    print("No")**
    # display the value of the remainder
    **print("The remainder is ",a%b)**# We can also create a function
**def dev_int(a,b):
    if a%b == 0:**# We can also use
        **print("Yes"**)# return a%b
    **else:
        Print("No")
        print("The remainder is ",a%b)**# return a%b
```

**练习 5)将摄氏温度转换为华氏温度，反之亦然**

```
# input nC or nF is expected, where n is an integer
**t = input()**# the last symbol is retrieved
**sign = t[-1]**# whole string expect last symbol
**t = t[0:-1]**# conversion into the integer
**t = int(t)**# if the sign denotes Celsius
**if sign == "C" or sign == "c":**
    # converting to celcius
    **t = t*(9/5)+32**
    # rounding to the integer
    **t = round(t)
    print(str(t)+"F")**# if the sign indicates Fahrenheit
**elif sign == "F" or sign == "f":**
    # converting to Fahrenheit
    **t = (t-32)*(5/9)**
    # rounding to the integer
    **t = round(t)
    print(str(t)+"C")**
```

**练习 6)计算质量、密度或体积**

```
**result = None**# user choose that he wants to calculate: mass(m), density(d) or 
# volume(v)
**flag = input("What to calculate? (m, d, v): ")**# if the mass is chosen, then you must ask for density and volume.
# calculate the mass by the formula m=dv.
**if flag == 'm':
    d = float(input("Density:")
    v = float(input("Volume:")
    result = d*v**# if density is selected, then the mass and volume are requested.   # The formula is d = m/v
**if flag == 'd':
    m = float(input("Mass:")
    v = float(input("Volume:")
    result = m/v**# if the volume is selected, the mass and density are read. The     # volume is found as v = m/d
**if flag == 'm':
    m = float(input("Mass:")    
    d = float(input("Density:")
    result = m/d**# regard less of the calculation branch, the result is written to   # the same 'result' variable. Formatted output with two decimal     # places
**print("%.2f"%result)**
```

**练习 7)点在哪个象限？**

```
**print("The coordinates of a point:")
x = float(input("x = "))
y = float(input("y = "))****if x>0 and y>0:
    print("The I quardrant")****elif x<0 and y>0:
    print("The II quardrant")****elif x<0 and y<0:
    print("The III quardrant")****elif x>0 and y<0:
    print("The IV quardrant")****elif x==0 and y==0:
    print("The point is at the origin of the coodinate system")****elif x==0:
    print("The point is on the x-axis")****elif y==0:
    print("The point is on the y-axis")**# Note: the sequence of check is not important, except for the      # following :
# The check of the equality of both coordinates to zero must 
# precede the checking for the equality to zero of only one
# coordinate.
```

**练习 8)有给定边的三角形吗？**

```
**print("Length of the sides")**
**a = float(input("a = "))
b = float(input("b = "))
c = float(input("c = "))**# A triangle exist only when the sum of any two of its sides is
# greater than the third party. In the if header, three sides are
# checked immediately. Each of them should be less than the sum 
# of the other. Therefore ,the logical AND operator is used.
**if a+b>c and a+c>b and b+c>a:
    print("The triangle exists")**# If at least one side turns out to be more than the sum of the                  # other ,then a triangle cannot be constructed from the given        # segments
**else:
    print("The triangle doesn't exists")**# Note: there is a notion of a degenerate triangle. In this case,    # the third party may equal the sum of the other two. In this case 
# in the condition instead of sign ">" you should use ">=".
```

**练习 9)判断一年是否是闰年**

```
# A year is entered and converted to an integer
**year = int(input())**# A year is a leap year if it is divided by 4\. However, centuries    # are not leap years. although they are divided by 4\. But centuries 
# that are divided into 400 are still leap years.
# If the remainder of the division by 4 is not zero, then the year 
# is not divided by 4, and therefore it is an usual.
**if year % 4 != 0:
    print("Usual year")**# Exclude century
# If a year is a century, it is divided 400?
**if year % 100 == 0:
    if year % 400 == 0:
        print("Leap year")**
    # If the century is not divisible by 400,
    **else:
        # The year is an usual.
        print("Usual year")**# In all other cases, the year is a leap.
**else:
    print("Leap year")**
```

**习题 10)该点是否属于圆心在原点的圆？**

```
**import math**# The coordinates of a point that is either inside the circle or not
**print("Point: ")
x = float(input("x = "))
y = float(input("y = "))**# The radius of a circle with the center at the origin
**print("Radius of circle: ")
r = float(input("r = "))**# It is necessary to calculate the length of the segment from the 
# origin to a given point. If this segment is not larger than the 
# radius of the circle then the point will belong to the circle# This segment is a hypotenuse of a right triangle; One leg of which
# is equal to the distance x, the second to the distance y. The 
# hypotenuse is found by the Pythagorean Theorem
**hyp = math.sqrt(x**2 + y**2)**# It hypotenuse is no longer than radius ...
**if hyp <= r:
    print("Point belongs to the circle.")**# Otherwise
**else:
    print("Point doesn't belongs to the circle.")**
```