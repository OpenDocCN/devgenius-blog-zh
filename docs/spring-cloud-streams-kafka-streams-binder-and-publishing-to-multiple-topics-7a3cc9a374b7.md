# 春云流 Kafka 流活页夹和发布到多个主题

> 原文：<https://blog.devgenius.io/spring-cloud-streams-kafka-streams-binder-and-publishing-to-multiple-topics-7a3cc9a374b7?source=collection_archive---------2----------------------->

随着对数据实时处理需求的增加，许多开发人员正在构建和处理流数据。事实证明，Kafka 是市场上使用最广泛的工具之一，可以轻松处理数百万条记录。Kafka 提供了 Kafka Streams DSL，它已被证明是当今以声明方式或“函数”方式处理流数据的首选方法之一。从 Spring Cloud Stream 3.x.x 开始 Spring 框架建议使用函数式方法来处理数据。Spring cloud stream 提供了两种绑定器来与 Kafka 接口，Spring Cloud Stream Kafka Binder 和 Spring Cloud Streams Kafka Streams Binder。在这篇文章中，我们将处理春天云流卡夫卡溪流活页夹。所以让我们开始吧。

当你从一个主题开始消费时，很快会有一段时间你可能需要在不止一个主题上发布消息。spring Cloud Stream Kafka Streams Binder 或 KStream Binder 简而言之，给了我们发布多个主题的记录的手段。为此，我们需要返回一个 KStream 数组。这是发布多个主题记录的一种方式。发布消息的另一种方式是，您可以显式地使用 KStream DSL 的方法来发送任何主题的数据。我刚才提到的第二种方法是一种引入“副作用”的方法，从而使你的函数成为不纯函数。众所周知，在处理 FP 时，我们应该总是以编写纯函数为目标，但是在现实世界中，不可能构建没有任何副作用的应用程序。见鬼了。甚至连 DB 调用都是“副作用”。如果你问我，根据“函数”(数学中)的定义，一个单一的输入永远不会产生两个不同的输出。所以当我们从一个函数执行中返回两个不同的东西时，我们已经违反了 FP 的规则。所以有一点点“副作用”也是可以容忍的。注意:(*设计决策*)如果你正在尝试做 FP，并且即将引入副作用，那么记得在“边界”上移动那些。因为这不是一个关于函数式编程的帖子，所以我不会在这方面继续深入。

# 返回一个 KStream 数组

在 KStream binder 中，Spring 提供了很多不错的特性，并从我们这里抽象出了很多东西。让我们假设我们有一个主题'*文本输入主题*，文本消息存储在这个主题上。每条消息的密钥也在字符串中。如果我们必须使用来自该主题的消息，我们必须在类路径中创建一个 Bean，如下所示:

```
@Bean
public Consumer<KStream<String, String>> consumeTextMessages(){
    return stringStringKStream -> stringStringKStream.print(Printed.*toSysOut*()); //don't use in prod
}
```

一旦完成，我们还需要告诉 spring 使用哪个主题的消息，然后 spring 会处理其他的事情。

```
spring:
  cloud:
    stream:
      bindings:
        consumeTextMessages-in-0:
          destination: text-input-topic
```

在这里，我们简单地将消息消费并打印到 sysout。

让我们对其进行一些处理，并尝试将处理结果发布到其他主题上。为此，我们将做一个非常简单的用例，将每个文本转换成大写字母，然后将大写字母的文本发布到另一个主题:' *text-upper-topic* '。当我们想要消费一个输入时，我们使用消费者函数接口，同样，当我们想要为一个输入返回值时，我们将使用函数函数接口。这个函数的输入将是来自输入主题的消息，这个函数的返回值将由 Spring 推送到输出主题。所以我们的 bean 看起来会像:

```
@Bean
public Function<KStream<String, String>, KStream<String, String>> upperCaseProcessor(){
    return stringStringKStream -> stringStringKStream
            .mapValues((ValueMapper<String, String>) String::toUpperCase);
}
```

剩下的就由我们来告诉 spring 这里讨论的主题了:

```
spring:
  cloud:
    stream:
      bindings:
        upperCaseProcessor-in-0:
          destination: text-input-topic
        upperCaseProcessor-out-0:
          destination: text-upper-topic
```

