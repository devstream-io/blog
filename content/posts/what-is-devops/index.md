---
title: "什么是 DevOps？看这一篇就够了！"
subtitle: ""
author: "胡涛 | Daniel Hu"
authorLink: https://www.danielhu.cn
tags: ["devops", "中文文章"]
categories: ["General", "中文文章"]
date: 2022-08-01

draft: false
toc:
  auto: false
resources:
- name: "featured-image"
  src: "banner.jpeg"
---

## 一、前因

我是一个“DevOps 工程师”，于是总会遇到有人问我：“什么是 DevOps？”

这个问题看似特别基础，基础到很多人懒得回答。但其实冷静一秒，问自己一句“什么是 DevOps？”可能每个 DevOps 工程师都知道“什么是 DevOps”，但是他们给出的答案不尽相同。

所以我会怎么回答这个呢？下面我们展开来聊聊。

> 特别强调：本文仅代表我个人现阶段的粗浅认知，本文观点不代表[思码逸](https://www.merico.cn)公司也不代表 [DevStream](https://github.com/devstream-io) 团队。

## 二、记忆

我第一次看到 DevOps 这个词，大概是在2016年的秋天。那时候我在 [H3C](https://www.h3c.com/cn/) 从事云计算研发相关工作。记得我接到的第一个任务是研究 [OpenStack](https://www.openstack.org) 的一个 CICD 相关的组件，叫做 [Solum](https://docs.openstack.org/solum/latest/)，那是我第一次知道什么是 CICD，第一次看到 DevOps 这个词。没错，只是看到 DevOps，但是我无法记住 DevOps 的定义。或者说，当时我甚至没有找到一个清晰易懂的关于 DevOps 的定义。可能很多人和我当年一样，对 DevOps 的印象，就是 **Dev + Ops**。

2018年的夏天，我开始在太保成研任云平台 PaaS 组负责人，兼任太保云 CMO(Configuration Management Officer) 一职。没错，我依旧是一个“云平台研发工程师”，但是再一次与 DevOps 结缘。太保云的 CMO，简单说就是负责太保云平台的源码管理、研发协作流程、版本管理、CICD、制品管理、发版流程等等。这个时候我其实已经开始研究一些 DevOps 相关的工具了，比如 GitLab、Jenkins、禅道、Artifactory、Nexus 等等；同时也在主导一些 DevOps 文化层面的建设，比如怎样的模式或行为在团队里是被鼓励的，怎样的事情是被禁止的…… 不过我只是在制定规则，而没有意识到这是“文化”。总之，那几年我也算是投身于 DevOps，致力于提升团队研发效率、交付效率与交付质量，但是同时我没有去仔细思考过“什么是 DevOps？”这个问题，我也没有刻意去思考过自己是不是在玩 DevOps。

去年(2021年)年底，我加入了[思码逸](https://www.merico.cn)，我的 title 第一次从“xxx 云平台研发工程师”变成了“xxx DevOps 工程师”（xxx 表示初级、中级、高级等）。那天我开玩笑说：“**以前，我在云原生领域兼职玩 DevOps；以后，我在 DevOps 领域兼职玩云原生**”。

好吧，这会我是名正言顺的“xxx DevOps 工程师”了，我总该知道“什么是 DevOps”吧！

## 三、他们说……

我们先来看一下几家典型的公司是如何定义他们眼中的 DevOps 的，包括：
- [Atlassian](https://www.atlassian.com)（代表产品：Jira、Trello 等）
- [微软](https://www.microsoft.com)
- [AWS](https://aws.amazon.com)

### 3.1、Atlassian 回答“什么是 DevOps？”

Atlassion 有一篇题为[DevOps](https://www.atlassian.com/devops)的文章，里面有这样一句话：

> DevOps is a set of practices, tools, and a cultural philosophy that automate and integrate the processes between software development and IT teams. It emphasizes team empowerment, cross-team communication and collaboration, and technology automation.

我尝试翻译一下：**DevOps 是一系列`实践`、`工具`和一个融合开发及 IT 团队的`文化理念`。DevOps 强调赋能团队、跨团队沟通与协作以及技术自动化**。

可以看到 Atlassian 给的等式是：

**DevOps = 工具 + 实践 + 文化**

Atlassian 还提到一个 DevOps 团队包含了开发和 IT 运维，大家一起协作，共同参与产品的整个生命周期，一起为提升软件质量和加速软件开发过程而努力。DevOps 模式下开发和运维不再是独立的“筒仓”，而是几乎被整合成一个团队，这个团队的工程师技术栈会覆盖开发、测试、运维等。同时 DevOps 团队会利用一系列的 DevOps 工具链来实现诸如持续集成、持续发布、流程自动化、高效协作等等目的。

Atlassion 给的“无穷环”长这样：

{{< figure title="DevOps Lifecycle from Atlassion" src="devops-at.png" >}}

用“无穷环”表示 DevOps 生命周期，是因为 DevOps 的根本理念是“持续”，也就是“没有终点”。Atlassion 将整个 DevOps 生命周期分成6个阶段，分别是：

- 计划（Plan）
- 构建（Build）
- 持续集成和部署（或者交付）（Continuous Integration and Deployment or Delivery）
- 监控和告警（Monitor and Alert）
- 运维（Operate）
- 持续反馈（Continuous Feedback）

另外从这个环里我们还能看到 Atlassian 想强调**沟通与协作是贯穿 DevOps 生命周期全过程的**。

### 3.2、微软回答“什么是 DevOps？”

微软这篇 [Introduce the foundation pillars of DevOps: Culture and Lean Product](https://docs.microsoft.com/en-us/learn/modules/introduce-foundation-pillars-devops/) 我特别喜欢！这个标题的意思是“介绍 DevOps 的基柱：文化和精益产品”。

文章第一句话：

> DevOps is the union of people, process, and products to enable continuous delivery of value to our end users.

**DevOps 是人、过程和产品的结合，使能持续地向终端用户交付价值**。

微软还提到：

> Typically, the goal for Development is to deliver more features faster, and the goal of Operations is to achieve better system stability. DevOps aligns these disciplines by using a framework of best practices proven to increase speed to market while improving system stability.

多数情况下，开发的目标是快速发布更多的新特性，而运维的目标是保证更高的系统可用性。DevOps 通过切实可行的最佳实践体系来拉齐这两个目标，在提升系统稳定性的同时加速产品交付到市场的速度。

这里微软可以看到微软给的第一个等式：

**DevOps = 人 + 过程 + 产品**

然后微软从“人 + 过程 + 产品”进一步提炼了 DevOps 的4大基柱：**文化、精益产品、架构和技术**。

也就是：**人 + 过程 + 产品 -> 文化、精益产品、架构 + 技术**

微软给的“无穷环”长这样：

{{< figure title="DevOps Lifecycle from Microsoft" src="devops-ms.png" >}}

图里描绘的 DevOps 生命周期还是分成6个阶段，分别是：

- 计划（Plan）
- 构建（Build）
- 持续集成（Continuous Integration）
- 部署（Deploy）
- 运维（Operate）
- 持续反馈（Continuous Feedback）

外加贯穿整个 DevOps 生命周期全过程的“协作（Collaboration）”。

在图外，微软还定义了对其而言 DevOps 的8大能力：

- 持续计划（Continuous Planning）
- 持续集成（Continuous Integration）
- 持续发布（Continuous Delivery）
- 持续运维（Continuous Operations）
- 持续质量（Continuous Quality）
- 持续安全（Continuous Security）
- 持续协作（Continuous Collaboration）
- 持续改进（Continuous Improvement）

*每次看到这里我总觉得微软的图该更新一版*。

另外微软有一句特别有深度总结：

> What is new? Continuous Everything. The process is a journey and requires a growth mindset to continually evolve and improve.

“Continuous Everything”，铿锵有力！微软强调 DevOps 过程是一段没有终点的旅途，要求我们抱着成长的观念模式，持续地改进，永不满足。

### 3.3、AWS 回答“什么是 DevOps？”

不难猜到，AWS 也有[一篇文章](https://aws.amazon.com/devops/what-is-devops/)来回答“What is DevOps?”

> DevOps is the combination of cultural philosophies, practices, and tools that increases an organization’s ability to deliver applications and services at high velocity.

**DevOps 是文化理念、实践和工具等的组合，能够提升一个组织快速交付应用和服务的能力**。

这里 AWS 给了一个等式：

**DevOps = 文化 + 实践 + 工具**

不过这篇文章里 AWS 不落俗套，没有画一个自己的“无穷环”，而是给了这样一张图：

{{< figure title="DevOps Lifecycle from AWS" src="devops-aws.png" >}}

这里提到了：

- 构建（Build）
- 测试（Test）
- 发布（Release）
- 监控（Monitor）
- 计划（Plan）

还可以看到这个“交付管道”和“反馈环”连接的是“企业”和“客户”，可见 AWS 希望强调“DevOps 的目的是更快地向客户交付”。

## 四、DevOps 文化

我曾一度片面以为 DevOps 要解决的问题就只是工具问题，也就是如何选择或者开发好用的 DevOps 工具 or 平台，从而提升企业内部整个研发生命周期的运行效率。不记得是哪一天，我突然有一个强烈的想法：工具只是工具而已，文化建设才是成败的关键！

文化决定了我们如何去做事，工具决定了，决定了啥？可能啥也决定不了。因为我认为工具也是被文化所决定的。

### 4.1、什么是文化？

简单说，文化就是一个组织的社交遗产，也就是一个组织对于其成员的各种行为的响应模式。

比如当我们说一个企业有“加班文化”时，其实是在说在这个企业内，员工加班会得到奖赏，而不加班会受到惩罚。或者我们说一个企业是“狼性文化”、“奋斗者文化”…… 不同的文化背后对应的也就是这个企业对于员工不同行为的不同响应模式。

一个企业的文化决定了在这个企业内：

1. 什么事情是对的，什么事情是错的；
2. 什么事情是重要的，什么事情是不重要的；
3. 什么事情是值得做的，什么事情是不值得做的。

所以**文化决定了一个企业会去招聘哪些人，会开除哪些人，会提拔哪些人**。

看到这里可能你已经在思考自己呆过的企业对员工有哪些要求，在鼓励什么，在惩罚什么…… 没错，此刻在你脑海中闪现的一幕幕就是企业文化。

### 4.2、什么是 DevOps 文化？

这幅图大家肯定都不陌生：

{{< figure title="DevOps Lifecycle" src="devops.png" >}}

**什么是 DevOps 文化？**

其实从这幅图中我们就能看到文化的影子。我们都知道 DevOps 强调打通开发团队与运维团队的壁垒，要求两个团队拉齐认知与责任，不再各自为战，而是一起为更快地交付更高质量的产品而努力。没错，这就是最基础的 DevOps 文化。

**那么如何拉齐认知与责任呢？**

首先可以确认的是，我们在组织架构上直接融合 Dev 和 Ops 团队，这并不是一个 DevOps 团队。人是不是坐在一起，改变的只是沟通的效率。这里我想强调两点：

1. 责任共担，在一个 DevOps 模式组建团队里，每个人都需要为软件开发交付的整个生命周期而负责；
2. 技能共享，通过持续学习，互相学习，让本是传统 Dev 的工程师学习 Ops 的技能，同时传统 Ops 的工程师也需要学习 Dev 的技能。

Dev 与 Ops 互相学习彼此领域技能，每个人都懂开发又懂运维，抱着“成长的观念”，持续学习，不满足于当前已掌握的技术栈。

但是我们也需要意识到不能要求每个工程师都精通开发与运维，这是不可能的。这里说的 Dev 掌握 Ops 能力，更多的是 Dev 能够借助完善的工具链从而掌握“应用运维”的能力，能够在自己完成开发之后，有能力和权限将应用部署上线，同时线上应用出问题后，能够直接对其负责，定位、修复、更新升级等。而一些基础设施的运维能力需要独立出来考虑，比如机房里的局域网配置、虚拟机挂 NAS 盘等传统运维能力。

同理 Ops 需要理解应用开发的生命周期，知道 Dev 的痛点，尤其是在流程上的痛点，比如怎样提升应用的构建速度，怎样优化应用的 cd 流程等，Ops 要关注应用的“生产过程”，进而发力去优化这个过程或相应的工具，让应用能够更可靠更快速地完成 cicd 流程等，更容易地部署上线或者对外交付。也就是说我们并不是要求 Ops 也去写业务代码，而是协助 Dev 去解决业务代码之外的痛点，让 Dev 能够更加专注于业务功能实现。

最后，一个 DevOps 模式组件的团队中每个人都为整个软件研发生命周期的速度和质量负责，每个具体的角色就像一个大头钉，底部很宽，代表着技术面广，关注整个软件研发生命周期的所有环节；同时顶部很高，在某个环节里专注，做好做精。

**DevOps 成功落地的关键是什么？**

我们前面说到的“其乐融融”的场景，我们希望 Dev 和 Ops 能够互相学习，共担责任，一起为更快更好地交付产品而努力。但是，工程师们为什么要这样做？他们的动力在哪里？

### 4.3、领导与激励

[Gartner](https://www.gartner.com/cn/about) 曾出过一个分析报告，表明在2023年，90%的 DevOps 改革将会失败（相较于预期）。而失败的主要原因是领导层管理方法的局限。

其实这是显而易见的，DevOps 可以称为一种“改革”，而很多人是抵触“变化”，抵触“新事物”的。比如 DevOps 鼓励接受失败，快速失败，从失败中学习经验，进而在更长的时间维度上争取更大的成功。但是可能你遇到的刚好是一个“失败惩罚型”领导，那么你的团队就会惧怕失败，从而放弃创造与尝试新技术，选择安于现状。

一个技术团队的领导首先自己需要懂技术，有丰富的经验，这是基础要求。但是除此之外，更重要的是团队领导能够激励整个团队，去发挥整个团队的主观能动性，让所有团队成员都能够有动力持续学习，快速学习，同时也能够敢于失败，快速失败且不惧怕失败，把失败当做一个学习的机会，进而不断成长，让整个团队的战斗力能够越来越强。

**所以领导怎样激励工程师呢？**

福利？比如一些大厂提供的免费零食或者定期的下午茶？免费的咖啡或者午餐？

没错，作为一个工程师，这一切的福利都会让其开心，但是其实无法激励其更加认真努力地工作。工程师的薪资水平普遍不低，所有这些零食也好，咖啡也好，大概率不会到其月薪的零头。同理，工程师找工作时，看重的也绝不会是一个企业是否提供免费午餐和下午茶。

**那么工程师看重的是什么？**

在选择一家企业的时候，可能工程师第一个考虑的是薪资，剩下的可能是成长的空间、工作内容是否感兴趣等等等等。但是进入一家公司以后，真正开始工作的时候，工程师看重的是什么？我认为可能是：

- **精通**
- **自驱**
- **目标**

我们逐个来解释一下。

**1. 精通**

我们在某个工作方向做的好，我们擅长某个技术方向，进而很好地完成相应的工作，这时候我们会有一种成就感，满足感，我们会觉得自己得心应手，同时大概率会获得认可，赞扬，因此接下来的时间里我们就更加愿意在这个方向上继续努力，做的更好。也就是说一个工程师能够有机会专注于自己精通的技术上发力，那么他大概率会感受到激励。

反例是什么呢？比如你是一个 Java 工程师，但是你的领导擅长 PHP，并且觉得 PHP 是世界上最好的语言，于是要求整个团队转向使用 PHP，这时候你会放弃自己研究多年的 Java 技术栈，努力学习 PHP 并决心干出一番成绩吗？

**2. 自驱**

我们希望组建一个学习型、创造型的团队，每个人能够持续成长，乐于创新，自我驱动。这就需要领导能够允许团队花时间去学习，去输入，而不是一味地输出，每时每刻汇报自己写了几行代码。同时这也要求领导自身勇于接受新事物，拥抱变化，而不是“不求有功，但求无过”。举个例子：假如你的领导最担心的是线上应用出事故，并且他认为稳定的第一要素就是不要引入新技术，新工具，那么这时候你的领导也不会在意你是不是有时间学习，也不会允许你花时间去研究新技术，因为这一切只会带来不稳定。如果领导害怕失控，因而拒绝创新，那么这样的团队成员也就只能满足于实现日复一日的常规需求开发迭代，而不会享受技术，自我驱动，拥抱创新。

**3. 目标**

显而易见，团队每个成员都需要知道自己为什么做？目的是什么？目标是什么？而不是领导心里藏着一个目标，然后简单地指挥团队成员完成一件件具体的零散的工作项。如果团队成员只知道今天需要完成事务A，明天需要完成事务B，而不知道为什么要做，最终要做成什么样，那么大家只会满足于机械地完成任务，而不会有动力追求“如何做得更好”。

## 五、总结

**所以 DevOps 是什么？**

我尝试给出我的答案：

**DevOps 是一种文化理念、工具与实践的结合，目的是更快更可靠地向用户持续交付价值**。其中最重要的是**文化**，文化要求 Dev 和 Ops 团队责任共担，目标一致，也要求整个团队持续学习，抱着成长的心态，Continuously Everything。其次 DevOps 离不开高效的**工具**集，工具是自动化的基础。最后我们要在各个环节追求最佳**实践**，不管是工具的使用，还是团队的协作模式，沟通方法上面。

最后，关于标题“什么是DevOps？看这一篇就够了！”，我想告诉你，DevOps 文化里不存在“够了”，所以我不得不承认，我撒谎了。本文只代表我个人现阶段的**粗浅**认知，我建议你查阅更多的资料，持续学习，永不满足。当然如果本文对你有一点点的帮助，那么我很满足。
