# 管理脚本参数-FileMaker

> 原文：<https://blog.devgenius.io/managing-script-parameters-filemaker-ef2e586255c1?source=collection_archive---------10----------------------->

![](img/a4cac22e7cf6e191bb51bd86ad90d15f.png)

可以通过多种方式调用 FileMaker 脚本——用户单击按钮、事件触发脚本、脚本调用另一个脚本等。大多数情况下，当调用脚本时，开发人员可以提供一个脚本参数来传递给脚本。使用 *Get ( ScriptParameter )* 可在脚本运行过程中随时检索该参数。

## 命名参数

很多时候，脚本参数用于控制脚本的哪个部分将运行。例如，一个脚本可以处理一个流程的所有部分，如*打开*、*保存*和*取消*。因此，调用脚本时会将这些字符串中的一个作为参数。

脚本的一般结构是:

```
If [ Get (ScriptParameter) = "open" ]
   #do something here
Else If [ Get (ScriptParameter) = "save" ]
   #do something here
Else If [ Get (ScriptParameter) = "cancel" ]
   #do something here
Else
   Show Custom Dialog [ "Error" ; "Invalid or missing parameter" ]
End If
```

这就产生了一个小的可读性问题——每个 If 语句并不直接直观，因为读者需要问“脚本参数是什么？”。

一个简单的解决方案是将您的脚本参数“解包”到一个变量中，该变量的命名要有意义。所以我们可以在脚本的开头添加一个脚本步骤。看到脚本的其余部分可读性如何了吗？

```
Set Variable [ $action; value: Get (ScriptParameter) ]

If [ $action = "open" ]
   #do something here
Else If [ $action = "save" ]
   #do something here
Else If [ $action = "cancel" ]
   #do something here
Else
   Show Custom Dialog [ "Error" ; "Invalid or missing parameter" ]
End If
```

## 不止一个参数呢？

脚本参数的限制之一是只有一个文本字符串。一旦开始使用脚本参数，您很快就会发现需要多个参数。多年来，FileMaker 开发人员已经使用了许多不同的方法来在单个参数中传递多条数据。

例如，可以使用标准分隔符传递三段数据:

```
David | Head | david@ulearnit.com.au
```

当从字段传递数据时，将其作为**列表**发送是有用的:

```
List ( first_name ; last_name ; email )
```

然后，可以将这个参数解析成三个单独的字符串。这样做时，将值直接解析成变量以备后用是最有用的。这很容易用列表示例来完成:

```
Set Variable [ $first ; GetValue ( Get (ScriptParameter); 1 ) ]
Set Variable [ $last ; GetValue ( Get (ScriptParameter); 2 ) ]
Set Variable [ $email ; GetValue ( Get (ScriptParameter); 3 ) ]
```

List 函数的优点(和缺点)之一是，如果值为空，将不会返回任何值，甚至不会返回空值。

在上面的例子中，如果 last_name 字段碰巧为空，那么脚本参数将只包含两个值。这意味着变量$last 将包含电子邮件地址，而变量$email 将为空(因此不存在)。

## JSON 来拯救

传递多个参数的常见方法是创建 **JSON** 代码。现在 FileMaker 中有了原生的 JSON 函数，这就容易多了。脚本参数设置为:

```
JSONSetElement ( "" ; 
     ["first"; person::first_name; JSONstring ];
     ["last"; person::last_name; JSONstring ];
     ["email"; person::email; JSONstring ]
  )
```

这将创建一个如下所示的字符串:

```
{"email":"david@ulearnit.com.au","first":"David","last":"Head"}
```

这可以分解成这样的变量:

```
Set Variable [ $json ; Get (ScriptParameter) ]
Set Variable [ $first ; JSONGetElement ( $json; "first" ) ]
Set Variable [ $last ; JSONGetElement ( $json; "last" ) ]
Set Variable [ $email ; JSONGetElement ( $json; "email" ) ]
```

同样，您最终会得到包含每条数据的命名变量。如果发生某些数据不存在的情况，变量仍然会被正确设置。

## 请参见:Get ( ScriptResult)

当通过**退出脚本**步骤向调用脚本返回数据时，这些技术同样适用。该数据可通过 *Get ( ScriptResult )* 函数访问。

## 结论

一旦开始使用脚本参数，您将希望采用一种标准方法来传递和处理来自 Get ( ScriptParameter)的数据。变量是可读性和重用性的好朋友。