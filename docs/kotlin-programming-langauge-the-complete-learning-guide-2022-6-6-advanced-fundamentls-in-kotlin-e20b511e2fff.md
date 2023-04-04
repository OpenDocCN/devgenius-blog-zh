# 科特林编程语言完全学习指南[2022] 6/6 科特林高级基础

> 原文：<https://blog.devgenius.io/kotlin-programming-langauge-the-complete-learning-guide-2022-6-6-advanced-fundamentls-in-kotlin-e20b511e2fff?source=collection_archive---------11----------------------->

![](img/2b6224e98c323e1a7a65216cd1b0dfd0.png)

学习 Kotlin 这种最强大和最有用的编程语言之一，并准备好开始开发 powerfull 原生移动应用程序和其他平台开发

# 目录

> *数组*
> 
> *列表*
> 
> *设定&地图*
> 
> *数组列表*
> 
> *λ表达式*
> 
> *访问修饰符*
> 
> *内部&嵌套类*
> 
> *异常处理*

# 数组

就像我们之前看到的一样。它是保存数据集合的数据类型，所以让我们创建一个数组

```
fun main() {
    val nums : IntArray = intArrayOf (1,2,3,4,5,6,7,8,9,10)
}
```

我们知道是科特林，我们可以让这个声明更简单

```
fun main() {
    val nums  = intArrayOf (1,2,3,4,5,6,7,8,9,10)
}
```

现在，编译器知道它是一个整数数组，否则我们也可以像下面这样做

```
fun main() {
    val nums  = arrayOf (1,2,3,4,5,6,7,8,9,10)
}
```

所有的都是一样的，但是我们让它变得更简单

> 注意:数组数据类型是数组第一个元素的引用，引用是变量在内存中的位置，所以数组保存它的元素的第一个元素的引用，让我们看看

```
fun main() {
    val nums  = arrayOf (1,2,3,4,5,6,7,8,9,10)
    println(nums)
}//output 
//[Ljava.lang.Integer;@8efb846
```

产量是多少？这就是我们所说的，它打印的是数组第一个元素的位置，而不是。值，还记得我们在 oop 部分讨论类时，让我们创建一个类，并尝试打印它来解释引用

```
fun main() {
    val ref :ExplainRefrence = ExplainRefrence("Mohab")
    print(ref)
}

class ExplainRefrence (name : String ){
    val name : String
    init {
        this.name=name
    }
}//output ExplainRefrence@7adf9f5f
```

它没有打印人类可读的东西，对吧，这个引用“这个对象在内存中的存储位置”对数组来说也是一样，如果我们想打印一个数组，我们不这样做

因为它是 kotlin，所以它在数组中有一个名为***content tostring*()**的方法，该方法将打印数组的内容

```
fun main() {
    val nums  = arrayOf (1,2,3,4,5,6,7,8,9,10)
    println(nums.contentToString())

}// [1, 2, 3, 4, 5, 6, 7, 8, 9, 10]
```

现在让我们尝试使用我们之前看到的 for 循环来逐项打印

```
fun main() {
    val nums  = arrayOf (1,2,3,4,5,6,7,8,9,10)
for (i in nums ){
    println(i)
}
}//1
//2
//3
//4
//5
//6
//7
//8
//9
//10
```

> **注意**:数组中第一个元素从 **0** 开始，所以 **(1)** 索引是 **0** 而不是 **(1)** 计算机从 0 开始计数，请看下面的例子

```
fun main() {
val nums  = arrayOf (1,2,3,4,5,6,7,8,9,10)
println(nums[0])
}//output 
//1
```

我们也可以重新分配这些值，如下所示

```
fun main() {
    val nums  = arrayOf (1,2,3,4,5,6,7,8,9,10)
    println(nums[5])
    nums [5]= 100
    println(nums[5])
}
```

一定要确保传递[]中的现有索引，不要像下面这样做

```
fun main() {
    val  nums  = arrayOf (1,2,3,4,5,6,7,8,9,10)

    println(nums[12])
}
```

