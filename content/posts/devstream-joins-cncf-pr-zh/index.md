---
title: "DevStream 进入 CNCF 沙箱，探索云原生时代的高效 DevOps 实践"
author: "Weisi Deng | 邓惟思"
authorLink: https://github.com/Ushuaiaff
tags: ["devstream", "open-source", "cncf", "中文文章"]
categories: ["Open Source", "CNCF", "开源", "中文文章"]
date: 2022-06-17

resources:
- name: "featured-image"
  src: "love.jpg"
- name: "featured-image-preview"
  src: "love.jpg"

keywords:
- DevOps
- DevStream
- CNCF
- open-source
- sandbox
- 开源
---

2022 年 6 月 15 日，云原生计算基金会 (CNCF) 宣布 [DevStream](https://www.devstream.io/) 正式成为 [CNCF 沙箱（Sandbox）项目](https://www.cncf.io/sandbox-projects/)。

DevStream 是一个开源的 DevOps 工具链管理器，可以通过一个简单的配置文件，将软件研发生命周期中各环节的 DevOps 工具统一管理起来，完成各工具的快速安装部署、工具间整合、最佳实践配置等工作。

许多研发团队可能会在 DevOps 工具链管理中遇到挑战，例如：
-  不知道如何选择 DevOps 工具
-  没有足够的人力、时间去调研大量 DevOps 工具
-  在 DevOps 工具链的整合和维护上力不从心

DevStream 主要解决开源 DevOps 工具链落地难、维护难的痛点，一方面让开发者少在 DevOps 工具上踩坑，投入更多的精力在更重要的业务逻辑上；另一方面让研发团队不再受限于维护和替换成本，能够更自由地选择最适宜的工具组合，使效能最大化。

## 主要特性
为了支持 DevOps 工具链的灵活高效管理，DevStream 具备以下特性：
-   **配置代码化**：统一管理 DevOps 各环节工具，工具链变更历史可回溯
-   **Core-Plugin 架构**：[内核](https://github.com/devstream-io/devstream/tree/main/internal/pkg/pluginengine) 与 [插件](https://github.com/devstream-io/devstream/tree/main/internal/pkg/plugin) 解耦，使 DevOps 工具链像乐高一样灵活可定制
-   **易于使用**：最佳实践沉淀为工具配置，方便用户开箱即用，例如 [ GitOps 工具链的快速搭建](https://docs.devstream.io/en/latest/best-practices/gitops/)
![DevStream 架构](https://i.imgur.com/VAPQpug.png)

自 2022 年 2 月上线 v0.1.0 并开源以来，DevStream 高速迭代。在本次进入沙箱之前，DevStream 已于 5 月中旬加入 [CNCF 云原生全景图](https://landscape.cncf.io/)的自动化和部署工具类别。

目前，DevStream 更新至 [v0.6.1](https://github.com/devstream-io/devstream/releases)，并新增以下关键功能：
- 更丰富的插件支持，已支持 JIRA/Trello 管理项目与事务并打通 GitHub/GitLab、Golang 脚手架生成、Jenkins/GitHub Actions/GitLab CI 管理 CI 流程等 [一系列工具插件](https://docs.devstream.io/en/latest/plugins/plugins-list/)，且还在 [持续新增中](https://blog.devstream.io/posts/%E7%BB%99dtm%E5%BC%80%E5%8F%91%E4%B8%80%E4%B8%AA%E6%8F%92%E4%BB%B6/)。
- 更完善的 [命令集](https://docs.devstream.io/en/latest/commands/autocomplete/)
- 更成熟的插件管理逻辑，自动感知并评估工具的状态变更，可作为 single source of truth 一站式管理各工具插件
- 更强大的配置管理逻辑，支持插件之间的依赖管理与配置引用等

{{< figure src="landscape.png" title="DevStream 进入 CNCF landscape" >}}

## DevStream 社区和开发者

几个月来，DevStream 产品变得强大、丰富，离不开它背后茁壮成长的社区：
- 发布 [28 篇中英文技术/社区博客](https://blog.devstream.io/)
- 吸引 20 位社区开发者、378 Github Star 和 88 Fork
- 举办 [ 4 场社区例会](https://space.bilibili.com/1737999178)，400+ 用户在社群中交流学习

进入 CNCF 沙箱后，DevStream 社区将组织多种多样的活动，持续打造开放友好的交流环境。期待更多社区成员参与进来，和我们一起定义 DevStream 的未来。

此外，DevStream 也期待与 CNCF 生态中众多 DevOps 相关项目密切合作，共建云原生时代的 DevOps 最佳实践。

{{< figure src="pr-stat.png" title="过去 6 个月社区 PR 统计，cr. Apache DevLake (incubating)" >}}

## 未来规划
DevStream 的愿景是成为 DevOps 工具链运维的一站式工具。就像 apk、apt、yum 等包管理工具能够为任何新环境轻松设置你最喜欢的软件包一样，DevStream 希望成为 DevOps 工具的软件包管理器。当开发者需要替换工具链上的某一个组件，用几行代码就可以轻松搞定。

在此基础上，用户能够根据不同场景下的 DevOps 工具链需求，创建不同发行版，使行业优秀实践能够被快速学习、复用。

## 如何参与 DevStream 社区？
欢迎所有人参与社区建设，让 DevStream 越来越有生命力！
- DevStream 代码仓库：https://github.com/devstream-io/devstream
- DevStream 官网：https://www.devstream.io/
- DevStream 文档：https://docs.devstream.io
- 如何参与贡献：https://docs.devstream.io/en/latest/contributing_guide/
- DevStream 社群：加入 [Slack](https://join.slack.com/t/devstream-io/shared_invite/zt-16tb0iwzr-krcFGYRN7~Vv1suGZjdv4w) 或扫描微信二维码
![DevStream 微信社群二维码](./devstream-wechat.png)

> CNCF (Cloud Native Computing Foundation) 成立于 2015 年 12 月，是 Linux Foundation 旗下的非盈利组织，致力于培育和维护一个厂商中立的开源生态系统，来推广云原生技术。
