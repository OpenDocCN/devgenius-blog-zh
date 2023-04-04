# #10.2 猜数字— Python 适合初学者

> 原文：<https://blog.devgenius.io/10-2-guess-the-number-python-for-beginners-d7ee55d970d1?source=collection_archive---------15----------------------->

## 使用 while-loops、for-loops 和 random 模块编写一个只有几行代码的猜数字程序

字幕承诺很多！但是我不会让你失望的！在本文中，我们将编写一个只有几行代码(大约 15 行)的猜数字程序，任何人都可以理解！开始吧！

![](img/ba07465e41684629d15d7164b54bf98a.png)

照片由[简·kopřiva](https://www.pexels.com/@koprivakart?utm_content=attributionCopyText&utm_medium=referral&utm_source=pexels)从[派克斯](https://www.pexels.com/photo/photo-of-a-red-snake-3280908/?utm_content=attributionCopyText&utm_medium=referral&utm_source=pexels)拍摄

## 快速笔记

这是上一篇[https://medium . com/dev-genius/10-importing-modules-and-sys-exit-98146308 ae3c](https://medium.com/dev-genius/10-importing-modules-and-sys-exit-98146308ae3c)的续篇。如果你对此有任何问题，请在下面的评论中提问。由于代码在代码本身中有解释(通过注释),我不打算对它说什么。

我也给你选择从要点[https://gist.github.com/](https://gist.github.com/)的代码。链接到这段代码(猜数字程序):[https://gist . github . com/l0 CKD 2 wn/AC 0edd 71277921000733 e8e 5896 a94 e](https://gist.github.com/l0ckD2wN/ac0edd71277921000733e8ee5896a94e)。玩得开心！

## 带有 while 循环的代码

下面你可以看到带有 while 循环的代码，使用了 random & sys 和 if-elif-else 语句。解释放在带有注释的代码中(hastags & green letters)。

```
import random # Importing random tu use randint
import sys # Importing sys to use sys.exit()

number = random.randint(1, 10) # This generates a random number
guesses = 0 # Variable to store guesses

while True: # While-loop to let the user guess as long as the guess is not equal to number
    guesses += 1 # Equal to guesses = 1 + guesses; plus 1 to guesses every round
    print('Guess my number! - Hint: It is between 1 and 10')
    guess = int(input()) # Getting input, casting to int to compare it to number

    if guess < number: # guess is lower than number
        print('You guess is too low.')
    elif guess > number: # guess is higher than number
        print('Your guess is too high.')
    else: # guess equals number
        print('Well done! You guessed my number in ' + str(guesses) + ' guesses.') # Printing out a message + guesses
        sys.exit() # We could also use break here - but we learned about sys.exit() last time, so let us use it!
```

因为解释在代码中，所以没有太多要说的。

## for 循环的代码

下面你可以看到带有 for 循环的代码，使用了 random 和 if-elif-else 语句。解释放在带有注释的代码中(hastags & green letters)。

```
import random # Importing random tu use randint

number = random.randint(1, 10) # This generates a random number

for guesses in range(5): # For-loop to let the user guess for 5 times (range(5))
    print('Guess my number! - Hint: It is between 1 and 10')
    guess = int(input()) # Getting input, casting to int to compare it to number

    if guess < number: # guess is lower than number
        print('You guess is too low.')
    elif guess > number: # guess is higher than number
        print('Your guess is too high.')
    else: # guess equals number
        break # Break out of the for-loop to get to final print

if guess == number: # guess equals number - have to check again to decide between lose or win - THIS IS WIN
    print('Well done! You guessed my number in ' + str(guesses + 1) + ' guesses.') # Printing out a message + guesses
else: # THIS IS LOSE - guess not equals number
    print('You did not guess my number in 5 guesses. My number was ' + str(number) + '. Try again!') # Printing out a message + number
```

我知道这次没有多少文字，但有很多代码。仔细阅读，努力理解。对于初学者来说，这是一个很好的例子，让他们看看用 Python 和编码可以做些什么又好又酷的事情。

下一次，在我们继续之前，我要澄清一些事情 Python 模块、库、包、库和 framekworks 之间的区别*。尽管这是一个更高级的主题，作为初学者你不必担心，但我将在这里介绍它，这样你就可以从一开始作为一名程序员就知道它们的区别和相似之处。之后，我们将玩一个经典游戏——但现在我们把它变成代码:石头、布、剪刀！*

和往常一样，如果你有任何关于 Python 或编码的问题，请在下面的评论中提出。

**直到那时！**

*l0ckD2wN*