**线程“main”Java . lang . arrayindexoutofboundsexception 中出现异常:MainKt.main(Main.kt:4)处长度为 10 的索引 12 超出界限
mainkt . main(main . kt)处的** 

事实是，这个数组只有 10 个元素，那么我们怎么能打印出一个元素存在于第 12 个元素中，而这个元素是不存在的呢

让我们看一些在数组内部存储的例子

```
fun main() {
    val  nums  = arrayOf (1,2,3,4,5,6,7,8,9,false )
val strgns = arrayOf("Mohab","Messi","Neymar")
val bools = arrayOf(false , true )
var mixed = arrayOf(false , 123.0,11123,"Mohab" )
}
```

我们也可以创建自己的类型，并将其存储在数组中

```
fun main() {
    val type1 =MyOwntype("Mohab")
    val type2 =MyOwntype(false )
    val type3 =MyOwntype(122 )
val myArray = arrayOf(type1,type2,type3)

    print(myArray[0].toString())
}

data class MyOwntype (val anyType : Any )

//output MyOwntype(anyType=Mohab)
```

# 列表

也存储一种类型或多种类型

```
fun main() {
    val players = listOf("Messi","Neymar","Ronaldo")
    print(players[0])
}//Messi
```

我们也可以在上面循环

```
fun main() {
    val players = listOf("Messi","Neymar","Ronaldo")
    for (player in players){
        println(player)
    }
}//
//Messi
//Neymar
//Ronaldo
```

```
fun main() {
    val players = listOf("Messi","Neymar","Ronaldo")
players.forEach( ){
    println(it)
}
}//Messi
//Neymar
//Ronaldo
```

**它** →引用列表里面的元素它喜欢的**玩家【n】**

```
fun main() {
    val players = listOf("Messi","Neymar","Ronaldo")
println(players.size)
}//3
```

现在这个列表是不可变的，我们不能向它添加元素

但是我们可以把它转换成不可变的

```
fun main() {
    val players = listOf("Messi","Neymar","Ronaldo")
 val mutablePlayers =    players.toMutableList()

    mutablePlayers.add("Mohab")
    println(mutablePlayers[3])
}//Mohab
```

我们可以创建一个类似下面的列表

```
fun main() {
    val players = mutableListOf<String >("Messi","Neymar","Ronaldo")
    players.add("hakimi")

}
```

这是泛型，它告诉编译器，这是一个只有字符串的可变列表，是可选的

```
fun main() {
    val players = mutableListOf("Messi","Neymar","Ronaldo")
    players.add("hakimi")
}
```

现在编译器也知道这是一个字符串的可变列表，泛型在下一个例子中很有用

```
fun main() {
    val players = mutableListOf<Any>("Messi","Neymar","Ronaldo")
    players.add(111.213)
}
```

我们把它做成一个<any>的列表，这样我们就可以存储任何类型，让我们看看如何让列表变空或者移除元素</any>

```
fun main() {
    val players = mutableListOf<Any>("Messi","Neymar","Ronaldo")
println(players)
    players.remove("Messi")
    println(players)
players.removeAt(1)
println(players)
players.removeAll(players)
 println(players)
}// output 
//[Messi, Neymar, Ronaldo]
//[Neymar, Ronaldo]
//[Neymar]
//[]
```

# 设置

移除重复元素的集合类型，不是排序集合，也有可变和不可变类， **mutableSetOf() setOf()**

```
fun main() {
val players = setOf ("Messi","Messi","Messi","Ronaldo")
    println(players.size)
}//output 
//2
```

是的，我们存储了 4 个元素，但实际上它们只有 2 个是唯一的，这就是 set 如何移除重复的元素，**“就像在学校学数学”**

让我们试着分类

```
fun main() {
val players = setOf ("Ronaldo","Messi","Messi","Messi","Abou Trika")
    println(players.sorted())
}//[Abou Trika, Messi, Ronaldo]
```

它按字母顺序排序，让它可变

