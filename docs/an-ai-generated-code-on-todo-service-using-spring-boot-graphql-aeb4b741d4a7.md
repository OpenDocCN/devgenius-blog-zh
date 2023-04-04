# 使用 ChatGPT 生成无聊的代码

> 原文：<https://blog.devgenius.io/an-ai-generated-code-on-todo-service-using-spring-boot-graphql-aeb4b741d4a7?source=collection_archive---------4----------------------->

![](img/8e4dae5663af4e6e124d31607e9ebbd0.png)

杰克逊·索法特在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄的照片

我在探索 ChatGPT。我将和大家分享我的一个经历。

我在系统中提出的第一个问题是

> 你能用 spring boot GraphQL 写一篇关于 todo 服务的文章吗？

我立即得到的回复就贴在下面

GraphQL 是一种用于 API 的查询语言，它允许客户端准确地请求他们需要的数据，使得在单个请求中获得所有需要的数据成为可能。这使得它成为传统 REST APIss 的一种高效而强大的替代方法，传统 REST API 通常需要多次往返来获取所有必要的数据。

在本文中，我们将学习如何使用 Spring Boot 和 GraphQL 创建一个 GraphQL 服务。我们将从建立项目开始，然后我们将定义模式，创建数据模型，最后，我们将实现 GraphQL 查询和变异解析器。

# 设置项目

首先，我们需要使用 Spring Initializr 创建一个新的 Spring Boot 项目。我们将选择以下依赖项:

*   网
*   GraphQL
*   Lombok(用于简化数据模型)

一旦创建了项目，我们需要将`graphql-java-tools`依赖项添加到`pom.xml`文件中:

```
<dependency>
    <groupId>com.graphql-java</groupId>
    <artifactId>graphql-java-tools</artifactId>
    <version>5.7.0</version>
</dependency>
```

这种依赖性允许我们使用`@GraphQLQuery`和`@GraphQLMutation`注释来定义查询和变异解析器。

# 定义模式

下一步是为我们的服务定义 GraphQL 模式。该模式定义了可以查询的数据类型以及可以执行的操作(查询和变异)。

在我们的例子中，我们将为待办事项列表应用程序创建一个简单的模式。该模式将定义以下类型:

*   `Todo`:表示状态为`id`、`title`、`completed`的待办事项。
*   `Query`:定义可以对数据进行的查询。
*   `Mutation`:定义可以对数据执行的突变(创建、更新和删除操作)。

下面是模式定义:

```
type Todo {
    id: ID!
    title: String!
    completed: Boolean!
}

type Query {
    todos: [Todo]
    todo(id: ID!): Todo
}

type Mutation {
    addTodo(title: String!, completed: Boolean!): Todo
    updateTodo(id: ID!, title: String, completed: Boolean): Todo
    deleteTodo(id: ID!): Todo
}
```

注意，我们使用 GraphQL 提供的`ID`和`String`类型，以及`!`和`[]`符号来分别表示不可空类型和列表类型。

# 创建数据模型

接下来，我们需要为我们的服务创建数据模型。我们将使用`Todo`类来表示一个待办事项。该类将具有以下属性:

*   `id`:待办事项的唯一标识。
*   `title`:待办事项的标题。
*   `completed`:表示待办事项是否完成的标志。

> 问 AI 系统的后续问题是“可以创建数据模型类和解析器服务吗？”

确切答复-

当然，这里是`Todo`类以及查询和变异解析器:

```
import lombok.AllArgsConstructor;
import lombok.Data;
import lombok.NoArgsConstructor;

@Data
@AllArgsConstructor
@NoArgsConstructor
public class Todo {
    private String id;
    private String title;
    private boolean completed;
}
```

