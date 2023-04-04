# 当德诺遇到维基数据

> 原文：<https://blog.devgenius.io/when-deno-meets-wikidata-302f8e5b6700?source=collection_archive---------23----------------------->

最近 [Deno 1.0](https://deno.land/v1) 发布了。我使用 Javascript 和 [Node.js](https://nodejs.org/en/) 已经有一段时间了。然而，我对 Typescript 的实验非常有限。Deno 为 Javascript 和 Typescript 提供了一个安全的运行时。这篇文章是关于我用 Deno 和 Typescript 访问 Wikidata 的实验。如果你一直在关注我过去的一些作品，如 [WDProp](https://github.com/johnsamuelwrites/wdprop) 或 [WikiProvenance](https://github.com/johnsamuelwrites/WikiProvenance) ，你可能已经注意到这两个作品都是用普通的 Javascript 开发的，没有任何 Node.js 模块。然而，考虑到 Typescript 的静态类型，我想使用 [Deno](https://deno.land/) 来访问 Wikidata。而且前期开发体验还算顺利。

![](img/8319c1103002b1779e5447afbfa5e1b2.png)

萨姆·欧文在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 拍摄的照片

# 德诺

安装 Deno 很容易，可以在用户环境中本地安装。

```
$ curl -fsSL https://deno.land/x/install/install.sh | sh ######################################################################## 100,0%##O#- # ######################################################################## 100,0% Archive: /home/john/.deno/bin/deno.zip inflating: deno Deno was installed successfully to /home/john/.deno/bin/deno
```

手动将`deno`目录添加到我的`$HOME/.bashrc`之后，我就可以开始使用它了。

```
export DENO_INSTALL="/home/john/.deno" export PATH="$DENO_INSTALL/bin:$PATH"
```

为了确保我的更改得到考虑，我运行了

```
$ source $HOME/.bashrc
```

在撰写本文时，我安装了以下版本的 Deno。

```
 $ deno --version
      deno 1.1.0
      v8 8.4.300
      typescript 3.9.2
```

我的第一个`Hello World`程序是在`hello.ts`文件中编写的

```
console.log("Hello World!")
```

执行它需要`run`选项，我可以看到文件被编译，输出可以在终端上看到。

```
$ deno run hello.ts 
Compile file:///home/john/deno/hello.ts 
Hello World!
```

这样我就可以用`deno`运行我的打字稿程序了。

# 维基数据

我的下一个目标是利用简单的 SPARQL 查询并从 [Wikidata SPARQL 端点](https://query.wikidata.org/)获取数据。例如，下面的 SPARQL 查询将给出来自 Wikidata 的 10 个人。

```
SELECT ?item { ?item wdt:P31 wd:Q5 } LIMIT 10
```

# Deno 和 Wikidata

利用 Wikidata SPARQL 端点，我首先编写了下面的代码`fetch.ts`。

```
 const result = await fetch("https://query.wikidata.org/sparql?query=SELECT%20%3Fitem%20%7B%0A%20%20%3Fitem%20wdt%3AP31%20wd%3AQ5%0A%7D%0ALIMIT%2010&format=json");
   const data = await result.json();
   console.log(data.results)
```

继续上面显示的相同执行风格，然而一个简单的`deno run`给出了以下错误:

```
 $ deno run fetch.ts
     Compile file:///home/john/deno/fetch.ts
     error: Uncaught PermissionDenied: network access to "https://query.wikidata.org/sparql?query=SELECT%20%3Fitem%20%7B%0A%20%20%3Fitem%20wdt%3AP31%20wd%3AQ5%0A%7D%0ALIMIT%2010&format=jsn", run again with the --allow-net flag
        at unwrapResponse ($deno$/ops/dispatch_json.ts:43:11)
        at Object.sendAsync ($deno$/ops/dispatch_json.ts:98:10)
        at async fetch ($deno$/web/fetch.ts:265:27)
        at async file:///home/john/contributions/deno/fetch.ts:1:16
```

这就是`deno`有趣的地方。与 node 不同，deno 需要用户的许可才能连接到互联网。因此选项`--allow-net`需要和`deno run`一起使用来运行上面的文件。

```
$ deno run --allow-net fetch.ts 
      {
        bindings: [
          { item: { type: "uri", value: "http://www.wikidata.org/entity/Q23" } },
          { item: { type: "uri", value: "http://www.wikidata.org/entity/Q42" } },
          { item: { type: "uri", value: "http://www.wikidata.org/entity/Q76" } },
          { item: { type: "uri", value: "http://www.wikidata.org/entity/Q80" } },
          { item: { type: "uri", value: "http://www.wikidata.org/entity/Q91" } },
          { item: { type: "uri", value: "http://www.wikidata.org/entity/Q157" } },
          { item: { type: "uri", value: "http://www.wikidata.org/entity/Q181" } },
          { item: { type: "uri", value: "http://www.wikidata.org/entity/Q185" } },
          { item: { type: "uri", value: "http://www.wikidata.org/entity/Q186" } },
          { item: { type: "uri", value: "http://www.wikidata.org/entity/Q192" } }
        ]
      }
```

下一个目标是利用函数，将调用 Wikidata 和打印结果的逻辑分开。首先，在文件`wdqs.ts`中创建一个函数来查询 Wikidata 并处理适当的响应/错误。请注意，该函数已被声明为异步的，并且还被声明为导出的，即，它可以在其他类型脚本文件中使用。

```
export async function query_wikidata(query: string){
    let queryresults:object = [];
    await fetch(query)
    .then(response => {
      if (!response.ok) {
        throw new Error("Problem in obtaining a response");
      }
      return response.json();
    })
    .then(results => {
      queryresults = results.results.bindings;
    })
    .catch(error => {
      console.error('Error in calling Wikidata API', error);
    });
    return (queryresults);
  }
```

现在，这个函数被导入到另一个文件`query.ts`中。参见导入函数`query_wikidata()`的第一行。在这个文件中，SPARQL 端点的 URL 作为输入传递。并且一旦获得结果，就获得了不同人类的维基数据标识符。

```
 import {query_wikidata} from "./wdqs.ts"

  let promise = query_wikidata("https://query.wikidata.org/sparql?query=SELECT%20%3Fitem%20%7B%0A%20%20%3Fitem%20wdt%3AP31%20wd%3AQ5%0A%7D%0ALIMIT%2010&format=json")
  promise.then(
     (result: any) => {
       for (let row of result) {
         console.log(row.item.value)
       }
     });
```

最后，使用`deno`运行新文件`query.ts`。

```
$ deno run --allow-net query.ts 
  Compile file:///home/john/deno/query.ts
  http://www.wikidata.org/entity/Q23
  http://www.wikidata.org/entity/Q42
  http://www.wikidata.org/entity/Q76
  http://www.wikidata.org/entity/Q80
  http://www.wikidata.org/entity/Q91
  http://www.wikidata.org/entity/Q157
  http://www.wikidata.org/entity/Q181
  http://www.wikidata.org/entity/Q185
  http://www.wikidata.org/entity/Q186
  http://www.wikidata.org/entity/Q192
```

我喜欢`deno` [在运行脚本之前询问用户权限](https://deno.land/manual/getting_started/permissions)的方式。有像`--allow-read`或`--allow-net`这样的标志，确保没有这些标志`deno`不会分别访问本地文件系统和本地环境。当前使用 URL 导入函数的方式(这里不讨论)非常有趣。我想在未来的 Wikidata 相关项目中进一步探索 Typescript 和 Deno。

*最初发布于*[*https://John Samuel . info*](https://johnsamuel.info/en/programming/deno-wikidata.html)*。*