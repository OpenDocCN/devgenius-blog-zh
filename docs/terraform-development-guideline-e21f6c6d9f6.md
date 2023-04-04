# 地形开发指南

> 原文：<https://blog.devgenius.io/terraform-development-guideline-e21f6c6d9f6?source=collection_archive---------6----------------------->

## 包括 Terraform 推荐的实践和模式

本开发公约文件主要针对中小型地形开发项目(尽管对于非常大的地形项目，大多数指南仍然适用)。

# 命名规格

## 一般惯例

1.在任何地方(资源名、数据源名、变量名、输出等)使用 snake_case(用 _(下划线)代替—(破折号))。
2。只使用小写字母和数字。
3。名字总是用单数名词。
4。始终使用双引号。

## 资源和数据源参数

1.不要在资源名称中重复资源类型(不部分重复，也不完全重复)

```
// Good
resource "aws_route_table" "public" {}// Bad
resource "aws_route_table" "public_route_table" {}// Bad
resource "aws_route_table" "public_aws_route_table" {}
```

2.如果没有更具描述性的通用名称，或者如果资源模块创建了此类型的单个资源，资源名称应命名为 this(例如，在 AWS VPC 模块中，有一个 aws_nat_gateway 类型的单个资源和多个 aws_route_table 类型的资源，因此 aws_nat_gateway 应命名为 this，aws_route_table 应具有更具描述性的名称，如 private、public、database)。
3。如果资源支持，将参数标记作为最后一个实际参数，后面跟随 depends_on 和 lifecycle(如果需要)。所有这些都应该用一个空行隔开。

```
// Good
resource "aws_nat_gateway" "this" {
  count = 2allocation_id = "..."
  subnet_id     = "..."tags = {
    Name = "..."
  }depends_on = [aws_internet_gateway.this]lifecycle {
    create_before_destroy = true
  }
} 
// Bad
resource "aws_nat_gateway" "this" {
  count = 2tags = "..."depends_on = [aws_internet_gateway.this]lifecycle {
    create_before_destroy = true
  }allocation_id = "..."
  subnet_id     = "..."
}
```

## 变量

1.不要在资源模块中重新发明轮子:使用“参数引用”一节中为您正在使用的资源定义的变量的名称、描述和默认值。
2。当 type 为 list(…)或 map(…)时，在变量名中使用复数形式。
3。如果可能，请使用特定类型。除非绝对必要，否则尽量避免使用 any 类型(这将禁用类型验证)。变量块中的关键字顺序如下:描述、类型、默认值、验证。
5。始终包括对所有变量的描述，即使你认为这是显而易见的(你将来会需要它)。
6。除非需要对每个键都有严格的约束，否则最好使用简单类型(数字、字符串、列表(…)、映射(…)、any)而不是通用类型，如 object()。
7。如果映射的所有元素都具有相同的类型(例如字符串)或可以转换为相同的类型(例如数字类型可以转换为字符串)，请使用特定的类型，如 map(map(string))。
8。值{}有时是地图，有时是对象。用 tomap(…)做地图，因为没有办法做对象。

## 输出

1.输出的名称应该描述它包含的属性，并且不像您通常希望的那样自由。
2。如果输出返回具有插值函数和多个资源的值，则{name}和{type}应尽可能通用(应省略前缀 as)。

```
// Good
output "security_group_id" {
  description = "The ID of the security group"
  value       = try(aws_security_group.this[0].id, aws_security_group.name_prefix[0].id, "")
}// Bad
output "this_security_group_id" {
  description = "The ID of the security group"
  value       = element(concat(coalescelist(aws_security_group.this.*.id, aws_security_group.web.*.id), [""]), 0)
}
```

3.如果返回值是一个列表，它应该有一个复数名称

```
output "rds_cluster_instance_endpoints" {
  description = "A list of all cluster instance endpoints"
  value       = aws_rds_cluster_instance.this.*.endpoint
}
```

4.总是包括所有输出的描述，即使你认为它是显而易见的。
5。避免设置敏感参数，除非您完全控制在所有模块的所有位置使用此输出。
6。如果需要处理复杂的数据结构并且预计会出现错误，请使用 try()(从 Terraform 0.13 开始提供)。try()是一个特殊的函数，它能够捕获在计算其参数时产生的错误，这在处理复杂的数据结构(其形状在实现时并不为人所知)时特别有用。

```
# Try example
data "http" "primary-server" {
  url = "[https://ip-ranges.amazonaws.com/ip-ranges.json](https://ip-ranges.amazonaws.com/ip-ranges.json)"# Optional request headers
  request_headers = {
    Accept = "application/json"
  }
}locals {
# This returns the sync token from the endpoint, the return value is of the type string.
  syncToken = try(jsondecode(data.http.primary-server.body).syncToken,
              "NO TOKEN AVAILABLE"
              )# This variable holds the all the unique regions returned by the endpoint. The return value is of the type list OR a string error value.
  regions = try(distinct([for items in jsondecode(data.http.primary-server.body).prefixes:
    items.region
  ]), "NO LIST PROVIDED IN LOCALS REGION VARIABLE")# This variable holds the all the unique services returned by the endpoint. The return value is of the type list OR a string error value.
  services = try(distinct([for items in jsondecode(data.http.primary-server.body).prefixes:
    items.service
  ]), "NO LIST PROVIDED IN LOCALS SERVICES VARIABLE")}output "response-json-regions" {
  value = local.regions
}output "response-json-services" {
  value = local.services
}
```

