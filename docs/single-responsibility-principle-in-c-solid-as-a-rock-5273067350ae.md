# 单一责任原则|坚如磐石

> 原文：<https://blog.devgenius.io/single-responsibility-principle-in-c-solid-as-a-rock-5273067350ae?source=collection_archive---------2----------------------->

![](img/a9ac676d18c59efd0a95642536a2b553.png)

本文是关于坚如磐石设计原则系列的五篇文章中的第一篇。坚实的设计原则集中于开发易于维护、可重用和可扩展的软件。在本文中，我们将看到一个 C++中的单一责任原则的例子，以及它的好处。

> */！\:原载@*[*www.vishalchovatiya.com*](http://www.vishalchovatiya.com/category/design-patterns/)*。*

顺便说一句，如果你还没有浏览过我以前关于设计原则的文章，下面是快速链接:

# 目的

> ***一个类应该只有一个理由去改变***

换句话说，SRP 声明类应该内聚到它有单一责任的程度，其中责任定义为“变化的原因”

# 动机:违反单一责任原则

```
class Journal {
    string          m_title;
    vector<string>  m_entries;public:
    explicit Journal(const string &title) : m_title{title} {}
    void add_entries(const string &entry) {
        static uint32_t count = 1;
        m_entries.push_back(to_string(count++) + ": " + entry);
    }
    auto get_entries() const { return m_entries; }
    void save(const string &filename) {
        ofstream ofs(filename); 
        for (auto &s : m_entries) ofs << s << endl;
    }
};int  main() {
    Journal journal{"Dear XYZ"};
    journal.add_entries("I ate a bug");
    journal.add_entries("I cried today");
    journal.save("diary.txt");
    return EXIT_SUCCESS;
}
```

*   上面的 C++例子看起来不错，只要你有一个单独的域对象，即`Journal`。但是在真实的应用程序中通常不是这样。
*   当我们开始添加像`Book`、`File`等域对象时。您必须为每个人分别实现 save 方法，这不是实际问题。
*   当你不得不改变或维护`save`功能时，真正的问题出现了。例如，某一天你将不再把数据保存在文件&所采用的数据库中。在这种情况下，你不得不经历每一个域[对象实现](http://www.vishalchovatiya.com/inside-the-cpp-object-model/) &需要修改的代码都是不好的。
*   在这里，我们违反了单一责任原则，提供了`Journal`第二类变更理由，即与`Journal`，ii 相关的事情。保存`Journal`
*   此外，代码也会变得重复、臃肿&难以维护。

# 解决方案:C++中的单一责任原则示例

*   作为解决方案我们所做的是 [***分离关注点***](https://en.wikipedia.org/wiki/Separation_of_concerns) 。

```
class Journal {
    string          m_title;
    vector<string>  m_entries;public:
    explicit Journal(const string &title) : m_title{title} {} 
    void add_entries(const string &entry) {
        static uint32_t count = 1;
        m_entries.push_back(to_string(count++) + ": " + entry);
    } 
    auto get_entries() const { return m_entries; }
    //void save(const string &filename)
    //{
    //    ofstream ofs(filename); 
    //    for (auto &s : m_entries) ofs << s << endl;
    //}
};struct SavingManager {
    static void save(const Journal &j, const string &filename) {
        ofstream ofs(filename);
        for (auto &s : j.get_entries())
            ofs << s << endl;
    }
};SavingManager::save(journal, "diary.txt");
```

*   `Journal`应该只照顾条目&与日志有关的事情。
*   应该有一个单独的中心位置或实体来完成储蓄工作。在我们的例子中，它是`SavingManager`。
*   随着你的`SavingManager`的增长，所有与保存相关的代码都会在一个地方。你也可以模板化它来接受更多的域对象 T2

# 单一责任原则的好处

# = >表现力

*   当类只做一件事时，它的接口通常有少量更具表现力的方法。因此，它也有少量的数据成员。
*   这提高了您的开发速度&使您作为软件开发人员的生活变得更加容易。

# = >可维护性

*   我们都知道需求会随着时间而变化&设计/架构也是如此。你的类的职责越多，你就越需要经常改变它。如果您的类实现了多个职责，它们就不再相互独立。
*   孤立的更改减少了对软件其他不相关区域的破坏。
*   由于编程错误与复杂性成反比，更容易理解使得代码更不容易出错&更容易维护。

# = >可重用性

*   如果一个类有多个职责，并且在软件的另一个领域中只有其中一个需要，那么其他不必要的职责会阻碍可重用性。
*   拥有一个单一的职责意味着这个类不需要修改或者修改很少就可以重用。

# 用 C++制作 SRP 友好软件的尺度

*   SRP 是一把双刃剑。太具体了&你将最终拥有数百个可笑的相互关联的类，很容易成为一个。
*   当你觉得你过度工程化时，你不应该使用坚实的原则。如果你把单一责任原则归结起来，一般的想法应该是这样的:

***SRP 是关于限制变化的影响。所以，把那些因为同样的原因而改变的东西收集在一起。把那些因为不同原因而改变的东西分开。***

*   更重要的是，如果你的类构造函数有 5-6 个以上的参数，这意味着你要么没有遵循 SRP，要么你没有意识到构建器的设计模式。

# 结论

SRP 是一个被广泛引用的重构理由。这往往是在没有充分理解 SRP 的要点及其背景的情况下完成的，导致代码库的碎片化，带来一系列负面后果。SRP 不是一条通向最小规模班级的单行道，它实际上是在聚集和划分之间提出了一个平衡点。

[有什么建议，查询或者想说](http://www.vishalchovatiya.com/contact-2/) `[Hi](http://www.vishalchovatiya.com/contact-2/)` [？减轻压力，只需点击一下鼠标。](http://www.vishalchovatiya.com/contact-2/) 🖱️