# 编写一个简单的 RBAC API (Python)

> 原文：<https://blog.devgenius.io/writing-a-simple-rbac-api-python-56db9283a1af?source=collection_archive---------7----------------------->

使用 Flask 在 Python 中编写简单的基于角色的访问控制 API

![](img/063b3fd89361ede734cd9bb0110599de.png)

在这个演示中，我们将使用 Flask 用 Python 编写一个 API 服务。我们将使用 JSON 来存储用户信息；尽管人们也可以使用数据库。我们将首先编写一个不需要身份验证的简单端点。然后，我们将编写一个要求用户使用基本身份验证来调用端点的脚本。然后，我们将编写一个只允许选择用户。我们将通过为用户定义角色来控制这一点，“父”和“子”，其中“父”比“子”拥有更好的权限。

**1。设计 API 概要文件**。以下是我如何设计我的。对于每个用户，我们有一个“用户 Id”、“用户名”、“密码哈希”、“角色”。稍后，我们定义一个动作字典，它显示哪些角色可以访问我们想要在受限访问下保护的每个端点。

```
{
   "Users":[
      {
         "UserId":"roy34",
         "UserName":"Roy",
         "PasswordHash":"$pbkdf2-sha256$29000$FALAeA.BsBZi7J2Tcq5Vyg$hMLsWa1lFOPVd4IJgTz6Ddef6b1n7YFWJIE5LP/I/wY",
         "Role":"Parent"
      },
      {
         "UserId":"eva41",
         "UserName":"Eva",
         "PasswordHash":"$pbkdf2-sha256$29000$/H8P4Ryj9B7DmNM6J.T8vw$grSInYIXaAOX7JN9DxoMidEjIDSDBFPSScmfNv5mUcE",
         "Role":"Child"
      }
   ],
   "Actions":{
      "getUsers":[
         "Parent"
      ],
      "getAppliance":[
         "Parent",
         "Child"
      ]
   }
}
```

**2。设置 flask 应用程序。我为这个项目制作了一个新的[虚拟人](https://docs.python.org/3/tutorial/venv.html)。这使得满足应用需求变得容易。(使用 pip 安装烧瓶)**

```
import flaskapp = flask.Flask(__name__)
app.config["DEBUG"] = True #Shows errors in response.@app.route('/', methods=['GET'])
def home():
    return "<h1>Welcome to the Home Controller</h1><p>Here is skeleton controller with RBAC to control your home</p>"app.run()
```

**3。需要基本身份验证才能访问端点。**我们使用模块[**python _ JSON _ config**](https://pypi.org/project/python-json-config/)**来读取概要文件。这是我最喜欢的图书馆之一。它将 JSON 作为对象加载，并将值作为其成员。Flask 的 HTTPBasicAuth 可用于在端点上强制 Auth。为了提高安全性，我们使用 sha256 哈希算法进行密码验证，而不是将密码存储为纯文本。我们使用 **passlib.hash** 来实现这一点，它允许你散列和验证一个给定的密码。 **flask_httpauth** 允许您编写自己版本的 verify_password(用户名，密码),它应该返回一个 bool 来控制对端点的验证。**

```
from flask_httpauth import HTTPBasicAuth
from python_json_config import ConfigBuilder
from passlib.hash import pbkdf2_sha256auth = HTTPBasicAuth() #will hold values provided for authbuilder = ConfigBuilder()
apiProfile = builder.parse_config('controller-profile.json')@app.route('/api/v1/users', methods=['GET'])
@auth.login_required
def api_users():
    return str(apiProfile.Users)@auth.verify_password
def verify_password(username, password):
    user = getProfile(apiProfile.Users, username)
    if(user == None):
        return False
    return(pbkdf2_sha256.verify(password, user["PasswordHash"]))def getProfile(Users, username):
    for user in Users:
        if(username == user["UserId"]):
            return user
```

****4。要求对某些端点进行授权。**这里我们定义一个端点来添加一个新用户。我们不希望每个人都能够添加新用户，只有少数经过授权的人才能添加。因此，我们定义了一个函数 authorizeUserforAction，该函数可用于验证发出调用的用户是否具有能够发出该调用的权限。**

```
from flask import request, jsonify, abort@app.route('/api/v1/user', methods=['POST'])
@auth.login_required
def api_add_user():
    authorized = authorizeUserforAction(username=auth.username(), roles = apiProfile.Actions.addUsers)
    if not authorized:
        abort(403)
    payload = request.get_json()
    try:
        payload["PasswordHash"] = pbkdf2_sha256.hash(payload["Password"])
        del payload["Password"]
    except:
        abort(400,"No Password present")
    user = getProfile(apiProfile.Users, payload["UserId"])
    if(user != None): abort(400, "User already exists, pick a different userID")
    apiProfile.Users.append(payload)
    configFile = open("controller-profile.json", "w")
    configFile.write(apiProfile.to_json())
    return "User created"def authorizeUserforAction(username, roles):
    user = getProfile(apiProfile.Users, username)
    if user["Role"] in roles:
        return True
    return False
```