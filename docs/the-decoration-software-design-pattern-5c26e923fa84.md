# 装饰软件设计模式

> 原文：<https://blog.devgenius.io/the-decoration-software-design-pattern-5c26e923fa84?source=collection_archive---------2----------------------->

![](img/a38527dce1a7926f9897de2be01618b3.png)

安妮·斯普拉特在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上的照片

Decorator 是一种结构设计模式，它允许通过将对象放在包含这些行为的特定包装对象中，将新行为附加到对象上。

为了理解这种设计模式，我们将假设我们正在为一家意大利餐馆运行一个应用程序，并且我们需要对膳食类进行建模。假设他们提供三种比萨饼:玛格丽塔比萨饼、那不勒斯比萨饼和油炸比萨饼。除此之外，每顿饭都可以选择添加沙拉和饮料。最初，我们将使用继承并抽象出 dish 类中的公共功能。

```
public interface Pizza {
    void prepare();
}public class MargheritaPizza implements Pizza{
    @Override
    public void prepare() {
        System.*out*.println("Margherita Pizza in the works!");
    }
}public class NapoletanaPizza implements Pizza {

    @Override
    public void prepare() {
        System.*out*.println("Napoletana Pizza in the works!");
    }
}public class FrittaPizza implements Pizza{
    @Override
    public void prepare() {
        System.*out*.println("Fritta Pizza in the works!");
    }
}
```

PizzaMealDecorator.java

这是我们将用于装饰/包装的基类。这个类实现了 pizza 接口，从而包装了 Pizza 对象。

```
public class PizzaMealDecorator implements Pizza{
    protected Pizza pizza;

    public PizzaMealDecorator(Pizza pizza){
        this.pizza = pizza;
    }

    @Override
    public void prepare() {
        this.pizza.prepare();
    }
}
```

Drink.java

```
public class Drink extends PizzaMealDecorator{
    public Drink(Pizza pizza) {
        super(pizza);
    }

    @Override
    public void prepare() {
        super.prepare();
        System.*out*.println("Adding a drink to the Pizza meal.");
    }
}
```

Salad.java

```
public class Salad extends PizzaMealDecorator {
    public Salad(Pizza pizza) {
        super(pizza);
    }

    @Override
    public void prepare() {
        super.prepare();
        System.*out*.println("Adding a salad to the Pizza meal.");
    }
}
```

演示

```
public class Demo {

public static void main(String[] args){
        PizzaMealDecorator margheritaPizzaMeal = new Salad(new   Drink(new MargheritaPizza()));
        margheritaPizzaMeal.prepare();
        System.*out*.println("-----------------");
        PizzaMealDecorator napolentanaPizzaMeal = new Drink(new NapoletanaPizza());
        napolentanaPizzaMeal.prepare();
        System.*out*.println("-----------------");
        PizzaMealDecorator frittaPizzaMeal = new      PizzaMealDecorator(new NapoletanaPizza());
       frittaPizzaMeal.prepare();
        System.*out*.println("-----------------");
    }
}
```

输出

```
Margherita Pizza in the works!
Adding a drink to the Pizza meal.
Adding a salad to the Pizza meal.
-----------------
Napoletana Pizza in the works!
Adding a drink to the Pizza meal.
-----------------
Napoletana Pizza in the works!
-----------------
```

# 赞成的意见

*   单一责任原则
*   您可以扩展一个对象的行为，而无需创建新的子类。
*   您可以在运行时添加或删除对象的责任。
*   通过将一个对象包装到多个装饰器中，可以组合几种行为。

# 骗局

*   很难从包装器堆栈中移除特定的包装器。因此，在我们的示例中，很难在添加时删除订单中的沙拉。
*   很难实现一个装饰器，使其行为不依赖于装饰器堆栈中的顺序。
*   层的初始配置代码可能看起来很难看。