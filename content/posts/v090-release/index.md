---
title: "DevStream v0.9.0 Release"
author: "Tiexin Guo | 郭铁心"
authorLink: https://www.guotiexin.com
tags: ["releases", "english-articles"]
categories: ["Releases"]
date: 2022-09-16
resources:
- name: "featured-image"
  src: "dtm090.png"
- name: "featured-image-preview"
  src: "dtm090.png"
keywords:
- DevStream
- Release
---

## Official Releases for Different Platforms

- [dtm-darwin-arm64](https://devstream.gateway.scarf.sh/releases/v0.9.0/dtm-darwin-arm64)
- [dtm-darwin-amd64](https://devstream.gateway.scarf.sh/releases/v0.9.0/dtm-darwin-amd64)
- [dtm-linux-amd64](https://devstream.gateway.scarf.sh/releases/v0.9.0/dtm-linux-amd64)

## Major Changes since Last Release

This version focuses on two things:

- refactoring
- making the Jenkins/GitLab/Harbor/Java combination prod-ready

Here we highlighted a few changes in this release:

### Core

* helm-type plugins now support local charts
* there is a new "force" flag to `dtm destroy` command, thanks to @csonezp
* k8s backend is supported for storing state

### Plugins

* GitHub Actions for Python is enhanced
* Jenkins/GitLab/Harbor plugins are greatly enhanced, including documentation, features, default values, etc.
* ArgoCD plugin now treat namespace in an "upsert" manner
* new plugin: harbor
* added many default options for each plugin

### Docs

* Install dtm with asdf in quickstart, thanks to @zhenyuanlau
* GitLab + Jenkins + Harbor On Premise Toolchain best practice (in Mandarin)
* There is a new document (in both English and Chinese) detailing how to setup a local development Environment for Golang/K8s related projects (not specific to DevStream)

## More

See the [full changelog here](https://github.com/devstream-io/devstream/compare/v0.8.0...v0.9.0).
