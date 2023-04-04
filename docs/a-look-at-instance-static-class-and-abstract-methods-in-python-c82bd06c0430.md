# Python 中的实例、静态、类和抽象方法

> 原文：<https://blog.devgenius.io/a-look-at-instance-static-class-and-abstract-methods-in-python-c82bd06c0430?source=collection_archive---------7----------------------->

![](img/549f839d0f180769b22f1b6391b0ea2b.png)

迈克尔·泽兹奇在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上的照片

面向对象编程，尽管受到许多人的严厉批评，仍然是许多软件开发领域的主导范式。

Python 之所以简洁，是因为您可以用过程化、函数化或面向对象的方式编写代码。然而，为了更好的可读性和可维护性，我建议尽可能使用 OOP。

即使您正在使用过程化或函数式风格编写代码，强烈建议您至少熟悉 OOP 最佳实践，因为您将使用的许多第三方框架和库将以 OOP 方式编写。能够阅读和理解这些代码将会给你很大的帮助。

在本文中，我将研究 Python 类可以包含的各种类型的方法，以及何时使用它们是一个好主意。

# 实例方法

实例方法是最常见的类型，这是有道理的。当从一个类中实例化一个对象时，您定义了这个对象将拥有的一些属性，也称为对象的*状态*。实例方法旨在访问和修改这种状态。虽然在 Python 中，我们不像在 Java 中那样显式声明对象的私有和公共属性，但是只通过您定义的实例方法来访问和修改您的实例属性是一个很好的实践。

您可以将实例方法与静态、类和抽象方法区分开来，因为实例方法是唯一不要求在其签名上使用装饰器的方法(我们将在代码示例中看到)。它们还包含关键字 *self* 作为第一个参数(相当于 Java 和 JavaScript 中的 *this* )，尽管这个名称完全是约定俗成的，理论上您可以随意命名。然而，我强烈建议坚持这个惯例，因为把它改变成除了*自己*之外的任何东西都不会让你的团队成员觉得很有趣。

让我们看一个简单的例子:

```
class Employee:def __init__(self, name, department, annual_salary):
    self.name = name
    self.department = department
    self.annual_salary  = annual_salarydef get_monthly_salary(self):
    return self.annual_salary / 12def earn_promotion(self,new_department, new_salary): 
    self.department = new_department
    self.annual_salary = new_salarytim = Employee('Tim Jones','Engineering',90_000)
print(tim.get_monthly_salary())
tim.earn_promotion('Management',105_000)
print(tim.get_monthly_salary())
```

除了用作对象构造器的 *__init__* dunder 方法，我们看到我们的 Employee 类还有 2 个方法，都是实例方法。

一个是为了从年薪中计算出月薪，另一个是为了以防工程师蒂姆因努力工作而得到奖励并获得晋升。如您所见，使用实例方法的主要原因是为了访问或修改对象的状态。

Python 非常灵活，也允许您直接更改对象属性，如下所示:

```
tim.department = 'Sales'
print(tim.department)
```

但是这通常被认为不是一个好的实践。理想情况下，您应该只通过该类的 API(在这种情况下，API 意味着该类公开的一组公共方法)来修改状态。您有时会看到开发人员通过在属性名称前添加单下划线或双下划线来强调属性是私有的，不能直接修改，但这不会阻止 Python 解释器执行直接的属性修改。

# 静态方法

当您想要调用某个类的某个方法，而不实际实例化该类时，静态方法非常有用。在 Python 中，当我们定义一个静态方法时，我们需要使用*静态方法*装饰器，正如你将在代码示例中看到的。因为静态方法不访问任何实例变量，所以它不需要一个*自身*属性。

但是，什么时候你甚至需要一个没有实际对象的类的方法呢？

一个很好的例子是，当您想按某个资源对一组方法进行分组时。假设你正在使用一个在圆上做各种计算的模块。您可以定义一个名为 Circles 的类，然后为所有不同种类的操作添加静态方法。下面是我的意思的一个示例代码片段:

```
import mathclass Circles: @staticmethod
    def get_area(radius):
        return radius * radius * math.pi @staticmethod
    def get_circumference(radius):
        return 2 * radius * math.piarea = Circles.get_area(3)
print(area)
circumference = Circles.get_circumference(3)
print(circumference)
```

