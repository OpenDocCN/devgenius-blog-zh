# 避免调用 Angular 模板中的表达式

> 原文：<https://blog.devgenius.io/avoid-calling-expressions-in-template-of-angular-39439c547653?source=collection_archive---------0----------------------->

代码中有很多乱七八糟、多余的调用。让我们重构它们。

![](img/ea79f29c6c4dcc7d7bd65372543b21e6.png)

这只是编写 Angular 应用程序的 HTML 模板的实用方面。

首先，我们需要运行 Lint 工具来发现可能的错误或警告。只需在 Angular 项目中打开 IntelliJ IDEA 终端并运行命令:

```
ng lint
// or for all files in project
npm run ng lint --files=src/app/**/*.ts// or try to run lint with 1 specific typescript file
npm run ng run projectName:lint ./src/my.component.ts
```

你会在你的角组件上看到很多警告。

让我们设想我们有这样一个简单的方法作为对象的属性:

```
<input value="{{ getName() }}" />
or
<div [termsOfName]=”getName()”> test </div>
```

这是一种低效的方法，会降低应用程序的性能。只需将`console.log("name")`添加到方法`getName()`中，你就会在浏览器的开发者工具中看到很多类似的调用。

生命周期挂钩

*Angular 在每个 DOM 事件之后运行的* [*变化检测*](https://angular.io/guide/glossary#change-detection) *过程中查找数据绑定值的变化:每次击键、鼠标移动、计时器滴答和服务器响应。*

当我们的函数在嵌套组件中调用时，生命周期更新，并且我们有许多类似的函数调用和任何 DOM 事件。

因此 Lint 认为这是一种无效的编程方式，并迫使我们重构代码。()括号用于当用户手动调用该方法时。[]括号只是向/从其他组件传递参数。

只是两种不同实践的一个例子:它们都工作得很好，但是…

```
<button value="Click"
  [termsDisabled]=”atLeastOneAdded()” />     // bad practice // rather better
  [termsDisabled]=”terms | validateAtLeastOneAddedWithPipe”>
</button>
```

我们不应该在模板中计算繁重的操作，我们应该只执行立即结束的方法。

有一些方法可以让我们的代码变得更好。

1.  **ngOnit()** -使用字段变量调用它们一次

通过每次触发`getName()`，我们只是减慢我们的应用程序。在具有大量元件的大部件的情况下，这成为一个问题。

在使用 ngOnInit 的方式中，`getName()`方法只会被调用一次，用户名的字符串表示会被赋给`name`变量。

```
name: string = "";
ngOnInit() {
  this.name = this.getName();
}
private getName(): string { return "data from other source"; }<input id="name" value="{{ name }}" />
```

**2。ngOnChanges()** —角度的有用标准接口

我们可以把我们的逻辑移到接口方法 ngOnChanges，如果输入对象的 1 得到一个新值，它就会触发。它将计算模板变量，并只提供结果，而不是许多多余的调用。

```
[@Component](http://twitter.com/Component)({
 template: `<div *ngIf="personIsAuthorized"></div>`
})
export class PersonComponent implements ngOnChanges {
  personIsAuthorized: boolean = false;

  @Input()
  isAuthorized?: boolean;

  ngOnChanges(changes: SimpleChanges) {
    if (changes.isAuthorized) {
      this.personIsAuthorized = this.isAuthorized;
    }
  }
}
```

在模板{{ personName }}中写入字段可能是一种简单的方法。当组件的输入参数获得新值时，我们只需用新的计算结果替换 personName。

```
[@Component](http://twitter.com/Component)({
 template: `
 <b>Morning, {{ personName }}</b>
 `
})
export class PersonComponent implements OnChanges {
 [@Input](http://twitter.com/Input)() person: { firstName: string, lastName: string };
 personName: string = "";

 ngOnChanges(changes: SimpleChanges) {
   if (changes.person) {
     this.personName = this.getPersonName();
   }
 }
 getPersonName() { 
   return this.person.firstName + " " + this.person.lastName;
 }
}
```

**3。管道** —它们允许我们缓存某个输入的结果。这确实减少了调用次数，因为只有当参数改变时才会调用管道。

通过告诉 Angular 管道是纯的，Angular 知道如果管道的输入不变，管道的返回值也不变。

Angular 仅在检测到输入值的纯变化时执行纯管道。纯粹的更改要么是对原始输入值(字符串、数字、布尔值、符号)的更改，要么是对对象引用(日期、数组、函数、对象)的更改。

让我们创建一个管道，并将一些逻辑移入其中。例如，我们有一个随时变化的电荷对象。该更改将调用一个管道并返回影响按钮属性的布尔结果。

```
import { Pipe, PipeTransform } from "[@angular/core](http://twitter.com/angular/core)";[@Pipe](http://twitter.com/Pipe)({
  name: "validateSelectedCharge",
  pure: true
})
export class ValidateSelectedChargePipe implements PipeTransform {
  transform(charges: Charges[]): boolean {
    return charges?.some(item => item?.selectedCharge?.tariffId);
  }
}and call it from html: send object 'charges' to the pipe
<button value="Click"
  [chargeDisabled]="charges | validateSelectedCharge" />now variable 'chargeDisabled' in other component has new value that calculated only once.
```

但是，在经常改变变量值的情况下…管道不是很好，因为管道在大多数情况下运行一次。如果您需要在同一个 web 页面上的组件之间发生变化时触发该值，最好使用常用的变量`{{ charges }}`。您将在 Typescript 级别分配一个新值，它将立即更新 html 模板级别的变量。

**4。异步管道**

我们可以使用异步管道只订阅一次可观察对象。而且会自动退订，不需要在 ngOnDestroy()里写退订方法。

```
@Component({
  selector: 'app-root',
  template: `<div [list]="postObservable | async"></div>`
})
export class PostComponent implements OnInit {
  postObservable: Observable<Post>;
  constructor(private postService: PostService) { } ngOnInit() {
    this.postObservable = this.postService.loadPost(0);
  }
}[@Injectable](http://twitter.com/Injectable)()
export class PostService{
  constructor(private http: HttpClient) { }

  loadPost(id:number) {
    return this.http.get<Post>("/api/post/${id}");
  }
}
```

通过将附加参数追加到模板中并使用`:`作为分隔符，可以将附加参数传递给 pipe 方法。
举例:`{{ param1 | pipeName : param2 : param3 }}`

```
export class PersonNamePipe implements PipeTransform {
  transform(person: any, args ? : any): string {
    return person?.firstName + " " + person?.lastName;
  }
}
```

**结论**

可以采用不同的策略来避免 HTML 级别上不必要的函数调用，并提高应用程序的性能。 **Lint** 完美显示错误和警告。在使用 Git 之前，用它来改进您的代码。

[](https://blog.angular-university.io/angular-reactive-templates/) [## 具有 ngIf 和异步管道的角反应模板

### 详细了解 Angular ngIf else 语法，包括它如何与异步管道集成以实现改进的…

blog.angular-university.io](https://blog.angular-university.io/angular-reactive-templates/)