回到我们最初的主题:我们如何发布多个主题的记录？如前所述，KStream binder 允许我们通过返回一个 KStream 数组来做到这一点。现在让我们假设一个用例，在输入主题上发布了两种类型的消息。一条消息是“我的第一条消息”，另一条消息是“我的第二条消息”。您希望在输出主题一上发布所有包含“第一”的消息，在输出主题二上发布所有包含“第二”的消息。为此，我们采用了一种叫做 k 流分支的技术。为了执行分支，我们首先需要调用 KStream 上的 *split* 方法，然后向*分支*方法提供*谓词*。任何满足谓词的记录都将被路由到相应的分支。branch 方法返回一个 String 和 KStream 的映射，因此我们需要提取所有的 KStream 并将它们作为数组返回。让我们看一个例子，这样就更清楚了。

```
@Bean
public Function<KStream<String, String>, KStream<String, String>[]> upperCaseProcessor(){
    return stringStringKStream -> {
        final Map<String, KStream<String, String>> first = stringStringKStream
                .peek((key, value) -> *log*.info("TEXT MSG READ."))
                .mapValues(value -> value.toUpperCase())
                .split()
                .branch((key, value) -> value.contains("FIRST"))
                .branch((key, value) -> value.contains("SECOND"))
                .noDefaultBranch();
        return first.values()
                .stream().map(stringStringKStream1 -> stringStringKStream1.mapValues((readOnlyKey, value) -> value.toLowerCase()))
                .toArray(KStream[]::new);
    };
}
```

在上面的方法中，我们返回函数的一个实例。这里首先要注意的是我们要返回的函数的类型:

```
Function<KStream<String, String>, KStream<String, String>[]>
```

这种类型表明函数的输入将是 KStream，但函数的返回类型将是 KStream 的数组。在拓扑中，我们使用峰值方法来记录消息的到达，然后映射这些值。**注意** :( *性能决策*)注意不要使用 *map* 方法，因为它可能会调用重新分区，从而降低处理速度。一般的规则是，如果不需要改变键，总是使用 *mapValues* 函数。

在我们将文本改为大写字母后，我们开始分支。我们必须首先调用 Split 函数，然后需要在 branch 函数中提供谓词。在分支调用之后，我们通过调用 *noDefaultBranch 来完成拓扑，*表明我们不需要另一个主题来分支我们不满足 Branch 方法中提到的任何谓词的消息。

一旦我们有了映射，我们就可以做更多的 java streams 魔术来最终返回所需的 KStream 数组。

拼图的最后一块是 application.yml 文件:

```
spring:
  cloud:
    stream:
      bindings:
        upperCaseProcessor-in-0:
          destination: src-textMsg-topic
        upperCaseProcessor-out-0:
          destination: out-textMsg-topic-0
        upperCaseProcessor-out-1:
          destination: out-textMsg-topic-1
```

现在出现了一个明显的问题，如果我们需要在多个主题上发布消息，而且是以两种不同的类型发布，该怎么办？要回答这个问题，请跟着做。

# 使用 KStream 向主题发布消息

为了实现这一点，我将再次考虑一个非常简单的用例:在您的输入主题中，您有一个订单的详细信息，您的任务是消费原始订单数据，然后在发送关于 *order-topic、*的订单信息之前屏蔽信用卡信息，同时您还需要将订单 id 发布到不同的主题。 *raw-order-topic* 和 *order-topic* hold key 为字符串，消息为 OrderInputMsg 用户自定义类型。应该保存订单 id 的主题将接受字符串格式的消息。

为了实现这个功能，我们将使用 KStream.to 方法。该方法将获取您想要发布消息的主题的名称，还将获取 KStream 将使用的 Serde 的信息。让我们看看将要创建的 Bean 定义:

在上面的定义中，我们返回的是函数的一个实例

```
Function<KStream<String, OrderInputMsg>, KStream<String, OrderInputMsg>>
```

该函数将接受 KStream 的输入，并返回 KStream 的一个实例。神奇之处在于实现。我们获取输入流，通过将值映射到订单 id 来创建中间 KStream，然后使用 KStream.to 函数将该信息发送到单独的主题。一旦完成，我们只需执行屏蔽信用卡号的逻辑，然后将生成的 KStream 提交给 *order-topic。*

GitHub 链接:[https://GitHub . com/NajeebArif/SpringKStreamHub/tree/two-different-types](https://github.com/NajeebArif/SpringKStreamHub/tree/two-different-types)

编码快乐！