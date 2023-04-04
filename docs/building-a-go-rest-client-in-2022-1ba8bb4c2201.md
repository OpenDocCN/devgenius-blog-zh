# 2022 年打造 Go REST 客户端

> 原文：<https://blog.devgenius.io/building-a-go-rest-client-in-2022-1ba8bb4c2201?source=collection_archive---------1----------------------->

![](img/3cee5d008f9f5fe2a02d997b627edd21.png)

图片由克里斯蒂娜@ wocintechchat.com 提供，来自 [unsplash](https://unsplash.com/photos/glRqyWJgUeY)

现在是 2022 年，RESTful APIs 还在被开发者消费。随着超过 90%的开发人员实现或集成 REST APIss，REST API 实现了 Roy Fielding 的抱负。Go 中泛型的引入简化了 REST API 集成。在这篇文章中，我将向你展示我用 Go 构建类型安全 API 客户端的方法。我将在的 API 中使用 [https://reqres .来获取测试数据。本文中定义的类型代表 reqres 的 API 数据。](https://reqres.in/api/users?page=2)

# 有数据吗？

因为这意味着是一个类型安全的客户端，所以我将从定义我的 API 类型开始。下面是类似于我的 API 将返回的 JSON 的类型:

```
type RequestObj struct {
 TotalPage int       `json:"total"`
 Data      []Profile `json:"data"`
}type Profile struct {
 Avatar string    `json:"avatar"`
 Email string     `json:"email"`
 FirstName string `json:"first_name"`
 LastName string  `json:"last_name"`
}
```

`RequestObj`类似于 API 返回的数据结构。接下来，我将开始定义一个通用函数来处理`GET`请求。该函数将具有以下签名:

```
func Get[T any](ctx context.Context, url string) (T, error)
```

我要添加的第一行代码是`var m T`，这将定义一个通用变量。我将把它用于早期返回，因为我还没有处理任何 JSON。下一行将初始化一个`http.Request`对象。下面的代码将做到这一点:

```
r, err := http.NewRequestWithContext(ctx, "GET", url, nil)
```

我选择用一个上下文来初始化请求对象，以便能够设置请求超时。下一步是实际执行请求。我将使用`http`包的默认客户端。启动请求的代码如下:

```
res, err := http.DefaultClient.Do(r)
```

接下来，我将使用`io`包从响应中读取所有字节。一旦读取完毕，我将把带有泛型类型的字节传递给 Go 的 JSON 解组器。以下是完整的`Get`功能:

```
func Get[T any](ctx context.Context, url string) (T, error) { var m T
  r, err := http.NewRequestWithContext(ctx, "**GET**", url, nil) if err != nil {
    return m, err
  } res, err := http.DefaultClient.Do(r) if err != nil {
    return m, err
  } body, err := io.ReadAll(res.Body)
  res.Body.Close() if err != nil {
    return m, err
  } return parseJSON[T](body)
}func parseJSON[T any](s []byte) (T, error) {
  var r T if err := json.Unmarshal(s, &r); err != nil {
    return r, err
  } return r, nil
}
```

现在我有了所有这些简洁的代码，是时候实现它了。下面是上面定义的客户端的一个实现。这将获取数据，并将第一个条目记录到控制台:

```
package mainimport (
 "context"
 "fmt"
 "log"
 "time"
)func main() { ctx := context.Background()
  timeout := 30 * time.Second reqContext, _ := context.WithTimeout(ctx, timeout)
  m, err := Get[RequestObj](reqContext, "[https://reqres.in/api/users?page=2](https://reqres.in/api/users?page=2)") if err != nil {
   log.Fatal(err)
  } fmt.Println(m.Data[0])}
```

下面是实际运行的代码:

![](img/5204be17c0c166ba9d880f0e3e80805a.png)

下一步将是处理张贴数据。

# 制作数据

我将首先定义我将用于 POST 请求的类型。下面是定义类型的代码:

```
type User struct {
  Name string         `json:"name,omitempty"`
  Job string          `json:"job,omitempty"`
  ID string           `json:"id,omitempty"`
  CreatedAt time.Time `json:"createdAt,omitempty"` 
}
```

如果你注意到了，我大量使用了`omitempty`，这将阻止向 API 服务器发送默认字段数据。接下来，我将定义向 API 发送数据的通用函数。该函数将具有以下签名:

```
func Post[T any](ctx context.Context, url string, data any) (T, error)
```

`Post`函数将类似于`Get`，除了它将初始化一个`io`阅读器并为请求设置一个报头。因此，我将介绍这两个过程。我还需要将我的类型转换成一个字节数组。下面是将执行所述任务的函数:

```
func toJSON(T any) ([]byte, error) {
 return json.Marshal(T)
}
```

回到`Post`函数，我将用我的类型生成的字节初始化一个`io`读取器。我将使用以下代码执行此任务:

```
b,_ := toJSON(data)
...byteReader := bytes.NewReader(b)
```

现在我有了一个`io`阅读器，我将用它初始化一个请求:

```
r, err := http.NewRequestWithContext(ctx, "POST", url, byteReader)
```

一旦请求被初始化，我将为请求添加一个头。这将告诉 REST 服务器如何理解我发送的数据。

```
r.Header.Add("Content-Type", "application/json")
```

下面是完整的`Post`功能:

```
func Post[T any](ctx context.Context, url string, data any) (T, error) { var m T **b, err := toJSON(data)** if err != nil {
    return m, err
  } **byteReader** := bytes.NewReader(**b**) r, err := http.NewRequestWithContext(ctx, "**POST**", url, **byteReader**) if err != nil {
    return m, err
  } // Important to set
 ** r.Header.Add("Content-Type", "application/json")** res, err := http.DefaultClient.Do(r) if err != nil {
   return m, err
  } body, err := io.ReadAll(res.Body)
  res.Body.Close() if err != nil {
    return m, err
  } return parseJSON[T](body)
}
```

下面是函数`Post`的一个实现:

```
package mainimport (
 "context"
 "fmt"
 "log"
 "time"
)func main() { ctx := context.Background()
 timeout := 30 * time.Second
 // Post data
 **user** := **User**{ Name : "morpheus", Job : "leader"} addContext, _ := context.WithTimeout(ctx,  timeout) **newUser**, err := Post[**User**](addContext, "[https://reqres.in/api/users](https://reqres.in/api/users)", **user**) if err != nil {
   log.Fatal(err)
 } fmt.Println( **newUser** )}
```

下面是运行中的代码，您还可以看到服务器生成的附加数据:

![](img/da5d3d3027df9e80975dc08d293f38bb.png)

# 结论

泛型是一个很好的补充。它使我能够为我的 API 客户端增加流动性。从某种意义上说，我可以用一行代码检索数据并将其断言为一种类型；).它还消除了定义变量并传递它来模拟一般行为的过时过程。我没有提到的一个重要的提醒是:不要忘记检查 HTTP 错误代码！

# 附加说明

[](https://github.com/cheikhshift/medium_examples/tree/main/http-client) [## medium _ examples/http-client at main cheikh shift/medium _ examples

### 中型文章的代码示例。在 GitHub 上创建一个帐户，为 cheikhshift/medium_examples 开发做贡献。

github.com](https://github.com/cheikhshift/medium_examples/tree/main/http-client) [](https://nordicapis.com/20-impressive-api-economy-statistics/) [## 20 项令人印象深刻的 API 经济统计|北欧 APIs |

### 科技发展很快。它还使用自己的技术术语，这些术语发展很快。可能很难跟上…

nordicapis.com](https://nordicapis.com/20-impressive-api-economy-statistics/)