# Kotlin 的对象映射高级功能和 QoL

> 原文：<https://blog.devgenius.io/object-mapping-advanced-features-qol-with-kotlin-c8ea8d9ebf20?source=collection_archive---------4----------------------->

![](img/c1d06d5cafaf0a32f2bdcc06d5661066.png)

由 [Link Hoang](https://unsplash.com/@linkhoang?utm_source=medium&utm_medium=referral) 在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍照

当使用多层应用程序、外部库、遗留代码库或外部 API 时，我们经常需要在不同的对象或数据结构之间进行映射。

在本教程中，我们将检查一些对象映射库的高级功能，以简化这项任务，同时节省开发和维护时间。

在我们的例子中，我们将使用库 [ShapeShift](https://github.com/krud-dev/shapeshift/) 。这是一个用于 Kotlin/Java 的轻量级对象映射库，有很多很酷的特性。

# 自动映射

我们将轰轰烈烈地开始。自动映射能够并且将会为您节省大量时间和模板代码。一些应用程序需要对象之间的手动映射，但是大多数应用程序通过使用这一特性将节省大量的时间。更好的是，有了 ShapeShift 的默认转换器，我们甚至可以在不同的数据类型之间使用自动映射。

# 简单映像

让我们从一个简单的自动映射例子开始。我们有两个对象，想象它们也可能有几十个、几百个、甚至几千个字段(对于这里的疯狂的人来说)。

```
class User {
    var id: String = ""
    var name: String? = null
    var email: String? = null
    var phone: String? = null
}class UserDTO {
    var id: String = ""
    var name: String? = null
    var email: String? = null
    var phone: String? = null
}
```

我们想要映射从`User`到`UserDTO`的所有字段。使用自动映射，我们不需要写任何锅炉板代码。映射器将定义如下:

```
val mapper = mapper<User, UserDTO> {
    autoMap(AutoMappingStrategy.BY_NAME_AND_TYPE)
}
```

瞧啊。所有字段将自动映射，无需任何手动锅炉板代码。

# 高级映射

在这个例子中，我们将使用[默认变压器](https://shapeshift.krud.dev/api-documentation/transformers#default-transformers)的功率来进一步进行自动映射。

```
class User {
    var id: String = ""
    var name: String? = null
    var birthDate: Date? = null
}class UserDTO {
    var id: String = ""
    var fullName: String? = null
    var birthDate: Long? = null
}
```

注意，在源类和目的类中,`birthDate`字段的类型是不同的。但是使用默认变压器的能力，我们仍然可以在这里使用自动映射。

```
val mapper = mapper<User, UserDTO> {
    autoMap(AutoMappingStrategy.BY_NAME)
}
```

我们将自动映射策略改为`BY_NAME`,这样它也可以映射不同类型的字段。现在我们需要向`ShapeShift`实例注册一个默认的转换器，以便它知道如何将`Date`转换为`Long`。

```
val shapeShift = ShapeShiftBuilder()
    .withTransformer(DateToLongMappingTransformer(), true)
    .build()
```

我们还可以在自动映射的基础上添加手动映射，以便添加/更改行为。源类和目的类对于`name`字段有不同的名称，所以我们将为它添加手动映射。

```
val mapper = mapper<User, UserDTO> {
    autoMap(AutoMappingStrategy.BY_NAME)
    User::name mappedTo UserDTO::fullName
}
```

自动映射非常适合不需要特定映射的用例。它有助于减少配置映射所需的手动模板代码的数量，也有助于保持头脑清醒。

# 变形金刚(电影名)

转换器是非常有用的功能，它允许您在映射字段时将字段的类型/值转换为不同的类型/值。

我们广泛使用的一些使用案例:

*   在服务器和客户端对象之间将日期转换为长整型，反之亦然。
*   将 JSON 字符串转换成它的实际类型，反之亦然。
*   将逗号分隔的字符串转换为枚举列表。
*   使用 Spring transformers 将另一个对象 id 从 DB 转换为它的对象或它的一个字段。

# 基本变压器

我们将从一个简单的变压器例子开始。迄今为止和迄今为止的变压器:

```
class DateToLongMappingTransformer : MappingTransformer<Date, Long> {
    override fun transform(context: MappingTransformerContext<out Date>): Long? {
        return context.originalValue?.time
    }
}class LongToDateMappingTransformer : MappingTransformer<Long, Date> {
    override fun transform(context: MappingTransformerContext<out Long>): Date? {
        context.originalValue ?: return null
        return Date(context.originalValue)
    }
}
```

我们现在需要做的就是[注册](https://shapeshift.krud.dev/api-documentation/transformers#registering-transformers)他们。

```
val shapeShift = ShapeShiftBuilder()
    .withTransformer(DateToLongMappingTransformer(), true) // "true" is optional, we are registering the transformers as default transformers, more on that later.
    .withTransformer(LongToDateMappingTransformer(), true)
    .build()
```

就是这样！我们现在可以在映射物体时使用变形金刚了。

```
class User {
    var id: String = ""
    var name: String? = null
    var birthDate: Date? = null
}class UserDTO {
    var id: String = ""
    var name: String? = null
    var birthDate: Long? = null
}val mapper = mapper<User, UserDTO> {
    User::id mappedTo UserDTO::id
    User::name mappedTo UserDTO::name
    User::birthDate mappedTo UserDTO::birthDate withTransformer DateToLongMappingTransformer::class // We don't have to state the transformer here because it is a default transformer
}
```

# 直列变压器

在某些用例中，我们希望转换值，但是我们不需要可重用的转换器，我们也不想创建一个一次性使用的类。

救援用直列式变压器！内联转换器允许转换值，而无需创建和注册转换器。

```
val shapeShift = ShapeShiftBuilder()
        .withMapping<Source, Target> {
            // Map birthDate to birthYear with a transformation function
            Source::birthDate mappedTo Target::birthYear withTransformer { (originalValue) ->
                originalValue?.year
            }
        }
        .build()
```

# 高级变压器

转换器还允许我们对数据库或其他数据源进行转换。

在这个例子中，我们将使用 [Spring Boot 集成](https://shapeshift.krud.dev/guides/spring-usage)的能力来创建带 DB 访问的变压器。

我们有三种型号:

*   作业—数据库实体。
*   用户—数据库实体。
*   UserDTO —客户端模型。

```
class Job {
    var id: String = ""
    var name: String = ""
}class User {
    var id: String = ""
    var jobId: String? = null
}class UserDTO {
    var id: String = ""
    var jobName: String? = null
}
```

我们希望通过从数据库中查询作业并将其设置在 DTO 上，将`User`上的`jobId`转换为`UserDTO`上的`jobName`。

在 Spring 的例子中，通常避免静态函数或域对象上的函数与应用程序上下文交互。

我们将使用 ShapeShift 的 [Spring integration](https://shapeshift.krud.dev/guides/spring-usage) 来创建一个组件，作为访问我们的 DAO bean 的转换器。

```
@Component
class JobIdToNameTransformer(
    private val jobDao: JobDao
) : MappingTransformer<String, String>() {
    override fun transform(context: MappingTransformerContext<out String>): String? {
        context.originalValue ?: return null
        val job = jobDao.findJobById(context.originalValue!!)
        return job.name 
    }
}
```

剩下要做的就是在我们的映射中使用这个转换器。

```
val mapper = mapper<User, UserDTO> {
    User::id mappedTo UserDTO::id
    User::jobId mappedTo UserDTO::jobName withTransformer JobIdToNameTransformer::class
}
```

使用变压器的另一个好处是它们的可重用性。在某些用例中，我们可以创建更多的通用转换器，这些转换器将具有广泛的应用。

# 默认变压器

注册变压器时，您可以指明变压器是否为[默认变压器](https://shapeshift.krud.dev/api-documentation/transformers#default-transformers)。当您将类型为< A >的字段映射到类型为< B >的字段而没有指定要使用的转换器时，将使用默认的类型为< A、B >的转换器。

正如我们已经看到的，默认转换器对于循环转换非常有用，尤其是对于自动映射。

# 深度映射

如果我们想在一个对象的字段中映射出/映射出可用的字段呢？我们甚至可以轻松做到这一点。

为了访问子类，我们可以使用`..`操作符。让我们看看下面的例子。

```
class From {
    var child: Child = Child() class Child {
        var value: String?
    }
}class To {
    var childValue: String?
}
```

我们希望将`From`类中的`Child`类中的`value`字段映射到`To`类中的`childValue`字段。我们将使用`..`操作符创建一个映射器。

```
val mapper = mapper<From, To> {
    From::child..From.Child::value mappedTo To::childValue
}
```

让我们更进一步，多层次的深度。

```
class From {
    var grandChildValue: String?
}class To {
    var child: Child = Child() class Child {
        var grandChild: GrandChild = GrandChild()
    } class GrandChild {
        var value: String?
    }
}
```

要访问 grand child 字段，我们只需使用两次`..`操作符。

```
val mapper = mapper<From, To> {
    From::grandChildValue mappedTo To::child..To.Child::grandChild..To.GrandChild::value
}
```

# 条件映射

条件允许我们向特定的字段映射添加一个谓词，以确定是否应该映射这个字段。

使用这个特性就像创建一个条件一样简单。

```
class NotBlankStringCondition : MappingCondition<String> {
    override fun isValid(context: MappingConditionContext<String>): Boolean {
        return !context.originalValue.isNullOrBlank()
    }
}
```

并将该条件添加到所需的字段映射中。

```
data class SimpleEntity(
    val name: String
)data class SimpleEntityDisplay(
    val name: String = ""
)val mapper = mapper<SimpleEntity, SimpleEntityDisplay> {
    SimpleEntity::name mappedTo SimpleEntityDisplay::name withCondition NotBlankStringCondition::class
}
```

# 内嵌条件

像变压器一样，条件也可以使用函数内联添加。

```
val mapper = mapper<SimpleEntity, SimpleEntityDisplay> {
    SimpleEntity::name mappedTo SimpleEntityDisplay::name withCondition {
        !it.originalValue.isNullOrBlank()
    }
}
```

# 注释映射

这个特性受到了很多人的讨厌，因为它违反了关注点分离的原则。我同意，这在某些应用程序中可能是一个问题，但是在某些用例中，所有对象都是同一个应用程序的一部分，在对象之上配置映射逻辑也非常有用。查看[文档](https://shapeshift.krud.dev/api-documentation/annotations)并自行决定。

# 结论

对象映射库并不是每个应用程序的解决方案。对于小而简单的应用程序，使用锅炉板映射功能就足够了。但是，在开发更大、更复杂的应用程序时，对象映射库可以让您的代码更上一层楼，节省您的开发和维护时间。所有这些都减少了 boiler-plate 代码的数量，并从整体上改善了开发体验。

就我个人而言，我曾经使用过手动映射功能，并对此感到满意。它“只是”一些简单的代码行。在升级我们的应用程序以使用对象映射作为我们的“无模板”框架的一部分(我们将在稍后讨论该框架)之后，我不能回头了。现在我们把更多的时间花在重要和有趣的事情上，几乎没有时间花在无聊的锅炉板代码上。