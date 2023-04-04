# 现代 C++中的生成器设计模式

> 原文：<https://blog.devgenius.io/builder-design-pattern-in-modern-c-7ca0e259e1b4?source=collection_archive---------11----------------------->

![](img/c77d9887234379f0373529f0acd74219.png)

在软件工程中，创造性的设计模式处理对象创建机制，试图以适合情况的方式创建对象。对象创建的基本或普通形式会导致设计问题或增加设计的复杂性。C++中的 Builder 设计模式通过 ***将复杂对象的构造与其表示*** 分离来解决这个具体问题。

> */！\:这篇文章最初发表在我的博客上。如果你有兴趣接收我的最新文章，* [*请报名参加我的简讯*](http://eepurl.com/gDNybv) *。*

顺便说一句，如果你还没有看过我关于创造性设计模式的其他文章，那么下面是列表:

1.  [**工厂**](http://www.vishalchovatiya.com/factory-design-pattern-in-modern-cpp/)
2.  [**建造者**](http://www.vishalchovatiya.com/builder-design-pattern-in-modern-cpp/)
3.  [**原型**](http://www.vishalchovatiya.com/prototype-design-pattern-in-modern-cpp/)
4.  [**单胎**](http://www.vishalchovatiya.com/singleton-design-pattern-in-modern-cpp/)

您在这一系列文章中看到的代码片段是简化的，而不是复杂的。所以你经常看到我不使用像`override`、`final`、`public`(同时继承)这样的关键字，只是为了让代码紧凑&可消耗(大部分时间)在单一标准屏幕尺寸。我也更喜欢`struct`而不是`class`，只是为了节省代码行，有时不写`public:`，也故意忽略[虚析构函数](http://www.vishalchovatiya.com/part-3-all-about-virtual-keyword-in-c-how-virtual-destructor-works/)，构造函数[，复制构造函数](http://www.vishalchovatiya.com/all-about-copy-constructor-in-cpp-with-example/)，前缀`std::`，删除动态内存。我也认为自己是一个务实的人，希望用尽可能简单的方式，而不是标准的方式或使用术语来传达一个想法。

***注:***

*   如果你直接来到这里，那么我建议你浏览一下[什么是设计模式？](http://www.vishalchovatiya.com/what-is-design-pattern/)第一，哪怕是微不足道的小事。相信会鼓励你对这个话题进行更多的探索。
*   您在本系列文章中遇到的所有这些代码都是使用 C++20 编译的(尽管我在大多数情况下使用了 C++17 的[现代 C++](http://www.vishalchovatiya.com/21-new-features-of-modern-cpp-to-use-in-your-project/) 特性)。所以如果你没有最新的编译器，你可以使用已经预装了 boost 库的 https://wandbox.org/。

# 目的

> **通过在单独的实体中提供 API，简洁地创建/实例化复杂的&复杂的对象分段&。**

*   当我们想要构造一个复杂的对象时，使用构建器设计模式。然而，我们不希望有一个复杂的构造函数成员或者一个需要很多参数的成员。
*   构建器设计模式逐步构建一个复杂的对象&最后一步将返回该对象。构造一个对象的过程应该是通用的，这样它就可以在各种方法的帮助下创建同一对象的不同表示。

# 没有建设者的生活

*   假设您必须使用 C++创建 HTML 生成器，那么非常简单的方法就是:

```
// <p>hello</p>
auto text = "hello";
string output;
output += "<p>";
output += text;
output += "</p>";
printf("<p>%s</p>", text);// <ul><li>hello</li><li>world</li></ul>
string words[] = {"hello", "world"};
ostringstream oss;
oss < "<ul>";
for (auto w : words)
    oss < "  <li>" < w < "</li>";
oss < "</ul>";
printf(oss.str().c_str());
```

*   一个成熟的开发人员会创建一个带有一堆构造函数参数和方法的类来添加一个子节点。这是一个很好的方法，但是它可能会使对象表示复杂化。
*   一般来说，有些对象很简单&可以在一次构造函数调用中创建，而其他对象则需要大量的创建过程。
*   拥有一个有 10 个构造函数参数的对象是没有效率的。相反，我们应该选择分段施工。
*   Builder 提供了一个 API，用于一步一步地构造对象，而不显示实际的对象表示。

# 现代 C++中的生成器设计模式示例

```
class HtmlBuilder;class HtmlElement {
    string                      m_name;
    string                      m_text;
    vector<HtmlElement>         m_childs;
    constexpr static size_t     m_indent_size = 4; HtmlElement() = default;
    HtmlElement(const string &name, const string &text) : m_name(name), m_text(text) {}
    friend class HtmlBuilder;public:
    string str(int32_t indent = 0) {
        ostringstream oss;
        oss << string(m_indent_size * indent, ' ') << "<" << m_name << ">" << endl; if (m_text.size()) oss << string(m_indent_size * (indent + 1), ' ') << m_text << endl; for (auto &element : m_childs)
            oss << element.str(indent + 1); oss << string(m_indent_size * indent, ' ') << "</" << m_name << ">" << endl;
        return oss.str();
    }
    static unique_ptr<HtmlBuilder> build(string root_name) { return make_unique<HtmlBuilder>(root_name); }
};class HtmlBuilder {
    HtmlElement     m_root;public:
    HtmlBuilder(string root_name) { m_root.m_name = root_name; }
    HtmlBuilder *add_child(string child_name, string child_text) {
        m_root.m_childs.emplace_back(HtmlElement{child_name, child_text});
        return this;
    }
    string str() { return m_root.str(); }
    operator HtmlElement() { return m_root; }
};int main() {
    auto builder = HtmlElement::build("ul");
    builder->add_child("li", "hello")->add_child("li", "world"); cout << builder->str() << endl;
    return EXIT_SUCCESS;
}
/*
<ul>
    <li>
        hello
    </li>
    <li>
        world
    </li>
</ul>
*/
```

*   我们通过将`HtmlElements`的数据成员设为私有来强制用户使用 builder。
*   正如你所看到的，我们已经在同一个文件&中声明了`HtmlBuilder` & `HtmlElement`这样做，我们需要向前声明，即`class HtmlBuilder;`，因为它是一个[不完整类型](https://docs.microsoft.com/en-us/cpp/c-language/incomplete-types)。在编译器解析它的实际声明之前，我们不能创建不完整类型的对象。原因很简单，编译器需要一个对象的大小来为它分配内存。因此指针是唯一的方法，所以我们采取了`[unique_ptr](http://www.vishalchovatiya.com/understanding-unique-ptr-with-example-in-cpp11/).`

# 复杂而流畅的构建器设计模式示例

*   下面是构建器设计模式的更复杂的例子，它被组织在四个不同的文件中(即`Person.h`、`Person.cpp`、`PersonBuilder.h`、`PersonBuilder.cpp`)。

`**Person.h**`

```
#pragma once
#include <iostream>
using namespace std;class PersonBuilder;class Person
{
    std::string m_name, m_street_address, m_post_code, m_city;  // Personal Detail
    std::string m_company_name, m_position, m_annual_income;    // Employment Detail Person(std::string name) : m_name(name) {}public:
    friend class PersonBuilder;
    friend ostream& operator<<(ostream&  os, const Person& obj);
    static PersonBuilder create(std::string name);
};
```

`**Person.cpp**`

```
#include <iostream>
#include "Person.h"
#include "PersonBuilder.h"PersonBuilder Person::create(string name) { return PersonBuilder{name}; }ostream& operator<<(ostream& os, const Person& obj)
{
    return os << obj.m_name
              << std::endl
              << "lives : " << std::endl
              << "at " << obj.m_street_address
              << " with postcode " << obj.m_post_code
              << " in " << obj.m_city
              << std::endl
              << "works : " << std::endl
              << "with " << obj.m_company_name
              << " as a " << obj.m_position
              << " earning " << obj.m_annual_income;
}
```

*   从上面的例子可以看出`Person`可能有很多类似个人&的专业细节。数据成员的计数也是如此。
*   在我们的例子中，有 7 个数据成员。为通过构造函数创建一个人所需的所有操作创建一个类可能会使我们的类变得臃肿&失去它最初的目的。此外，库用户需要注意所有那些构造函数参数序列。

`**PersonBuilder.h**`

```
#pragma once
#include "Person.h"class PersonBuilder
{
    Person person;public:
    PersonBuilder(string name) : person(name) {} operator Person() const { return move(person); } PersonBuilder&  lives();
    PersonBuilder&  at(std::string street_address);
    PersonBuilder&  with_postcode(std::string post_code);
    PersonBuilder&  in(std::string city);
    PersonBuilder&  works();
    PersonBuilder&  with(string company_name);
    PersonBuilder&  as_a(string position);
    PersonBuilder&  earning(string annual_income);
};
```

`**PersonBuilder.cpp**`

```
#include "PersonBuilder.h"PersonBuilder&  PersonBuilder::lives() { return *this; }PersonBuilder&  PersonBuilder::works() { return *this; }PersonBuilder&  PersonBuilder::with(string company_name) {
    person.m_company_name = company_name; 
    return *this;
}PersonBuilder&  PersonBuilder::as_a(string position) {
    person.m_position = position; 
    return *this;
}PersonBuilder&  PersonBuilder::earning(string annual_income) {
    person.m_annual_income = annual_income; 
    return *this;
}PersonBuilder&  PersonBuilder::at(std::string street_address) {
    person.m_street_address = street_address; 
    return *this;
}PersonBuilder&  PersonBuilder::with_postcode(std::string post_code) {
    person.m_post_code = post_code; 
    return *this;
}PersonBuilder&  PersonBuilder::in(std::string city) {
    person.m_city = city; 
    return *this;
}
```

*   我们可以将所有这些与构造相关的 API 塞进`Person`，将任务委托给单独的实体，即`PersonBuilder`。

`**Main.cpp**`

```
#include <iostream>
#include "Person.h"
#include "PersonBuilder.h"
using namespace std;int main()
{
    Person p = Person::create("John")
                                .lives()
                                    .at("123 London Road")
                                    .with_postcode("SW1 1GB")
                                    .in("London")
                                .works()
                                    .with("PragmaSoft")
                                    .as_a("Consultant")
                                    .earning("10e6"); cout << p << endl;
    return EXIT_SUCCESS;
}
```

*   上面的结构看起来不是更直观、自然&简单明了吗？
*   如果你关注的是`lives()` & `works()`这样的空白方法，那么不用担心，它会在优化中被淘汰。
*   你还可以观察到，我们通过将构造函数私有&只暴露`create**(**std::string name**)**` API，迫使用户使用构建器而不是构造函数。
*   除非需要，否则不要通过设计接口或抽象类来使事情过于复杂。我在网上的许多构建器设计模式例子中看到过这一点。

# 生成器设计模式的好处

*   在构建器模式中，代码行数至少增加了一倍。但是这种努力在设计灵活性、构造函数的参数更少或没有参数以及可读性更高的代码方面得到了回报。
*   构建器设计模式还有助于最小化构造器中的参数数量&因此不需要为构造器的可选参数传递空值。
*   在对象构建过程中，不可变对象的构建不需要太多复杂的逻辑。
*   将构造与[对象表示](http://www.vishalchovatiya.com/memory-layout-of-cpp-object/)分离使得对象表示切片&精确。拥有一个独立的构建器实体提供了创建实例化不同对象表示的灵活性。

# 常见问题汇总

**什么时候应该使用构建器设计模式？**

每当创建新对象时，需要设置许多参数，其中一些(或全部)是可选的。

**为什么我们在实现构建器设计模式时需要一个构建器类？**

这不是必须的，但是这样做有一些好处:
-根据 [SRP](http://www.vishalchovatiya.com/single-responsibility-principle-in-cpp-solid-as-a-rock/) ，构建对象的关注点应该在单独的实体中。
-原物不会臃肿。
-易维护代码&。
-测试&理解一个有许多输入参数的构造函数变得更加复杂。

**建筑设计模式的最大优势！**

更有表现力的代码。
`MyClass o = new MyClass(5, 5.5, 'A', var, 1000, obj9, "hello");`
——改为
`MyClass o = MyClass.builder().a(5).b(5.5).c('A').d(var).e(1000).f(obj9).g("hello");`
——你可以看到哪个数据成员正在被什么&赋值甚至改变赋值的顺序。

[有什么建议、疑问或者想说](http://www.vishalchovatiya.com/contact-2/) `[Hi](http://www.vishalchovatiya.com/contact-2/)` [？减轻压力，只需点击一下鼠标。](http://www.vishalchovatiya.com/contact-2/) 🖱️