# 主文件结构

将文件中的每个组件组合在一起

```
# data source arguments# resource/module# resource `null_resource`
```

应尽可能使用隐式依赖关系。depends_on 参数只应作为最后的手段使用。在使用它的时候，总是包括一个注释来解释为什么使用它，以帮助未来的维护者理解附加依赖的目的。

# 项目结构

```
terraform-project
├─ modules
│  ├─ module_1
│  │  ├─ main.tf
│  │  ├─ outputs.tf
│  │  ├─ variables.tf
│  ├─ module_2
│  │  ├─ main.tf
│  │  ├─ outputs.tf
│  │  ├─ variables.tf
│
│─ variables
│  │─ dev.backend.tfvars
│  │─ dev.tfvars
│  │─ uat.backend.tfvars
│  │─ uat.tfvars
│  │─ production.backend.tfvars
│  │─ production.tfvars
│
├─ main.tf
├─ outputs.tf
├─ variables.tf
├─ versions.tf
│
│
└─ README.md
```

module: Module 是连接的资源的集合，这些资源一起执行共同的动作(例如:aws_vpc 创建 vpc、子网、NAT 等)
main.tf:调用模块、局部变量和数据源来创建所有资源
变量。tf:包含 main.tf 中使用的变量的声明

outputs.tf:包含在 main.tf
versions 中创建的资源的输出。tf:包含 Terraform 和 providers
variables 文件夹:保存将应用于特定部署的独立变量文件，例如，dev.tfvars 将在部署我们的开发环境时使用。
dev.backend.tfvars:针对特定于环境的后端的配置(用于保存状态文件)
dev.tfvars:针对开发的配置(类似实例类型，…)
uat.tfvars:针对 uat 的配置(类似实例类型，…)
production.tfvars:针对生产的配置(类似实例类型，…)

变量. tf

```
variable "environment" {
  type = string
}variable "region" {
  description = "AWS Region"
  type        = string
  default     = "us-west-2"
}
```

outputs.tf

```
# We add an output for the VPC's NAT EIPs, so that we can give these to
# a third party in case they need to add it to a firewall.
output "vpc_nat_eips" {
  description = "External IPs used by NAT gateways"
  value       = module.prod_vpc.nat_eips
}
```

versions.tf

```
terraform {
  required_version = "~> 1.0"backend "s3" {
    bucket         = "orgname-prod-terraform-state-us-west-2"
    key            = "app-my-webapp-prod/terraform.tfstate"
    region         = "us-west-2"
    dynamodb_table = "terraform-state-lock-dynamo"
    encrypt        = "true"
  }
}provider "aws" {
  version = "~> 3.0"
  region  = var.region
  profile = "dev-profile"
}
```

开发工具

```
environment = "dev"
region = "us-east-1"
```

# 最佳实践

## 使用共享模块

一般来说，避免在 Terraform 存储库中放置模块。尽可能使用 Terraform 注册表中的模块。不要多此一举。
始终设置模块版本。

```
// Good
module "rds" {
  source  = "terraform-aws-modules/rds/aws"
  version = "2.18.0"
  ...
}// Bad
module "rds" {
  source  = "terraform-aws-modules/rds/aws"
  ...
}
```

## 国营商店

不提交状态存储(。tfstate 文件)，因为该文件未加密，可能会暴露敏感数据。请改用 terraform 远程状态管理。您可以
使用 Terraform Cloud 远程状态管理功能存储状态。
使用云存储存储状态，例如 Azure blob 存储

```
terraform {
  required_providers {
    azurerm = {
      source  = "hashicorp/azurerm"
      version = "=2.46.0"
    }
  }
    backend "azurerm" {
        resource_group_name  = "tfstate"
        storage_account_name = "<storage_account_name>"
        container_name       = "tfstate"
        key                  = "terraform.tfstate"
    }
}
```

使用状态锁定来保护状态。如果你的后台支持，Terraform 将为所有写状态的操作锁定你的状态。这可以防止其他人获得锁并潜在地破坏您的状态。S3 后端还通过 dynamodb 支持状态锁定和一致性检查，这可以通过将 dynamodb_table 字段设置为现有的 Dynamo DB 表名来实现。一个 DynamoDB 表可以用来锁定多个远程状态文件。Azure 后端支持 Azure Blob 存储原生功能的状态锁定和一致性检查。GCS 后端支持状态锁定。使用版本控制或备份状态文件，以便在需要时可以回滚到以前的状态。
·每个环境维护一个状态

