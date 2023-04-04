# GitOps 第 1 部分:基本上什么是 Git Flow 和 GitOps，它可以适用于任何地方吗

> 原文：<https://blog.devgenius.io/gitops-part-1-basically-what-is-git-flow-and-gitops-is-it-can-apply-for-everywhere-af259ec1460c?source=collection_archive---------3----------------------->

![](img/f2cea31ad49e775893c99b2e553cf9e2.png)

真实因观点不同而不同，但真理永远不变。

> 那么……我们如何确保我们从正确的角度看待它呢？

其实要抛弃相对性，专注于找到逻辑上最正确的那个。某事正确的事实是基于这样一个事实，即它比它的替代物错误更少。让我们抛开哲学上的讨论，回到正题。

首先，所有项目都需要灵活和健壮的 Git 分支策略。但是 Git 流程是随着团队结构、规模、产品结构而变化的。因此，没有一种 git 分支策略可以完美地适用于任何地方。但是许多人仍然会陷入这样的错误，Git 流“最佳实践”对每个团队和项目都是完美的…

![](img/7a3281fb42375d850cb547ce3bbafe5b.png)

Git 流的最佳实践(据称)

上面的流程可能适合一些使用 Scrum 的团队。

**例如:**对于上面的流程，通常需要为“开发”分支提供一个环境。不同的开发人员可以从“特性”分支开发多个特性。当一个完整的系统准备好在开发环境中进行测试时，开发人员被创建来请求合并要开发的特性。所以在这种情况下，有许多可能性存在，整合或任何其他基于原因的问题都可能发生。因此，为 git 带来基于历史的备份或任何其他解决方案需要太多时间，这意味着工作成本增加。当然，在这一点上，习惯于 rebase、squash 或任何其他基于 git 使用习惯会使修复这些问题变得容易，但还没有达到令人满意的水平。

![](img/119a3b63040f04e167601b5d37300437.png)

Scrum 进展的总结

但这显然并不总是完全奏效，回顾会议向我们展示了冲突，这是这种 scrum 完美主义方法的自然结果。但是 Git 流的设计是基于这样一个假设，即目标在 sprint 结束时完全实现了。业务管理方法和 git 流方法在这一点上是冲突的(在这一点上我不会责怪 scrum，它已经有了一个追溯系统，因为它预见到了这一点)。

稳定发布分支所基于的另一个解决方案是，在开发分支上测试每个特性分支集成，然后与发布分支合并的逻辑对于敏捷来说也是无用的。我认为我们需要接受没有完美的发展目标的方法。

## 当我们接受这一点并理解团队结构时，我们就可以创建更有效的分支策略。

![](img/60a1740725508b3227eca7a413266491.png)

但是我们可以使用 scrum base、开源、瀑布、螺旋或任何类型的软件开发方法。但是应该理解的是，git 分支策略的设计并不依赖于你选择的过程管理模型(除非你能排除人为因素，你需要考虑团队和产品结构)。因此，我们可以从 Git 分支策略中获得灵感的最常见的风格是:

*   Git 流
*   GitHub 流
*   一个流程
*   基于干线的流程

## 所以最有效的分支策略是团队和产品结构设计的定制分支策略。

顺便说一下，在这一点上，还应该考虑 git 的使用实践。所以一些开发团队成员可以提出一个不适合 rational git 使用的建议，比如:

> 为什么功能分支名称不是我们的名称，这将更好地 git 使用实践…:)

![](img/0aa2ea662aff18673f836de4b777a7ac.png)

应该创建一个 git 分支策略来解决团队和各方(客户、产品所有者等)的问题。)并且与 git 的使用实践不冲突。

## 提供这些之后，进入下一个升级级别并构建 **GitOps 系统是很重要的。**

# 什么是 GitOps

一般来说，到目前为止，git 存储库只包含应用程序代码或应用程序配置。但是基于 GitOps 的存储库也包含在基于操作的文件中，如；基础设施管理，创建或删除。这适用于在同一个地方使用 git 存储库管理 infra。

![](img/76b56b7307c4deafb93ec3635234b784.png)

至此，我们可以举一个基本的例子说明 GitOps 的一些基本类型:

*   **Terraform:** 提供我们通过 yaml 文件创建基于云资源的基础设施(repo 内部)。
*   **Gitlab CI:** 在一些分支内部发生事情时触发，通过 gitlab-ci.yml 文件(repo 内部)采取行动。
*   **Helm:** 通过 Helm 图表文件根据系统条件对 k8s 环境进行配置，使其更加灵活/可配置。