```
fun main() {
val players = setOf ("Ronaldo","Messi","Messi","Messi","Abou Trika")
    println(players.sorted())
    val mutablePlayers = mutableSetOf("Ronaldo","Messi","Messi","Messi","Abou Trika")
    mutablePlayers.add("Ronaldinho")
    mutablePlayers.add("Neymar")
    println(mutablePlayers.sorted())

}//output 
//[Abou Trika, Messi, Ronaldo]
//[Abou Trika, Messi, Neymar, Ronaldinho, Ronaldo]
```

# 地图

也是一种集合类型，以键和值对的形式存储数据，就像每个值都有它的键，以便在内存中存储的映射中找到它

每个键都是唯一的，不能重复，每个键只能保存一个值

```
fun main() {
val playersMap = mapOf(11 to "Neymar",10 to "Messi",22 to "Abou trika")

}
```

这就是我们如何声明一个地图，我们说内马尔(值)有一个 11(键)，但如果我们用相同的键存储两个值会发生什么，如下所示？

```
fun main() {
val playersMap = mapOf(11 to "Neymar",10 to "Messi",22 to "Abou trika",11 to "Ronaldo")
    print(playersMap[11])
}// Ronaldo
```

具有重复键的第一个值将被覆盖以存储新值

> 下面是如何使用键访问地图中的值

```
print(playersMap[11]) 
```

我们可以像下面一样访问所有的键或值，

```
fun main() {
val playersMap = mapOf(11 to "Neymar",10 to "Messi",22 to "Abou trika",11 to "Ronaldo")

    for (key in playersMap.keys){
        println(key)
    }
}// output 
//11
//10
//22
```

```
fun main() {
val playersMap = mapOf(11 to "Neymar",10 to "Messi",22 to "Abou trika",11 to "Ronaldo")

    for (player in playersMap.values){
        println(player)
    }
}//
//Ronaldo
//Messi
//Abou trika
```

我们也可以访问这两者

```
fun main() {
val playersMap = mapOf(11 to "Neymar",10 to "Messi",22 to "Abou trika",11 to "Ronaldo")

    for (key in playersMap.keys){
        println("in your map $key holds ${playersMap[key]}")
    }
}//output 
//in your map 11 holds Ronaldo
//in your map 10 holds Messi
//in your map 22 holds Abou trika
```

键可以是任何类型的数据，不仅是整型，

```
fun main() {
val playersMap = mapOf("11" to "Neymar","10" to "Messi","22" to "Abou trika","11" to "Ronaldo")

    for (key in playersMap.keys){
        println("in your map $key holds ${playersMap[key]}")
    }
}//
//in your map 11 holds Ronaldo
//in your map 10 holds Messi
//in your map 22 holds Abou trika
```

看，我们也把它改成了字符串，让它变得可变

```
fun main() {

    val mutablePLayersMap= mutableMapOf("11" to "Neymar","10" to "Messi","22" to "Abou trika","11" to "Ronaldo")
    mutablePLayersMap.putIfAbsent("1","Cassialls")
    print(mutablePLayersMap)
}// 
//{11=Ronaldo, 10=Messi, 22=Abou trika, 1=Cassialls}
```

putIfAbsent()如果传递给它的键不存在，它将把它放在映射的内部

```
fun main() {

    val mutablePLayersMap= mutableMapOf("11" to "Neymar","10" to "Messi","22" to "Abou trika","11" to "Ronaldo")
    mutablePLayersMap.putIfAbsent("1","Cassialls")
    println(mutablePLayersMap)
    mutablePLayersMap.putIfAbsent("1","Neuer")
    println(mutablePLayersMap)
}//
//{11=Ronaldo, 10=Messi, 22=Abou trika, 1=Cassialls}
//{11=Ronaldo, 10=Messi, 22=Abou trika, 1=Cassialls}
```

第一次把它放进去，因为键不存在，但是在它没有放进去之后，让我们看看 put()

```
fun main() {

    val mutablePLayersMap= mutableMapOf("11" to "Neymar","10" to "Messi","22" to "Abou trika","11" to "Ronaldo")
    mutablePLayersMap.putIfAbsent("1","Cassialls")
    println(mutablePLayersMap)
    mutablePLayersMap.putIfAbsent("1","Neuer")
    println(mutablePLayersMap)
    mutablePLayersMap.put("1","Neuer")
    println(mutablePLayersMap)

}//output 
//{11=Ronaldo, 10=Messi, 22=Abou trika, 1=Cassialls}
//{11=Ronaldo, 10=Messi, 22=Abou trika, 1=Cassialls}
//{11=Ronaldo, 10=Messi, 22=Abou trika, 1=Neuer}
```

