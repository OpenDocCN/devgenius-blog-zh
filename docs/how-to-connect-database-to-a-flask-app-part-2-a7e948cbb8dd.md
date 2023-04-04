# 如何将数据库连接到 Flask 应用程序——第 2 部分

> 原文：<https://blog.devgenius.io/how-to-connect-database-to-a-flask-app-part-2-a7e948cbb8dd?source=collection_archive---------5----------------------->

## 通过 SQLAlchemy |集成 Flask WTForms 将 SQLite 数据库连接到 Flask 应用程序

![](img/d137848d57193b941b8fd0ad782ce121.png)

埃米尔·佩龙在 [Unsplash](https://unsplash.com/s/photos/code?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 上的照片

# 介绍

这是本系列的第 2 部分，在本文中，我将从第 1 部分停止的地方完成其余的验证。如果你错过了前面的部分，请在继续之前，点击下面的链接并参考它。

[](/how-to-connect-database-to-a-flask-app-part-1-60611deea17a) [## 如何将数据库连接到 Flask 应用程序——第 1 部分

### 通过 SQLAlchemy 将 SQLite 数据库连接到 Flask 应用程序

blog.devgenius.io](/how-to-connect-database-to-a-flask-app-part-1-60611deea17a) 

正如我在上一篇文章中提到的，我将安装 **Flask WTForms** 并创建单独的`forms`和`models`配置文件。但在此之前，我将在应用程序的根目录中创建一个与根目录同名的新目录。

```
root directory -> Flask_DBdirectory inside -> Flask_DB
```

# 将现有文件移动到新目录

我们需要将现有的`statis`、`templates`目录和`sqlite DB`目录移动到新创建的`Flask_DB`目录中。

之后，我们需要在这个目录中创建 3 个 python 配置文件，如`forms.py`、`models.py`和`routes.py`。

然后，我们在目录中创建另一个名为`__init__.py`的 python 配置文件。现在我们已经将所有需要的配置文件添加到目录中，当您运行`tree`命令时，它应该看起来像这样。

```
.
├── __init__.py
├── db.sqlite3
├── forms.py
├── models.py
├── routes.py
├── static
└── templates
    ├── base.html
    ├── home.html
    ├── login.html
    └── register.html
```

但是我们的`app.py`配置文件仍然在根目录中，我们需要将其重命名为`run.py`。现在我们几乎完成了配置文件的移动和重命名。因此，我们可以开始更新现有的文件，以完成我们的应用程序的验证部分。

# 更新`run.py file`

这里，我们需要将这个文件设置为起点，并将其他配置移动到我们新创建的配置文件中。因此，新的`run.py`文件如下所示。

[如果您在应用程序中设置了一个`.flaskenv`文件，它也需要修改如下]

```
FLASK_ENV=development
FLASK_APP=run.py
```

# 正在更新 __init__。py 文件

新的`__init__.py`配置文件如下所示，这里我们已经将所有的`models`和`routes`移动到不同的配置文件中，其余的将驻留在这里。

# 更新 models.py 文件

之前，我们在`app.py`配置文件中创建了用户模型。我们可以直接将该部分移动到`models.py`配置文件中，并导入必要的包，如下面的代码片段所示。

# 安装烧瓶 WTForms

使用 Flask WTForms，我们可以很容易地创建表单并在`HTML`配置文件中显示它们。之前，我们使用通用的`HTML`元素和`Bootstrap`配置了我们的`registration`和`login`页面。但是当您安装 Flask WTForms 时，您可以非常容易地生成它们。

因此，首先，您需要在您的虚拟环境中安装 Flask WTForms，然后我们可以开始在`forms.py`文件中创建我们自己的表单。要安装 Flask WTForms，请使用下面的命令。

```
pip install Flask-WTF 
```

然后将下面的代码添加到您的`forms.py`配置文件中。

现在我们已经成功创建了表单，接下来我们需要根据表单修改`register.html`和`login.html`文件。使用下面的代码片段*【你可以通过* [*这个网站*](https://flask-wtf.readthedocs.io/en/1.0.x/) *找到更多关于 Flask WTForms 的信息】。*

# 添加验证

在我们的 Flask 应用程序中，我们需要确保一个**用户名或一个**电子邮件不能在两个用户之间重复。简单地说，用户名和电子邮件是唯一的字段，在数据库中不能重复。

为了确保这一点，我们可以在`forms.py`文件中应用以下函数来验证这两个条件，如下所示。

```
def validate_username(self, username):
  user = User.query.filter_by(username=username.data).first()
  if user:
    raise ValidationError('That username is taken. Please choose a different one.') def validate_email(self, email):
  user = User.query.filter_by(email=email.data).first()
  if user:
    raise ValidationError('That email is taken. Please choose a different one.')
```

因此，完成的`forms.py`文件如下所示。

# 更新 routes.py 文件

最后，我们来到了我们的`routes.py`文件，这是一个配置文件，其中包含了我们在应用程序中的所有路线。该文件包含与之前相同的路线，但是我们需要修改该文件中的`login`和`register`路线。

让我们先创建我们的`register`路线。

```
@app.route('/register', methods=['GET', 'POST'])
def register():
  form = RegistrationForm()
  if form.validate_on_submit():
    username = form.username.data
    email = form.email.data
    password = form.password.data new_user = User(username=username, email=email, password=password) db.session.add(new_user)
    db.session.commit() flash('Successfully Registered!, You are now able to log in', 'success') 
    return redirect(url_for('login'))return render_template('register.html', title='Register', form=form)
```

然后我们可以开始修改`login`路线

```
@app.route('/login', methods=['GET', 'POST'])
def login():
  form = LoginForm() if form.validate_on_submit():
    user = User.query.filter_by(email=form.email.data).first()
    if user and user.password == form.password.data:
      return redirect(url_for('home'))
    else: 
      flash('Login Unsuccessful. Please check username and password', 'danger')return render_template('login.html', title='Login', form=form)
```

因此，我们完成的`routes.py`文件如下所示。

您已成功完成剩余的验证，您的数据库和路线将正常工作。

# 参考

*   [烧瓶 WTForms](https://flask-wtf.readthedocs.io/en/1.0.x/)
*   [如何通过 Flask-WTF 使用和验证 Web 表单](https://www.digitalocean.com/community/tutorials/how-to-use-and-validate-web-forms-with-flask-wtf)

**感谢您的阅读！**

**快乐编码！**👨🏻‍💻