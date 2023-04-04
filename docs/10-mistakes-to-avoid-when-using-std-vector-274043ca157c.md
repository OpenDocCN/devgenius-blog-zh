# 使用 std::vector 时要避免的 10 个错误

> 原文：<https://blog.devgenius.io/10-mistakes-to-avoid-when-using-std-vector-274043ca157c?source=collection_archive---------1----------------------->

C++中使用的默认容器是 std::vector，以防你没有充分的理由使用其他容器。尽管这可能是标准库中最常用的部分，但在使用它时，有几个陷阱不仅仅是初学者可能会陷入的。

## **(1)创建一个有大小的向量，当你需要的是容量【正确性，效率】**

以下代码有什么问题？

```
std::vector<std::string> strs(10);
for(size_t i=0; i<strs.size(); ++i) {
    strs[i] = std::to_string(i);
}
```

问题是我们实际上创建了 10 个空字符串，只是为了在循环中给它们分配新的字符串。

实现[与](http://coliru.stacked-crooked.com/a/8e97887b9db10141)相同的正确方法是:

```
std::vector<std::string> strs;
strs.reserve(10);
for(size_t i=0; i<10; ++i) {
    strs.push_back(std::to_string(i));
}
```

设置大小而不是容量也可能导致实际的错误，如果我们创建一个比我们需要的更大的向量，并使用向量大小作为元素计数的指示(这就是大小的含义)。然而，我们在 vector 中管理的元素的实际“真实”数量更少。

## (2) **未设置容量，需要时更好【效率】**

以下代码有什么问题？

```
std::vector<std::string> strs;
for(size_t i=0; i<1000; ++i) {
    strs.push_back(std::to_string(i));
}
```

嗯，向量可能需要在循环中多次调整大小。如果你知道，或者甚至可以有一个正确的猜测，向量的大小——提前使用它！

![](img/95d8dfb7fc778c975ce6c1d65ac83346.png)

Jan Antonin Kolar 在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄的照片

假设我们想从文件中读取行。我们可能有一个有根据的猜测，行数是文件的大小除以 40。我们可以添加一个`Max_Line_Count`常数边界作为安全措施。然后，在将行读入向量之前，我们可以提前预留容量:

```
std::vector<std::string> v;
v.reserve(std::min(file_size / 40, Max_Line_Count));
// ... read lines into the vector using push_back
```

如果向量是长期存在的，您可能希望使用以下方法将向量缩小到所需的容量:

```
v.shrink_to_fit(); // shrink the vector to the required capacity
```

另一方面，如果向量不是长期存在的，并且您没有预见到内存问题，就让它保持原样。对`shrink_to_fit`的调用代价很高，因为它可能需要重新分配和移动/复制所有项目。

## **(3)手动将向量中的值初始化为零【冗余代码，效率】**

以下代码有什么问题？

```
std::vector<int> v(size);
for(size_t i=0; i<size; ++i) {
    v[i] = 0;
}
```

如果你的答案是可以通过调用`std::fill`来更优雅、更有效地完成，那么这不是正确的答案。答案是这个循环是多余的，因为向量元素在默认情况下是零初始化的。不需要循环。

附带说明:从 C++98 开始，以上是正确的，但是基于不同的向量构造器，直到 C++11，它是基于 cppreference 中的[向量的构造器#3:](https://en.cppreference.com/w/cpp/container/vector/vector)

```
explicit vector(size_type count, const T& value = T(), const Allocator& alloc = Allocator());
```

从 C++11 开始，它基于 cppreference 中的 [vector 的构造函数#4(例如，对于 C++20):](https://en.cppreference.com/w/cpp/container/vector/vector)

```
constexpr explicit vector(size_type count, const Allocator& alloc = Allocator());
```

## **(4)假设用[ ]访问坏索引会抛出【潜在 bug】**

下面的[代码](http://coliru.stacked-crooked.com/a/2b33caf67d6a3c8a)编译运行成功:

```
std::vector<int> v(1);
assert (v[0] + v[1] == 0);
```

当然，它可能会崩溃，但在许多情况下，它不会，因为未定义的行为并不一定会崩溃。不要假设您会在测试期间捕获这些潜在的错误，它们可能会逃脱到生产中。静态代码分析以及运行时清理程序可能有助于发现这样的错误。

如果我们用`-fsanitize=address`、`g++`或`clang`、[运行上面的例子，我们将得到](http://coliru.stacked-crooked.com/a/6893fcf1ddca9a6d):

```
==4155==ERROR: AddressSanitizer: heap-buffer-overflow on address 0x602000000014 at pc 0x000000400e5b bp 0x7fffa419bfc0 sp 0x7fffa419bfb8
READ of size 4 at 0x602000000014 thread T0
#0 0x400e5a in main (/tmp/1640625996.1503701/a.out+0x400e5a)
…
```

![](img/77e8eda1c97a4e5f50c8d38e3d96e152.png)

照片由[晋振伟](https://unsplash.com/@michaeljinphoto?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄

另一种选择是使用`vector::at`而不是`[]`，后者有边界检查。然而，那些追求每一点性能的人可能会取消这个选项，进行额外的内部检查。无论如何，底线是 vector 的[]与 C-array 的[]相比并不安全。

## **(5)在同一位置擦除和插入，而不是赋值【效率】**

我们编写的代码有时倾向于模仿我们试图实现的故事的语义。假设这个故事是，一个人需要从一个向量中移除，以便在相同的位置添加另一个向量。可以实现以下内容:

```
template<typename C, typename T1 = C::value_type,
                     typename T2 = C::value_type>
void replace(C& v, T1&& remove, T2&& replaceWith) {
    auto itr =
        std::find(v.begin(), v.end(), std::forward<T1>(remove));
    if(itr != v.end()) {
        itr = v.erase(itr);
        v.insert(itr, std::forward<T2>(replaceWith));
    }
}
```

但是，如果在 vector 中管理的类型没有阻塞它的赋值，我们可以用下面的方法得到[同样的结果](http://coliru.stacked-crooked.com/a/9f4b0751ef079729):

```
template<typename C, typename T1 = C::value_type,
                     typename T2 = C::value_type>
void replace(C& v, T1&& remove, T2&& replaceWith) {
    auto itr =
        std::find(v.begin(), v.end(), std::forward<T1>(remove));
    if(itr != v.end()) {
        *itr = std::forward<T2>(replaceWith);
    }
}
```

诚然，这两个功能是不一样的，但对于几乎任何实际目的来说，它们会有相同的结果，第二个更有效。

## **(6)在基于范围的过程中改变向量【错误】**

你们中的许多人会立即发现下面的代码是不好的，当我们在基于范围的 for 循环中改变向量时，会遇到未定义的行为:

```
std::vector<int> v = {1, 2, 3};
for(auto i : v) {
    if(i % 2) v.push_back(i);
}
```

主要问题是调用`push_back`可能需要增加向量的容量，因此向量可能需要重新分配，而我们仍然在旧的分配上迭代。

有趣的是，即使发生了重新分配，导致从释放的内存中读取，这当然是一种未定义的行为，[它仍然可以运行而不会崩溃](http://coliru.stacked-crooked.com/a/bcd1d84c2e31579f)。

![](img/182ad4d05244df678163826a32778a18.png)

由 [Mocno Fotografia](https://unsplash.com/@mocno?utm_source=medium&utm_medium=referral) 在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄的照片

如果我们用`-fsanitize=address`，在`g++`或`clang`，[上运行上面的例子，我们将得到](http://coliru.stacked-crooked.com/a/83e03c639f48bf60):

```
==3078==ERROR: AddressSanitizer: heap-use-after-free on address 0x602000000014 at pc 0x000000401e80 bp 0x7ffcc0c63040 sp 0x7ffcc0c63038
READ of size 4 at 0x602000000014 thread T0
#0 0x401e7f in main (/tmp/1640634028.6327093/a.out+0x401e7f)
…
```

## **(7)使用失效的迭代器或指针【Bug】**

前面的例子已经展示了在基于范围的 for 中使用无效迭代器，同时改变了向量。但这并不是唯一的情况。

在添加或删除项目时，持有向量的迭代器或指向向量数据内部某个位置的指针并不是罕见的错误。如果在指向的位置之前删除了一个项目，或者如果向量的大小超过了它的容量，那么指向向量中一个位置的迭代器、引用和指针将会失效。

请注意，持有指数并不总是合适的解决方案。如果删除或添加该索引之前的项目*,该索引也可能无效。当向量改变时，如果你需要在外部保持向量中的位置，这可能是你需要移动到列表的情况之一。*

## **(8)不释放分配[内存泄漏]**

诚然，`std::vector`管理它自己的分配，但是如果你的 vector 持有指向已分配内存的指针，这个内存不会被 vector 释放，释放它是你的责任。

```
std::vector<Shape*> vec;
vec.push_back(new Rectangle());
vec.push_back(new Circle());
// it’s the programmer’s responsibility to release
// the Rectangle and Circle allocated above
```

这就是`std::unique_ptr`可以派上用场的地方，它帮助您管理分配，并在 vector 失效时自动释放它们:

```
std::vector<std::unique_ptr<Shape>> vec;
vec.push_back(std::make_unique<Rectangle>());
vec.push_back(std::make_unique<Circle>());
// the Rectangle and Circle allocated above
// would be automatically released by unique_ptr
```

## **(9)冗余清洗循环[** 冗余代码，**效率】**

有些人不喜欢在离开后留下一片狼藉，他们觉得在离开前必须做一些清理工作。

但是在编码中，这可能会变得低效，如下例所示:

```
class ShapeStore {
    std::vector<std::unique_ptr<Shape>> shapes;
public:
    ~ShapeStore() {
        shapes.clear(); // <= redundant and costly
    }
};
```

载体无论如何都会死亡并被释放。如果你想知道上面的析构函数的代价是什么，因为我们只是显式地进行清理，而不是在向量到达它自己的析构函数时进行同样的清理，那么在这种情况下代价很高，因为你只是通过实现一个你实际上不需要的析构函数，放弃了你的类及其成员的默认移动操作。我们将用最后一个项目来讨论向量及其元素的移动操作。

## **(10)冗余复印——而不是移动【效率】**

如果可以移动，就不要复制。对于向量本身和它的元素来说都是如此。

![](img/5210371edc64f91941945dce2605a84b.png)

Anton Maksimov 5642.su 在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上的照片

移动矢量比复制矢量效率高得多。和移动元素也可能更有效，这取决于向量中保存的实际数据。

导致代码避免移动向量的原因:

*   在保存向量的类中没有移动操作(移动构造函数和移动赋值)。例如，如果您实现了一个析构函数或声明了一个虚析构函数(即使您是用=default 来实现的),并因此退出了零规则，而没有恢复默认的移动操作或实现它们，就可能会发生这种情况。
*   传递一个没有额外用途的本地向量，而不调用 std::move(嗯，这与传递任何可以移动的本地资源是一样的，忘记调用 std::move)，如下例所示:

```
void foo() {
    auto widgets = widgetsFromDb(); // a container, e.g. a vector
    for(auto& widget : widgets) {
        widget->doYourThing();
    }
    // doAdditionalStuffWithWidgets(widgets); // oops, copying

    // this is the proper way to pass our container on,
    // as we don't need it anymore:
    doAdditionalStuffWithWidgets(std::move(widgets));
}
```

导致代码避免移动向量中的元素的原因:

*   忘记在元素类 move 构造函数上添加`noexcept`，这会导致 vector 的 resize 复制元素而不是移动它们。
*   向 vector 传递一个没有额外用途的局部变量，而不调用 std::move(嗯，这与传递任何可以移动的局部资源是一样的，只是忘记了调用 std::move)，如下例所示:

```
void foo() {
    auto widget = getWidgetFromDb(); // a single widget
    widget->doYourThing();
    // vec.push_back(widget); // oops, copying

    // this is the proper way to pass the widget,
    // as we don't need it anymore:
    vec.push_back(std::move(widget));
}
```

在这种情况下，可以补充的是，如果元素可以被放置到向量中，使用`emplace_back`应该比`push_back`更好。

*   通过复制而不是移出，将向量中的元素弹出到变量中:

```
// wrong way - inefficient:
auto val = vec.back(); // copying
vec.pop_back();// wrong way - undefined behavior:
auto& val = vec.back(); // reference is OK
vec.pop_back(); // but now the reference is invalidated// right way, efficient and correct:
auto val = std::move(vec.back()); // allow call of move if available
vec.pop_back();
```

## 最后

大多数 C++程序员都非常熟悉`std::vector`以及本文中提供的大部分技巧。然而，您仍然可以在产品代码中找到上述列表中的错误。如果没有适当的静态代码分析器或仔细的代码审查，这些缺陷可能很难发现。因此，睁大你的眼睛，对出现的问题进行代码审查，但是记住，不出现这些问题的最好方法是从一开始就避免它们。