# 加速您的测试在 Google Cloud 上自动创建项目

> 原文：<https://blog.devgenius.io/speed-up-your-tests-automating-project-creation-on-google-cloud-e60b36923ed7?source=collection_archive---------11----------------------->

通过在 Google Cloud 中使用 Terraform 自动创建和删除项目来加速测试。

![](img/54c5be6b595332e31959577cdbef6622.png)

由[Amr Taha](https://unsplash.com/@amr_taha?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)在 [Unsplash](https://unsplash.com/s/photos/headphones-on-desk?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 上拍摄的照片

最近，我遇到了这样一种情况:必须尝试一些新的东西，时间被一些平凡的任务所消耗，比如创建一个测试项目，然后担心删除它或者至少删除所有使用过的服务。

因此，为了加快速度，我不仅开始自动化服务的设置，还开始自动化项目本身的创建。

这是我一直在使用的样本模板。

## 账单账户

首先，您需要一个现有的计费帐户来关联您的新项目。如果你想创建服务，谷歌不会免费提供，所以，如果没有地方收费，你就不会被允许创建服务。

您需要[Google _ billing _ account](https://registry.terraform.io/providers/hashicorp/google/latest/docs/data-sources/billing_account)terra form 对象。

在这里，Terraform 将搜索一个名为`My Billing Account`的`OPEN`计费账户。

## 项目

有了计费帐户的参考，我们可以继续创建一个 [google_project](https://registry.terraform.io/providers/hashicorp/google/latest/docs/resources/google_project) 。

正如您所看到的，您可以通过在项目参数中直接硬编码计费帐户 id 来保存初始步骤，但是这可能会在以后引入安全问题。

## 服务

如果你已经有了一些使用谷歌云的经验，你会知道有些服务开箱后就被禁用了。对于自动化项目创建的想法来说，这将是一个挫折。

幸运的是，Terraform 还为我们提供了 [google_project_service](https://registry.terraform.io/providers/hashicorp/google/latest/docs/resources/google_project_service) ，我们可以在其中维护我们的项目所使用的服务。

如果您正在考虑以这种方式维护生产环境，我建议将项目创建与实际服务隔离开来(分别管理它们)。

至于测试，对我来说，这种方法创造了奇迹，我只是在项目 id 中添加了一个随机字符串。