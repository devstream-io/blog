---
title: "State of Kubernetes Secrets Management in 2022"
author: "Tiexin Guo | 郭铁心"
authorLink: https://www.guotiexin.com
tags: ["devops", "kubernetes", "tools", "english-articles"]
categories: ["DevOps", "Kubernetes", "English Articles"]
date: 2022-11-22
canonical_url: https://www.doppler.com/blog/kubernetes-secrets-management-in-2022
resources:
- name: "featured-image"
  src: "secrets.jpg"
- name: "featured-image-preview"
  src: "secrets.jpg"
keywords:
- DevOps
- Kubernetes
- Secrets
---

Note: this blog is originally published [here](https://www.doppler.com/blog/kubernetes-secrets-management-in-2022).

Secrets are already a fundamental building block of the modern Software Development Lifecycle (SDLC). Applications, CI/CD systems, API access, Databases, etc., all require some form of secrets/tokens/credentials in one way or another: secrets are literally involved in every single stage of the SDLC. And, for security reasons, you can't put the secret in your version control system (although there are tools, like [SOPS](https://github.com/mozilla/sops), which make it possible to share secrets using source code management systems). These are precisely the reasons why we should manage secrets properly.

On the other hand, Kubernetes (K8s) has become the de-facto place to run your containerized workload, especially if you want to follow the [The Twelve-Factor App methodology](https://12factor.net/) and be as cloud-native as you can.

In this blog, we will have a deep dive into secrets and K8s. Specifically, we will have an overview of the secrets management in K8s.

Without further adieu, let's get started.

---

## 1 K8s Secrets Usage and Challenges

### 1.1 Recap of K8s Secrets

A quick recap: basically, there are two major ways in which secrets can be consumed by a container in a Pod in a K8s environment:

- mounted as data volumes
- exposed as environment variables

Then, within the Pod, it can either read the content of the secrets by reading a file (in the case of mounting secrets as data volumes), or read it as environment variables (in the case of exposing secrets as env vars).

### 1.2 Read Config/Secrets from Files

The file-reading method might be easy to use and benefit local development because you can prepare a local file containing all the required secrets for local testing and expect the same to run in K8s without extra modification to your app.

However, it's relatively old-school, maybe only older apps were initially written in this way, and it's against the Twelve-Factor App methodology.

An app's config will likely vary between deploys (staging, production, developer environments, etc.). The use of secrets files has its weaknesses and brings challenges, such as:

- Should you check the secrets files in your version control system?
	+ If so, how to encrypt them (since they are secrets)?
	+ If not, where to store the secret files?
- What if secrets files are scattered in different repos?
- Where is the single source of truth?
- What if the secrets files are language/framework-specific, but there is another app in another language/framework that needs access to the same secrets?

### 1.3 The Twelve-Factor App Way: Environment Variables

The Twelve-Factor App's answer to the secret problem is to store config in environment variables.

Env vars are easy to change between deploys without changing any code; unlike files, there is little chance of them being checked into the code repo accidentally; and unlike custom files, or other config mechanisms such as Java System Properties, they are language/OS-agnostic.

---

## 2 More on Secrets as Environment Variables

As the most popular way of using secrets with K8s (probably), it has its own challenge: storing the secret's values.

If we create K8s Secrets using YAML files, where do we store those YAMLs? In a Git repo? It isn't secure; you know you shouldn't do this. Technically, you can keep them encrypted, but you'd have to manage encryption keys and different repos. Even if that's fine with you, managing multiple environments and sharing secrets between teams with different needs and permissions is hard. Due to these security concerns and operational overhead, YAML with a version control system seems out of the picture.

If we don't store those YAMLs at all (thus using K8s Secrets/etcd as the single source of truth), it's hard to share secrets or migrate. Imagine when you need to move to another K8s cluster You'd have to export-import, either manually or programmatically (meaning extra overhead). Not to mention whenever you need to update a value, you'd have to execute a bunch of commands manually, and manual is error-prone. So, K8s Secrets as the single source of truth is also a no-go.

This leaves us no better option than finding a place to store those values centrally.

Enter secrets manager.

---

## 3 Secrets Managers with K8s

Secret managers are created to solve these aforementioned specific problems.

There are multiple choices on the market, like AWS Secrets Manager, HashiCorp Vault, etc. Although different secrets manager has different quirks and features, in general, a secrets manager is the place to store secret values centrally that can be used as the single source of truth.

Secrets managers would solve the security, permission, sharing, single source of truth, operational overhead issues, and so on, once and for all, leaving only one challenge: how to synchronize the secrets values from secrets managers to K8s, i.e., secrets managers to K8s integration.

Basically, there are three major ways for this:

### 3.1 Secrets Manager API Access

This way, you don't need to use K8s Secrets. You can directly read secrets from a secret manager via its API/SDK.

The downside is obvious, though.

First, you might need to change your code to call those APIs.

Second, you are vendor locked-in (think of the tremendous overhead if you wanted to change to another secrets manager: you'd have to touch the code of all the apps).

### 3.2 Agent/Sidecar

Here, let's use HashiCorp Vault as an example to illustrate:

The HashiCorp Vault has an Agent Injector, which alters pod specifications to include Vault Agent containers that render Vault secrets to a shared memory volume using [Vault Agent Templates](https://www.vaultproject.io/docs/agent/template). The injector is a [Kubernetes Mutation Webhook Controller](https://kubernetes.io/docs/reference/access-authn-authz/admission-controllers/). The controller intercepts pod events and applies mutations to the Pod if annotations exist within the request. This functionality is provided by the [vault-k8s](https://github.com/hashicorp/vault-k8s) project. For more info, read [this](https://www.vaultproject.io/docs/platform/k8s/injector).

This isn't easy to understand, but basically, by rendering secrets to a shared volume, containers within the Pod can consume Vault secrets without being Vault-aware.

There are, however, a few downsides:

First, the agent injector would inject the secrets as a volume. Every container in the Pod will be configured to mount a shared memory volume. This volume is mounted to `/vault/secrets`. To read the secrets, the containerized app must read files, which isn't Twelve-Factor App compliant, as aforementioned in section 1.1. While this method would work as an intermediate, transitional solution, if you are going full-blown cloud native soon, you would have to upgrade from it.

Second, for the injection to work, the user must configure secret injection using annotations. Example:

```yaml
vault.hashicorp.com/agent-inject-secret-foo: database/roles/app
vault.hashicorp.com/agent-inject-secret-bar: consul/creds/app
vault.hashicorp.com/role: 'app'
```

(The first annotation will be rendered to `/vault/secrets/foo`, and the second annotation will be rendered to `/vault/secrets/bar`.)

And this brings operational overhead: you need to update all your Deployments' annotation. Also, you are vendor locked-in now: if you want to move to another secret manager, you'd have to rework all the Deployment YAMLs.

### 3.3 Operator

Here, let's use [external-secrets](https://github.com/external-secrets/external-secrets) as an example:

External Secrets Operator is a Kubernetes operator that integrates external secret management systems like AWS Secrets Manager, HashiCorp Vault, Google Secrets Manager, Azure Key Vault, and many more. The operator reads information from external APIs and automatically injects the values into a K8s Secret.

There are many advantages:

- it supports multiple secrets managers - no vendor-lockin;
- no need to update your Deployment YAMLs;
- apps don't need to be changed; they can stay external-secrets-agnostic and use K8s Secrets natively.

There is one downside, which is also operational overhead: you don't need to maintain K8s Secrets YAMLs anymore, but you have to maintain the YAMLs for external secrets. An example:

```yaml
apiVersion: external-secrets.io/v1beta1
kind: SecretStore
metadata:
  name: secretstore-sample
spec:
  provider:
    aws:
      service: SecretsManager
      region: us-east-1
      auth:
        secretRef:
          accessKeyIDSecretRef:
            name: awssm-secret
            key: access-key
          secretAccessKeySecretRef:
            name: awssm-secret
            key: secret-access-key
```

This is much better than maintaining native K8s Secrets YAMLs, though, since they contain no sensitive information and can be checked into your version control system.

### 3.4 Secrets CSI

The most recent emerging way is Secrets Store CSI Driver for Kubernetes secrets, which integrates secrets stores with Kubernetes via a Container Storage Interface (CSI) volume.

The Secrets Store CSI Driver allows Kubernetes to mount multiple secrets stored in enterprise-grade external secrets managers into their pods as a volume. Once the Volume is attached, the data is mounted into the container's file system.

While this seems similar to the Agent/Sidecar way where you'd still have to read files, Secrets CSI also supports synchronizing secrets as native K8s Secrets with an extra `SecretProviderClass` custom resource, making it Twelve-Factor App compatible. An example:

```yaml
apiVersion: secrets-store.csi.x-k8s.io/v1
kind: SecretProviderClass
metadata:
  name: my-provider
spec:
  # accepted provider options: azure or vault or gcp
  provider: vault
  # [OPTIONAL] SecretObject defines the desired state of synced K8s secret objects
  secretObjects:
  - data:
  	# data field to populate
    - key: username
      # name of the mounted content to sync. this could be the object name or the object alias
      objectName: foo1
    # name of the Kubernetes Secret object
    secretName: foosecret
    # type of the Kubernetes Secret object e.g. Opaque, kubernetes.io/tls
    type: Opaque
```

This is more or less the same level of operational overhead as external-secrets: you need to manage one YAML per secret.

---

## 4 Pain Point: Auto-Reload

OK, now that we've covered many major integration methods between a secrets manager and K8s, let's get to the most significant pain point: if the secret values change in the secrets managers, how to reload the pods that are using those secrets?

With the Secrets Store CSI Driver, it can periodically update the pod mount and Kubernetes Secret with the latest content from external secrets managers (auto rotation), but the CSI driver does not restart the application pods. It only handles updating the pod mount and K8s Secret, similar to how Kubernetes handles updates to K8s Secret mounted as volumes.

Same story with other choices; there are a few "hacks", but there isn't one out-of-the-box, elegant solution with these major secrets managers. You'd have to rely on something else to do the reload/restart upon ConfigMap/Secrets change.

For example, there is one open-source project [reloader](https://github.com/stakater/Reloader) which is a Kubernetes controller to watch changes in ConfigMap and Secrets and do rolling upgrades on Pods with their associated Deployment, StatefulSet, DaemonSet, and DeploymentConfig.

---

## 5 What Are We Talking About When We Talk K8s Secrets Management

What are we really talking about when we talk about secrets managers with K8s?

Here's a list of features that I'd prioritize:

- easy to use, intuitive secrets management experience;
- easy to sync secrets to K8s Secrets without a complex setup and installation process;
- Twelve-Factor App compliant, cloud-native;
- no operational overhead: no need to change app code or Deployment YAMLs, no need to add annotations or manage extra YAMLs;
- last but not least, the killer feature: automatically reload a Deployment/Pod when secrets change.

In short: make managing and syncing secrets as easy as possible.

A secrets manager like this should exist years ago, but even in 2022, it's still frustrating to manage secrets in K8s. 

---

## 6 An Introduction to Doppler

You've probably heard and maybe even tried quite a few famous secrets managers already. For example, HashiCorp Vault; for another, AWS Secrets Manager. While they are both excellent options, they aren't exactly the ideal tool if we measure them against the list of criteria above.

Now, a new kid is on the block: [Doppler](https://www.doppler.com/).

Simply put, Doppler is trying to make it as easy as it should be to manage secrets. Easy to use? Check. Easy to sync secrets as K8s Secrets? Check. Twelve-Factor App, cloud-native? Check. No overhead? Check. Automatic reload upon secrets update? Check!

If you are interested in Doppler, [here's a quick tutorial](https://medium.com/4th-coffee/doppler-a-brief-introduction-to-secrets-managers-e779b48fac1b).

---

## 7 Summary

The evolving Kubernetes Secrets landscape now provides development teams with many choices for storing, managing, syncing, and injecting secrets into containers.

I hope this post has given you some new ideas for improving your secrets management workflows in Kubernetes.
