# LeetCode Easy 问题真的很容易吗？

> 原文：<https://blog.devgenius.io/is-leetcode-easy-question-really-easy-2b9aaf32f911?source=collection_archive---------9----------------------->

![](img/e93826d2f35b649bcddf8abe60e295dd.png)

由 [OPPO Find X5 Pro](https://unsplash.com/@oppofindx5pro?utm_source=medium&utm_medium=referral) 在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍照

本去参加一家科技公司的技术面试。

有人问他这个问题:

```
Input: [0,1,1,2,3,3]Output: [0,1,2,3]
```

什么？删除数组中的重复项？简单。

然后，他打开 Python IDE 并输入以下代码。

```
foo = [0,1,1,2,3,3]foo = set (foo)print(foo)
```

> 采访者:set()函数确实有助于保留独特的元素。但是这种实现没有考虑输入的升序特征。而这是否保证了输出的升序？

Ben 很快意识到了排序的问题，并肯定 set()不能保证输入和输出的顺序。

> 面试官:看来你对 Python 比较熟悉。我们的日常开发使用 Java。如果你还不擅长 Java，也没关系。您可以稍后继续使用 Python 进行面试。

Ben 在实习期间做了一些 Java 开发，所以他说他可以转到 Java。然后，他编写了以下代码:

```
public static void main(String[] args) {
    List<Integer> inputNumbers = Arrays.*asList*(0,1,1,2,3,3);
          System.*out*.println(inputNumbers.stream().distinct().collect(Collectors.*toList*()));
}
```

> 面试官:是的。Stream API 可以在这方面有所帮助。但是我们想设计一个 API 方法来完成任务。你能定义一个吗？

```
public List<Integer> func(List<Integer> inputNumbers){
    ...
}
```

> 记者:等等..编码风格和命名惯例也在评估中。

经过一番思考，Ben 将函数的名字改成了一个更好的名字，并开始实现代码。

```
public List<Integer> removeDuplicates(List<Integer> inputNumbers){
    List<Integer> distinctNumbers = new ArrayList<>();
    int prev = inputNumbers.get(0);
    int i;
    for (i=1; i<inputNumbers.size(); i++){
        if (prev!=inputNumbers.get(i)){
            distinctNumbers.add(prev);
        }
        prev = inputNumbers.get(i);
    }
    distinctNumbers.add(inputNumbers.get(i-1));
    return distinctNumbers;
}
```

> 记者:好的，它能跑。但是如果输入为空呢？或者输入数组只有一个元素？

Ben 意识到他错过了那些案例，然后添加了输入检查的条件语句。

**本是谁？**

这些问题是我朋友去阿里巴巴面试的时候问的。

但是，任何人都可能是本。

面试仍然可以继续——你如何改善时间复杂性？参数类型不是整数怎么办？如何让方法适应更多的数据类型？你如何确保这个方法的线程安全？

如果你做过一些 LeetCode，你可能会意识到这是一个“简单”的问题。

一开始，Ben 通过编写 Python 代码很快就进入了这个问题。但他应该做的是，首先明确要求。输入模式是什么样的？预期的输出应该是什么？有哪些边缘案例？沟通在面试和工作中都起着重要的作用。

LeetCode 向我们展示了数据结构、算法和解决问题的不同方法。但是软件工程不仅仅是这些。它还有更多——干净的编码实践、可扩展性、设计模式、不同的业务场景和其他软性的东西。

那么我们应该如何准备面试呢？

你坚持练习 LeetCode 问题是一件好事。在解决一个问题后，我们应该尝试评估当前答案的复杂性、可扩展性、可维护性和其他可能的用例，而不是死记硬背下一个问题，这样我们就可以在更多的维度上进行思考。当我们练习得越多，在面试中被问到类似问题时，我们就越不会惊慌失措。

如果你有更多的空闲时间，你可以探索 GitHub 上那些成熟的代码库，帮助你更广泛地了解其他开发人员是如何解决问题的。

推荐你阅读你感兴趣的话题。然后你就可以和面试官自然地谈论这个话题了。我的意思是，每个人都喜欢不断自我学习的人。

对于最近要去做面试的人，希望我的建议能帮到你。祝你好运~

我希望这篇文章对你有所帮助。如果你像我一样，渴望学习 Java 和更多关于后端工程的知识，请关注我的频道，了解我在日常工作和生活中获得的灵感。

> ***阅读更多:***[*一个关于 Java 静态关键字在职期间的案例*](/a-case-about-java-static-keyword-during-my-job-53cebb6af597)[*如何解决这个 Java 多线程面试问题？*](/how-can-you-solve-this-java-multithreading-interview-problem-8e6ec53fab27)
> 
> ***获取连接:***[*我的 LinkedIn*](https://www.linkedin.com/in/daini-wang-5127b2182)