# 使用 Azure CLI 和 Kudu 管理您的 Azure 部署插槽

> 原文：<https://blog.devgenius.io/manage-your-azure-deployment-slots-with-azure-cli-and-kudu-5341e3d810c0?source=collection_archive---------3----------------------->

![](img/870ef57ca03e263068e25315d6297de4.png)

在本文中，我将向您展示如何使用 Azure CLI 创建和管理 Azure 部署插槽。

# 创建 Azure Web 应用资源

首先，您需要登录您的 Azure 帐户。为此，请执行以下命令:

```
az login
```

然后，您可以继续创建资源组、应用服务计划和 web ap 资源。

```
# Login to your Azure account
az login# Create a new resource group
az group create -n training-rg -l westus# Create a new app service plan
az appservice plan create -g training-rg -n training-plan --sku S1# Create a new Web App resource
az webapp create -g training-rg -p training-plan -n origintechnologiestraining
```

## 添加部署插槽

要创建新的插槽，请使用以下命令:

```
az webapp deployment slot create -n origintechnologiestraining -g training-rg --slot staging
```

# 创建和部署. NET 5 应用程序

既然你的 Azure 资源已经准备好了，是时候创建你的 web 应用了。在这个演示中，我将使用 dotnet CLI 创建一个新的 API 项目。

```
dotnet new webapi -o Training -f net5.0
```

要在本地运行您的应用程序，请运行以下命令

```
dotnet run -p Training\Training.csprojBuilding...
info: Microsoft.Hosting.Lifetime[0]
      Now listening on: [https://localhost:5001](https://localhost:5001)
info: Microsoft.Hosting.Lifetime[0]
      Now listening on: [http://localhost:5000](http://localhost:5000)
info: Microsoft.Hosting.Lifetime[0]
      Application started. Press Ctrl+C to shut down.
info: Microsoft.Hosting.Lifetime[0]
      Hosting environment: Development
info: Microsoft.Hosting.Lifetime[0]
      Content root path: C:\Users\ivanp\Desktop\Training
```

# 部署应用程序

是时候部署您的应用程序了。为此，你必须编译并创建一个应用程序及其依赖项的发布包，压缩该包，然后使用 kudu 将其推送到 Azure。这听起来可能不太容易，因为这只是几个命令的问题。

```
# Compile and publish the application and its dependencies
dotnet publish -f net5.0 -o Release -c Release Training.csproj# Create a ZIP archive of the output folder of the dotnet publish command
Compress-Archive -Path Release/* -DestinationPath release.zip# Deploy the ZIP file to your web app using the kudu zip push deployment
az webapp deployment source config-zip -g training-rg -n origintechnologiestraining --src release.zip
```

除了最后一条指令之外，前面的指令对于部署插槽是相同的，最后一条指令也将使用 **-s** 参数指定插槽名称。

```
az webapp deployment source config-zip -g training-rg -n origintechnologiestraining --src release.zip -s staging
```

恭喜你！您已经成功地在 Azure 上部署了新的 API。

# 交通路线

使用 Azure CLI，插槽之间的路由流量配置非常简单。事实上，您只需执行以下命令就可以做到:

```
az webapp traffic-routing set --distribution staging=50 --name origintechnologiestraining -g training-rg
```

# 交换部署插槽

要交换两个插槽，请在命令提示符下执行以下命令:

```
az webapp deployment slot swap -g training-rg -n origintechnologiestraining --slot staging --target-slot production
```

# 参考

*   **蔚蓝 CLI:**https://docs.microsoft.com/en-us/cli/azure/?[view=azure-cli-latest](https://docs.microsoft.com/en-us/cli/azure/?view=azure-cli-latest)