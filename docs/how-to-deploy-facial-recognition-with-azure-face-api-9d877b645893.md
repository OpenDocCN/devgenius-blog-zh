# 如何使用 Azure Face API 部署面部识别

> 原文：<https://blog.devgenius.io/how-to-deploy-facial-recognition-with-azure-face-api-9d877b645893?source=collection_archive---------9----------------------->

Face API 是微软 azure AI 支持的服务，可以分析和检测图像中的人脸。

Azure Face API 允许您的应用程序将面部识别与单个 API 集成，消除了从头开发面部识别功能的麻烦。该服务提供最先进的算法，可以识别、检测和分析图像中的面部特征，使面部识别变得轻松自如。这篇文章讨论了 Azure Face API 服务的使用及其用例。

**为什么要投资 Azure Face API？**

面部识别和面部 API 是安全的重要方面，包括身份验证、出于隐私原因的面部模糊以及无数其他场景。面部应用编程接口可以帮助你建立安全的应用程序，内置控制安全访问帐户。如果没有基本的面部识别服务，您的应用程序上的数据很容易受到未经授权的访问，这可能会导致财务和数据损失。

# Azure Face API 用例

Azure Face API 帮助您:

1.身份验证

API 帮助您根据政府颁发的护照或其他身份证明文件来识别用户。身份验证场景包括新用户注册或开户、员工验证和在线评估。

2.无接触访问控制

Azure 面部识别消除了存在安全和卫生风险的物理卡。访问卡可能会丢失或被盗，而内置的 API 仍在服务中，没有任何延迟或中断。

Azure Face API 可用于机场、体育场、俱乐部、学校、健身房、医院以及任何需要加强其安全性的实体的签到。

3.表面修订

面部编辑允许您模糊视频或图像中个人的面部，以保护他们的隐私。这有助于您识别照片或视频中的特定个人，而不会泄露其他人的身份。

Azure Face API 也有助于找到相似的面孔，以帮助面部识别。

# 如何入门 Azure Face API 使用面向？网

