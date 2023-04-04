# 如何使用 docker 和 github 操作设置 CI/CD

> 原文：<https://blog.devgenius.io/how-to-set-up-ci-cd-using-docker-and-github-actions-c2502073c74c?source=collection_archive---------6----------------------->

![](img/3756c6b676c4b70e6ba29eb362338868.png)

在本文中，我们将讨论如何在 google cloud run 上使用 github 操作自动部署 dockerized 应用程序。

我们将使用以下架构在 gcp 云运行服务上部署我们的代码。

```
1\. Dockerizing the application.
2\. Building the docker image.
3\. Pushing the image to google container registry i.e GCR.
4\. Running the container to cloud run by fetching image from gcr.
```

首先，您应该:

1.  [Google 服务账号密钥](https://cloud.google.com/iam/docs/creating-managing-service-account-keys)作为 json 文件下载。
2.  建筑形象档案。

我假设你的应用程序中已经有 docker 文件。你可以点击这个链接来设置 python flask 应用程序的 docker 文件。

分享下面我的 docker 文件下面

```
FROM python:3
USER root
WORKDIR /appCOPY . .
EXPOSE 8000RUN pip install -r requirements.txt
CMD ["gunicorn"  ,"--timeout","2000","-w","1", "-b", "0.0.0.0:8000", "main:app"]
```

如果您想使用 github actions 构建部署到 google cloud 的管道，您需要使用 google 创建服务帐户，并下载该帐户的凭证 json 文件。Json 文件如下所示

```
{"type": "service_account","project_id": "project id","private_key_id": "","private_key": "-----BEGIN PRIVATE KEY-----
-----END PRIVATE KEY-----\n","client_email": "loreal-frontend-cloud-r@loreal-321234.iam.gserviceaccount.com","client_id": "11445571437233213417","auth_uri": "https://accounts.google.com/o/oauth2/auth","token_uri": "https://oauth2.googleapis.com/token","auth_provider_x509_cert_url": "","client_x509_cert_url": ""}
```

下面您可以找到 workflow.yaml 文件，它应该在。存储库的 github/workflows 目录。

在编写 yaml 文件之前，需要在 github 中保密的事项:

1.  gcp 项目标识:GCP 项目标识。
2.  gcp 图像 _ 存储库 _ 名称:图像将被推送到的 GCP 存储库。
3.  gcp 电子邮件:GCP 服务帐户电子邮件。
4.  GCP 应用端口:请求将被发送到这个端口上的容器。在我的例子中是 8000，因为我在端口 8000 上运行 gunicorn 服务器。
5.  GCP 凭证:粘贴 json 凭证文件的内容。
6.  GCP 实例 id:运行容器的服务的 ID。

```
name: "Docker push and deploy app on Google Cloud Run"

on:
  push:
    branches: ["master"]
jobs:
  build:
    name: Setup Gcloud Account and Deploy the application on google cloud
    if: "!contains(github.event.head_commit.message, 'skip ci')"
    runs-on: ubuntu-latest

    env:
      IMAGE_NAME: gcr.io/${{ secrets.GCP_PROJECT_ID }}/${{ secrets.GCP_IMAGE_REPOSITORY_NAME}}

    steps:
    - name: Print image tag
      run: echo $GITHUB_RUN_ID
    - name: Login To GCP
      uses: google-github-actions/setup-gcloud@v0
      with:
        project_id: ${{ secrets.GCP_PROJECT_ID }}
        service_account_email: ${{ secrets.GCP_EMAIL }}
        service_account_key: ${{ secrets.GCP_CREDENTIALS }}

    - name: Configure Docker
      run: gcloud auth configure-docker --quiet

    - name: Checkout repository
      uses: actions/checkout@v2

    - name: Build Docker image
      run: docker build . -t $IMAGE_NAME:$GITHUB_RUN_ID

    - name: Push Docker image
      run: docker push $IMAGE_NAME:$GITHUB_RUN_ID

    - name: Deploy Docker image
      run: gcloud run deploy ${{ secrets.GCP_INSTANCE_ID }} --image $IMAGE_NAME:$GITHUB_RUN_ID --allow-unauthenticated --port ${{ secrets.GCP_APP_PORT }} --region us-central1 --platform managed
```

了解 workflow.yaml 文件:

1.  分支:每当主分支上有任何变化，我的工作(即设置 Gcloud 帐户和部署谷歌上的应用程序)将被触发。
2.  仅当 git 提交消息中没有跳过 ci 的消息时，才运行作业。
3.  设置环境变量 IMAGE_NAME。
4.  登录您的 gcp 帐户。
5.  将 [gcloud 注册为 Docker 凭证助手](https://cloud.google.com/sdk/gcloud/reference/auth)。
6.  使用 **docker build 构建 docker 镜像。-t $ IMAGE _ NAME:$ GITHUB _ RUN _ ID**
7.  将 docker 映像推送到 gcr。
8.  部署到 gcp 云运行服务。