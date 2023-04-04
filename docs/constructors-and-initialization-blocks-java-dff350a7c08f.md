# 构造函数和初始化块。Java 语言(一种计算机语言，尤用于创建网站)

> 原文：<https://blog.devgenius.io/constructors-and-initialization-blocks-java-dff350a7c08f?source=collection_archive---------13----------------------->

![](img/d6c56ab88d14db3ebc18bfc8052084f7.png)

构造函数是负责初始化对象的命名代码块。它带有一个类的名称，是一个特殊的方法。它可以有参数，因此在创建对象时，这些参数必须在括号中指定。一个简单构造函数的例子。

```
Trees () 
{
Number = ++nubmers; 
}
```

使用此构造函数，可以移除 setNumbers 方法。可以有几个构造函数，但是它们的类型和参数数量必须不同。下面是一个简单的例子，一个类有几个构造函数(见清单 1.0)。)

**清单 1.0。**
使用多个构造函数的类的例子

```
public class Toys {
    String size;
    String color;
    static int numbers = 0;
    int number;

    // First constructor
    Toys() {
         number = ++numbers;
    }

    // Second constructor
    Toys(String s) {
        number = ++numbers;
        size = s;
    }

    // Third constructor
    Toys(String s1, String s2) {
        number = ++numbers;
        size = s1;
        color = s2;
    }

    public static void main(String[] args) {
        Toys ball = new Toys(); // Usage of the first constructor
        ball.size = "small";
        ball.color = "red";

        Toys car = new Toys("big"); // Usage of the second constructor
        car.color = "green";

        Toys horse = new Toys("little", "brown"); // Usage of the third consttructor
    }

}
```

您可以将所谓的初始化块放在类声明中。创建对象时将执行初始化块。它被放在花括号中，例如带有初始化块的类。

```
public class MyString {
    static int numbers = 0;
    String NameString;
    { // Initialization block
        NameString = "String " + String.valueOf(++numbers);
    }
}
```

还有静态初始化块，它与初始化块的不同之处在于左花括号前的 static 修饰符(静态初始化只涉及调用该类的静态元素)。

参考这个

有时有必要在方法体中使用对调用该方法的对象的引用。这个有专门参考。清单 1.1 展示了一个使用这个引用的例子。

**清单 1.1。**
引用了这个例子

```
import java.awt.print.Book;

public class Books {
    String author;
    String nameBook;
    int number;
    static int numbers = 0;

    Books() {
        number = ++numbers;
    }

    Books(String author, String nameBook) {
        this(); // Calling the first constructor
        this.author = author;
        /* The value of the author property of the object that called this method becomes the value of the author variable,
        * which is a parameter of this constructor and receives a value when you call*/
        this.nameBook = nameBook;
    }

    public static void main(String[] args) {
        Books myLikeBook = new Books();
        myLikeBook.author = "Gerbert Schildt";
        myLikeBook.nameBook = "Java. The Complete Reference";
        Books myBook = new Books("Bruce Eckel", "Thinking in Java");
    }
}
```