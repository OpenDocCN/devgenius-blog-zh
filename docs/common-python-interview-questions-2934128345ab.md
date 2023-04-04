# 常见的 Python 面试问题

> 原文：<https://blog.devgenius.io/common-python-interview-questions-2934128345ab?source=collection_archive---------8----------------------->

通过这些问题让你的技术面试过程变得顺利

![](img/8ef3a92b8019c88ebdc36393eef0d6e1.png)

由 [Van Tay Media](https://unsplash.com/@vantaymedia?utm_source=medium&utm_medium=referral) 在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄的照片

真正的学习是在我们处于通知期的时候。我们开始争取最好的结果。这是你开始知道你需要多少改进的时候了。记住你会成功的，这只是时间问题。有些人来得早，有些人来得晚，但成功的保证是为每个人准备的。所以继续学习和战斗吧👏。

我们不要多谈了，进入我们真正的议程吧。我将列出所有的 Python🐍SDE 这个角色的面试问题。问题分为 4 个部分，每次将涵盖每个部分。部分是—

1.  基于理论的
2.  基于编码
3.  基于模式的
4.  基于 Web 框架(Django)

# **理论基础题**

1.  Python 是编程语言还是脚本语言？
2.  Python 是一种怎样的解释语言？
3.  PEP8 是什么？
4.  Python 有什么特点？
5.  什么是虚拟环境？
6.  你为什么选择 Python？
7.  Python 中如何管理内存？
8.  Python 中的动态类型强制转换是什么？
9.  Python 中的命名空间是什么？
10.  Python 区分大小写吗？
11.  什么是`is`运算符？
12.  Python 中的`None`是什么？
13.  Python 中的`pass`是什么？
14.  什么是 lambda 函数？
15.  什么是迭代器？(问得最多的)
16.  Python 中的生成器是什么？(问得最多的)
17.  Python 迭代器和生成器有什么区别？(问得最多的)
18.  Python 中的 decorator 是什么？(问得最多的)
19.  Python 中的数据类型有哪些？
20.  什么是列表？
21.  什么是元组？
22.  列表和元组的区别？(问得最多的)
23.  什么是列表理解？(问得最多的)
24.  什么是词典理解？
25.  Python 中的字符串是什么？(问得最多的)
26.  如何删除字符串中的空格？
27.  序列和列表的区别是什么？
28.  Python 中的 string 表示列表吗？
29.  什么是可变性？
30.  字符串是可变对象吗？(问得最多的)
31.  列表和元组哪个更好，为什么？(问得最多的)
32.  Python 中的模块是什么？(问得最多的)
33.  Python 中的包是什么？
34.  __ 名 _ _ 是什么？
35.  什么是集合？
36.  套和冻套有什么区别？
37.  什么是`self`？(问得最多的)
38.  什么是`__init__`法？(问得最多的)
39.  `__init__`方法在 Python 中充当构造函数吗？(问得最多的)
40.  什么是`__new__`法？
41.  静态方法是什么？
42.  静态方法和类方法的区别。
43.  实例方法和类方法的区别。
44.  方法和函数的区别。
45.  什么是抽象类？(问得最多的)
46.  我们能实例化一个抽象类吗？
47.  什么是抽象方法？
48.  Python 类的 SOLID 属性是什么？
49.  Python 中的 MRO 是什么？
50.  Python 中有哪些继承？(问得最多的)
51.  Python 支持多重继承吗？(问得最多的)
52.  什么是 Python 中的重载或者运算符重载？
53.  Python 中的`super()`方法是什么？
54.  怎样才能在子类内部调用父类方法？
55.  什么是类属性？
56.  类属性和对象属性有什么区别？
57.  我们可以修改对象属性吗？
58.  Python 中的垃圾收集器是什么？
59.  Python 中的多线程是什么？
60.  Python 中的多重处理是什么？
61.  什么是上下文管理器？
62.  什么是环境变量？
63.  浅拷贝和深拷贝有什么区别？(问得最多的)

# 基于编码

注意——一个编程问题可以有多种解决方法。

1.  检查这个数是否是质数

```
*# method 1
from math import *
def check_prime(num):
    for i in range(2, int(sqrt(num)) + 1):
        if num % i == 0:
            return False
    return True

print(check_prime(6))

# method 2
def check_prime(num):
    for i in range(2, int(num / 2) + 1):
        if num % i == 0:
            return False
    return True

print(check_prime(4))*
```

2.打印给定范围内的所有质数。

```
from math import *# method 1
def prime_in_range(limit):
    prime_list = []
    if limit > 2:
        for i in range(2, limit):
            is_prime = True
            for j in range(2, int(sqrt(i)) + 1):
                if i % j == 0:
                    is_prime = False
                    break
            if is_prime:
                prime_list.append(i)

    return prime_list
print(prime_in_range(50))

# method 2

def number_of_prime(limit):
    count = 1
    prime_start = 2
    while count <= limit:
        is_prime = True
        for i in range(2, int(sqrt(prime_start)) + 1):
            if prime_start % i == 0:
                is_prime = False
                break
        if is_prime:
            yield prime_start
            count += 1
        prime_start += 1

prime_list = list(number_of_prime(10))
print(prime_list)
```

3.检查给定的年份是否是闰年。

```
def check_leap_year(year):
    if year % 4 == 0 and year % 100 != 0:
        return True
    elif year % 400 == 0 and year % 100 == 0:
        return True
    return False

print(check_leap_year(2017))
```

4.不使用加号(+)运算符将两个数相加。

```
def add_number(a, b):
    if b == 0:
        return a
    else:
        return add_number(a ^ b, (a & b) << 1)

print(add_number(1, 2))
```

5.检查给定的字符串是否是回文。

```
# method 1
def check_palindrome(string):
    return string == "".join(reversed(string))

print(check_palindrome('abaa'))

# method 2

def check_palindrome(string):
    return string == string[::-1]

print(check_palindrome('aba'))

# method 3
def check_palindrome(string):
    for i in range(int(len(string)/2)):
        if string[i] != string[len(string)-1-i]:
            return False
    return True

print(check_palindrome("aabaa"))
```

6.创建斐波那契数列

```
# method 1
def fibonacci():
    a, b = 0, 1
    while True:
        yield a
        a, b = b, a + b

f = fibonacci()
fibo_series = [next(f) for ele in range(10)]
print(fibo_series)# method 2
def fibonacci(limit):
    if limit == 0:
        return 0
    elif limit == 1:
        return 1
    else:
        return fibonacci(limit-1) + fibonacci(limit-2)

for i in range(10):
    print(fibonacci(i))# method 3
def fibonacci(limit):
    a, b = 0, 1
    fibo_series = []
    for i in range(limit):
        fibo_series.append(a)
        a, b = b, a + b
    return fibo_series

f = fibonacci(10)
print(f)
```

7.找出两个字符串是否是变位词。

```
# method 1
def anagram_check(string1, string2):
    '''
    Time Complexity: O(nlogn)
    Auxiliary Space: O(1)

    :param string1:
    :param string2:
    :return:
    '''
    print(sorted(string1))  # ['e', 'h', 'l', 'l', 'o']
    return sorted(string1) == sorted(string2)

print(anagram_check('hello', 'holle'))# method 2from collections import Counter
def anagram_check(string1, string2):
    '''
    Time Complexity: O(n)
    Auxiliary Space: O(1)
    :param string1:
    :param string2:
    :return:
    '''
    return Counter(string1) == Counter(string2)

print(anagram_check('hello', 'holle'))
```

8.在数组中找出总和等于给定数字的可能的元素对。

```
# method 1
def sum_of_two(arr, value):
    list_p = []
    for val in arr:
        rem = value - val
        if rem in arr and arr.index(val) < arr.index(rem):
            list_p.append((arr.index(val),arr.index(rem)))
    print(list_p)

sum_of_two([1,2,3,4, 5, 7,8,9,11], 10)

# method 2
def sum_of_two(arr, value):
    list_p = {}

    for i in range(len(arr)):
        rem = value-arr[i]
        if rem in list_p:
            print(rem,arr[i])
        list_p[arr[i]] = i    

sum_of_two([1,2,3,4,7,5,8,9,11], 10)# method 3def sum_of_two(arr, value):
    set_p = set()

    for i in range(len(arr)):
        rem = value - arr[i]
        if rem in set_p:
            print(rem, arr[i])
        else:
            set_p.add(arr[i])

sum_of_two([1, 2, 3, 4, 7, 5, 8, 9, 11], 10)
```

9.在主函数执行前后打印一些东西

```
def my_decorator(fun):
    def wrap():
        print("Hello")
        f = fun()
        print("Welcome")

    return wrap

@my_decorator
def print_my_name():
    print('Python')

print_my_name()
```

10.使用装饰器计算执行时间。

```
import time

def find_time(fun):
    def wrap(*args, **kwargs):
        start = time.time()
        fun(*args, **kwargs)
        end = time.time()
        print(start, end)
        return fun(*args, **kwargs)

    return wrap

@find_time
def sum_of_two(arr, value):
    temp = {}

    for i in range(1, len(arr) + 1):
        time.sleep(1)
        rem = value - arr[i]

        if i in temp:
            return i, temp.get(i)
        temp[arr[i]] = i

print(sum_of_two([1, 3, 5, 7, 11], 10))
```

11.检查给定的数字是否是正方

```
from math import *# method 1
def check_perfect_square(num):
    number_square_root = sqrt(num)
    return num == (number_square_root * number_square_root)

print(check_perfect_square(3))# method 2
def check_perfect_square(num):
    for i in range(num+1):
        if i*i == num:
            return True
    return  False

print(check_perfect_square(4))# method 3
def check_perfect_square(num):
    i = 1
    while (i * i) <= num:

        if num % i == 0 and (num / i == i):
            return True
        i += 1
    return False

print(check_perfect_square(9))# method 4
def check_perfect_square(num):
    number_square_root = num ** (1 / 2)
    return num == (number_square_root * number_square_root)

print(check_perfect_square(4))
```

12.检查给定的数字是否是完美的立方体。

```
# method 1
def check_perfect_cube(num):
    cube_root = round(num ** (1 / 3))
    return num == (cube_root * cube_root * cube_root)

print(check_perfect_cube(9))# method 2
def perfect_cube_in_range(a, b):
    perfect_cube_list = []

    for i in range(a, b + 1):
        cube_root = round(i ** (1 / 3))

        if i == (cube_root * cube_root * cube_root):
            perfect_cube_list.append(i)

    return perfect_cube_list

print(perfect_cube_in_range(1, 100))# method 3
def check_perfect_cube(num):
    for i in range(num + 1):
        cube = i * i * i
        if cube == num:
            return True
    return False

print(check_perfect_cube(64))
```

13.复杂度为 O(1)的前 n 个奇数之和。

```
def sum_of_first_n_odd(n):
    return n * n

print(sum_of_first_n_odd(20))
```

14.反转一个数字的位数。

```
# method 1
def reverse_number(num):
    r = reversed(str(num))
    return int("".join(r))

print(reverse_number(234))# method 2
def reverse_number(num):
    r = 0
    while (num):
        rem = num % 10
        r = r * 10 + rem
        num = num // 10
    return r

print(reverse_number(234))
```

15.使用递归检查回文。

```
# method 1
def palindrom(num):
    if num < 10:
        return num
    return int(str((num % 10)) + str(palindrom(num // 10)))

print(palindrom(111) == 111)
```

16.三个中的最大数量

```
def greatest(a, b, c):
    return max(a, b, c)

print(greatest(1, 2, 4))
```

17.检查给定的数字是否是二进制的。

```
# method 1
def is_binary(num):
    while num:
        rem = num % 10
        if rem not in [0, 1]:
            return False
        num //= 10
    return True

print(is_binary(1024))# method 2
def is_binary(num):
    l = list(str(num))
    return all(int(ele) == 0 or int(ele) == 1 for ele in l)print(is_binary(10))
```

18.用递归求出一个数的数字之和。

```
def sum_of_digit(num):
    if num < 10:
        return num

    return (num % 10) + sum_of_digit(num // 10)

print(sum_of_digit(101))
```

19.求一个数的质因数。

```
# method 1
import math
def prime_factors(num):
    while num % 2 == 0:
        print(2)
        num //= 2

    for i in range(3, int(math.sqrt(num)) + 1, 2):

        while num % i == 0:
            print(i)

            num //= i
    if num > 2:
        print(num)

prime_factors(315)# method 2
def prime_factors(n):
    c = 2
    while n > 1:

        if n % c == 0:
            print(c)
            n //= c
        else:
            c += 1

prime_factors(315)
```

20.检查给定的数是否是完全数。

```
def perfect_number(num):
    s = 0
    for i in range(1, num - 1):
        if num % i == 0:
            s += i
    return s == num

print(perfect_number(7))
```

21.求一个数的阶乘

```
# method 1
def fact(num):
    c = 1
    while num > 0:
        c *= num
        num -= 1
    return c

print(fact(5))# method 2
def fact(num):
    if num < 1:
        return 1

    return num * fact(num - 1)

print(fact(5)) 
```

22.计算一个数的幂

```
# method 1
def cal_power(n, m):
    power = 1

    for i in range(m):
        power *= n
    return power

print(cal_power(2, 3))# method 2
def cal_power(n, m):
    if m == 0:
        return 1
    return n*cal_power(n,m-1)print(cal_power(3,3))
```

23.求两个数的 LCM。

```
def lcm(a, b):
    greater = max(a, b)

    while True:
        if greater % a == 0 and greater % b == 0:
            return greater
        greater += 1

print(lcm(4, 6))
```

24.计算两个数的 GCD

```
def gcd(a, b):
    lowest = min(a, b)

    while True:
        if a % lowest == 0 and b % lowest == 0:
            return lowest
        lowest -= 1

print(gcd(4, 6))
```

25.将十进制数转换成二进制数。

```
def binary(num):
    b = ''
    while num > 0:
        rem = num % 2
        b = str(rem) + str(b)
        num //= 2
    return b
```

26.将十进制数转换成八进制数。

```
def octal(num):
    b = ''
    while num > 0:
        rem = num % 8
        b = str(rem) + str(b)
        num //= 8
    return b

print(octal(64))
```

27.从字符串中移除所有给定的字符。

```
def remove_char(string,c):
    return string.replace(c,'')

print(remove_char('Hello','l'))
```

28.从字符串中移除给定字符的第一个匹配项。

```
def remove_char(string,c):
    return string.replace(c,'',1)
print(remove_char('Hello','l'))
```

29.计算给定字符串中给定字符的出现次数。

```
def count_char(string, c):
    return string.count(c)

print(count_char('Hello', 'l'))
```

30.计算给定字符串中出现的所有字符。

```
# method 1
from collections import Counter

def count_char(string):
    return Counter(string)

print(count_char('Hello'))# method 2
def count_char(string):
    t = {}
    for i in string:
        if i in t:
            t[i] += 1
        else:
            t[i] = 1
    return tprint(count_char('Hello'))
```

31.检查给定字符是否为数字。

```
# method 1
def is_digit(c):
    return c.isnumeric()

print(is_digit('2'))

# method 2
def is_digit(c):
    return c.isdigit()

print(is_digit('c'))
```

32.将小写元音转换成大写。

```
def convert_upper(string):
    vowel_list = ['a', 'e', 'i', 'o', 'u']
    l = map(lambda x: x.upper() if x in vowel_list else x, string)
    return "".join(l)

print(convert_upper("Welcome to Python String"))
```

33.删除给定字符串中的元音。

```
def remove_vowel(string):
    vowel_list = ['a', 'e', 'i', 'o', 'u']
    l = filter(lambda x: x if x not in vowel_list else "", string)
    return "".join(l)

print(remove_vowel("Welcome to Python String"))
```

34.打印频率最高的字符。

```
# method 1
def find_highest_freq(string):
    max_count = 0
    max_char = None
    temp = {}

    for c in string:
        if temp.get(c):
            temp[c] += 1
            if max_count < temp[c]:
                max_count = temp[c]
                max_char = c
        else:
            temp[c] = 1
            if max_count < temp[c]:
                max_count = temp[c]
                max_char = c

    return max_char

print(find_highest_freq("Hello"))

# method 2

from collections import Counter
def find_highest_freq(string):
    c = dict(Counter(string))
    c = sorted(c.items(),key=lambda x:x[1])
    return c[-1]

print(find_highest_freq("Hellooaso"))
```

35.将所有出现的第一个元音替换为'-'

```
def replace_all_vowel(string):
    first_vowel = None
    vowel_list = ['a', 'e', 'i', 'o', 'u']
    for c in string:
        if c.lower() in vowel_list:
            first_vowel = c
            break
    return string.replace(c, '-')

print(replace_all_vowel("Hellooaso"))
```

36.用'-'替换第一个元音字母的第一个出现

```
def replace_first_vowel(string):
    first_vowel = None

    for c in string:
        if c.lower() in ['a', 'e', 'i', 'o', 'u']:
            first_vowel = c
            break
    return string.replace(c, '-', 1)

print(replace_first_vowel("Hello"))
```

37.从给定的字符串中删除空格

```
def remove_space(string):
    return string.replace(" ", "")

print(remove_space("  Hello welcome to string  "))
```

38.连接两根弦

```
# method 1
def join_str(str1, str2):
    return "".join([str1, str2])print(join_str("Hello", "Mello"))# method 2
def join_str(str1, str2):
    return str1 + str2

print(join_str("Hello", "Mello"))
```

39.从给定的字符串中删除重复的字符。

```
def remove_repeated(string):
    return "".join(list(set(string)))

print(remove_repeated("Hello"))
```

40.从给定的字符串中删除重复的字符，保持顺序。

```
# method 1
def remove_repeated(string):
    temp = {}
    l = list(string)
    for i in range(len(string)):
        if temp.get(string[i], None):
            del l[i]
        else:
            temp[string[i]] = 1
    return "".join(l)

print(remove_repeated("Hello"))# method 2
def remove_repeated_char(string):
    t = []
    for i in string:

        if i not in t:
            t.append(i)

    print("".join(t))

remove_repeated_char("hellomel")
```

41.计算给定字符串中整数的和。

```
def cal_sum(string):
    l = [int(c) for c in string if c.isdigit()]
    return sum(l)

print(cal_sum("1string@123"))
```

42.打印给定字符串中所有不重复的字符

```
# method 1
from collections import Counter
def non_repeating(string):
    counting = dict(Counter(string))
    for key, value in counting.items():
        if value == 1:
            print(key)

non_repeating("Hello")

# method 2
def non_repeating(string):

    for c in string:
        if string.count(c) == 1:
            print(c)

non_repeating("Hell0")

# method 3
def non_repeating(string):
    temp = {}
    l = string

    for c in string:
        if temp.get(c):
            l = l.replace(c,"")
        else:
            temp[c] = 1
    return l

print(non_repeating("Hello"))
```

43.对字符串排序

```
def sort_char(string):
    s = sorted(string)
    print("".join(s))

sort_char("Hello")

# using sort
def sort_char(string):
    l = list(string)
    l.sort()
    print("".join(l))

sort_char("Hello")
```

44.以逆序对字符串进行排序

```
def sort_char(string):
    l = sorted(string, reverse=True)
    print("".join(l))

sort_char("Hello")
```

45.展平给定列表

```
import itertools

l = [[1,2,4],[3,4],[6,4]]

l = itertools.chain(*l)
print(list(l))
```

46.从 n 个连续数字的列表中找出缺失的数字。

```
def missing(arr):
    arr_sum = sum(arr)
    n = len(arr) + 1
    return int((n * (n + 1) / 2) - arr_sum)

arr = [i for i in range(1, 101) if i != 49]
print(missing(arr))
```

47.找到列表中第二高的元素。

```
def second_highest_number(arr):
    sorted_arr = sorted(arr)

    for i in range(len(arr) - 1, 0, -1):
        if sorted_arr[i] != sorted_arr[i - 1]:
            print(sorted_arr[i - 1])
            break

arr = [1, 3, 4, 6, 6, 7, 8, 8]
second_highest_number(arr)
```

48.从给定列表中找出前两个元素。

```
def top_two_number(arr):
    sorted_arr = sorted(arr)

    for i in range(len(arr) - 1, 0, -1):
        if sorted_arr[i] != sorted_arr[i - 1]:
            print(sorted_arr[i],sorted_arr[i - 1])
            break

arr = [1, 3, 4, 6, 7, 8]
top_two_number(arr)
```

49.将列表向左旋转 2 个位置。

```
# method 1
def left_rotate(arr):
    count = 1
    while count <= 1:
        for i in range(len(arr) - 1):
            arr[i], arr[i - 1] = arr[i - 1], arr[i]
        count += 1
    print(arr)

arr = [1, 2, 3, 4, 5, 6]
left_rotate(arr)

# method 2
def left_rotate(arr):

    arr.append(arr.pop(0))
    print(arr)

arr = [1,2,3,4,5,6]
left_rotate(arr)

# method 3
from collections import deque
def left_rotate(arr):

    item = deque(arr)
    item.rotate(-1)
    print(item)

arr = [1,2,3,4,5,6]
left_rotate(arr)
```

50.将列表向右旋转 2 个位置。

```
# method 1
def right_rotate(arr):
    arr.insert(0, arr.pop())
    print(arr)arr = [1, 2, 3, 4, 5, 6]
right_rotate(arr)

# method 2
from collections import deque
def right_rotate(arr):
    item = deque(arr)
    item.rotate(1)
    print(item)

arr = [1,2,3,4,5,6]
right_rotate(arr)
```

51.使用递归计算两个数的和。

```
def sum_by_recursion(a, b):
    if a == 0:
        return b
    if b == 0:
        return a
    else:
        return sum_by_recursion(a + 1, b - 1) if b <= a else sum_by_recursion(a - 1, b + 1)

print(sum_by_recursion(5, 5))
```

52.求连续数的子表。

```
def sub_list(arr):
    count = None
    prev = 0
    item = []

    for i in range(len(arr) - 1):
        diff = abs(arr[i] - arr[i + 1])
        if diff > 1:
            item.append(arr[prev:i + 1])
            prev = i + 1
    if len(arr[prev + 1:len(arr)]):
        item.append(arr[prev:len(arr)])
    print(item)

arr = [1,2,3,7,6,5,10,11]
sub_list(arr)
```

53.从列表中删除重复的元素。

```
# method 1
arr = [1,2,3,4,5,6,7,7,7]
def remove_duplicate(arr):
    t = []
    for i in arr:
        if i not in t:
            t.append(i)
    print(t)

remove_duplicate(arr)# method 2
from collections import OrderedDict
arr = [1,2,3,4,5,6,7,7,7]

def remove_duplicate(arr):
    l = list(OrderedDict.fromkeys(arr))
    print(l)

remove_duplicate(arr)
```

54.以逆序找出给定列表中 4 个元素的所有子列表。

```
# method 1
import numpy as np
def make_sublist(arr,num):
    l = np.array_split(arr,num-1)
    p = [list(e)[::-1] for e in l]
    return p 

arr = [1,2,3,4,5,6,7,8,9,10,11]
print(make_sublist(arr,4))# method 2
arr = [1, 2, 3, 4, 5, 6, 7, 8, 9, 10, 11]
def sub_list(arr, n, k):
    c = 0
    l = k - 1

    for i in range(k - 1, n, k):
        for j in range(int(k / 2)):
            arr[c], arr[l] = arr[l], arr[c]
            c += 1
            l -= 1
        c = i + 1
        if c + k < n:
            l = c + k - 1
        else:
            l = n - 1
    rem = int((l - c) / 2) + 1
    if c < n:
        for i in range(rem):
            arr[c], arr[l] = arr[l], arr[c]
            c += 1
            l -= 1
    print(arr)

sub_list(arr, len(arr), 4)# method 3
arr = [1, 2, 3, 4, 5, 6, 7, 8, 9, 10, 11]
k = 4
def sublist(arr, n, k):
    c = 0
    for i in range(k - 1, len(arr), k):
        arr[c:i + 1] = arr[c:i + 1][::-1]
        c = i + 1
    if c < len(arr):
        arr[c:len(arr)] = arr[c:len(arr)][::-1]
    print(arr)

sublist(arr, len(arr), k)
```

55.找到给定列表中的峰值元素。

```
# a peak element is the one which is not smaller than its adjacent
# elementdef peak(arr, n):
    for i in range(len(arr)):

        if i != len(arr) - 1:
            if arr[i] > arr[+1]:
                return i
        else:
            if arr[i] > arr[i - 1]:
                return i

n = int(input("Enter the value: "))
arr = []
for i in range(n):
    p = int(input(""))
    arr.append(p)

print(peak(arr, n))
```

56.找出元素总和等于给定数字的子阵列。

```
# method 1
def sub_array(arr, value):
    for i in range(len(arr)):
        current_sum = arr[i]
        for j in range(i + 1, len(arr)):
            if current_sum == value:
                print(i, j - 1)
                return
            if current_sum > value:
                break

            current_sum += arr[j]
arr = [1, 4, 20, 3, 10, 5]
sub_array(arr, 33)# method 2
def sub_array(arr, value, n):
    current_sum = arr[0]
    start_index = 0
    i = 1
    while i <= n:
        while current_sum > value and start_index < i - 1:
            current_sum -= arr[start_index]
            start_index += 1
        if current_sum == value:
            print(start_index, i - 1)
            break
        if i < n:
            current_sum += arr[i]
        i += 1

arr = [15, 2, 4, 8, 9, 5, 10, 23]
sub_array(arr, 23, len(arr))
```

57.将给定列表中的所有负元素移到一边。

```
# method 1
def move_negative(arr, n):
    l = sorted(arr, reverse=True)
    print(l)

arr = [1, 2, -1, -2, 3, 4, -3, -4]
move_negative(arr, len(arr))# method 2
def move_negative(arr, n):
    i = 0
    while i < n:
        if arr[i] < 0:
            arr.insert(0, arr.pop(i))
        i += 1
    print(arr)

arr = [1, 2, 4, -1, -2, 5, 6, -8, 4]
move_negative(arr, len(arr))
```

58.两个列表的并集和交集

```
def union_and_intersection(arr1, arr2):
    arr1 = set(arr1)
    arr2 = set(arr2)
    print("Union of list - ", list(arr1.union(arr2)))
    print("Intersection of list- ", list(arr1.intersection(arr2)))

arr1 = [1, 2, 4, 5, 6, 7]
arr2 = [5, 6, 7, 8, 9, 10]
union_and_intersection(arr1, arr2)
```

59.在 3 个排序列表中找到公共元素。

```
def common(arr1, arr2, arr3):
    return list(set(arr1).intersection(set(arr2), set(arr3)))

arr1 = [1, 2, 3, 4, 5]
arr2 = [4, 5, 6, 7, 8]
arr3 = [5, 6, 7, 8, 9]

print(common(arr1, arr2, arr3))
```

60.找出 0 和 1 个数相等的子数组的索引。

```
def find_subarray(arr):
    for i in range(len(arr)):
        z = o = 0
        if arr[i] == 0:
            z += 1
        if arr[i] == 1:
            o += 1
        for j in range(i + 1, len(arr)):
            if arr[j] == 0:
                z += 1
            if arr[j] == 1:
                o += 1

            if z == o:
                print(i, j)

arr = [1, 0, 0, 1, 0, 1, 1]
find_subarray(arr)
```

61.在给定的列表中排列交替的正负元素。

```
# method 1
def arrange(arr):
    p = []
    n = []
    for i in arr:
        if i < 0:
            n.append(i)
        else:
            p.append(i)
    arr.clear()
    for i in range(len(p)):
        arr.append(p[i])
        if i < len(n):
            arr.append(n[i])
    while i < len(n):
        arr.append(n[i])
        i += 1
    print(arr)

arr = [-5, -2, 5, 2, 4, -7, -1, -8, 0, -8]
arrange(arr)# method 2
def alternate(arr):
    c = 1
    l = list(filter(lambda x: x>=0,arr))

    for i in arr:
        if i < 0:
            l.insert(c,i)
            c += 2 if c < len(l) else 1
    print(l)

arr = [-5, -2, 5, 2, 4, -7,1, 0]
alternate(arr)
```

62.使用列表理解对列表中的奇数元素求平方。

```
def square(arr):
    l = [e ** 2 if e % 2 != 0 else e for e in arr]
    print(l)

arr = [1, 3, 5, 2, 4, 6, 7]
square(arr)
```

63.从对象列表中获取对象的最大值。

```
from operator import attrgetter

class Employee:
    def __init__(self, name, salary):
        self.name = name
        self.salary = salary

    def __str__(self):
        return "[{}] - {}".format(self.name, self.salary)

joy = Employee("Joy", 20000)
erika = Employee("Erika", 22000)
joyce = Employee("Joyce", 32000)
phebe = Employee("Phebe", 30000)

emp_list = [joy, erika, joyce, phebe]
print(max(emp_list, key=attrgetter("salary")))
```

64.在给定列表中查找包含整数的字符串元素的索引。

```
l = ['sravan',98,'harsha','jyothika','deepika',78,90,'ramya']

for index,value in enumerate(l):
    if type(value) is str:
        print(index)
```

65.按行连接两个列表。

```
def concatenate(l, m):
    result = [list1 + list2 for list1, list2 in zip(l, m)]
    print(result)

l = [[4, 3, 5], [1, 2, 3], [3, 7, 4]]
m = [[1, 3], [9, 3, 5, 7], [8]]

concatenate(l, m)
```

66.根据字符串列表中的数值对该列表中的元素进行排序。

```
def sorting_key(string):
    numeric_value = [ele for ele in string if ele.isdigit()]
    if len(numeric_value) > 0:
        return int(numeric_value[0])
    return -1

arr = ["There are","7 continent","and","7 seas","in the","world"]

arr.sort(key=sorting_key)
print(arr)
```

67.用频率和项目的乘积替换列表中相同的连续元素。

```
l = [3, 3, 3, 3, 6, 7, 5, 5, 5, 8, 8, 6, 6, 6, 6, 6, 1, 1, 1, 2, 2]

def identical_freq_replace(arr):
    t = {}
    for i in range(len(arr)):

        if t.get(arr[i], None):
            t[arr[i]] += 1
        else:
            t[arr[i]] = 1
    arr.clear()

    for key, value in t.items():
        arr.append(key * value)

    print(arr)

identical_freq_replace(l)
```

68.从每个数字中减去 k，如果结果是负数，用 0 代替

```
arr = [2345, 8786, 2478, 8664, 3568, 28]
t = []

def substract_digit(arr, k):
    for e in arr:
        c = "".join([str(max(0, int(s) - k)) for s in str(e)])
        t.append(int(c))
    print(t)

substract_digit(arr, 4)
```

69.给定两个列表，写一个代码以这样的方式打印列表:每个列表中的元素交替出现。

```
# method 1
def write_alternative(arr1, arr2, n1, n2):
    t = []
    c = 0
    for i in arr1 if n1 >= n2 else arr2:
        t.append(i)
        t.append(arr2[c]) if n2 <= n1 else t.append(arr1[c])
        c += 1
        if n2 <= n1:
            if c == n2:
                c = 0
        else:
            if c == n1:
                c = 0

    print(t)

arr1 = ['a', 'b', 'c', 'd']
arr2 = [5, 7, 3]
write_alternative(arr1, arr2, len(arr1), len(arr2))# method 2
from itertools import cycle
def write_alternative(arr1, arr2, n1, n2):
    l = [e for comb in list(zip(cycle(arr2), arr1)) for e in comb]

    print(l)

arr1 = ['a', 'b', 'c', 'd']
arr2 = [5, 7, 3, 5, 8, 7, 9, 10]
write_alternative(arr1, arr2, len(arr1), len(arr2))# method 3
from itertools import cycle,chain
def write_alternative(arr1,arr2,n1,n2):
    l = list(chain(*zip(cycle(arr1),arr2)))
    print(l)

arr1 = ['a', 'b', 'c','d']
arr2 = [5, 7, 3,5,8,7,9,10]

write_alternative(arr1,arr2,len(arr1),len(arr2))
```

70.Python 程序拆分连接在一起的连续相似字符。

```
from itertools import groupby

def separate(string):
    g = groupby(string)

    l = ["".join(grp) for e, grp in g]
    print(l)

string = "ggggffggisssbbbeessssstt"
separate(string)
```

# 基于模式

1.  三角形

```
 * 
   * * 
  * * * 
 * * * * 
* * * * *

def pattern(n):
    for i in range(n):
        for k in range(n - i - 1):
            print(" ", end="")
        for j in range(0, i + 1):
            print("*", end=" ")
        print()

pattern(5)
```

2.对角三角形

```
* * * * * 
 * * * * 
  * * * 
   * * 
    * 

def opposite_triangle(n):
    for i in range(n):
        for j in range(i):
            print(end=" ")

        for k in range(n - i):
            print("*", end=" ")
        print()

opposite_triangle(5)
```

3.直角三角形。

```
* 
* * 
* * * 
* * * * 
* * * * * 

def right_angled_triagle(n):
    for i in range(1,n+1):

        for k in range(i):
            print("*", end=" ")
        print()

right_angled_triagle(5)
```

4.相反向下的直角三角形

```
* * * * * 
* * * * 
* * * 
* * 
* 

def oppsite_right_angled_triagle(n):
    for i in range(n):

        for k in range(n - i):
            print("*", end=" ")
        print()

oppsite_right_angled_triagle(5)
```

5.对角直角三角形

```
 *
   **
  ***
 ****
*****

def right_angled_triagle(n):
    for i in range(n + 1):

        for j in range(n - i):
            print(end=" ")

        for k in range(i):
            print("*", end="")
        print()

right_angled_triagle(5)
```

6.对角直角三角形

```
*****
 ****
  ***
   **
    *

def oppsite_right_angled_triagle(n):
    for i in range(n):

        for j in range(i):
            print(end=" ")

        for k in range(n - i):
            print("*", end="")
        print()
```

7.混合符号三角形。

```
*****
$****
$$***
$$$**
$$$$*

def mix_symbol_triangle(n):
    for i in range(n):

        for j in range(i):
            print(end="$")

        for k in range(n - i):
            print("*", end="")
        print()

mix_symbol_triangle(5)
```

8.附着直角三角形

```
*
**
***
****
*****
****
***
**
*

def attached_triagle(n):
    for i in range(n):

        for j in range(i + 1):
            print("*", end="")
        print()

    for i in range(n - 1):

        for j in range(n - 1 - i):
            print("*", end="")
        print()

attached_triagle(5)
```

9.相对附着的直角三角形

```
 *
   **
  ***
 ****
*****
 ****
  ***
   **
    *

def pattern(n):
    for i in range(n):
        for j in range(n - i - 1):
            print(end=" ")

        for k in range(i + 1):
            print("*", end="")
        print()

    for i in range(1, n):

        for j in range(i):
            print(end=" ")

        for k in range(n - i):
            print("*", end="")

        print()

pattern(5)
```

10.附着三角形

```
 * 
   * * 
  * * * 
 * * * * 
* * * * * 
 * * * * 
  * * * 
   * * 
    * 

def pattern(n):
    for i in range(n):
        for j in range(n - i - 1):
            print(end=" ")
        for k in range(i + 1):
            print("*", end=" ")
        print()
    for i in range(1, n):

        for j in range(i):
            print(end=" ")

        for k in range(n - i):
            print("*", end=" ")
        print()

pattern(5)
```

11.数字三角形

```
1
22
333
4444
55555

def pattern(n):
    for i in range(1, n + 1):

        for j in range(i):
            print(i, end="")

        print()
pattern(5)
```

12.数字三角形

```
1
12
123
1234
12345

def pattern(n):
    for i in range(1, n + 1):

        for j in range(1, i + 1):
            print(j, end="")

        print()

pattern(5)
```

13.相反的数字三角形

```
012345
01234
0123
012
01
def pattern(n):
    for i in range(n):
        for j in range(n - i + 1):
            print(j, end="")
        print()

pattern(5)
```

14.数字三角形

```
1 
3 3 
5 5 5 
7 7 7 7 
9 9 9 9 9

def pattern(n):
    for i in range(1, n + 1):

        for j in range(i):
            print(i * 2 - 1, end=" ")

        print()
pattern(5)
```

15.数字三角形

```
1 
2 1 
3 2 1 
4 3 2 1 
5 4 3 2 1

def pattern(n):
    for i in range(1, n + 1):

        for j in range(i, 0, -1):
            print(j, end=" ")
        print()

pattern(5)
```

16.相反的数字三角形

```
5 4 3 2 1 
4 3 2 1 
3 2 1 
2 1 
1 
def pattern(n):
    for i in range(n):

        for j in range(n - i, 0, -1):
            print(j, end=" ")
        print()

pattern(5)
```

17.反向三角形计数为 10

```
1 
3 2 
6 5 4 
10 9 8 7 

def pattern(n):
    t = c = 1
    row = 1
    while c <= n:
        for i in range(row):
            print(t, end=" ")
            t -= 1
        print()

        row += 1
        c += row
        t = c

pattern(10)
```

18.裤子式样

```
********************
*********__*********
********____********
*******______*******
******________******
*****__________*****
****____________****
***______________***
**________________**
*__________________*

def pattern(n):
    for i in range(n):

        for j in range(n - i):
            print("*", end="")

        for k in range(i * 2):
            print(end="_")

        for l in range(n - i):
            print("*", end="")
        print()

pattern(10)
```

19.空心三角形

```
 * 
   * * 
  *   * 
 *     * 
* * * * * 

def pattern(n):
    for i in range(n):

        for j in range(n - i - 1):
            print(end=" ")

        for k in range(i + 1):
            if k == 0 or k == i or i == n - 1:
                print("*", end=" ")
            else:
                print(" ", end=" ")

        print()

pattern(5)
```

# 基于 Web 框架(Django)

1.  姜戈是什么？
2.  Django 有什么特点？
3.  Django 遵循什么架构？
4.  Django 的生命周期是怎样的？
5.  什么是中间件？
6.  姜戈的会议是什么？
7.  解释 Django 的项目结构。
8.  什么是上下文处理器？
9.  姜戈是铁板一块吗？
10.  Django 模型中使用了哪些继承？
11.  什么是模型？
12.  如何在模型中进行更改？
13.  Django 迁移是如何工作的？
14.  什么是模板标签和过滤器？

**谢谢！**对于阅读，希望会有帮助。