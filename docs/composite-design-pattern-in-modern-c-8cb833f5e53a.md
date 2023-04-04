# 现代 C++中的复合设计模式

> 原文：<https://blog.devgenius.io/composite-design-pattern-in-modern-c-8cb833f5e53a?source=collection_archive---------13----------------------->

![](img/cbe3436490e1738c6410a47365beb512.png)

GoF 将复合设计模式描述为“将对象组成一个树形结构来表示部分-整体层次结构。Composite 让客户统一处理单个对象和对象的组合”。这对我来说似乎太复杂了。所以，我不想用树叶的术语。相反，我直接看到了在现代 C++中实现复合设计模式的两三种不同方式。但简单来说，复合设计模式是一种结构化的设计模式，其目标 ***是以对待单个对象*** 的方式对待一组对象。

> */！\:本文已原创发表于我的* [*博客*](http://www.vishalchovatiya.com/composite-design-pattern-in-modern-cpp/) *。如果你有兴趣接收我的最新文章，* [*请报名参加我的简讯*](http://eepurl.com/gDNybv) *。*

顺便说一句，如果你还没有看过我关于结构设计模式的其他文章，下面是列表:

1.  [**适配器**](http://www.vishalchovatiya.com/adapter-design-pattern-in-modern-cpp/)
2.  [桥**桥**桥](http://www.vishalchovatiya.com/bridge-design-pattern-in-modern-cpp/)
3.  [**复合**](http://www.vishalchovatiya.com/composite-design-pattern-in-modern-cpp/)
4.  [**装饰者**](http://www.vishalchovatiya.com/decorator-design-pattern-in-modern-cpp/)
5.  [**立面**](http://www.vishalchovatiya.com/facade-design-pattern-in-modern-cpp/)
6.  [**蝇量级**](http://www.vishalchovatiya.com/flyweight-design-pattern-in-modern-cpp/)
7.  [**代理**](http://www.vishalchovatiya.com/proxy-design-pattern-in-modern-cpp/)

您在这一系列文章中看到的代码片段是简化的，而不是复杂的。所以你经常看到我不使用像`override`、`final`、`public`(同时继承)这样的关键字，只是为了让代码紧凑&可消耗(大多数时候)在单一标准屏幕尺寸。我也更喜欢`struct`而不是`class`，只是为了节省代码行，有时不写`public:`，也故意忽略[虚析构函数](http://www.vishalchovatiya.com/part-3-all-about-virtual-keyword-in-c-how-virtual-destructor-works/)，构造函数[，复制构造函数](http://www.vishalchovatiya.com/all-about-copy-constructor-in-cpp-with-example/)，前缀`std::`，删除动态内存。我也认为自己是一个务实的人，希望用尽可能简单的方式，而不是标准的方式或使用术语来传达一个想法。

***注:***

*   如果你是在这里被直接绊倒的，那么我建议你浏览一下[什么是设计模式？](http://www.vishalchovatiya.com/what-is-design-pattern/)第一，哪怕是微不足道的小事。相信会鼓励你对这个话题进行更多的探索。
*   您在这一系列文章中遇到的所有代码都是使用 C++20 编译的(尽管我在大多数情况下使用了 C++17 的现代 C++特性)。因此，如果你没有最新的编译器，你可以使用已经预装了 boost 库的 https://wandbox.org/。

# 目的

> **以统一的方式对待单个&组对象。**

![](img/2967fe15c82eef2074cf22437933a95e.png)

*   那么它到底是什么，我们为什么需要它。我们知道，对象通常通过继承或组合来使用其他对象、字段、属性或成员。
*   例如，在绘图应用程序中，您有一个`Shape`(例如`Circle`)可以在屏幕上绘制，但是您也可以有一组从集合`Shape`继承而来的`Shape`(例如`vector<Circle>`)。
*   它们有一些通用的 API，你可以调用它们中的一个或者另一个，而不需要事先知道你是在处理单个元素还是整个集合。

# C++中的复合设计模式示例

*   因此，如果你想到一个应用程序，如 PowerPoint 或任何类型的矢量绘图应用程序，你知道你可以绘制和拖动单个形状。
*   但是您也可以将形状组合在一起。当您将几个形状组合在一起时，您可以将它们视为单个形状。所以你可以抓取整个东西，拖动它，调整它的大小等等。
*   因此，我们将围绕几种不同形状的想法实现复合设计模式。

# 经典复合设计模式

```
struct Shape {
    virtual void draw() = 0;
};struct Circle : Shape {
    void draw() { cout << "Circle" << endl; }
};struct Group : Shape {
    string              m_name;
    vector<Shape*>      m_objects; Group(const string &n) : m_name{n} {} void draw() {
        cout << "Group " << m_name.c_str() << " contains:" << endl;
        for (auto &&o : m_objects)
            o->draw();
    }
};int main() {
    Group root("root");
    root.m_objects.push_back(new Circle); Group subgroup("sub");
    subgroup.m_objects.push_back(new Circle); root.m_objects.push_back(&subgroup);
    root.draw(); return EXIT_SUCCESS;
}
/*
Group root contains:
Circle
Group sub contains:
Circle
*/
```

# 使用奇怪重复出现的模板模式的复合设计模式(CRTP)

*   正如你可能已经注意到的，机器学习现在是一个非常热门的话题。机器学习机制的一部分是使用神经网络，这就是我们现在要看的。

```
struct Neuron {
    vector<Neuron*>     in, out;
    uint32_t            id; Neuron() {
        static int id = 1;
        this->id = id++;
    } void connect_to(Neuron &other) {
        out.push_back(&other);
        other.in.push_back(this);
    } friend ostream &operator<<(ostream &os, const Neuron &obj) {
        for (Neuron *n : obj.in)
            os << n->id << "\t-->\t[" << obj.id << "]" << endl; for (Neuron *n : obj.out)
            os << "[" << obj.id << "]\t-->\t" << n->id << endl; return os;
    }
};int main() {
    Neuron n1, n2;
    n1.connect_to(n2);
    cout << n1 << n2 << endl;
    return EXIT_SUCCESS;
}
/* Output
[1]    -->    2
1    -->    [2]
*/
```

*   正如你所看到的，我们有一个神经元结构，它与其他神经元有连接，这些神经元被建模为输入-输出神经元连接的指针向量。这是一个非常基本的实现，只要你只有单独的神经元，它就能很好地工作。现在有一件事我们还没有考虑到，当你有一个以上的神经元或神经元群连接时会发生什么。
*   假设我们决定做一个神经元层，现在一层神经元基本上就像一个集合。

```
struct NeuronLayer : vector<Neuron> {
    NeuronLayer(int count) {
        while (count-- > 0)
            emplace_back(Neuron{});
    } friend ostream &operator<<(ostream &os, NeuronLayer &obj) {
        for (auto &n : obj)
            os << n;
        return os;
    }
};int main() {
    NeuronLayer l1{1}, l2{2};
    Neuron n1, n2;
    n1.connect_to(l1);  // Neuron connects to Layer
    l2.connect_to(n2);  // Layer connects to Neuron
    l1.connect_to(l2);  // Layer connects to Layer
    n1.connect_to(n2);  // Neuron connects to Neuron return EXIT_SUCCESS;
}
```

*   现在，正如您可能已经猜到的那样，如果您要正面实现这个功能，您将总共有四个不同的功能。即

```
Neuron::connect_to(NeuronLayer&)
NeuronLayer::connect_to(Neuron&)
NeuronLayer::connect_to(NeuronLayer&)
Neuron::connect_to(Neuron&)
```

*   所以这是状态空间爆炸&置换问题，这并不好，因为我们想要一个既能枚举层又能枚举单个神经元的函数。

```
template <typename Self>
struct SomeNeurons {
    template <typename T>
    void connect_to(T &other);
};struct Neuron : SomeNeurons<Neuron> {
    vector<Neuron*>     in, out;
    uint32_t            id; Neuron() {
        static int id = 1;
        this->id = id++;
    } Neuron* begin() { return this; }
    Neuron* end() { return this + 1; }
};struct NeuronLayer : vector<Neuron>, SomeNeurons<NeuronLayer> {
    NeuronLayer(int count) {
        while (count-- > 0)
            emplace_back(Neuron{});
    }
};template <typename Self>
template <typename T>
void SomeNeurons<Self>::connect_to(T &other) {
    for (Neuron &from : *static_cast<Self *>(this)) {
        for (Neuron &to : other) {
            from.out.push_back(&to);
            to.in.push_back(&from);
        }
    }
}template <typename Self>
ostream &operator<<(ostream &os, SomeNeurons<Self> &object) {
    for (Neuron &obj : *static_cast<Self *>(&object)) {
        for (Neuron *n : obj.in)
            os << n->id << "\t-->\t[" << obj.id << "]" << endl; for (Neuron *n : obj.out)
            os << "[" << obj.id << "]\t-->\t" << n->id << endl;
    }
    return os;
}int main() {
    Neuron n1, n2;
    NeuronLayer l1{1}, l2{2}; n1.connect_to(l1); // Scenario 1: Neuron connects to Layer
    l2.connect_to(n2); // Scenario 2: Layer connects to Neuron
    l1.connect_to(l2); // Scenario 3: Layer connects to Layer
    n1.connect_to(n2); // Scenario 4: Neuron connects to Neuron cout << "Neuron " << n1.id << endl << n1 << endl;
    cout << "Neuron " << n2.id << endl << n2 << endl; cout << "Layer " << endl << l1 << endl;
    cout << "Layer " << endl << l2 << endl; return EXIT_SUCCESS;
}
/* Output
Neuron 1
[1]    -->    3
[1]    -->    2Neuron 2
4    -->    [2]
5    -->    [2]
1    -->    [2]Layer 
1    -->    [3]
[3]    -->    4
[3]    -->    5Layer 
3    -->    [4]
[4]    -->    2
3    -->    [5]
[5]    -->    2
*/
```

*   如你所见，在 CRTP 的帮助下，我们用一个`SomeNeurons::connect_to`方法涵盖了所有四种不同的排列场景。和`Neuron` & `NeuronLayer`都通过自己的模板化符合这个接口。
*   **C** 精心**R**ecurring**T**emplate**P**attern 在这里派上了用场&有非常直接的实现规则，即 ***分离出依赖类型的&独立功能，并使用自引用模板*** 将类型 *独立功能与基类绑定。*
*   我已经写了一篇关于高级 C++概念的文章，包括 CRTP。

# 复合设计模式的好处

1.  通过消除同构对象集合上的许多循环来降低代码复杂性。
2.  这个实习生增加了代码的可维护性和可测试性，减少了破坏现有运行和测试代码的机会。
3.  复合设计模式中描述的关系不是子类关系，而是集合关系。这意味着客户端/API 用户不需要关心操作(如平移、旋转、缩放、绘图等)。)无论是单个[对象](http://www.vishalchovatiya.com/inside-the-cpp-object-model/)还是整个集合。

# 常见问题汇总

**我什么时候应该使用复合设计模式？**

—您希望客户端能够忽略对象组和单个对象之间的差异。
—当你发现你以同样的方式使用多个对象，并循环执行有点类似的动作时，那么复合是一个不错的选择。

**复合设计模式的常见例子是什么？**

— File & Folder(文件集合):这里 File 是一个单独的类。文件夹继承文件并保存文件集合。

**Decorator&复合设计模式有什么区别？**

—装饰者致力于增强界面。
—组合用于统一单个&对象组的接口。