这实际上是一种非常常见的模式，对于许多用例来说非常有用。看看 Java 内置的[数组类](https://docs.oracle.com/javase/7/docs/api/java/util/Arrays.html)就知道了。它的所有方法都是静态的，如果你在 Java 中使用数组，你很可能必须使用这个类。

另一个好的用例是*工厂*设计模式。尽管工厂也可以被实例化并具有实例方法，但如果工厂的返回值依赖于全局变量或与实际工厂的实例属性无关的变量，那么使用静态方法的工厂并不少见。这里有一个例子:

```
class EmployeeFactory:@staticmethod
def get_employee(name,department,salary):
    if department == 'design':
        return Designer(name, department, salary)
    elif department == 'management':
        return Manager(name, department, salary)
    elif department == 'engineering':
        return Engineer(name, department, salary)name = request.args['name']
department = request.args['department']
salary = request.args['salary']
employee = EmployeeFactory.get_employee(name, department, salary)
```

如您所见，我们根据传递的部门实例化了工厂中某个雇员类型的对象。然而，我们是从 HTML 表单(或您能想到的任何其他来源)获取部门值的，所以没有必要实例化工厂。我们只是在工厂内部访问该方法，因为它是静态的。

# 类方法

类方法有点类似于静态方法，主要区别在于类方法可以访问*类变量*。在我的经验中，它们比静态和实例方法更不常见，但实际上我在产品代码中不止一次地使用过它们。与静态方法类似，类方法也需要装饰器，只不过这次是类方法*。*

使用类方法的一个很好的例子是，如果你有一个值，它需要成为你的类的一部分，但是这个值是一个常量。访问令牌和秘密就是一个很好的例子。看一下这段代码:

```
import os
import requests
from requests.auth import HTTPBasicAuthclass TweetCollector: API_KEY = os.getenv('TWITTER_API_KEY')
    API_SECRET = os.getenv('TWITTER_SECRET')
    TWITTER_API_BASE_URL = os.getenv('TWITTER_API_BASE_URL') @classmethod
    def get_tweets_from_handle(cls,handle_id):
        req = requests.get(cls.TWITTER_API_BASE_URL,      auth=HTTPBasicAuth(cls.API_KEY, cls.API_SECRET))
    return req.json()
```

这里我们有一个名为 TweetCollector 的类，它调用 Twitter API 并获取属于某个 Twitter 句柄的 tweets。然而，为了使用 Twitter 的 API，我们需要使用 API key + API secret，所以将它们添加到 TweetCollector 类是有意义的，因为它们永远不会改变，并且我们需要它们用于 TweetCollector 发出的每个调用。

类方法的另一个很好的用例是，如果您希望在实例化类中的对象之前跟踪类内部的值。这里有一个例子:

```
class Package: package_id = 1_000_001

    def __init__(self, sender, reciever, weight):
        self.sender = sender
        self.reciever = reciever
        self.weight = weight
        self.package_id = Package.assign_package_id() def get_package_id(self):
        return self.package_id @classmethod
    def assign_package_id(cls):
        package_id = cls.package_id
        cls.package_id += 1
        return package_id print(Package.package_id)
new_package = Package('Amy from NY', 'Jose from Madrid', 10)
print(new_package.get_package_id())
print(Package.package_id)
```

这里我们有一个名为 Package 的类，它同时具有实例和类方法。我们使用一个名为 *package_id* 的类变量来确保每个包都使用正确的 id 进行序列化，该 id 在每次实例化一个新的包对象后递增。请记住，这个 package_id 只存储在应用程序内存中，所以一旦程序终止，它将恢复到初始值。我承认，这可能不是生产的最佳用例，但肯定可以用于各种用例。

# 抽象方法

我们都知道 Python 不支持接口。当我们知道对一个接口 进行 [*编程被认为是实现 OOP 的一个非常好的实践时，这有时真的会令人沮丧。它基本上意味着，如果某个类希望成为某个类型，您应该实现需要履行的某个契约。*](https://en.wikipedia.org/wiki/Interface-based_programming)

例如，如果一个名为 *ArrayList* 的类想要成为 *List* 家族的一部分，它需要实现 *List* 接口声明的所有方法。有点像大学兄弟会的入会仪式。如果你想被正式接纳为会员，你需要做其他会员做的所有事情。

它是多态性的基石，在 Java 和 C#等语言中广泛使用，甚至在 Golang 中也是如此，Golang 并不是真正的 OOP 语言(至少不是完全意义上的)。不得不在没有接口的情况下编写 OOP 有点问题，尤其是如果你试图实现类似于[六边形架构](https://en.wikipedia.org/wiki/Hexagonal_architecture_(software))的东西。

幸运的是，有一种变通方法- *抽象方法。*

假设我正在做一个项目，它是一组域驱动的微服务。我被委派在其中一个服务的数据层工作，让我们称之为*用户*服务。它的作用是注册新用户，获取活动用户的列表，以及删除不活动的用户。

我需要创建一个类来完成我对这个有用资源的所有数据库调用，但是，团队仍然不确定我们将使用哪个数据库。事实上，我们甚至不确定它是 SQL 数据库还是 NoSQL 数据库。

更糟糕的是，我们可能会在项目进行到一半时切换数据库。

我聪明地决定创建一个抽象类(缺少一个接口)，它将描述这个服务用于所有数据库调用的方法。

我的意思是:

```
from abc import ABC, abstractmethodclass UserRepository(ABC): @abstractmethod
    def add_user(username, email, password):
        pass @abstractmethod
    def get_active_users():
        pass @abstractmethod
    def delete_inactive_user(username):
        pass
```

这将作为任何将进行数据库调用的类的契约。如果它想成为一个用户存储库，它需要实现这三个方法，因为我们的业务需求明确声明我们需要能够添加新用户，获得活动用户的列表，并且能够删除非活动用户。

我们正在从内置的 [abc 模块](https://docs.python.org/3/library/abc.html)中导入 ABC(抽象基类的缩写)，该模块用于 Python 中关于抽象类的所有功能。

如果您试图实例化这个抽象类，您将得到一个错误:

*类型错误:无法用抽象方法 add_user、delete_inactive_user、get_users 实例化抽象类 UserRepository】*

既然团队最终决定使用 MySQL 作为我们的数据库，我们可以通过继承我们的基类来实现将在生产中执行数据库调用的实际类:

```
class MySQLUserRepository(UserRepository): def add_user(username, email, password):
        # Here we implement the mysql logic        add_user_using_mysql_logic()

    def get_users():
        users = fetch_users_from_mysql()
        return users def delete_inactive_user(username):
        delete_user_using_mysql_logic()
```

既然所有的抽象方法都是通过实现特定于从 MySQL 访问数据的逻辑来实现的，那么这个类就可以成功地用在我们的代码中了。

如果我出于某种原因忘记实现最后一个方法，让它保持抽象，我会得到下面的错误:

*类型错误:无法用抽象方法 delete_inactive_user* 实例化抽象类 MySQLUserRepository

现在，如果项目进行了 4 个月，团队意识到对于我们的用例来说，迁移到像 DynamoDB 这样的 NoSQL 数据库更有意义，我可以创建一个新的类，它也继承了抽象类，方法名称以及预期的最终结果和高级功能将保持不变。当然，访问 DynamoDB 的逻辑会有所不同。但是，这意味着我只需要更改该类中的逻辑，而服务中的其他内容将保持不变:

```
class DynamoDBUserRepository(UserRepository):

    def add_user(username, email, password):
        add_user_using_dynamodb_logic() def get_users():
        users = fetch_users_from_dynamodb()
        return users def delete_inactive_user(username):
        delete_user_using_dynamodb_logic()
```

从所有这些例子中可以看出，每种方法类型都有一个很好的用例。有时，您也可以根据自己的编码习惯来选择方法类型，但是在这种情况下，您应该始终考虑其他类型是否是更好的解决方案。

本文到此为止。一如既往，如果你喜欢，请尽情鼓掌:)

如果我错过了什么，请留下评论。

感谢阅读！