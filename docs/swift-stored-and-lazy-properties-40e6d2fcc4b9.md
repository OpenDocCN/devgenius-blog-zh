# Swift |存储和惰性属性

> 原文：<https://blog.devgenius.io/swift-stored-and-lazy-properties-40e6d2fcc4b9?source=collection_archive---------18----------------------->

## 属性第 1 部分

![](img/d538f74bba598c109523e9b80d4fb3d6.png)

[C M](https://unsplash.com/@ubahnverleih?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 在 [Unsplash](https://unsplash.com/s/photos/collect?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 上拍照

存储属性是存储与类或结构的实例关联的值的基本属性。

*   var →变量存储属性
*   let →常量存储属性

# 可以指定默认值和初始值。

## 结构

> 即使存储的属性不是可选的，也自动创建初始值设定项

```
struct Person{
    var age: Int = 13 // stored property
    let name: String // stored property
}let person1 : Person = Person(age:21, name: "Elizabeth") 
```

## 班级

> 如果不是可选的，你必须指定一个默认值或者通过初始化器初始化它

```
class Person{
    var age: Int  = 13 // variable stored property 
    let name: String //constant stored property 

    init(age:Int, name:String){
         self.age = age 
         self.name = name 
    }}let person2 : Person = Person(age:21, name: "Elizabeth")
```

上面的结构自动提供了一个默认的初始化器，但是类需要指定一个用户定义的初始化器或者设置一个初始值。如果您指定了类的存储属性的初始值，就没有必要实现上面的自定义初始化器。

# 更复杂的例子

```
struct Mammal{
    var name:String  = "John"
    var age: Int  = 15
}class Person{
    var mammal:Mammal = Mammal()
    var occupation:String = "Student"
}let person1:Person = Person()
person1.occupation = "Barista"
person1.mammal = Mammal(name:"Joshua", age: 21)print("name:\(person1.mammal.name), age:\(person1.mammal.age), occupation: \(person1.occupation)")
```

输出如下所示

```
name:Joshua, age:21, occupation: Barista
```

# 惰性存储属性

如果在创建实例时没有属性值，可以将存储的属性声明为可选的。延迟存储属性是在需要时赋值的属性，**只能在调用后初始化该值。**

请注意，它只能用在变量之后。常量与在需要时赋值的惰性存储属性不兼容，因为它们必须在实例完全创建之前进行初始化。

使用惰性属性有助于清理不必要的性能或空间浪费。

懒惰的财产规则

1.  惰性属性必须与变量一起使用。默认情况下，声明为 lazy 的变量不能用 let 声明，因为一个值最初并不存在，后来才被创建。
2.  默认情况下，lazy 只能用于结构和类
3.  不能与计算属性一起使用

使用可选

```
struct Mammal{
    var name:String
    var age: Int
}class Person{
    var mammal:Mammal?
    var occupation:String = "Student"
}let person1:Person = Person()
person1.occupation = "Barista"//There is no mammal property when creating instance 
print("occupation: \(person1.occupation)")//Mammal property created
person1.mammal = Mammal(name: "Joshua", age: 21)print("name:\(person1.mammal!.name), age:\(person1.mammal!.age)")
```

使用惰性属性

```
struct Mammal{
    var name:String
    var age: Int
}class Person{
    lazy var mammal = Mammal(name:"new Name",age:100)
    var occupation:String = "Student"
}let person1:Person = Person()
person1.occupation = "Barista"//There is no mammal property when creating instance 
print("occupation: \(person1.occupation)")//Lazy property mammal is now initialized. 
print(person1.mammal.name)
print(person1.mammal.age)
```

这就是懒惰和存储属性的全部内容。有 3 个关键词你需要从这个博客中带走

*   var →变量存储属性
*   let →常量存储属性
*   惰性→惰性存储属性

感谢您的阅读！如果有任何问题，请告诉我！