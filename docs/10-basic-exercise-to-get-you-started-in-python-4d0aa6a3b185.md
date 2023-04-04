# Python 入门的 10 个基本练习|第 1 部分

> 原文：<https://blog.devgenius.io/10-basic-exercise-to-get-you-started-in-python-4d0aa6a3b185?source=collection_archive---------8----------------------->

这些练习将帮助你增强你代码，如何在你的代码中使用基础数学，学习内置函数，库和模块

**练习 1)计算行星上的年数**

```
# TODO: import the constant pi from the maths module
from math import pir = input('Radius of the orbit (million Km): ')
v = input('Orbital speed (km/s): ')# TODO: strings are converted to float numbers.
r = float(r)
v = float(v)# TODO: translate million of kilometer into just kilometer
r = r * 1000000# TODO: year, expressed in seconds. the length of 
# circumference is being found (2 * Pi * r),
# that is the orbit , and is divided by the speed
year = 2 * pi * r/v# TODO: To translate seconds into days, you need 
# to divide by 60 and get the minutes,
# then divide by 60 and get the hours,
# then divide by 24 and get the days.
year = year/(60 * 60 * 24)print(round(year))
```

**练习 2)计算随机三位数的和**

```
# TODO: import the constant random() from the 
# random module. The random() function generates
# a random fractional number from 0 to 1
from random import random# TODO: When the number is multiplied by 900
# a random number is obtained
# from 0 to 899.(9).
# if you add 100 to it,
# you get a number from 100 to 999.(9).
n = random() * 900 + 100# TODO: the fraction part is discarded,
# the number is displayed.
a = n//100# TODO: the last digit of the number
# is deleted by dividing by 10 whole.
# Then finding the remainder when
# dividing by 10 extracts the last digit
# which in the original number
# was in the middle.
b = (n//10) % 10# TODO: The last digit (the lowest digit)
# of the number is extracted
# by finding the remainder 
# by division by 10
c = n % 10# TODO: The sum of digits is calculated
# and displayed on the screen.
print(a+b+c)
```

**练习 3)求直角三角形的面积和周长**

```
# TODO: import the math module 
# The math module containing various math 
# formulas in form of function  
import math# TODO: the input() function returns a string
AB = input("length of the first leg: ")
AC = input("length of the second leg: ")# TODO: strings are converted to real number with the 
# help of float() function.
AB = float(AB)
AC = float(AC)# TODO: Find the hypotenuse
# by the Pythagorean theorem:
# "the sum of the squares of the legs is
# equal to the square of the hypotenuse."
# The sqrt() function of the math module
# extract a square root.
# Operation ** raises a number to a power.
BC = math.sqrt(AB**2 + AC**2)# TODO: The area of a right angle triangle is equal to 
# half the area of the corresponding rectangle.
S = (AB*AC)/2# TODO: Perimeter is the sum of all sides.
P = AB + AC + BCprint("Area of the triangle: %.2f" %S)
print("Perimeter of the triangle: %.2f" %P)
```

**练习 4)圆柱体的总表面积。**

```
# TODO: Import the constant pi from the math module.
from math import pi# TODO: discribe the cylinder height input string and 
# convert it to wholenumber (float).
h = float(input('H = '))# TODO: discribe the cylinder base radius input string and 
# convert it to wholenumber (float).
r = float(input('R = '))# TODO: A cylinder has two bases. Area of each 
# is Pi * R * R.
circles = 2*(pi * r ** 2)# TODO: The area of the side surface of a cylinder 
# is 2 * Pi * R * H.
side = 2 * pi * r * h# TODO: Total surface area.
area = circles + side
print('A = ',round(area,2))
```

**习题 5)圆的周长和面积。**

```
# TODO: The math module contains the pow() function 
# and the constant pi.
import mathr = input('Radius = ')# TODO: The input string is converted to float number.
r = float(r)# TODO: The length of circumference is 2 * Pi * R.
ln = 2 * math.pi * r# TODO: The area of the circle is Pi * R * R.
area = math.pi * math.pow(r,2)# Note: In Python you can raise to a power 
# using the ** operator instead of pow()
# Then:
# area = math.pi * r ** 2
print('Length = %.2f'%ln)
print('Area = %.2f'%area)
```

**练习 6)求过两个已知点的直线(y = kx + b)的方程**

