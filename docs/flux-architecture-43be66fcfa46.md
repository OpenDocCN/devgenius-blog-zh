# 通量架构

> 原文：<https://blog.devgenius.io/flux-architecture-43be66fcfa46?source=collection_archive---------9----------------------->

# 概观

Flux 在脸书用于他们的前端 web 应用程序。与其说它是一个框架，不如说它是一个模式。Flux 的三个主要部分是动作、调度器和存储。数据流是单向的，这使得调试更加容易。Redux 是一个使用 Flux 的库。

![](img/3f038d62193a127466e85b9079412369.png)

# 行动

当用户与应用程序交互时，就会调用这个动作。例如，如果您想在待办事项列表中添加一个新任务，您可以为此创建一个操作。当定义一个动作时，它应该包含有效负载或数据，以及在存储中被解释的类型。

```
export function addTodo(text) { return { type: 'ADD_TODO', text }}
```

# 分配器

调度程序处理不断变化的数据流。调度员的唯一任务是将动作分配给商店。商店将有登记在调度程序中的回调。当一个动作被调用时，调度程序会将该动作发送到所有商店。在 Redux 中，用户不是定义调度程序的人。

# 商店

存储包含数据和逻辑。状态是存储所有实际数据的地方，修改数据的逻辑是回调。switch 语句用于解释调用适当回调的动作。

```
function todos(state = [], action) {
    switch (action.type) {
        case ADD_TODO:
            return [
                ...state,
                {
                    text: action.text,
                    completed: false
                 }
             ]
        case REMOVE_TODO:
            return [
                ...state.filter(todo => todo.text !== action.text)
             ]
        default:
            return state
        }
    }
}
```

# 结论

Redux 是 Flux 架构的最好例子，但是不使用 Redux 也可以实现它。使用 Flux 通过使用单向数据绑定而不是双向数据绑定，使调试更容易。通过使用单向，当操作更新数据时，使用该数据的所有页面都将被更新。