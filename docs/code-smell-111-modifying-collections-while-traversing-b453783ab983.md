# 代码味道 111 —遍历时修改集合

> 原文：<https://blog.devgenius.io/code-smell-111-modifying-collections-while-traversing-b453783ab983?source=collection_archive---------12----------------------->

## 在遍历时更改集合可能会导致意外错误

![](img/3fddd3afe14fe776e8be475f0cc45189.png)

克里斯汀·罗伊在 [Unsplash](https://unsplash.com/s/photos/travel) 上的照片

*TL；DR:不要在遍历集合时修改它们*

# 问题

*   意外的结果
*   并发问题

# 解决方法

1.避免改变收藏

2.制作收藏副本

# 语境

我们过度优化了我们的解决方案，认为复制集合是昂贵的。

对于中小型收藏来说，情况并非如此。

语言以许多不同的方式迭代集合。

修改它们通常是不安全的。

# 示例代码

## 错误的

```
Collection<Integer> people = new ArrayList<>();
//here we add elements to the collection…

for (Object person : people) {
 if (condition(person)) {
 people.remove(person);
 }
}
//We iterate AND remove elements
```

## 对吧

```
Collection<Integer> people = new ArrayList<>();
//here we add elements to the collection…List<Object> iterationPeople = ImmutableList.copyOf(people);

for (Object person : iterationPeople) {
 if (condition(person)) {
 people.remove(person);
 }
}
//We iterate a copy and remove from originalcoll.removeIf(currentIndex -> currentIndex == 5);
//Or use language tools (if available)
```

# 侦查

[X]半自动

许多语言在编译和运行时都提供控制-

# 标签

*   快速失败

# 结论

这是我们在第一堂课上学到的东西。

这在工业和现实世界的软件中经常发生

# 关系

[](/code-smell-53-explicit-iteration-d481d8a7e5d7) [## 代码味道 53——显式迭代

### 我们在学校学过循环。但是枚举器和迭代器是下一代。

blog.devgenius.io](/code-smell-53-explicit-iteration-d481d8a7e5d7) 

# 更多信息

[](https://stackoverflow.com/questions/223918/iterating-through-a-collection-avoiding-concurrentmodificationexception-when-re) [## 遍历集合，避免在移除集合中的对象时出现 ConcurrentModificationException…

### 我们都知道你不能因为 ConcurrentModificationException:for(Object I:l){ if…

stackoverflow.com](https://stackoverflow.com/questions/223918/iterating-through-a-collection-avoiding-concurrentmodificationexception-when-re) 

> 虫子就是虫子。你写的代码有 bug，因为你确实有。如果它是运行时安全意义上的安全语言，操作系统会崩溃，而不是以可利用的方式进行缓冲区溢出。

肯·汤普森

[](/software-engineering-great-quotes-3af63cea6782) [## 软件工程名言

### 有时一个简短的想法可以带来惊人的想法。

blog.devgenius.io](/software-engineering-great-quotes-3af63cea6782) 

本文是 CodeSmell 系列的一部分。

[](/how-to-find-the-stinky-parts-of-your-code-fa8df47fc39c) [## 如何找到你的代码中有问题的部分

### 代码很难闻。让我们看看如何改变香味。

blog.devgenius.io](/how-to-find-the-stinky-parts-of-your-code-fa8df47fc39c)