# 现代 C++中的装饰设计模式

> 原文：<https://blog.devgenius.io/decorator-design-pattern-in-modern-c-4af9053ccc7f?source=collection_archive---------7----------------------->

![](img/9f8162b049eaa7e9daff02e73874e724.png)

在软件工程中，结构设计模式处理对象和类之间的关系，即对象和类如何以适合情况的方式交互或建立关系。结构设计模式通过识别关系来简化结构。在这篇关于结构设计模式的文章中，我们将看一看并不复杂但微妙的设计模式，它是现代 C++中的装饰设计模式，因为它具有可扩展性和可测试性。也就是 ***被称为*** 。

> */！\:本文已原创发表于我的* [*博客*](http://www.vishalchovatiya.com/composite-design-pattern-in-modern-cpp/) *。如果你有兴趣接收我的最新文章，* [*请报名参加我的简讯*](http://eepurl.com/gDNybv) *。*

顺便说一句，如果你还没有看过我关于结构设计模式的其他文章，下面是列表:

1.  [**适配器**](http://www.vishalchovatiya.com/adapter-design-pattern-in-modern-cpp/)
2.  [桥**桥**桥](http://www.vishalchovatiya.com/bridge-design-pattern-in-modern-cpp/)
3.  [**复合**](http://www.vishalchovatiya.com/composite-design-pattern-in-modern-cpp/)
4.  [**装饰者**](http://www.vishalchovatiya.com/decorator-design-pattern-in-modern-cpp/)
5.  [**立面**](http://www.vishalchovatiya.com/facade-design-pattern-in-modern-cpp/)
6.  [**飞锤**](http://www.vishalchovatiya.com/flyweight-design-pattern-in-modern-cpp/)
7.  [**代理**](http://www.vishalchovatiya.com/proxy-design-pattern-in-modern-cpp/)

您在这一系列文章中看到的代码片段是简化的，而不是复杂的。所以你经常看到我不使用像`override`、`final`、`public`(同时继承)这样的关键字，只是为了让代码紧凑&可消耗(大部分时间)在单一标准屏幕尺寸。我也更喜欢`struct`而不是`class`，只是为了节省代码行，有时不写`public:`，还会故意忽略[虚拟析构函数](http://www.vishalchovatiya.com/part-3-all-about-virtual-keyword-in-c-how-virtual-destructor-works/)，构造函数[，复制构造函数](http://www.vishalchovatiya.com/all-about-copy-constructor-in-cpp-with-example/)，前缀`std::`，删除动态内存。我也认为自己是一个务实的人，希望用尽可能简单的方式，而不是标准的方式或使用术语来传达一个想法。

***注:***

*   如果你是在这里被直接绊倒的，那么我建议你浏览一下[什么是设计模式？](http://www.vishalchovatiya.com/what-is-design-pattern/)一、哪怕是鸡毛蒜皮的小事。相信会鼓励你对这个话题进行更多的探索。
*   您在本系列文章中遇到的所有这些代码都是使用 C++20 编译的(尽管我在大多数情况下使用了 C++17 之前的现代 C++特性)。因此，如果你无法获得最新的编译器，你可以使用已经预装了 boost 库的[https://wandbox.org/](https://wandbox.org/)。

# 目的

> **给物体增加附加功能。**

*   有时我们不得不在不重写或改变现有代码的情况下增加现有对象的功能，只是为了坚持[开闭原则](http://www.vishalchovatiya.com/open-closed-principle-in-cpp-solid-as-a-rock/)。这也保留了[单一责任原则](http://www.vishalchovatiya.com/single-responsibility-principle-in-cpp-solid-as-a-rock/)以获得额外的功能。

# C++中的装饰设计模式示例

*   为了实现这一点，我们在 C++中有两种不同的装饰设计模式:

1.  **动态装饰器**:通过引用或指针聚集被装饰的对象。
2.  **静态装饰器**:从被装饰对象继承。

# 动态装饰器

```
struct Shape {
    virtual operator string() = 0;
};struct Circle : Shape {
    float   m_radius; Circle(const float radius = 0) : m_radius{radius} {}
    void resize(float factor) { m_radius *= factor; }
    operator string() {
        ostringstream oss;
        oss << "A circle of radius " << m_radius;
        return oss.str();
    }
};struct Square : Shape {
    float   m_side; Square(const float side = 0) : m_side{side} {}
    operator string() {
        ostringstream oss;
        oss << "A square of side " << m_side;
        return oss.str();
    }
};
```

*   因此，我们有一个由两个不同的`Shape`组成的层级(即`Square` & `Circle` ) &我们希望通过添加颜色来增强这一层级。现在我们突然不打算创建另外两个类，例如彩色圆圈&一个彩色正方形。那太多了&不是一个可扩展的选项。
*   相反，我们可以有如下的`ColoredShape`。

```
struct ColoredShape : Shape {
    const Shape&    m_shape;
    string          m_color; ColoredShape(const Shape &s, const string &c) : m_shape{s}, m_color{c} {}
    operator string() {
        ostringstream oss;
        oss << string(const_cast<Shape&>(m_shape)) << " has the color " << m_color;
        return oss.str();
    }
};// we are not changing the base class of existing objects
// cannot make, e.g., ColoredSquare, ColoredCircle, etc.int main() {
    Square square{5};
    ColoredShape green_square{square, "green"};    
    cout << string(square) << endl << string(green_square) << endl;
    // green_circle.resize(2); // Not available
    return EXIT_SUCCESS;
}
```

***为什么这是一个动态装饰器？***
因为你可以通过提供需要的参数在运行时实例化`ColoredShape`。换句话说，您可以在运行时决定哪个`Shape`(即`Circle`或`Square`)将被着色。

*   您甚至可以像下面这样混合装饰器:

```
struct TransparentShape : Shape {
    const Shape&    m_shape;
    uint8_t         m_transparency; TransparentShape(const Shape& s, const uint8_t t) : m_shape{s}, m_transparency{t} {} operator string() {
        ostringstream oss;
        oss << string(const_cast<Shape&>(m_shape)) << " has "
            << static_cast<float>(m_transparency) / 255.f * 100.f
            << "% transparency";
        return oss.str();
    }
};int main() {
    TransparentShape TransparentShape{ColoredShape{Square{5}, "green"}, 51};
    cout << string(TransparentShape) << endl;
    return EXIT_SUCCESS;
}
```

## 动态装饰器的局限性

如果看一下`Circle`的定义，可以看到圆有一个方法叫`resize()`。我们不能像聚合基于接口`Shape` &那样使用这个方法，因为它只被其中暴露的方法所绑定。

# 静态装饰器

*   如果你不知道你要装饰哪个对象，你想在运行时选择它们，动态装饰器是很棒的，但是有时候你知道你在编译时想要的装饰器，在这种情况下你可以使用 C++模板继承的组合。

```
template <class T>  // Note: `class`, not typename
struct ColoredShape : T {
    static_assert(is_base_of<Shape, T>::value, "Invalid template argument"); // Compile time safety string      m_color; template <typename... Args>
    ColoredShape(const string &c, Args &&... args) : m_color(c), T(std::forward<Args>(args)...) { } operator string() {
        ostringstream oss;
        oss << T::operator string() << " has the color " << m_color;
        return oss.str();
    }
};template <typename T>
struct TransparentShape : T {
    uint8_t     m_transparency; template <typename... Args>
    TransparentShape(const uint8_t t, Args... args) : m_transparency{t}, T(std::forward<Args>(args)...) { } operator string() {
        ostringstream oss;
        oss << T::operator string() << " has "
            << static_cast<float>(m_transparency) / 255.f * 100.f
            << "% transparency";
        return oss.str();
    }
};int main() {
    ColoredShape<Circle> green_circle{"green", 5};
    green_circle.resize(2);
    cout << string(green_circle) << endl; // Mixing decorators
    TransparentShape<ColoredShape<Circle>> green_trans_circle{51, "green", 5};
    green_trans_circle.resize(2);
    cout << string(green_trans_circle) << endl;
    return EXIT_SUCCESS;
}
```

*   如你所见，我们现在可以调用`resize()`方法，这是动态装饰器的限制。你甚至可以像我们之前做的那样混合装饰者。
*   所以本质上，这个例子所展示的是，如果你准备放弃装饰器的动态组合特性，如果你准备在编译时定义所有的装饰器，你将获得使用继承的额外好处。
*   这样，你就可以通过装饰器&混合装饰器来访问你正在装饰的任何对象的成员。

# 用现代 C++实现装饰设计模式的函数方法

*   到目前为止，我们一直在谈论装饰者设计模式，它装饰了一个类，但是你也可以对函数做同样的事情。以下是一个典型的日志记录器示例:

```
// Need partial specialization for this to work
template <typename T>
struct Logger;// Return type and argument list
template <typename R, typename... Args>
struct Logger<R(Args...)> {
    function<R(Args...)>    m_func;
    string                  m_name; Logger(function<R(Args...)> f, const string &n) : m_func{f}, m_name{n} { } R operator()(Args... args) {
        cout << "Entering " << m_name << endl;
        R result = m_func(args...);
        cout << "Exiting " << m_name << endl;
        return result;
    }
};template <typename R, typename... Args>
auto make_logger(R (*func)(Args...), const string &name) {
    return Logger<R(Args...)>(std::function<R(Args...)>(func), name);
}double add(double a, double b) { return a + b; }int main() {
    auto logged_add = make_logger(add, "Add");
    auto result = logged_add(2, 3);
    return EXIT_SUCCESS;
}
```

*   上面的例子对你来说可能有点复杂，但是如果你对 variadic temple 有一个清晰的了解，那么它不会花超过 30 秒来理解这里发生了什么。

# 装饰设计模式的好处

1.  Decorator 有助于在运行时和编译时增加现有对象的功能。
2.  Decorator 还提供了以任何顺序添加任意数量装饰器的灵活性&混合使用。
3.  装饰器是解决排列问题的好办法，因为你可以用任意数量的装饰器包装一个组件。
4.  对已经发布的代码应用装饰设计模式是一个明智的选择。因为它支持应用程序的向后兼容性&更少的单元级测试，因为更改不会影响代码的其他部分。

# 常见问题汇总

什么时候使用装饰设计模式？

—当您需要能够在运行时向对象分配额外的行为，而不破坏使用这些对象的代码时，请使用装饰设计模式。
—当类有 [final](https://en.cppreference.com/w/cpp/keyword/final) 关键字时，这意味着该类不能被进一步继承。在这种情况下，装饰设计模式可能会有所帮助。

**使用装饰设计模式有什么弊端？**

—decorator 可能会使组件的实例化过程变得复杂，因为您不仅要实例化组件，还要将它包装在许多 decorator 中。
—装饰设计模式的过度使用可能会使系统在两方面变得复杂，即维护&学习曲线。

**适配器&装饰器设计模式的区别？**

— **适配器更改现有对象的接口**
—**装饰器增强现有对象的接口**

**Proxy&Decorator 设计模式的区别？**

— **代理**提供了一个有些相同或者**简单的接口**T21—**装饰**提供了**增强的接口**