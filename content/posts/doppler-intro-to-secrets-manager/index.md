---
title: "Doppler: A Brief Introduction to Secrets Managers"
author: "Tiexin Guo | 郭铁心"
authorLink: https://www.guotiexin.com
tags: ["security", "devsecops", "tutorial", "english-articles"]
categories: ["Security", "DevSecOps", "Tutorial", "English Articles"]
date: 2022-08-11

resources:
- name: "featured-image"
  src: "doppler.png"
- name: "featured-image-preview"
  src: "doppler.png"
keywords:
- DevOps
- DevSecOps
- Security
- Secrets Manager
- SecretsOps
---

_Disclaimer:_

_This article is originally published on [4th-Coffee, a Medium DevOps Publication](https://medium.com/4th-coffee/doppler-a-brief-introduction-to-secrets-managers-e779b48fac1b)._

_The views and opinions expressed in this article are solely my own; they did not, do not, and will not represent my employer._

_Author's note:_

_This blog post walks you through the basic usage and popular integrations of Doppler, and makes a comparison between Doppler and other equivalents. Basic DevOps knowledge is required, for example, Kubernetes, kubectl, helm, minikube, 12-factor app, etc._

---

## 1 A Brief History of Secrets

### 1.1 Secrets in the Software Development Lifecycle (SDLC)

OK, I'll cut to the chase and get straight to the point:

If you are in the software engineering business, you _are_ gonna have to deal with secrets. I'm not kidding.

- When you are developing an app, the app itself might need access to a database, so it needs the password. Maybe the app talks to another API that requires authentication, so you need a token, which is a secret.
- When you launch your virtual machine, you might need to provide an initial password too.
- In your continuous integration system, you might want to do something in your pipeline to write something to somewhere, but that somewhere is access-controlled. You'd have to provide a secret to make it work. For example, uploading some artifacts to a private S3 bucket.
- In your continuous deployment system, you might want to operate a virtual machine or a Kubernetes cluster for deployment, both of which require authentication. That's a secret.

I could go on, but I don't have to because I'm pretty sure you already have got the point: you don't _have to_ deal with secrets; you _must_.

### 1.2 The History of Secrets Management

Before the emergence of secret managers, there were a few ways to handle secrets (not so gracefully.)

One way is to store it locally, be it an ENV file or what have you. But storing secrets locally has a few drawbacks:

- If you lose it, you lose it forever.
- If you share it with others and then change it, it's hard to version-control them and change all the copies simultaneously.
- It's hard to have any level of access control; you can't "forbid" others to share it with somebody else.

It's crystal clear that you don't want to store your secrets locally, for sure. What choice do you have, then? Oh, easy-peasy, we have a git repo; we'll just do it there; it'll work. I know it would work, but deep down in your heart, you know you shouldn't do this because it's "bad practice" and "frowned upon."

OK, so the git way is out. But wait. Or is it? What if I store it encrypted? I hear that [Ansible Vault](https://docs.ansible.com/ansible/latest/user_guide/vault.html) or [git-crypt](https://github.com/AGWA/git-crypt) can handle the encryption/decryption automatically for me? Yep, that's right. But before diving into those solutions, let me stop you first:

- Managing "who has access and who don't" and "who can access what" is a bit of a hassle for tools like these. It's possible but definitely brings operational overhead.
- There are chances you will need to use the same secrets in other places that don't use Ansible, don't use git. As in the previous example, your CI may need access to an AWS S3 bucket.
- There is a chance that the same secret exists in multiple vaults or repos, so it's hard to keep track of which is the latest version.

A solution like this is more of a special-purpose one, not universal for all usages. For configuration management with Ansible, if you want to handle some secrets only for that use, go for Ansible Vault. For a small team with a single repo, go for git-crypt. You can't go wrong with these answers, and even if you do, it's a two-way door solution: you can quickly reverse back and move to something more sophisticated and suitable for your exact needs should you need it.

Another way is to rely on the power of other tools. For example, if you are using a Kubernetes cluster, K8s can store and handle secrets. But the obvious drawback is that it's not simple if you want to use those secrets elsewhere other than from within a K8s cluster.

Heck, how to solve all those problems, then?

Enter secrets manager.

### 1.3 Secrets Managers

Let me try to write a nice and short answer first (or maybe just forget about the "nice" part completely; anything "short" will do:)

Just like any secrets manager for personal usage, like [LastPass](https://www.lastpass.com/), [1Password](https://1password.com/), [BitWarden](https://bitwarden.com/) or any other tool, a secrets manager in the DevOps field is for securely storing secrets and credentials, as the single source of truth, which can be then consumed (hopefully in an automated manner) in any stage of the software development lifecycle.

I guess I can safely assume that we can skip the part on "why do I need a secrets manager," can't I? Then let's get down to the more important question:

Based on a simple Google search, there are many secret managers. Which one to use? What criteria should I be looking for when choosing one?

### 1.4 Choosing Your Secrets Manager

Based on my professional experience, there are a few things to watch:

**Is it easy to use?**

If not, maybe it's not worth it. After all, the underlying reason for using a secrets manager is to reduce our operational overhead, not to increase it.

**Does it integrate well with all the tools you are already using?**

For example, if a secrets manager can't be integrated with your choice of CI system, it's useless.

**Does it require a lot of changes to your existing applications fleet?**

If you'll have to change all your apps to use it, maybe it's not worth it after all.

---

## 2 The New Kid on the Block: Doppler

You've probably heard and maybe even tried quite a few famous secrets managers already. For example, HashiCorp Vault; for another, AWS Secrets Manager. While they are both excellent options, they both have their own quirks and features, which we will discuss later in this article.

For now, let's talk about an emerging choice: [Doppler](https://www.doppler.com/).

Why Doppler? Good question.

According to [Crunchbase](https://www.crunchbase.com/organization/doppler), Doppler was founded in 2018 and got a $20M series A funding in April 2022. According to [GitHub](https://github.com/DopplerHQ), it seems they are not fully open-source, but they do have a few open-source projects.

Simply put:

> Doppler is the multi-cloud SecretOps Platform developers and security teams trust to provide secrets management at enterprise scale.

OK, the "multi-cloud" and "scale" parts got me. Let's give it a go.

---

## 3 Doppler Quickstart

### 3.1 Create a Doppler Project

Let's [sign in using this link](https://dashboard.doppler.com/login). For an easier setup, we can choose GitHub SSO to log in.

Upon your first successful sign-in, the wizard will walk you through the steps required to create a project. Here I created an "example-project."

In the dashboard of Doppler, we can see that we can create multiple projects, and for each project, we can have multiple environments, just like what a real-world project should look like.

In the "example-project" DEV environment, I have created a secret named "NAME", with the value "Tiexin" (my name). I.E., a key-value pair `NAME: Tiexin`.


### 3.2 CLI Setup

For macOS users, install the CLI by running the following commands:

```shell
brew install gnupg
brew install dopplerhq/cli/doppler
```

For other O.S., see [the official doc here](https://docs.doppler.com/docs/install-cli).

For local development, we use the `doppler login` command, which will open a browser window and ask you to authenticate. Run:

```shell
doppler login
```

Now that the CLI is installed and we are logged in, let's configure it for use with a project in our development environment:

```shell
cd ./your/project/directory
doppler setup
```

You'll get a similar interactive experience similar to this:

![](https://files.readme.io/59b4e0d-doppler-yaml-setup.gif)

---

## 4 Doppler Python Integration

For simplicity, let's test the usage of Doppler with Python.

### 4.1 Preparing a Simple Python Test File

Prepare the most straightforward python file `main.py` with only two lines of content:

```python
import os

print(os.getenv("NAME"))
```

Note that the "NAME" is the secret we have created earlier in step 3.1.

The code here is supposed to get the environment variable "NAME" and print it out.

### 4.2 Go Time

Next, instead of running `python3 main.py`, we prefix the command with `doppler run -- `. I.E., run:

```shell
doppler run -- python3 main.py
```

And we can get the result:

```shell
$ doppler run -- python3 main.py
Tiexin
```

Now we see how it works: the `doppler run -- ` prefix will inject the secrets from Doppler to the environment variables to your program so that you don't have to manage ENV vars or .env files anymore!

### 4.3 Production Environment?

OK, this is nice and easy to read a secret directly from a program without changing anything (almost; only need to prefix the command with `doppler run -- `.)

But, what if we are not in the development environment?

Nowadays, Kubernetes has become the de-facto place to orchestrate your containerized workload. What if I'm running my app in K8s, following the [12-factor app](https://12factor.net/) methodology by storing all config in the environment?

Next, let's tackle exactly that:

---

## 5 Doppler K8s Integration

For Kubernetes, Doppler offers a [Kubernetes Operator](https://docs.doppler.com/docs/kubernetes-operator), a background service that syncs secrets to Kubernetes with automatic deployment updates to ensure applications always have the latest version of secrets.

### 5.1 Creating a K8s Cluster

To test this, we need a K8s cluster.

> If you haven't got one yet, I recommend creating one using [minikube](https://minikube.sigs.k8s.io/docs/start/).
> 
> For macOS users, it's easiest to use the minikube + Docker driver + Docker Desktop combo to spin up a working K8s cluster in a few minutes at the press of a few buttons. Run: `minikube start --driver docker`
>
> We also need to use [helm](https://helm.sh/) to install the K8s operator.

### 5.2 Install Doppler K8s Operator

Once we get our K8s cluster up and running, we can install the Doppler K8s operator by running:

```shell
helm repo add doppler https://helm.doppler.com
helm install --generate-name doppler/doppler-kubernetes-operator
```

If you prefer to use `kubectl` instead of helm to install Doppler K8s operator, you can deploy it by applying the latest installation YAML directly from the Doppler Kubernetes GitHub repository. Run:

```shell
kubectl apply -f https://github.com/DopplerHQ/kubernetes-operator/releases/latest/download/recommended.yaml
```

### 5.3 K8s Operator Setup

Then, we need two things to get the Dopper secrets synchronized to our K8s cluster:

1. Doppler Token Secret: used by the operator so that it can sync the secrets in Doppler. Creating one is the same as described in section 3.2.
2. `DopplerSecret`: A [CRD](https://kubernetes.io/docs/concepts/extend-kubernetes/api-extension/custom-resources/) that references the Doppler Token Secret that provides the name namespace for the Kubernetes secret created and managed by the Operator.

For the Doppler CLI to access secrets for your projects, it needs an access token. A Doppler Service Token provides read-only secrets access to a specific config within a project. It adheres to the least privilege principle by ensuring an application only has access to a single config within a project for use in live environments.

To create a secret token:

1. Go to the Project and select a Config
2. Click the Access tab.
3. Click on Generate Service Token.
4. Copy the Service Token as it is only shown once.

See the screenshot animation below:

![](https://files.readme.io/5ade11b-ServiceTokens.gif)

Then, with the generated Doppler Service Token, use it in the command below to create your Doppler token secret:

```shell
kubectl create secret generic doppler-token-secret \
  --namespace doppler-operator-system \
  --from-literal=serviceToken=dp.st.dev.XXXX
```

(Replace the "dp.st.dev.XXXX" part with your generated token.)

Then, to create the CRD, first, create a YAML file with the following content, and use `kubectl` to apply it:

File `crd.yaml`:

```yaml
apiVersion: secrets.doppler.com/v1alpha1
kind: DopplerSecret
metadata:
  name: dopplersecret-test # DopplerSecret Name
  namespace: doppler-operator-system
spec:
  tokenSecret: # Kubernetes service token secret (namespace defaults to doppler-operator-system)
    name: doppler-token-secret
  managedSecret: # Kubernetes managed secret (will be created if does not exist)
    name: doppler-test-secret
    namespace: default # Should match the namespace of deployments that will use the secret
```

Run:

```shell
kubectl apply -f crd.yaml
```

Now, if we run:

```shell
kubectl get secrets
```

We will see a K8s secret is created automatically with the content in the Doppler project. We can use that secret directly! What happened under the hood is, the K8s operator has injected the secrets into Kubernetes from Doppler.

### 5.4 K8s Deployment Demo

Now we are ready for a Doppler-K8s integration test.

Here, we will use a simple app written in Golang.

- The source code is [here](https://github.com/IronCore864/go-hello-http/blob/master/main.go). Basically, this program creates an HTTP server and returns "Hello, world!". If the ENV VAR `NAME` is set, it returns "Hello, $NAME!" instead.
- I've built the Docker image as well, which is [`ironcore864/go-hello-http:1.0.0`](https://hub.docker.com/layers/go-hello-http/ironcore864/go-hello-http/1.0.0/images/sha256-91f6d29ed6efa5f39a2fc9ed6a1e7f84af1200f1b06f52990b000441a4768028?context=repo).

Now, let's deploy this app in our K8s cluster. Create a YAML file with the following content:

File `app.yaml`:

```yaml
---
apiVersion: apps/v1
kind: Deployment
metadata:
  name: hello
  labels:
    app: hello
  annotations:
    secrets.doppler.com/reload: 'true'
spec:
  replicas: 1
  selector:
    matchLabels:
      app: hello
  template:
    metadata:
      labels:
        app: hello
    spec:
      containers:
      - name: hello
        image: "ironcore864/go-hello-http:1.0.0"
        envFrom:
        - secretRef:
            name: doppler-test-secret
        ports:
        - containerPort: 8080
---
kind: Service
apiVersion: v1
metadata:
  name: hello
spec:
  selector:
    app: hello
  ports:
    - port: 80
      targetPort: 8080
```

Then deploy it:

```shell
kubectl apply -f app.yaml
```

Then, let's forward the service port to the local host and try to access it:

```shell
kubectl port-forward svc/hello 8080:80
# in another terminal tab
$ curl localhost:8080
Hello, Tiexin!
```

As we can see, the app is reading the Doppler secret "NAME" correctly, and we did not change anything to the app at all; everything is K8s native!

### 5.5 Automatic Redeployments

If we pay attention to the `app.yaml` file in the previous section, there is an annotation in the deployment with the value `secrets.doppler.com/reload: 'true'`.

This enables the operator to reload a deployment when a secret's content in Doppler has changed. How great is that! No extra effort is required whatsoever, only a simple annotation.

### 5.6 Clean Up

If we created the cluster using minikube, we could destroy it by running:

```shell
minikube delete
```

---

## 6 Final Thoughts and Comparisons

### 6.1 Quirks and Features

In the dashboard of Doppler, there is a feature where you can easily import your secrets by copying and pasting the whole env file contents, which is a very helpful feature if you didn't have any form of secrets manager before and can get you started quickly by importing all your env files.

### 6.2 Project-Based Access Control

If you click around in the web-based dashboard U.I., you can see that Doppler has users, groups, and roles, just like any tool that supports role-based access control (RBAC;) but at the same time, all the secrets are managed and separated by the concept of "project", I.E., project-based access control.

With other secrets managers, for example, HashiCorp Vault or AWS Secrets Managers, you can achieve the same goal by putting all secrets of a specific project into a separate path and then setting up RBAC. This has its advantages and disadvantages as well. The good thing is flexibility: you decide where to store what. The bad is that if you are not well-organized, the secrets will be messy in the secrets manager, and it will be hard to manage the roles and access policies.

Doppler's project-based access eliminated these downsides, making it cleaner to manage. However, the downside is that if there is a "shared" secret between different projects, it can be a bit tricky to manage:

- For workplaces on paid plans, there is a solution where you can [reference secrets across configs and projects](https://docs.doppler.com/docs/secrets#referencing-across-projects), so that you can create a shared project for shared secrets between multiple projects.
- If you are using free plans of Doppler, there isn't a way to do a cross-project secret referencing, meaning that you'd have to store it at multiple places.

Besides projects, Doppler has the concept of "environments" built in. There isn't any extra operational overhead if you are (chances are, you probably really are) managing multiple environments.

### 6.3 Cloud Service Only, no On-Premise

HashiCorp Vault can be used both on-premise and as a cloud-based service, but AWS secrets manager only can be used as a cloud service. Here, Doppler currently takes the same path as AWS secrets manager: it can only be used as a cloud service; you can't deploy it on-prem.

This wouldn't be an issue for tech companies and small start-ups. However, given the current concerns on data ownership and security, especially in specific sectors of business (FinTech, for example,) no on-premise deployment might be a deal breaker.

However, while Doppler don't have the on-premise deployment option yet, they do have a zero-trust BYOK (Bring Your Own Key) Enterprise Key Management solution to [put data access control in customers' hands](https://docs.doppler.com/docs/enterprise-key-management).

### 6.4 Good Integrations with Minimum Operational Overhead

From my personal point of view, the most vital point of Doppler is its integrations, primarily with Kubernetes.

As aforementioned, Kubernetes has become the de-facto place to run your containerized workload. It's so crucial that it itself has almost transformed from a "tool" to a "platform." So, the ease of integration with K8s is crucial, especially for 12-factor apps and cloud-native believers.

This is where Doppler shines. It provides an excellent and easy-to-use K8s operator that basically does everything for you. It syncs the secrets automatically, and it can detect secret value changes and redeploy your workloads.

For HashiCorp Vault, you'd have to [use an agent as a sidecar to inject secrets](https://learn.hashicorp.com/tutorials/vault/kubernetes-sidecar) (which will be presented as a file in the container, not so 12-factor compliant) or [mount secret via CSI](https://learn.hashicorp.com/tutorials/vault/kubernetes-secret-store-driver?in=vault/kubernetes) and [sync](https://secrets-store-csi-driver.sigs.k8s.io/topics/sync-as-kubernetes-secret.html) (this method achieves more or less the same as Doppler K8s operator.) But, with Vault, it's not easy to redeploy upon secret value change.

It's worth mentioning that there is another solution for K8s secrets sync, which is [Kubernetes External Secrets](https://github.com/external-secrets/kubernetes-external-secrets). It allows you to use an external secrets manager, like AWS Secrets Manager or HashiCorp Vault to add secrets in Kubernetes securely. However, it's not easy to make it sync automatically upon secrets change, and this solution is only for Kubernetes; you'd still have to use a secrets manager for it to work.

### 6.5 Does It Mean We Should Never Use HashiCorp Vault Anymore?

Short answer, no.

Vault is more than a "secrets manager for projects" (applications/microservices.) What you can achieve with Vault is decided by its [engine](https://www.vaultproject.io/docs/secrets). For example, with the Vault AWS engine, you can implement temporary AWS access control automatically. For another, you can manage database secrets and rotations with Vault.

On the other hand, _at the moment_, AWS secrets manager and Doppler are more focused on the management of application secrets.

With that said, Doppler is catching up to Vault with [dynamic secrets](https://docs.doppler.com/docs/dynamic-secrets) for AWS. Dynamic secrets are one-off secret "leases" that are valid for a defined period of time. They're required to have a time-to-live (TTL) and are automatically deleted upon expiration via their integration, so they work similarly to Vault's AWS engine. Also, according to Doppler, multi-cloud secrets rotation is coming in Q4 2022.

---

## 7 Summary

Choosing the right tool for the job is never easy. There is no one-size-fits-all silver bullet answer for everyone; it always depends.

For example, if you want one tool to manage everything for you, HashiCorp Vault is probably the way to go. Still, it also comes with downsides, for example, the Kubernetes secrets injection/sync and no native, out-of-the-box project-level access control.

For another example, if you are already using AWS services and want perfect integrations with AWS services, maybe AWS secrets manager is the correct answer for you. But in this case, you face the vendor-lockin issue, and it might not be easy if you use a hybrid or multi-cloud model.

If what you want is:

- a place to easily manage all the secrets used by your applications/microservices
- apps run in k8s
- you have some legacy env files to worry about
- you don't need the tool to do everything (for example, no need to manage infrastructure credentials with it)
- you believe the "do-one-thing-and-do-it-right" philosophy
- you want cloud-based service to reduce operational overhead to the minimum

I think it's fair to say that Doppler is a great choice.

If you enjoy this article, please like, comment, and subscribe. See you at the next one!
