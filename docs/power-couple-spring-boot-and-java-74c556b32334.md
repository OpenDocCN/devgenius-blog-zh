# Spring Boot 和爪哇的电力耦合

> 原文：<https://blog.devgenius.io/power-couple-spring-boot-and-java-74c556b32334?source=collection_archive---------9----------------------->

![](img/b257ec18b9d793168315ef019d2a879d.png)

在学 Java 之前，我在学校学过 C#。这对我来说既简单又高效。我觉得我可以用它做任何事。

在我大学的最后一年，我们学习了 java，它看起来非常难学，因为 C#看起来更紧凑*(至少对我来说)*而且容易学，你不需要做太多。但是我喜欢 Java，我想做得更多。

上了几节 java 课之后，我们的老师给我们展示了抽象类，我记得我们的课堂一片死寂，没有人理解它，我们只是从老师的电脑上复制代码，然后给老师*看，“是的，我们成功了，它成功了。”*但是不，我们没有这样做，然后我想*“我永远不会用这个垃圾，谁在乎我可以不用它写代码。”是的，我是个哑巴…*

在专业领域，每个应用程序都很大，不管它是什么，因为在学校，他们向我们展示了编码世界的一小部分，没有足够的时间来展示它的一半。

大型应用程序更加复杂，因此你需要使用一个好的逻辑来提高效率。有大量的策略和模式可以使用。你用得越多，你就会本能地自动去做。你在[这里](https://www.javatpoint.com/design-patterns-in-java)看设计模式。我不会解释策略或设计模式。

我将向您展示当我必须用 2 个不同的函数类型执行 2 个不同的类类型时我在做什么，我不会使用 *if-else* 或 *switch-case。*挺野的吧？

我们有一个对象，它有 4 种不同的类型*“A-first function，A-SecondFunction，B-FirstFunction 和 B-SecondFunction”。*当 A-FirstFunction 出现时，我想触发 ClassA.firstFunction()，但 B-SecondFunction 出现时，我想触发 ClassB.secondFunction()，通常你可以这样做，只需放置 if else 并通过的*instance 检查对象，就完成了。但是有更有效的方法可以做到这一点。*

我告诉过你我永远不会使用抽象类，对吗？嗯……在公司和更大的项目中工作，抽象类成了我每天使用的东西。它非常好用，让我的工作变得更加容易。

> 当你认为你永远不会使用某样东西，因为它很难或很难学的时候，你应该更加注意这一点。因为学难的东西会让你的生活更轻松。

所以说够了…

首先，我们需要创建 ClassA 和 ClassB。它们都有两个函数，所以我要创建一个抽象类，并把它作为父类。

```
public abstract class AbstractClass {

    private Map<String, Function<String, String>> functionalMap = new HashMap<>();

public AbstractClass() {
    this.functionalMap.put("first", firstFunction());
    this.functionalMap.put("second", secondFunction());
  }

protected abstract Function<String, String> firstFunction();

protected abstract Function<String, String> secondFunction();

public Map<String, Function<String, String>> getFunctionalMap() {
    return functionalMap;
  }
}
```

在这个抽象类中，我们有两个函数。我使用 java [函数接口](https://www.baeldung.com/java-8-functional-interfaces)，用它我可以创建一个值可以被函数化的映射。你想通了，对吗？

然后我们创建另外两个类；a 类和 b 类。

```
@Service("A")
public class ClassA extends AbstractClass {

    @Override
    protected Function<String, String> firstFunction() {
        return "Class A:First "::concat;
    }

    @Override
    protected Function<String, String> secondFunction() {
        return "Class A:First "::concat;
    }
}@Service("B")
public class ClassB extends AbstractClass {

    @Override
    protected Function<String, String> firstFunction() {
        return "Class B:First "::concat;
    }

    @Override
    protected Function<String, String> secondFunction() {
        return "Class B:Second "::concat;
    }
}
```

使用 spring boot，我将它们创建为一个 bean，并给它们起了一个名字，它们在函数中使用了自己独特的进程。

非常经典的编码，对吗？但是魔法从这里开始…

通常当你想启动 ClassA.firstFunction()时，你只需要这样做；

```
@Autowired
private ClassA classA;
...
classA.firstFunction().apply("value");
...
```

但是不，我们不会用那个。这就是为什么 Spring Boot 和爪哇是一对强有力的夫妇。

我们用 AbstractClass 和 spring boot 的魔法扩展了我们的类，我将把子类放到地图中，并按名称调用它们。

```
@Component
public class Executor {

    private final Map<String, AbstractClass> abstractClassMap;

    public Executor(Map<String, AbstractClass> abstractClassMap) {
        this.abstractClassMap = abstractClassMap;
    }

    ...
}
```

通过这样做*(或自动布线)* spring 为我们做了所有的工作。Spring 按名称将 AbstractClass 的孩子放入映射中，您可以在@Service("name ")中按指定的名称获取他们。所以如果你想得到 A 级，你可以像这样。

```
AbstractClass aClass = abstractClassMap.get("A");
```

这样你就可以得到并执行你的函数。但是这还没有完成…

在 AbsctractClass 中，我们有一个映射，我放置了一些函数，因为我不想像*a class . first class()那样执行它们。【值】*应用。

我可以用这个来处决他们。*“getFunctionalMap()。get(“第一”)。*【值】【应用】

所以我们需要做的就是在 executor 类中放入最后一个函数；

```
...
public String init(String className, String functionName, String value) {
    return abstractClassMap.get(className).getFunctionalMap().get(functionName).apply(value);
}
...
```

在这个函数中，您可以使用要处理的值执行给定的类名和给定的函数名。

```
Executor executor = run.getBean(Executor.class);
System.*out*.println(executor.init("A","second","value"));
```

有了这一行代码，你可以调用无限的类和函数。我使用这种技术的次数比任何东西都多*(当然如果我需要的话)*。