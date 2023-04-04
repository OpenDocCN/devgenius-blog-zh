# 你听说过 Faker.js 的新闻吗？

> 原文：<https://blog.devgenius.io/have-you-heard-about-the-faker-js-news-3dec01bbdf9b?source=collection_archive---------13----------------------->

![](img/533a66fc089e7fe5a7f1531e96f9749b.png)

Faker 是一个为你生成虚假(但合理)数据的库。模拟数据。用于测试、开发等的数据。

Faker 于 2004 年由 Jason Kohles 在 Perl 中首次实现，此后被移植到许多语言中，包括 [Ruby](https://github.com/faker-ruby/faker) 、 [Python](https://faker.readthedocs.io/en/master/) 、 [Java](https://github.com/DiUS/java-faker) 、 [Clojure](https://github.com/paraseba/faker) 、 [PHP](https://fakerphp.github.io/) 和 [C#](https://github.com/bchavez/Bogus) 。

这是 JavaScript 端口。

Faker 项目是由 Marak Squires 维护的，他是一个早期的有影响力的 Node 爱好者和专业人士，他在 2022 年 1 月 4 日[变得无赖和恶意](https://www.bleepingcomputer.com/news/security/dev-corrupts-npm-libs-colors-and-faker-breaking-thousands-of-apps/)。包被删除，项目被放弃。

流行的开源库“colors”和“faker”的用户在看到他们的应用程序后感到震惊，他们使用这些库，打印乱码数据并破坏。

一些人猜测，如果 NPM 图书馆已被攻陷，但事实证明有更多的故事。

Marak 采取的这些行动的效果，影响了数百万用户和数千个项目。

仅在 npm 上， *colors* library 就获得了超过[2000 万的周下载量](https://www.npmjs.com/package/colors)，并且有将近 19，000 个项目依赖于它。鉴于， [*faker*](https://www.npmjs.com/package/faker) 在 npm 上获得超过 280 万的每周下载量，并拥有超过 2500 名家属。

最初，用户怀疑这些项目使用的库“colors”和“faker”受到了威胁，类似于去年 [coa、rc、](https://www.bleepingcomputer.com/news/security/popular-coa-npm-library-hijacked-to-steal-user-passwords/)和 [ua-parser-js](https://www.bleepingcomputer.com/news/security/popular-npm-library-hijacked-to-install-password-stealers-miners/) 库被恶意参与者劫持的情况。

但是，事实上，正如 BleepingComputer 所看到的那样，这两个包背后的开发人员似乎故意[提交了](https://github.com/Marak/colors.js/commit/074a0f8ed0c31c35d13d28632bd8a049ff136fb6?diff=unified)造成重大错误的代码。

一个被破坏的*冒牌货* [版本 6.6.6 发布到](https://github.com/Marak/faker.js/blob/master/package.json#L4) GitHub 和 npm。

开发者[嘲笑](https://github.com/Marak/colors.js/issues/285)“我们注意到在 1.4.44-liberty-2 版本的 colors 中也有一个错误。”。

“请注意，我们正在努力解决这个问题，很快就会有解决方案。”

[Zalgo 文本](https://www.bleepingcomputer.com/news/technology/why-certain-characters-glitch-gmail-youtube-and-twitter/)是指某些看起来不正常的非 ASCII 字符。

开发商这种恶作剧背后的原因似乎是报复——针对大公司和开源项目的商业消费者，他们广泛依赖免费和社区支持的软件，但据开发商称，并没有回馈社区。

2020 年 11 月，马拉克警告说，他将不再用他的“免费工作”支持大公司，商业实体应该考虑要么分叉项目，要么以每年“六位数”的工资补偿开发人员。

“恕我直言，我将不再用我的免费工作来支持财富 500 强(以及其他小公司)。别的就不多说了”之前[的开发者写](http://web.archive.org/web/20210704022108/https://github.com/Marak/faker.js/issues/1046)。

“把这个作为一个机会，给我一个六位数的年度合同，或者叉这个项目，让别人来做。

有趣的是，到今天为止，BleepingComputer 注意到 faker 的 GitHub repo 的 [README](https://github.com/marak/Faker.js/) 页面也被开发者修改为引用[艾伦·施瓦茨](https://en.wikipedia.org/wiki/Aaron_Swartz):

“艾伦·施瓦茨到底发生了什么？”

斯沃茨是美国程序员、企业家和著名的黑客行动主义者，他在一场法律诉讼后自杀。

为了让所有人都能免费获取信息，黑客行动主义者从麻省理工学院校园网上的 JSTOR 数据库下载了数百万篇期刊文章，据称是通过反复轮换他的 IP 和 MAC 地址来绕过 JSTOR 和麻省理工学院设置的技术障碍。

在这样做的过程中，斯沃茨可能触犯了计算机欺诈和滥用法案，并面临刑事指控，最高可判处 35 年监禁。

# 神秘的蠕虫罐

马拉克的大胆举动打开了一个难题，引起了不同的反应。

开源软件社区的一些成员赞扬了开发者的行为，而其他人对此感到震惊。

一名用户在推特上写道:“显然‘colors . js’的作者对没有得到报酬感到愤怒[原文如此]…所以他决定每次他的图书馆被加载时都印上美国国旗……WTF。”。

一些[称](https://twitter.com/vaultec81/status/1479991084587499521)这是“又一个 OSS 开发者耍流氓”的例子，而信息安全专家 *VessOnSecurity* 称这一行为为“[不负责任的](https://twitter.com/VessOnSecurity/status/1480189534625320960)”，声称:

“如果你对商业免费使用你的自由代码有问题，不要发布自由代码。通过破坏你自己广泛使用的东西，你不仅伤害了大企业，也伤害了任何使用它的人。这训练人们不要更新，因为东西可能会坏。"

据报道，GitHub 已经暂停了该开发者的账户。这也引起了不同的反应:

“从[GitHub]中删除自己的代码违反了他们的服务条款？WTF？这是绑架。我们需要开始分散自由软件源代码的托管，”[软件工程师塞尔吉奥·戈麦斯](https://twitter.com/sgomez/status/1480148595059965956)回应道。

“我不知道发生了什么，但我在 GitLab private instance 上托管了我所有的项目，只是为了让这样的事情发生在我身上。永远不要相信任何互联网服务提供商，”[另一个人发推文](https://twitter.com/Lakr233/status/1480178455736033282)。

"马拉克设计了 faker 和 colors，阻止了数以吨计的项目，并期望什么都不会发生？"[陈述](https://twitter.com/piemadd/status/1480017317278978048)一个叫皮耶罗的开发者。

请注意，马拉克令人惊讶的举动是在最近引起互联网大火的 [Log4j 崩溃之后。](https://www.bleepingcomputer.com/news/security/new-zero-day-exploit-for-log4j-java-library-is-an-enterprise-nightmare/)

开源库 Log4j 广泛用于各种 Java 应用程序，包括由公司和商业实体开发的应用程序。

但是，在大量利用 Log4shell 漏洞后不久，开源库的维护者在假期无偿工作来修补项目，因为越来越多的 CVE 被发现。

人们开始担心大企业如何习惯于“T2”利用“T3”开源软件；不停地消耗它，却没有回馈足够的钱来支持那些放弃空闲时间来维持这些重要项目的无偿志愿者。

一些人还批评了网民和 bug 赏金猎人对 Log4j 维护者的穷追猛打，他们已经“在不眠不休地研究缓解措施；修复、文档、CVE、查询回复等。”【 [1](https://twitter.com/yazicivo/status/1469349956880408583) 、 [2](https://twitter.com/pentestscraps/status/1473707415635963919) 、 [3](https://twitter.com/GossiTheDog/status/1469848362325270530) 。

一位推特用户写道:“对 colors.js/faker.js 作者破坏他们自己的软件包的回应确实说明了有多少企业开发者认为他们在道德上有权享受开源开发者的无偿劳动而不做出任何回报。”。

关于 [OSS 可持续性问题](https://github.blog/2019-01-17-lets-talk-about-open-source-sustainability/)，时间会告诉我们开源软件的未来。

与此同时,“颜色”和“faker”NPM 项目的用户应该确保他们没有使用不安全的版本。降级到较早版本的 colors(例如 1.4.0)和 faker(例如 5.5.3)是一种解决方案。

# 相关文章:

[npm 依赖性正在破坏一些 React 应用程序——修复方法如下](https://www.bleepingcomputer.com/news/security/npm-dependency-is-breaking-some-react-apps-today-heres-the-fix/)

[GitHub 代码扫描现在发现更多安全漏洞](https://www.bleepingcomputer.com/news/security/github-code-scanning-now-finds-more-security-vulnerabilities/)

[我们目前所知的所有 Log4j、logback 错误，以及为什么你必须放弃 2.15](https://www.bleepingcomputer.com/news/security/all-log4j-logback-bugs-we-know-so-far-and-why-you-must-ditch-215/)