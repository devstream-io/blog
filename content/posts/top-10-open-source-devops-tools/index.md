---
title: "Top 10 Open-Source DevOps Tools That You Should Know"
author: "胡涛 | Daniel Hu"
authorLink: https://www.danielhu.cn
tags: ["devops", "open-source", "tools", "english-articles"]
categories: ["DevOps", "Open Source", "English Articles"]
date: 2022-10-24

resources:
- name: "featured-image"
  src: "banner.jpeg"
keywords:
- DevOps
- Open-Source
- Tools
---

Do you know how many open-source tools are in DevOps?

Have you ever seen the picture below?

Have you ever felt lost in the DevOps world with seemingly more than 1000 different tools?

If you are struggling with these questions now, then today you are in luck because I’m here to help you figure out the most popular DevOps tools.

{{< figure src="devops-periodic-table.png" title="DevOps Periodic Table" >}}

## Wait, what’s the DevOps?

"What's the DevOps?"

This's a controversial topic, and I'm not gonna go into it in this post. I'd like to give you an equation here only:

**DevOps = Culture + Toolchain + Practice**

And today I just want to introduce you to some popular tools.

If you are a Chinese reader, I can give you an article that I wrote some months ago which is named:

- [什么是 DevOps？看这一篇就够了！](https://www.danielhu.cn/what-is-devops/).

## Top 10 Open-Source DevOps Tools Rundown

### 1、GitLab CE

- [GitLab Source Code Repository](https://gitlab.com/gitlab-org/gitlab)

There are 2 major editions of GitLab:

- GitLab Community Edition (CE) is available freely under the MIT Expat license.
- GitLab Enterprise Edition (EE) includes extra features that are more useful for organizations with more than 100 users.

Before I introduce you to GitLab, I'd like to talk a little bit about Git.
Git is a free and open-source distributed version control system designed to handle everything from small to very large projects with speed and efficiency.
Version control provides developers with a means by which they can keep track of all the changes and updates in their codes.
With Git, you must have thought that we need a "Git server". As an alternative to GitHub, if you want to deploy the "Git server" privately, GitLab CE might be a good choice.

GitLab CE is an open-source end-to-end software development platform with built-in version control, issue tracking, code review, CI/CD, etc.
The main features of GitLab CE are:

- Manage Git repositories with fine-grained access controls that keep your code secure;
- Perform code reviews and enhance collaboration with merge requests;
- Complete continuous integration (CI) and continuous deployment/delivery (CD) pipelines to build, test, and deploy your applications;
- Each project can also have an issue tracker, issue board, and a wiki.

GitLab has been used by more than 100,000 organizations, and it's the most popular solution to manage Git repositories on-premises.

### 2、Jenkins

- [Jenkins Source Code Repository](https://github.com/jenkinsci/jenkins)

Jenkins is one of the best open-source continuous integration and automation server due to the over 1,800 plugins to support creating, delivering, and automating any project.

Using Jenkins to automate your development workflow will help you to focus on the work that matter most.
Jenkins is commonly used for:

- Building projects
- Running tests
- Static code analysis
- Deployment

You can execute repetitive tasks, save time, and optimize your development process with Jenkins.

### 3、Argo CD

- [Argo CD Source Code Repository](https://github.com/argoproj/argo-cd)

Argo CD is a declarative, continuous delivery tool for Kubernetes, which uses the principle of GitOps.
GitOps is a principle of using Git as the "single source of truth" for application deployment information.
It ensures that the state of a deployment is the same as the state which is defined in the Git repository.

Why should you use Argo CD？

- Application definitions, configurations, and environments should be declarative and version controlled.
- Application deployment and lifecycle management should be automated, auditable, and easy to understand.

### 4、Tekton

- [Tekton Source Code Repository](https://github.com/tektoncd/pipeline)

Tekton is a powerful yet flexible Kubernetes-native open-source framework for creating CI/CD systems.
It's open-source and part of the CD Foundation, a Linux Foundation project.

The main project with Tekton is Tekton Pipelines. It provides k8s-style resources for declaring CI/CD-style pipelines.
The Tekton Pipelines are cloud-native and decoupled:

- Run on Kubernetes;
- Have Kubernetes clusters as a first-class type;
- Use containers as building blocks;
- One Pipeline can be used to deploy to any k8s cluster;
- The Tasks which make up a Pipeline can easily be run in isolation;
- Resources such as git repositories can easily be swapped between runs.

### 5、Ansible

- [Ansible Source Code Repository](https://github.com/ansible/ansible)

Ansible is a radically simple IT automation system.
It handles configuration management, application deployment, cloud provisioning, ad-hoc task execution, network automation, and multi-node orchestration.
Ansible makes complex changes like zero-downtime rolling updates with load balancers easy.

While Ansible leverages IaC, it uses SSH for its push nodes, so it's agentless.
Ansible is very easy to use for its Playbooks are written in YAML that is readable by humans.

The Design Principles of Ansible are:

- Have an extremely simple setup process with a minimal learning curve;
- Manage machines quickly and in parallel;
- Avoid custom agents and additional open ports, be agentless by leveraging the existing SSH daemon;
- Describe infrastructure in a language that is both machine and human-friendly;
- Focus on security and easy auditability/review/rewriting of content;
- Manage new remote machines instantly, without bootstrapping any software;
- Allow module development in any dynamic language, not just Python;
- Be usable as non-root;
- Be the easiest IT automation system to use, ever.

In a nutshell, Ansible is one of the most simple yet effective IT orchestration and configuration management tools.

### 6、Helm

- [Helm Source Code Repository](https://github.com/helm/helm)

Maybe you are already using Docker and Kubernetes, which are the basis of containerization.
After that, you will need a mechanism to manage the Kubernetes resources. Yes, helm can help you to achieve this.

Helm is a tool that streamlines installing and managing Kubernetes applications. Think of it like apt/yum/homebrew for Kubernetes.

- Helm renders your templates and communicates with the Kubernetes API.
- Helm runs on your laptop, CI/CD, or wherever you want it to run.
- Charts are Helm packages that contain at least two things:
  - A description of the package (Chart.yaml)
  - One or more templates, which contain Kubernetes manifest files
- Charts can be stored on disk, or fetched from remote chart repositories (like Debian or RedHat packages)

You can use Helm to:

- Find and use popular software packaged as Helm Charts to run in Kubernetes;
- Share your own applications as Helm Charts;
- Create reproducible builds of your Kubernetes applications;
- Intelligently manage your Kubernetes manifest files;
- Manage releases of Helm packages.

In addition, Helm is a CNCF graduated project.

### 7、Harbor

- [Harbor Source Code Repository](https://github.com/goharbor/harbor)

Harbor is an open-source trusted cloud native registry project that stores, signs, and scans content.
Harbor extends the open-source Docker Distribution by adding the functionalities usually required by users such as security, identity and management.
Having a registry closer to the build-and-run environment can improve image transfer efficiency.
Harbor supports the replication of images between registries and also offers advanced security features such as user management, access control and activity auditing.

The 8 main features of Harbor:

- **Cloud native registry**: With support for both container images and [Helm](https://helm.sh) charts, Harbor serves as a registry for cloud-native environments like container runtimes and orchestration platforms.
- **Role-based access control**: Users access different repositories through 'projects' and a user can have different permission for images or Helm charts under a project.
- **Policy-based replication**: Images and charts can be replicated (synchronized) between multiple registry instances based on policies with using filters (repository, tag and label).
  Harbor automatically retries a replication if it encounters any errors. This can be used to assist load-balancing, achieve high availability, and facilitate multi-datacenter deployments in hybrid and multi-cloud scenarios.
- **Vulnerability Scanning**: Harbor scans images regularly for vulnerabilities and has policy checks to prevent vulnerable images from being deployed.
- **LDAP/AD support**: Harbor integrates with existing enterprise LDAP/AD for user authentication and management, and supports importing LDAP groups into Harbor that can then be given permissions to specific projects.
- **Image deletion & garbage collection**: System admin can run garbage collection jobs so that images(dangling manifests and unreferenced blobs) can be deleted and their space can be freed up periodically.
- **Graphical user portal**: User can easily browse, search repositories and manage projects.
- **Easy deployment**: Harbor can be deployed via Docker compose as well Helm Chart, and a Harbor Operator was added recently as well.

Helm is a CNCF graduated project too.

### 8、Sonarqube

- [Sonarqube Source Code Repository](https://github.com/SonarSource/sonarqube)

SonarQube empowers developers to write cleaner and safer code.
It can catch tricky bugs to prevent undefined behavior from impacting end-users and make sure your codebase is clean and maintainable, to increase developer velocity.

You can integrate Sonarqube scanning into the Jenkins/Tekton Pipeline to catch code issues as they occur in the CI process.

### 9、Prometheus

- [Prometheus Source Code Repository](https://github.com/prometheus/prometheus)

Prometheus is the second graduated project of CNCF. It is a systems and service monitoring system.
It collects metrics from configured targets at given intervals, evaluates rule expressions, displays the results,
and can trigger alerts when specified conditions are observed.

The features that distinguish Prometheus from other metrics and monitoring systems are:

- A multi-dimensional data model (time series defined by metric name and set of key/value dimensions);
- PromQL, a powerful and flexible query language to leverage this dimensionality;
- No dependency on distributed storage; single server nodes are autonomous;
- An HTTP pull model for time series collection;
- Pushing time series is supported via an intermediary gateway for batch jobs;
- Targets are discovered via service discovery or static configuration;
- Multiple modes of graphing and dashboarding support;
- Support for hierarchical and horizontal federation.

### 10、DevStream

- [DevStream Source Code Repository](https://github.com/devstream-io/devstream)

We've gone through 9 DevOps tools, and maybe at this moment you are thinking that DevOps tools are too much and these make you feel overwhelmed.
Maybe you wish you could have an open-source tool to help you manage those DevOps tools. You choose the best DevOps tools that you love,
then one tool could take care of the rest. Ok, DevStream do is it.

DevStream is a DevOps toolchain manager. It can help you to kick off DevOps quickly and easily.

With DevStream, you can define your desired DevOps tools in a single human-readable YAML config file, and at the press of a button (one single command),
you will have your whole DevOps toolchain and SDLC workflow set up.

## Summary

At the last, I have to tell you some more bit about the DevOps tools I mentioned above. They are the top 10 tools just in my mind.
If you have a different "top 10" idea, that's fine.
I also welcome you to post your "top 10" in the comment section below.
