# 如何在 Nx workspace 中创建一个工作空间生成器作为库？

> 原文：<https://blog.devgenius.io/how-to-create-a-workspace-generator-as-a-library-in-nx-workspace-1b312c88b543?source=collection_archive---------6----------------------->

![](img/5a10cfee59008c89f1f119d50983dcaf.png)

有序的 Nx 工作空间将帮助您完成更多任务。*照片由* [*卡尔·海尔达尔*](https://unsplash.com/@carlheyerdahl?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) *上*[*Unsplash*](https://unsplash.com/s/photos/workspace-generator?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)

如何创建 Nx 生成器？如何在你的 Nx 工作区使用？我们如何将工作区生成器转换成可发布的库？无聊怎么会对你有好处呢？

Nx 是一个强大的 monorepo 管理工具。它帮助您利用最强大的 monorepo 优势(IMO)之一—标准。我所说的标准是指——无论你开发什么，或者你来自哪个团队，作为一名开发人员，你都会有宾至如归的感觉。

# 标准优势或:为什么无聊是好的？

为了运行库的构建过程，开发人员可以在 cli 中键入以下内容:

`npx nx run myLibrary:build`

如果所述开发者想要运行测试？

`npx nx run myLibrary:test`

但是如果这个开发者想要运行超级服务器呢？

`npx nx run mySuperDuperServer:serve`

如果我想用 docker goodies 为生产构建它呢？

`npx nx run mySuperDuperServer:build`

以及如何测试所述服务器？

`npx nx run mySuperDuperServer:test`

这很无聊吧？您可以用同样的方式构建、测试和服务。即使运行`e2e`测试看起来也是这样:

`npx nx run components:e2e`

无聊！

但这就是标准的力量。假设一个开发者从`components`团队到`super duper server`团队？很简单！无需解释基础设施、安装等。只要记住`npx`、`nx run`、你正在工作的组件的名字和你想要运行的目标(e2e/服务/构建/测试等等)就行了。).

你现在明白为什么无聊是好的了吧？这就是标准的力量。当开发人员需要记住不太麻烦的事情时，他们的工作效率会更高。现在我们可以专注于最重要的事情:)

# 什么是工作空间生成器？

如构建/测试/e2e 等。执行者，我们也有发电机。发电机是让我们产生(嗯…啊！！！)来自模板的新代码片段。

假设您正在创建 ui 组件或可注入服务或角度模块…您不希望总是从头开始编写相同的样板文件。或者更糟的是…从不同的文件夹复制并手动更改文件名、类名、测试名、观察名…

而且，为了让它变得无趣，我们希望所有东西都有相同的语法:

`npx nx generate @nrwl/angular:library`

这将使用预先构建的 nrwl angular 插件生成一个 angular 库。如果我们有自己的需求，想要自己的发电机呢？进入工作区生成器！

使用工作区生成器，您可以构建自己的生成器，并像这样运行它们:

`npx nx workspace-generator vivid-component my-component`

这看起来有点像，但不完全一样。为什么？因为`workspace generator`是一个单独的命令。因此，您正在运行一个不同的命令，该命令运行一个不属于插件的生成器。这打破了我们的无聊(例如，我们有另一个模式要记住)。为什么我们不能:

`npx nx g @vonage/nx-vivid:component my-component`

嗯……我们可以，只是现在它不是一个工作区生成器——它是一个在我们的 Nx 工作区内使用的 Nx 插件！

# 如何在 Nx 工作区内部构建一个 Nx 插件？

这是容易的部分。我们开始吧。

1.  将 Nx 插件库添加到工作区:`npm i -D @nrwl/nx-plugin`
2.  在工作区生成插件库:`npx nx g @nrwl/nx-plugin:plugin nx-vivid --import-path @vonage/nx-vivid`

这两个步骤产生了一个名为`nx-vivid`的新库，它包含一个存根生成器和一个存根执行器。它还为发电机增加了一个`e2e`测试。`e2e`测试帮助您测试通常由生成器完成的文件操作。

现在剩下的就是实际编写插件了。

# an Nx 插件怎么写？

就像编写任何其他代码一样。先看看我们要做什么。在我们的存储库中，我们有一个保存 UI 组件库的`components`库。

我们所有的组件都存在于`src/lib`文件夹中。

它们看起来都差不多，或者至少开始是一样的:

1.  索引条目文件
2.  基类文件
3.  模板填充
4.  `scss`文件
5.  自述文件
6.  UI 测试文件

他们的内容也相当重复。因为我是一个`test first`类型的人，所以让我们从`e2e`测试开始。

# 写 Nx 插件 E2E

`Nx`有很多实用程序来编写和测试插件。因此，测试非常简单:

```
import {
  checkFilesExist,
  ensureNxProject,
  readJson,
  runNxCommandAsync,
  uniq,
} from '@nrwl/nx-plugin/testing';describe('nx-vivid e2e', () => {
  beforeAll(() => {
    ensureNxProject('@vonage/nx-vivid', 'dist/libs/nx-vivid');
  }); afterAll(() => {
    runNxCommandAsync('reset');
  }); describe('--directory', () => {
    it('should create src in the specified directory', async () => {
      const project = uniq('nx-vivid');
      await runNxCommandAsync(
        `generate @vonage/nx-vivid:component ${project}`
      );
      expect(() =>
        checkFilesExist(`libs/components/src/lib/${project}/index.ts`)
      ).not.toThrow();
      expect(() =>
        checkFilesExist(`libs/components/src/lib/${project}/ui.test.ts`)
      ).not.toThrow();
      expect(() =>
        checkFilesExist(`libs/components/src/lib/${project}/README.md`)
      ).not.toThrow();
      expect(() =>
        checkFilesExist(`libs/components/src/lib/${project}/${project}.ts`)
      ).not.toThrow();
      expect(() =>
        checkFilesExist(`libs/components/src/lib/${project}/${project}.template.ts`)
      ).not.toThrow();
      expect(() =>
        checkFilesExist(`libs/components/src/lib/${project}/${project}.spec.ts`)
      ).not.toThrow();
      expect(() =>
        checkFilesExist(`libs/components/src/lib/${project}/${project}.scss`)
      ).not.toThrow();
    }, 120000);
  });
});
```

事情是这样的:

1.  上面的代码启动了一个新的 Nx 工作空间(`ensureNxProject('@vonage/nx-vivid', 'dist/libs/nx-vivid');`)。
2.  然后它运行我们想要的命令:
    `await runNxCommandAsync( generate @vonage/nx-vivid:component ${project} );`。相当于跑了`npx nx generate @vonage/nx-vivid:component my-project`，这就是我们想要的！
3.  之后，它希望在库的文件夹中生成所有文件。

相当简单！