## 我们可以举太多基于 GitOps 的工具的例子，但是主要的理念是尽可能地在 git repo 内部管理操作。

> GitOps 有 3 个主要组件:IaC(Infra as Code)，CI/CD 管道，Git Repo

# 为什么我们需要 GitOps

如果我们比较一下最近的过去和现在:像 IIS，SVN，TFS，Tomcat 这些技术并没有太多的交互，实际上，系统的本质并不适合交互。但是 git，docker，k8s 或者任何现在的 tech 基本都是相互交互的。此外，有许多利益相关者的微体系结构和混沌系统的管理会导致共识问题。

得益于 GitOps，相关的系统管理得到了灵活快速的管理。但首要的是良好的 DevOps 实践。但是，应该应用 GitOps 实践，以免中断 DevOps 流程。尽管这取决于系统特性，但这并不总是 git repo 下的最佳方式。在这里，GitOps 实践的设计应该考虑 DevOps 实践。

# GitOps 和 DevOps 的差异

DevOps 是比 GitOps 更哲学和更全面的方法。所以实际上两者最终的目的是一样的。

目的是，用更少的努力产出更敏捷和高质量的软件。但是 DevOps 涵盖了应用监控、测试、源代码管理、部署、编排等流程。所以甚至有时为了使 DevOps 流程更好，你需要提供更好的项目管理自动化基础设施(例如；当任务/问题的状态从“进行中”变为“测试”时，系统应该能够自动将该任务/问题分配给测试人员。

![](img/f669d9801a1cf7d720888b79a5bff5a0.png)

实际上，DevOps 的目标是根据当前条件下的项目和团队结构，以最佳方式呈现所有 SDLC 流程。有时为了达到这个目的，你需要使用 helm(管理 K8S 的简单工具)，但有时你需要用 CI/CD 设计更好的 git 流程…

综上所述，与 DevOps 相比，GitOps 更加具体。但主要是 GitOps 更清晰(这并不意味着只有一个 GitOps 应用路径)。此外，当您服务于 GitOps 目的时，您也将服务于更好的 DevOps 基础架构。

## 参考资料:

[](https://www.redhat.com/en/topics/devops/what-is-gitops) [## 什么是 GitOps？

### GitOps 是一套使用 Git(一个开源版本)管理基础设施和应用程序配置的实践…

www.redhat.com](https://www.redhat.com/en/topics/devops/what-is-gitops) [](https://medium.com/@patrickporto/4-branching-workflows-for-git-30d0aaee7bf) [## Git 的 4 个分支工作流

### 在本文中，我们将介绍 Git 用户最流行的分支工作流，因此您可以决定哪种更适合…

medium.com](https://medium.com/@patrickporto/4-branching-workflows-for-git-30d0aaee7bf) [](https://www.toptal.com/software/trunk-based-development-git-flow) [## 基于主干的开发与 Git 流

### 为了开发高质量的软件，我们需要能够跟踪所有的变更，并在必要时撤销它们。版本…

www.toptal.com](https://www.toptal.com/software/trunk-based-development-git-flow) [](https://senexrex.com/scrum/) [## 混乱

### 背景:有创造力的人——如工程师、设计师、经理、研究人员、律师、建筑师和…

senexrex.com](https://senexrex.com/scrum/)  [## 软件开发流程|维基世界

### 在软件工程中，软件开发过程是将软件开发工作划分为…

www.wikiwand.com](https://www.wikiwand.com/en/Software_development_process) [](https://iamabhishek-dubey.medium.com/why-gitops-is-so-exciting-164379a331f4) [## 为什么 GitOps 这么刺激？

### 最初，我们已经看到了 DevOps、DevSecOps 和许多其他的 op，但是现在一个新的术语“GitOps”越来越出名了…

iamabhishek-dubey.medium.com](https://iamabhishek-dubey.medium.com/why-gitops-is-so-exciting-164379a331f4) [](https://dzone.com/articles/top-7-benefits-of-gitops-how-to-reinforce-your-sof) [## GitOps 的 7 大优势:如何增强您的软件生产力并加速业务发展…

### 当今世界视时间为头等大事。也就是说，颠覆性技术支持的创新是最重要的…

dzone.com](https://dzone.com/articles/top-7-benefits-of-gitops-how-to-reinforce-your-sof)