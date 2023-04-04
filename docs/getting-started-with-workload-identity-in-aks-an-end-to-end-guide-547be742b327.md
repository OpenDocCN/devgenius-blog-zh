# AKS 中的工作负载身份入门(端到端指南)

> 原文：<https://blog.devgenius.io/getting-started-with-workload-identity-in-aks-an-end-to-end-guide-547be742b327?source=collection_archive---------7----------------------->

![](img/ff96d4d74e69088e201179535bfb3ef0.png)

照片由 [Unsplash](https://unsplash.com/?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 上的 [Clément Hélardot](https://unsplash.com/@clemhlrdt?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 拍摄

这篇文章旨在为在您的 Azure Kubernetes 集群中设置和使用工作负载身份提供一种快速的方法。如果你感兴趣的话，这篇文章也可以和[这篇](https://azure.github.io/azure-workload-identity/docs/quick-start.html)快速入门一起使用，它提供了更多的背景知识。

假设:您已经安装了 az cli。您已经使用过 AKS，并且安装了 **kubectl** 实用程序和**舵** **图表**。您还拥有一个容器注册表和一个密钥库，其中至少有一个秘密可用于快速启动。

**步骤 1:** 创建 Azure Kubernetes 集群

第一步是创建一个 kubernetes 集群，正如您可能已经猜到的，本指南将以删除该集群结束。如果您想在不同的区域创建集群，请更新 **—位置**参数。

```
C:\> az aks create --resource-group my-aks-group --name my-aks-cluster --node-count 1 --generate-ssh-keys --location eastus2 --attach-acr <Your-Container-Registry-Name>
```

**步骤 2:** 获取新 AKS 集群的凭证

因为集群是刚刚创建的，所以您必须执行下面的 get-credentials 命令来将集群的凭证添加到您的本地 **kubeconfig** 中。

```
c:\> az aks get-credentials --resource-group my-aks-group --name my-aks-cluster
```

**步骤 3:** 上一步也会将此识别为当前上下文。但是如果您想在以后做同样的事情，您可以使用下面的命令。

```
c:\> kubectl config use-context my-aks-cluster 
```

**步骤 4:** 在 AKS 集群中启用 OIDC 发行者

为了使用工作负载身份，您必须启用 OIDC 发行者，并使用下面的命令获取 OIDC 发行者的发行者 URL。从这一步开始，我使用-g 和-n 来表示资源组名和 AKS 集群名。

这也可以作为步骤 1 本身的一部分来完成

```
c:\> az aks update -g my-aks-group -n my-aks-cluster --enable-oidc-issuer
```

要确认上一步是否成功，请执行以下命令。该命令查询并打印 OIDC 颁发者 URL

```
c:\> az aks show -g my-aks-group -n my-aks-cluster --query "oidcIssuerProfile.issuerUrl" -otsv
```

**第五步:**安装变异进汽网挂钩

我们需要安装一个变异的准入 web 挂钩，以便将一个签名的服务帐户令牌投射到您的工作负载卷。这也注入了一堆后续所需的变量。安装时使用了舵轮图。

首先，使用下面的命令获取订阅的租户 id。

```
az account show --subscription <Your-Subscription-Id-Or-Name> --query tenantId -otsv
```

现在您已经有了租户 id，我们可以开始了。第一步是添加 azure 工作负载报告

```
c:\> helm repo add azure-workload-identity https://azure.github.io/azure-workload-identity/charts
```

下一步是更新 helm repo 定义，因为我们添加了 azure workload repository

```
c:\> helm repo update
```

最后，我们执行变异准入网络挂钩的安装

```
c:\> helm install workload-identity-webhook azure-workload-identity/workload-identity-webhook --namespace azure-workload-identity-system --create-namespace --set azureTenantID="{AZURE_TENANT_ID_From_the_Previous_Step}"
```

关于网页挂钩的更多细节，你可以点击[这里](https://azure.github.io/azure-workload-identity/docs/installation/mutating-admission-webhook.html)。

**步骤 6:** 创建 params.json 文件

在下一步中，您将创建一个 Azure AD 应用程序和该应用程序的联合凭据。为了创建联邦凭证，您需要一个参数文件。首先，运行以下命令来获取 OIDC 颁发者的 URL。

```
c:\> az aks show -g my-aks-group -n my-aks-cluster --query "oidcIssuerProfile.issuerUrl" -otsv
```

用以下内容创建一个名为 params.json 的新文件。记住使用上一步中显示的值将<oidc_issuer_url>替换为 OIDC 发行人。</oidc_issuer_url>

```
{    
    "name": "kubernetes-federated-credential",    
    "issuer": "<oidc_issuer_url>",    
    "subject": "system:serviceaccount:default:workload-identity-sa",          
    "description": "Kubernetes service account federated credential",    
    "audiences": [      
       "api://AzureADTokenExchange"    
    ]  
}
```

**步骤 7:** 创建服务主体 Azure AD 应用程序，并在 Azure AD 应用程序和 AKS 集群之间创建联合凭证

第一步是创建服务主体

```
az ad sp create-for-rbac --name "MyApp WID App"
```

首先，将下面的内容保存为 display_required_ids.bat

```
FOR /F %%i IN ('az ad sp list --display-name "MyApp WID App" --query "[0].appId" -otsv') DO set APPLICATION_CLIENT_ID=%%i
echo APPLICATION_CLIENT_ID is %APPLICATION_CLIENT_ID% FOR /F %%i IN ('az ad app show --id %APPLICATION_CLIENT_ID% --query id -otsv') DO set APPLICATION_OBJECT_ID=%%i
echo APPLICATION_OBJECT_ID is %APPLICATION_OBJECT_ID% 
```

现在，运行批处理文件来获取相关应用程序的服务主体客户机 id 和对象 id

```
c:\> display_required_ids.bat
```

现在，您可以为此服务主体创建密钥存储策略。用上一步显示的值替换

```
c:\> az keyvault set-policy --name key-vault-name -g my-aks-group --secret-permissions get --spn <APPLICATION_CLIENT_ID>
```

最后，您可以在 AAD 应用程序和 kubernetes 服务帐户之间建立一个联合身份凭证，这将在下一步中创建。用上一步显示的值替换

```
c:\> az ad app federated-credential create --id <APPLICATION_OBJECT_ID> --parameters params.json
```

如果您在联合凭据绑定到的情况下重用服务主体/Azure AD 应用程序，您将收到一个错误，指示联合凭据已经存在。在这种情况下，您可以使用下面的步骤来更新现有的联合凭据。您可以使用以下命令获得关于联合凭据的详细信息

```
c:\> az ad app federated-credential show --id <APPLICATION_OBJECT_ID> --federated-credential-id kubernetes-federated-credential
```

如果没有错误，您可以继续更新

```
c:\> az ad app federated-credential update --id <APPLICATION_OBJECT_ID> --parameters params.json
```

**步骤 8:** 创建服务帐户和 pod

下面的清单创建了服务帐户和到目前为止使用工作负载 id 设置的 pod。在下面的清单中，您必须用在步骤 7 中运行批处理文件时显示的值替换**<application _ client _ id>**。

此外，容器中使用的图像是微软提供的现成产品。如果您喜欢从您的容器注册中心推送和使用图像，您也可以这样做。各种语言的来源可以在[这里](https://azure.github.io/azure-workload-identity/docs/topics/language-specific-examples/msal.html)找到。

最后，您还应该更新 **<密钥库名称>** 和 **<秘密名称>** 的值。

```
apiVersion: v1
kind: ServiceAccount
metadata:
  annotations:
    azure.workload.identity/client-id: <application_client_id>
  labels:
    azure.workload.identity/use: "true"
  name: workload-identity-sa
  namespace: default
---
apiVersion: v1
kind: Pod
metadata:
  name: quick-start
  namespace: default
spec:
  serviceAccountName: workload-identity-sa
  containers:
    - image: ghcr.io/azure/azure-workload-identity/msal-net
      name: oidc
      resources:
          limits:
            memory: 512Mi
            cpu: "1"
          requests:
            memory: 256Mi
            cpu: "0.2"
      env:
      - name: KEYVAULT_NAME
        value: <key-vault-name>
      - name: SECRET_NAME
        value: <secret-name>
  nodeSelector:
    kubernetes.io/os: linux
```

**步骤 9:** 验证输出

为了进行验证，您可以获取在上一步中创建的 pod 的日志。日志应该包含清单中标识的机密的值。

```
c:\> kubectl logs quick-start
```

**步骤 10:** 清理已创建的资源

我们现在可以开始清理了。首先，逐一运行以下命令来删除 pod 和服务帐户

```
kubectl delete pod quick-start
kubectl delete sa workload-identity-sa --namespace default
```

如果您愿意，也可以删除 Azure AD 应用程序、服务主体和联合凭据。

**第十一步:**删除上下文

现在，您可以使用下面的命令在本地 kubeconfig 中删除这个 AKS 集群的上下文。

```
c:\> kubectl config delete-context my-aks-cluster
```

**第十二步:**删除集群

最后，我们还可以删除 AKS 群集，指南到此结束！

```
c:\> az aks delete --resource-group my-aks-group --name my-aks-cluster
```

就是这样。您已经成功利用了工作负载标识！请记住，工作负载标识旨在取代不推荐使用的 pod 标识。这仍然是在预览中，希望它会很快出来预览。