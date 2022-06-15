---
title: "用 AWS S3 来保存 DevStream 的状态"
author: "Daniel Hu | 胡涛"
authorLink: https://www.danielhu.cn
tags: ["how-to", "devstream", "中文文章"]
categories: ["DevStream How-To", "中文文章"]
date: 2022-05-31

resources:
- name: "featured-image"
  src: "s3.jpg"
- name: "featured-image-preview"
  src: "s3.jpg"
keywords:
- DevStream
- GoLang
- s3
---

> 本文翻译自 [Using AWS S3 to Store DevStream State](../s3-remote-state/)

从 [上一个 release 版本 v0.6.0](../v060-release/) 开始, DevStream 支持通过 AWS S3 来保存状态了。

在这篇博文里，我们准备掩饰如何使用 AWS S3 来保存 DevStream 的状态。

## 术语

_状态（State）：如果你还不熟悉 DevStream 的状态定义，请先阅读 [这个文档](https://docs.devstream.io/en/latest/core-concepts/core-concepts/) 和 [这篇博客](../creating-a-dtm-plugin-for-anything/)。_

_后端（Backend）_: 实际保存状态的地方，目前可以是 s3 或者 local（本地文件）。

## 配置

在主配置文件中，我们有一段配置是用来描述状态的，目前支持 local 和 s3 两种：

- `local` 示例：

```yaml
varFile: variables-gitops.yaml

toolFile: tools-gitops.yaml

state:
  backend: local
  options:
    stateFile: devstream.state
```

- `s3` 示例：

```yaml
varFile: variables-gitops.yaml

toolFile: tools-gitops.yaml

state:
  backend: s3
  options:
    bucket: devstream-remote-state
    region: ap-southeast-1
    key: devstream.state
```

和配置 state 相关的更新信息可以查看[这里](https://docs.devstream.io/en/latest/core-concepts/stateconfig/)。

简而言之，我们可以使用 "backend" key 来指定将状态存到哪里：要么本地保存，要么存到 S3 桶里。如果使用 S3，我们就需要指定 bucket，region 和 S3 key。

## 配置文件例子

在这个 demo 里，我们使用下面配置：

- `config.yaml`:

```yaml
varFile: variables-gitops.yaml

toolFile: tools-gitops.yaml

state:
  backend: s3
  options:
    bucket: devstream-test-remote-state
    region: ap-southeast-1
    key: devstream.state
```

- `variables-gitops.yaml`:

```yaml
githubUsername: IronCore864
repoName: dtm-test-go
defaultBranch: main

dockerhubUsername: ironcore864

argocdNameSpace: argocd
argocdDeployTimeout: 5m
```

- `tools-gitops.yaml`:

```yaml
tools:
- name: github-repo-scaffolding-golang
  instanceID: default
  options:
    owner: [[ githubUsername ]]
    org: ""
    repo: [[ repoName ]]
    branch: [[ defaultBranch ]]
    image_repo: [[ dockerhubUsername ]]/[[ repoName ]]
```

## 开始

开始往下操作之前，请检查好你的本地测试环境是否已经配置上了和 AWS 相关的几个环境变量：

对于 macOS/Linux 用户，执行：

```shell
export AWS_ACCESS_KEY_ID=ID_HERE
export AWS_SECRET_ACCESS_KEY=SECRET_HERE
export AWS_DEFAULT_REGION=REGION_HERE
```

更多信息，请查阅 [官方文档](https://docs.aws.amazon.com/cli/latest/userguide/cli-chap-configure.html).

## Apply

接着我们可以执行 `dtm apply` 命令了：

```shell
tiexin@mbp ~/work/devstream-io/devstream $ ./dtm apply
2022-05-30 17:07:59 ℹ [INFO]  Apply started.
2022-05-30 17:07:59 ℹ [INFO]  Using dir <.devstream> to store plugins.
2022-05-30 17:07:59 ℹ [INFO]  Using s3 backend. Bucket: devstream-test-remote-state, region: ap-southeast-1, key: devstream.state.
2022-05-30 17:08:00 ℹ [INFO]  Tool (github-repo-scaffolding-golang/default) found in config but doesn't exist in the state, will be created.
Continue? [y/n]
Enter a value (Default is n): y

2022-05-30 17:08:08 ℹ [INFO]  Start executing the plan.
2022-05-30 17:08:08 ℹ [INFO]  Changes count: 1.
2022-05-30 17:08:08 ℹ [INFO]  -------------------- [  Processing progress: 1/1.  ] --------------------
2022-05-30 17:08:08 ℹ [INFO]  Processing: (github-repo-scaffolding-golang/default) -> Create ...
2022-05-30 17:08:12 ℹ [INFO]  The repo dtm-test-go has been created.
2022-05-30 17:08:29 ✔ [SUCCESS]  Tool (github-repo-scaffolding-golang/default) Create done.
2022-05-30 17:08:29 ℹ [INFO]  -------------------- [  Processing done.  ] --------------------
2022-05-30 17:08:29 ✔ [SUCCESS]  All plugins applied successfully.
2022-05-30 17:08:29 ✔ [SUCCESS]  Apply finished.
```

从输出日志中可以看到，backend 用了 S3，还有对应的 region，bucket 和 key 等信息。

## 检查状态文件

执行完 `apply` 之后，我们来从 S3 下载这个 state 文件，然后检查一下里面的内容：

```shell
tiexin@mbp ~/work/devstream-io/devstream $ aws s3 cp s3://devstream-test-remote-state/devstream.state .
```

接着打开刚才下载到的文件，我们可以看到类似这样的内容：

```yaml
github-repo-scaffolding-golang_default:
  name: github-repo-scaffolding-golang
  instanceid: default
  dependson: []
  options:
    branch: main
    image_repo: ironcore864/dtm-test-go
    org: ""
    owner: IronCore864
    repo: dtm-test-go
  resource:
    org: ""
    outputs:
      org: ""
      owner: IronCore864
      repo: dtm-test-go
      repoURL: https://github.com/IronCore864/dtm-test-go.git
    owner: IronCore864
    repoName: dtm-test-go
```

这个内容和我们使用 local 类型 backend 时保存到本地的文件是一模一样的。

--- 

喜欢这个“快速教程”吗？下面几篇 DevOps 相关博文推荐给你：

- [9 Extraordinary Terraform Best Practices That Will Change Your Infra World](../9-terraform-best-practices/)
- [Dagger (the CI/CD Tool, not the Knife) In-Depth - Everything You Need to Know (as of Apr 2022)](../dagger-in-depth/)
- [A Brief History of the DMCA](../dmca-takedowns/)
