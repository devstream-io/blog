---
title: "如何开始参与开源社区"
subtitle: "致 DevStream 社区“明天”的 Contributor 们的一些“唠叨”"
author: "胡涛 | Daniel Hu"
authorLink: https://www.danielhu.cn
tags: ["devstream-team", "devstream", "how-to", "中文文章"]
categories: ["DevStream Team", "中文文章"]
date: 2022-05-22

resources:
- name: "featured-image"
  src: "banner.jpeg"
- name: "featured-image-preview"
  src: "banner.jpeg"
keywords:
- DevStream
- OpenSource
---

{{< admonition >}}

来都来了，不看完好意思走？

{{< /admonition >}}

## TL;DR (本文精华)

哥们（姐们）你（们）好！既然你开始寻找“如何参与 DevStream 社区”，那么我假定你已经知道 DevStream 项目的 org 地址和主库地址了。

**“假如不知道呢？”** 假如你这样问我。

**“好，我会再说一次”** 我会这样告诉你。

- Org：<https://github.com/devstream-io>
- Repo：<https://github.com/devstream-io/devstream>

不管是 org 主页还是 repo 里的 README，你都可以找到我们的 **Slack** 频道或者**微信群二维码**，请进入组织，然后找到 [“Daniel Hu”](https://www.danielhu.cn)，群里艾特我，发送一条消息：“我想参与社区”。结束。

**“结束了？”** 可能你又会问。

**“结束了。”** 我想告诉你。

对，就是这么简单，你带着一颗心来，剩下的就都不是事了，我会手把手教你。（手把手，不是物理上的接触哈，尤其是男同胞们请注意。）

## 多说点？

好好好，我知道你意犹未尽，那就再听我“胡扯”一些吧！

先声明一点，我不是权威，我也没有啥系统的理论知识，单纯基于一些不成熟的经验，表达一些不成熟的想法，仅代表我个人。

### 第一步：了解项目

你开始准备参与 DevStream 了，那么第一步你肯定应该先尝试通过公开的资料了解 DevStream。有哪些资料呢？

1. [README](https://github.com/devstream-io/devstream#devstream-what-is-it-anyway)（必读；如果你需要中文版，好吧，确实有，但是我建议你读英文版。）
2. [Contribute 文档](https://docs.devstream.io/en/latest/contribute/)（必读；开始 Contribute 之前读一下 Contribute 文档不过分吧？；注意文末的 [development](https://docs.devstream.io/en/latest/development/development-workflow/)链接哦！）
3. [其他文档](https://docs.devstream.io/en/latest/)（可选；你可以选择感兴趣的内容浏览下。）
4. [博客](https://blog.devstream.io)（可选；博客站点会轻松一些，假如你感兴趣并且有时间，欢迎浏览下我们平时发的博文。）

### 第二步：寻找贡献点

1. 最简单的方式当然是从我们的 [good first issue](https://github.com/devstream-io/devstream/issues?q=is%3Aissue+is%3Aopen+label%3A%22good+first+issue%22) 开始。当然，很抱歉，我能猜到很大概率你点开这个链接后看不到 issues 或者看不到适合自己的 issues，因为太抢手了，我们来不及放足够的 good first issues 上去。

2. 文档。没错，文档！文档无论何时都可以继续完善，文档不可能做到完美！（我们的文档在主库的 docs 目录下，所以你的文档类型的贡献也会被合入主库，文档的贡献同样重要且能够看被看见，被认可！）

3. 单元测试/e2e测试等。测试覆盖率永远不嫌高，如果打开我们的代码库，你一定可以找到需要完善测试用例的地方，大胆去加测试用例吧，这是我们非常需要，不，非常急需的一块内容！没错，我们非常欢迎你来帮助 DevStream 完善 UT/E2E，让 DevStream 更健壮！

4. 代码里的 TODO。如果你开始刷源码（或者直接搜索 todo），你一定可以看到我们留了很多的 TODO 在里面。选一个你觉得有必要实现的而且你 hold 得住的，提个 [issue](https://github.com/devstream-io/devstream/issues/new/choose) 告诉我们，然后我们会把这个任务分配给你！

5. bugfix/enhancement/... 恭喜你，如果从这一步开始，说明你xxxx（一堆褒义词）！打开 [issues](https://github.com/devstream-io/devstream/issues) 列表，你总能找到贡献点，然后勇敢地留下你的评论，接着一切都会顺理成章！

### 第三步：提交你的贡献

如果你认真看了前面我列的“必读”材料，你肯定已经知道[开发工作流](https://docs.devstream.io/en/latest/development/development-workflow/)了。

不需要我赘述些什么，到这里你应该能顺利开始一个 pr，接下来的事情不会太复杂。（假如你遇到了任何困难，别害羞，给我发个微信消息、邮件、GitHub 上直接艾特、…… 任何方式都行）

### 第四步：标题不重要，看内容

你都看到这一步了，反思一下，你进群了吗？加我微信了吗？README 和 Contribute 文档看了吗？issues 列表看了吗？少年，万事开头难，勇敢迈出第一步吧！回到前文，一步一步走！

**“我都看了呀，你个xx”** 可能你会这样说。

**“对不起对不起，大哥（姐），小弟口无遮拦，多有得罪，还望海涵！如果有啥不满意的地方，请提个 issue 告诉我！”** 我小声地告诉你。

---

**就说这么多，开始你的第一个 pr 吧，享受开源的乐趣！**
