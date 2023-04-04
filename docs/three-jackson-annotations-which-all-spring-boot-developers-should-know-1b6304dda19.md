# @JsonIgnore、@JsonProperty 和@JsonAlias 在春季开机中的重要性

> 原文：<https://blog.devgenius.io/three-jackson-annotations-which-all-spring-boot-developers-should-know-1b6304dda19?source=collection_archive---------0----------------------->

在本文中，我将详细介绍三种广泛使用的 Jackson 注释，它们在 Spring boot 的各种用例中都很有用。

**First @JsonIgnore**

*当 Java 类被序列化为 JSON 对象时，这个注释有助于我们忽略它的某些属性。*

我们先来了解一下，既然反正是要忽略的，为什么还需要这样的属性。可能有各种原因，比如

1.  你想对客户隐藏数据，如密码，出生日期等。
2.  客户不想要你未经处理的原始数据，如 JSON 字符串等。

要做到这一点，你需要做什么？您只需在特定字段上方添加一个@JsonIgnore 注释。

```
[@Builder](http://twitter.com/Builder)
[@NoArgsConstructor](http://twitter.com/NoArgsConstructor)
[@AllArgsConstructor](http://twitter.com/AllArgsConstructor)
[@Data](http://twitter.com/Data)
public class Student implements Serializable {
 private static final long serialVersionUID = 1L; private Integer id;
 private String name;
 private String class;

 // Client don't want this unprocessed data
 [@JsonIgnore](http://twitter.com/JsonIgnore)
 private String jsonResult; 

 // They want processed data instead
 private Result result;
}
```

例如，这里我们忽略了`jsonResult`，当这个 Java 对象被序列化时，它不会出现在 JSON 对象中。它看起来会像这样。

```
{
 "id": 1,
 "name": "Ram",
 "class": 9,
 "result": {
  "math": 90,
  "science": 95,
  "english": 80
 }
}
```

这不是使用该注释的唯一方式。假设您对 GET Response 和 POST 请求主体使用一个公共类。在这种情况下，您可能必须允许客户端设置该属性，但限制他们获取该属性。要做到这一点，只需将@JsonIgnore 添加到 Getter 方法中，并保留 Setter 方法。

```
public class User {
    private String name;
    private String password; // We are still hiding it from the client in the Get Request
    [@JsonIgnore](http://twitter.com/JsonIgnore)
    public String getPassword() {
        return password;
    } // But allowing them to set it in the POST request
    public void setPassword(String password) {
        this.password = password;
    }
}
```

**秒@JsonProperty**

*这个注释帮助我们为一个字段定义一个逻辑属性，这个属性将被用于序列化和反序列化。*

让我们讨论@JsonProperty 的一些常见用例。

1.  如果您使用自动生成的 getters 和 setters，那么您可以将@JsonProperty 用于像`isActive`这样的布尔字段，这样 JSON 对象和 Java 对象就有了一致的字段名`isActive`。否则，默认情况下，Jackson 将从 getter 或 setter 中确定属性名，您可能会看到 JSON 和 Java 对象中的差异。
2.  假设我们从某个 API 获取数据，其中属性名在 snake_case 或 PascalCase 中，但我们遵循 camelCase，那么我们也可以使用@JsonProperty。

```
public class Student implements Serializable {
 private static final long serialVersionUID = 1L; private Integer id;
 private String name;
 private String class;

 @JsonProperty("isActive")
 private boolean isActive;

 @JsonProperty("rollNumber")
 private boolean ROLL_NUMBER;
}
```

例如，这里我们将字段`isActive`的属性名定义为 isActive，这样即使是 JSON 对象也可以有相同的属性名，而不是像`active`那样。

其次，对于`ROLL_NUMBER`字段，我们将属性名定义为 rollNumber，这样 JSON 对象可以在 camelCase 中而不是 UPPER_CASE 中有一个属性名。

**第三个@JsonAlias**

*这个注释帮助我们在反序列化过程中为要映射到 Java 字段的 JSON 属性定义不同的别名。*

我们通过一个例子来理解这一点。

假设我们正在聚合来自各种流媒体平台 API 的电影和电视剧数据，并将其存储在我们的应用程序类中。让我们假设，我们有一门电影课和一门电视剧课。

当我们从各种来源获取数据时，我们可以预期属性名的各种命名约定，但是我们希望将相似的数据映射到一个 Java 字段，对吗？这就是@JsonAlias 的用武之地。怎么会？

让我们看一遍电影课的例子。

```
public class Movie { // This way our application will consider
    // title, movieName and name all three during deserialization.
    @JsonProperty("name")
    @JsonAlias({"title", "movieName"})
    private String name;

    // Similarly here, it would consider all name  
    // like genre, type and category
    @JsonProperty("genre")
    @JsonAlias({"type", "category"})
    private String genre;

    private String streamingPlatform;
}
```

例如，这里我们有两个 Java 字段，其中 JSON 属性可以有不同的名称，这取决于数据来自哪个 API。因此，通过给定别名如`title`和`movieName`，我们告诉我们的应用程序，每当它在 JSON 对象中遇到这些属性名时，它应该设置我们的`name`字段。通过使用@JsonProperty，我们可以为 Java 字段定义一个逻辑属性名，它将在序列化和反序列化中使用。

这些是杰克逊的一些注释，我已经见过很多次了，随身带着这些工具很方便。

但是也有许多其他注释可以用在不同的场景中。如果您感兴趣，您可以在此详细浏览所有这些内容。

1.  [https://www.baeldung.com/jackson-annotations](https://www.baeldung.com/jackson-annotations)
2.  [https://github . com/faster XML/Jackson-Annotations/wiki/Jackson-Annotations](https://github.com/FasterXML/jackson-annotations/wiki/Jackson-Annotations)
3.  [https://github . com/faster XML/Jackson-Databind/wiki/Databind-Annotations](https://github.com/FasterXML/jackson-databind/wiki/Databind-Annotations)