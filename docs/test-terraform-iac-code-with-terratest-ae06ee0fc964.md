# 用 Terratest 测试 Terraform IaC 代码

> 原文：<https://blog.devgenius.io/test-terraform-iac-code-with-terratest-ae06ee0fc964?source=collection_archive---------4----------------------->

Terraform 是多云环境下重要的 IaC 工具。我分享了 Terraform [资源](https://billtcheng2013.medium.com/terraform-resources-to-be-productive-in-2020-d3ca98b0ea37)来开始它。然而，对于任何代码，如果你不能测试它，这在 DevOps 世界是一个大问题。 [Terratest](https://github.com/gruntwork-io/terratest) 是测试 terraform 代码的流行工具。这是端到端的测试(您不能在 terraform 代码中对一段代码或功能进行单元测试，相反，您可以提供基础设施，然后测试最终结果)。

那么你能测试什么呢？让我们看一些样品来找出答案。

# Hello world 示例

## 地形代码

[](https://github.com/gruntwork-io/terratest/tree/master/examples/terraform-hello-world-example) [## terra test/examples/terra form-hello-world-master grunt work-io/terra test 的示例

### 这个文件夹包含了最简单的 Terraform 模块——一个只输出“你好，世界”的模块——来演示你如何…

github.com](https://github.com/gruntwork-io/terratest/tree/master/examples/terraform-hello-world-example) 

```
terraform {
  # This module is now only being tested with Terraform 0.13.x. However, to make upgrading easier, we are setting
  # 0.12.26 as the minimum version, as that version added support for required_providers with source URLs, making it
  # forwards compatible with 0.13.x code.
  required_version = ">= 0.12.26"
}# website::tag::1:: The simplest possible Terraform module: it just outputs "Hello, World!"
output "hello_world" {
  value = "Hello, World!"
}
```

上面的代码什么也不做，只是返回输出“Hello，World！”

## 地形试验

[](https://github.com/gruntwork-io/terratest/blob/master/test/terraform_hello_world_example_test.go) [## terra test/terra form _ hello _ world _ example _ test . go at master grunt work-io/terra test

### 此时您不能执行该操作。您已使用另一个标签页或窗口登录。您已在另一个选项卡中注销，或者…

github.com](https://github.com/gruntwork-io/terratest/blob/master/test/terraform_hello_world_example_test.go) 

Terratest 测试代码用 Go 编写，Go [测试命名约定](https://ieftimov.com/posts/testing-in-go-naming-conventions/)遵循 Test +被测函数名称的格式。Terratest 测试代码通常有 4 个部分:

1.  Terraform 选项，将 terraform 代码设置为测试和变量
2.  设置延迟销毁操作，当功能结束时，terratest 将自动运行销毁
3.  初始化并应用 terraform 代码以供应基础架构
4.  验证基础设施

```
package testimport (
 "testing""github.com/gruntwork-io/terratest/modules/terraform"
 "github.com/stretchr/testify/assert"
)func TestTerraformHelloWorldExample(t *testing.T) {
 // website::tag::2:: Construct the terraform options with default retryable errors to handle the most common
 // retryable errors in terraform testing.
 terraformOptions := terraform.WithDefaultRetryableErrors(t, &terraform.Options{
  // website::tag::1:: Set the path to the Terraform code that will be tested.
  TerraformDir: "../examples/terraform-hello-world-example",
 })// website::tag::5:: Clean up resources with "terraform destroy" at the end of the test.
 defer terraform.Destroy(t, terraformOptions)// website::tag::3:: Run "terraform init" and "terraform apply". Fail the test if there are any errors.
 terraform.InitAndApply(t, terraformOptions)// website::tag::4:: Run `terraform output` to get the values of output variables and check they have the expected values.
 output := terraform.Output(t, terraformOptions, "hello_world")
 assert.Equal(t, "Hello, World!", output)
}
```

上面的测试只是从 terraform 中检索输出，并使用 assert。等于与预期值进行比较。主要代码是

```
// package
"github.com/gruntwork-io/terratest/modules/terraform"// retrieve terraform output
output := terraform.Output(t, terraformOptions, "hello_world")
```

# HTTP 测试

## 地形代码

[](https://github.com/gruntwork-io/terratest/tree/master/examples/terraform-http-example) [## terra test/examples/terra form-http-master grunt work-io/terra test 上的示例

### 此文件夹包含一个简单的 Terraform 模块，它在 AWS 中部署资源，演示如何使用 Terratest…

github.com](https://github.com/gruntwork-io/terratest/tree/master/examples/terraform-http-example) 

代码只是提供 EC2 实例服务 web 内容。因此，如果我们能够到达 HTTP 端点，就意味着基础设施配置正确。

## 地形试验

[](https://github.com/gruntwork-io/terratest/blob/master/test/terraform_http_example_test.go) [## terra test/terra form _ http _ example _ test . go at master grunt work-io/terra test

### 此文件包含双向 Unicode 文本，其解释或编译可能与下面显示的不同…

github.com](https://github.com/gruntwork-io/terratest/blob/master/test/terraform_http_example_test.go) 

主要代码是必需的包，使用 AWS 包与 AWS 交互，与 HTTP 端点交互。

```
// package to interact with AWS
"github.com/gruntwork-io/terratest/modules/aws"# package to interact with HTTP endpoint
http_helper "github.com/gruntwork-io/terratest/modules/http-helper"
```

AWS 代码

```
// Pick a random AWS region to test in. This helps ensure your code works in all regions.
 awsRegion := aws.GetRandomStableRegion(t, nil, nil)// Some AWS regions are missing certain instance types, so pick an available type based on the region we picked
 instanceType := aws.GetRecommendedInstanceType(t, awsRegion, []string{"t2.micro", "t3.micro"})
```

设置 terraform 代码的变量

```
// Construct the terraform options with default retryable errors to handle the most common retryable errors in
 // terraform testing.
 terraformOptions := terraform.WithDefaultRetryableErrors(t, &terraform.Options{
  // The path to where our Terraform code is located
  TerraformDir: "../examples/terraform-http-example",// Variables to pass to our Terraform code using -var options
  Vars: map[string]interface{}{
   "aws_region":    awsRegion,
   "instance_name": instanceName,
   "instance_text": instanceText,
   "instance_type": instanceType,
  },
 })
```

检索实例 URL 的 terraform 输出

```
// Run `terraform output` to get the value of an output variable
 instanceURL := terraform.Output(t, terraformOptions, "instance_url")
```

验证 HTTP 端点(因为代码在 terraform 代码完成后立即运行，但是 web 服务器需要一些时间来启动，所以您需要设置 retry 来等待 web 服务器完全启动)

```
// Verify that we get back a 200 OK with the expected instanceText
 http_helper.HttpGetWithRetry(t, instanceURL, &tlsConfig, 200, instanceText, maxRetries, timeBetweenRetries)
```

# AWS 实例属性

## 地形代码

[](https://github.com/gruntwork-io/terratest/tree/master/examples/terraform-aws-example) [## terra test/examples/terra form-AWS-master grunt work-io/terra test 的示例

### 此文件夹包含一个简单的 Terraform 模块，它在 AWS 中部署资源，演示如何使用 Terratest…

github.com](https://github.com/gruntwork-io/terratest/tree/master/examples/terraform-aws-example) 

它部署一个 EC2 实例，并给该实例一个名称标签。

## 地形试验

[](https://github.com/gruntwork-io/terratest/blob/master/test/terraform_aws_example_test.go) [## terra test/terra form _ AWS _ example _ test . go at master grunt work-io/terra test

### 此时您不能执行该操作。您已使用另一个标签页或窗口登录。您已在另一个选项卡中注销，或者…

github.com](https://github.com/gruntwork-io/terratest/blob/master/test/terraform_aws_example_test.go) 

主要代码是使用 AWS 包来检索 AWS 资源属性并进行验证。

```
// Run `terraform output` to get the value of an output variable
 instanceID := terraform.Output(t, terraformOptions, "instance_id")aws.AddTagsToResource(t, awsRegion, instanceID, map[string]string{"testing": "testing-tag-value"})// Look up the tags for the given Instance ID
 instanceTags := aws.GetTagsForEc2Instance(t, awsRegion, instanceID)// website::tag::3::Check if the EC2 instance with a given tag and name is set.
 testingTag, containsTestingTag := instanceTags["testing"]
 assert.True(t, containsTestingTag)
 assert.Equal(t, "testing-tag-value", testingTag)// Verify that our expected name tag is one of the tags
 nameTag, containsNameTag := instanceTags["Name"]
 assert.True(t, containsNameTag)
 assert.Equal(t, expectedName, nameTag)
```

# 摘要

在高层次上，我们可以在下面测试

1.  我们可以将基础架构视为黑盒，并像最终用户一样进行交互以进行验证(使用这种方法来测试类似“公众无法访问，但可以通过特定的 VPC/虚拟网络访问，因为很难进行 terratest 以切换到特定的 VPC”这样的场景会很复杂)
2.  检索输出以与我们的预期值进行比较
3.  我们可以使用云提供商接口来获取资源并验证[属性](https://github.com/gruntwork-io/terratest/blob/master/test/terraform_aws_network_example_test.go)。

# 附录

Terraform 测试工具比较

[](https://abstraction.blog/2021/06/20/terraform-testing-tools-comparison) [## 比较测试 Terraform 代码的五大工具

### "质量意味着即使没有人注意，也要把事情做好."-亨利·福特我已经有几年没有…

abstraction .博客](https://abstraction.blog/2021/06/20/terraform-testing-tools-comparison) 

其他重要的地形工具

[](https://betterprogramming.pub/5-essential-terraform-tools-to-use-everyday-e910a96e70d9) [## 日常使用的 5 种基本地形工具

### 通过 Terraform 提高效率并利用您的代码

better 编程. pub](https://betterprogramming.pub/5-essential-terraform-tools-to-use-everyday-e910a96e70d9) [](https://blog.gruntwork.io/automatically-enforce-policies-on-your-terraform-modules-using-opa-and-terratest-d6a3f34330a1) [## 使用 OPA 和 Terratest 在 Terraform 模块上自动实施策略

### 许多组织都有业务和法律要求，这些要求必须在他们的基础架构上持续实施…

博客. gruntwork.io](https://blog.gruntwork.io/automatically-enforce-policies-on-your-terraform-modules-using-opa-and-terratest-d6a3f34330a1) [](https://medium.com/devops-dudes/using-terragrunt-to-enhance-iac-with-terraform-77a8c885b540) [## 使用 Terragrunt 增强与 Terraform 的 IaC

### 通过结合 Terraform 和 Terragrunt 使基础设施即代码变得更容易

medium.com](https://medium.com/devops-dudes/using-terragrunt-to-enhance-iac-with-terraform-77a8c885b540)  [## Terraform 代码布局和使用 Terragrunt

### Terraform 可以让你随心所欲地组织代码。

medium.com](https://medium.com/@AaronKalair/terraform-code-layout-and-using-terragrunt-db6864967916) [](https://docs.microsoft.com/en-us/azure/developer/terraform/best-practices-end-to-end-testing) [## 对 Terraform 项目实施端到端的 Terratest 测试

### 端到端(E2E)测试用于在将程序部署到生产环境之前验证程序是否有效。一个示例场景…

docs.microsoft.com](https://docs.microsoft.com/en-us/azure/developer/terraform/best-practices-end-to-end-testing) [](https://medium.com/codex/terraform-best-practices-testing-your-code-f25053b06c4c) [## Terraform 最佳实践—测试您的代码

### 当开始使用 Terraform 时，很难知道在许多领域什么被认为是“最佳实践”。

medium.com](https://medium.com/codex/terraform-best-practices-testing-your-code-f25053b06c4c) [](https://mschirbel.medium.com/testing-iac-how-could-we-do-it-b4187f6c1145) [## 测试 IaC——我们该如何做

### 嗨伙计们，你们好吗？我希望你们都过得很好。

mschirbel.medium.com](https://mschirbel.medium.com/testing-iac-how-could-we-do-it-b4187f6c1145) [](https://medium.com/trendyol-tech/terratesting-221cf58d3ae1) [## 地面测试

### 通常，Terraform 用于在云平台上实现基础设施的供应。所以我开始想…

medium.com](https://medium.com/trendyol-tech/terratesting-221cf58d3ae1) [](https://itnext.io/six-ways-to-take-your-terraform-to-the-next-level-e08b3e21b243) [## 六种方法让你的地形更上一层楼

### 让你的地形更具可扩展性和使用乐趣的工具和技巧。

itnext.io](https://itnext.io/six-ways-to-take-your-terraform-to-the-next-level-e08b3e21b243)