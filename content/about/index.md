---
title: "About DevStream Blog | 关于 DevStream 博客"
date: 2022-05-06
---

{{< admonition >}}
博客站点迁移中...
{{< /admonition >}}

## 传送门 - English Version // todo @IronCore


## 传送门

- 官网: <https://devstream.io>
- GitHub: <https://github.com/devstream-io/devstream>
- 文档: <https://docs.devstream.io>
- Slack: [点我加入](https://join.slack.com/t/devstream-io/shared_invite/zt-16tb0iwzr-krcFGYRN7~Vv1suGZjdv4w>)
- 微信群: // todo

## DevStream 是什么？- English Version // todo @IronCore


## DevStream 是什么？

简单来说，DevStream 是一个开源 DevOps 工具链管理工具。

DevStream 要解决的问题是将涵盖 DevOps 全生命周期的主流开源工具管理起来，包括这些工具的安装部署、最佳实践配置、工具间的打通等等。

我们可以通过一个例子更直观地理解 DevStream 的能力：假如你现在成立一家公司，或者具体一点，你要组建一个研发团队，在开始写代码前你需要做哪些事情？有些事情是绕不开的，比如：
- 你需要选择一个地方来存放代码，也许是 GitHub，也许是 GitLab；
- 你需要一个工具来完成项目管理或者说需求管理、Issue 管理等等工作，也许你会选择 Jira 或者禅道或者 Trello；
- 你需要选择一种开发语言，选择一个开发框架，比如你决定用 Golang 来开发，假如这是一个 web 项目，你需要考虑 web 框架用什么？“第一行”代码怎么写，也就是第一个脚手架怎么组装；
- 然后你需要配置一些 ci 自动化，比如 GitHub 上添加 actions 来完成代码的扫描、测试等等；
- 当然 cd 工具也不能少，不管你选择 Jenkins 还是 ArgoCD；
- 如果 cd 完成了，接下来可能你马上要开始纠结日志、监控、告警等等方案应该怎么定了
- 如果想得更多，或许你希望 GitHub 上别人给你提的 issue 能够自动同步到你的 Jira 或者 Trello……
- ……

可见在一个软件的开发生命周期中，除了业务代码编码本身，在 DevOps 工具链上我们将花费大量精力去选型、打通、落地、维护……

而 DevStream 要做的事情，就是屏蔽 DevOps 工具链落地的复杂度，实现“一键部署开源 DevOps 工具链”，甚至是实现“DevOps toolchain as code”。
