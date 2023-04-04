# Node.js 最佳实践—项目结构

> 原文：<https://blog.devgenius.io/node-js-best-practices-project-structure-48a3b4dcaad7?source=collection_archive---------0----------------------->

![](img/6bbe26e345b93370fa1b9e6d5ca9472b.png)

[明锐丹](https://unsplash.com/@octadan?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上的照片

像任何类型的应用程序一样，JavaScript 应用程序也必须写得很好。

否则，我们以后会遇到各种各样的问题。

在本文中，我们将看看在编写节点应用程序时应该遵循的一些最佳实践。

# 文件夹结构

我们的节点应用程序应该遵循一些标准的文件夹结构。

例如，我们可以有这样的东西:

```
src
│   app.js          
└───api             
└───config          
└───jobs            
└───loaders         
└───models          
└───services        
└───subscribers     
└───types 
```

`app.js`是 app 的入口。

`api`拥有端点的控制器。

`config`有环境变量和配置相关的东西。

`jobs`有预定的工作。

`loaders`拥有应用程序启动时运行的代码。

`models`拥有数据库模型。

`services`有业务逻辑。

`subscribers`拥有队列等事件处理程序。

`types`是 TypeScript 项目的类型定义。

# 三层架构

3 层架构由控制器、服务层和数据访问层组成。

控制器是客户端的接口。

它接受请求并发送响应。

服务层具有逻辑，它从控制器获取信息并返回给控制器。

数据访问层具有与服务层对话的数据库逻辑。

我们不应该将业务逻辑放在控制器中。

当我们编写单元测试时，将它们分开会使测试更容易。

当我们测试控制器时，我们可以模拟所有的服务实体。

# 业务逻辑的服务层

服务层用于业务逻辑。

这一切都是为了做控制器和数据库层不做的事情。

例如，我们可以写:

```
route.post('/', 
  validators.userSignup, 
  async (req, res, next) => {
    const userParams = req.body;
    const { user } = await UserService.Signup(userParams);
    return res.json(user);
  });
```

`UserService.Signup`有业务逻辑。

请求和响应由控制器处理。

# 发布/子层

发布/订阅层用于侦听来自外部源的事件。

pub 部分向其他模块发送数据。

分离这个逻辑是有意义的，因为它们创建了一个内聚层。

# 依赖注入

依赖注入让我们可以在一个地方处理所有的依赖初始化。

例如，我们可以写:

```
export default class UserService {
  constructor(userModel, companyModel, employeeModel){
    this.userModel = userModel;
    this.companyModel = companyModel;
    this.employeeModel = employeeModel;
  } getUser(userId){
    const user = this.userModel.findById(userId);
    return user;
  }
}
```

我们有`UserService`类，它在构造函数中包含了所有需要的依赖项。

然后我们在整堂课中使用。

我们可以使用第三方解决方案来简化这一过程。

[typedi](https://www.npmjs.com/package/typedi) 库为我们提供了一个依赖注入容器，让我们注入依赖。

我们可以通过编写如下代码将依赖注入与 Express 结合使用:

```
route.post('/', 
  async (req, res, next) => {
    const userParams = req.body;
    const userServiceInstance = Container.get(UserService);
    const { user} = userServiceInstance.Signup(userParams);  
    return res.json(user);
  });
```

我们从`typedi` 调用`Container.get`方法来获取`UserService`，这样我们就可以使用它了。

# Cron 作业和重复任务

Cron 作业和计划任务应该在它们自己的文件夹中。

我们可以使用服务层的业务逻辑。

此外，我们不应该使用`setTimeout`或其他原始方式来延迟代码的执行。

相反，我们应该使用一个库来帮助我们。

这样，我们可以控制失败的作业，并对成功的作业进行反馈。

![](img/e321323381bb07c3f65063627d2248aa.png)

照片由[亚历山大·巴甫洛夫·博尔莫丁](https://unsplash.com/@bormot?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄

# 结论

我们应该创建具有标准结构的应用程序。

这些文件夹紧密地组织了我们应用程序的不同部分。