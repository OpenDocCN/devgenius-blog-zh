# 设置您的全栈应用程序:Rails API 和 React/Redux

> 原文：<https://blog.devgenius.io/setting-up-your-full-stack-application-rails-api-and-react-redux-428ff7a7a7c0?source=collection_archive---------0----------------------->

## 创建了完整的堆栈应用程序，但需要帮助设置？这里有一个分步指南，介绍了设置 Rails API 和 React/Redux 前端所需的基础知识。

![](img/d451d3a050142c9a816dea600fb89933.png)

照片由[扬西·敏](https://unsplash.com/@yancymin?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄

# 目录

1.  [简介](#a1e9)
2.  [设置你的 Rails API](#39ac)
3.  [设置您的 React/Redux 前端](#1e1f)
4.  [结论](#1607)
5.  [想要更多？查看其他资源！](#24bf)

# 介绍

有时候开始一个新项目最困难的部分就是…开始。一旦你有了一个想法，画出你的实体关系图(ERD)和线框，决定你想要工作的特性，创建项目并将存储库连接到 GitHub，最后是时候开始编码了！

虽然您可能已经准备好开始构建您的项目，但所有项目都需要一个坚实的基础。这就是本指南的作用所在！对我来说，开始的设置通常是相同的，因此在单调的过程中，我已经忘记了某些步骤背后的推理。为了与那些好奇或需要每个步骤的提醒的人分享，本指南将解释，这样你就可以有意识地设置你的项目。

# 设置您的 Rails API

重申一下，以下步骤是我在设置 Rails API 时经常做的。也就是说，可能有些步骤(即 gems)是你的项目不需要的。另外，我经常用 Postgres 作为数据库来创建我的 Rails API。无论您选择使用哪个数据库，都没有其他步骤重要，但是我将强调每个部分的作用，以便您可以了解自己的项目。

1.  转到存放 Rails API 的项目目录
2.  使用以下内容切换到 Rails API 文件夹:

```
cd <rails_api_folder_name>
```

3.在您的 gem 文件中取消注释:

```
gem ‘bcrypt’
gem ‘rack-cors’
```

Bcrypt 是我们将用来散列我们用户的密码。rack-cors gem 支持跨来源资源共享(cors ),即当 web 应用程序发出跨来源 HTTP 请求时，该请求的来源(即域)不同于它自己的来源。

4.在您的 gem 文件中，您可以选择添加以下任意或所有 gem:

```
gem ‘jwt’
gem ‘active_model_serializers’
gem ‘activerecord-reset-pk-sequence’
```

“jwt”gem 用于设置用于用户认证的 JSON Web 令牌。serializer 的 gem 允许我们使用 serializer，这使我们能够控制发送到前端的内容。最后一个 gem(Active Record-reset-PK-sequence)用于您的种子数据，并将您的活动记录表的 id 重置为零。

5.取消对 config/initializer/CORS . Rb 中以下代码的注释，并将 origins 后面的“example.com”更改为“*”:

```
Rails.application.config.middleware.insert_before 0, Rack::Cors do
 allow do
  origins '*'resource '*',
   headers: :any,
   methods: [:get, :post, :put, :patch, :delete, :options, :head]
 end
end
```

将原点改为' * '允许任何原点发出请求。如果你有兴趣了解更多关于 CORS 的信息，请查看 Chris 的这个[深度解释](https://medium.com/ruby-daily/understanding-cors-when-using-ruby-on-rails-as-an-api-f086dc6ffc41)。

6.在您的终端运行中:

```
bundle install
spring stop
```

只有当您在 gem 文件中包含 gem“active record-reset-PK-sequence”时，您才需要运行 spring stop。

7.如果您想要包含多个序列化程序并让它显示多个深度关联(换句话说，如果您正在序列化深度嵌套的关联)，请在 config/initializer 中创建一个新文件。在新文件中，添加以下代码行:

```
ActiveModelSerializers.config.default_includes = '**'
```

你可以给这个文件起任何名字，但是我建议命名为“active _ model _ serializers.rb”。此外，即使这允许您使用嵌套关联的序列化程序，关联也只能是单向的。

8.使用以下工具创建您的模型、迁移、控制器和路由文件:

```
rails g resource <model_name> <attribute:datatype>
rails g resource Book title:string pages:integer author:belongs_to
```

别忘了添加你的联想！您可以使用 belongs_to 宏来设置外键和 belongs_to 关联。但是，这意味着你需要首先基于 has_many 进行创建；简单来说，首先创建独立/不依赖他人获取信息的模型。想一想:你不能先创作一本书而没有作者。

**有趣的事实**:通常你也会得到你的视图文件夹，但是如果你用- api 标志生成应用程序，它会配置生成器跳过视图文件夹。

**奖励**:如果您使用 Bcrypt 作为您用户的密码，而不是使用 password 作为属性(password:string)，那么您将使用“password_digest”作为表中的列。然而，在您的模式和迁移中，只有*才会使用“password_digest”。在其他任何地方，包括当您创建用户实例时，您都将使用“password”。例如:*

```
rails g resource User name:string age:integer password_digest:string
User.create!(name: “bob”, age: 15, password: “pw321”)
```

9.进入每个模型的文件(app/models ),为需要的模型添加验证和/或其余的关联宏。如果我们继续以作者和图书为例，您可以向作者模型添加:

```
has_many :books
validates :first_name, presence: true
```

如果使用 Bcrypt 作为用户密码，可以在用户模型中使用名为 has_secure_password 的宏。

10.如果您使用序列化程序，对于每个序列化程序，请具体说明您使用的关联宏。这里的关联宏与您的模型关联宏没有任何关系，您应该考虑将哪个关联发送到您的前端更重要。

11.使用以下内容创建您的 Postgres 数据库:

```
rails db:create
```

12.运行您的迁移并使用以下内容创建您的方案:

```
rails db:migrate
```

13.在 routes.rb 中指定您将需要的路径。由于 rails g resource，您将自动获得所有的 RESTful 路径，(基于前面的示例，它看起来像 resources :books)，但是如果您知道您不需要所有的路径，您可以单独写出您需要的路径。

14.在 db/seeds.rb 中创建一些种子数据，并用以下内容作为种子:

```
rails db:seed
```

在创建种子数据之前，您可能希望在您的控制台(rails c)中测试是否可以创建每个模型的实例。更具体地说，您应该检查您的验证和关联是否在工作。此外，在该文件的顶部，对于每个类，您可以从依赖/所属类开始，包括 class_name.destroy_all 和 class_name.reset_pk_sequence。

# **设置您的 React/Redux 前端**

以下步骤将用于 React/Redux 前端，但是如果您不想使用 Redux，您可以跳过 Redux 步骤！Redux just *在 React 已经拥有的基础上构建*，给你一个更简单的方法来管理你的状态；如果你选择不使用 Redux，你不会失去任何功能。

但是，要知道选择并入 Redux 而不使用它，就像一开始不并入 Redux，后来又想加入一样困难。也就是说，我不会解释 Redux 或与之相关的术语。如果你感兴趣，这里有一份亚历克斯·格里夫的[小抄](https://gist.github.com/alexgriff/0e247dee73e9125177d9c04cec159cc6)。

1.  转到存放 React 应用程序的项目目录。
2.  使用以下内容切换到 React 应用程序文件夹:

```
cd <react_app_folder_name>
```

3.在您的终端中键入以下命令来安装软件包:

```
npm install react-router-dom
npm install redux
npm install react-redux
```

“react-router-dom”是指如果你想在你的应用程序中加入路线。如果您想合并 redux，请仅安装“redux”和“react-redux”。

4.在 index.js 的顶部导入以下内容:

```
// ROUTING STUFF
import {BrowserRouter} from 'react-router-dom'// REDUX STUFF
import {createStore} from 'redux'
import {Provider} from 'react-redux'
```

此外，如果您决定使用多个减速器，可以稍后返回并添加，但可以选择启动:

```
import {combineReducers} from ‘redux’
```

5.在 index.js 中设置初始全局状态:

```
let initialStateOfBookReducer = {
   books: []
}
```

6.在 index.js 中设置您的减速器:

```
let bookReducer = (state = initialStateOfBookReducer, action) => {
 switch(action.type){
   default:
    return state
 }
}
```

7.如果您使用多个减速器(combineReducer ),您需要添加以下内容，以便为商店进行设置:

```
let combineReducerPojo = {
 books: bookReducer,
 author: authorReducer
}let rootReducer = combineReducers(combineReducerPojo)
```

8.在 index.js 中设置您的商店:

```
let storeObj = createStore(
 bookReducer,
 window.__REDUX_DEVTOOLS_EXTENSION__ && window.__REDUX_DEVTOOLS_EXTENSION__()
)
```

bookReducer 后面的一行是允许你使用你的[Redux DevTools](https://chrome.google.com/webstore/detail/redux-devtools/lmhkpmbekcpmknklioeibfkpmmfibljd)Google chrome 扩展。你也可以把同一行代码[复制到这里](https://github.com/zalmoxisus/redux-devtools-extension#11-basic-store)。此外，如果您正在使用 combineReducer 并遵循上面的步骤，而不是 bookReducer，它将是 rootReducer。

9.将存储传入 index.js 中的<provider>:</provider>

```
ReactDOM.render(
 <Provider store={storeObj}>
  <BrowserRouter>
   <App/>
  </BrowserRouter>
 </Provider>,
 document.getElementById('root')
);
```

只要应用程序由提供者和浏览器包装，提供者和浏览器的顺序并不重要。如果你没有使用 Redux，你不需要包含 Provider，只需要用 BrowserRouter 包装 App。

10.要设置路线，请在 App.js 中导入以下内容:

```
import { Route, Switch } from 'react-router-dom'
```

我们正在导入路由，以便将组件放在路径后面；换句话说，当你转到一个特定的路径时，所提供的组件就会呈现出来。我们还包含了 Switch，这样一次只能显示一条路由，因此只能显示一个组件。

您也可以导入以下内容，但它们在其他组件中可能更有用:Link、NavLink、withRouter、Redirect。Link 本质上是一个锚标记，而 NavLink 是 Link 的一个花哨版本，所以当 URL 与 NavLink 的路径匹配时，它将有一个“active”类名。将[与路由器](https://reactrouter.com/web/api/withRouter)一起使用可以访问 this.props.history 和最近的<路线>的匹配。重定向正如它的名字所暗示的那样；它会重定向到一个新位置。

11.要使用路线，在 App.js 的返回中:

```
return (
 <div className=”App”>
  <Switch>
   <Route path="/" exact component={Home}/>
  <Switch/>
 </div>
)
```

这是用路由和交换机设置路由的基础；只需将您的路由打包在一个交换机中。请注意，编写路由的顺序很重要，因此一个有用的技巧是使用“exact”来指定只有当 url 与提供的路径完全匹配时，组件才会呈现。这对于路径起点可能相同的嵌套路由尤其重要。

12.奖金！如果您使用 Redux，对于每个需要或更新全局状态的组件，在每个文件的顶部，您需要:

```
import {connect} from ‘react-redux’
```

导出组件时，您将像这样导出:

```
export default connect(mapStateToProps, mapDispatchToProps)(App)
```

要将 withRouter 用于某个组件，您将使用包装在 withRouter 中的 App 导出:

```
export default connect(mapStateToProps, mapDispatchToProps)(withRouter(App))
```

# 结论

现在你可以开始真正的工作了。下一步，当之无愧的舞蹈休息！

有一个坚实的工作基础总是很重要的；我希望这份指南能给你信心，让你有一个好的开始，并能继续你的项目。非常感谢您的阅读，祝您建设愉快！

如果你喜欢这篇文章，看看我的其他文章！一个好的起点是我的相关文章，“[创建您的全栈应用程序:Rails API 和 React](https://medium.com/dev-genius/creating-your-full-stack-application-rails-api-and-react-7155026e453a) ”，或者“[获取请求和控制器动作:将前端连接到后端](https://medium.com/swlh/fetch-requests-and-controller-actions-connecting-the-frontend-to-the-backend-733a87ffe757)”，它涵盖了全栈应用程序的请求响应周期。

# 想要更多吗？查看其他资源！

## **设置您的项目**

📖[https://dev.to/sylwiavargas/recipe-for-rails-backend-1m4i](https://dev.to/sylwiavargas/recipe-for-rails-backend-1m4i)📖[https://medium . com/swlh/cheat sheet-for-setting-a-rails-back end-and-a-react-frontend-terminal-commands-1 bacea 893 a80](https://medium.com/swlh/cheatsheet-for-setting-up-a-rails-backend-and-a-react-frontend-terminal-commands-1bacea893a80)

## 指定要使用的端口

📖[https://guides.rubyonrails.org/kindle/command_line.html](https://guides.rubyonrails.org/kindle/command_line.html)

## 克-奥二氏分级量表

📖[https://medium . com/@ admatbandara/setting-up-CORS-to-my-rails-API-a 6184 e 461 a 0f](https://medium.com/@admatbandara/setting-up-cors-to-my-rails-api-a6184e461a0f)
📖[https://medium . com/ruby-daily/understanding-CORS-when-using-ruby-on-rails-as-an-API-f 086 DC 6 ffc 41](https://medium.com/ruby-daily/understanding-cors-when-using-ruby-on-rails-as-an-api-f086dc6ffc41)

## 使用 JWT 认证

📖[https://medium . com/better-programming/build-a-rails-API-with-jwt-61 FB 8 a 52d 833](https://medium.com/better-programming/build-a-rails-api-with-jwt-61fb8a52d833)

## 反应路线

📖[https://medium . com/the-andela-way/understanding-the-fundamentals-of-routing-in-react-b29f 806 b 157 e](https://medium.com/the-andela-way/understanding-the-fundamentals-of-routing-in-react-b29f806b157e)

## Redux 术语备忘单

📖[https://gist . github . com/Alex Griff/0e 247 dee 73 e 9125177d 9 c 04 CEC 159 cc 6](https://gist.github.com/alexgriff/0e247dee73e9125177d9c04cec159cc6)