put 会将它添加到映射中，即使它覆盖了具有相同键的相同值

# 数组列表

动态分配，意味着它的大小可以增加或减少，我们也可以读和写，也可以添加元素，也可以有重复的元素

```
val myList = ArrayList<Any>() // empty arrayList 
```

```
val myList = ArrayList<Any>(100) // array with limted size of 100 elements only 
```

```
myList.add()//add an element to arrayList
```

```
myList.clear()// clears the arrayList
```

```
myList.get(0) // get the eleemnt of [n] index
```

```
myList.remove("mohab")// removes eleemtns from arrayList
```

# λ表达式

允许我们写简短的代码，没有 nam 的函数(不是 anonymus)这个函数没有 delcared，它作为一个表达式传递，函数体写在->(那个箭头)叫做 lambda 之后

让我们看看返回 int 类型值的普通函数

```
fun main() {
println(add(1,2))

}
fun add (x : Int , y : Int ):Int {
    return  x+y ;
}
//output 
//3
```

λ函数

```
fun main() {
println(lambdaAdd(1,2))

}
val lambdaAdd: (Int , Int )->Int = {a:Int , b : Int ->a+b}
```

我们也可以像下面这样重新输入

```
fun main() {
println(shorterLambdaAdd(1,2))
}
val shorterLambdaAdd = {x : Int , y : Int ->x+y }
```

甚至更短，

```
fun main() {
println(shorterLambdaAdd)
}
val shorterLambdaAdd = {x : Int , y : Int -> println(x+y ) }
```

# 访问修饰符(可见性修饰符)

它们用于限制类、类间成员的使用，可用于方法和类

公共、私有、内部、受保护

> 可以从项目内部的任何地方访问，在 kotlin 中，一切都是公开的，除非我们限制
> 
> **private** →只能从声明成员的块中访问，不能在我们所在的作用域之外访问
> 
> **内部** →“不在 java 中”使成员只在实现它的文件或模块内部可见

还记得我们在讨论继承时说过，在 kotlin 中所有的类都是 final 类，所以没有类可以继承，除非我们把它开放

> 如果这个类是开放的，那么它可以被继承，protected 使超类只对它的孩子可见

# 嵌套类和内部类

在另一个类中创建的类，不管有没有关键字 inner，我知道在它们自己的文件中创建独立的类总是更好，但是如果只是想让一个类使用另一个类，那么创建一个嵌套在外部类中的类会更好

**嵌套类**

它是该类的一个**静态**，因此外部类成员可以访问它，而无需从

内部类不能访问外部类成员

```
class OuterClass {
    private var name : String ="Mohab"

    class InnerClass (){
var jobTitle : String = "Programmer"
        private var age : Int  = 24
        fun printData (){
            println(" iam $name and i work as a  $jobTitle ")
        }
    }
}// error Unresolved reference: name
```

该错误是因为内部不能访问外部类成员

```
fun main() {
println(OuterClass.InnerClass().jobTitle)
    OuterClass.InnerClass().printData()
}

class OuterClass {
    private var name : String ="Mohab"

    class InnerClass (){
var jobTitle : String = "Programmer"
        private var age : Int  = 24
        fun printData (){
            println("  i work as a  $jobTitle ")
        }
    }
}
```

我们也可以从内部类创建一个对象

```
fun main() {
println(OuterClass.InnerClass().jobTitle)
    OuterClass.InnerClass().printData()
    var innerObject = OuterClass.InnerClass()

}
```

“内部”键不能创建在接口或非内部类中，但它比嵌套的“它可以访问外部成员”有更大的威力

```
class OuterClass {
    private var name : String ="Mohab"

 inner    class InnerClass (){
var jobTitle : String = "Programmer"
        private var age : Int  = 24
        fun printData (){
            println(" hello iam $name i work as a  $jobTitle ")
        }
    }
}// now it works 
```

