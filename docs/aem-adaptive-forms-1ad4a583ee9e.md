# AEM 自适应表单

> 原文：<https://blog.devgenius.io/aem-adaptive-forms-1ad4a583ee9e?source=collection_archive---------1----------------------->

AEM 表单为我们提供了在运行时创建、更新、发布和管理表单的能力。使用 AEM 表单，我们可以添加、更新和删除 AEM 表单中的任何字段。

作为本博客的一部分，我们将讨论以下主题:

1.  为什么我们需要 AEM 表格？
2.  在本地 AEM 实例中设置 AEM 表单
3.  问题陈述
4.  表单是如何工作的？
5.  (计划或理论的)纲要
6.  表单创建和与模式的集成
7.  AEM 表单与 servlet 的集成

## 让我们试着理解为什么我们首先需要 AEM 表格？

假设我们有一个使用下面的 HTML 代码创建的传统表单，放在 form.html 内部

```
<form action="/bin/submitform">
  <label>First name:</label><br>
  <input type="text" id="fname"><br>
  <label>Last name:</label><br>
  <input type="text" id="lname">
</form>
```

# 作为代码的一部分部署在服务器上的上述 HTML 表单的问题:

1.  **任何与字段标签、类型或验证**相关的更新都需要更改代码并重新构建。
2.  **添加新字段或删除旧字段**也需要代码更改和新构建。
3.  为了**更新或添加表单，提交动作 URL** 也需要构建。
4.  添加新表单也需要构建。
5.  简而言之，做任何与形式相关的改变都需要一个构建。

**AEM 自适应表单使我们能够在运行时进行上述所有更改，而无需构建。**

# 在本地 AEM 实例中设置 Adobe 表单

