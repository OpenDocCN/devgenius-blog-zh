# 在 Java 和 JavaScript 中通过多个字段对对象数组进行排序

> 原文：<https://blog.devgenius.io/sorting-an-array-of-object-by-multiple-fields-in-java-and-javascript-e66f0366fc71?source=collection_archive---------0----------------------->

这是一篇关于排序的文章，我将在其中讲述如何在 Java 中使用 lambda 函数和在 JavaScript 中使用 arrow 函数通过多个字段对一个对象数组进行排序。

![](img/72df63e0f0d1582a059497c3dbe7f45e.png)

照片由[苏拉娅·欧文](https://unsplash.com/@traxing?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄

# 首先让我们来看看 Java

假设我们有一个雇员对象数组。其中 Employee 是一个具有两个字段(姓名和薪水)的类。

第一个问题是根据薪水对数组进行排序。我想我们都知道这一点。所以我们可以这样做。

```
// let's assume we have an array of Employee - employees
// Employee is a class of two fields (String name, int salary)Arrays.sort(employees, (a,b) -> a.salary - b.salary);
```

第二个问题是根据薪水和姓名对数组进行排序。

## 这背后的想法非常简单。

1.  **我们要对比一下初级领域。**
2.  **只有当两个主字段相同时，我们才能返回辅助字段的比较结果。** *(所以如果有更多的字段，你可以用类似的方式扩展)*

现在让我们来看看实际情况

```
// let's assume we have an array of Employees - employees
// Employee is a class of two fields (String name, int salary)Arrays.sort(employees, (a, b) -> 
    ((a.salary - b.salary) != 0) ? (a.salary - b.salary):
     (a.name.compareTo(b.name)));
```

如果有两个以上字段，您可以用类似的方式扩展它。但为此，我建议编写一个定制的比较器。你可以扩展**比较器类**并且**覆盖比较方法**。

这种方法甚至可以用于对**收藏**进行排序。甚至排序方法也接受**比较器**功能。

`Collections.sort(collection, Comparator function)`

# 现在让我们来看看 JavaScript

我们将举类似的例子。我们有一组雇员对象。其中每个对象有两个字段(姓名和薪水)。

第一个问题是根据薪水对数组进行排序。这很简单。我们可以这样做。

```
// We will assume employees is the arrayemployees.sort((a, b) => a.salary - b.salary)
```

第二个问题是根据薪水和名字排序。

我们将遵循我们已经讨论过的同样的想法。所以我们可以这样做。

```
// We will assume employees is the arrayemployees.sort((a, b) => 
    ((a.salary - b.salary) != 0) ? (a.salary - b.salary):
     (a.name.localCompare(b.name)));
```

但是在 JavaScript 中，我们可以进一步简化。

在我们开始写代码之前，让我们先来看看我们将要用到的一些重要的 JavaScript 技巧。

1.  `0 == false`【这将是真的】
2.  `(false || (2-3))`【本将-1】
3.  `(true || (2-3))`【这将是真的】

延伸第 2 点和第 3 点我们可以说可以得出这样的结论:

4.`((4-6) || (2-3))`【本将-2】

上面的代码我们可以简化为

```
// We will assume employees is the arrayemployees.sort((a, b) => 
    (a.salary - b.salary) || (a.name.localCompare(b.name)));
```

这样，将这个想法扩展到更多领域变得更加简单。

比方说，如果我们必须根据三个字段进行排序，我们可以这样做。

```
array.sort((a,b) => 
    (a.field1 - b.field1) || 
    (a.field2 - b.field2) || 
    (a.field3 - b.field3));
```

*   上面我们已经介绍了升序排序。如果您必须按降序对任何字段进行排序。你可以只做`b-a`而不做`a-b`。