但是现在它不是静态的，所以我们需要从外部获取一个对象来访问内部

```
fun main() {
println(OuterClass().InnerClass().jobTitle)
    OuterClass().InnerClass().printData()
    var innerObject = OuterClass().InnerClass()

}
```

# 异常处理

处理程序运行时可能出现的运行时问题

> **异常** →运行时问题导致程序终止，例如一个数字被减 0 或内存耗尽或没有网络…等等
> 
> 现在 exeption handeling 允许我们处理这个错误，这样计算机或电话就可以继续运行程序而不用终止它
> 
> **Throwable (throw)** →一个使用关键字(throw)抛出异常(无论是来自 kotlin 还是我们自己类型的异常)的类，也允许我们知道错误发生在哪里
> 
> **try{}** →这是一段代码{}，我们在其中添加了可能导致错误的代码，以便编译器尝试在其中执行任何操作
> 
> **catch(e:Exception ){}** →总是出现在 try 块之后，其中的代码只有在 try {}抛出错误时才执行，所以我们告诉计算机，捕捉这个错误，并执行代码来处理它
> 
> **最后** - >它告诉我们编译器是否处理了异常

让我们看看异常的例子

> **算术异常→** 当数字减少 0 时
> 
> **ArrayIndexOutOfBoundExceptions→**当我们调用数组中不存在的索引时
> 
> **NullPointerException(NPE)**→当我们使用空值时

```
try {}catch(e:ArithmeticException){}
```

让我们看一个例子

```
fun main() {
    val zero : Int = 0
    val no : Int = 10
    println(no/zero)
    println("I Was Called After Dividng is done ")
}
```

**线程“main”中出现异常 Java . lang . arithmetic Exception:/by zero
at mainkt . main(main . kt:5)
at mainkt . main(main . kt)**

看到它甚至没有完成线后，没有打印，但让我们尝试处理它

```
fun main() {
    val zero : Int = 0
    val no : Int = 10
try {    println(no/zero)
}catch (e:ArithmeticException){
    println("An Error Occured but it was handled you tried to divide a number by zero")
}
    println("I Was Called After Dividng is done ")
}//output 
//An Error Occured but it was handled you tried to divide a number by zero
//I Was Called After Dividng is done
```

如果我们试图做很多可能导致错误的事情，我们也可以有 n 个异常处理

```
fun main() {
    val zero : Int = 0
    val no : Int = 10
    val myArray = arrayOf("Mohab")
try {
    println(myArray[1])
    println(no/zero)
}catch (e : ArrayIndexOutOfBoundsException){
    println("An Error Occured but it was handled you tried to call an  index is not exist")

}

catch (e:ArithmeticException){
    println("An Error Occured but it was handled you tried to divide a number by zero")
}
    println("I Was Called After Dividng is done ")
}//output 
An Error Occured but it was handled you tried to call an  index is not exist
I Was Called After Dividng is done 
```

看到它停在第一个 catch 块**“ArrayIndexOutOfBoundsException”**

让我们来看一个例子，在 finally 中，什么将被错误地执行，或者没有错误

```
fun main() {
    try {
        println(10 / 5)
    } catch (e: ArithmeticException) {
        println("An Error Occured but it was handled you tried to call an  index is not exist")
    }finally {
        println(" I Will Work anyway ")
    }

}
```

让我们看看投掷

```
fun main() {
var myAge : Int = 0
countAge(myAge)
    println("I will be printed  whenever  your func is done ")
}
fun countAge (age : Int ){
    if (age<=0){
        throw CustomException("No one can have negative or zero years")
    }
}
class CustomException(message: String?) : Exception(message) {
}
```

线程“main”中的异常客户异常:在 MainKt.countAge(Main.kt:10)的
在 MainKt.main(Main.kt:5)的
在 MainKt.main(Main.kt)的
中，任何人都不能有负数或零年

**这是 kotlin 编程语言完整指南的最后一部分，等待下一个指南**

**如果这篇文章真的对你有帮助，请为我鼓掌**

感谢您的阅读，等待您的评论和回复…