# 使用开放策略代理(OPA)构建更好的策略代码

> 原文：<https://blog.devgenius.io/building-with-open-policy-agent-opa-for-better-policy-as-code-53c544cc2031?source=collection_archive---------10----------------------->

当我们听到术语“作为代码的策略”时，它在很大程度上与在组织内大规模应用策略的实施方面有关。无论是在 GitOps 环境中，还是像[开放策略代理](https://www.openpolicyagent.org/)(又名 OPA)这样的流行开源项目中，应用全球策略都是最终目标。

然而，OPA 作为一个开源项目，其功能是非常独特的——有一整套预实施工具，为我们的系统提供了长期被忽视的难以置信的洞察力。由于抽象和自动化层的存在，云世界中出现了新一波基础设施漂移。当构建 [Firefly](https://gofirefly.io/) 时，我们将 OPA 的力量作为一个策略发现引擎，而不仅仅是一个执行机制。

这篇文章将深入探讨一种新的思考方式，即在大规模设计高度分布式云系统时，将策略视为代码。它将展示 OPA 如何不仅仅用于策略的实施，还用于深入了解您的系统及其资源利用率，并最终根据这些宝贵的信息定制您想要应用的策略。

![](img/3b9b5b1d6b7b76bdfe9171a7fbcd4c2a.png)

# 什么是 OPA，它是如何工作的？

对于那些熟悉 Open Policy Agent 的人，您可以继续下一部分——但是为了让每个人都在同一页面上，我们将快速浏览 OPA，以及它目前如何在大规模分布式系统中使用，如微服务或基于 Kubernetes 的系统。

OPA 是一个开源项目，由 [CNCF](https://www.cncf.io/) (云原生计算基金会)主持，专为云原生系统打造，结合了一种易于学习、开发人员可读的语言([减压阀](https://www.openpolicyagent.org/docs/latest/policy-language/))，以及策略模型和 API，为跨您的堆栈应用策略提供了一个通用框架。这使您能够将策略从代码中分离出来，并根据需要经常应用它，而不受代码更改或部署的影响。

OPA 本质上是一个灵活的引擎，它不仅使您能够枚举和编写策略，而且具有强大的搜索功能，不需要学习任何新的定制语法(例如，与其他数据库一样)，可以应用于任何 JSON 数据集。

在幕后，策略执行的工作方式是，为了在您的系统中应用特定的实践或策略，您必须基于系统中预先存在的事件或输入来这样做。一旦识别出这些事件或输入，就采取策略动作。因此，在我们决定如何处理这个输入(例如，用策略术语来说，允许/拒绝)之前，我们需要首先验证这个输入。作为一个引擎，OPA 能够验证数据集上的规则和策略。随后采取的行动取决于选择定义什么。

# 使用 OPA 将策略升级为代码

作为构建云资产管理工具的任务的一部分，我发现了直接了解我们的云中真正发生了什么的重要性。很多时候，由于错误甚至仅仅是知识资源的缺乏而被错误配置。

这些错误配置可能会导致未来的问题，无论是功能和成本后果，还是更多的安全后果。这些类型的错误配置或错误包括任何来自未连接的数据存储和流血成本，以及更高风险的错误，例如配置了过度许可访问的服务，这可能是一个严重的安全威胁。

OPA 是为解析 JSON 而构建的，在这个世界上，它在配置基础设施方面无处不在，OPA 能够遍历层次结构，扫描属性和特性以进行策略定义，并且具有完全在数据源之外的额外好处。这实际上意味着可以提取并压缩到 JSON 的任何数据源(甚至是非常大的 JSON 文件)，其中可以确定一个键/值对；OPA 可以轻松地对其进行搜索和解析，以提取与您的系统及其资源相关的信息。

一个内置功能是它的动态分类功能，这对于策略的实施很重要，但在此之前是一个关键步骤。为了能够提取关于错误配置或处于可能在云部署中出现问题的状态的资源的数据，我只需要按照预定义的(即现有的)标准进行搜索。

为什么这如此独特，需要我们看看如何使用其他技术来实现这一点。假设我想从两个不同的来源(例如一个 Elasticsearch 集群和 MongoDB 数据库)提取数据集，合并数据并提取某种细粒度的洞察力。嗯，这需要我首先用专有的语法对每一个进行检索。在检索到数据后，我需要智能地“加入”它。一旦组合了非常大的数据集，我就需要将它统一成一种格式，以便能够轻松地解析两个数据源。

现在让我们考虑用 OPA 来代替。

通过将所需的数据导出到 JSON，我已经可以绕过入门所需的专有检索和连接。通过用开发人员友好的减压阀语法非常简单地转换查询，我能够用一种统一的语言搜索多个不同的数据集，并且基本上通过最小搜索标准的增量进行过滤。这不仅使这种搜索民主化，因为它不需要数据库专家，它还使这个过程大大缩短，更简单，更灵活和可定制。

有时候，首先知道自己拥有什么也同样重要——然后再决定可以用它做什么，以及谁拥有哪些权限。

# OPA for Policy as Code in Action

OPA 提供了一个很好的平台来编写复杂的策略来识别许多事情，例如异常、错误配置或不良实践。

下面我将通过真实世界的例子来演示使用 OPA 和不使用 OPA 解析和提取相关数据集，这是在许多用例中实际利用它的好方法，这里只有两个代码示例。

**示例#1** 假设您是一家拥有多个 AWS 帐户的大型组织的 CISO，您想让您的帐户中的所有活动 IAM 用户都没有配置 MFA 设备，这是遵守公司安全标准所必需的。我们可以从所有 AWS 帐户中提取以下用户列表作为 JSON(使用 AWS API，这通常是一项非常复杂的任务，或者基本上使用 Firefly 的一个命令):

JSON

```
[
    ....
    {
        "Path": "/",
        "UserName": "liav",
        "Arn": "arn:aws:iam::123456789:user/liav",
        "CreateDate": "2021-04-25 08:53:34+00:00",
        "MFA": [
            {
            "UserName": "liav",
            "SerialNumber": "arn:aws:iam::123456789:mfa/liav",
            "EnableDate": "2021-04-25 09:00:38+00:00"
            }
        ],
        "Groups": [
            {
            "GroupName": "RND-Admins"
            }
        ]
    },
    {
        "Path": "/",
        "UserName": "jery",
        "Arn": "arn:aws:iam::123456789:user/jerry",
        "CreateDate": "2021-04-25 08:53:34+00:00",
        "MFA": [],
        "Groups": [
            {
            "GroupName": "DevOps-Admins"
            }
        ]
    },
    {
        "Path": "/",
        "UserName": "tom",
        "Arn": "arn:aws:iam::0987654321:user/tom",
        "CreateDate": "2021-04-25 08:53:34+00:00",
        "MFA": []
    }
    ....
]
```

如我们所见，一些用户没有与其帐户相关联的 MFA 设备。

为了识别这些使用 OPA 的用户，我们可以编写一个简单的策略来匹配每个没有 MFA 的用户:

YAML

```
package match# We define the keyword match as the placeholder of whether the input matches the policy or not 
default match = false# We define a policy to determine what is user with MFA protection
active_with_mfa {
    # The input keyword is a placeholder for the json value we wish to test our policy with
    is_array(input.MFA)
    count(input.MFA) > 0
}match {
    not active_with_mfa
}
```

现在，我们有了数据集和政策。使用简单的 Golang 代码，我们可以`get`或`match`资产或 IAM 用户(在这种情况下)。

去

```
package opaimport (
	"context"
	"encoding/json"
	"fmt"
	"sync" "github.com/open-policy-agent/opa/ast"
	"github.com/open-policy-agent/opa/rego"
)func GetMatchedAssets(ctx context.Context, regoPolicy string, dataset []map[string]interface{}) (err error) {
	var wg sync.WaitGroup
	compiler, err := ast.CompileModules(map[string]string{
		"match.rego": regoPolicy,
	})
	if err != nil{
		return fmt.Errorf("failed to complie rego policy: %w", err)
	}

	for _, asset := range dataset {
		wg.Add(1)

		go func(assetMap map[string]interface{}) {
			defer wg.Done()

			regoCalc := rego.New(
				rego.Query("data.match"),
				rego.Compiler(compiler),
				rego.Input(assetMap),
			)
			resultSet, err := regoCalc.Eval(ctx)
			if err != nil || resultSet == nil || len(resultSet) == 0{
				wg.Done()
			}

			for _, result := range resultSet {
				for _, expression := range result.Expressions {
					expressionBytes, err := json.Marshal(expression.Value)
					if err != nil {
						wg.Done()
					}

					var expressionMap map[string]interface{}
					err = json.Unmarshal(expressionBytes, &expressionMap)
					if err != nil {
						wg.Done()
					}

					if matched, ok := expressionMap["match"]; ok && matched.(bool){
						fmt.Printf("Asset matched policy: %s", getArn(assetMap))
					}
				}
			}
		}(asset)
	} wg.Wait() return nil
}
```

结果当然会是(上面结果的更简洁版本):

```
....
Asset matched policy: arn:aws:iam::123456789:user/jery
Asset matched policy: arn:aws:iam::0987654321:user/tom
....
```

实施例 2

作为一名 DevOps 工程师，我希望所有 Kubernetes 部署都带有最新的图像标签(这导致了在 Pod 中运行图像的问题和不准确性),并修复它们。

因此，我从多个集群中提取了一个所有实时部署 YAML 配置的列表(这可以使用 Kubernetes API 来完成，但要复杂一些，也可以使用 Firefly 中的一个命令来完成),格式为 JSON:

JSON

```
[
    ....
    {
        "apiVersion": "apps/v1",
        "kind": "Deployment",
        "metadata": {

            "creationTimestamp": "2021-12-09T08:59:27Z",
            "generation": 1,
            "name": "webapp",
            "namespace": "prod",
            "resourceVersion": "54800292",
            "uid": "06d1ccc8-d400-4cfc-993b-0826a2fab73b"
        },
        "spec": {
            "replicas": 1,
            "template": {
                "spec": {
                    "containers": [
                        {
                            "env": [
                                {
                                "name": "AWS_REGION",
                                "value": "us-east-1"
                                }
                            ],
                            "image": "webapp:43d733",
                            "imagePullPolicy": "Always",
                            "name": "webapp",
                            "ports": [
                                {
                                "containerPort": 80,
                                "name": "http",
                                "protocol": "TCP"
                                }
                            ]
                        }
                    ]
                }
            }
        }
    },
    {
        "apiVersion": "apps/v1",
        "kind": "Deployment",
        "metadata": {

            "creationTimestamp": "2021-12-29T08:59:27Z",
            "generation": 1,
            "name": "webapp",
            "namespace": "staging",
            "resourceVersion": "585858",
            "uid": "06d1ccc8-d400-4cfc-993b-dhfjdfhjhff"
        },
        "spec": {
            "replicas": 1,
            "template": {
                "spec": {
                    "containers": [
                        {
                            "env": [
                                {
                                "name": "AWS_REGION",
                                "value": "us-east-2"
                                }
                            ],
                            "image": "webapp:latest",
                            "imagePullPolicy": "Always",
                            "name": "webapp",
                            "ports": [
                                {
                                "containerPort": 80,
                                "name": "http",
                                "protocol": "TCP"
                                }
                            ]
                        }
                    ]
                }
            }
        }
    },
    {
        "apiVersion": "apps/v1",
        "kind": "Deployment",
        "metadata": {

            "creationTimestamp": "2021-12-09T08:59:27Z",
            "generation": 1,
            "name": "db",
            "namespace": "prod",
            "resourceVersion": "767767",
            "uid": "06d1ccc8-d400-4cfc-993b-jdhf84r"
        },
        "spec": {
            "replicas": 1,
            "template": {
                "spec": {
                    "containers": [
                        {
                            "env": [
                                {
                                "name": "AWS_REGION",
                                "value": "us-east-1"
                                }
                            ],
                            "image": "db",
                            "imagePullPolicy": "Always",
                            "name": "webapp",
                            "ports": [
                                {
                                "containerPort": 27017,
                                "name": "mongo",
                                "protocol": "TCP"
                                }
                            ]
                        }
                    ]
                }
            }
        }
    }
    ....
]
```

除了我们的 prod web 应用程序，我们还可以看到其他带有`latest`标签的部署，或者根本没有标签。我们将编写一个策略来标识没有固定映像标记(默认为最新)或带有`latest`标记的部署:

YAML

```
# We define the keyword match as the placeholder of whether the input matches the policy or not 
default match = false# We define a policy to determine what is a deployment with latest image tag
latest {
  # The input keyword is a placeholder for the json value we wish to test our policy with
  input.kind == "Deployment"
  output := split(input.spec.template.spec.containers[_].image, ":")
  # latest image tag
  output[1] == "latest"
}
latest {
  input.kind == "Deployment"
  output := split(input.spec.template.spec.containers[_].image, ":")
  # No image tag
  count(output) == 1
}match {
    latest
}
```

结果当然会是:

```
....
Asset matched policy: arn:k8s:v1::staging:deployment/webapp
Asset matched policy: arn:k8s:v1::prod:deployment/db
....
```

这使得提取重要的见解成为可能，即哪些数据回答了这个特定的查询子集，然后我们可以智能地决定如何应用最合适的策略。复杂程度的差异令人难以置信。突然之间，以前需要经验丰富的专家(如数据库工程师)才能完成的过程，可以由任何能够学习减压阀语法的人来完成(这对开发人员来说是可读和可理解的)。

这完全颠覆了当前的 OPA 范式，不再使用它最流行的对信息和数据进行解析和分类的用例(完全从用户那里抽象出来)，本质上只是基于这些数据执行一个全局策略。在 OPA 的文档中，它实际上非常清楚地指出，它将策略决策制定与策略实施分离，但 OPA 仍然主要用于实施。

# 用代码明智地选择您的策略

有了这种将 OPA 视为统一数据检索引擎的新思路，您就可以根据具体的异常、错误配置、变更、误用等情况选择应用更细粒度的策略。这些只是几个例子。

通过利用一个非常受欢迎的开源项目，进入这一关键信息的门槛已经降低，云和 DevOps 工程师可以快速了解他们高度复杂的云操作的状态。对于那些刚刚开始使用它的人来说，它还有一个额外的好处，那就是一个优秀的、支持性的社区。

在当今高度分散的无限可扩展运营中，流程中有多个利益相关方，能够快速识别您的云配置、部署的资源和使用情况变得越来越重要。随着云成本失控，云攻击面每天都在增加，在这种特定情况下，知识就是力量，最终你如何利用这些知识，这将是一种超级力量。