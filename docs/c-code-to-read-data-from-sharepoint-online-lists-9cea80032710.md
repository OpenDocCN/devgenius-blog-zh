# 从 Sharepoint Online 列表读取数据的 C#代码

> 原文：<https://blog.devgenius.io/c-code-to-read-data-from-sharepoint-online-lists-9cea80032710?source=collection_archive---------5----------------------->

![](img/26d41e9888c5233a06e97b76423d8b3d.png)

图拉格摄影在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 拍摄的照片

Sharepoint 客户端库可用于读取、修改和删除 Sharepoint online 中的数据。尽管微软有关于如何使用代码示例库的优秀文档，但在 Sharepoint 上，数据可能是不同的格式，因此需要基于数据类型的不同读取方式。这些信息可以在文档中找到，但这需要一些时间和精力来找到它，还需要 Sharepoint 客户端库中不同类的先验知识。如果您只是想访问 Sharepoint 数据，而不了解 Sharepoint 客户端类是如何工作的，本文可能会节省一些时间。

[](https://docs.microsoft.com/en-us/sharepoint/dev/sp-add-ins/complete-basic-operations-using-sharepoint-client-library-code) [## 使用 SharePoint 客户端库代码完成基本操作

### 您可以使用 SharePoint 客户端对象模型(CSOM)来检索、更新和管理 SharePoint 中的数据。SharePoint…

docs.microsoft.com](https://docs.microsoft.com/en-us/sharepoint/dev/sp-add-ins/complete-basic-operations-using-sharepoint-client-library-code) 

在本文中，我将介绍 Sharepoint 列表最常用的数据类型，以及如何使用 Microsoft Sharepoint 客户端库读取它们。

**连接和认证**

我将使用 PnP。使用 Sharepoint 进行连接和身份验证的框架。关于通过 PnP 框架连接的不同方式的更多信息可以在这里找到。

[](https://github.com/pnp/pnpframework/blob/dev/docs/MigratingFromPnPSitesCore.md) [## PNP framework/migrating frompnpsitescore . MD at dev PNP/PNP framework

### PnP 站点核心已经存档，不再维护，但是 PnP 框架包含了几乎所有的 PnP 站点…

github.com](https://github.com/pnp/pnpframework/blob/dev/docs/MigratingFromPnPSitesCore.md) 

对于认证，我将只使用应用程序认证(最新版本的认证)以及证书。更多信息可以在我之前的文章中找到。

[](https://medium.com/@sujaykhandek/connect-to-sharepoint-online-using-azure-app-only-authentication-through-ssis-package-fbffce0d6365) [## 通过 SSIS 包连接到 Sharepoint Online。

### 有不同的方法通过 SSIS 连接到 sharepoint，使用 sharepoint 列表源，使用 ODATA 源。但是…

medium.com](https://medium.com/@sujaykhandek/connect-to-sharepoint-online-using-azure-app-only-authentication-through-ssis-package-fbffce0d6365) 

**连接/认证代码**

```
var cc = new PnP.Framework.AuthenticationManager("SharePointClientID","SharePointCertPath", "SharePointPassword",
"SharePointDomain").GetContext("SharePointSiteURL");List CMS = cc.Web.Lists.GetByTitle("SharePointList");
```

请注意，您将需要“”中上述代码中的所有信息。所有的信息都可以在之前关于连接和认证的文章中找到。

**从列表中读取所有行/字段**

```
// Load all rows from the target list
CamlQuery query = CamlQuery.CreateAllItemsQuery();
ListItemCollection items = CMS.GetItems(query);
cc.Load(items);
cc.ExecuteQuery();// Find all fields in the target list
FieldCollection myFields = CMS.Fields;
cc.Load(myFields);
cc.ExecuteQuery();
```

上述代码加载 sharepoint 特定列表类 items 和字段类 myFields 中的所有行。

为了访问 Sharepoint 数据中的每一行，我将使用 for 循环。请不要忘记，下面循环的所有代码都应该在循环内部运行。

```
foreach (ListItem listItem in items) {
```

**在 SSIS 脚本中访问字符串/日期时间/双精度/布尔型字段并分配给变量。**

我使用 SSIS 从 Sharepoint 列表中读取数据，因此我将把从 Sharepoint 列表中读取的值赋给脚本组件的 SSIS 输出变量。

```
Output0Buffer.outputStringVariable = Convert.ToString(listItem["string FieldName on sharepoint"]);Output0Buffer.outputDatetimeVariable = Convert.ToDateTime(listItem["datetime FieldName on sharepoint"]);Output0Buffer.outputBooleanVariable = Convert.ToBoolean(listItem["boolean FieldName on sharepoint"]);Output0Buffer.outputDoubleVariable = Convert.ToDouble(listItem["Double FieldName on sharepoint"]);
```

访问字符串变量是最简单的，只需要将 listitem 转换成字符串。

**重要提示:**如果你想访问包含空值的 float、boolean、datetime 类型的变量，那么，首先将它们转换为 string，然后显式地将它们转换为 cast 会更容易，因为使用 ToDatetime 或 ToDouble 会将空值转换为相应数据类型的默认值。

**访问特殊的 Sharepoint 数据类型**

sharepoint 中有一些特殊的数据类型需要在访问它们之前进行显式转换，例如 FieldLookupValue[]、FieldLookupValue、FieldUserValue。这段代码应该适用于大多数特殊的数据类型。

如果是这种特殊数据类型的列表，即 FieldLookupValue[]，则

```
public string GetLookupValue(ListItem listItem, string Name)
    {
        List<string> LookupField = new List<string>();
        var LookupIdField = listItem[Name] as FieldLookupValue[];

        foreach (FieldLookupValue lookupValue in LookupIdField as FieldLookupValue[])
        {
            LookupField.Add(Convert.ToString(lookupValue.LookupId));
            LookupField.Add(lookupValue.LookupValue);
        }
        string result = String.Join(";#", LookupField);
        return result;
    }// this code will join loopupId and loopupvalue with ;# separator// Inside the for loop above
Output0Buffer.outputStringVariable = GetLookupValue(listItem, "FieldLookupValue FieldName on sharepoint");
```

当它是单个特殊数据类型时，例如 FieldLookupValue，则

```
public string GetSingleLookupValue(ListItem listItem, string Name)
    {
        string result = "";
        var c = (FieldLookupValue)listItem[Name];
        result = c.LookupId + ";#" + c.LookupValue;
        return result;
    }// this code will join loopupId and loopupvalue with ;# separator// Inside the for loop above
Output0Buffer.outputStringVariable = GetSingleLookupValue(listItem, "FieldLookupValue FieldName on sharepoint");
```

对于不同的数据类型，你只需要替换下面一行代码。例如，要访问 FieldUserValue 类型的数据而不是 FieldLookupValue 类型的数据，请在上面的代码块中使用这一行代码。

```
var c = (FieldUserValue)listItem[Name];
```

要了解不同的特殊数据类型，可以参考文档。

[](https://docs.microsoft.com/en-us/previous-versions/office/sharepoint-csom/ee540961%28v=office.15%29) [## FieldLookupValue 类(Microsoft。SharePoint.Client)

### 升级到 Microsoft Edge 以利用最新的功能、安全更新和技术支持。

docs.microsoft.com](https://docs.microsoft.com/en-us/previous-versions/office/sharepoint-csom/ee540961%28v=office.15%29) 

我希望这对某人有帮助。我知道这是一个非常具体的话题，但微软目前还没有一个 sharepoint online connector 支持他们在 SSIS 的最新身份验证方法，所以我花了一些时间和阅读了多个文档，才开始使用这个和我以前的文章创建一个新的 sharepoint online connector，所以希望这可以为试图做同样事情的人节省时间。

如果有任何问题，请随时到 sujaykhandek@gmail.com 找我。