# 颤动中的网络呼叫

> 原文：<https://blog.devgenius.io/network-calls-in-flutter-6ddd008a3a44?source=collection_archive---------0----------------------->

## 一个关于使用 **Dio** 的指南，它是 Flutter 中`http`包的一个替代品，用于简化网络请求和干净代码实践的方法。

![](img/040a4aa805bd5983bf0fd73cdc6f060d.png)

颤振+ Dio = ❤️

每个移动应用程序都必须通过互联网与外部 API 通信，以提供增强用户体验和增加应用程序功能集的附加功能。这可以包括身份验证、自定义业务逻辑、文件上传等。

大多数 Flutter 开发者使用`http`包来实现这一点。虽然这很有效，但是还有一个更好的、不太为人所知的包叫做[***Dio***](https://pub.dev/packages/dio)。

> Dio 是 Dart 的一个强大的 HTTP 客户端，它支持拦截器、全局配置、表单数据、请求取消、文件下载、超时等。

## **目录—**

1.  安装和设置
2.  基本要求
3.  表单数据和文件上传
4.  截击机
5.  样板工程

![](img/414fa2bc85a098076695c4d35a6860da.png)

Giphy 的 GIF

## 1.安装和设置

要将 Dio 添加到您的 flutter 项目中，只需将下面一行添加到您的`pubspec.yaml`文件中

```
dependencies:
   dio: ^3.0.9
```

然后运行— `flutter pub get`(如果您的代码编辑器没有自动获取依赖项的话)。

要使用 Dio，只需通过调用构造函数来创建 Dio 实例—

```
import 'package:dio/dio.dart';Dio dio = **new** Dio();
```

## 2.基本要求

让我们首先讨论基本的、`GET`、`POST`、`PUT`和`DELETE`请求。我将为每种类型的请求创建单独的函数，这样我们就可以在本文末尾编译的 helper 类中使用它们的更多细节。

```
**void** getHTTP(String url) **async** {
  **try** {
    Response response = **await** dio.**get**(url);
    // Do whatever
  } **on** DioError **catch** (e) {
    // Do whatever
  }
}**void** postHTTP(String url, Map data) **async** {
  **try** {
    Response response = **await** dio.**post**(url, data: data);
    // Do whatever
  } **on** DioError **catch** (e) {
    // Do whatever
  }
}**void** putHTTP(String url, Map data) **async** {
  **try** {
    Response response = **await** dio.**put**(url, data: data);
    // Do whatever
  } **on** DioError **catch** (e) {
    // Do whatever
  }
}**void** deleteHTTP(String url) **async** {
  **try** {
    Response response = **await** dio.**delete**(url);
    // Do whatever
  } **on** DioError **catch** (e) {
    // Do whatever
  }
}
```

## 3.表单数据和文件上传

大多数应用程序需要将文件/图像作为`multipart/form-data`发送到服务器，由于 Dio 包中所有可用的类和方法，使用 Dio 很容易实现这一点。

```
// Single File with Additional Data
FormData formData = FormData.fromMap({
  "name": "Ryan Dsilva",
  "age": 21,
  "file": **await** MultipartFile.fromFile("PATH", filename:"OPTIONAL"),
});// Multiple Files with Additional Data
FormData formData = FormData.fromMap({
  "name": "Ryan Dsilva",
  "age": 21,
  "files": [
    **await** MultipartFile.fromFile("PATH", filename:"OPTIONAL"),
    MultipartFile.fromFileSync("PATH", filename:"OPTIONAL")
  ],
}); 
```

> 对于任何想使用同步版本读取文件的人来说，有一个函数——`MultipartFile.fromFileSync()`，可以不使用`async`关键字。

然后，这个 FormData 对象可以像往常一样作为有效负载在`POST`和`PUT`请求中发送。

> **注意事项** —当发送多个文件时，表单数据中的关键字作为`files[]`而不是`files`发送，即包括方括号。所以如果你的 API 有问题，不要使用 FormData 的`fromMap()`构造函数，自己构建。我已经浪费了好几个小时来解决这个小细节。

## 4.截击机

几乎每个现代 API 都由 JSON Web 令牌保护，将授权令牌附加到头部的做法现在被认为是标准的。Dio 和常规的`http`包都允许给请求添加头，但是 Dio 有一个更有趣的东西叫做 ***拦截器*** 。

拦截器是在预期动作之前运行的代码块，即在动作完成之前拦截动作。拦截器可以在三个地方运行— `onRequest`、`onResponse`和`onError`

拦截器适用于每个请求、响应或错误，如果配置如下所示。拦截器的一些常见用例是使用 JSON Web 令牌的授权、JSON 解析等。拦截器的一般结构如下所示—

```
dio.interceptors.add(InterceptorsWrapper(
    onRequest:(RequestOptions options) **async** {
     *// Do something before the request is sent*
     **return** options;
    },
    onResponse:(Response response) **async** {
     *// Do something with response data*
     **return** response;
    },
    onError: (DioError e) **async** {
     *// Do something with response error*
     **return** e;
    }
));
```

对于附加 JSON Web 令牌的情况，拦截器可以编写如下

```
**Dio** addInterceptors(Dio dio) {
  return dio
    ..interceptors.add(InterceptorsWrapper(
    onRequest: (RequestOptions options) => reqInterceptor(options),
    onError: (DioError e) **async** {
      **return** e.response.data;
    }),
  );
}**dynamic** reqInterceptor(RequestOptions options) **async** {
  // Fetching JWT logic
  const token = '';  
  options.headers.addAll({"Authorization": "Bearer: $token"});
  **return** options;
}
```

`reqInterceptor()`从应用程序中获取 JWT，并将其添加到请求的头部。

## 5.样板模板

如果您已经阅读了所有这些部分，您会知道这会导致大量样板代码，即使使用 Dio 带来的语法糖，因此我觉得有必要创建一个可以在每个项目中重用的 Helper 类——更类似于各种模板。这一个文件可以在每个 Flutter 项目中创建，你已经设置好 Dio 并准备好了。这也有助于保持代码的可维护性和可读性。

这是整个助手类—

这是文件的重要部分—

*   ***基本选项*** —这些是可用于所有请求的全局设置。比如基本 URL、响应类型、超时等。可以在这里设置为全局变量。
*   ***拦截器*** —任何必须在请求/响应/错误之前运行的定制拦截器。对大多数人来说，连接 JWT 几乎是不需要动脑筋的事情，而且可能是大多数应用程序最常见的用例。
*   ***方法***——标准的 HTTP 方法`GET`、`POST`、`PUT`和`DELETE`被一个`try..catch`块包围，该块捕获`DioError`对象，这是 Dio 包装任何出现的错误的方式。

要使用这个类，只需创建一个对象，然后在需要的地方调用相应的方法

```
ApiBaseHelper api = ApiBaseHelper();Response res = api.get('ENDPOINT_URL');
```

就是这样！我希望你学到了新的东西，并很高兴在你的下一个项目中尝试 Dio。非常感谢你能走到这一步。如果这篇文章对你有帮助，请分享，感谢你的反馈。

![](img/1cee5c9cabde6ac5d07db3d430eb8bbe.png)

男高音 GIF

我很快会带着更多的内容回来！在 GitHub 上关注我，了解我的最新项目—

[](https://github.com/RyanDsilva) [## RyanDsilva -概述

### 21 ●全栈网络/应用开发者● Python 和深度学习●自然语言处理● Flutter ●音乐家…

github.com](https://github.com/RyanDsilva) 

[1] Dio by 扑华—[https://pub.dev/packages/dio](https://pub.dev/packages/dio)

[](https://pub.dev/packages/dio) [## 飞镖包

### Language: English | 中文简体 A powerful Http client for Dart, which supports Interceptors, Global configuration, FormData…

公共开发](https://pub.dev/packages/dio)