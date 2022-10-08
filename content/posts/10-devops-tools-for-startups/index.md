---
title: "10 Best DevOps Tools for Start-ups"
author: "Tiexin Guo | 郭铁心"
authorLink: https://www.guotiexin.com
tags: ["devops", "start-up", "tools", "english-articles"]
categories: ["DevOps", "Tools", "English Articles"]
date: 2022-10-08

resources:
- name: "featured-image"
  src: "tools.jpg"
- name: "featured-image-preview"
  src: "tools.jpg"
keywords:
- DevOps
- start-up
---

> _Author's note: the opinions and thoughts expressed in this blog post (including but not limited to the choice of tools, reviews/comments on tools, comparisons, etc.) reflect only the author's views, which are my own and don't represent either DevStream or my employer's opinion._

OK. You are in a small start-up, and you want to move fast. To move fast, you will need automation instead of doing stuff manually. So, it would be best if you had a bunch of DevOps tools that can accelerate your software development lifecycle (SDLC.) 

The thing is, though, hundreds of tools can help you increase engineering productivity.

Which ones to choose? That's the million-dollar question. 

But worry no more; I've got you covered. Fasten your seatbelt and read on because this blog is about maximizing your SDLC speed.

---

## 1 Terraform

Before writing the first line of code, you probably need to decide on the infrastructure to run your workload. So, let's start with infra.

You've got two options:

- on-premise (rent/build your own data center)
- cloud

If you are a small start-up, chances are, you can't afford to build or rent data centers. Also, start-ups, by definition, are fast-moving entities that focus on speed over many things else; hence you probably can't afford the operational overhead (not to mention financial overhead) of managing physical infrastructure.

So, the cloud is your obvious choice.

And this leaves me no option but to start with the elephant in the room: [Terraform](https://www.terraform.io/).

Nowadays, you probably can't talk about the cloud without talking about Terraform. Starting in 2014, it has been used by many people. Start-ups, global corps, you name it, they use it. Terraform defines your infrastructure as code so that it can generate the same result every time you execute it. Combined with the cloud, you can have your infrastructure up and running in no time (and tear them down immediately when you don't need them anymore.)

To know more about infrastructure as code and Terraform, read my blogs:

- [Infrastructure as Code: Introduction, Best Practices, and How to Choose the Right Tool](https://medium.com/4th-coffee/on-devops-8-infrastructure-as-code-introduction-best-practices-and-choosing-the-right-tool-2c8f46d1f34)
- [Infrastructure as Code — Clean Code, Terraform Introduction, and Best Practices](https://medium.com/4th-coffee/on-devops-9-infrastructure-as-code-clean-code-terraform-introduction-and-best-practices-5d266132c70a)
- [Infrastructure as Code — Introduction to CloudFormation, AWS CDK, EKSCTL, and AWS SAM](https://medium.com/4th-coffee/on-devops-10-infrastructure-as-code-in-aws-an-introduction-to-cloudformation-eksctl-cdk-sam-a2b1dabe163)
- [Security in Infrastructure as Code with Terraform — Everything You Need to Know](https://medium.com/4th-coffee/on-devops-22-security-in-infrastructure-as-code-with-terraform-everything-you-need-to-know-174651b0f056)
- [9 Extraordinary Terraform Best Practices That Will Change Your Infra World](https://medium.com/gitguardian/9-extraordinary-terraform-best-practices-that-will-change-your-infra-world-8e000313e88f)

---

## 2 Kubernetes

Now you've got your infrastructure. What next?

Yep, containerized workload.

A widely accepted methodology for constructing cloud-native applications is the [Twelve-Factor Application](https://12factor.net/), which describes principles and practices to build cloud-optimized apps. Systems built upon these principles can deploy and scale rapidly and add features to react quickly to market changes.

Of the 12 factors, special attention is given to portability (across environments, declarative automation) and [disposability](https://12factor.net/disposability). Your workload (service instances) should be disposable, favor fast start-up to increase scalability opportunities, and graceful shutdowns to leave the system in a correct state.

Docker containers (along with an orchestrator) inherently satisfy this requirement. So, [Kubernetes](https://kubernetes.io/) has become the de-facto place to run your cloud-native workload.

Also known as K8s, Kubernetes is an open-source system for automating containerized applications' deployment, scaling, and management.

No matter which cloud you are using, there is a Kubernetes service for you to create a cluster quickly. You can use Terraform for that; Some cloud providers even provide tools to easily create a cluster with a config file at the click of the Enter key. For example, AWS [EKS](https://aws.amazon.com/eks/) has [eksctl](https://eksctl.io/) for this.

When your services grow more and more, you probably need a service mesh to add an extra layer of security, observability, and reliability to Kubernetes. The most famous option is probably [Istio](https://istio.io/), but it's not our recommendation here. Istio is a bit too complicated. As a fast-moving start-up, the recommendation is [Linkerd](https://linkerd.io/).

Linkerd is still a service mesh, but it's ultralight (hence fast) and simple. You can have it up and running with a command within a few minutes, adding minimum operational overhead.

---

## 3 Trello

Now that we've sorted out the cloud infrastructure out, let's move on to project management.

For start-ups, you probably will do [agile software development](https://en.wikipedia.org/wiki/Agile_software_development), and the most famous choices of agile frameworks probably are Scrum and Kanban. Compared to Scrum, Kanban has fewer "rituals" (meetings,) hence less operational overhead.

No matter your choice, you need a synchronized tool to track your work in progress. This is where issue and project tracking tools like Jira come in.

However, Jira is relatively too complicated. You can definitely get more value out of it if you know how to utilize all the quirks and features, but to get quickly started, a free version of [Trello](https://trello.com/en) should suffice.

Alternatively, you can use GitHub Projects in your GitHub repository.

---

## 4 GitHub + GitHub Actions

OK, now we've got a place to manage our project and track issues; let's code.

Where to store your source code? You need a source code management system, and [GitHub](https://github.com/) is a no-brainer.

Plus, you can integrate Trello with GitHub. For example, suppose you are working on an open-source project where your users would raise issues in GitHub issues, but you need to track them in Trello. In that case, you can automatically synchronize those GitHub issues to Trello tickets.

One of the reasons that we picked GitHub as our source code management system is because of [GitHub Actions](https://github.com/features/actions). 

GitHub Actions makes it easy to automate all your software workflows. Build, test, and even deploy your code right from your source code management system. Although it can do continuous deployment (CD), we only intend to use it as our continuous integration (CI) system here.

One benefit of using GitHub Actions as your CI is, by definition, CI interacts with your code a lot. And if your code and your CI are the same systems, you will save yourself a lot of trouble of integrating your code repositories into your CI systems. No more overhead configuring authentication, authorization, webhooks, or what have you.

If, for example, you decide to deploy something like Jenkins or Tekton and use it as your choice of CI, they would take up some resources of your infrastructure. On the contrary, GitHub Actions has some [free quota](https://docs.github.com/en/billing/managing-billing-for-github-actions/about-billing-for-github-actions), so when you just started your company, probably it's more than enough, and it costs you nothing at all, no need to register some extra self-hosted runners to it at all.

For an introduction to CI and how to choose the right CI for you, see my blog post [An Introduction to CI](https://medium.com/devops-dudes/an-introduction-to-ci-with-comparison-of-17-major-ci-tools-in-2020-and-how-to-choose-the-best-ci-b0cc4ec4f95). It's an old article, but the principles apply.

---

## 5 Argo CD

Once you have your code and the continuous integration workflow set up, you need to deploy your apps, hopefully in an automated fashion. That is what continuous deployment is for: the software is delivered through automated deployments.

The GitOps pattern is a continuous deployment practice using Git repositories as the source of truth for defining the desired application state. Application definitions, configurations, and environments should be declarative and version controlled; application deployment and lifecycle management should be automated, auditable, and easy to understand.

[Argo CD](https://argo-cd.readthedocs.io/en/stable/) is a declarative, continuous delivery tool for GitOps in Kubernetes. It automates the deployment of the desired application states in the specified target environments.

For more information on continuous deployment and GitOps, see my blog posts:

- [An Introduction to Continuous Delivery and Continuous Deployment](https://medium.com/4th-coffee/on-devops-18-an-introduction-to-continuous-delivery-and-continuous-deployment-ae1ff208b3be)
- [Pushing Continuous Deployment to the Next Level — GitOps, Best Practices, and Popular Tools](https://medium.com/4th-coffee/on-devops-19-continuous-deployment-to-the-next-level-gitops-best-practice-and-popular-tools-1a2c885b974)

If you prefer other tools for GitOps, there is also Flux CD.

---

## 6 Doppler

Every stage of the software development lifecycle is intertwined with secrets and credentials.

When you launch a virtual machine, you might need to provide an initial password.

When you are developing an app, the app itself might need access to a database, so it needs the password. Or maybe the app talks to another API that requires authentication, so you need a token, which is a secret.

In your CI system, you might want to write back something to your version control system (for example, build status, tags, etc.), so you have to create a user for your CI and save your password somewhere safe so that your CI system can read it.

In your CD system, you might want to SSH into a machine using a private key to do deployment; or maybe you don't use virtual machines, but instead, you have containers running in a Kubernetes cluster, in which case, you still need to manage the access from your CD system to your Kubernetes cluster.

Secrets are involved in every stage of the software development cycle, which is why you need to manage them properly.

[Doppler](https://www.doppler.com/) enables developers and security teams to keep their secrets and app configuration in sync and secure across devices, environments, and team members. Say goodbye to `.env` files.

Other choices include [HashiCorp Vault](https://www.vaultproject.io/), [AWS Secrets Manager](https://aws.amazon.com/secrets-manager/), etc., but the features differ. The lesser-known Doppler (compared to HashiCorp Vault) is recommended here because of its great Kubernetes integration, where secrets can be synchronized to Kubernetes as native secrets, and the update of a secret in Doppler can trigger a redeployment of your K8s app. For more information on Doppler, see my blog post [Doppler: A Brief Introduction to Secrets Managers](https://medium.com/4th-coffee/doppler-a-brief-introduction-to-secrets-managers-e779b48fac1b).

If you only need to share secrets within a team, there are other choices, like [SOPS](https://github.com/mozilla/sops). For more information on SOPS, see my blog post [A Comprehensive Guide to SOPS: Managing Your Secrets Like A Visionary, Not a Functionary](https://medium.com/4th-coffee/a-comprehensive-guide-to-sops-managing-your-secrets-like-a-visionary-not-a-functionary-9ceefee917d).

For more information on secrets managers, see my blog posts:

- [Secret Management — an Introduction to Secret Manager (HashiCorp Vault, AWS Secrets Manager), and Best Practice](https://medium.com/4th-coffee/on-devops-11-secret-management-an-introduction-to-secret-manager-and-best-practice-da74c6f03aa7)
- [Secret Management — Kubernetes Secrets Management Challenges and Integration with Secret Managers](https://medium.com/4th-coffee/on-devops-12-secret-management-kubernetes-secrets-integration-with-secret-mangers-a630e17f7f66)

---

## 7 Trivy

Since we heavily rely on container images for our cloud-native workload, image security has become a more and more important topic.

Container images play a crucial role in container security. Any container created from an image inherits all its characteristics—including security vulnerabilities, misconfigurations, or even malware.

[Trivy](https://github.com/aquasecurity/trivy) is a security scanner. It is reliable, fast, effortless, and works wherever you need it. Trivy has different scanners that look for various security issues, and the most famous use case is for container image Known vulnerabilities (CVEs) scanning.

You can run it as a CLI tool locally to scan your local container image and other artifacts before pushing it to a container registry or deploying your application.

What's more, What's more, Trivy is designed to be used in CI and can be easily integrated with your CI pipelines.

---

## 8 Prometheus + Grafana + Loki

[Prometheus](https://prometheus.io/) is probably the most famous open-source systems monitoring and alerting toolkit. Since its inception in 2012, many companies and organizations have adopted Prometheus, and the project has a very active developer and user community.

Prometheus collects and stores its metrics as time series data, i.e., metrics information is stored with the timestamp at which it was recorded.

[Grafana](https://grafana.com/), on the other hand, enables you to query, visualize, alert on, and explore your metrics, logs and traces wherever they are stored.

The Prometheus/Grafana combo probably is the most famous centralized monitoring tool. The Prometheus Operator, which manages Prometheus clusters atop Kubernetes, is recommended to install this combo easily. See the open-source project [kube-prometheus](https://github.com/prometheus-operator/kube-prometheus) for more info.

After we get our centralized monitoring system, we need centralized logging as well. The [ELK stack](https://www.elastic.co/what-is/elk-stack) is a popular choice for that, but since we've already got Grafana, here [Loki](https://grafana.com/oss/loki/) is recommended. 

Loki is a log aggregation system designed to store and query logs from all your applications and infrastructure, and it can show logs inside Grafana. You can use Grafana to see both logs and monitoring metrics!

Loki is a horizontally scalable, highly available, multi-tenant log aggregation system inspired by Prometheus. It is designed to be very cost-effective and easy to operate. It does not index the contents of the logs but rather a set of labels for each log stream. Since we've already got Grafana, Loki seems to be a more logical choice compared to installing another tech stack, which is the ELK.

---

## 9 Jaeger

We've already got centralized logging and monitoring. Why [Jaeger](https://www.jaegertracing.io/)?

For modern cloud-native, distributed microservice architecture workload, most operational problems are ultimately grounded in networking and observability.

Compared to the old monolithic application, it is an order of magnitude larger problem to network and debug a set of intertwined distributed services.

So, logging and monitoring alone won't be enough; we need to be able to monitor distributed transactions, optimize performance and latencies, and analyze root causes.

And this is precisely where an end-to-end distributed tracing tool like [Jaeger](https://github.com/jaegertracing/jaeger) kicks in: it helps monitor and troubleshoot transactions in complex distributed systems.

---

## 10 Opsgenie

OK, now we have everything to run our workload and to make them observable, the last but not least is [incident response lifecycle](https://www.atlassian.com/incident-management/incident-response/lifecycle) and incident management.

Incident management tools try to solve one issue: acting as a single source of truth for alerts, they centralize alerts, and only the right people would be notified at the right time.

You can define your own on-call schedules and routing rules to fit any workflow, and you will never miss a critical alert.

Popular choices include [Opsgenie](https://www.atlassian.com/software/opsgenie) and [PagerDuty](https://www.pagerduty.com/), and here Opsgenie is recommended. It's only a personal preference; Opsgenie seems prettier and more user-friendly, and it has all the integrations you need.

---

## Summary

OK, here we have them: 10 easy-to-use, open-source (mostly) DevOps tools to boost start-ups' SDLC:

1. Infrastructure as Code: Terraform
2. Infrastructure: Cloud/Kubernetes/Linkerd
3. Project/Issue Tracking: Trello
4. Source Code Management + CI: GitHub + GitHub Actions
5. CD/GitOps: Argo CD
6. Secrets Manager: Doppler
7. Container Image Security: Trivy
8. Centralized Logging/Monitoring: Prometheus + Grafana + Loki
9. Centralized Tracing/Observability: Jaeger
10. Incident Management: Opsgenie

---

If you like this article, please like, comment, and subscribe.

Soon, I'll write about open-source DevOps tools and newly emerged tools.

Stay tuned!
