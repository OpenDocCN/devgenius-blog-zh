# C# 9 早期评审

> 原文：<https://blog.devgenius.io/c-9-early-review-5bcd88296c54?source=collection_archive---------2----------------------->

![](img/6de5f5e0575c907d739e64130e53dc21.png)

# 已经实施

## 1.模式匹配改进

**1.1 类型模式**

```
void M(object o1, object o2)
{
    var t = (o1, o2);
    if (t is (int, string)) {} // test if o1 is an int and o2 is a string
    switch (o1) {
        case int: break; // test if o1 is an int
        case System.String: break; // test if o1 is a string
    }
}
```

**1.2 圆括号图案**

带括号的模式允许程序员在任何模式周围加上括号。这对于 C# 8.0 中的现有模式来说不是很有用，但是新的模式组合子引入了一个程序员可能想要覆盖的优先级。

```
primary_pattern
    : parenthesized_pattern
    ;
parenthesized_pattern
    : '(' pattern ')'
    ;
```

1.3 关系模式

关系模式允许程序员表达输入值在与常数值比较时必须满足关系约束:

```
public static LifeStage LifeStageAtAge(int age) => age switch
    {
        < 0 =>  LiftStage.Prenatal,
        < 2 =>  LifeStage.Infant,
        < 4 =>  LifeStage.Toddler,
        < 6 =>  LifeStage.EarlyChild,
        < 12 => LifeStage.MiddleChild,
        < 20 => LifeStage.Adolescent,
        < 40 => LifeStage.EarlyAdult,
        < 65 => LifeStage.MiddleAdult,
        _ =>    LifeStage.LateAdult,
    };
```

**1.4 模式组合器**

三种新的模式形式

1.  *图案* `and` *图案*
2.  *图案*图案`or`图案*图案*
3.  `not`图案*图案*

```
//example 1
bool IsLetter(char c) => c is (>= 'a' and <= 'z') or (>= 'A' and <= 'Z');//example 2
switch (o)
{
    case 1 or 2:
    case Point(0, 0) or null:
    case Point(var x, var y) and var p:
}
```

## 2.目标类型的新

当类型已知时，不需要为构造函数指定类型。允许字段初始化而不复制类型。

```
Dictionary<string, List<int>> field = new() {
    { "item1", new() { 1, 2, 3 } }
};
```

当可以从用法中推断出类型时，允许省略该类型。

```
XmlReader.Create(reader, new() { IgnoreWhitespace = true });
```

实例化一个对象，但不拼写其类型。

```
private readonly static object s_syncObj = new();
```

## 3.λ丢弃参数

未使用的参数不需要命名。丢弃的目的很明确，即它们是未使用的/丢弃的。

允许丢弃(`_`)作为 lambdas 和匿名方法的参数。例如:

*   兰达斯:`(_, _) => 0`，`(int _, int _) => 0`
*   匿名方法:`delegate(int _, int _) { return 0; }`

## 4.局部函数的属性

现在属性可以成为局部函数声明的一部分。

```
class C
{
    void M()
    {
        int x;
        local1();
        Console.WriteLine(x);

        [Conditional("DEBUG")]       
        void local1()
        {
            x = 42;
        }
    }
}
```

## 5.本地 int

标识符`nint`和`nuint`是新的上下文关键字，表示本地有符号和无符号整数类型。当名称查找在该程序位置没有找到可行的结果时，标识符仅被视为关键字。

```
nint x = 3;
string y = nameof(nuint);
_ = nint.Equals(x, 3);
```

类型`nint`和`nuint`由底层类型`System.IntPtr`和`System.UIntPtr`表示，编译器将这些类型的额外转换和操作表现为本机 int。

## 6.扩展部分

语言将会改变，允许用一个显式的可访问性修饰符来注释`partial`方法。这意味着它们可以被标记为`private`、`public`等...

当一个`partial`方法有一个显式的可访问性修饰符时，尽管语言会要求声明有一个匹配的定义，即使可访问性是`private`:

```
partial class C
{
    // Okay because no definition is required here
    partial void M1();// Okay because M2 has a definition
    private partial void M2();// Error: partial method M3 must have a definition
    private partial void M3();
}partial class C
{
    private partial void M2() { }
}
```

此外，这种语言将取消对出现在具有显式可访问性的`partial`方法上的内容的所有限制。这样的声明可以包含非 void 返回类型、`ref`或`out`参数、`extern`修饰符等...这些签名将具有 C#语言的全部表达能力。

```
partial class D
{
    // Okay
    internal partial bool TryParse(string s, out int i); 
}partial class D
{
    internal partial bool TryParse(string s, out int i) { }
}
```

这明确地允许`partial`方法参与`overrides`和`interface`实现:

```
interface IStudent
{
    string GetName();
}partial class C : IStudent
{
    public virtual partial string GetName(); 
}partial class C
{
    public virtual partial string GetName() => "Jarde";
}
```

## 7.函数指针

这个特性提供了一些语言结构，这些结构暴露了当前不能被有效访问或者根本不能被访问的低级 IL 操作码:`ldftn`、`ldvirtftn`、`ldtoken`和`calli`。这些低级操作码在高性能代码中非常重要，开发人员需要一种有效的方法来访问它们。

## 8.跳过本地初始化

允许通过`SkipLocalsInitAttribute`属性抑制`localsinit`标志的发出。

# 在发展中

## 1.记录

