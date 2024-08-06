---
title: 使用 Flutter 加速应用开发
toc: true
---

![]({{site.flutter-files-cn}}/posts/images/2021/04/c1e6141049825.gif)

*作者 / Larry McKenzie*

> 本文由 eBay 技术负责人 Larry Mckenzie 和 Corey Sprague 撰写。你可以收听他们在 Google [Apps, Games & Insights 播客](https://zhuanlan.zhihu.com/p/337692560) 中的讨论，详细了解如何使用 Flutter 构建应用。

构建原生应用本来就不轻松，如果需要在不到一年的时间内构建出一款应用，同时还要满足目标受众的严格要求，就更加困难了。我们在构建 eBay Motors 应用时，就遇到了这样的挑战。但在 Google 的界面工具包 Flutter 的帮助下，我们得以快速针对 Android 和 iOS 平台提供优质且一致的体验。

## **eBay Motors**

过去十几年来，eBay 在汽车市场的业务一直十分活跃。但在几年前，我们的领导层发现了一个机遇: 我们可以在 eBay 平台提供定制化的车辆买卖服务。购买汽车不同于大多数在线购物: 购车是一项较为重大的消费，而且销售过程中存在一些特殊的问题需要考虑。eBay 倾向于上架经典车型、稀有车型、改装车等独特品类的车辆。eBay卖家往往想要为自己的爱车找到真正懂得欣赏和珍惜的买家。很多 eBay 买卖双方都在寻找更加个性化、更为独特的服务，所以我们想为这些用户构建一款特殊的应用——eBay Motors 应用便应运而生。

## **目标受众**

eBay Motors 的目标受众是广大汽车爱好者。尽管爱好者们每天都会关注汽车，但可能几年才会买一次车。因此，打造一款能够让他们愿意定期使用的应用并不容易。开发团队着重研究了这些人的使用动机，以及他们希望与其他汽车爱好者互动的方式。

买家可能正在物色下一辆车，或者只是随便看看或寻找灵感。在考虑购买车辆时，买家希望在决定购买之前充分获取相关信息。他们希望能够从各个角度查看车辆 - 内饰、外观、发动机、传动系统。他们想了解车况，也想了解卖家。当前车主的用车情况怎样？他们每天都开车吗？车辆是否在修理厂维修抛光过？车辆是每年行驶一次，只在车展上展出吗？我们想构建一种可以充分满足这些需求的体验。

开发团队在与卖家谈论出售车辆方面的问题时，发现卖家明显格外关心买家是谁，而并非总是关心价格。一些卖家表示，如果他们觉得买家真的会悉心呵护他们的爱车，那么降价出售并非不可接受。这种态度与传统汽车市场大相径庭，在传统汽车市场中，卖家总是希望能够以最高价格出售车辆。

在调查中，人们认为买卖双方的关系并没有在交易达成后就结束。卖家希望能够跟踪爱车情况，并继续与他们的买家交流。买家同样希望保持联系，以便询问: "嗨，我想改装这里，但我发现你之前改装过其他部位。可以给我讲讲具体情况吗？" 因此，我们希望构建一些功能，使买卖双方形成一个不仅仅只是讨论交易的社区。

## **选择 Flutter**

在开发这款应用时，eBay Motors 的领导层为开发团队提供了很大的自主权。唯一的要求是: 这款应用必须在一年内完成开发。根据我们的调查，显然这款应用必须包含 eBay 用户所期望的全部功能: 巨细无遗的商品详情页、拍卖、消息功能、搜索等。开发团队还积极地添加了有助于培养活跃社区的功能。这意味着我们的工作范围十分庞杂，我们认为，如果依然沿用两支独立平台开发团队的模式，将无法在截止日期前完成任务。

显然，我们需要一种新的应用开发方法。开发团队之前对跨平台开发工具进行过一些评估，但并没有找到令我们满意的解决方案。

但在这个项目开始时，Flutter 1.0 发布了。Flutter 是 Google 的界面工具包，可基于单个代码库为移动、网络和桌面端构建富有吸引力的原生编译应用。这看起来很有前景，所以我们的开发团队开始了更加详细的调查。

对 Flutter 进行彻底的评估后，开发团队对其相当满意。短短几周内，开发团队便确信 Flutter 是开发这款应用的最佳选择。

eBay Motors 之前组建了两支开发团队，分别采用不同的工作方式、工作安排和编程风格。在决定使用 Flutter 时，必须弄清楚应该如何协调两支开发团队。由于时间紧迫，因此开发团队在如何构建应用方面必须达成一致。所有人都秉持着相同的目标 - 打造高品质的产品、快速交付并超越预期。

两支团队开会讨论了所有分歧，尽管双方各有让步，但最终结果非常协调，这也为顺利发布产品做好了充分准备。

两支团队都缺乏 Flutter 和 Dart 的使用经验，但官方提供的指引非常详尽，学习起来非常简单。

## **首个版本**

我们在 2019 年 3 月收到了第一份产品需求——我们需要在三个月内向我们的 CEO 交付包含可用交易体验的 Beta 版应用。

第一个版本需求因为时间紧张，迫使团队大幅削减应用的功能。我们知道，即便在 Flutter 的帮助下，我们也不可能一次性解决所有问题——无法构建出包含所有买卖功能的完整社区和完善的消息功能。我们必须制定合理的开发策略，确定需要优先开发的重点功能，而非同时开发所有功能。团队通过讨论削减哪些功能，总结出了大量关注重点。这样我们就确定了研究重点，以及随之而来的开发重点。每个微小变化都会解锁下一个变化，使我们共同朝着里程碑一步步迈进。我们积极地关注问题，专注地解决用户希望我们处理的细节。这一过程困难且常常凌乱不堪，但在 Flutter 的强力助推下，我们以意想不到的方式加速追赶上了一个个任务节点。每到达一个节点，团队都会为之振奋。

实现首个里程碑之后，我们还要攻克多个后续里程碑，而 Flutter 使得我们能够一次性解决问题，并继续向前迈进。我们的产品从交付给 CEO 的 Beta 版发展为 eBay 内测 Beta 版，供数千名 eBay 用户使用。一个月后，我们的产品进入了公开 Beta 版测试阶段。终于，我们于 2019 年 12 月底正式发布了 iOS 和 Android 版应用。

对于 eBay Motors 而言，每个时刻都意义非凡。开发团队也通过这些时刻一步步地建立了信心。将产品交付给新用户、获得反馈，并进行更正和调整，我们可谓 "边做边学"。朝着目标快速地交付小规模迭代，也有助于我们的产品团队取得成功。即使应用的功能和复杂程度不断提高，Flutter 也让我们能够持续快速地交付产品。

由于在发布时大幅削减了应用功能，导致此时我们并没有聊天和社区功能。因此，在 2020 年 1 月，开发团队着手从零开始构建我们的社区，并在发布后的几个月内增加了 50% 的应用功能。

## **Flutter 的影响**

在刚开始采用 Flutter 时，我们只是将其视为一种支持共享代码并节省时间的工程工具。但 Flutter 帮助开发团队解决了许多我们并未期待它能解决的问题。在某些情况下，它甚至解决了我们未知的问题。

我们希望确保用户界面在各个平台之间的一致性: 将用户体验重点凝聚在 eBay Motors 品牌上，而非迎合特定平台的设计语言。但是，产品必须保留所在平台的具体行为，例如滚动的物理效果以及导航机制。幸运的是，Flutter 以开箱即用的方式使这些问题迎刃而解。这意味着设计师可以省去很多工作，因为他们只需要制作一份与平台无关的设计即可。

在产品需求方面，我们希望平台之间不存在差异。由于每项需求只需实现一次，所以减轻了我们不少的负担。我们开会的次数减少了，事半功倍。

随着开发团队对 Flutter 的了解日益加深，我们意识到，代码共享可谓意义非凡。在我们的代码库中，我们在 Android 和 iOS 之间共享了 98.6% 的代码。只有大约 0.5% 的代码为平台特定的原生代码。其余部分包括我们的持续集成流水线、自动化工具和开发者支持。

在开发过程中，通过共享代码可以节省大量资金，而在测试方面也同样如此。在开发团队曾使用过的所有平台当中，Flutter 在测试方面是佼佼者之一。它的测试功能帮助我们加快了进度，并让我们在发布产品时信心十足。我们从一开始就采取了强制执行 100% 代码覆盖率的策略，Flutter 让我们能够轻松实现这一目标。

随着应用进入生产阶段，节省资金的优势也在持续体现。由于我们不必单独处理 iOS 或 Android 错误，因此支持内容的可预测性要高得多。开发团队可以将新的版本发布到更具包容性的 Android 测试版渠道，之后再充满信心地发布 iOS 版本。由于只需构建和支持一个应用，我们能够节省大量资金。

## **Flutter 入门技巧**

如果是初上手 Flutter，在解决移动应用中的某些问题的时候，必须摆脱先入为主的观念，因为 Flutter 所使用的范式与传统 iOS 和 Android 截然不同。当你理解了 Flutter 所用的应用构建方式后，一切都将水到渠成。最好的学习方法是花些时间研究 Flutter 代码库。

Flutter 的一大优点就是开源。你可以查看 Flutter 团队是如何构建每个组件的，从中可以收获很多有价值的信息。

Flutter 社区也非常活跃。如果你需要 Flutter 框架尚未提供的解决方案，那么很可能已经有人构建了解决这个问题的 package。我们鼓励你的团队探索并查找所需信息，也欢迎你贡献自己的专业知识并积极拥抱开源文化。

## **Flutter 的前景**

在 eBay Motors 刚开始使用 Flutter 时，还鲜有大型公司公开使用该框架: 它当时仅是一款小众工具包，似乎还缺乏实战检验。时至今日，这个框架势头愈发强劲，很多公司向其敞开了怀抱。令人兴奋的是，Flutter 提供了最为现代且最为优秀的应用开发体验。而最棒的一点是，它仍在不断地发展与改进。

随着 Flutter 的发展，Dart 得到了大量的资源投入。Swift 和 Kotlin 中诸如空安全等一些必不可少的特性得到了积极的引入。看到 Flutter 的持续发展，以及这种发展为构建 Flutter 应用所带来的改进，我们感到十分喜悦。无论公司体量大小，当大家逐渐意识到 Flutter 可以帮助他们基于单个代码库开发桌面、web 和移动端应用，自然也会有更多团队采用 Flutter。

## **结语**

虽然 eBay Motors 开发团队的所有成员都拥有原生 iOS 或原生 Android 开发背景，但在用过 Flutter 之后，所有人都不约而同地喜欢上了它。Flutter 的开发者体验明显好于以往: 它可以利用热重载功能让开发者在进行编码时就获得快速反馈，而且由于开发流程的顺畅，使得问题能够快速得到解决，这些都是非常有价值的优势。在两年的使用过程中，惊喜无处不在。我们常能听到同事大喊: "我爱 Flutter！我刚发现了一个新的妙招！"

对于我们这样规模的团队，如果没有选择 Flutter，就没办法完成 eBay Motors 应用的开发。它让我们能够将资源集中到一起，这是分别构建两个平台应用的模式所无法实现的。Flutter 是帮助我们加快开发进程的加速器。

![]({{site.flutter-files-cn}}/posts/images/2021/04/3ac77f1134a10.gif)

更多 Google Play 开发者播客节目，请移步《[Apps, Games & Insights 播客节目合辑](https://www.ximalaya.com/keji/34766927/)》，了解不同领域的开发者通过多种视角与主题，探讨海外市场开发与发行的经验心得。

你对使用 Flutter 构建应用有何想法？欢迎在评论区分享你的评论。