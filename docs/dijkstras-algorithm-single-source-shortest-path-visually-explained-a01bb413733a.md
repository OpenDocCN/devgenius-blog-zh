# Dijkstra 算法:单源最短路径直观解释

> 原文：<https://blog.devgenius.io/dijkstras-algorithm-single-source-shortest-path-visually-explained-a01bb413733a?source=collection_archive---------12----------------------->

![](img/1fd89a16ac2fb9e4a42ffa39e5f8a647.png)

ijkstra 的算法可以应用于有向或无向图，以找到从单个源到每个顶点的最短路径。它只能与非负的边权重一起使用。

![](img/6f91a111e6557c72dacd78770d46ec1b.png)

生成一个表来跟踪到源顶点的距离。除了源本身，到每个顶点的距离被初始化为无穷大。另一个变量π用于跟踪到达指定顶点的前一个顶点。

![](img/37c043f1a024a48814387b851de0cb5a.png)

Dijkstra 的算法从源 S 开始，按照字母顺序放松与其直接相连的边。要松弛的第一条边是从 S 到 A。A 的值从无穷大更新为 5，前一条边更新为 S。边 S-A 也被标记为完成。

![](img/6c519b18f7877424fd185742938700d1.png)![](img/e80779621e6f306e537189f7d6a0b8a0.png)

接下来，放松边缘 S-I。到 I 的距离被更新为 15，并且前趋被标记为 S。边 S-I 被标记为已访问。来自 S 的所有出站边都已被访问，因此 S 被标记为已完成。

![](img/2f8a91eca80030f54f14c00ae44a8335.png)![](img/8f1d743f2196cbc905a757092f47a62c.png)

Dijkstra 算法从顶点 S 移动到顶点 A。选择顶点 A 是因为它是离源最近的、被访问过的顶点。重复相同的过程:从顶点 a 开始访问所有直接的出站顶点。这些顶点是 C 和 e。首先访问顶点 C，因为它是按字母顺序排列的。到 C 的距离被更新为 8，因为可以在 5 个单位内到达顶点 A，并且可以在另外 3 个单位内到达顶点 C。前任设置为 A，边 A-C 标记为已访问。

![](img/d311569a4f078ef79d8a618dfc752229.png)![](img/a45327a52b0f0e3ad8815bf8cee989c3.png)

接下来访问顶点 E。到顶点 E 的距离被更新为 7 (S-A-5 + A-E-2 ),并且 E 的前身被更新为 A。由于来自 A 的所有出站边都已被访问，所以顶点 A 被标记为完成。

![](img/8eb4abab1f99c6b1e74c9973e7bc301c.png)![](img/2bdd4fa8b44c1d5286623d2edeae716d.png)

Dijkstra 的算法选择下一个最近的顶点。顶点 I 当前距离源 15 个单位，顶点 E 距离源 7 个单位，顶点 C 距离源 8 个单位。顶点 E 是最近的，所以接下来选择它。因为从顶点 E 没有出站边，所以它被标记为完成。

![](img/f01a88f842843a1a4bf99ecce1232a20.png)

因为顶点 C 在 8 个单位之外，而顶点 I 在 15 个单位之外，所以接下来选择顶点 C。顶点 C 有 3 条出站边:C-B、C-D 和 C-H。顶点 B 最先被访问，因为它按字母顺序是列表中的下一个。到 B 的距离被更新为 15 (S-A-C-8 + C-B-7 ),并且前趋被设置为 C。边 C-B 被标记为已访问。

![](img/c7a743e65743c8ed4a6236ce091c3da5.png)![](img/4ab13c34d896a580e27e3b1a9b8d90e2.png)

Dijkstra 的算法接下来访问顶点 D。到顶点 D 的距离被更新为 14 (S-A-C-8 + C-D-6 ),并且 D 的前身被设置为 C。边 C-D 被标记为已访问。

![](img/e0cab275dd4f28bc1b435afe75a8d5b9.png)![](img/5a56934aba36a56e8043c05f458fa220.png)

访问来自 C 的最后一条出站边，即边 C-H。到顶点 H 的距离被更新为 11 (S-A-C-8 + C-H-3 ),并且到 H 的前趋被更新为 C。由于已经从 C 访问了所有出站边，所以顶点 C 被标记为完成。

![](img/2ed455ce36b60be7176d43004f7c2f85.png)![](img/6b241fccd9693b5c5fc76b4bb370f3bf.png)

Dijkstra 的算法选择下一个顶点。顶点 I 距离源 15 个单位，顶点 B 距离源 15 个单位，顶点 D 距离源 14 个单位，顶点 H 距离源 11 个单位。因为顶点 H 是离源最近的顶点，所以接下来选择它。顶点 H 有两条出站边:H-C 和 H-D。首先访问顶点 C。顶点 C 已经可以在 8 个单位内到达，所以没有点可以在 20 个单位内到达。到顶点 C 的距离保持不变，边 H-C 被标记为已访问。

