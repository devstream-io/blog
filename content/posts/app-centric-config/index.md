---
title: "App-Centric Configuration: Adding More Value to Your Life"
author: "Tiexin Guo | 郭铁心"
authorLink: https://www.guotiexin.com
tags: ["how-to", "devstream", "english-articles"]
categories: ["DevStream How-To", "English Articles"]
date: 2022-11-24
resources:
- name: "featured-image"
  src: "wordcloud.png"
- name: "featured-image-preview"
  src: "wordcloud.png"
keywords:
- DevStream
- DevOps
- Engineering Platform
- ALM
- SDLC
---

## DevStream's Story

[About nine months ago, DevStream was first publicly released](https://github.com/devstream-io/devstream/releases/tag/v0.1.0). Since then, it has evolved a lot. If this is the first time you have come across DevStream, maybe read [this blog](https://blog.devstream.io/posts/hello-world/) for a quick overview.

In the current incarnation (v0.9), every single DevOps tool (including integrations of tools and CI/CD pipeline setup for apps) is treated as a "Tool", which is a DevStream concept. So, if you would like to define a DevOps platform that does the following:

- repository scaffolding
- continuous integration w/ GitHub Actions
- install Argo CD as the tool for continuous deployment
- continuous deployment w/ Argo CD

Your DevStream `tools.yaml` config file would look similar to this:

```yaml
tools:
- name: repo-scaffolding
  instanceID: myapp
  options:
    destinationRepo:
      owner: [[ githubUser ]]
      repo: [[ app ]]
      branch: main
      repoType: github
    sourceRepo:
      org: [[ githubUser ]]
      repo: dtm-scaffolding-python
      repoType: github
    vars:
      ImageRepo: [[ dockerUser ]]/[[ app ]]
      AppName: [[ app ]]
- name: githubactions-python
  instanceID: default
  dependsOn: [ repo-scaffolding.myapp ]
  options:
    owner: [[ githubUser ]]
    repo:  [[ app ]]
    language:
      name: python
    branch: main
    docker:
      registry:
        type: dockerhub
        username: [[ dockerUser ]]
        repository: [[ app ]]
- name: helm-installer
  instanceID: argocd
- name: argocdapp
  instanceID: default
  dependsOn: [ "argocd.default", "githubactions-python.default" ]
  options:
    app:
      name: [[ app ]]
      namespace: argocd
    destination:
      server: https://kubernetes.default.svc
      namespace: default
    source:
      valuefile: values.yaml
      path: helm/[[ app ]]
      repoURL: ${{repo-scaffolding.myapp.outputs.repoURL}}
```

A quick explanation - what DevStream does with this config is the following: 

- install Argo CD;
- repository scaffolding for myapp;
- CI/CD for myapp.

It looks nice and works like a charm.

However, suppose you have more than one app to worry about (more often than not, that would be the case in the microservice era). In that case, things start to get a bit complicated: you'd have to repeat the `githubactions-golang` and `argocdapp` sections as many times as the number of apps you manage.

That is to say, to manage ten apps/microservices, your DevStream YAML file may grow to well past 300 lines of config.

Today, we are happy to announce that it's no longer the case. We've simplified it. By a big margin. With the release of v0.10.

Read on.

---

## Adding Value to Your Life

A big config is definitely harder to read and maintain, and we don't like that. That's why we decided to solve this problem in the first place.

However, we waited to get right into it.

Instead, we started to think about our market positioning and value proposition. What is DevStream anyway? What problems does it solve? Why would other engineers want to use it instead of building stuff manually or purchasing one-stop DevOps platforms?

If we could precisely answer these questions, we would know how to improve it to the next level.

The discussion went on and on. The whole team, including our CEO, discussed multiple days. We've put tens of hours of thought and debate into it, until we figured out where exactly DevStream can add value to your life:

- installing DevOps tools (but that's not the point at all)
- integrating DevOps tools and building engineering platforms (now we're talking)
- application lifecycle/software development lifecycle management: 
  - repository bootstrapping
  - continuous integration pipelines, integrating best practices, typical stages, and steps into it
  - continuous deployment pipelines, integrating helm chart/Dockerfile best practices
  - ...

_Note: read more on [Application Lifecycle Management (ALM) here](https://www.redhat.com/en/topics/devops/what-is-application-lifecycle-management-alm) and [Software Development Lifecycle (SDLC) here](https://medium.com/gitguardian/how-adding-security-into-devops-accelerates-the-sdlc-pt-1-459134252ee7)._

---

## Lightbulb: Apps? Apps!

Since many values DevStream could bring are around applications/microservices and their lifecycle management, why not simply build configurations based on that?

We call this new thought "app-centric" configuration, and let's get right into it:

--- 

## Backward Compatibility

First things first, the previous "tools" config still works. We wouldn't want to break that. That is to say, if you copy-paste the first code snippet from the beginning of this article, it's supposed to work without any problem, just like previous versions. 

---

## (Much) Shorter, Simpler, Easier, While Achieving the Same

You'd be surprised that the config below does exactly the same thing as the code snippet at the very beginning of this article.

```yaml
apps:
- name: myapp
  spec:
    language: python
    framework: django
  repo:
    url: github.com/devstream-io/myapp
  repoTemplate:
    url: github.com/devstream-io/dtm-scaffolding-python
  ci:
    - type: githubactions
  cd:
    - type: argocdapp
```

Is this magic? How'd we do that?

First, we created a new abstraction that is called "apps". Each app corresponds to a real-world application or microservice that you manage.

Secondly, we simplified the configurations as much as possible with five small sections:

- spec: language, framework related to the application, which is shared information with other sections
- repo: where to create/bootstrap the repository for the app
- repoTemplate: the template used to bootstrap the app's repo
- ci: setting up continuous integration for this app
- cd: setting up continuous deployment for this app

Last but not least, more "defaults" and "best practices" are integrated, by default, into DevStream, so that you don't have to override most of the configs.

Neat, right? I know.

---

## One File to Rule Them All

Putting It All Together:

```yaml
config:
  state:
  backend: local
  options:
    stateFile: devstream.state

tools:
- name: helm-installer
  instanceID: argocd

apps:
- name: myapp1
  spec:
    language: python
    framework: django
  repo:
    url: github.com/devstream-io/myapp1
  repoTemplate:
    url: github.com/devstream-io/dtm-scaffolding-python
  ci:
  - type: githubactions
  cd:
  - type: argocdapp
- name: myapp2
  spec:
    language: golang
    framework: gin
  repo:
    url: github.com/devstream-io/myapp2
  repoTemplate:
    url: github.com/devstream-io/dtm-scaffolding-golang
  ci:
  - type: githubactions
  cd:
  - type: argocdapp
```

I hope you enjoy this new feature. Have fun experimenting tools together with apps!