## 安全性

使用 IAM 策略限制对后端状态存储的访问。
不要在 Terraform 代码或变量文件中硬编码敏感数据(密码、ssh 密钥等)，不要将其推送给 Git，动态定义敏感数据(terrafrom 计划或 terrafrom 应用，或使用环境变量作为第一步，这与 Github 操作很好地集成)或使用 Vault Provider 来处理敏感数据。

```
$ export AWS_ACCESS_KEY_ID=”accesskey”
$ export AWS_SECRET_ACCESS_KEY=”secretkey”
$ export AWS_DEFAULT_REGION=”ap-southeast-2"
$ terraform plan
```

## 评论

为其他开发人员编写好的注释来理解您的代码(这里有一些好的注释准则 1、2)。Terraform 语言支持三种不同的注释语法:

#开始单行注释，在行尾结束。
//也开始单行注释，作为#的替代。
/*和*/是可能跨越多行的注释的开始和结束分隔符。#单行注释样式是默认的注释样式，应该在大多数情况下使用。自动配置格式化工具可能会自动将// comments 转换成# comments，因为双斜线样式不是惯用的。

# 工具作业

## 格式化

始终使用 terraform fmt 将 terraform 配置文件重写为规范的格式和样式，例如，如果您使用 Visual Studio 代码，则可以使用 Terraform Visual Studio 代码扩展

通过运行以下命令，检查您的配置格式并使其整洁。

```
terraform fmt -diff
```

格式化命令以规范的格式和样式重写 Terraform 配置文件。它应用了 Terraform 语言样式约定以及其他一些小的调整来提高可读性。默认情况下，该命令检查工作目录中的所有 Terraform 文件，但也可以在子目录中递归运行。查看命令帮助文本以了解更多信息

```
terraform fmt --help
```

## 确认

使用“terraform validate”来验证 terraform 代码

# 典型地形工作流程

对于开发人员手动测试，您可以使用核心工作流:terraform init、terraform plan、terraform apply，它们使用本地状态存储。然而，在自动化程度更高的环境中，我们建议使用远程状态后端。
1。为环境使用特定的后端配置

```
terraform init -backend-config="./variables/dev.backend.tfvars"
```

2.应用 terraform 环境特定的配置

```
terraform apply -var-file="./variables/dev.tfvars"
```

# 参考

[](https://www.terraform-best-practices.com/) [## 欢迎

### 本文档试图系统地描述使用 Terraform 的最佳实践，并为…提供建议

www.terraform-best-practices.com](https://www.terraform-best-practices.com/) [](https://www.terraform.io/cloud-docs/guides/recommended-practices) [## 索引- Terraform 推荐做法| hashi corp 的 Terraform

### 搜索 Terraform 文档本指南适用于希望从一个平台提高其 Terraform 使用的企业用户

www.terraform.io](https://www.terraform.io/cloud-docs/guides/recommended-practices) [](https://upcloud.com/community/stories/terraform-best-practices-beginners/) [## 面向初学者的 Terraform 最佳实践

### Terraform 是我们最喜欢的基础设施管理工具之一，当涉及到将基础设施配置为…

upcloud.com](https://upcloud.com/community/stories/terraform-best-practices-beginners/) [](https://levelup.gitconnected.com/using-terraforms-try-can-and-input-validation-eb45037af2b2) [## 使用 Terraform 的 try()、can()和输入验证

### 了解如何使用 Terraform 的 can()和 try()函数。

levelup.gitconnected.com](https://levelup.gitconnected.com/using-terraforms-try-can-and-input-validation-eb45037af2b2) [](https://javaadpatel.com/terraform-best-practices/) [## 面向 Azure 的 Terraform 最佳实践

### DevOps Terraform 最佳实践，帮助您创建更易维护、更安全的 Terraform 配置。Terraform 是…

javaadpatel.com](https://javaadpatel.com/terraform-best-practices/) [](https://iamondemand.com/blog/how-to-use-terraform-like-a-pro-part-2/) [## 如何像专业人士一样使用 Terraform:第二部分

### 深入阅读我们的“像专业人士一样使用 Terraform”系列的第 2 部分，了解它可以解决的用例——从#多层…

iamondemand.com](https://iamondemand.com/blog/how-to-use-terraform-like-a-pro-part-2/) [](https://medium.com/devops-mojo/terraform-best-practices-top-best-practices-for-terraform-configuration-style-formatting-structure-66b8d938f00c) [## Terraform —最佳实践

### 使用 Terraform 的最佳实践。

medium.com](https://medium.com/devops-mojo/terraform-best-practices-top-best-practices-for-terraform-configuration-style-formatting-structure-66b8d938f00c) [](https://slalom-consulting-ltd.github.io/terraform-guidelines/) [## 地形-指南

### 由詹姆斯伍尔芬登这是一个指南，以书面 Terraform 符合障碍伦敦风格，它遵循哈希公司…

slalom 咨询有限公司](https://slalom-consulting-ltd.github.io/terraform-guidelines/)