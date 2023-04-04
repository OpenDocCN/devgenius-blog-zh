# 通过使用 Tailwind CSS-2 构建 fintech 应用程序来学习 Angular 9。登录表单

> 原文：<https://blog.devgenius.io/learn-angular-9-by-building-a-fintech-application-with-tailwind-css-2-login-form-b5269f9c1d51?source=collection_archive---------1----------------------->

![](img/a89b2582d2e1e143880a1a9bb33ec367.png)

[Duomly —编程在线课程](https://www.duomly.com)

本文原载于[https://www . blog . duomly . com/angular-course-building-a-banking-application-with-tailwind-CSS-lesson-2-log in-form/](https://www.blog.duomly.com/angular-course-building-a-banking-application-with-tailwind-css-lesson-2-login-form/)

几天前，我们发布了 Angular 课程的第一课，我们正在为银行应用程序构建前端，你可以在 [SQL 注入教程](https://www.blog.duomly.com/sql-injection-attack-tutorial-for-beginners/)中看到。

在第一课中，我们开始使用 [Angular CLI](https://cli.angular.io/) 设置项目，实现 [TailwindCSS](https://tailwindcss.com/) ，并为登录表单创建 UI。

今天，我想更进一步，为我们已经创建的组件创建逻辑。因此，如果您在上一部分中与我一起创建应用程序，那么让我们打开代码并启动应用程序。如果没有，请查看[角度课程—第 1 课:启动项目](https://www.blog.duomly.com/angular-course-building-a-banking-application-with-tailwind-css-lesson-1-start-the-project/)或查看我们的 [GitHub](https://github.com/Duomly/angular9-tailwind-bank-frontend/tree/Angular9-TailwindCSS-Course-Lesson1) 并从那里获取代码。

当然，和往常一样，如果你更喜欢看教程而不仅仅是阅读，我们会在我们的 Youtube 频道上为你准备一段视频。

双向角航向

在播放列表中，您还可以找到我们为您准备的本课程的第一课和其他课。

还有一件事，在后端，我将使用你可以在我们的 [Golang 课程](https://www.blog.duomly.com/golang-course-with-building-a-fintech-banking-app-lesson-1-start-the-project/)中找到的应用程序，所以你也可以创建和使用它。

让我们粉碎棱角！

# 1.为登录组件创建逻辑—处理输入和验证

让我们使用`ng serve`启动现有项目，并打开`login.component.ts`文件。我想从创建一个函数开始，该函数将从输入中获取并保存值，以便能够在提交函数中使用它们。

让我们首先定义两个值 username 和 password，并为这两个值分配一个空字符串。

```
username: string = '';
password: string = '';
```

现在，我们将创建一个`onKey()`函数:

```
onKey(event: any, type: string) {
    if (type === 'username') {
        this.username = event.target.value;
    } else if (type === 'password') {
        this.password = event.target.value;
    }
}
```

现在，我们需要为用户名字段创建一个验证来防止 SQL 注入。我的朋友向你展示了如何将 SQL 命令输入到应用程序中，我们希望避免这种情况，所以，如果用户名不仅仅是字母和数字，我们将不允许发送用户名。

让我们在`password: string = '';`下创造另一个价值`isUsernameValid: boolean = true;`。

```
isUsernameValid: boolean = true;
```

现在，让我们创建一个 validateUsername()函数。

```
validateUsername(): void {
    const pattern = RegExp(/^[\w-.]*$/);
    if (pattern.test(this.username)) {
        this.isUsernameValid = true;    
    } else {
        this.isUsernameValid = false;
    }
}
```

为了让它工作，我们将这个函数放在`onKey()`函数中。

```
onKey(event: any, type: string) {
    if (type === 'username') {
        this.username = event.target.value;
        this.validateUsername();
    } else if (type === 'password') {
        this.password = event.target.value;
    }
}
```

这一步的最后一点是在模板中实现 onKey 函数，并在用户名无效时显示错误消息。让我们打开`login.component.html`，找到输入。

```
<form class="bg-white shadow-md rounded px-8 pt-6 pb-8 mb-4">
  <img class="logo" src="../../assets/logo.png" />
  <div class="mb-4">
    <label class="block text-gray-700 text-sm font-bold mb-2" for="username">
      Username
    </label>
    <input class="shadow appearance-none border rounded w-full py-2 px-3 text-gray-700 leading-tight focus:outline-none focus:shadow-outline" id="username" type="text" placeholder="Username" (keyup)="onKey($event, 'username')" required>
    <p *ngIf="!isUsernameValid" class="text-red-500 text-xs italic">Your username can consist of letters and numbers only!</p>
  </div>
  <div class="mb-6">
    <label class="block text-gray-700 text-sm font-bold mb-2" for="password">
      Password
    </label>
    <input class="shadow appearance-none border rounded w-full py-2 px-3 text-gray-700 mb-3 leading-tight focus:outline-none focus:shadow-outline" id="password" type="password" placeholder="******************" (keyup)="onKey($event, 'password')" required minlength="6">
  </div>
  <div class="flex items-center justify-between">
    <button class="bg-blue-500 hover:bg-blue-700 text-white font-bold py-2 px-4 rounded focus:outline-none focus:shadow-outline" type="button" (click)="onSubmit()">
      Sign In
    </button>
    <a class="inline-block align-baseline font-bold text-sm text-blue-500 hover:text-blue-800" href="#">
      Forgot Password?
    </a>
  </div>
</form>
```

太好了，现在我们准备好创建服务并进行 API 调用了！

# 2.如何在 Angular 中创建服务？

让我们用 Angular CLI 来创建`login.service.ts`。为此，我们需要以下命令:

`ng generate service services/login/login`

让我们打开创建的服务，从导入所需的包开始。

```
import { HttpClient, HttpHeaders } from '@angular/common/http';
import { BehaviorSubject } from 'rxjs';
```

现在，让我们定义一个名为 httpOptions 的常量，它将是我们的 API 请求的一部分。

```
const httpOptions = {
  headers: new HttpHeaders({
    'Access-Control-Allow-Origin': '*',
  })
};
```

下一步是定义我们的 URL 并将`HttpClient`传递给构造函数。

```
url: any = <your_api_url>; constructor(
    private http: HttpClient,
  ) { }
```

很好，现在我们可以开始创建登录功能了。在函数内部，我们需要处理响应。如果我们将获得一个令牌，这意味着用户已登录。我们将这些数据保存到会话存储中，以保持用户仅在会话期间登录。

```
login(Username: string, Password: string): any {
    this.http.post(this.url, { Username, Password }, httpOptions).toPromise().then((res: any) => {
      if (res && res.jwt) {
        sessionStorage.setItem('jwt', res.jwt);
      }
    });
  }
```

除此之外，如果登录调用将返回一个错误消息，我们需要处理它。为此，我们将使用来自`rxjs`的`behaviorSubject`。来实施吧！

```
errorSubject: any = new BehaviorSubject<any>(null);
errorMessage: any = this.errorSubject.asObservable();...login(Username: string, Password: string): any {
    this.http.post(this.url, { Username, Password }, httpOptions).toPromise().then((res: any) => {
      if (res && res.jwt) {
        sessionStorage.setItem('jwt', res.jwt);
        this.errorSubject.next(null);
      } else if (res.Message) {
        this.errorSubject.next(res.Message);
      }
    });
  }
```

让我们从`@angular/router`导入`Router`，在登录到仪表板视图后重定向用户。

```
import { Router } from '@angular/router';
...
constructor(
  private http: HttpClient,
  private router: Router,
) { }...
login(Username: string, Password: string): any {
  this.http.post(this.ulr, { Username, Password }, httpOptions).toPromise().then((res: any) => {
    if (res && res.jwt) {
      sessionStorage.setItem('jwt', res.jwt);
      this.errorSubject.next(null);
      this.router.navigateByUrl('dashboard');
    } else if (res.Message) {
      this.errorSubject.next(res.Message);
    }
  });
}
```

在我们让它一起工作之前，我们不能忘记将`HttpClientModule`添加到`app.module.ts`文件中。因此，让我们打开它，并确保您的代码看起来像下面的一个。

```
import { BrowserModule } from '@angular/platform-browser';
import { NgModule } from '@angular/core';
import { HttpClientModule } from '@angular/common/http';import { AppRoutingModule } from './app-routing.module';
import { AppComponent } from './app.component';
import { LoginComponent } from './login/login.component';
import { DashboardComponent } from './dashboard/dashboard.component';@NgModule({
  declarations: [
    AppComponent,
    LoginComponent,
    DashboardComponent
  ],
  imports: [
    BrowserModule,
    AppRoutingModule,
    HttpClientModule
  ],
  providers: [],
  bootstrap: [AppComponent]
})
export class AppModule { }
```

好了，现在我们可以回到`login.component.ts`文件，让我们使用 LoginService 创建一个`onSubmit()`函数。当然，我们必须导入服务并将其设置在构造函数中。

```
import { LoginService } from './../services/login/login.service';...constructor(
  private loginService: LoginService,
) { }onSubmit() {
  if (this.isUsernameValid) {
    this.loginService
      .login(this.username, this.password);
  }
}
```

接下来我们要添加的是错误值，如果错误没有出现，我们将在这里监听。我们将在`ngOnInit()`方法中订阅来自 LoginService 的错误消息。

```
error: any = null;
... ngOnInit(): void {
    this.loginService
      .errorMessage
      .subscribe(errorMessage => {
        this.error = errorMessage;
      });
  }
```

我们就快成功了，我们只需要切换到`login.component.html`文件并向我们的模板添加值。

首先，让我们将`onSubmit()`功能分配给我们的登录按钮。

```
<button class="bg-blue-500 hover:bg-blue-700 text-white font-bold py-2 px-4 rounded focus:outline-none focus:shadow-outline" type="button" (click)="onSubmit()">
  Sign In
</button>
```

接下来让我们填充通知元素中缺少的值。同样，让我们定义元素何时可见。

```
<div *ngIf="error" class="notification bg-indigo-900 text-center py-4 lg:px-4">
    <div class="p-2 bg-indigo-800 items-center text-indigo-100 leading-none lg:rounded-full flex lg:inline-flex" role="alert">
      <span class="flex rounded-full bg-indigo-500 uppercase px-2 py-1 text-xs font-bold mr-3">ERROR</span>
      <span class="font-semibold mr-2 text-left flex-auto">{{error}}</span>
    </div>
</div>
```

瞧啊。登录差不多了！现在是时候采取防范措施，防止未登录的用户进入应用程序。

# 3.如何在角路由中创建守卫？

因此，首先让我们使用 Angular CLI 创建一个保护文件。

`ng generate guard services/guards/auth-guard`

现在，让我们来看看我们的`login.service.ts`文件，我们将在这里再创建一个函数`isAuthenticated()`。

```
isAuthenticated() {
    if (sessionStorage.getItem('jwt')) {
      return true;
    } else {
      return false;
    }
  }
```

好了，我们可以切换到我们的`auth-guard.service.ts`文件了。首先，让我们导入登录服务、路由器和激活。

```
import { Router, CanActivate } from '@angular/router';
import { LoginService } from './login/login.service';
```

接下来，我们可以在构造函数中设置 LoginService 和 Router，然后创建 canActiveate()函数，如果用户通过身份验证，该函数将返回 true，如果用户未通过身份验证，该函数将把我们重定向到仪表板组件，并返回 false。

```
@Injectable()
export class AuthGuardService implements CanActivate {
  constructor(
    public login: LoginService,
    public router: Router) { } canActivate(): boolean {
    if (!this.login.isAuthenticated()) {
      this.router.navigateByUrl('/');
      return false;
    }
    return true;
  }
}
```

# 4.创建仪表板组件

让我们使用 Angular CLI 创建一个仪表板组件。

`ng generate component dashboard`

现在，让我们打开`dashboard.component.ts`文件，我们将在那里创建一个简单的标题。

```
<div id="dashboard-component" class="container mx-auto grid">
  <div class="flex mb-4 mt-24">
    <div class="mx-auto">
      <h3 class="text-gray-700 font-normal mx-auto">
        Hi User, here is your dashboard:
      </h3>
    </div>
  </div>
</div>
```

此外，让我们在 dashboard.component.scss 文件中添加一行自定义样式。

```
#dashboard-component {
  h3 {
    font-size: 24px;
  }
}
```

最后一步，让我们为仪表板组件添加一个路由。请打开`app-routing.module.ts`文件并创建一条新路线。在这条路线中，我们将使用 AuthGuard，因此我们也需要导入它。

```
import { DashboardComponent } from './dashboard/dashboard.component';
import { AuthGuardService as AuthGuard } from './services/auth-guard.service';const routes: Routes = [
  { path: '', component: LoginComponent },
  { path: 'dashboard', component: DashboardComponent, canActivate: [AuthGuard] },
];
```

现在，是时候测试它了！

# 结论

我希望你能够为第一课和第二课创建代码，并为我们的应用程序运行登录。

记得看一下 Golang 课程，在那里你可以为这个应用程序构建一个后端，这样你就有了一个完全运行的项目。如果你没有后端，你可以很容易地模仿数据。

在接下来的课程中，我们将让用户数据显示在仪表板上，并创建其他有趣的功能。

请继续关注，如果你喜欢这门课程，请在评论中告诉我们！
如果你迷失在代码中，这里有一个 GitHub 库，包含第二课，这样你就可以比较代码并找到错误。

[角度航向—第二课— Github 代码](https://github.com/Duomly/angular9-tailwind-bank-frontend/tree/Angular9-TailwindCSS-Course-Lesson2)

感谢您的阅读，
来自 Duomly 的安娜

![](img/da0dbcf44b16aceb7cca413a6d9a5ef7.png)

[Duomly —编程在线课程](https://www.duomly.com)