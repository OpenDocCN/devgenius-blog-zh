# 如何成为一名 Web3 开发者

> 原文：<https://blog.devgenius.io/how-to-become-a-web3-developer-e1e76a23dcc7?source=collection_archive---------0----------------------->

## 导航错综复杂的 web3 世界的初学者指南

每个人都在谈论 web3，疯狂的资金正在流入这个领域。一个普通的区块链开发者每年挣 14 万美元。进入这个行业是你作为开发者能做的最好的事情！

由于 web3 行业相对较新，所以没有很多资源，而且很多都没有更新。我整理了一份最佳资源和指南的清单，来创建学习 web3 和 Solidity 的终极路线图。

![](img/a2f943c7fed71c3f420c2e15edb51ea7.png)

[赛·基兰·阿纳加尼](https://unsplash.com/@anagani_saikiran?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)在 [Unsplash](https://unsplash.com/s/photos/learning-coding?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 上的照片

# 1.区块链基础知识

成为一名 web3 开发者首先需要了解的是区块链。这将有助于您轻松构建和优化智能合同(我们将在后面详细讨论)。让我们看看什么是区块链，以及它是如何工作的。

区块链是一个不可变的、公共的、分散的数据库，由用户所有。数据以块的形式存储，因此得名“区块链”。为了将块链接在一起，每个块包含其数据、其散列和前一个块的散列。

> 哈希是从数据中计算出来的唯一字符串。哈希会根据信息发生变化。在 NodeJS 中实现 JWT 令牌时，您可能会遇到这种情况。

如果有人篡改了其中的一个块，哈希就会发生变化，而下一个块会因为链被破坏而变得无效。然后，该人必须重新计算链中的所有块，这将花费很长时间(10 分钟/块)。

区块链的副本存储在世界各地的计算机上，称为节点。所以你还需要访问大多数计算机，并重复这个过程。计算机几乎不可能足够快地完成所有这些工作，因此网络不会注意到，也不会试图将欺诈者踢出区块链。

这里有一个简短的 Udemy 课程，深入探讨了区块链的基本原理:

[https://www . udemy . com/course/区块链-和-比特币-基本面/](https://www.udemy.com/course/blockchain-and-bitcoin-fundamentals/)

# 2.分散应用

DApps 或分散式应用程序是建立在区块链之上的应用程序。DApps 中使用的主要技术有:

1.  前端:JavaScript 框架，如 React、Vue、Angular
2.  后端:Rust 和 Solana 或 Solidity 和 Ethereum

现在，你可能会想，有哪些使用 DApps 的例子？以下是 DApps 可以彻底改变的几个行业:

1.  托管——无论何时你买/卖房子，你都需要相信买家会按时付款，或者把钱存在第三方公司。买家不付款，或者第三方卷款跑路怎么办？DApps 可以确保资金安全转移
2.  记录-一旦将某项内容添加到区块链中，就不能对其进行编辑或移除。这在维护房屋记录、医疗记录等时非常方便。
3.  支付——加密货币可用于轻松安全的支付，以转移价值。虽然现在汽油费很高，但我相信将来会降低的。

# 3.前端 Web 开发基础

正如我之前提到的，DApps 可能有区块链技术驱动后端，但前端是 JavaScript。以下是你需要学习的内容:

1.  HTML —常见的 HTML 标签
2.  CSS 基本属性，伸缩，网格
3.  CSS 框架[可选]——引导、语义 UI、Tailwind 等
4.  JavaScript——变量、函数、类、ES6 等。
5.  JavaScript 框架[推荐] — React/Vue/Angular

我还建议学习 web2 后端，作为万一 web3 不工作时的退路。以下是您应该为后端学习的内容:

1.  NodeJS 基础—事件循环、I/O
2.  API 框架—快速
3.  数据库— MongoDB、SQL、PostgreSQL

这是柯尔特·斯蒂尔教授的一门非常棒的网络开发课程:

![](img/20ff20576108da4181d9d6abc6b88cc1.png)

[https://www.udemy.com/course/the-web-developer-bootcamp](https://www.udemy.com/course/the-web-developer-bootcamp/)/

# 4.以太坊基础

以太坊是一个处理智能合同的区块链。以太坊是迄今为止 2022 年最受欢迎的创建智能合约的区块链。可靠性是用来开发智能合同的语言。官方文档是开始了解以太坊更多信息的好地方:

[](https://ethereum.org/en/developers/docs/) [## 以太坊开发文档| ethereum.org

### 介绍 ethereum.org 开发者文档。

ethereum.org](https://ethereum.org/en/developers/docs/) 

# 5.智能合同

智能合约是区块链上执行合约的不可变代码。智能合约类似于 JavaScript 中的类。这些是用来给 DApps 供电的。

仅仅理解智能合同的概念是不够的，你还应该能够开发它们。这就是可靠的来源。Solidity 是一种面向对象的高级编程语言，专门用于轻松构建智能合约。

但是，由于 Solidity 非常新，学习它的资源非常少。最好的学习方法是通过参考文档来构建项目和解决您面临的问题。让我们看一些遵循类似模式的资源。

## 1.建筑空间

Buildspace 是一个基于群组的学习平台，也是学习 Web3 的最佳资源之一。你可以用 Solana，Polygon 和 Ethereum 构建 DApps，NFT 集合，NFT 浏览器游戏，DAOs 等等。

你还可以在项目完成时获得一个免费的 NFT，这真是太酷了！你可以进入他们的专属招聘栏，那里是最大的三家网络公司招聘的地方。

[](https://buildspace.so/) [## 建筑空间

### 开始建立很酷的 web3 项目，获得 NFTs，在 crypto 中获得秘密工作机会。

buildspace.so](https://buildspace.so/) 

## 2.LearnWeb3DAO

学习 Web3 DAO 是 Web3 的另一个神奇资源。它有 4 个不同的轨道—大一，大二，大三，大四，适合不同技能水平的开发人员。您将学习构建 DApps、NFT 集合、ICO 令牌、Dao、DeFi 协议等等。

哦，我有没有提到这是因为每月 0 美元的高额费用？不仅如此，您还可以与 1000 名其他开发人员一起构建项目，参与黑客马拉松，并结交终生的朋友！

[](https://www.learnweb3.io/) [## 学习网络 3 道

### LearnWeb3:帮助开发人员进入 Web3

www.learnweb3.io](https://www.learnweb3.io/) 

## 3.神秘僵尸

你喜欢游戏吗？你会喜欢这个的。CrytoZombies 是一个游戏化的编程课程，在这里你可以使用智能合约建立一个僵尸工厂。你应该从其他资源中学习，并使用 CrytoZombies 练习你的技能。

[](https://cryptozombies.io/) [## #1 Solidity 教程&以太坊区块链编程课程| CryptoZombies

### CryptoZombies 是最受欢迎的交互式 Solidity 教程，它将帮助您学习区块链编程…

cryptozombies.io](https://cryptozombies.io/) 

## 4.纳德·达比特

纳德·达比特谈到:

1.  反应
2.  Web3
3.  无服务器
4.  区块链
5.  挑战

如果你对 web 开发或 web3 感兴趣，你应该订阅他的 youtube 频道:

[https://www.youtube.com/c/naderdabit/featured](https://www.youtube.com/c/naderdabit/featured)

## 5.自由代码营

FreeCodeCamp 发布了一个 16 小时的关于稳健的免费课程。你会学到所有关于稳健、区块链和智能合约的知识。

本课程将向您全面介绍区块链的所有核心概念，智能合约、可靠性、NFTs/ERC721s、ERC20s、编码分散金融(DeFi)、python 和可靠性、Chainlink、以太坊、可升级智能合约和全栈区块链开发。

[](https://www.freecodecamp.org/news/learn-solidity-blockchain-and-smart-contracts-in-a-free/) [## 在 16 小时的免费课程中学习可靠性、区块链和智能合约

### 区块链工程师需求量极大。几乎每天他们都在构建价值数十亿美元的应用程序。我们只是…

www.freecodecamp.org](https://www.freecodecamp.org/news/learn-solidity-blockchain-and-smart-contracts-in-a-free/) 

# 6.将您的智能合同与您的前端连接

既然你已经知道如何开发智能合同，你需要实际使用它们。有两个主要的库可以做到这一点——web3 . js 或 ethers.js。让我们看看为什么 ethers.js 比 web 3 更好:

1.  小得多的尺寸
2.  更少的错误
3.  更好的文档
4.  更受欢迎
5.  对初学者来说更容易
6.  额外功能

这里有一个关于 ethers.js 的很棒的教程:

我建议也学习 web3.js，因为有些代码库可能会用到它。web3.js 上有一个速成班:

> 查看此回购了解更多—【https://github.com/adrianmcli/web3-vs-ethers 

# 7.魔力

Alchemy 是一套开发人员工具，用于更快地原型化、调试和交付产品。炼金术支持各种链，像以太坊，多边形，斯塔克尼，流等等。它有一个很棒的 NFT API，可以让你轻松启动和运行你的 NFT 收藏。它还支持 web3.0 推送通知，并且它还有一个超级区块链 api！

[](https://www.alchemy.com/) [## 炼金术-区块链 API 和节点服务|以太坊、多边形、流量、Crypto.org+更多

### 炼金术超节点炼金术超节点是应用最广泛的区块链 API，用于以太坊，多边形，预言，乐观…

www.alchemy.com](https://www.alchemy.com/) 

# 8.再搅拌

Remix 是一个专门使用 Solidity 构建以太坊智能合约的浏览器 IDE。不需要任何设置，您可以立即开始编写代码。

它为您编译代码，并帮助您轻松地测试它。不仅如此，您还可以轻松部署您的智能合同。

[](https://remix-project.org/) [## 混合以太坊 IDE 和社区

### 请随时联系我们，我们是一个 7 人团队，主要在柏林(德国)、美国、印度远程工作…

remix-project.org](https://remix-project.org/) 

# 9.建筑工人

虽然 Remix 很棒，但有时这还不够，我想念我的 42 个扩展的 VS 代码设置。如果你使用 VS 代码，你需要一个本地以太坊环境。这就是安全帽发挥作用的地方。

Hardhat 帮助您轻松部署合同、运行测试和调试 Solidity 代码。您可以在各种网络上部署您的合同，如 Ropsten、Rinkeby、Mainnet 等。哦，它还支持打字稿🤩

> 我喜欢在 Remix 上构建和测试我的智能合约的功能，然后在完成后将其转移到 VS 代码中。

这里有一个关于安全帽的教程:

# 10.松露

Truffle 是我的智能合约开发工具。它帮助您轻松地编译您的智能合约，并在您的前端代码中使用它。Ganache 还带有 Truffle，它模拟了区块链，添加了测试帐户等。避免样板文件真的很有帮助。

帖子到此结束！如果你喜欢，留下一个(或两个)掌声，并关注我，不要错过任何帖子。查看我的公司网站,在那里我帮助 SaaS 公司制造令人敬畏的产品。再见了。