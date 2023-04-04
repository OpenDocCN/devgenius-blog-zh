# 打字稿中的装饰者

> 原文：<https://blog.devgenius.io/decorators-in-typescript-8cdabbf4bd9d?source=collection_archive---------4----------------------->

![](img/ae3860fdfc57163553fe76bc6ca5e7d5.png)

在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上由 [Spacejoy](https://unsplash.com/@spacejoy?utm_source=medium&utm_medium=referral) 拍摄的照片

装饰器类似于 JAVA 中的注释或 C#中的属性。它们为类、属性或函数提供定义。装饰器是 ECMAScript 7 的一部分。装饰器有助于在现有定义中注入额外的功能，而无需重写或复制现有代码。

要使用 decorator，我们需要将 tsconfig.json. 中的 ***【实验解码器】设为 true，否则，在编译 typescript TSC<filename . ts>时，我们将被提示以下编译时错误***

> 错误 TS1219:对 decorators 的实验性支持是一项在未来版本中可能会发生变化的功能。请在“tsconfig”或“jsconfig”中设置“experimentalDecorators”选项以删除此警告。

我在使用 Nest js 开发微服务时遇到了这一点，理解它们变得非常重要，因为它在 Nest js 生态系统中被广泛使用。例如 Nest js 中的@UseGuards 就是一个方法修饰器的例子。

Decorators 可以被视为带有输入参数的简单函数，在运行时提供类、属性或函数的定义。注意@ <decorator>的用法。</decorator>

装饰器是在定义阶段调用的，而不是在实例创建阶段。为了更好地理解，让我们看看这个例子

```
function simpleDecorator(target:Function)
{
   console.log("I am from simple decorator");
}

@simpleDecorator
export class ClassType
{
  constructor()
  {
    console.log("I am from ctor");
  }
}
let instance = new ClassType();
```

```
output: 
I am from simple decorator
I am from ctor 
```

“我来自 simple decorator”在“我来自 ctor”之前打印在控制台中。我们可以看到，decorator 是在实例创建之前调用的。事实上，如果我们创建了多个 classType 实例，我们将会看到 decorators 在定义 classType 时只被调用一次。

好的，但是为什么我们把 decorator 作为一个函数，而不是上面例子中的另一个类型呢？这是因为 TS 中的类基本上是 Javascript 中的一个函数。如果我们没有将类型作为函数，而是将它作为字符串类型，那么我们将得到一个错误“类型' type ClassType '的参数不可赋给类型' string '的参数。”

我们可以将多个 decorators 应用到一个类、属性或函数中吗？是的。

如果我们必须将参数传递给装饰者呢？为此，我们必须使用装饰工厂。装饰工厂返回一个装饰函数

```
//Decorator factory return a decoratory function 
function decoratorFactory(paramters: any[])
{
  return function(ctor:Function)
  {
  console.log(`Data passed onto decorator:${paramters}`);
  }
}

@decoratorFactory(["param 1", "param 2", "param n"])
export class TargetClass{
}
```

***装修工类型***

1.  班级装修工。
2.  方法装饰者。
3.  房产装修工。
4.  参数装饰器。

***类装饰者***

到目前为止，我们看到的所有上面的例子都是关于类装饰者的。使用类级装饰器，我们可以通过原型注入属性

```
function Component(targetClass:Function)
{
  //Injecting porperty via decorator.
  targetClass.prototype.FrmDecoraator = "prototype property set by decorator";
  console.log(targetClass.name);

  //Accessing the name property of the target class Function.
  console.log((<any>targetClass).StaticProperty); 
}

//class decorator
@Component
export class TargetClass
{ 
  static StaticProperty:string ="static property";
  constructor()
  {
    console.log("I am from ctor");
  }

}
let instanceOfTargetClass= new TargetClass();
//Injected prototype from decorator function into TargetClass
console.log((<any>instanceOfTargetClass).FrmDecoraator); 
```

***物业装修工:***

属性装饰器应用于类中的属性。它是一个接受类的原型和属性键作为输入参数的函数。例如，在下面的例子中，我们可以访问静态属性值，但是不能访问实例属性值，因为 decorators 是在实例创建之前创建的。

```
function propetryDecorators(target:any,propertyKey:string)
{
    console.log(target.constructor.name);
    console.log(`property Key name :${propertyKey}`);
    /*Property value is undefined for non staic property
     as it is not yet initialized, but we can access the value of static property here. */
    console.log(`property value :${target[propertyKey]}`);
}

export class propetryDecoratorExample
{
 @propetryDecorators
  name:string  = "property inside class"

  @propetryDecorators
  static staticProperty:string ="Static property";
}
let instance = new propetryDecoratorExample();
```

***法装饰者:***

顾名思义，这些装饰器应用于类中的方法。方法装饰器是带有三个参数的函数(类原型、方法名和关于方法的可选参数描述符)。例如 Nest 中的 AuthGaurd()就是一个方法修饰器的例子。AuthGaurd 在方法调用之前首先被触发。

***参数装饰器***

修饰方法的参数。例如:嵌套 js 路径中的 [@Body](https://hashnode.com/@Body) ()、 [@Param](https://hashnode.com/@Param) ()都是参数修饰类型。参数装饰器可以用来检查参数是否是方法的一部分。要进一步探究参数，比如说参数的类型，我们需要安装一个第三方工具 NPM install reflect-metadata—save-dev，并在 tsconfig.json 文件中启用“emideoratormetadata”:true。

```
import 'reflect-metadata';

function paramDecorator(target:any,methodName:string, parameterIndex:number)
{
console.log(`target: ${target.constructor.name}`);
console.log(`methodName : ${methodName}`);
console.log(`parameterIndex : ${parameterIndex}`);
//using reflection to probe more on the parameter.
let designType = Reflect.getMetadata(
    "design:type", target, methodName);
    console.log(`designType: ${designType}`);
    let designParamTypes = Reflect.getMetadata(
    "design:paramtypes", target, methodName);
    console.log(`paramtypes : ${designParamTypes}`);
    if(designParamTypes !== String)
    {
       console.log("not string");
    }
}
class ClassType
{
 Method(@paramDecorator param:number ,   nonParameterDecorator:string)
 {}
}
```

```
After enabling the “emitDecoratorMetadata”: true, Generated .js file will have info about the __decorate __metadata.
```

```
__decorate([
    __param(0, paramDecorator),
    __metadata("design:type", Function),
    __metadata("design:paramtypes", [Number, String]),
    __metadata("design:returntype", void 0)
], ClassType.prototype, "Method", null);
```

*进一步阅读本主题的参考资料*

由 Nathan Rozentals 先生编写的 Packt Mastering Type script 第二版

[https://docs.nestjs.com/custom-decorators](https://docs.nestjs.com/custom-decorators)

[https://JavaScript . plain English . io/master-the-typescript-key of-type-operator-bf7f 18865 a8b](https://javascript.plainenglish.io/master-the-typescript-keyof-type-operator-bf7f18865a8b)

感谢您的阅读。直到我们再次见面，我祝你一切顺利:)