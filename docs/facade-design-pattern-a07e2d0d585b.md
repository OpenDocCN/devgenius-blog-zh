# 立面设计模式

> 原文：<https://blog.devgenius.io/facade-design-pattern-a07e2d0d585b?source=collection_archive---------6----------------------->

# 概观

外观设计的主要思想是隐藏应用程序的复杂性。用户将使用接口/外观来访问任何方法。

![](img/e133805a26258e1fb1b03c0deaafcf06.png)

丹尼尔·冯·阿彭在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上的照片

# 履行

## 连接

```
// Animal.java
public interface Animal {
   void species();
}
```

具体类的接口。

## 类实现

```
// Dog.java
public class Dog implements Animal {
   @Override
   public void species() {
      System.out.println("Dog");
   }
}
```

接口实现。

```
// Cat.java
public class Cat implements Animal {
   @Override
   public void species() {
      System.out.println("Cat");
   }
}// Bird.java
public class Bird implements Animal {
   @Override
   public void species() {
      System.out.println("Bird");
   }
}
```

## 外表

```
// AnimalMaker.java
public class AnimalMaker {
   private Animal dog;
   private Animal cat;
   private Animal bird;

   public AnimalMaker() {
      dog = new Dog();
      cat = new Cat();
      bird = new Bird();
   }

   public void dogSpecies(){
      dog.species();
   }
   public void catSpecies(){
      cat.species();
   }
   public void birdSpecies(){
      bird.species();
   }
}
```

用户访问具体类方法的门面。

## 演示

```
// Demo.java
public class Demo {
   public static void main(String[] args) {
      AnimalMaker animalMaker = new AnimalMaker();

      animalMaker.dogSpecies();
      animalMaker.catSpecies();
      animalMaker.birdSpecies();		
   }
}>> Dog
>> Cat
>> Bird
```

您可以通过外观调用它们的方法，而不是创建具体的类。

# 结论

如果你有一个复杂的系统，你可能会使用 facade 模式。您可能希望隐藏复杂性，并公开一种简化的方式供他人使用。