如果您想使单个属性不可变，那么只有 Init 属性是很好的选择。如果您希望整个对象是不可变的，并且表现得像一个值，那么您应该考虑将它声明为一个*记录*:

```
public data class Person
{
    public string FirstName { get; init; }
    public string LastName { get; init; }
}
```

类声明上的`data`关键字将其标记为记录。这给它注入了几个额外的类似价值的行为，我们将在下面深入探讨。一般来说，记录更应该被视为“价值”——数据！–而不是作为物品。它们不应该有可变的封装状态。相反，您通过创建表示新状态的新记录来表示随时间的变化。它们不是由它们的身份定义的，而是由它们的内容定义的。

为了帮助这种风格的编程，记录允许一种新的表达方式；`with`-表情:

```
var otherPerson = person with { LastName = "Hanselman" };
```

简短声明:

```
public data class Person { string FirstName; string LastName; }//Means exactly the same as the one we had before:public data class Person
{
    public string FirstName { get; init; }
    public string LastName { get; init; }
}
```

继承:

```
public data class Person { string FirstName; string LastName; }
public data class Student : Person { int ID; }
```

## 2.参数零检查

这允许使用参数上的小注释来简化参数的标准`null`验证:

```
// Before
void Insert(string s) {
  if (s is null)
    throw new ArgumentNullException(nameof(s)); ...
}// After
void Insert(string s!) {
  ...
}
```

## 3.协变收益

支持*协变返回类型*。具体来说，允许方法的重写返回比它重写的方法更派生的返回类型，同样，允许只读属性的重写返回更派生的返回类型。方法或属性的调用方将从调用中静态接收更精确的返回类型，并且出现在更多派生类型中的重写将被要求提供至少与其基类型中的重写一样具体的返回类型。
举例:

```
class Compilation ...
{
    virtual Compilation WithOptions(Options options)...
}class CSharpCompilation : Compilation
{
    override CSharpCompilation WithOptions(Options options)...
}
```

## 4.静态 lambdas

为了避免在为方法参数提供 lambda 函数时意外捕获任何局部状态，请在 lambda 声明前面加上 static 关键字。这使得 lambda 函数就像一个静态方法，没有捕获局部变量，也没有对这个或 base 的访问。

```
int y = 10;
someMethod(x => x + y); // captures 'y', causing unintended allocation.
```

有了这个建议，你可以使用 static 关键字来避免这个错误。

```
int y = 10;
someMethod(static x => x + y); // error!const int y = 10;
someMethod(static x => x + y); // okay :-)
```

## 5.顶级语句

允许一系列的*语句*出现在*编译单元*(即源文件)的*名称空间 _ 成员 _ 声明*之前。

示例:

```
// Example #1
await System.Threading.Tasks.Task.Delay(1000);
System.Console.WriteLine("Hi!");// would be converted tostatic class $Program
{
    static async Task $Main(string[] args)
    {
        await System.Threading.Tasks.Task.Delay(1000);
        System.Console.WriteLine("Hi!");
    }
}--------------------------------------------------------------------// Example #2
await System.Threading.Tasks.Task.Delay(1000);
System.Console.WriteLine("Hi!");
return 0;// would be converted tostatic class $Program
{
    static async Task<int> $Main(string[] args)
    {
        await System.Threading.Tasks.Task.Delay(1000);
        System.Console.WriteLine("Hi!");
        return 0;
    }
}--------------------------------------------------------------------// Example #3
System.Console.WriteLine("Hi!");
return 2;// would be converted tostatic class $Program
{
    static int $Main(string[] args)
    {
        System.Console.WriteLine("Hi!");
        return 2;
    }
}
```

## 6.目标类型条件

对于条件表达式`c ? e1 : e2`，当

1.  `e1`和`e2`没有通用类型，或者
2.  存在一个通用类型，但表达式`e1`或`e2`中的一个没有隐式转换为该类型

定义了一个新的隐式*条件表达式转换**，其允许从条件表达式到任何类型`T`的隐式转换，其中存在从`e1`到`T`以及从`e2`到`T`的转换。如果条件表达式既没有`e1`和`e2`之间的通用类型，也不受*条件表达式转换*的约束，则为错误。*

## *7.扩展 GetEnumerator*

*允许`foreach`循环识别满足`foreach`模式的扩展方法`GetEnumerator`方法，并在表达式出错时循环该表达式。*

## *8.放松`ref`和`partial`修改器的顺序*

*目前，`partial`必须直接出现在`struct`、`class`或其他类型声明关键字之前。如果类型是一个`ref`结构，`ref`必须紧接在`partial`或`struct`之前出现。似乎可以使用各种其他关键字来消除这些上下文修饰符的歧义，并允许我们放松对`partial`和`ref`在修饰符列表中出现位置的限制。*

## *9.模块初始化器*

*   *使库能够在加载时立即进行一次性初始化，开销最小，用户无需显式调用任何东西*
*   *当前`static`构造函数方法的一个特别的痛点是，运行时必须对使用静态构造函数的类型进行额外的检查，以决定是否需要运行静态构造函数。这增加了可观的开销。*
*   *使源代码生成器能够运行一些全局初始化逻辑，而无需用户显式调用任何东西*

*附:在这里你可以阅读更多关于所有新功能的信息[https://github . com/dot net/roslyn/blob/master/docs/Language % 20 feature % 20 status . MD](https://github.com/dotnet/roslyn/blob/master/docs/Language%20Feature%20Status.md)*