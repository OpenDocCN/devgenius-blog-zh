# 通过 Java Servlets 和 Java Web 服务(SOAP)向 ADABAS REST 服务发送 HTTP 请求

> 原文：<https://blog.devgenius.io/creating-soap-service-d8264ed1f3b7?source=collection_archive---------5----------------------->

HTTP 请求，Java Servlet，Java Web 服务，SOAP，WSDL 代码示例

![](img/034c4ae9425fb4af847122dff64b706b.png)

[灭霸好友](https://unsplash.com/es/@thanospal?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上的照片

Servlet 是一种 Java 编程语言类，用于扩展托管通过请求-响应编程模型访问的应用程序的服务器的功能(参见 [Oracle Java Servlet](https://docs.oracle.com/cd/E19798-01/821-1841/bnafe/index.htm) )。Web 服务也是基于请求-响应编程模型的，但是在 Java 编程的上下文中，它更特定于 REST 风格或基于 SOAP 的通信(参见 [O'Reilly Web Services](https://www.oreilly.com/library/view/java-web-services/9780596157708/ch01.html) )。这篇文章演示了向 ADABAS REST 服务发送 HTTP 请求的 Java Servlet 和 Java Web 服务编程。

```
PRE-REQUISITE:
This post works on the project WEB1 that was created in the [previous post](https://medium.com/dev-genius/sending-ajax-request-with-jwt-token-to-access-adabas-rest-web-services-28baa254e9ae). 
Read that post and create the WEB1 project first.
```

# (1) Java Servlet

右键单击 **WEB1** 项目，选择新建/其他…然后在“选择向导”窗口中选择 **Servlet** 。

![](img/92e331b73a70c663b58a2b0da05a232d.png)

输入包名，如`com.adabase.employee`和类名，如`ProcessLogin`。单击下一步按钮。

![](img/216491dc24f8853b9f649120106facdd.png)

输入一些描述，例如`send basic auth to backend`。

![](img/42087361e6f701a10ed892e105eea346.png)

接受修饰符、接口和方法的默认值。单击完成按钮。

![](img/81036ea143d0e4c58bc866ec0447c27c.png)

IDE 为**ProcessLogin.java**生成代码，如下所示。

```
package com.adabase.employee;

import java.io.IOException;
import javax.servlet.ServletException;
import javax.servlet.annotation.WebServlet;
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;

/**
 * Servlet implementation class ProcessLogin
 */
@WebServlet(description = "send basic auth to backend", urlPatterns = { "/ProcessLogin" })
public class ProcessLogin extends HttpServlet {
 private static final long serialVersionUID = 1L;

    /**
     * @see HttpServlet#HttpServlet()
     */
    public ProcessLogin() {
        super();
        // TODO Auto-generated constructor stub
    }

 /**
  * @see HttpServlet#doGet(HttpServletRequest request, HttpServletResponse response)
  */
 protected void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
  // TODO Auto-generated method stub
  response.getWriter().append("Served at: ").append(request.getContextPath());
 }

 /**
  * @see HttpServlet#doPost(HttpServletRequest request, HttpServletResponse response)
  */
 protected void doPost(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
  // TODO Auto-generated method stub
  doGet(request, response);
 }

}
```

在这个阶段，如果开发人员运行这个项目，即右键单击 **WEB1** 项目，选择上下文菜单`Run As/Run On Server`，查看 URL“[http://localhost:8080/web 1/process log in](http://localhost:8080/WEB1/ProcessLogin)”，这个人将看到如下所示的输出。

![](img/ddd426628869cc40cb86c51c78a98dc2.png)

类似地，如果开发人员使用 POSTMAN 发送 post 请求，应该会得到相同的输出，因为 doPost()方法包含一个调用 doGet()方法的语句，即`doGet(request, response);`。

![](img/a604a3ef06fb7883cb0b0fb141ebf432.png)

向 **doPost()** 方法添加代码。

```
package com.adabase.employee;

import java.io.IOException;
import java.net.HttpURLConnection;
import java.net.URL;
import java.util.Scanner;

import javax.servlet.ServletException;
import javax.servlet.annotation.WebServlet;
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;

/**
 * Servlet implementation class ProcessLogin
 */
@WebServlet(description = "send basic auth to backend", urlPatterns = { "/ProcessLogin" })
public class ProcessLogin extends HttpServlet {
 private static final long serialVersionUID = 1L;

    /**
     * @see HttpServlet#HttpServlet()
     */
    public ProcessLogin() {
        super();
        // TODO Auto-generated constructor stub
    }

 /**
  * @see HttpServlet#doGet(HttpServletRequest request, HttpServletResponse response)
  */
 protected void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
  // TODO Auto-generated method stub
  response.getWriter().append("Served at: ").append(request.getContextPath());
 }

 /**
  * @see HttpServlet#doPost(HttpServletRequest request, HttpServletResponse response)
  */
 protected void doPost(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
  // TODO Auto-generated method stub
  //doGet(request, response);
  HttpURLConnection connection = (HttpURLConnection) new URL("http://localhost:8190/login").openConnection();
  String strgBasicAuth = request.getHeader("Authorization"); 
  connection.setRequestProperty ("Authorization", strgBasicAuth);
  connection.setRequestMethod("POST");

  int responseCode = connection.getResponseCode();
  if(responseCode == 200){
   String strgResponse = "";
   Scanner scanner = new Scanner(connection.getInputStream());
   while(scanner.hasNextLine()){
    strgResponse += scanner.nextLine();
    strgResponse += "\n";
   }
   scanner.close();

   response.getWriter().append(strgResponse); 
 }
 }
}
```

保存代码。使用 POSTMAN 向 URL“[http://localhost:8080/web 1/process log in](http://localhost:8080/WEB1/ProcessLogin)”发送一个具有**基本授权**的 POST 请求。预期的输出如下。

![](img/ccf04781d6e3b417aabe2d1179a32014.png)

修改文件**login.html**，使 AJAX 请求中的 URL 指向上面的 URL，即`[http://localhost:8080/WEB1/ProcessLogin](http://localhost:8080/WEB1/ProcessLogin)`。

```
function reqsLogin(encodedUsrPwd){
 var request = $.ajax({
     url:      "http://localhost:8080/WEB1/ProcessLogin",
        contentType: "application/json; charset=utf-8",
        dataType: "json",
  method: "POST",
  headers: {
       authorization: "Basic "+encodedUsrPwd
     }, 
    cache:    false,     
    data: {},
  }); 
```

删除**会话存储**中的现有令牌。再次登录并检查**会话存储**是否收到新令牌。

![](img/ed1f61eddccbee7f5aec0c419091bd27.png)

# (2) Java 网络服务(SOAP)

右键单击 **WEB1** 项目，选择新建/其他…然后在“选择向导”窗口中选择**类**。

![](img/af8a9979853b5531384dc1076cfa0021.png)

输入包名，如`com.adabase.employee`和类名，如`LoginService`。单击下一步按钮。

![](img/4002792e86aecfe3ea4326029174c4b8.png)

添加代码。

```
package com.adabase.employee;

import java.io.IOException;
import java.net.HttpURLConnection;
import java.net.ProtocolException;
import java.net.URL;
import java.util.Scanner;

public class LoginService {
 public String loginService(String strgBasicAuth) {
  String strgResponse="";
  HttpURLConnection connection = null;

  try {
   connection = (HttpURLConnection) new URL("http://localhost:8190/login").openConnection();   
   connection.setRequestProperty ("Authorization", strgBasicAuth);
   connection.setRequestMethod("POST");
   int responseCode = connection.getResponseCode();
   if(responseCode == 200){
    Scanner scanner = new Scanner(connection.getInputStream());
    while(scanner.hasNextLine()){
     strgResponse += scanner.nextLine();
     strgResponse += "\n";
    }
    scanner.close();
    }   
  } catch (ProtocolException e) {
   // TODO Auto-generated catch block
   e.printStackTrace();
  } catch (IOException e) {
   // TODO Auto-generated catch block
   e.printStackTrace();
  }
  return strgResponse;
 }
}
```

创建 **Web 服务提供者 Java 类**。

右键单击 **WEB1** 项目，选择新建/其他…然后在“选择向导”窗口中选择 **Web 服务**。

![](img/c28cf92bc7517458bea079d0872519c3.png)

单击浏览类，然后搜索 **LoginService** 类。

![](img/f591b74ecdd6703a500bb9ccd9466ae7.png)

向上移动**测试服务**的滑动旋钮。勾选**发布 Web 服务**。单击下一步。

![](img/9a3facff6c20d8b84cadf635c9207221.png)

接受将创建 **WSDL** 文件的默认设置。WSDL 是一种 XML 格式，用于将网络服务描述为一组对包含面向文档或面向过程信息的消息进行操作的端点(参见 [W3C WSDL](https://www.w3.org/TR/2001/NOTE-wsdl-20010315) )。

![](img/600959b6e9ffd8e9fce35b9f08505590.png)

点击**启动**按钮测试服务。

![](img/c67a4d4b8e672b2ec2868a4fb4ac11d8.png)

网络浏览器将打开**网络服务浏览器**页面并显示表格。输入基本授权码，即`Basic YWRtaW46bWFuYWdl`，然后点击 Go 按钮。状态面板将在短时间内输出如下所示的字符串。

![](img/bc3264f0efa585e555867cd0a27feba0.png)

上面的 **Web 服务浏览器**提供了两个重要信息:
a)WSDL 文件的**物理位置**即
`file:/C:/Users/razzi/workspace1011ajax/WEB1/WebContent/wsdl/LoginService.wsdl`
b)Web 服务 **URL** 即`http://localhost:8080/WEB1/services/LoginService`

邮递员也可以用来发送 **SOAP 请求**。如下设置**割台参数**。

![](img/011da129adbc964a0096cff4007402a5.png)

然后，设置**主体(XML)** 内容如下。

![](img/65d1e22d938feea651f44c64dc73843f.png)

```
<soapenv:Envelope xmlns:soapenv="http://schemas.xmlsoap.org/soap/envelope/" xmlns:q0="http://jee.javapapers.com" xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance">
  <soapenv:Body>
<q0:loginService>
  <q0:strgBasicAuth>Basic YWRtaW46bWFuYWdl</q0:strgBasicAuth>
</q0:loginService>
  </soapenv:Body>
</soapenv:Envelope>
```

Web 服务将返回以下响应。

![](img/d49f2fd0671fa75c36ec928af656eacd.png)

```
<?xml version="1.0" encoding="UTF-8"?>
<soapenv:Envelope xmlns:soapenv="http://schemas.xmlsoap.org/soap/envelope/" xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance">
    <soapenv:Body>
        <loginServiceResponse >
            <ns1:loginServiceReturn xmlns:ns1="http://employee.adabase.com">{&quot;AdminRole&quot;:true,&quot;token&quot;:&quot;eyJraWQiOiIxIiwiYWxnIjoiUlMyNTYifQ.eyJzdWIiOiJhZGFyZXN0IiwiYXVkIjpbInJvbGUxIiwicm9sZTIiXSwiaXNzIjoiaHR0cHM6XC9cL3NvZnR3YXJlYWcuY29tIiwiVVVJRCI6IkNoZWNrVVVJRCIsImV4cCI6MTY2ODE3MTI5NiwiU2Vzc2lvbiI6IjY5NDU1MjdhYmMzMzA4YWM4NjQ2YTkzM2ZjZmE2ZmZmZmRmM2JmYmQifQ.kklDO_DMllFN1sEUb19LE2XzyPnNEC-3CT431VK4gDJ7bsZATTwOj3mKu4j9a-stpm5RDPxoCojjL8VggTgQEyN4kBCNzY0ZVfc6xr031ZKmcM-3HA6-IVvGxzAykWsmGmSgr58JpNu9KUhNoKMLf2ZpRe-NsfTKTRB4jqUlEkkGt84a748hmumnVGkp6nMWNmGliuueq5CT8fF8kA3KrzHft-twcSr9jyUAB8uT-RvU4-DgU1g1ks2HqScobAnC4HqB0OQlHdjtEQb1PI1HORC7mfziiizd_lv8dQGEisJzQcCyGnXX6uQvI5sZnnmBkyyhdqQRuvXlxn-ngqHgqk4Ohemkk8eEFO5nYn8eJFCPj4heVd8yXRxXoVt7BmeEyp0-ddhJJBTfkP2HdKY24FhspKNzPXxUq1mGrwhFtl3yKje3VjZBeEs4sianmX5EnZn3QG4jigCi6apItwtZ-vPJdqPTMfpksPgP3BUP8NpHv1-NZmEnLGbNMy16Lf_pBIeMTmwXhOpYMeIOEzoc92qh3Ocl7x5CuQoCaRgPR9XDKbGY1-fFX37MffhKpBXnJt0IRqOp5zSj1ODn8RTUwsHXhk7eVF9CGoOfPBfGxoKoNMYwySuqrh5L9uTVMSod9l_6hM7q8rLIGDV51f5GxLVIXpqqWXI7oS0HMHg1T4A&quot;}
</ns1:loginServiceReturn>
        </loginServiceResponse>
    </soapenv:Body>
</soapenv:Envelope>
```

生成的 WSDL 文件内容如下。该文件位于项目文件夹中，即`file:/C:/Users/razzi/workspace1011ajax/WEB1/WebContent/wsdl/LoginService.wsdl`。

```
<?xml version="1.0" encoding="UTF-8"?>
<wsdl:definitions targetNamespace="http://employee.adabase.com" xmlns:apachesoap="http://xml.apache.org/xml-soap" xmlns:impl="http://employee.adabase.com" xmlns:intf="http://employee.adabase.com" xmlns:wsdl="http://schemas.xmlsoap.org/wsdl/" xmlns:wsdlsoap="http://schemas.xmlsoap.org/wsdl/soap/" xmlns:xsd="http://www.w3.org/2001/XMLSchema">
<!--WSDL created by Apache Axis version: 1.4
Built on Apr 22, 2006 (06:55:48 PDT)-->
 <wsdl:types>
  <schema elementFormDefault="qualified" targetNamespace="http://employee.adabase.com" >
   <element name="loginService">
    <complexType>
     <sequence>
      <element name="strgBasicAuth" type="xsd:string"/>
     </sequence>
    </complexType>
   </element>
   <element name="loginServiceResponse">
    <complexType>
     <sequence>
      <element name="loginServiceReturn" type="xsd:string"/>
     </sequence>
    </complexType>
   </element>
  </schema>
 </wsdl:types>
   <wsdl:message name="loginServiceRequest">
      <wsdl:part element="impl:loginService" name="parameters">
      </wsdl:part>
   </wsdl:message>
   <wsdl:message name="loginServiceResponse">
      <wsdl:part element="impl:loginServiceResponse" name="parameters">
      </wsdl:part>
   </wsdl:message>
   <wsdl:portType name="LoginService">
      <wsdl:operation name="loginService">
         <wsdl:input message="impl:loginServiceRequest" name="loginServiceRequest">
       </wsdl:input>
         <wsdl:output message="impl:loginServiceResponse" name="loginServiceResponse">
       </wsdl:output>
      </wsdl:operation>
   </wsdl:portType>
   <wsdl:binding name="LoginServiceSoapBinding" type="impl:LoginService">
      <wsdlsoap:binding style="document" transport="http://schemas.xmlsoap.org/soap/http"/>
      <wsdl:operation name="loginService">
         <wsdlsoap:operation soapAction=""/>
         <wsdl:input name="loginServiceRequest">
            <wsdlsoap:body use="literal"/>
         </wsdl:input>
         <wsdl:output name="loginServiceResponse">
            <wsdlsoap:body use="literal"/>
         </wsdl:output>
      </wsdl:operation>
   </wsdl:binding>
   <wsdl:service name="LoginServiceService">
      <wsdl:port binding="impl:LoginServiceSoapBinding" name="LoginService">
         <wsdlsoap:address location="http://localhost:8080/WEB1/services/LoginService"/>
      </wsdl:port>
   </wsdl:service>
</wsdl:definitions>
```

据观察，Java Servlets 和 Java Web 服务在某种程度上是相互关联的，因为它们都在 **Web 环境**中运行。SOAP 在消息内容结构方面非常严格。然而，Servlet 或 REST 风格的 Web 服务更加灵活。它们涉及更少的配置，这意味着开发会相对更快。但是对于那些使用遗留系统的开发人员来说，了解 REST 和 SOAP 是必不可少的。

本帖是“Adabas & Natural 入门”系列的一部分，该系列包括:
(1) [设置 Adabas & Natural 社区版(Docker 版)](https://medium.com/@mohamad.razzi.my/getting-started-with-adabas-natural-part-1-6597688406ad)。
(2) [通过 Adabas REST Web app 访问 Adabas 数据库](https://medium.com/@mohamad.razzi.my/getting-started-with-adabas-natural-part-2-34621e576fa4)。
(3) [Adabas“周期组”和 JSON 数据格式的“多值”表示](/getting-started-with-adabas-natural-part-3-a334822db12)。
(4) [使用 Adabas TCP-IP 节点包访问 Adabas 数据库](https://medium.com/@mohamad.razzi.my/getting-started-with-adabas-natural-part-4-728e6977ad4f)。
(5) [使用 NaturalONE IDE 的大型机编程(Natural)介绍](https://medium.com/@mohamad.razzi.my/getting-started-with-adabas-natural-part-5-1665a0be42ab)。
(6) [使用自然编程和自然 IDE 访问 Adabas 数据库](https://medium.com/@mohamad.razzi.my/getting-started-with-adabas-natural-part-6-48b4b2fd3e6d)。
(7) [大型机数据库编程基础](https://medium.com/@mohamad.razzi.my/mainframe-database-programming-fundamentals-b34fd88acf6e)。
(8) [大型机数据库编程中级](https://medium.com/@mohamad.razzi.my/mainframe-database-programming-27803b92a3a3)。
(9) [使用自然 AJAX 框架开发 AJAX 网页](https://medium.com/@mohamad.razzi.my/developing-ajax-web-pages-e270eb59fc92)。
(10)[AJAX 实际上是如何工作的](https://medium.com/@mohamad.razzi.my/how-does-ajax-actually-work-2f57cf4ddc55)？
(11) [向 ADABAS REST Web app 发送带有 JWT 令牌的 AJAX 请求](https://medium.com/dev-genius/how-does-ajax-actually-work-2f57cf4ddc55)。
(12) [通过 Java Servlets 和 Java Web Services (SOAP)向 ADABAS REST 服务发送 HTTP 请求](https://medium.com/@mohamad.razzi.my/creating-soap-service-d8264ed1f3b7)。