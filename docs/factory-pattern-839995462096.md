# 工厂设计模式

> 原文：<https://blog.devgenius.io/factory-pattern-839995462096?source=collection_archive---------3----------------------->

# 概观

工厂模式是 Java 中流行的设计模式，因为它是创建对象的最佳方式之一。总的想法是允许用户创建对象而不暴露其背后的逻辑。相反，为用户提供了一个界面来知道调用什么方法来创建对象。

![](img/aca59c4273c418c63a08fda1edc06f56.png)

在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上[科学高清](https://unsplash.com/@scienceinhd?utm_source=medium&utm_medium=referral)拍摄的照片

# 履行

## 连接

Animal.java

```
public interface Animal {
   void species();
}
```

## 实现接口

Dog.java

```
public class Dog implements Animal {
   @Override
   public void species() {
      System.out.println("It's a dog.");
   }
}
```

Cat.java

```
public class Cat implements Animal {
   @Override
   public void species() {
      System.out.println("It's a cat.");
   }
}
```

Bird.java

```
public class Bird implements Animal {
   @Override
   public void species() {
      System.out.println("It's a bird.");
   }
}
```

## 工厂

```
public class AnimalFactory {
   // use getAnimal method to get object of type animal 
   public Animal getAnimal(String animalSpecies){
      switch(animalSpecies.toLowerCase()) {
          case "dog":
              return new Dog();
          case "cat:
              return new Cat();
          case "bird":
              return new Bird();
          default:
              return null;
      }
   }
}
```

## 使用工厂

```
public class FactoryPattern {
   public static void main(String[] args) {
      AnimalFactory animalFactory = new AnimalFactory(); // Get Dog object through factory
      Animal dog = animalFactory.getAnimal("dog");

      // Call species method of Dog
      dog.species();

      // Get Cat object through factory
      Animal cat = animalFactory.getAnimal("cat");

      // Call species of Cat
      cat.species();

      // Get Bird object through factory
      Animal bird = animalFactory.getAnimal("bird");

      // Call species of Bird
      bird.species();
   }
}>> It's a dog.
>> It's a cat.
>> It's a bird.
```

# 结论

工厂设计模式包含如何创建对象的逻辑。它还可以用来包含创建新对象的所有逻辑。这使得查找代码比搜索整个应用程序更容易。这也使得向工厂添加新类变得更加容易。如果创建一个新对象的逻辑相当简单，你可能不需要工厂。