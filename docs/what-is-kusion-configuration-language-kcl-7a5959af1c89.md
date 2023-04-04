# 什么是 Kusion 配置语言(KCL)

> 原文：<https://blog.devgenius.io/what-is-kusion-configuration-language-kcl-7a5959af1c89?source=collection_archive---------14----------------------->

![](img/a863157850914912064dfe330e88553e.png)

# KCL 是什么？[](https://kcl-lang.github.io/docs/user_docs/getting-started/intro#what-is-kcl)

[Kusion 配置语言(KCL)](https://github.com/KusionStack/KCLVM) 是一种开源的基于约束的记录和函数语言。KCL 通过成熟的编程语言技术和实践，改进大量复杂配置的编写，致力于围绕配置构建更好的模块化、可扩展性和稳定性，逻辑编写更简单，自动化速度快，生态扩展性好。

# 什么是配置？[](https://kcl-lang.github.io/docs/user_docs/getting-started/intro#what-is-configuration)

当我们部署软件系统时，我们不认为它们是固定的。不断发展的业务需求、基础设施需求和其他因素意味着系统在不断变化。当我们需要快速改变系统行为，并且改变过程需要昂贵且冗长的重构和重新部署过程时，业务代码的改变往往是不够的。配置可以为我们提供一种低开销的方式来改变系统功能。例如，我们经常为我们的系统配置编写如下所示的 JSON 或 YAML 文件。

*   JSON 配置

```
{
    "server": {
        "addr": "127.0.0.1",
        "listen": 4545
    },
    "database": {
        "enabled": true,
        "ports": [
            8000,
            8001,
            8002
        ],
    }
}
```

复制

*   YAML 构型

```
server:
  addr: 127.0.0.1
  listen: 4545
database:
  enabled: true
  ports:
  - 8000
  - 8001
  - 8002
```

复制

我们可以根据需要选择将静态配置存储在 JSON 和 YAML 文件中。此外，配置还可以用高级语言存储，这允许更灵活的配置，可以对其进行编码、呈现和静态配置。KCL 就是这样一种配置语言。我们可以编写 KCL 代码来生成 JSON/YAML 和其他配置。

# 为什么要发展 KCL？[](https://kcl-lang.github.io/docs/user_docs/getting-started/intro#why-develop-kcl)

除了一般配置，云原生配置的特点还包括量大面广。例如，Kubernetes 提供了一种声明式应用程序编程接口(API)机制，其开放性允许用户充分利用其资源管理功能；然而，这也意味着容易出错的行为。

*   Kubernetes 配置缺乏用户端验证方法，无法检查数据的有效性。
*   Kubernetes 公开了 500 多个模型，2000 多个字段，并允许用户自定义模型，而无需考虑多个站点、多个环境和多个部署拓扑的配置重用。碎片化配置给大规模配置的协同编写和自动化管理带来很多困难。

云原生社区已经做了大量的尝试来改进他们的配置技术，这些技术可以分为三类:

*   用于模板、补丁和验证的基于低级数据格式的工具，这些工具使用外部工具来增强重用和验证。
*   领域特定语言(DSL)和配置语言(CLs)来增强语言能力。
*   基于通用语言(GPL)的解决方案，使用 GPL 的云开发工具包(CDK)或框架来定义配置。

以前的努力不能满足所有这些需求。一些工具基于 Kubernetes API 来验证配置。虽然它支持检查丢失的属性，但这种验证通常很弱，并且仅限于开放应用编程接口(OpenAPI)。一些工具支持定制的验证规则，但是规则描述很麻烦。在配置语言方面，专注于减少样板文件，只有少数专注于类型检查、数据验证、测试等。

Helm 使用参数化模板技术来解决动态配置问题。随着规模的增加，参数化模板往往变得复杂和难以维护；必须手动识别参数替换位置。但它繁琐且容易出错，参数会逐渐侵蚀模板，模板中的任何值都可能逐渐演变成参数。与直接使用 Kubernetes API 相比，这种结合了许多参数的模板的可读性往往更差。Kustomize 使用代码修补来实现多环境配置代码的重用。

# 为什么要用 KCL？[](https://kcl-lang.github.io/docs/user_docs/getting-started/intro#why-use-kcl)

KCL 是一种现代高级领域语言，是一种编译型、静态、强类型语言。它为开发人员提供了通过记录和函数式语言设计来编写配置(config)、建模抽象(schema)、业务逻辑(lambda)和环境策略(rule)作为核心元素的能力。

KCL 试图提供独立于运行时的可编程性，并不原生提供线程、IO 等系统功能，而是支持面向云原生运行场景的功能，试图为解决领域问题提供稳定、安全、低噪声、低副作用、易自动化、易治理的编程支持。

与用 GPL 编写的客户端运行时不同，KCL 程序通常运行并生成底层数据，并集成到客户端运行时访问工具中，这些工具可以通过在推送到运行时之前单独测试和验证 KCL 代码来提供左移稳定性保证。KCL 代码也可以编译成 wasm 模块，经过全面测试后，可以集成到服务器运行时中。

*   易用性:源于 Python、Golang 等高级语言，融入了低副作用的函数式语言特性。
*   设计良好:独立的规范驱动的语法、语义、运行时和系统模块设计。
*   快速建模:[模式](https://kcl-lang.github.io/docs/reference/lang/tour#schema)-以配置类型和模块化抽象为中心。
*   丰富的功能:基于[配置](https://kcl-lang.github.io/docs/reference/lang/codelab/simple)、[模式](https://kcl-lang.github.io/docs/reference/lang/tour/#schema)、[λ](https://kcl-lang.github.io/docs/reference/lang/tour/#function)、[规则](https://kcl-lang.github.io/docs/reference/lang/tour/#rule)的类型、逻辑和策略配置。
*   稳定性:基于[静态类型系统](https://kcl-lang.github.io/docs/reference/lang/tour/#type-system)、[约束](https://kcl-lang.github.io/docs/reference/lang/tour/#validation)和[规则](https://kcl-lang.github.io/docs/reference/lang/tour#rule)的配置稳定性。
*   可扩展性:通过隔离配置块的[自动合并机制](https://kcl-lang.github.io/docs/reference/lang/tour/#-operators-1)实现高可扩展性。
*   快速自动化:[CRUD API](https://kcl-lang.github.io/docs/reference/lang/tour/#kcl-cli-variable-override)、[多语言 SDK](https://kcl-lang.github.io/docs/reference/xlang-api/overview)、[语言插件](https://kcl-lang.github.io/docs/reference/plugin/overview)的梯度自动化方案
*   高性能:使用 Rust & C 和 [LLVM](https://llvm.org/) 的高编译时间和运行时性能，支持编译到原生代码和 [WASM](https://webassembly.org/) 。
*   API 亲缘性:原生支持 API 生态规范，如 [OpenAPI](https://kcl-lang.github.io/docs/tools/cli/openapi/) 、Kubernetes CRD、Kubernetes YAML 规范。
*   开发友好:友好的开发体验，丰富的语言工具(格式、Lint、测试、Vet、Doc 等)。)和 [IDE 插件](https://kcl-lang.github.io/docs/tools/Ide/)。
*   安全性和可维护性:面向领域，无系统级功能，如本机线程和 IO，低噪音和安全风险，易于维护和治理。
*   生产就绪:广泛应用于蚂蚁集团平台工程和自动化的生产实践中。

有关更多语言设计和功能，请参见 [KCL 文档](https://kcl-lang.github.io/docs/reference/lang/tour)。KCL 虽然不是通用语言，但是有相应的应用场景。开发者可以通过 KCL 编写 config、schema、function 和 rule，其中 config 用来定义数据，schema 用来描述数据的模型定义，rule 用来验证数据，schema 和 rule 也可以结合使用充分描述数据的模型和约束，另外我们还可以使用 KCL 中的 lambda pure 函数来组织数据代码，封装常用代码，需要时直接调用。

# KCL 是干什么用的？[](https://kcl-lang.github.io/docs/user_docs/getting-started/intro#what-is-kcl-for)

您可以使用 KCL 来

*   生成低级静态配置数据，如 JSON、YAML 等。
*   使用模式建模减少配置数据中的样板文件。
*   为配置数据定义带有规则约束的模式，并自动验证它们。
*   无副作用地组织、简化、统一和管理大型配置。
*   利用隔离的配置模块可扩展地管理大型配置。
*   用作平台工程语言，通过 [KusionStack](https://kusionstack.io/) 交付现代应用。

通过 KCL 编译器、语言工具、IDE 和多语言 API，您可以在以下情况下使用 KCL:

*   配置和自动化:抽象和管理不同规模的配置，包括小规模配置(应用程序、网络、微服务、数据库、监控、CI/CD 管道等。)，大规模云原生 kubernetes 配置和自动化。此外，通过 [KCL OpenAPI 工具](https://kcl-lang.github.io/docs/tools/cli/openapi/)和 KCL 的包管理能力，可以充分抽象和重用现有模型。
*   安全性和合规性:利用 KCL 动态参数的能力，使用代码来定义、更新、共享和执行策略。通过利用基于 KCL 代码的自动化来管理策略，而不是依赖手动过程，这允许团队更快地移动，并减少由于人为错误而导致的错误的可能性。
*   意图描述:KCL 可以用来描述工具、脚本和工作流，它访问一个定制的引擎来消费和执行意图。

# 如何选择？[](https://kcl-lang.github.io/docs/user_docs/getting-started/intro#how-to-choose)

简单的答案是:

*   如果需要编写结构化的键值对，或者使用 Kubernetes 的原生工具，推荐使用 YAML/JSON/Kustomize/Helm。
*   如果您想使用编程语言的便利性来删除具有良好可读性的样板文件，或者如果您已经是 Terraform 用户，那么建议使用 HCL。
*   如果您希望使用类型系统来提高稳定性和维护可伸缩的配置，建议使用 CUE。
*   如果您想要像现代语言一样的类型和建模、可伸缩的配置、内部纯函数和规则，以及生产就绪的性能和自动化，那么推荐使用 KCL。

# vs YAML/JSON

YAML/JSON 配置适用于小规模的配置场景。对于需要频繁修改的大规模云原生配置场景，更适合 KCL。涉及的主要区别是配置数据抽象和部署之间的区别:

使用 KCL 进行配置的优势在于:对于静态数据，抽象一层的优势意味着整体系统具有部署灵活性。不同的配置环境、租户和运行时可能对静态数据有不同的要求，甚至不同的组织可能有不同的规范和产品要求。KCL 可用于向用户公开最需要和最频繁修改的配置。

# vs . Jsonnet/GCL[](https://kcl-lang.github.io/docs/user_docs/getting-started/intro#vs-jsonnetgcl)

GCL 是一种用 Python 实现的声明式配置编程语言，提供了支持模板抽象的必要语言能力。但是，编译器本身是用 Python 写的，语言本身是解释和执行的。对于大型模板实例(如 kubernetes 模型)，性能很差。

Jsonnet 是一种用 C++实现的数据模板语言，适合应用程序和工具开发人员，可以生成配置数据并组织、简化和管理大型配置，并且没有副作用。

Jsonnet 和 GCL 非常擅长减少样板文件。它们都可以使用代码来生成配置，就像工程师只需要编写高级的 GPL 代码，而不是手动编写容易出错且难以理解的服务器二进制代码。Jsonnet 减少了 GCL 的一些复杂性，但在很大程度上属于同一类别。两者都有许多运行时错误，类型检查和约束能力不足。

# vs . HCL[](https://kcl-lang.github.io/docs/user_docs/getting-started/intro#vs-hcl)

HCL 是一种 Go 实现结构化配置语言。HCL 的原生语法受 libucl 和 nginx 配置的启发。用于创建一种对人和机器友好的结构化配置语言，主要用于 devops 工具、服务器配置、资源配置作为 [Terraform 语言](https://www.terraform.io/language)。

HCL 与 GCL 有一些惊人的相似之处。它确实引入了穷人版本的继承:文件覆盖。字段可以在多个文件中定义，这些文件以文件名的特定顺序被覆盖。虽然没有 GCL 那么复杂，但它也有一些相同的问题。模式是固定的，能力是有限的。

HCL 的用户界面不是通过 Terraform provider 模式定义直接感知的。此外，当编写复杂的对象和必需/可选字段定义时，用户界面很麻烦。动态参数受变量的条件字段约束。资源本身的约束需要由提供者模式定义，或者与 Sentinel/减压阀和其他策略语言相结合。语言本身的完整性不能自我封闭，其实现方法也不统一。

# vs . CUE[](https://kcl-lang.github.io/docs/user_docs/getting-started/intro#vs-cue)

CUE 可以通过 struct、无继承等特性作为建模，在模型定义之间没有冲突的情况下可以实现高度抽象。因为 CUE 在运行时执行所有约束检查，所以在大规模建模场景中可能会出现性能瓶颈。CUE 将类型和值组合成一个概念。它通过各种语法简化了约束的编写。例如，泛型类型和枚举不是必需的。求和类型和空值合并是一回事。CUE 支持配置合并，但它是完全幂等的。它可能无法满足复杂的多租户和多环境配置场景的要求。对于复杂的循环和约束场景，编写起来很复杂，编写需要精确配置修改的场景也很繁琐。

对于 KCL，通过 KCL schema 进行建模，通过语言级工程和一些面向对象的特性(比如单继承)可以实现高模型抽象。KCL 是一种静态编译语言，对于大规模建模场景开销很低。KCL 提供了更丰富的检查声明性约束语法，这使得编写起来更容易。对于一些配置字段组合约束，编写更简单(与 CUE 相比，KCL 提供了更多的 if guard 组合约束、all/any/map/filter 等集合约束编写方法，更容易编写)。

# vs. Kustomize[](https://kcl-lang.github.io/docs/user_docs/getting-started/intro#vs-kustomize)

Kustomize 的核心能力是它的文件级覆盖能力。但是有一个多重覆盖链的问题，因为找到一个特定属性值的语句并不能保证它就是最终值，因为在别处出现的另一个特定值可以覆盖它。对于复杂的场景，检索 Kustomize 文件的继承链通常不如检索 KCL 代码的继承链方便，需要仔细考虑指定的配置文件覆盖顺序。此外，Kustomize 无法解决 YAML 配置编写、约束验证、模型抽象和开发等问题，更适合简单配置场景。

在 KCL 中，配置合并操作可以细化到代码中的每个配置属性，合并策略可以灵活设置，不局限于整体资源，通过 KCL 的 import 语句可以静态分析配置之间的依赖关系。

# vs 赫尔姆[](https://kcl-lang.github.io/docs/user_docs/getting-started/intro#vs-helm)

Helm 的概念源于操作系统的包管理机制。它是一个基于模板化 YAML 文件的包管理工具，支持包中资源的执行和管理。

KCL 自然地提供了一个超集的头盔功能，这样你就可以直接使用 KCL 作为替代。对于已经采用 Helm 的用户，可以将 KCL 中的栈编译结果打包成 Helm 格式使用。

# vs CDK[](https://kcl-lang.github.io/docs/user_docs/getting-started/intro#vs-cdk)

用 CDK 的高级语言编写可以很好地集成到应用程序项目中，它实际上是客户端运行时的一部分。对于 KCL，由 KCL 编写的外部配置和策略与客户机运行时相分离。

一般的语言通常是矫枉过正，即远远超出了需要解决的问题。通用语言存在各种安全问题，比如能力边界问题(启动本地线程、访问 IO、网络、代码无限循环等安全风险)。例如，在音乐领域，有专门的音符来表达音乐，便于学习和交流，这是用一般语言无法表达清楚的。

此外，由于其通用语言风格多样，因而具有统一维护、管理和自动化的成本。通用语言通常用于编写客户端运行时，它是服务器运行时的延续。不适合写独立于运行时的配置，编译成二进制，最后从进程启动。此外，稳定性和可扩展性不容易控制。然而，配置语言经常被用来写数据，它与简单的逻辑相结合，它描述了预期的最终结果，然后由编译器或引擎使用。

# 对比 OPA/减压阀[](https://kcl-lang.github.io/docs/user_docs/getting-started/intro#vs-oparego)

尽管减压阀不是作为数据定义语言设计的，但这种用于开放策略代理(OPA)的语言也解决了能够从多个来源添加约束的问题。

减压阀植根于逻辑编程。它基于 Datalog，一种 Prolog 的受限形式，而 KCL 基于静态类型结构。类型化特征结构是为了解决 Prolog 在人类语言编码应用中的缺点而设计的。将 Datalog 变体用于本质上是约束验证的任务有点奇怪。Datalog 是一种优秀的查询语言。但是对于强制约束来说，这有点麻烦，因为首先需要有效地查询要应用约束的值。

此外，KCL 的方法更容易找到约束的规范化和简化表示，这使得它更适合于为从 OpenAPI 生成的创建 OpenAPI。

KCL 网站是

[](https://kcl-lang.github.io) [## 抽象验证生产就绪-抽象验证生产就绪|抽象…

### KCL 是一种开源的基于约束的记录和函数语言，主要用于配置和策略场景。

kcl-lang.github.io](https://kcl-lang.github.io) 

如果你喜欢 KCL，在 Github 上给我一颗星吧。谢谢大家！

https://github.com/KusionStack/KCLVM