[这是对那个](https://github.com/Vonage/vivid-3/pull/393/commits/a0dc62a0ab362b653d788aa771cecc15202c6645)的提交

你能猜到如何运行 e2e 测试吗？准备好你无聊的哈欠:`npx nx run nx-vivid-e2e:e2e`。测试失败是因为我们还没有设置组件的生成器！

# 向 Nx 插件添加生成器

发电机由三个主要部分组成:

1.  模式-生成器输入的模式
2.  模板文件——带有占位符的文件，将被复制和操作到一个完整的工作库/应用程序/组件/无论什么
3.  实际的逻辑和它的测试文件(在我们的例子中是`index.ts`和`index.spec.ts`)文件——我们告诉什么应该去哪里。

如果您想在逻辑文件中进行类型检查，模式是一个`json`文件和一个可选的`d.ts`文件。

```
{
  "$schema": "http://json-schema.org/schema",
  "cli": "nx",
  "$id": "vivid-component",
  "type": "object",
  "properties": {
    "name": {
      "type": "string",
      "description": "Component name",
      "$default": {
        "$source": "argv",
        "index": 0
      }
    }
  },
  "required": ["name"]
}
```

这是它看起来的样子。有许多类型的属性，`Nx`机制还允许您向消费者提问(比如使用`express`或`nestjs`)。你可以在[的 Nx 文档中了解更多。](https://nx.dev/generators/generator-options)

模板文件就是您期望在输出中看到的内容——带有占位符。例如，您在文件名和文件夹名中设置`__fileName__`作为组件相关文件名的占位符。在文件中，你使用标签如`{<%= className %>}`或`<%= name %>`作为动态属性的占位符。

最后，`logic`文件将它们全部绑定。它导出一个异步函数，在调用生成器时使用:

```
import {
  Tree,
  formatFiles,
  names,
  joinPathFragments,
  getWorkspaceLayout,
  generateFiles, offsetFromRoot
} from '@nrwl/devkit';
import {VividComponentGeneratorOptions} from "./schema";
import {join} from "path";export interface NormalizedSchema extends VividComponentGeneratorOptions {
  fileName: string;
  className: string;
  projectRoot: string;
}function normalizeOptions(tree: Tree, options: VividComponentGeneratorOptions): NormalizedSchema {
  const projectDirectory = names(options.name).fileName;
  const className = names(options.name).className; const name = projectDirectory.replace(new RegExp('/', 'g'), '-');
  const fileName = names(projectDirectory).fileName; const { libsDir, npmScope } = getWorkspaceLayout(tree); const projectRoot = joinPathFragments(libsDir, 'components/src/lib', projectDirectory); return {
    ...options,
    fileName,
    name,
    className,
    projectRoot
  };
}function createFiles(tree: Tree, options: NormalizedSchema) {
  const {className, name, propertyName} = names(options.name); generateFiles(tree, join(__dirname, './files'), options.projectRoot, {
    ...options,
    dot: '.',
    className,
    name,
    propertyName,
    cliCommand: 'nx',
    strict: undefined,
    tmpl: '',
    offsetFromRoot: offsetFromRoot(options.projectRoot)
  });
}export default async function vividComponentGenerator(tree: Tree, schema: VividComponentGeneratorOptions) {
  const options = normalizeOptions(tree, schema);
  createFiles(tree, options);
  await formatFiles(tree);
}
```

在上面的例子中，函数`vividComponentGenerator`被导出。它处理从用户那里收到的选项，创建文件，然后运行`formatFiles`...格式化文件(主要是林挺)。

最后，我们需要告诉插件生成器的存在以及如何到达它。这是在主插件的`generators.json`文件中完成的:

```
{
  "$schema": "http://json-schema.org/schema",
  "name": "nx-vivid",
  "version": "0.0.1",
  "generators": {
    "component": {
      "factory": "./src/generators/component/index",
      "schema": "./src/generators/component/schema.json",
      "description": "nx-vivid component generator"
    }
  }
}
```

添加完所有这些之后，`e2e`测试通过。

[查看本部分](https://github.com/Vonage/vivid-3/pull/393/commits/300128442bf4025f1ce9a175bcdeb0998aa9aebd#diff-a90080815abcb99f4ed3dbf945b04e2975d952c14972536df29691a87196eb68)提交的代码。

# 测试你的 Nx 插件

我们看到了插件的`e2e`测试。注意，我还写了`unit tests`(在[提交](https://github.com/Vonage/vivid-3/pull/393/commits/300128442bf4025f1ce9a175bcdeb0998aa9aebd#diff-a90080815abcb99f4ed3dbf945b04e2975d952c14972536df29691a87196eb68)中)。我实际上是在写真正的代码之前写的。虽然 [TDD](https://yonatankra.com/5-tdd-lessons-when-writing-javascript-algorithm/) 超出了本文的范围，但是让我们来谈谈插件的测试。

在这种情况下，单元测试做的事情和`e2e`测试做的事情是一样的——当我们给出一个特定的输入(例如组件的名字)时，它们确保生成正确的文件:

在上面的代码片段中，我们使用 Nx devkit 来生成工作区的虚拟文件树。然后，我们用所需的选项运行生成器。我们希望生成的树包含替换了占位符的模板文件。

测试的经验法则是这样的:如果你可以用单元测试和 E2E 覆盖同样的事情，那么最好用单元测试。他们要快得多。就这么简单。

那为什么 Nx 给我们设置了一个 E2E 插件基础设施呢？在与 Nrwl[的](https://nrwl.io/) [Craigory](https://twitter.com/EnderAgent) 交谈时，他给出了一个相当肯定的答案:

![](img/f73ddf4caf287642e2f4ae575e140b01.png)

这个答案非常适合喜欢经验法则的人:

1.  它支持我们的经验法则“更多单位—更少 e2e”
2.  我给出了另一个针对 Nx 插件的经验法则——“e2e 是执行者——单元是生成器”

# 摘要

标准很重要。它们有助于开发人员体验、可伸缩性、敏捷性，我可能忘记了标准的一些好处。

`Nx`允许您利用标准，不仅如此，其机制还迫使您标准化工作空间。

我们从 Lerna 迁移到 Nx，祝福这一天。一切都是标准化的。甚至定制的生成器、linters 和其他对所有开发者来说看起来都是一样的。处理文档的开发人员使用与处理组件或任何其他部分的开发人员相同的命令语法。甚至生成器插件本身也有同样的开发者体验。

我没有提到您可以获得的其他好处，如依赖图、几乎所有东西的模拟运行、并行执行、缓存、云缓存等等。

我们将何去何从？这个生成器用于在我们自己的库中生成内部代码片段。由于一些构建限制，这是我们的第一个设计。我们相信我们克服了这些限制，并打算从库中提取组件——因此每个组件都将“独立”存在。

这意味着，我们的生成器将为组件生成完整的库，而不是在现有的库中生成代码片段。或者更好的是，我们可以组成一个`component library`发电机，在引擎盖下使用我们的`component generator`。

非常感谢来自 [Nrwl](https://nrwl.io/) 的 [Craigory](https://twitter.com/EnderAgent) 的精彩评论和讨论！