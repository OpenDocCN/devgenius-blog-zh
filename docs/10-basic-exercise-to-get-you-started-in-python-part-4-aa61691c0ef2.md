# Python 入门的 10 个基本练习|第 4 部分

> 原文：<https://blog.devgenius.io/10-basic-exercise-to-get-you-started-in-python-part-4-aa61691c0ef2?source=collection_archive---------13----------------------->

这些练习是本博客第 1、2、3 部分的延续。如果您还没有阅读第 1 部分，下面是链接:-[https://medium . com/@ arbaazkan 96/10-basic-exercise-to-get-you-started-in-python-4d 0 aa 6a 3b 185](https://medium.com/@arbaazkan96/10-basic-exercise-to-get-you-started-in-python-4d0aa6a3b185)

Part-2 下面是链接:-[https://medium . com/@ arbaazkan 96/10-basic-exercise-to-get-you-started-in-python-part-2-5e 7 ddf 83020 b](https://medium.com/@arbaazkan96/10-basic-exercise-to-get-you-started-in-python-part-2-5e7ddf83020b)

Part-3 下面是链接:-[https://medium . com/@ arbaazkan 96/10-基础-锻炼-让你开始-用 python-part-3-eda19d76ff7a](https://medium.com/@arbaazkan96/10-basic-exercise-to-get-you-started-in-python-part-3-eda19d76ff7a)

现在让我们从练习开始

**练习 1)一个数的数字的和与积**

```
# An integer is entered. The program should calculate
# the product of its digit.**a = abs(int(input()))****suma = 0
mult = 1****while a > 0:
    digit = a % 10
    suma += digit
    mult *= digit
    a = a // 10****print("Sum : ", suma)
print("Product : ", mult)**
```

**练习 2)确定一个整数的位数**

```
# Calculate the number of digits in an integer entered 
# from the keyboard using an arithmetic method.**n = abs(int(input()))
count = 1
n //= 10****while n > 0:
    n //= 10
    count += 1****print(count)**
```

**练习 3)最简单的计算器**

```
# Write a program that perform a user-specified arithmetic operation
# (+,-,*,/) on a pair of keyboard-entered number. Loop the program 
# in such a way that entering the sign of the operation and numbers
# as well as calculations continue until the user himself wants to 
# stop the program.**print("Zero as an operation sign will terminate the program ")****while True:
    s = input("Sign (+,-,*,/): ")
    if s == '0':
        break
    if s in ('+', '-', '*', '/'):
        x = float(input("x = "))
        y = float(input("y = "))
        if s == '+': 
            print("%.2f"%(x + y))
        elif s == '-':
            print("%.2f"%(x - y))
        elif s == '*':
            print("%.2f"%(x * y))
        elif s == '/':
            if y != 0:
                print("%.2f"%(x / y))
            else:
                print("Division by zero!")
    else:
        print("Invalid operation sign")**
```

**练习 4)斐波那契数列**

```
# The number of elements in given. The program should display
# the elements of the Fibonacci series in the specified amount**n = int(input())
f1 = f2 = 1
print(f1, f2, end='')
i = 1****while i < n:
    f1, f2 = f2, f1+f2
    print(f2, end='')
    i += 1****print()**
```

**练习 5)求最大公约数(GCD)**

```
# Two natural numbers are entered. Find their greatest common 
# divisor using Euclid's algorithm.**a = int(input())
b = int(input())****while a != 0 and b != 0:
    if a > b:
        a %= b
    else:
        b %= a****gcd = a + b
print(gcd)**
```

**练习 6)一个数是质数还是复数？**

```
# A natural number is entered. The program must determine weather
# it is prime or composite.**import math****if n < 2:
    print("A number must be more then 1")
    quit()****if n == 2:
    print("It's prime number")
    quit()****i = 2
limit = int(math.sqrt(n))****while i <= limit:
    if n % i == 0:
        print("It's composite number")
    i += 1****print("It's prime number")** 
```

**练习 7)确定输入的质数的数量**

```
# The user enters natural numbers. The program should count how many
# Primes are among them.**from math import sqrt****count = 0
num = int(input())****while num >= 2:
    prime_flag = True
    for i in range(2, int(sqrt(num))+1):
        if num % i == 0:
            prime_flag = False
            break
    if prime_flag == True:
        count += 1
    num = int(input())****print("Quality of prime numbers: ", count)** 
```

【练习 8】找出在指定系数范围内有解的二次方程

```
# Integer ranges are entered for the coefficients "a", "b" and "c"
# of the quadratic equations. Go through all possible combinations
# of coefficients and determine for which of them the quadratic 
# equation has roots and for which it does not. If the equation has
# roots, calculate them.**from math import sqrt****a1 = int(input('a1: ')
a2 = int(input('a2: ')
b1 = int(input('b1: ')
b2 = int(input('b2: ')
c1 = int(input('c1: ')
c2 = int(input('c2: ')
a = range(a1, a2 + 1)
b = range(b1, b2 + 1)
c = range(c1, c2 + 1)****for i in a:
    if i == 0:
        continue
    for j in b:
        for k in c:
            print(i, j, k, end='')
            D = j*j - 4*i*k
            if D >= 0:
                x1 = (-j - sqrt(D))/(2*i)
                x2 = (-j + sqrt(D))/(2*i)
                x1 = round(x1,2)
                x2 = round(x2,2)
                print('Yes',x1,x2)
            else:
                print('No')**
```

**练习 9)将整数的位数顺序颠倒过来**

```
# Using arithmetic operations, reverse the order of the digit of the 
# number**n1 = int(input("An integer: "))
n2 = 0****while n1 > 0:
    digit = n1 % 10
    n1 = n1 // 10
    n2 = n2 * 10
    n3 = n2 + digit****print("Inverse number : ", n2)**
```

**练习 10)计算一个随机三位数的数字之和**

```
# Write a program that generates a random three digit number and 
# calculates the sum of its digits.**from random import randint** **n = randint(100,999)****print(n)****s = str(n)
a = int(s[0])
b = int(s[1])
c = int(s[2])****print(a+b+c)**
```