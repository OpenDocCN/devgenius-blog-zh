# # 8.1 while 循环中的 break & cotinue 语句— Python 面向初学者

> 原文：<https://blog.devgenius.io/8-1-break-cotinue-statements-in-while-loops-python-for-beginners-663b7d358d34?source=collection_archive---------16----------------------->

## 从循环中脱离或重新开始

在本文中，我将向您展示我们在编码过程中经常使用的两条新语句。*中断*，*继续*。

![](img/963472a85756222296fdcc5274a7abe7.png)

照片由[简·kopřiva](https://www.pexels.com/@koprivakart?utm_content=attributionCopyText&utm_medium=referral&utm_source=pexels)从[派克斯](https://www.pexels.com/photo/photo-of-a-red-snake-3280908/?utm_content=attributionCopyText&utm_medium=referral&utm_source=pexels)拍摄

## 什么是中断语句(在 while 循环中)

**while 循环中的 break-statement 表示代码必须在此时退出循环。****break-statement 的语法就是关键字 *break* 。现在，使用 break 是不必要的，但是它是一种使我们的代码更干净的方法。你可以看到这里的区别:**

```
# Without break
run = True
while run: # While run == True, run this while loop (we already learned this)
    print('Enter you password:')
    password = input().lower() # Lower gives us the string in lower case
    if password == 'password':
        run = False
```

```
# With break
while True: # True gets always evaluated to True
    print('Enter your password')
    password = input().lower() # Lower gives us the string in lower case
    if password == 'password':
        break # We break out of the loop
```

你看带 break 的代码有点短，对吧？**这类似于将条件中检查的变量设置为 False(当 while 循环的条件为 False 时，它不会运行)。**

## 什么是 continue 语句(在 while 循环中)

**while 循环中的 continue-statement 表示代码必须从头开始 while 循环。因此，while 循环中 continue-statement 下面的所有内容都被跳过，然后再次检查条件，如果条件被评估为 True，while 循环将再次运行。**

```
# Without continue
x = 0
while x < 3:
    x += 1
    print(x)
# OUTPUT: 1, 2, 3
```

```
# with continue
x = 0
while x < 3:
    x += 1
    if x == 2:
        continue
    print(x)
# OUTPUT 1, 3
```

我们检查 x == 2。如果 x == 2 我们使用**关键字*继续*** 。这样我们就跳过了 print(x)语句，所以 x 不会为 2 打印。我们跳回原位，检查条件(在本例中始终为真)，并再次运行 while 循环。

## “真”和“假”值

一小段信息:**我们也可以使用其他数据类型(字符串、数字)，就像它们是 python 代码中的布尔值(真或假)。对于数字，我们使用 0 或 0.0 作为假值，也使用' '(空字符串)作为假值。其他一切都被视为真理。**为了更清楚地看这个程序:

```
num_cats = 0
print('How many cats do you have?')
num_cats = input()
if num_cats:
    print('That is a lot')
print('Program done')
```

如果 num_cats(猫的数量)为 0(无输入—用户仅按退格键来发送输入)，If 语句将被评估为 False，并且不会运行。否则，如果用户给出某种输入，我们将得到一些输出(“这是很多”)。

这都是些新的好东西。我给你工具让你的蟒蛇看起来更干净更短。这篇文章中给出的东西在你的代码中并不是必须的，但是就像我说的，知道这些是很好的。

直到那时！

*l0ckD2wN*