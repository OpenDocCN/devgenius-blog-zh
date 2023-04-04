# 带 MongoDB 和 JWT 认证的 Flask

> 原文：<https://blog.devgenius.io/flask-with-mongodb-and-jwt-authentication-5d9ba2c3ef7e?source=collection_archive---------3----------------------->

# 带有 MongoDB 和 JWT 认证的 Flask。

在这个任务中，我尝试使用 Flask 和 mongo DB 制作带有 JWT 令牌认证的软件。想法是注册一个用户，一个用户可以管理自己的模板(选择，更新，删除，插入)。

# 端点

1.  注册用户
2.  登录用户(JWT)
3.  插入模板
4.  选择所有模板
5.  更新模板
6.  删除模板

# 代码部分:

导入所有必需的模块

```
from flask import Flask
from pymongo import MongoClient
import hashlib
from flask import Flask, request, jsonify
from flask_jwt_extended import JWTManager, create_access_token, get_jwt_identity, jwt_required
import datetime
import hashlib
import urllib
```

设置 JWT 身份验证

```
app = Flask(__name__)
jwt = JWTManager(app) # initialize JWTManager
app.config['JWT_SECRET_KEY'] = '38dd56f56d405e02ec0ba4be4607eaab'
app.config['JWT_ACCESS_TOKEN_EXPIRES'] = datetime.timedelta(days=1) # define the life span of the token
```

JWT 的秘密可以通过秘密图书馆获得。

1.  去终点站
2.  键入以下几行代码

```
>>> import secrets
>>> secrets.token_hex(16)
'38dd56f56d405e02ec0ba4be4607eaab'
```

