# 面向对象编程 5 分钟

> 原文：<https://blog.devgenius.io/object-oriented-programing-in-5-minutes-421993786719?source=collection_archive---------0----------------------->

![](img/f75764540c34eb5ecaf22a6d5cbe90de.png)

拉格斯技术人员在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄的照片

当谈到面向对象编程时，我发现许多教程使这个概念的理解变得复杂。要想很好的理解它，先来理解它解决的问题。

在过去，我们有程序设计，它将一个程序分成一组函数，这些函数对数据进行操作。

但是随着程序的增长，我们得到了许多相互关联的功能。

当我们对一个功能进行更改时，这可能会破坏其他功能，这是有问题的。

因此，面向对象编程(OOP)来解决这个问题。它有 4 个小概念:封装、抽象、继承和多态。

# 1-封装

这意味着将一组相关的字段和函数创建到一个称为对象的单元中。

我们将这些字段称为属性，将这些函数称为方法。

**举例:**

教师有一些属性，例如基本工资、加班时间、工资率；有一些方法，例如 get wage()；还有一些设置器，用于更新属性值；还有一些获取器，用于获取属性值。

```
package com.selcote.abstraction;public class Employee{
    private float baseSalary;
    private float rate;
    private float overtime;

    public float getWage(){
        return this.baseSalary + (rate * overtime);
    }

    public float getBaseSalary() {
        return baseSalary;
    }

    public void setBaseSalary(float baseSalary) {
        this.baseSalary = baseSalary;
    }

    public float getRate() {
        return rate;
    }

    public void setRate(float rate) {
        this.rate = rate;
    }

    public float getOvertime() {
        return overtime;
    }

    public void setOvertime(float overtime) {
        this.overtime = overtime;
    }

}
```

正如您所看到的，getWage()方法没有参数，它访问对象属性并对它们进行操作，而不需要向它传递参数。这是最佳实践之一，拥有不带参数的方法。

> 参数数量越少，该功能越容易使用和维护。

除此之外，我们将属性设为私有，我们可以通过传递 setters 和 getters 来访问它们，这**隐藏了类中结构化数据对象**的值或状态，并防止未授权方直接访问它们。

# 2-抽象

这种技术是对外界隐藏细节，这给了我们很多好处。

我们可以通过使对象方法私有来隐藏它们，并暴露更少的公共方法。

因此，通过这样做，我们使我们的对象接口更简单。

> 使用和理解具有较少方法和属性的对象比具有许多方法和属性的对象要好。

此外，这个概念减少了变更的影响，这意味着如果您更改任何私有或内部方法，这将不会泄漏到外部。因为这些方法或属性都不为该包含对象的外部所知。

此外，我们可以只向外部公开一个接口，实现将在运行时定义。

示例:

```
package com.selcote.abstraction; public interface Car {  
 void turnOn();   
 void turnOff();   
} package com.selcote.abstraction;public class ManualCar implements Car {      @Override  
   public void turnOn() {  
     System.out.println("turn on the manual car");  
   }      @Override  
   public void turnOff() {   
     System.out.println("turn off the manual car");  
   }  

}package com.selcote.abstraction;public class AutomaticCar implements Car { 

    @Override  
    public void turnOnCar() {   
       System.out.println("turn on the automatic car");  
    }   
    @Override  
    public void turnOffCar() {    
      System.out.println("turn off the automatic car");   
    }  
}
```

让我们测试一下汽车的功能:

```
package com.selcote.abstraction;public class CarTest {  
 public static void main(String[] args) {          Car car1 = new ManualCar();   
       Car car2 = new AutomaticCar();    
       car1.turnOnCar();   
       car1.turnOffCar();     
       car2.turnOnCar();   
       car2.turnOffCar();  

 }  
}
```

输出:

```
turn on the manual car
turn off the manual car
turn on the automatic car
turn off the automatic car
```

***客户端程序只知道汽车和汽车提供的功能。***

***内部实现细节对客户端程序隐藏。***

# 3-继承

这种机制允许我们消除冗余代码。

让我们考虑一下 html 元素，比如文本框、选择框、复选框等等。这些元素有一些共同点，如 hidden，innerHtml 属性和方法，如 click()和 focus()。

我们可以在一个名为 Html element 的通用对象中定义这些属性和方法，并让其他对象继承它，而不是为每种类型的 Html 元素重新定义这些属性和方法。

# 4-多形性

如果我们试图理解这个构成名称，Poly 表示许多，morphism 表示形式，所以:

> 多形=多种形式

这是一种允许去掉 **i *f 和 else 或者转换语句的技术。***

因此，多态性是一个方法根据它所作用的对象做不同事情的能力。换句话说，多态性允许您定义一个接口并拥有多个实现。

示例:

```
public class Animal{
   public void sound(){
      System.out.println("Animal is making a sound");   
   }
}class Dog extends Animal{
    @Override
    public void sound(){
        System.out.println("Barking");
    }
    public static void main(String args[]){
    	Animal obj = new Dog();
    	obj.sound();
    }
}
```

输出:

```
Barkingclass Cat extends Animal{
    @Override
    public void sound(){
        System.out.println("Meow");
    }
    public static void main(String args[]){
    	Animal obj = new Cat();
    	obj.sound();
    }
}
```

输出:

```
Meow
```

# 结论:

OOP 通过解决许多问题简化了编程领域:

封装= >降低复杂性+提高可重用性

抽象= >降低复杂性+降低代码变化的影响

继承= >消除冗余代码

多态= >重构 if/switch case 语句

请在评论中与我分享您的想法，并为之鼓掌:-)

另外，考虑在 medium 上关注我，订阅我的频道: [Selcote(问题解决社区)](https://www.youtube.com/channel/UCAHhPG4g-alLRGFjk8rXyIA)获取更多精彩内容。

# 推荐书籍

## [干净的代码:敏捷软件技术手册](https://amzn.to/3j1lNWx)

## [头脑优先设计模式:一个对大脑友好的指南](https://amzn.to/3vG0Zbl)

## [干净的建筑](https://amzn.to/3cVvkfy)

# 计算机和显示器

## [新款苹果 MacBook Pro](https://amzn.to/3wOP10M)

## [戴尔 27 英寸 Ultrasharp U2719D 显示器](https://amzn.to/3zIcT7M)

## [双臂支架桌面支架](https://amzn.to/2SNdHI3)

## [USB C Hub 多端口适配器](https://amzn.to/2VfxiS6)

## 我用于编码的 IDE:

- IntelliJ
- Vscode