![](img/d26e45a1450e6421c80112b80796e2d7.png)

由于经由边 H-D 到顶点 D 的距离(12 个单位)比经由边 C-D 到顶点 D 的距离(14)短，所以到顶点 D 的距离被更新为 12；顶点 D 的前身被更新为 H。边 H-D 被标记为完成；因为已经访问了来自 H 的所有出站边，所以 H 被标记为完成。

![](img/e81f0cb2bc7ede92e55d14228c9e46cd.png)![](img/984505e05d13aaf68731e45bc023f3a0.png)

Dijkstra 的算法通过检查所有可用的、未访问的顶点来选择下一个最近的边。15 个单位可以到达顶点 I，12 个单位可以到达 D，15 个单位可以到达 B。因为 D 是离源最近的顶点，所以选择 D。顶点 D 有一条向外的边 D-I，从 D 到顶点 I 的距离是 14。由于通过边 D-I 可以在 14 个单位内到达顶点 I，因此到顶点 I 的距离被更新为 14，并且前趋被设置为 D。边 D-I 被标记为已访问。顶点 D 的所有出站边都已被访问，并且顶点 D 被标记为完成。

![](img/11b387666953643457b50c7f868cd376.png)

Dijkstra 的算法检查所有可以访问的边。顶点 I 距离 14 个单位，顶点 B 距离 15 个单位。因为顶点 I 是离源最近的一个，所以接下来选择顶点 I。

![](img/31e6e5f377a938afe9629488ba299728.png)

顶点 I 没有出站边，因此顶点 I 被标记为完成。顶点 B 是源中唯一可用的、未被访问的边，因此接下来选择顶点 B。

![](img/e61dc7e0fce00a796c2ae7eec004bf37.png)

顶点 B 有一条出站边:B-F，顶点 F 可以用 19 个单位
(S-A-C-B-15 + B-F-4)到达。到 F 的距离被更新为 19，并且前趋被设置为 B。边 B-F 被标记为已访问，并且因为没有来自顶点 B 的其他出站边，所以它被标记为完成。

![](img/c1a87775246e95a9d50db92cd609018d.png)![](img/ac958c16292a231cb414ef228d78607e.png)

顶点 F 是离源最近的顶点。顶点 F 有一条向外的边 F-G。顶点 G 可以用 21 个单位到达。到 G 的距离被更新为 21，并且 G 的前身被设置为 F。边 F-G 被标记为已访问，并且因为没有来自 F 的其他出站边，所以顶点 F 被标记为完成。

![](img/65a041d5e9bcd1ea4faf4fdbdf2b0927.png)![](img/85b7c020b4d9cc224547abb0a5088ee3.png)

顶点 G 是最后一个未被访问的顶点。顶点 G 有一条向外的边，从 G 到 B。通过边 G-B 到顶点 G 的距离是 21 个单位。因为通过边 C-B 到 B 的距离比通过边 G-B 到 B 的距离更近，所以到 B 的距离保持为 15°。边 G-B 被标记为已访问，并且因为来自 G 的所有出站边都已被访问，所以顶点 G 被标记为完成。有向图中的所有顶点都被访问过了，那么 Dijkstra 的算法就完成了。下面的列表显示了从源到每个顶点的最短路径。

![](img/956680067318dd6d3b078957970b71ef.png)![](img/a82f5f42bc5b550dddca1d6be1861d6a.png)

如果你喜欢你所读的，看看我的书，[](https://www.amazon.com/Illustrative-Introduction-Algorithms-Dino-Cajic-ebook-dp-B07WG48NV7/dp/B07WG48NV7/ref=mt_kindle?_encoding=UTF8&me=&qid=1586643862)**算法的说明性介绍。**

*![](img/27e5c00d1617c85d831ec941c19ad2f9.png)*

*Dino Cajic 目前是 [LSBio(寿命生物科学公司)](https://www.lsbio.com/)、[绝对抗体](https://absoluteantibody.com/)、 [Kerafast](https://www.kerafast.com/) 、 [Everest BioTech](https://everestbiotech.com/) 、 [Nordic MUbio](https://www.nordicmubio.com/) 和 [Exalpha](https://www.exalpha.com/) 的 IT 负责人。他还是我的自动系统公司的首席执行官。他有十多年的软件工程经验。他拥有计算机科学学士学位，辅修生物学。他的背景包括创建企业级电子商务应用程序、执行基于研究的软件开发，以及通过写作促进知识的传播。*

*你可以在 [LinkedIn](https://www.linkedin.com/in/dinocajic/) 上联系他，在 [Instagram](https://instagram.com/think.dino) 上关注他，或者[订阅他的媒体出版物](https://dinocajic.medium.com/subscribe)。*

*阅读 Dino Cajic(以及 Medium 上成千上万的其他作家)的每一个故事。你的会员费直接支持迪诺·卡吉克和你阅读的其他作家。你也可以在媒体上看到所有的故事。*