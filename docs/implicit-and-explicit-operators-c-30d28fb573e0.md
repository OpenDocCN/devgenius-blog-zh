# 隐式和显式运算符— C#

> 原文：<https://blog.devgenius.io/implicit-and-explicit-operators-c-30d28fb573e0?source=collection_archive---------2----------------------->

![](img/d4b1ebe3c262dd8277c25266b517b92b.png)

照片由[罗西·斯泰格勒斯](https://unsplash.com/@rosiefoto13?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 拍摄

左边还是右边？你总是有比看得见的更多的选择。有时您需要将一些对象转换为另一种类型。例如，当您想要将一个域实体映射到它的 DTO(数据传输对象)时。假设您有 30 个域实体和另外 30 个 DTO 实体，每个域实体一个。每种类型的实体都有一个方法将这些对象转换成它的 DTO。这是我会想到的第一个选择。

# 如果我们有办法自动完成这种转换会怎么样？

会很棒的，对吧？是的。我向您展示 C#的**隐式**和**显式**运算符。他们的目标是简化一个类的转换过程。让我们看一些代码。

```
public class MessageTableEntity
{
    public Guid Id { get; set; }
    public string Message { get; set; }
    public string CreatedBy { get; set; }
}public class MessageDto
{
    public string Id { get; set; }
    public string Message { get; set; }
}
```

所以 DTO 类不需要看到谁创建了消息，假设这是私有信息，我们不想让它可见。对于表实体和 DTO 之间的任何转换，我们将进行如下操作:

```
public MessageDto ConvertToDto(MessageTableEntity tableEntity)
{
    MessageDto dto = new MessageDto
    {
        Id = tableEntity.Id.ToString(),
        Message = tableEntity.Message
     };
    return dto;
}
```

但是，如果我们需要对具有多个属性的类进行多次这样的操作，这将是一项非常枯燥的工作。不需要有几个方法来进行类之间的映射。

# 我们如何定义隐式和显式运算符？

在这种情况下，我将在`MessageTableEntity` 和`MessageDto`之间进行转换，但是不要忘记，您可以定义这个操作符来从 DTO 转换到表实体。

## 含蓄的

```
public class MessageTableEntity
{
    public Guid Id { get; set; }
    public string Message { get; set; }
    public string CreatedBy { get; set; } **public static implicit operator MessageDto(
                                       MessageTableEntity entity)
    {
        return new MessageDto
        {
            Id = entity.Id.ToString(),
            Message = entity.Message
        };
    }**
}
```

## 明确的

```
public class MessageTableEntity
{
    public Guid Id { get; set; }
    public string Message { get; set; }
    public string CreatedBy { get; set; } **public static explicit operator MessageDto(
                                       MessageTableEntity entity)
    {
        return new MessageDto
        {
            Id = entity.Id.ToString(),
            Message = entity.Message
        };
    }**
}
```

代码基本相同，唯一的变化是所用操作符的名称。

# 如何使用？

假设我们有下面的`MessageTableEntity`实例:

```
MessageTableEntity tableEntity = new MessageTableEntity
{
    Id = Guid.NewGuid(),
    Message = "Test message",
    CreatedBy = "User_ABC"
};
```

## 使用隐式运算符

```
MessageDto dto = tableEntity;
```

## 使用显式运算符

```
MessageDto dto = (MessageDto)tableEntity;
```

显式转换类似于强制转换操作。我们使我们要将对象转换成的类型可见。隐式运算符，如果你不知道存在一个隐式定义，就不太容易理解。

# 我可以同时定义两个运算符吗？

不可以。您只能定义一个运算符。如果定义了**显式操作符**，那么**只能显式转换对象**。然而，如果你定义了**隐式操作符**，你可以用两种方式**使用它，隐式和显式**。

```
MessageDto dto = tableEntity;
MessageDto dto = (MessageDto)tableEntity;
```

上面的例子，**这是可能的，因为我们在`MessageTableEntity`和`MessageDto`之间定义了隐式操作符**。现在是你的选择了，你可以选择你认为最适合你的情况的方法。

# 如果我有一个不同类的转换怎么办？

是的，有可能。给定了`MessageTableEntiy`类，假设我们也想要一个到`string`的隐式转换。

```
public class MessageTableEntity
{
    public Guid Id { get; set; }
    public string Message { get; set; }
    public string CreatedBy { get; set; }public static implicit operator MessageDto(
                                       MessageTableEntity entity)
    {
        return new MessageDto
        {
            Id = entity.Id.ToString(),
            Message = entity.Message
        };
    } **public static implicit operator string(
                                       MessageTableEntity entity)
    {
        return entity.Message;
    }**
}
```

然后，我们可以进行以下转换:

```
MessageDto dto = tableEntity;
MessageDto dto = (MessageDto)tableEntity;
**string message = tableEntity;**
```

# 结论

当我们有一个解决方案，其中类之间的转换通常要进行多次时，这些操作符为我们提供了一个新的选择。含蓄也好，明示也好，都没关系，这是你自己的喜好。记住隐式操作符为您提供的多功能性，也不要忘记显式操作符为您提供的最佳控制，因为转换类的代码更加清晰可见。