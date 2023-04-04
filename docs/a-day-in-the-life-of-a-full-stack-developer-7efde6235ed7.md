# 全栈开发人员的一天。

> 原文：<https://blog.devgenius.io/a-day-in-the-life-of-a-full-stack-developer-7efde6235ed7?source=collection_archive---------3----------------------->

![](img/f60ed5fe76e62a819e6045bf07b1472b.png)

斯蒂芬·斯坦鲍尔在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄的照片

作为一个全栈开发者通常是一个令人沮丧的经历。即使是最简单的应用也需要你知道难以置信的数量。你必须知道 html 是如何工作的，如何用 [CSS](https://en.wikipedia.org/wiki/Cascading_Style_Sheets) 以百万种方式中的一种来设计 [HTML](https://en.wikipedia.org/wiki/HTML) 的样式，然后也许你想要某种交互。你马上会面对数百个 [web 框架/库](https://en.wikipedia.org/wiki/Comparison_of_web_frameworks)，它们都有不同的最佳实践来做本质上相同的事情。

也许你决定用[反应](https://reactjs.org/)。很快你就会面对现实，你将需要 [npm](https://www.npmjs.com/get-npm) / [yarn](https://yarnpkg.com/) 来管理你的[包](https://en.wikipedia.org/wiki/Package_manager)，[web pack](https://webpack.js.org/)/[browser ify](http://browserify.org/)/[roll up](https://rollupjs.org/guide/en/)来管理你的包，一个路由器，某种状态管理系统( [Redux](https://redux.js.org/introduction/getting-started) 或类似的)。你用[打字稿](https://www.typescriptlang.org/)吗？如果是这样，您现在需要 typescript 编译器，以及正确的构建脚本或 webpack 插件来完成这项工作。

设计这些怎么样？也许你会少跟[走](http://lesscss.org/)或者[顶嘴](https://sass-lang.com/documentation/syntax)..或者也许你决定 [JSS](https://cssinjs.org/?v=v10.1.1) 是一个更好的自包含组件的方法？也有像 [material-ui](https://material-ui.com/) 或 [react-bootstrap](https://react-bootstrap.github.io/) 这样的库给你更高级别的部件来组装，但是你仍然需要大量的[代码](https://codesandbox.io/s/38n3q?file=/demo.js)来完成任何有用的事情。

太好了，你有一个前端工作。现在你想在某个地方保存一些数据。您现在需要一个用于后端的堆栈。你用的是 [java](https://www.oracle.com/java/technologies/) 、[ASP.NET](https://dotnet.microsoft.com/apps/aspnet)、 [express](https://expressjs.com/) 、[土星](https://saturnframework.org/)吗？你使用什么数据库技术？ [Sql server](https://www.microsoft.com/en-in/sql-server/sql-server-2019) ， [Postgres](https://www.postgresql.org/) ， [Azure table](https://azure.microsoft.com/en-gb/services/storage/tables/) ， [Couchbase](https://www.couchbase.com/) ， [Redis](https://redis.io/) ..清单还在继续，每个都有自己的优点和缺点、集成库、部署复杂性和怪癖。你如何管理[连接字符串](https://en.wikipedia.org/wiki/Connection_string)和其他敏感的配置数据？

现在你对你的后端满意了，你需要让两者互相交流。我们又一次面临着错综复杂的局面。[序列化](https://en.wikipedia.org/wiki/Serialization)是如何处理的？如果 [Json](https://www.json.org/json-en.html) ，你有没有中间模型来处理数据和更高类型的破坏等。你如何保持你的后端和前端模型同步？我们是否使用了 [REST](https://en.wikipedia.org/wiki/Representational_state_transfer) ，RPC，或者 [websockets](https://en.wikipedia.org/wiki/WebSocket) ，或者甚至是它们的组合？我们如何从我们的 [GUI](https://en.wikipedia.org/wiki/Graphical_user_interface) 中触发加载和保存—我们是否需要存储集中状态来驱动我们的 [GUI](https://en.wikipedia.org/wiki/Graphical_user_interface) ，管理不完整的状态，如[验证](https://en.wikipedia.org/wiki/Data_validation)等？

太好了！你有所有的工作。现在，假设您的应用程序是基于用户的，我如何辨别哪个用户是哪个用户？现在有了 [OAuth](https://en.wikipedia.org/wiki/OAuth) 和更高抽象层次的 [OIDC](https://openid.net/connect/) 。然后是理解如何安全地管理令牌、如何将令牌附加到请求、在服务器上检查令牌签名、当用户拥有无效令牌时返回正确的错误等的复杂性。你用类似 [IdentityServer](https://identityserver.io/) 的东西运行你自己的服务器吗，或者也许你使用类似 [Okta](https://www.okta.com/discover/okta-for-worlds-largest-organizations/?utm_campaign=search_google_emea_uki_ao_it_branded-okta_exact&utm_medium=cpc&utm_source=google&utm_term=okta&utm_page={url}&gclid=Cj0KCQjwnv71BRCOARIsAIkxW9FJwy8sD17bkkTPDt47bIcKlsidMt5UJNyW-zHKknss61IAzx1l9jwaAqUREALw_wcB) 的服务。

我已经筋疲力尽了。请耐心等待，因为我们还远未完成！

然后就是[源码控制](https://en.wikipedia.org/wiki/Version_control)。人们普遍认为 [git](https://git-scm.com/) 是当今事实上的标准，但是你仍然需要在某个地方托管它，为你的贡献者管理 [ssh 密钥](https://www.ssh.com/ssh/key/)，选择[分支策略](https://www.creativebloq.com/web-design/choose-right-git-branching-strategy-121518344)等等。

如果你已经做到了这一步，那么是时候考虑持续集成和部署了——这本身就是一门科学。然后就是 [SSL](https://blog.hubspot.com/marketing/what-is-ssl) ， [DNS 配置](https://www.wpbeginner.com/glossary/dns/)， [SEO](https://en.wikipedia.org/wiki/Search_engine_optimization) ， [SSR](https://medium.com/@baphemot/whats-server-side-rendering-and-do-i-need-it-cb42dc059b38) ，analytics。

我几乎肯定跳过了这里的一些步骤，因为魔鬼总是在细节中。我的观点是，这个过程非常复杂，容易出错，耗费时间，对正确的决策来说是主观的。每种工具本身都是一种专长，一个人可能会花费数百个小时去理解如何正确使用一种工具。因为时间是有限的，因此不可避免地会误用工具，导致其他维护问题。毕竟不是每个人都能成为所有事情的专家！

在我们继续之前，我只想指出，我很清楚这是一个快速发展的领域。改进每天都在发生，所有级别都有很棒的模板可用，允许您快速搭建出以上部分的大块。问题是，如果你想做一些重要的事情，你仍然必须知道它是如何工作的，所以不管怎样，现实仍然是一样的——整合的痛苦和选择的麻痹！

# 当然，全栈环境是一个雷区，但我们正在努力解决一个难题！

毫无疑问，设计鲁棒的全栈解决方案所需的技能水平很高，甚至比 10 年前还要高。尽管工具已经有了长足的进步，但是还有很多东西需要了解。为解决全栈问题而设计的库和框架无疑是非常强大的，你几乎可以做任何你能想到的事情。这里的问题是，伴随着巨大的权力而来的是巨大的责任，在构成过程的任何层面上制造不可挽回的混乱变得越来越容易。我想在这里强调，这种权力确实有正当的时候。绝对有问题需要这种程度的控制，但事实是这些是少数。

人们普遍认为——如果你想写博客，你不会手工推出自己的博客引擎。这是一个已经解决的问题！基础网站也是如此。当然，你可以在 html 和 css 中完成所有这些，是的，它可能会更快，但对于刚刚起步的小公司来说，开发人员的费用高得惊人，一个非技术人员可以用 wix、squarespace、webflow 或其他数百个 [CMS](https://en.wikipedia.org/wiki/Content_management_system) 衍生产品的一小部分费用快速获得类似的结果。我知道你们这些 html 和 css 的铁杆完美主义者不愿意接受这一点，但重要的是要认识到，历史对其他行业的类似艺术家并不仁慈。

出版的合理化是一个教科书般的例子，说明软件如何毁灭了一个高技能工人的世界和他们的整个行业。随着台式电脑和软件的出现，如 Photoshop T1、T2、Quark Express T3、InDesign 和 Illustrator，设计和印刷面临着巨大的合理化。以前，分室专家准备复杂的印刷设计材料，使用笔、手术刀、胶片、蒙版、照片技术和排版，所有这些结合起来产生一个成品。设计师、视觉设计师、印刷工人、[排版工人](https://en.wikipedia.org/wiki/Typesetting)、拼贴艺术家、[石印工人](https://en.wikipedia.org/wiki/Lithography)…等等。等等。举例来说，照片编辑和润饰是一门非常熟练的艺术，很少有人能掌握。图像被渲染成印刷品或大幅面胶片；手工蚀刻和绘制极其细致的细节，使用染料和化学反应进行喷涂或重新着色。这些过程通常是严格保密的，技术人员几乎是炼金术士。不用说，他们是一个高回报的精英群体。Photoshop 一下子让它们变得多余。几年之内，出版界缩减了员工数量，工资也大幅下降。然而，毁灭带来了创造:随着互联网的发展，这些设计师可以利用他们的设计技巧来创造在线材料，这是以前没有的。

回到软件，我们现在有电子商务平台，我们有工作流平台，ML 组装平台，还有很多。这种趋势似乎不可避免地会继续下去，并慢慢占据越来越多我们今天所看到的“全栈开发”。

# 好吧，这是非常反乌托邦的，那么我们从这里去哪里？

软件开发的伟大之处在于它是最小的公分母。只要人工智能不能抽象地思考，不能产生能够跨越未知领域的代码，就永远需要有人来开发软件，让更多人可以使用工具。然而，不可避免的一件事是，形势正在发生变化。

在我看来，这里有两个部分在起作用，组件化和组装:

组件化是程序构建和重用的基础构件。总会有人需要一个新模块来做所有其他可用模块都不会做的事情。建造这些总是需要技巧和时间，因为无论做什么都是未知的领域。

拼图的第二块是组装——以任意方式将许多组件连接或组合在一起，以获得更大的最终结果。组装通常是一个技能要求较低的过程，通过项目计划间接编排，或者直接通过工具甚至代码组合来编排。毫无疑问，这是一个变革的领域，更高级的工具开始消除将组件组装成有用的东西的苦差事。如果你看看许多现有的解决方案，如 CMS、工作流工具等，它们都是组装工具。

在 [Leansquad](https://www.leansquad.co.uk/) 我们相信快速组装是未来的趋势，并试图通过想象一个系统来帮助塑造这一前景，在这个系统中，装配工和组件开发人员可以以更少的妥协共同工作。

*下次加入我们，我们将谈论我们在*[*lean squad*](https://www.leansquad.co.uk/)*开发的软件如何帮助解决这些棘手的问题，并拥抱即将到来的不可避免的革命。*