设置 Mongo 数据库，我在 mongodb 上在线创建了这个数据库。点击此链接创建在线 mongo db 数据库[https://www . mongo db . com/docs/atlas/online-archive/connect-to-online-archive/](https://www.mongodb.com/docs/atlas/online-archive/connect-to-online-archive/)

```
client = MongoClient("mongodb+srv://Sohaib:sohaib@cluster0.ljdhvgj.mongodb.net/?retryWrites=true&w=majority")
db = client["mydatabase"]
users_collection = db["users"]
templates_collection = db["templates"]
```

简单的 Hello world 路线，检查是否一切正常。

```
@app.route('/')
def hello_world():
	return 'Hello, World!'
```

完成后，进入终端并启动应用程序。我将这段代码写在 main.py 文件中。请在启动应用程序之前安装这些要求。`pip install -r requirements.txt`

```
python main.py
```

# 注册 API

现在让我们创建一个注册用户的端点。

```
@app.route("/api/v1/users", methods=["POST"])
def register():
    new_user = request.get_json() # store the json body request
    # Creating Hash of password to store in the database
    new_user["password"] = hashlib.sha256(new_user["password"].encode("utf-8")).hexdigest() # encrpt password
    # Checking if user already exists
    doc = users_collection.find_one({"username": new_user["username"]}) # check if user exist
    # If not exists than create one
    if not doc:
        # Creating user
        users_collection.insert_one(new_user)
        return jsonify({'msg': 'User created successfully'}), 201
    else:
        return jsonify({'msg': 'Username already exists'}), 409
```

要检查此端点，请转到终端启动应用程序。在那之后打开你的邮差像这样打网址。

```
[http://localhost:5000/api/v1/users](http://localhost:5000/api/v1/users)
```

端点的主体

```
{
  "username":"sohaib123", 
  "password":"sohaib123"
}
```

端点响应

```
{
  "msg": "User created successfully"
}
```

![](img/14f41154c92e191e4469f372cd0cf2bb.png)

# 登录用户

该端点用于用户登录

```
@app.route("/api/v1/login", methods=["post"])
def login():
    # Getting the login Details from payload
    login_details = request.get_json() # store the json body request
    # Checking if user exists in database or not
    user_from_db = users_collection.find_one({'username': login_details['username']})  # search for user in database
    # If user exists
    if user_from_db:
        # Check if password is correct
        encrpted_password = hashlib.sha256(login_details['password'].encode("utf-8")).hexdigest()
        if encrpted_password == user_from_db['password']:
            # Create JWT Access Token
            access_token = create_access_token(identity=user_from_db['username']) # create jwt token
            # Return Token
            return jsonify(access_token=access_token), 200 return jsonify({'msg': 'The username or password is incorrect'}), 401[http://localhost:5000/api/v1/login](http://localhost:5000/api/v1/login)
```

端点的主体

```
{
  "username":"sohaib123", 
  "password":"sohaib123"
}
```

端点响应

```
{
  "access_token": "eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJmcmVzaCI6ZmFsc2UsImlhdCI6MTY2Njc4MDg2MywianRpIjoiNTM2Zjg2MTQtODllZi00YWI1LWIxNDgtNmYxOTAxYTYyMDY4IiwidHlwZSI6ImFjY2VzcyIsInN1YiI6InNvaGFpYjEyMyIsIm5iZiI6MTY2Njc4MDg2MywiZXhwIjoxNjY2ODY3MjYzfQ.IRsLXY2hugjJkmtpHi79X8JgueXUmDIzYC7DlYIQD_A"
}
```

![](img/9c0d8d5e466348443ee4677bd31eee8b.png)

# 创建模板

创建模板这里的模板只是一个演示对象，你可以创建书、汽车、自行车等`@jwt_required()`这一行帮助我们验证访问令牌是否有效

```
@app.route("/create", methods=["POST"])
@jwt_required()
def create_template():
    """Creating the template with respect to the user Returns:
        dict: Return the profile and template created
    """
    # Getting the user from access token
    current_user = get_jwt_identity() # Get the identity of the current user
    user_from_db = users_collection.find_one({'username' : current_user})

    # Checking if user exists
    if user_from_db:
        # Getting the template details from json
        template_details = request.get_json() # store the json body request
        # Viewing if templated already present in collection
        user_template = {'profile' : user_from_db["username"],  "template": template_details["template"]}
        doc = templates_collection.find_one(user_template) # check if user exist
        # Creating collection if not exists

        if not doc:
            templates_collection.insert_one(user_template)
            print("user_template ", user_template)
            return jsonify({'msg': 'Template created successfully'}), 200
        # Returning message if template exists
        else: return jsonify({'msg': 'Template already exists on your profile'}), 404
    else:
        return jsonify({'msg': 'Access Token Expired'}), 404[http://localhost:5000/create](http://localhost:5000/create)
```

端点的主体

```
{
  "template":"My First Template"
}
```

端点响应

```
{
  "msg": "Template created successfully"
}
```

在 postman 的 auth 选项中给出访问令牌

```
eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJmcmVzaCI6ZmFsc2UsImlhdCI6MTY2Njc4MDg2MywianRpIjoiNTM2Zjg2MTQtODllZi00YWI1LWIxNDgtNmYxOTAxYTYyMDY4IiwidHlwZSI6ImFjY2VzcyIsInN1YiI6InNvaGFpYjEyMyIsIm5iZiI6MTY2Njc4MDg2MywiZXhwIjoxNjY2ODY3MjYzfQ.IRsLXY2hugjJkmtpHi79X8JgueXUmDIzYC7DlYIQD_A
```

访问令牌

![](img/5767978ed28f2001f373768e90b6647c.png)

创建端点

![](img/928fb497ebd38285b3c15765b4d7dbdf.png)

# 获取所有模板

获取特定用户的所有模板`@jwt_required()`这一行帮助我们验证访问令牌是否有效

```
@app.route("/get", methods=["GET"])
@jwt_required()
def get_template():
    """Get the templates of specefic user Returns:
        dict: Return the profile and template 
    """
    # Getting the user from access token
    current_user = get_jwt_identity() # Get the identity of the current user
    user_from_db = users_collection.find_one({'username' : current_user})
    # Checking if user exists
    if user_from_db:
        # Viewing if templated already present in collection
        user_template = {'profile' : user_from_db["username"]}
        return jsonify({"docs":list(db.templates.find(user_template, {"_id":0}))}), 200
    else:
        return jsonify({'msg': 'Access Token Expired'}), 404[http://localhost:5000/get](http://localhost:5000/get)
```

端点响应

```
{
  "docs": [
    {
      "profile": "sohaib123",
      "template": "My First Template"
    }
  ]
}
```

在 postman 的 auth 选项中给出访问令牌

```
eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJmcmVzaCI6ZmFsc2UsImlhdCI6MTY2Njc4MDg2MywianRpIjoiNTM2Zjg2MTQtODllZi00YWI1LWIxNDgtNmYxOTAxYTYyMDY4IiwidHlwZSI6ImFjY2VzcyIsInN1YiI6InNvaGFpYjEyMyIsIm5iZiI6MTY2Njc4MDg2MywiZXhwIjoxNjY2ODY3MjYzfQ.IRsLXY2hugjJkmtpHi79X8JgueXUmDIzYC7DlYIQD_A
```

访问令牌

![](img/cc71427f214e4ea18141d45a76d32ebb.png)

创建端点

![](img/c0446fc3122ab6d98c88bb778684f860.png)

# 更新模板

更新模板`@jwt_required()`这一行帮助我们验证访问令牌是否有效

```
@app.route("/update", methods=["POST"])
@jwt_required()
def update_template():
    """Updating the template with respect to the user Returns:
        dict: Return the profile and template created
    """
    # Getting the user from access token
    current_user = get_jwt_identity() # Get the identity of the current user
    user_from_db = users_collection.find_one({'username' : current_user})

    # Checking if user exists
    if user_from_db:
        # Getting the template details from json
        template_details = request.get_json() # store the json body request
        # Viewing if templated already present in collection
        user_template = {'profile' : user_from_db["username"],  "template": template_details["old_template"]}
        doc = templates_collection.find_one(user_template) # check if user exist
        # Updating collection if not exists

        if doc:
            doc["template"] = template_details["new_template"]
            templates_collection.update_one(user_template, {"$set": {"template":doc["template"]}}, upsert=False)
            return jsonify({'msg': 'Template Updated successfully'}), 200
        # Returning message if template exists
        else: return jsonify({'msg': 'Template not exists on your profile'}), 404
    else:
        return jsonify({'msg': 'Access Token Expired'}), 404[http://localhost:5000/update](http://localhost:5000/update)
```

端点的主体

```
{
  "old_template":"My First Template",
  "new_template":"My First Updated Template"
}
```

端点响应

```
{
  "msg": "Template Updated successfully"
}
```

在 postman 的 auth 选项中给出访问令牌

```
eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJmcmVzaCI6ZmFsc2UsImlhdCI6MTY2Njc4MDg2MywianRpIjoiNTM2Zjg2MTQtODllZi00YWI1LWIxNDgtNmYxOTAxYTYyMDY4IiwidHlwZSI6ImFjY2VzcyIsInN1YiI6InNvaGFpYjEyMyIsIm5iZiI6MTY2Njc4MDg2MywiZXhwIjoxNjY2ODY3MjYzfQ.IRsLXY2hugjJkmtpHi79X8JgueXUmDIzYC7DlYIQD_A
```

访问令牌

![](img/69b2a6dc239651b962e004c61b7d2556.png)

更新终点

![](img/2c8794cebddfc683b41d101e7492fcfb.png)

检查数据库中是否有更新

![](img/639821c7b8e014bcc83bf44215d826d0.png)

# 删除模板

更新模板`@jwt_required()`这一行帮助我们验证访问令牌是否有效

```
@app.route("/delete", methods=["POST"])
@jwt_required()
def delete_template():
    """Creating the template with respect to the user Returns:
        dict: Return the profile and template created
    """
    # Getting the user from access token
    current_user = get_jwt_identity() # Get the identity of the current user
    user_from_db = users_collection.find_one({'username' : current_user})

    # Checking if user exists
    if user_from_db:
        # Getting the template details from json
        template_details = request.get_json() # store the json body request
        # Viewing if templated already present in collection
        user_template = {'profile' : user_from_db["username"],  "template": template_details["template"]}
        doc = templates_collection.find_one(user_template) # check if user exist
        # Creating collection if not exists

        if doc:
            templates_collection.delete_one(user_template)
            print("user_template ", user_template)
            return jsonify({'msg': 'Template Deleted Sucessfully'}), 404
        # Returning message if template exists
        else: return jsonify({'msg': 'Template not exists on your profile'}), 404
    else:
        return jsonify({'msg': 'Access Token Expired'}), 404[http://localhost:5000/delete](http://localhost:5000/delete)
```

端点的主体

```
{
  "template":"My First Updated Template"
}
```

端点响应

```
{
  "msg": "Template Deleted Sucessfully"
}
```

在 postman 的 auth 选项中给出访问令牌

```
eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJmcmVzaCI6ZmFsc2UsImlhdCI6MTY2Njc4MDg2MywianRpIjoiNTM2Zjg2MTQtODllZi00YWI1LWIxNDgtNmYxOTAxYTYyMDY4IiwidHlwZSI6ImFjY2VzcyIsInN1YiI6InNvaGFpYjEyMyIsIm5iZiI6MTY2Njc4MDg2MywiZXhwIjoxNjY2ODY3MjYzfQ.IRsLXY2hugjJkmtpHi79X8JgueXUmDIzYC7DlYIQD_A
```

访问令牌

![](img/c69f50d9c14506e6b0d9d7924abec8ec.png)

更新终点

![](img/b2af62c0b727af27b12574d43731fbd9.png)

检查是否从数据库中删除

![](img/20967d3b955703686d91ca7342f33594.png)

这就是我们如何在 flask，MongoDB 的帮助下制作一个 JWT 认证软件。希望你喜欢我的博客。

# 测试案例

现在让我们为端点编写测试用例。我写了 3 个测试用例，

1.  主要终点
2.  登记
3.  注册

```
import json
from flask import Flask
from main import app def test_flask():
    """
    GIVEN a Flask application configured for testing
    WHEN the '/' page is requested (GET)
    THEN check that the response is valid
    """
    flask_app =  app
 # Create a test client using the Flask application configured for testing
    with flask_app.test_client() as test_client:
        response = test_client.get('/')
        assert response.status_code == 200

def test_registration():
    flask_app =  app # Create a test client using the Flask application configured for testing
    with flask_app.test_client() as test_client:
        data = {
            "username":"test1",
            "password":"1234"
        }
        response = test_client.post('/api/v1/users', data=json.dumps(data), headers={"Content-Type": "application/json"})
        assert response.status_code == 201 or response.status_code == 409def test_login():
    flask_app =  app
 # Create a test client using the Flask application configured for testing
    with flask_app.test_client() as test_client:
        data = {
            "username":"test1",
            "password":"1234"
        }
        response = test_client.post('/api/v1/login', data=json.dumps(data), headers={"Content-Type": "application/json"})
        assert response.status_code == 200
```

现在去终端写，`pytest`

![](img/2a17e12e236a508ce7cb375c800ca7f4.png)

# 作者

*   索海卜·安瓦尔
*   Gmail:[sohaibanwaar36@gmail.com](mailto:sohaibanwaar36@gmail.com)
*   LinkedIn : [这里有一些专业的谈话](https://www.linkedin.com/in/sohaib-anwaar-4b7ba1187/)
*   堆栈溢出:[在这里获得我的帮助](https://stackoverflow.com/users/7959545/sohaib-anwaar)
*   在这里查看我的杰作