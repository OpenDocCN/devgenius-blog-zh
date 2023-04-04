# 使用 Fetch 通过 React 发出 POST 请求(基于 Laravel 的 REST API)

> 原文：<https://blog.devgenius.io/using-fetch-post-with-react-and-laravel-api-c99019367f2e?source=collection_archive---------2----------------------->

开始使用 React 的开发人员经常会遇到许多教程和片段，这些教程和片段教他们如何使用全局 fetch API 发出 GET 请求。但当涉及到发布请求时，许多人选择退出第三方库 axios，因此准备了相同的指南/教程/课程。

我自己也面临过这个问题，对于刚刚入门的人来说，决定选择哪一个(axios 或 fetch)可能是一个令人困惑的话题。就个人而言，使用 fetch 进行学习，然后转向 axios 等第三方库是我推荐的，我遇到过许多面临同样问题的开发人员。

这里有一个使用 React 进行 POST 请求的快速指南，使用由 Laravel 生成的 API 端点。

![](img/864084afae6166368f3c6d801ad4bacb.png)

照片由[劳塔罗·安德烈亚尼](https://unsplash.com/@lautaroandreani?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 拍摄

**考虑下面的 api 端点**

```
POST    http://127.0.0.1:8000/api/v1/todos
```

假设上面的端点提供了一个 API 来将一个 Todo 项存储到数据库中，让我们来看看用 Laravel 制作的简单 API。

## API 的定义(拉韦勒)

出于本教程的目的，让我们在 routes/api.php 文件中编写整个 API。

```
Route::post('/v1/todos', function(Request $request){ $validator= $request->validate([
        'title' => 'required|unique:todos',
        'description' => 'required',
       ]); if($validator->fails())
        {
            return response()->json([
                'errors' => $validator->errors()->first()
            ], 422);
        }
        Todo::create($request->all()); return response()->json(['success' => 'Todo Added'],200);});
```

## 在反应中形成组件

现在我们来看看由 todos 表单和 API 实现组成的 react 组件。

**表格:**

```
export default function Todos(){ return (const [title, setTitle] = useState("");
const [description, setDescription] = useState("");
const [errors, setErrors] = useState("");
const history = useNavigate();function handleSubmit(e) {
    const todo = {title, description};
    e.preventDefault(); fetch('http://127.0.0.1:8000/api/v1/todos',{
      method: 'POST',
      headers: {
        "Content-Type" : "application/json",
        "accept" : "application/json"},
         body: JSON.stringify(todo)
       }).then(async response => {
         if (!response.ok) {
         const validation = await response.json();
         setErrors(validation.errors);
         console.log(validation.errors);
       }else{
        history('/categories')
       }
    })
  }<>
<form onSubmit={handleSubmit}><
input 
type="text" 
placeholder="Title" 
value={title} 
onChange={(e) => {
setTitle(e.target.value); 
errors.title=null
}} 
/>{errors && (
 <span>{errors.title}</span>
)}<
input 
type="text" 
placeholder="Description" 
value={description} 
onChange={(e) => {
setDescription(e.target.value); 
errors.description=null
}} 
/>{errors && (
 <span>{errors.description}</span>
)}<button type="submit"> Submit </button></form></>)
```

这里获取的主要部分是上面的 handleSubmit()函数。让我们来看看上述函数的主要代码行。

fetch API 的第一个参数是我们的端点。

```
fetch('http://127.0.0.1:8000/api/v1/todos',{
....
});
```

第二个参数是一个对象，它决定了我们要对这个端点做什么。

与 axios 不同，fetch API 要求我们在发送之前将 javascript 对象转换为 JSON 字符串。这就是我们在 headers 部分的行中所做的:`body: JSON.stringify(todo).`这是作为 JSON 字符串发布的内容，然后由我们的 Laravel API 处理，之后我们处理响应。

因为 fetch()在默认情况下是异步的，所以我们不需要使用 async 关键字来等待通过 Laravel API 发送的验证响应。

如果有任何服务器端错误，`respose.ok`返回 false。

在本教程中，我们只考虑服务器端的验证错误。这可以进一步用适当的响应代码来检查，比如`if(response.status == 422){...}`之类的。但现在，我们会保持干净。

`const validation = await response.json();`接收响应，在我们的例子中，包含验证错误。然后我们使用我们的`errors`状态，并使用`setErrors(validation.errors)`设置错误。fetch() api 还要求我们手动将解析主体文本的结果解析为 json，因此`await response.json()`会保留在我们的代码中。

最后，我们的表单由`span` 标签组成，如果状态中存在错误，就会呈现这些标签。

```
{errors && (
 <span>{errors.title}</span>
)}// And{errors && (
 <span>{errors.description}</span>
)}
```

同样，我们工作代码的主要部分如下:

```
 method: 'POST',
    headers: {
        "Content-Type" : "application/json",
        "accept" : "application/json"},
         body: JSON.stringify(todo)
       }).then(async response => {
         if (!response.ok) {
         const validation = await response.json();
         setErrors(validation.errors);
         console.log(validation.errors);
       }else{
        history('/categories')
       }
```

**结论**

总之，上面的过程需要做大量的工作，但是作为 reactJS 的初学者，这是使用预构建的 fetch() api 用 react 处理 POST 请求的方法。如前所述，可以做更多的工作来改进这里的代码，比如检查正确的响应代码来相应地处理错误，或者使用正确的函数来清除`onchange`上的错误状态，等等。但是为了开始使用 react 中的 fetch()处理 POST 请求，这是合适的。API 也不需要特定于 Laravel。无论是谁在处理 API 端点，都一定会确保验证响应作为异常从服务器端抛出，这是一个很好的实践。因此，如果您对 Laravel 的 API 有一个基本的了解，这可能是最好的工作，但是对任何能够使用 API 和处理响应的人都适用。同样，这里的要点是能够使用现有的 fetch() api，而不是第三方库。

请随意评论，并向我提供反馈，告诉我如何改进这段代码。