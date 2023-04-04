# React 提示— GraphQL 查询、URL 参数和 React 路由器

> 原文：<https://blog.devgenius.io/react-tips-graphql-queries-url-parameters-and-react-router-5759c44badc6?source=collection_archive---------3----------------------->

![](img/4716b088e531fbe27b4e1b67672bc702.png)

由[奥地利国家图书馆](https://unsplash.com/@austriannationallibrary?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄的照片

React 是一个用于创建 web 应用程序和移动应用程序的流行库。

在本文中，我们将了解一些编写更好的 React 应用程序的技巧。

# 使用 React Apollo 进行 GraphQL 查询

我们可以通过使用`client.query`方法用 React Apollo 进行 GraphQL 查询。

一旦我们通过调用`withApollo`函数更新组件，这就可用了。

例如，我们可以写:

```
class Foo extends React.Component {
  runQuery() {
    this.props.client.query({
      query: gql`...`,
      variables: { 
        //...
      },
    });
  } render() { 
    //...
  }
}export default withApollo(Foo);
```

一旦我们用我们的组件调用`withApollo`来返回一个组件，道具中的`client.query`方法就可用了。

该方法接受一个具有`query`和`variables`属性的对象。

属性接受一个由表单标签返回的查询对象。

`variables`有一个带有变量的对象，我们在查询字符串中插入这些变量。

我们也可以用`ApolloConsumer`组件包装我们的应用程序。

例如，我们可以写:

```
const App = () => (
  <ApolloConsumer>
    {client => {
      client.query({
        query: gql`...`,
        variables: { 
          //...
        }
      })
    }}
  </ApolloConsumer>
)
```

我们可以在这里查询。

对于功能组件，有一个`useApolloClient`钩子。

例如，我们可以写:

```
const App = () => {
  const client = useApolloClient();
  //...
  const makeQuery = () => {
    client => {
      client.query({
        query: gql`...`,
        variables: { 
          //...
        }
      })
    }
  }
  //...
}
```

钩子返回我们可以用来查询的客户机。

还有一个`useLazyQuery`钩子，我们可以用它来进行查询。

例如，我们可以写:

```
const App = () => {
  const [runQuery, { called, loading, data }] = useLazyQuery(gql`...`)
  const handleClick = () => runQuery({ 
    variables: { 
      //...
    } 
  })
  //...
}
```

让我们进行查询。

# React 路由器将 URL 参数传递给组件

如果我们创建一个允许我们向其传递参数的路由，我们就可以传递 URL 参数。

例如，我们可以写:

```
const App = () => {
  return (
    <Router>
      <div>
        <ul>
          <li>
            <Link to="/foo">foo</Link>
          </li>
          <li>
            <Link to="/bar">bar</Link>
          </li>
          <li>
            <Link to="/baz">baz</Link>
          </li>
        </ul> <Switch>
          <Route path="/:id" children={<Child />} />
        </Switch>
      </div>
    </Router>
  );
}function Child() {
  const { id } = useParams(); return (
    <div>
      <h3>ID: {id}</h3>
    </div>
  );
}
```

我们有一个带有`useParams`钩子的`Child`组件。

它让我们得到我们想要的 URL 参数，它是从导航中传递过来的。

它以键值对的形式返回 URL 参数。

键是我们定义的，值是我们导航时传递的。

在`App`中，我们有带路径的`Link`组件。

此外，我们还有`Switch`组件，这些组件具有采用`id` URL 参数的路由。

`Route`有我们通过的路线。`children`有显示的组件。

# 防止 React 组件中的表单提交

有几种方法可以防止 React 组件中的表单提交。

如果表单中有一个按钮，我们可以将按钮的`type`设置为`button`。

例如，我们可以写:

```
<button type="button" onClick={this.onTestClick}>click me</Button>
```

如果我们有一个表单提交处理函数传入了`onSubmit`prop；

```
<form onSubmit={this.onSubmit}>
```

然后在第`onSubmit`个方法中，我们调用`preventDefault`来停止默认提交行为。

例如，我们可以写:

```
onSubmit (event) {
  event.preventDefault();
  //...
}
```

# Match 对象的类型脚本类型

我们可以使用`RouteComponentProps`接口，这是一个提供一些基本成员的通用接口。

如果我们愿意，我们可以传入自己的接口来匹配更多的成员。

例如，我们可以写:

```
import { BrowserRouter as Router, Route, RouteComponentProps } from 'react-router-dom';interface MatchParams {
  name: string;
}interface MatchProps extends RouteComponentProps<MatchParams> {}const App = () => {
  return (
    <Switch>
      <Route 
         path="/products/:name" 
         render={({ match }: MatchProps) => (
            <Product name={match.params.name} 
         /> )} 
      />
    </Switch>
  );
}
```

我们使用我们创建的`MatchProps`接口作为`render` prop 中 props 参数的类型。

然后我们就可以随心所欲的参考`match.params`了。

# 结论

我们可以使用 React Apollo 客户端在 React 组件中进行 GraphQL 查询。

React Router 让我们可以轻松地传递 URL 参数。

它与 JavaScript 和 TypeScript 一起工作。

我们可以用提交处理程序中的`event.preventDefault()`来阻止表单提交。