```
import java.util.ArrayList;
import java.util.HashMap;
import java.util.List;
import java.util.Map;
import java.util.UUID;

import org.springframework.stereotype.Service;

import com.coxautodev.graphql.tools.GraphQLMutationResolver;
import com.coxautodev.graphql.tools.GraphQLQueryResolver;

@Service
public class TodoResolver implements GraphQLQueryResolver, GraphQLMutationResolver {

    private final Map<String, Todo> todos = new HashMap<>();

    @GraphQLQuery(name = "todos")
    public List<Todo> getTodos() {
        return new ArrayList<>(todos.values());
    }

    @GraphQLQuery(name = "todo")
    public Todo getTodo(@GraphQLArgument(name = "id") String id) {
        return todos.get(id);
    }

    @GraphQLMutation(name = "addTodo")
    public Todo addTodo(@GraphQLArgument(name = "title") String title, @GraphQLArgument(name = "completed") boolean completed) {
        Todo todo = new Todo(UUID.randomUUID().toString(), title, completed);
        todos.put(todo.getId(), todo);
        return todo;
    }

    @GraphQLMutation(name = "updateTodo")
    public Todo updateTodo(@GraphQLArgument(name = "id") String id, @GraphQLArgument(name = "title") String title, @GraphQLArgument(name = "completed") boolean completed) {
        Todo todo = todos.get(id);
        if (todo == null) {
            return null;
        }
        todo.setTitle(title);
        todo.setCompleted(completed);
        return todo;
    }

    @GraphQLMutation(name = "deleteTodo")
    public Todo deleteTodo(@GraphQLArgument(name = "id") String id) {
        return todos.remove(id);
    }

}
```

到目前为止一切顺利　‘😎，然后我问了下面的跟进问题

> "你能在这里使用 mongo db 吗？"

```
import java.util.List;
import java.util.Optional;
import java.util.UUID;

import org.springframework.stereotype.Service;

import com.coxautodev.graphql.tools.GraphQLMutationResolver;
import com.coxautodev.graphql.tools.GraphQLQueryResolver;

@Service
public class TodoResolver implements GraphQLQueryResolver, GraphQLMutationResolver {

    private final TodoRepository todoRepository;

    public TodoResolver(TodoRepository todoRepository) {
        this.todoRepository = todoRepository;
    }

    @GraphQLQuery(name = "todos")
    public List<Todo> getTodos() {
        return todoRepository.findAll();
    }

    @GraphQLQuery(name = "todo")
    public Optional<Todo> getTodo(@GraphQLArgument(name = "id") String id) {
        return todoRepository.findById(id);
    }

    @GraphQLMutation(name = "addTodo")
    public Todo addTodo(@GraphQLArgument(name = "title") String title, @GraphQLArgument(name = "completed") boolean completed) {
        Todo todo = new Todo(UUID.randomUUID().toString(), title, completed);
        return todoRepository.save(todo);
    }

    @GraphQLMutation(name = "updateTodo")
    public Optional<Todo> updateTodo(@GraphQLArgument(name = "id") String id, @GraphQLArgument(name = "title") String title, @GraphQLArgument(name = "completed") boolean completed) {
        Optional<Todo> optionalTodo = todoRepository.findById(id);
        if (optionalTodo.isEmpty()) {
            return Optional.empty();
        }
        Todo todo = optionalTodo.get();
        todo.setTitle(title);
        todo.setCompleted(completed);
        return Optional.of(todoRepository.save(todo));
    }

    @GraphQLMutation(name = "deleteTodo")
    public Optional<Todo> deleteTodo(@GraphQLArgument(name = "id") String id) {
        Optional<Todo> optionalTodo = todoRepository.findById(id);
        if (optionalTodo.isEmpty()) {
            return Optional.empty();
        }
        todoRepository.deleteById(id);
        return optionalTodo;
    }

}
```

在这个实现中，我们将一个`TodoRepository`实例注入 resolver 类。这个存储库用于访问 MongoDB 数据库中的待办事项。

查询和变异解析器方法被更新为使用存储库而不是内存存储。例如，`getTodos()`方法现在使用存储库的`findAll()`方法从数据库中获取所有待办事项。

可怕的🔥！！。下一个跟进问题

> 我如何从操场上测试它？

要从操场上测试 todo GraphQL 服务，请遵循以下步骤:

1.  通过运行主类来启动 Spring Boot 应用程序。
2.  在您的网络浏览器中访问[http://localhost:8080/GraphQL](http://localhost:8080/graphql)，打开 graph QL 游乐场。
3.  在 playground 中，您可以编写和执行 GraphQL 查询和变异来对 todo 项执行各种操作。例如，您可以使用以下查询来查询所有待办事项:

```
query {
  todos {
    id
    title
    completed
  }
}
```

4.您还可以使用以下变体创建一个新的 todo 项

```
mutation {
  addTodo(title: "Write an article", completed: false) {
    id
    title
  }
}
```

5.类似地，您可以使用适当的变化来更新和删除 todo 项。

kudos ChatGPT👏

# 结论

人工智能系统很快就生成了内容和代码。但是这里需要强调一点，要在实际项目中使用人工智能生成的代码，开发人员应该对该技术有基本的了解，因为它需要修改。

你有什么看法，我来评论一下！