```
# TODO: To find the equation of a straight 
# line (y = kx + b) that passes through two
# known points, it is necessary to solve the
# system of equation {y1 = kx1 + b and y2 = kx2 + b},
# where x1 ,y1 ,x2 ,y2 are known variables, k and b 
# are coefficients to be found. From this system
# of equations derive the coefficients b and k:# TODO: The second equation is converted to the
# form (b = y2 - kx2).After it the value of b 
# is substituted into the first equation and get 
# k = (y1 - y2)/(x1 - x2). At the end k and b
# are substituted, into the equation y = kx + b
# and get the equation of a perticular lineprint('A(x1 : y1):')
x1 = float(input('\tx1 = '))
y1 = float(input('\ty1 = '))
print('A(x1 : y1):')
x2 = float(input('\tx2 = '))
y2 = float(input('\ty2 = '))
print('Equation:')
k = (y1 - y2) / (x1 - x2)
b = y2 - k + x2
print('\ty = %.2f * x + %.2f'%(k, b))
```

**练习 7)把数字表示为符号。求差&和**

```
n1 = '3'
n2 = '8'# TODO: The ord() function returns the character
# number in the symbol table.
a = ord(n1)
b = ord(n2)# TODO: symbol of the digits follow one another
# in the table. Therefore, the difference
# of their codes will be the same as the
# difference of digits
diff = a-b# TODO: If we subtract the code of zero from the
# code of digit, we get the number corresponding
# to the digit character.
a = a - ord('0')
b = b - ord('0')# TODO: After that you simply find the sum of the number.
suma = a + b
print("%s - %s = %d" %(n1, n2, diff))
print("%s - %s = %d" %(n1, n2, suma))
```

**练习 8)确定字母表中的字母数。**

```
# TODO: Any lowercase letter should be entered.
c = input("Letter(a-z): ")# TODO: The ord() function returns the ordinal number
# of the character in the symbol table.
c = ord(c)# TODO: The code of the first letter of the alphabet.
a = ord('a')# TODO: Since the letters follow in order, by subracting
# the character codes, we find the distance between them.
n = c - a# TODO: Since it is necessary to find not the distance,
# but the ordinal number of the letter in the alphabet,
# we add 1.
n = n + 1
print("Its number is %d"%n)
```

**练习 9)获取给定范围内的随机整数和浮点数**

```
# TODO: From the random module, we import the random()
# function which generates a random float number,
# and the randint() function which generates a 
# random integer.
from random import random, randint# TODO: we request the lower and the upper limits of the
# range, within these limits a random integer will 
# be generated.
print("Range of integer: ")
imin = int(input())
imax = int(input())# TODO: The randint() function generates a random number 
# n that is not less than imin and more than imax.
n = randint(imin, imax)
print("%d"%n)# TODO: we request the lower and upper limit of the range,
# within these limits a random float will be generated.
print("Range of floats: ")
fmin = int(input())
fmax = int(input())# TODO: The random() function generate a float number from 
# zero to one. The 1 is not in the range
n = random()# TODO: Multiply the number by the length of the range.
# For example if fmax = 5.6, fmin = 3.2, than we get
# a random number from to 2.4 .
n = n*(fmax - fmin)# TODO: Shift the lower limit of the number by the value of 
# fmin. Thus, the random number will be in the range
# from 3.2 to 5.6
n = n + fmin
print("%.3f"%n)
```

**练习 10)位运算**

```
# TODO: First we will create 2 input strings using input()
# function
n1 = input("Binary n1: ")
n2 = input("Binary n2: ")# TODO: Strings n1 and n2 are converted to decimal number,
# since bitwise operations can be performed only with
# numbers. while the binary representation of a number
# is a string
n1 = input(n1, 2)
n2 = input(n2, 2)# TODO: In computer memory, binary operations are performed
# on bits of number, although we specify decimal number 
# as operands. Also, the result is a decimal number.
bit_or = n1 | n2
bit_and = n1 & n2
bit_xor = n1 ^ n2# TODO: The bin() function converts a decimal number to a
# binary number, which is a string, the first two 
# character of which are '0b'.
bit_or = bin(bit_or)
bit_and = bin(bit_and)
bit_xor = bin(bit_xor)
print("n1: %10s"%bin(n1))
print("n2: %10s"%bin(n2))
print("  --------------")
print(" OR: %10"%bit_or)
print("AND: %10"%bit_and)
print("XOR: %10"%bit_xor)
```

这些是基本的练习。按照你的视角去实践，去理解，去修改。谢谢你，我会带着这个博客的第二部分回来，直到那时保持乐观。