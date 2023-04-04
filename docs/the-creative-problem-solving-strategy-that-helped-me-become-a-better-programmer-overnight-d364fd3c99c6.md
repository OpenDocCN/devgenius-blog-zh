# 帮助我一夜之间成为更好的程序员的创造性解决问题的策略

> 原文：<https://blog.devgenius.io/the-creative-problem-solving-strategy-that-helped-me-become-a-better-programmer-overnight-d364fd3c99c6?source=collection_archive---------9----------------------->

作为一名初级开发人员，我一直在寻找工具、技巧和策略来帮助我更好地理解如何完成我的工作。起初，我关心的是帮助我学习语言和语法的资源。我写了帮助我实现这一目标的我最喜欢的资源。虽然花时间用这些来练习特定语言技能非常有效，但我最近学到了一个创造性解决问题的策略，这让我一夜之间成为了一名更好的程序员。

![](img/26e8211626b5344382b43ed68a3c921e.png)

由[约书亚·阿拉贡](https://unsplash.com/@goshua13?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄的照片

在分享我所学到的东西之前，想想你会如何用你选择的编程语言解决下面的问题。我第一次遇到这个解题挑战是在 V. Anton Spraul 的 [*像程序员一样思考*](https://www.amazon.com/Think-Like-Programmer-Introduction-Creative/dp/1593274246) 中。

**问题:一个横着的三角形**

> 指令:编写一个程序，使用不超过两个输出语句打印#字符的侧边三角形，如下所示:

```
#
##
###
####
###
##
#
```

我的第一个倾向是打开一个代码编辑器或 REPL，开始输入我认为可以完成任务的代码(我想它会在我不断试错的过程中出现)。).

唉，我限制了自己的默认模式，取而代之的是，应用问题解决策略**先用伪代码**减少问题。

换句话说，我打开我的代码编辑器，把解决这个问题的算法分解成更小的步骤，用简单的英语打成注释，就像这样。

```
// Create a method definition called sideways_triangle.// Inside the method:// create a variable called Row with an initial value of 1;// create a variable called Iterations with an initial value of 1;// use a loop to print out #'s equal to the value of Iterations;// add a newline character to the end of each line of #s;// if the value of Row is less than or equal to 4, increment the value of Iterations by 1 after each new loop cycle;// else if the value of Row is greater than 4, decrement the value of Iterations by 1 after each new loop cycle;// increment the value of Row by 1 with each new loop cycle;// end the loop when either the Rows variable equals 8 or the Iterations variable equals 0.
```

公平地说，我没有像第一次尝试时那样坐下来写下这些步骤。我编写了部分代码，然后意识到我需要添加一个额外的步骤(为迭代创建一个单独的变量)，并且会在我头脑中计划好我的方法时回去编辑我的伪代码。和最终的算法一样，我的简单英语编码也有很多迭代。

然而，一旦我花时间来充实我如何解决横向三角形问题的想法，这个编程挑战最困难的部分就完成了。我想出了解决问题的方法。我创造了一个算法！

有了我的伪代码，剩下要做的就是编写代码来执行我在每一行中安排的步骤。我储存在脑子里的一些代码。但是其他的，我只好上网查了。这完全没问题。

选择用 Ruby 解决这个挑战后，我想到了下面的解决方案(注意伪代码与 Ruby 代码的等价性)。

```
// Create a method definition called sideways_triangle.
**def sideways_triangle**// Inside the method:// create a variable called Row with an initial value of 1;
  **row = 1**// create a variable called Iterations with an initial value of 1;
  **iterations = 1**// use a loop to print out #'s equal to the value of Iterations;
  **while row < 8 do**// add a newline character to the end of each line of #s;// if the value of Row is less than or equal to 4, increment the value of Iterations by 1 after each new loop cycle;
    **if row < 4** **print "#" * iterations + "\n"** **iterations += 1**// else if the value of Row is greater than 4, decrement the value of Iterations by 1 after each new loop cycle;
    **else** **print "#" * iterations + "\n"** **iterations -=1** **end**// increment the value of Row by 1 with each new loop cycle;
    **row += 1**// end the loop when either the Rows variable equals 8 or the Iterations variable equals 0.
  **end****end**
```

嘣！这个管用。如果我现在走开，我会成功地用 Ruby 编写一个算法来解决横向三角形问题。

虽然这是一个停下来保存你的代码以应对个人编程挑战的好地方，但在编写产品代码时，你可以简单地删除伪代码注释以清理你的文件，并使你的算法的计算机语言部分更具可读性，就像这样:

```
 **def sideways_triangle**
  **row = 1**
  **iterations = 1** **while row < 8 do**
    **if row < 4
      print "#" * iterations + "\n"
      iterations += 1**
    **else
      print "#" * iterations + "\n"
      iterations -=1
    end**
    **row += 1**
  **end
end**
```

在这一点上，你甚至可以找到机会来重构和精炼你生成的代码。不管怎样，当我们**用伪代码**减少问题时，我们首先做了解决问题的繁重工作。正是在这个经常被忽视的解决问题的步骤中，我们确保了先用人类语言理解我们算法的必要步骤，然后再将简单的英语翻译成计算机也能理解的代码。

> “任何傻瓜都能写出计算机能理解的代码。优秀的程序员会写出人类能理解的代码。”—马丁·福勒

这种方法不仅帮助我更有效地编写代码来解决问题，还提升了我对我使用的代码以及它到底在做什么的反思性批判性思考。