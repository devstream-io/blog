---
title: "Using AWS S3 to Store DevStream State"
author: "Tiexin Guo | 郭铁心"
authorLink: https://www.guotiexin.com
tags: ["how-to", "devstream", "english-articles"]
categories: ["DevStream How-To", "English Articles"]
date: 2022-05-30

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

In our [latest release v0.6.0](../v060-release/), using AWS S3 to store DevStream state is supported.

In this blog, we are going to demonstrate the usage of AWS S3 to store DevStream's state files.

## Terminology

_State: if you don't already know about DevStream state, please read [this doc](https://docs.devstream.io/en/latest/core-concepts/core-concepts/) and [this blog](../creating-a-dtm-plugin-for-anything/) helps, too._

Backend: where to actually store the state file. It can be either `local` or `s3` at the moment.

## Configuration

In the main config file, we have a section to configure the state. Currently, local and S3 are supported.

Local example:

```yaml
varFile: variables-gitops.yaml

toolFile: tools-gitops.yaml

state:
  backend: local
  options:
    stateFile: devstream.state
```

S3 example:

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

More on configuring state [here](https://docs.devstream.io/en/latest/core-concepts/stateconfig/).

In short, we can use the "backend" keyword to specify where to store the state: either locally or in an S3 bucket. If S3 is used, we need to specify the bucket, region, and the S3 key as well.

## Config File Examples

In this demo, we use the following configs:

`config.yaml`:

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

`variables-gitops.yaml`:

```yaml
githubUsername: IronCore864
repoName: dtm-test-go
defaultBranch: main

dockerhubUsername: ironcore864

argocdNameSpace: argocd
argocdDeployTimeout: 5m
```

`tools-gitops.yaml`:

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

## Getting Started

Before reading on, now is a good time to check if you have configured your AWS related environment variables correctly or not.

For macOS/Linux users, do:

```shell
export AWS_ACCESS_KEY_ID=ID_HERE
export AWS_SECRET_ACCESS_KEY=SECRET_HERE
export AWS_DEFAULT_REGION=REGION_HERE
```

For more information, see the [official document here](https://docs.aws.amazon.com/cli/latest/userguide/cli-chap-configure.html).

## Apply

Then let's run `dtm apply`:

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

As we can see from the output, the S3 backend is used, and it also shows the bucket and key you are using, and in which region this bucket lives.

## Checking the State File

After `apply`, let's download the state file from S3 and check it out:

```shell
tiexin@mbp ~/work/devstream-io/devstream $ aws s3 cp s3://devstream-test-remote-state/devstream.state .
```

And if we open the downloaded file, we will see something similar to the following:

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

which is exactly the same as if we were using the local backend to store state.

--- 

Like this quick tutorial? Then I suggest to read more of our latest DevOps blogs:
- [9 Extraordinary Terraform Best Practices That Will Change Your Infra World](../9-terraform-best-practices/)
- [Dagger (the CI/CD Tool, not the Knife) In-Depth - Everything You Need to Know (as of Apr 2022)](../dagger-in-depth/)
- [A Brief History of the DMCA](../dmca-takedowns/)