要开始，获取您的 [**azure 订阅**](https://azure.microsoft.com/free/cognitive-services/) 。入门是免费的。您可以在开始使用 API 时进行升级。

第一步:获取 [**Visual Studio IDE**](https://visualstudio.microsoft.com/vs/) 或者最新版本的 [**。网芯**](https://dotnet.microsoft.com/download/dotnet-core) 。

步骤 2:您的 azure 帐户需要拥有认知服务贡献者角色，以接受服务条款并创建资源。您可以联系管理员将此角色分配给您的帐户。

步骤 3:一旦分配了角色，您就可以在 Azure 门户中创建您的 face 资源来检索您的端点和密钥。资源部署后，单击“转到资源”。接下来，您将需要端点和密钥来将您的应用程序连接到 Azure API。我们将在下面向您展示放置密钥和端点的位置。

步骤 4:在 Visual Studio 中，创建一个新的。NET 核心应用程序并安装客户端库。在解决方案资源管理器中右键单击“项目解决方案”，然后选择“管理 NuGet 包”。在软件包中，选择“包含”和“预发布”并搜索“Microsoft。azure . Cognitive services . vision . face .在那里，选择版本 2.7.0-preview.1，然后单击安装。

步骤 5:找到项目目录并导航到名为“program.cs”的文件。在这里，添加以下代码:

*使用系统；*

*使用系统。集合。泛型；*

*使用系统。木卫一；*

*使用系统。Linq*

*使用系统。穿线；*

*使用系统。线程。任务；*

*使用微软。azure . cognitive services . vision . face；*

*使用微软。azure . cognitive services . vision . face . models；*

步骤 6:导航到 Program 类，并为我们在步骤 3 中讨论的端点和键添加变量。参见下面的代码。

//从 Azure 门户中的 Face 订阅获取订阅密钥和端点。

*const string SUBSCRIPTION _ KEY = " PASTE _ YOUR _ FACE _ SUBSCRIPTION _ KEY _ HERE "；*

*const string 端点= " PASTE _ YOUR _ FACE _ ENDPOINT _ HERE "；*

第 7 步:接下来，找到应用程序的 main 方法，并使用下面的代码添加调用。(这些调用将在后面实现。)

*//认证。*

*iface client client = Authenticate(端点，SUBSCRIPTION _ KEY)；*

*//检测——从人脸中获取特征。*

*DetectFaceExtract(客户端，IMAGE_BASE_URL，RECOGNITION_MODEL4)。wait()；*

*//查找相似——从人脸列表中查找相似的人脸。*

*FindSimilar(客户端，IMAGE_BASE_URL，RECOGNITION_MODEL4)。wait()；*

*//验证—比较两幅图像是否为同一个人。*

*验证(客户端，IMAGE_BASE_URL，RECOGNITION_MODEL4)。wait()；*

*//Identify——识别人物组中的一张(多张)脸(本例中创建人物组)。*

*IdentifyInPersonGroup(客户端，IMAGE_BASE_URL，RECOGNITION_MODEL4)。wait()；*

*// LargePersonGroup —创建，然后获取数据。*

*LargePersonGroup(客户端，IMAGE_BASE_URL，RECOGNITION_MODEL4)。wait()；*

*//面分组——自动将相似的面分组。*

*组(客户端，IMAGE_BASE_URL，RECOGNITION_MODEL4)。wait()；*

*// FaceList —创建一个面部列表，然后获取数据*

在我们继续之前，请注意下面的接口代表了 Face 客户端库最突出的特性。

FaceClient 代表使用面部识别服务的授权。所有的 face 服务功能都需要这个接口，包括类和实例的创建。它需要您的订阅信息才能启动。

*FaceOperations 负责基本的人脸检测任务。*

*DetectedFace 表示关于一张脸的完整数据，从中你可以得到关于这张脸的更多信息。*

*FaceListOperations 管理存储在云中的 FaceList 结构(携带不同的分类面)。*

*persongrouppersontextensions 管理包含一组属于一个人的面孔的 Person 构造。*

*PersonGroupOperations 管理包含一组不同的 person 对象的组结构。*

第 8 步:在这一步中，您将使用. NET 的人脸客户端库来验证客户端，实现人脸检测、人脸识别、相似人脸功能。

验证客户端

对于此任务，创建一个新方法，并使用步骤 3 中的端点和关键数据进行初始化。

接下来，使用您的密钥创建一个新对象“ApiKey ServiceClient Credentials”。

接下来，您将把它与新的“FaceClient”对象的端点一起使用。参见下面的代码。

*/**

**认证*

**使用订阅密钥和区域创建客户端。*

**/*

*public static iface client Authenticate(字符串端点，字符串密钥)*

*{*

*返回新的 face client(new ApiKeyServiceClientCredentials(key)){ Endpoint = Endpoint }；*

*}*

步骤 10:在这一步中，添加一个代码来检测图像中的人脸。它会将检测到的人脸的 ID 打印到控制台，并在程序的内存中存储一份副本。参见下面的代码。

接下来，您可以使用下面的代码通过 DetectedFace.faceRectangle 属性在面周围绘制矩形。

步骤 11:接下来，创建一个 PersonGroup 来添加一个 identity 操作，该操作将一个图像与一个 PersonGroup 进行比较以进行识别。

使用下面的代码创建一个包含六个不同对象的 PersonGroup。添加字符串变量(如下所示)。请注意，字符串变量位于类的根，以正确表示 ID。

接下来，在新方法中添加以下代码。这段代码将执行识别操作。请注意，此代码定义了变量“sourceImageFileName”。

接下来，通过添加以下代码创建一个 Person 对象。请注意，每个 Person 对象都通过唯一的标识 ID 字符串链接到一个 PersonGroup。添加代码时，将变量的 client、RECOGNITION_MODEL 和 URL 传递到方法中。

步骤 12:培训新创建的人员组

您需要从图像中提取面部数据，并在不同的人对象中进行分类。然后训练新创建的人员组，使面部特征识别或识别与每个人对象相关联。使用下面的代码启动这个过程。

部署代码后，这个特定的 Person 组及其链接的 Person 对象就可以进行身份、验证和组操作了。

要启用面部识别，请添加以下代码。

下面的代码将把识别的结果打印到控制台。该应用程序会将源文件图像中的每一张脸与创建的 PersonGroup 中的一个人进行匹配。这也将结束您的识别方法。参见下面的代码。

此外，您还可以让应用程序找到相似的脸，检测面部特征进行比较，并找到和打印匹配。识别方法类后，您可以通过单击 IDE 窗口顶部的“调试”来运行应用程序。