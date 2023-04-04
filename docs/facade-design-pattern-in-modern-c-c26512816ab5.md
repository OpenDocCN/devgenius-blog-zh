# 现代 C++中的外观设计模式

> 原文：<https://blog.devgenius.io/facade-design-pattern-in-modern-c-c26512816ab5?source=collection_archive---------11----------------------->

![](img/cef05c09a80f90c1b9d477968e86d792.png)

Facade 设计模式是使用 ***为复杂系统*** 提供统一接口的结构化设计模式。这与建筑结构中的外观相同，外观是一个对象，作为一个面向前面的接口，掩盖了一个更复杂的底层系统。C++中的外观设计模式可以:

*   通过提供单一简化的 API，屏蔽与更复杂组件的交互，提高软件库的可读性和可用性。
*   为更通用的功能提供特定于上下文的接口。
*   作为更广泛地重构整体式或紧耦合系统的起点，以支持更松散耦合的代码。坦白说，连我都不明白这一点。我刚刚从维基百科上复制了这个。

> */！\:本文已原创发表于我的* [*博客*](http://www.vishalchovatiya.com/facade-design-pattern-in-modern-cpp/) *。如果你有兴趣接收我的最新文章，* [*请报名参加我的简讯*](http://eepurl.com/gDNybv) *。*

在我们继续之前，让我纠正一下 Facade 的拼写，即“faade”&它读作“fa；sa；d”。加在字母 C 下面的一个钩或尾，称为“尖音”,在大多数欧洲语言中用来表示该字母发音的变化。我们就不细说了，否则就跑题了。

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

*   如果你是在这里被直接绊倒的，那么我建议你浏览一下[什么是设计模式？](http://www.vishalchovatiya.com/what-is-design-pattern/)一、哪怕是鸡毛蒜皮的小事。相信会鼓励你对这个话题进行更多的探索。
*   您在本系列文章中遇到的所有这些代码都是使用 C++20 编译的(尽管我在大多数情况下使用了 C++17 的[现代 C++](http://www.vishalchovatiya.com/21-new-features-of-modern-cpp-to-use-in-your-project/) 特性)。因此，如果你无法获得最新的编译器，你可以使用[https://wandbox.org/](https://wandbox.org/)，它也预装了 boost 库。

# 目的

> 通过隐藏系统复杂性来提供统一的接口。

*   这是迄今为止我遇到的最简单最容易的设计模式。
*   换句话说，外观设计模式就是在大量复杂的代码上提供一个简单且易于理解的界面。

# C++中的外观设计模式示例

*   想象一下，你建了一个智能房子，所有东西都在遥控器上。所以，要打开灯，你需要按下按钮——电视、空调、闹钟、音乐等等也是如此……
*   当你离开一所房子的时候，你需要按 100 个按钮来确定一切都关闭了&如果你像我一样懒，这可能会有点烦人。
*   所以我为离开和回来定义了一个门面。(外观函数代表按钮……)。所以当我来和离开的时候，我只需要打一个电话&一切都会搞定…

```
// Stolen from: https://en.wikibooks.org/wiki/C%2B%2B_Programming/Code/Design_Patterns
struct Alarm {
    void alarm_on()    { cout << "Alarm is on and house is secured"<<endl; }
    void alarm_off() { cout << "Alarm is off and you can go into the house"<<endl; }
};struct Ac {
    void ac_on()    { cout << "Ac is on"<<endl; }
    void ac_off()    { cout << "AC is off"<<endl; }
};struct Tv {
    void tv_on()    { cout << "Tv is on"<<endl; }
    void tv_off()    { cout << "TV is off"<<endl; }
};struct HouseFacade {
    void go_to_work() {
        m_ac.ac_off();
        m_tv.tv_off();
        m_alarm.alarm_on();
    }
    void come_home() {
        m_alarm.alarm_off();
        m_ac.ac_on();
        m_tv.tv_on();
    }
private:
    Alarm   m_alarm;
    Ac      m_ac;
    Tv      m_tv;
};int main() {
    HouseFacade hf;
    //Rather than calling 100 different on and off functions thanks to facade I only have 2 functions...
    hf.go_to_work();
    hf.come_home();
    return EXIT_SUCCESS;
}
```

*   请注意，我们刚刚将不同的不相关/有点相关的类组合成了`HouseFacade`。我们还可以使用多态`turn_on()` & `turn_off()`方法的接口，在各自的[子类](http://www.vishalchovatiya.com/memory-layout-of-cpp-object/)中覆盖，创建一个`Ac`、`Tv`、`Alarm`对象的集合，以添加[复合设计模式](http://www.vishalchovatiya.com/composite-design-pattern-in-modern-cpp/)来实现更复杂的功能。
*   但是这将使系统更加复杂，增加学习曲线。这与最初使用的外观设计模式完全相反。

# 立面设计模式的好处

1.  Facade 定义了一个更高级别的接口，通过包装复杂的子系统，使子系统更容易使用。
2.  这减少了成功利用子系统所需的学习曲线。

# 常见问题汇总

**Facade 是包含很多其他类的类吗？**

是的。它是应用程序中许多子系统的包装器。

**是什么让它成为设计模式？对我来说，它就像一个正常的班级。**

所有的设计模式也是普通的类。

**门面设计模式的实际用例是什么？**

门面设计模式的一个典型应用是控制台/终端/命令提示符，你会发现在 Linux 或 Windows 中这是访问操作系统提供的机器功能的统一方式。

**适配器&立面设计模式的区别？**

适配器包装一个类，而外观可能代表许多类