# 现代 C++中的适配器设计模式

> 原文：<https://blog.devgenius.io/adapter-design-pattern-in-modern-c-32d92d56c4bf?source=collection_archive---------12----------------------->

![](img/96087c072b57c8016f011ead1247aabc.png)

在软件工程中，结构设计模式处理对象和类之间的关系，即对象和类如何以适合情况的方式交互或建立关系。结构设计模式通过识别关系来简化结构。在这篇关于结构设计模式的文章中，我们将了解一下现代 C++中的适配器设计模式，其中 ***用于将现有类的接口转换为客户端/API-用户期望的另一个接口*** 。适配器设计模式使得由于接口不兼容而无法协同工作的类能够协同工作。

> */！\:本文已原创发表于我的* [*博客*](http://www.vishalchovatiya.com/adapter-design-pattern-in-modern-cpp/) *。如果你有兴趣接收我的最新文章，* [*请报名参加我的简讯*](http://eepurl.com/gDNybv) *。*

顺便说一句，如果你还没有看过我关于结构设计模式的其他文章，下面是列表:

1.  [**适配器**](http://www.vishalchovatiya.com/adapter-design-pattern-in-modern-cpp/)
2.  [桥**桥**桥](http://www.vishalchovatiya.com/bridge-design-pattern-in-modern-cpp/)
3.  [**复合**](http://www.vishalchovatiya.com/composite-design-pattern-in-modern-cpp/)
4.  [**装饰者**](http://www.vishalchovatiya.com/decorator-design-pattern-in-modern-cpp/)
5.  [**立面**](http://www.vishalchovatiya.com/facade-design-pattern-in-modern-cpp/)
6.  [**飞锤**](http://www.vishalchovatiya.com/flyweight-design-pattern-in-modern-cpp/)
7.  [**代理**](http://www.vishalchovatiya.com/proxy-design-pattern-in-modern-cpp/)

您在这一系列文章中看到的代码片段是简化的，而不是复杂的。所以你经常看到我不使用像`override`、`final`、`public`(同时继承)这样的关键字，只是为了让代码紧凑&可消费(大多数时候)在单一标准屏幕尺寸。我也更喜欢`struct`而不是`class`，只是为了节省代码行，有时不写`public:`，还会故意忽略[虚拟析构函数](http://www.vishalchovatiya.com/part-3-all-about-virtual-keyword-in-c-how-virtual-destructor-works/)，构造函数[，复制构造函数](http://www.vishalchovatiya.com/all-about-copy-constructor-in-cpp-with-example/)，前缀`std::`，删除动态内存。我也认为自己是一个务实的人，希望用尽可能简单的方式，而不是标准的方式或使用术语来传达一个想法。

***注:***

*   如果你是在这里被直接绊倒的，那么我建议你浏览一下[什么是设计模式？](http://www.vishalchovatiya.com/what-is-design-pattern/)一、哪怕是鸡毛蒜皮的小事。相信会鼓励你对这个话题进行更多的探索。
*   您在本系列文章中遇到的所有这些代码都是使用 C++20 编译的(尽管我在大多数情况下使用了 C++17 之前的现代 C++特性)。因此，如果你无法获得最新的编译器，你可以使用已经预装了 boost 库的[https://wandbox.org/](https://wandbox.org/)。

# 目的

> ***从你拥有的界面中获取你想要的界面。***

*   适配器允许两个不兼容的类一起工作，方法是将一个类的接口转换成客户机/API 用户期望的接口，而不改变它们。基本上，添加中间类，即适配器。
*   如果你发现自己处于使用适配器的情况，那么你可能正在处理库、模块、插件等之间的兼容性。如果没有，那么你可能会有严重的设计问题，因为如果你在设计的早期就遵循了[依赖倒置原则](http://www.vishalchovatiya.com/dependency-inversion-principle-in-cpp-solid-as-a-rock/)。使用适配器设计模式就不是这样了。

# C++中的适配器设计模式示例

*   实现适配器设计模式很容易，只需确定您拥有的 API 和您需要的 API。创建一个组件，该组件聚合(具有对…的引用)适配器。

# 经典适配器

```
struct Point {
    int32_t     m_x;
    virtual void draw(){ cout<<"Point\n"; }
};struct Point2D : Point {
    int32_t     m_y;
    void draw(){ cout<<"Point2D\n"; }
};void draw_point(Point &p) {
    p.draw();
}struct Line {
    Point2D     m_start;
    Point2D     m_end;
    void draw(){ cout<<"Line\n"; }
};struct LineAdapter : Point {
    Line&       m_line;
    LineAdapter(Line &line) : m_line(line) {}
    void draw(){ m_line.draw(); }
};int main() {
    Line l;
    LineAdapter lineAdapter(l);
    draw_point(lineAdapter);
    return EXIT_SUCCESS;
}
```

*   您还可以通过利用 [C++模板](http://www.vishalchovatiya.com/c-template-a-quick-uptodate-look/)创建一个通用适配器，如下所示:

```
template<class T>
struct GenericLineAdapter : Point {
    T&      m_line;
    GenericLineAdapter(T &line) : m_line(line) {}
    void draw(){ m_line.draw(); }
};
```

*   当你考虑到当你需要做其他事情时，非泛型方法很快变得非常多余，那么泛型方法的有用性有望变得更加明显。

# 使用现代 C++的可插拔适配器设计模式

*   适配器应该使用客户机/API 用户已知的相同的旧目标接口来支持被适配器(它们是不相关的并且具有不同的接口)。下面的例子通过使用 C++11 的 [lambda 函数](http://www.vishalchovatiya.com/learn-lambda-function-in-cpp-with-example/) &函数头来满足这个属性。

```
/* Legacy code -------------------------------------------------------------- */
struct Beverage {
    virtual void getBeverage() = 0;
};struct CoffeeMaker : Beverage {
    void Brew() { cout << "brewing coffee" << endl;}
    void getBeverage() { Brew(); }
};void make_drink(Beverage &drink){
    drink.getBeverage();                // Interface already shipped & known to client
}
/* --------------------------------------------------------------------------- */struct JuiceMaker {                     // Introduced later on
    void Squeeze() { cout << "making Juice" << endl; }
};struct Adapter : Beverage {              // Making things compatible
    function<void()>    m_request; Adapter(CoffeeMaker* cm) { m_request = [cm] ( ) { cm->Brew(); }; }
    Adapter(JuiceMaker* jm) { m_request = [jm] ( ) { jm->Squeeze(); }; } void getBeverage() { m_request(); }
};int main() {
    Adapter adp1(new CoffeeMaker());
    make_drink(adp1); Adapter adp2(new JuiceMaker());
    make_drink(adp2);
    return EXIT_SUCCESS;
}
```

*   可插拔适配器会找出当时插入的是哪个对象。一旦一个对象被插入，并且它的方法被分配给委托对象(在我们的例子中就是`m_request`),这个关联就一直持续到另一组方法被分配。
*   可插拔适配器的特点是，它将为它所适应的每种类型都提供构造函数。在每一个方法中，它执行委托[分配](http://www.vishalchovatiya.com/2-wrong-way-to-learn-copy-assignment-operator-in-cpp-with-example/)(一个，或者多个，如果有更多的方法用于重新路由)。
*   可插拔适配器提供了以下两个主要优势:

1.  你可以绑定一个接口(在构造函数参数中绕过 lambda 函数)，不像我们在上面例子中做的对象。
2.  当适配器和被适配器具有不同数量的参数时，这也很有帮助。

# 适配器设计模式的好处

1.  [开闭原则](http://www.vishalchovatiya.com/open-closed-principle-in-cpp-solid-as-a-rock/):适配器模式的一个优点是你不需要改变现有的类或接口。通过引入一个新的类，它充当接口和类之间的适配器，可以避免对现有代码进行任何更改。
2.  这也限制了您对软件组件的更改范围，并避免了其他组件或应用程序中的任何更改和副作用。
3.  通过以上两点，即单独的类(即[单一责任原则](http://www.vishalchovatiya.com/single-responsibility-principle-in-cpp-solid-as-a-rock/))对于特殊功能&更少的副作用，很明显我们确实需要更少的维护，学习曲线&测试。
4.  AdapterDesing 模式还遵循[依赖倒置原则](http://www.vishalchovatiya.com/dependency-inversion-principle-in-cpp-solid-as-a-rock/)，因此您可以在多个版本之间保持二进制兼容性。

# 常见问题汇总

**何时使用适配器设计模式？**

—当您想要使用一些现有的类，但其接口与您的代码的其余部分不兼容时，请使用适配器类。
—当你想要重用一些现有的子类，这些子类缺少一些不能添加到超类中的公共功能。
—例如，假设您有一个函数接受天气对象&以摄氏度为单位打印温度。但是现在你需要打印华氏温度。在这种不兼容的情况下，您可以使用适配器设计模式。

**现实生活中&适配器设计模式的实际例子？**

—在 STL 中，堆栈、队列和优先级队列是来自队列和向量的适配器。当 stack 执行[stack](https://en.cppreference.com/w/cpp/container/stack):push()时，底层 vector 做`vector::push_back()`。
-作为存储卡和笔记本电脑之间适配器的读卡器。
-您的移动&笔记本电脑充电器是一种适配器，它将标准电压&电流转换为您的设备所需的电压。

**桥接&适配器设计模式有什么区别？**

—适配器通常与现有的应用程序一起使用，以使一些原本不兼容的类很好地协同工作。
— [Bridge](http://www.vishalchovatiya.com/bridge-design-pattern-in-modern-cpp/) 通常是预先设计好的，允许你独立开发应用程序的各个部分。

**装饰器&适配器设计模式有什么区别？**

—适配器将一个接口转换为另一个接口，而不增加额外的功能\
— [装饰器](http://www.vishalchovatiya.com/decorator-design-pattern-in-modern-cpp/)将新功能添加到现有的接口中。

**代理&适配器设计模式有什么区别？**

—适配器设计模式将一个类的接口转换为兼容但不同的接口。代理提供相同但简单的接口，或者有时作为唯一的包装器。