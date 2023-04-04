# 第二部分:智能合同和角度服务交互

> 原文：<https://blog.devgenius.io/part-2-smart-contract-and-angular-service-interaction-5862f6ec3f48?source=collection_archive---------4----------------------->

![](img/c6e2ceb824e9d66d26714ccb72d9a907.png)

## 如何在我们的角度应用程序中向区块链读写数据。

在[第 1 部分](https://medium.com/p/ace1c60b2523)中，我们对这个[存储库](https://github.com/pguso/angular-hardhat-starter-dapp)进行了大致的了解，您可以将它作为使用 Angular 开发您的 DApps 的起点。

在这一部分，我们想看一下我们用来与用 Solidity 编写的智能合同交互的服务。

这是将存储和读取区块链图像的智能合同:

```
// SPDX-License-Identifier: MIT
pragma solidity >=0.7.0 <0.9.0;

contract Gallery {
  Image[] private images;
  mapping(address => Image[]) private authorToImages;

  struct Image {
    string title;
    string imageMetaDataUrl;
  }

  function store(string memory title, string memory imageMetaDataUrl) public {
    Image memory image = Image(title, imageMetaDataUrl);

    images.push(image);
    authorToImages[msg.sender].push(image);
  }

  function retrieveAllImages() public view returns (Image[] memory) {
    return images;
  }

  function retrieveImagesByAuthor() public view returns (Image[] memory) {
    return authorToImages[msg.sender];
  }
}
```

让我们一步一步来。

在这里，我们将所有上传的图像存储在一个数组中:

```
Image[] private images;
```

在这里，我们将作者的地址映射到他们上传的图像。我们使用一个映射作为引用，就像数组和结构一样。

> 映射可以被看作是虚拟初始化的[哈希表](https://en.wikipedia.org/wiki/Hash_table)，使得每个可能的键都存在，并且被映射到一个字节表示全为零的值:类型的[默认值](https://docs.soliditylang.org/en/v0.4.21/control-structures.html#default-value)。

```
mapping(address => Image[]) private authorToImages;
```

这里使用了一个结构，它就像一个对象，我们也可以在元数据中存储图像的标题，我想展示如何使用结构。参数 imageMetaDataUrl 将保存 IPFS 到 JSON 对象的 Url，JSON 对象保存图像和描述的 URL。

```
struct Image {
  string title;
  string imageMetaDataUrl;
}
```

我们将标题和 imageMetaDataUrl()作为字符串传递给存储函数，该函数将初始化的结构 Image()推送到 images 数组和 authorToImages()映射。

```
function store(string memory title, string memory imageMetaDataUrl) public {
  Image memory image = Image(title, imageMetaDataUrl);

  images.push(image);
  authorToImages[msg.sender].push(image);
}
```

retrieveAllImages()是一个简单的 getter 方法，它将返回所有图像。

```
function retrieveAllImages() public view returns (Image[] memory) {
  return images;
}
```

通过 retrieveImagesByAuthor()方法，我们将通过 msg.sender(发件人的地址)获得通过该地址上传的所有图像。

```
function retrieveImagesByAuthor() public view returns (Image[] memory) {
  return authorToImages[msg.sender];
}
```

现在，我们来看看将与部署的智能合同交互的角度服务。

```
import { Injectable} from '@angular/core';
import { ethers} from "ethers";
import { environment} from "../../environments/environment";
import Gallery from '../../../artifacts/contracts/Gallery.sol/Gallery.json'
import detectEthereumProvider from "@metamask/detect-provider";

@Injectable({
  providedIn: 'root'
})
export class GalleryService {
  public async getAllImages(): Promise<any[]> {
    const contract = await GalleryService.*getContract*()

    return await contract['retrieveAllImages']()
  }

  public async getImagesByAuthor(): Promise<any[]> {
    const contract = await GalleryService.*getContract*(true)

    return await contract['retrieveImagesByAuthor']()
  }

  public async addImage(title: string, fileUrl: string): Promise<boolean> {
    const contract = await GalleryService.*getContract*(true)
    const transaction = await contract['store'](
      title,
      fileUrl
    )
    const tx = await transaction.wait()

    return tx.status === 1
  }

  private static async getContract(bySigner=false) {
    const provider = await GalleryService.*getWebProvider*()
    const signer = provider.getSigner()

    return new ethers.Contract(
      environment.contractAddress,
      Gallery.abi,
      bySigner ? signer : provider,
    )
  }

  private static async getWebProvider(requestAccounts = true) {
    const provider: any = await detectEthereumProvider()

    if (requestAccounts) {
      await provider.request({ method: 'eth_requestAccounts' })
    }

    return new ethers.providers.Web3Provider(provider)
  }
}
```

我们进口商品中最有趣的部分是我们画廊合同的细节。

```
import Gallery from '../../../artifacts/contracts/Gallery.sol/Gallery.json'
```

我们的服务 getWebProvider()底部有两个助手方法，将应用程序连接到元掩码。我们使用 npm 包来检测提供方。

> 一个用于检测元掩码以太网提供程序或在`window.ethereum`注入的任何提供程序的小实用程序。

```
private static async getWebProvider(requestAccounts = true) {
  const provider: any = await detectEthereumProvider()

  if (requestAccounts) {
    await provider.request({ method: 'eth_requestAccounts' })
  }

  return new ethers.providers.Web3Provider(provider)
}
```

我们的另一个助手方法 getContract()返回我们的合同实例。如果 bySigner 设置为 true，我们将为合同提供用户(帐户)的上下文。

```
private static async getContract(bySigner=false) {
  const provider = await GalleryService.getWebProvider()
  const signer = provider.getSigner()

  return new ethers.Contract(
   environment.contractAddress,
  Gallery.abi,
  bySigner ? signer : provider,
 )
}
```

在 getAllImages()中，我们获取了智能合同的一个实例，并在其上调用 retrieveAllImages()方法来获取所有上传的图像。

```
public async getAllImages(): Promise<any[]> {
  const contract = await GalleryService.getContract()

  return await contract['retrieveAllImages']()
}
```

在 getImagesByAuthor()中，我们通过签名者(登录用户)获取合同，并调用合同的 retrieveImagesByAuthor()方法。

```
public async getImagesByAuthor(): Promise<any[]> {
  const contract = await GalleryService.getContract(true)

  return await contract['retrieveImagesByAuthor']()
}
```

addImage()方法是将数据保存到区块链的唯一方法。它将接收标题和图像元数据的 IPFS URL，由签名者获取合同，调用智能合同的存储方法，并将参数传递给将保存到区块链的合同。

最后，我们将等待事务完成，数据保存到区块链，如果事务成功，将返回数据。

```
public async addImage(title: string, fileUrl: string): Promise<boolean> {
  const contract = await GalleryService.getContract(true)
  const transaction = await contract['store'](
    title,
    fileUrl
  )
  const tx = await transaction.wait()

  return tx.status === 1
}
```

现在，让我们看看如何将图像和数据保存到 IPFS。这是我们用来联系 IPFS 的角度服务。

```
import { Injectable} from "@angular/core"
import { create } from "ipfs-http-client"
import { environment} from "../../environments/environment"

@Injectable({
  providedIn: 'root'
})
export class IpfsService {
  public async uploadFile(data: any): Promise<string> {
    let url = ''
    const client = IpfsService.*getClient*()

    try {
      const added = await client.add(data)
      url = `${environment.ipfs}/ipfs/${added.path}`
    } catch (error) {
      console.log(error)
    }

    return url
  }

  private static getClient(): any {
    // @ts-ignore
    return create(`${environment.ipfs}:5001/api/v0`)
  }
}
```

我们在服务的底部有一个助手方法 getClient()，它将返回 HTTP API 客户端的一个实例，该实例解析为 IPFS HTTP API 的一个运行实例。我们使用一个 Infura 节点[https://ipfs . in fura . io](https://ipfs.infura.io):5001/API/v 0，但是你也可以使用你自己的节点。

uploadFile()方法接收一个图像或数据对象的 URL，并用 client.add()将它上传到 IPFS，client . add()返回一个散列，在下一行中，我们返回上传资产的完整 URL。

在我们的组件中，我们只需要用我们想要上传的文件(图像)或 JSON 数据对象调用 uploadFile()方法。

```
const fileUrl = await this.ipfs.uploadFile(eventTarget.files[0])// orconst metaDataUrl = await this.ipfs.uploadFile(JSON.stringify({
  fileUrl,
  description
}))
```

我希望这给你一个很好的总结，用 Angular 构建你自己的 DApps。如果我不能回答你的所有问题，欢迎你添加评论。