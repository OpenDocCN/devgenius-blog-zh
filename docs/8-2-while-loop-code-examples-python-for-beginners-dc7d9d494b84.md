# #8.2 while-loop 代码示例— Python 面向初学者

> 原文：<https://blog.devgenius.io/8-2-while-loop-code-examples-python-for-beginners-dc7d9d494b84?source=collection_archive---------14----------------------->

## 使用我们的 python 技能来做奇妙的事情

和 if-elif-else 语句一样，你会得到一些很好的示例程序。试试它们，随心所欲地改变它们，并从中获得乐趣！

![](img/370d87d7cd18e397b562e8352d4b7b06.png)

照片由[简·kopřiva](https://www.pexels.com/@koprivakart?utm_content=attributionCopyText&utm_medium=referral&utm_source=pexels)从[派克斯](https://www.pexels.com/photo/photo-of-a-red-snake-3280908/?utm_content=attributionCopyText&utm_medium=referral&utm_source=pexels)拍摄

## 第一个例子，询问最喜欢的颜色:

本例中使用: *while 循环*、 *continue-statement* 、 *break-statement*

```
while True:
    print('What is your favorite color?')
    fav_color = input().lower() # .lower() gives us the string in lower case
    if fav_color == '': # '' means empty string, so no given input
        continue # continue-statement, we return to the start of the loop
    elif fav_color == 'black':
        print('You are cool!')
    else:
        continue # continue-statement, we return to the start of the loop
    print('Interesting... You came until here. You must have a good color-taste.') # We can only see this, if we choose the right color (black)
    break # break-statement, we break out of the loop
```

在这个例子中，我们询问用户他最喜欢的颜色。如果他输入“黑色”，我们会打印出“你很酷”和一条小字信息，说明如果他一直到这里(那里)，他的颜色品味很好。否则，如果用户选择了另一种颜色或不喜欢的颜色，我们就跳回来检查条件(在这个例子中总是真的)，并再次运行 while 循环。

## 第二个例子，倒计时程序:

本例中使用: *while 循环*

```
print('I countdown for you! Enter your start number:')
start_num = int(input()) # input() returns a string, we have to cast it to an int
print('Now enter your last number:')
last_num = int(input()) # input() returns a string, we have to cast it to an int
current_num = start_num
while current_num <= last_num:
    print(current_num)
    current_num += 1 # Adding 1 to current_num so the value raises (countdown)
```

在这个例子中，我们给代码两个数字，一个起始数字和一个结束数字。然后代码从开始到最后一个数字为我们倒计时。当然，你可以输入一个字母并破坏程序(它会给你一个错误)，但是为了简单起见，我们不会检查类似的东西。你可以看到代码倒计时非常快——试着战胜它！

这篇文章没什么好说的。尝试这些程序，改变它们(用它们做实验)并从中获得乐趣。

和往常一样，如果你有任何关于 Python 或编码的问题，请在下面的评论中提出。

**直到那时！**

*l0ckD2wN*