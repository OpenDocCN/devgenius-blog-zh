# 掌舵 3.7 更改 OCI 注册

> 原文：<https://blog.devgenius.io/helm-3-7-changes-to-oci-registry-16f2887e0704?source=collection_archive---------0----------------------->

## 掌舵| OCI |注册表|变更日志| CI/CD |管道

我想通了——也许这能节省你的时间。

![](img/d18d6cb46d753f64a6311391ac9f2027.png)

照片由 [Syed Hussaini](https://unsplash.com/@syhussaini?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 在 [Unsplash](https://unsplash.com/s/photos/helm?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 上拍摄

> 由于这不是一个“故事”，我将开门见山，不做过多解释。
> 这都是基于使用`HELM_EXPERIMENTAL_OCI=1`

# 将图表推送到注册表

## 之前:(掌舵< 3.7)

```
# CHART_REPO_URL=my.registry.name:port
echo "$REGISTRY_PASS" | helm registry login --username ${REGISTRY_USER} --password-stdin ${CHART_REPO_URL}helm chart list
version=$(grep "^version:" $name/Chart.yaml | cut -d: -f2 | tr -d " ")helm chart save $name ${CHART_REPO_URL}/$name
helm chart push ${CHART_REPO_URL}/$name:$version
```

## After: (Helm ≥ 3.7)

Documentation still needs updating: [https://github.com/helm/helm-www/issues/1196](https://github.com/helm/helm-www/issues/1196)

```
# CHART_REPO_URL=my.registry.name:port
echo "$REGISTRY_PASS" | helm registry login --username ${REGISTRY_USER} --password-stdin ${CHART_REPO_URL}# `helm chart list` no longer exists!
version=$(grep "^version:" $name/Chart.yaml | cut -d: -f2 | tr -d " ")helm package $name
helm push $name-$version.tgz oci://${CHART_REPO_URL}
```

# 从注册表中提取并解压缩图表

## 之前:

```
# CHART_REPO_URL=my.registry.name:port
helm chart pull ${CHART_REPO_URL}/${CHART}:${CHART_VERSION}
helm chart export ${CHART_REPO_URL}/${CHART}:${CHART_VERSION}
```

## 之后:

```
# CHART_REPO_URL=my.registry.name:port
helm pull --untar oci://${CHART_REPO_URL}/${CHART} --version ${CHART_VERSION}
```

# 舵变更日志

已更改的舵命令:

*   `helm chart [export|list|remove]` —已移除
*   `helm chart [pull|push]` —变更为`helm [pull|push]`
*   `helm chart save` —变更为`helm package`
*   OCI 注册表的网址需要以`oci://`开头

[](https://github.com/helm/helm/releases/tag/v3.7.0) [## 释放舵 3.7.0 舵/舵

### Helm v3.7.0 是一个功能版本。鼓励用户升级以获得最佳体验。社区在不断发展…

github.com](https://github.com/helm/helm/releases/tag/v3.7.0) 

# 支持或关注我

感谢阅读或鼓掌。如果你想让我继续，做更多或简单地表示你喜欢它，请考虑主演我的 repos 或关注我，这样我的贡献会得到更多的关注。

[](https://drpsychick.org/drpsychick-on-the-web-a9ccfb0df17e) [## 网上心理咨询

### 我所在的所有网站的一个简单的“登陆页”。

drpsychick.org](https://drpsychick.org/drpsychick-on-the-web-a9ccfb0df17e)