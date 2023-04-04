# 打字稿经典函数与箭头方法

> 原文：<https://blog.devgenius.io/typescript-classical-function-vs-arrow-method-6f08b04e891?source=collection_archive---------0----------------------->

何时使用不同的符号。

![](img/b7c174e1d1abce5157806f26325f6654.png)

华金·索罗拉，1910 年

经典函数和箭法挺像的，只是语法不一样。

从 ES2015 开始，有一个*箭头函数*语法可用:它有轻量级语法，使用匿名函数，没有显式返回。

```
const result = (person) => {
  return `Good afternoon, ${person}`;
}
RxJs method "find" which looks for records in array:const preSelectedTariff = this._tariffs?.find(tariff => tariff.tariffUniqueId === this.selectedTariff?.tariffUniqueId);
```

Lambda 是一个小型匿名函数。它可以接受任意数量的参数，但只能有一个表达式。但是匿名会产生一些问题:

1.  更难调试

当您得到一个错误时，您将无法跟踪函数的名称或者错误发生的确切行号。

2.块作用域——逻辑上适用于经典函数。函数可以以任何顺序声明，应用程序可以以任何方式工作。

```
callMyFunction();function callMyFunction() {
  console.log(`Rnd number ${getAnother(1000)}`);
}
function getAnother(max: number) {
  return Math.floor(Math.random() * (max + 1));
}
```

这是命令式编程和函数式编程。他们每个人都有自己的优势。但是由于块范围的原因，函数应该按照它们出现的顺序来声明。

```
tryToCallInWrongOrder();const tryToCallInWrongOrder = () => {
  console.log(`Trying to call the function ${getAnother(1001)}`);
}
function getAnother(max: number) {
  return Math.floor(Math.random() * (max + 1));
}
```

3.没有自我引用

如果你的函数需要一个自引用，那么它就不会工作。例如，它可以是递归或事件处理程序。

**当你不应该使用箭头方法时:**

1.  对象方法

当在对象内部使用 arrow 函数时，它从外部作用域继承`this`值，而不是从局部作用域继承，并且`count` 变量不可用。

```
const counter = {
  count: 0,
  getFollowing: () => this.count+1,
  getCurrent: () => this.count
};
console.log( counter.getFollowing() );
```

2.在 JavaScript 中:使用 arguments 对象的函数。

箭头函数没有`arguments`对象。因此，如果您有一个使用`arguments`对象的函数，则不能使用 arrow 函数。

```
const concat = (separator) => {
    let args = Array.prototype.slice.call(arguments, 1);
    return args.join(separator);
}
```

但是参数在常规函数中起作用:

```
function getChar() {
  return Array.prototype.slice.call(arguments,0,1);
}
console.log(getChar('a','b','c'));// or use separator like this:
function concat(separator) {
    let args = Array.prototype.slice.call(arguments, 2);
    return args.join(separator);
}
console.log( concat(',', 'A', 'B', 'C', 'D', 'E') );
```

3.当它使你的代码更难理解时

正则函数提供了 100%清晰的输入参数和输出类型语法。但是 arrow 方法对于哪种输入类型和输出是什么并不那么直接。这就是函数式编程方法。

![](img/82b70f0cae1b998829bb767e3ac9f8e8.png)

美丽拯救世界

**该方法的两种表示法之间的优势差异是什么？**

1.  **继承**

Arrow 方法不允许在子类中继承相同的方法，它不会被覆盖。

```
class Parent {
  method = () => { console.log("Parent method"); }
}
class Child extends Parent {
  method = () => {  // won't override Parent method super.method();
    // won't work because refers to Parent.prototype.method, 
    // which doesn't exist
  }
}
```

**2。新**

常规函数是可构造的，可以使用 new 关键字调用它们。

```
const kv = new ***Map***();

kv.set("Test msg", "Unknown");
kv.set("", "Unknown");
kv.set(null, "Unknown");
kv.forEach((value, key) => {
  ***expect***(new RequestTypePipe().transform(key)).toEqual(value);
});
```

但是，箭头函数永远不能用作构造函数。因此，不能用 new 关键字调用它们:

```
let add = (x: number, y: number) => console.log(x + y);
const error = new add(1, 1); //TypeError: 'add' is not a constructor
const result = add(1000, 1);
```

**新的**关键字提供了创建实例的清晰方法:

```
private readonly unsubscribe$: Subject<void> = new Subject();
private chargeSubscription: Subscription;ngOnDestroy(): void {
  this.unsubscribe$.next();
  this.unsubscribe$.complete();
}private readonly subscribeToQuotation = (): void => {
  this.chargeSubscription = this.myService()
    .pipe(takeUntil(this.unsubscribe$))
    .subscribe(charge => this.handleCharge(charge));
};
```

**3。参数绑定**

```
const mapToLocationCode = (geo: Geo): string => geo?.code || "";return mapToLocationCode(houseObject.geo);
```

但是，如果要访问箭头函数中的参数，可以使用 rest 运算符:

```
var arrowFunction = (...args) => {
    console.log(...args)
}
arrowFunction("my", "additional", "argument");
```

**4。隐式返回**

经典函数使用 **return** 关键字来返回值。如果它被遗漏，那么函数将返回未定义。

箭头函数减少了代码，提供隐式返回，使代码更干净。

```
let message = () => 1001;
const result = (message: () => void) => { console.log(message) };// pass message as a parameter
result(message); 
```

这来自科特林的世界:如果没有争论——用下划线:

```
let myArrowMethod = _ => console.log("Arrow Method");
```

**5。这个**

ES6 arrow 方法没有' this '，不能绑定到一个`this`关键字。我们所能做的就是引用外部作用域变量。

```
const thename = "Alex";
let nameObject = {
  name : "Another Alex",
  arrowMethod: () => {
    console.log('arrowFunc: ' + thename);  //access to outer variabl
  },
  classicFunction() {
    console.log('classicFunction: ' + this.name); 
  }   
}
nameObject.arrowMethod();
nameObject.classicFunction();
```

常规函数用它自己的定义创建一个对象和一个作用域，但用 arrow 方法不一样。

```
let currentValue: { value: string };// Classic Function
const saveFunction = function (data: any) {
  currentValue = { 'value': data };
  console.log(currentValue);
}
// Arrow Function
const arrowMethod = (data: any) => {
  currentValue = { 'value': data };
  console.log(currentValue);
}
saveFunction('method');
arrowMethod('arrow');
```

![](img/fc3ec149d01dfb0a990d4890bdd54ceb.png)

华金·索罗拉，“大浪如圣塞巴斯蒂安”，1918 年

**结论**

还有很多其他的区别，我描述其中的主要区别。很难说，什么时候选择这些方法之一。函数式编程需要更少的代码，经常使用箭头方法！