1.  这将需要从[链接](https://experience.adobe.com/#/downloads/content/software-distribution/en/aem.html?fulltext=forms*&orderby=%40jcr%3Acontent%2Fjcr%3AlastModified&orderby.sort=desc&layout=list&p.offset=0&p.limit=24)安装适应性表单包。在左侧导航中搜索表单。

![](img/d95fb0041b36f8c1cc70babffec56154.png)

**重要提示:**

*   请检查您的 AEM 本地实例版本，并安装相同版本的 adaptive forms 软件包。按照以下步骤检查您的本地 AEM 实例版本:

![](img/eef85d9c08bfcefb9561e54ae7cc93e2.png)![](img/cd8d5a2201f19b4c4de026579c85bc20.png)

*   确保根据操作系统安装所需的。它可能发生在我们可以在 windows 操作系统上安装 Linux 表单包的地方，反之亦然。
*   如果您无法访问上述 URL 来下载自适应表单包，请联系您的主管或管理员。

2.接受 EULA 条款和条件，并点击下载下载此表格包。

![](img/086616eb8743ea7aeea6003f5cd03b3b.png)

3.进入[包管理器](http://localhost:4502/crx/packmgr/index.jsp)上传上面下载的**adobe-AEM FD-win-pkg-6 . 0 . 566 . zip**包。

![](img/b924e8395505fd2a2a3cf9678cb442d5.png)

4.单击 install 在 AEM 服务器上安装这个包，如下所示。

软件包安装可能需要大约 10-15 分钟。

![](img/3a1cff4ac5b4952307cee3cd3439a218.png)

5.成功安装后，将出现下面的弹出窗口，要求重启 AEM 服务器。

让我们重新启动 AEM 服务器并进行验证。

![](img/60ff5e93e93fa59d9c6cd33739797db1.png)

5.打开下面的 [URL](http://localhost:4502/sites.html/content) 以验证表单包是否成功安装。

![](img/2151fe68d7806893ee487c4b9e133c39.png)

# 形成问题陈述

1.  作为此博客的一部分，我们将创建以下表单:

![](img/62d196bfb6a38a3be0230faaec0f4e57.png)

2.在表单提交时，它将生成以下 JSON

```
{
 "fistName": "",
 "lastName": "",
 "age": "",
 "address": {
  "addressLine": "",
  "city": ""
 }
}
```

3.调用定制的 AEM servlet，以步骤 2 中提到的预定义 JSON 格式收集表单提交数据。

# 表单提交流程

表单包含多个字段。众所周知，在表单提交时，第三方 API 或 servlet 总是希望数据采用某种预定义的格式。

作为这个博客的一部分，我们将使用 **JSON Schema** 以 servlet 和第三方 API 期望的相同预定义格式收集数据。

![](img/a5e4ec64b16fe11564faf91dcef9e6c7.png)

# Adobe 表单实现

1.  打开下面的[网址](http://localhost:4502/sites.html/content)，点击表格平铺。

![](img/2151fe68d7806893ee487c4b9e133c39.png)

2.点击下面的表格和文档标题:

![](img/9c71b53f7b0930b20d9d0ade8ed83a76.png)

## 创建和上传模式

3.如前所述，让我们创建一个如下所示的 **JSON 模式**，Servlet 或第三方 API 将接受该模式。

用 **<文件名> .schema.json** 扩展名存储下面的模式文件。

```
{
  "$schema": "[http://json-schema.org/draft-07/schema#](http://json-schema.org/draft-07/schema#)",
  "$id": "[http://example.com/product.schema.json](http://example.com/product.schema.json)",
  "title": "User",
  "description": "User Form",
  "type": "object",
  "properties": {
    "firstName": {
      "type": "string"
    },
    "lastName": {
      "type": "string"
    },
    "age": {
      "type": "string"
    },
    "address": {
      "type": "object",
      "properties": {
        "addressline": {
          "type": "string"
        },
        "city": {
          "type": "string"
        }
      }
    }
  }
}
```

上面的 **JSON 模式**将创建下面的 **JSON** 作为输出

```
{
 "fistName": "",
 "lastName": "",
 "age": "",
 "address": {
  "addressLine1": "",
  "city": ""
 }
}
```

4.如下图所示选择元模型图块。

![](img/c3599325bdf2b4795ebc5785b460f33f.png)

5.点击创建按钮，选择**文件上传**选项，上传 **JSON 模式**。选择上面的模式文件并点击上传按钮，如下所示。

![](img/90d77bd60d591f2b3b5debcb7a3751e7.png)

6.它将在成功上传时开始出现，如下所示:

![](img/c3adfece0b7bca1c42301b0ccd93be5e.png)

# 创建表单

7.返回并点击下方的**表单&文档**平铺，如下图所示:

![](img/9c71b53f7b0930b20d9d0ade8ed83a76.png)

8.选择**空白模板**创建自适应表单，点击**下一个**按钮。

![](img/5809dbcab51ec83267065d397241f6b7.png)

9.在基本选项卡上输入**标题**和**名称**。

![](img/14684b80dbe4c77c31b27963913b8e5b.png)

10.选择**表单模型**选项卡，然后选择**模式作为选择自字段的一部分。选择我们在步骤 6 中上传的模式。**选定的 JSON 模式将帮助我们映射表单字段。相同的 JSON 将作为 post 请求数据的一部分。这与 servlet 和第三方 API 期望的 JSON 格式相同:

点击**创建**按钮创建一个表单。

![](img/ae1415d0ad914c8fe4f255af3708d5ec.png)

11.单击“编辑”按钮编辑表单，以添加字段、提交操作和其他必需的详细信息。

![](img/760000968f9b7bb533a5e88329f902a2.png)

12.突出显示的第 1 点是我们将在后续步骤中讨论的内容。

突出显示的 2 号点将显示我们要拖放的内容或字段层次结构。

突出显示的第 3 点是拖放表单字段。

![](img/d428b2c7cb6a444fa753d3c20c4f3c7c.png)

13.导航到下面突出显示的选项，并验证模式结构是否存在。

![](img/f5aaa2d24d015719a4067c8a638ec28e.png)

13.点击**拖动组件到这里——根面板** parsys 会弹出一个选择**面板**的窗口。

![](img/8c7c73b1d15b3ff9b86a53779267c00c.png)

14.选择**拖拽组件到**段，点击 **+** 按钮拖拽组件。

![](img/6d3059f67dd8378314eb71fff63b2726.png)

15.选择下面的**文本框**选项，以拥有一个文本字段。

![](img/8a5b359837faac47d2eefda580231322.png)

16.选择文本框 parsys 并单击配置按钮将打开左侧导航菜单，以填写字段特定信息。

![](img/7260931506e72bc3241683dec51a8a57.png)

17.提供名称、将成为字段标签的标题、占位符，选中**必填字段**复选框，提供必填字段错误消息。

下面我们来详细讨论一下高亮显示的**绑定引用**字段的红色。该字段将帮助我们在创建的字段和先前上传的模式之间建立联系。

![](img/62936193e45884e7e9cfc3a849f5f25c.png)

18.点击绑定参考选项后，将出现以下屏幕。选择必填字段，然后单击确定。

![](img/b38c599c5325291f4d6e6316779ace98.png)

19.对姓氏和年龄重复第 15、16、17 和 18 步。

![](img/b5f2256022d30878e095cf5bf8d3b6de.png)

20.作为此步骤的一部分，拖放作者下拉列表。

![](img/43cd92b282aaf95c6702fd70146f16df.png)

注意:这个下拉菜单我们只是用来学习的，它将帮助我们学习更多关于表单的特性。

21.配置下拉列表并填写下面突出显示的值:

![](img/cd2e9094284d6bfb53a08a094cd264c0.png)

22.平行于第一个面板再拖放一个面板。为面板提供自定义名称。

![](img/96131c324e732f81ef48dee648054d06.png)

23.在**第二个面板**中，拖放地址行和城市字段的剩余部分，重复步骤 15、16、17 和 18。

**注意:不要将地址行和城市字段设为必填。**

![](img/826e1681ea3f253452e715aa983de4fe.png)

24.我们希望在页面上隐藏的地址相关字段。这可以通过使用 OOTB 配置逐个隐藏所有字段来实现，这将是一项繁琐的任务。作为替代，我们可以隐藏面板本身，在面板下拖放所有与地址相关的字段，如下所示。

![](img/8686c8187d338903e2310ed5e107e97e.png)

25.作为表单**预览的一部分，**地址相关字段将不会出现。

![](img/75934c36e868c7aaae5e168bb92c15b6.png)

26.我们希望在选择**必填字段**“是”选项时显示所有地址字段。

选择下面突出显示选项来定义规则。

![](img/e5f7b1424bb10851a693fda008396102.png)

27.单击下面的“创建”按钮来定义规则:

![](img/2c8d74dcd290bbbf5a0ad3fe28cd6000.png)

28.从所需的**地址中选择“是”选项时，定义以下规则至显示的地址部分及其所有字段？**字段，点击**完成**按钮。

![](img/77fd8fff408ee83ad7430fd652cbca1d.png)

29.进入预览选项，选择**是选项，地址栏显示如下:**

![](img/c74c786cb6635018bb25872b9f686b02.png)

30.作为该步骤的一部分，我们将编写自定义代码，以在选择所需的发件人地址的**否**选项时再次隐藏地址部分。领域。

重复步骤 26 和 27。

选择下面的选项以遍历代码编辑器:

![](img/319bdc4a6ee5c52c80c0567be555a97a.png)![](img/0643f2df1fa1723d938c80a7f5557ab4.png)

31.在内联代码编辑器中粘贴下面的代码。

```
if (this.value == "no") {
    alert("Selected No");
    addressPanel.visible = false;
}
```

![](img/0d5e4bd976f1521fa6baa74d2966b319.png)

32.在选择选项为**时，No** 将弹出警告并隐藏地址部分及其字段。

![](img/bf403f55626d1ececd28f31dad71e719.png)

33.让我们为表单提交编写一个**提交按钮**:

![](img/66bd4ee08ede001fa3ba8a49b755e613.png)

34.选择顶部容器并点击配置按钮以执行表单级创作，并选择**提交动作**下拉字段下的**提交至 REST 端点**选项。

表单提交将是一个 POST 请求调用，因为我们将发送用户表单数据作为请求有效负载的一部分。

选择**启用 POST 请求**复选框，并提供第三方 API 或 servlet URL 作为 POST 请求字段的 **URL 的一部分。**

![](img/32bf23fcdf482b7c9aa702f3e0dd9b64.png)

# 创建内容页面并使用 AEM 表单

35.选中复选框，允许**常规**组下的 **AEM 表单**作为模板的一部分拖放到页面上。

![](img/b4c70e4b49f988ee50e0281a93d8bf26.png)

36.创建页面，拖放 AEM 表单组件。

![](img/8bc72c52c0c34c2d2454b54e7e78c997.png)

37.使用下面突出显示的选项配置组件。

![](img/f75d86a98998b85089c6dba9e43d7000.png)

38.在“资产路径”字段下提供我们作为此博客的一部分创建的 AEM 表单路径，然后单击“确定”将在页面上加载 AEM 表单。

作为该对话框的一部分，我们还可以提供感谢页面 URL 或消息。

![](img/90fd601f5e30d45aa828facec9a80615.png)

39.它将加载下面的 AEM 表单内容页面

![](img/b4ff4a6fb106082adea17a4973b515f8.png)

40.创建下面的 POST Servlet 来收集数据

提交的数据以参数的形式出现，可以使用下面的语法进行收集。

**req . getparameter(" data XML ")**

```
import org.apache.sling.api.SlingHttpServletRequest;
import org.apache.sling.api.SlingHttpServletResponse;
import org.apache.sling.api.servlets.HttpConstants;
import org.apache.sling.api.servlets.SlingAllMethodsServlet;
import org.osgi.framework.Constants;
import org.osgi.service.component.annotations.Component;
import org.slf4j.Logger;
import org.slf4j.LoggerFactory;

import javax.servlet.Servlet;
import java.io.IOException;

@Component(
    service=Servlet.class,
    property={
        Constants.*SERVICE_DESCRIPTION* + "=Custom Servlet",
        "sling.servlet.methods=" + HttpConstants.*METHOD_POST*,
        "sling.servlet.paths=" + "/bin/user"
    }
)
public class UserDetailServletByPath extends SlingAllMethodsServlet {
    private static final long *serialVersionUID* = 1L;
    private final Logger logger = LoggerFactory.*getLogger*(getClass());

    @Override
    protected void doPost(final SlingHttpServletRequest req,
        final SlingHttpServletResponse resp) throws IOException { logger.error("User >>>> {}", **req.getParameter("dataXml")**); resp.setStatus(SlingHttpServletResponse.*SC_OK*);
        resp.setContentType("application/json;charset=UTF-8");
        resp.getWriter().print("{\"response message\" : \" Service Called\"}");
    }
}
```

41.在表单中填入以下值，并在提交后收集值作为 Post servlet (/bin/user)的一部分。

![](img/dff7674a5ace75d41b84dd4bc2e34182.png)

## 输出:

检查 error.log 文件以验证作为表单的一部分提交的值。

![](img/df116655586fe105684504bf3970222e.png)

```
{
 "afData": {
  "afUnboundData": {
   "data": {
    "address": "yes"
   }
  },
  "afBoundData": {
   **"data": {
    "firstName": "John",
    "lastName": "Carter",
    "age": "28",
    "address": {
     "addressline": "Victoria",
     "city": "Washington"
    }**
   **}**
  },
  "afSubmissionInfo": {
   "computedMetaInfo": {},
   "stateOverrides": {},
   "signers": {}
  }
 }
}
```

成功的表单提交将进行以下三次后续调用:

![](img/9c0883ff67cc3568ec161016a520ec8b.png)

我希望你发现这篇文章有趣且内容丰富。请分享给你的朋友来传播知识。

您可以关注我即将发布的博客[关注](https://medium.com/@toimrank)。
谢谢！