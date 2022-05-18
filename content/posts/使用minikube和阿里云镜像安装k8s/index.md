---
title: "minikube搭建本地开发测试环境"
author: "HaoKe | 柯浩"
authorLink: http://github.com/KeHaohaoke
tags: ["kubernetes", "minikube"]
categories: ["kubernetes","minikube","General","中文文章"]
date: 2022-05-18
---
> **相关背景**： 在日常开发测试中，我们验证和k8s有关的功能的时候，并不需要生产环境规格的k8s集群
> 比如(3 master+3 node)，这种规格的集群，我们个人一般不具有这种配置，也没有必要为了本地测试开发的验证而去专门搭建这样的集群（除非有特殊需要）。
> 本文则介绍一种利用minikube来搭建k8s本地开发测试环境的方法。
> 本文是在MacOS并且是Intel芯片的机器上进行安装minikube的。

> **概要**：本文讲述了在MacOS Intel机器上：
> 
> 使用minikube分别以vmware作为driver和使用docker作为driver来安装one node的k8s

## minikube安装准备

工欲善其事，必先利其器。我们先来下载minikube。

``` shell
brew  install minikube
```

> 如果你的Mac上没有安装brew，请参考：[Homebrew官网](https://brew.sh/)进行安装

接下来，我们还要下载kubectl

``` shell
brew install kubectl
```


在MacOS上，minikube支持多种不同的driver：
 -  Docker
 - 	VirtualBox
 - 	VMWare Fusion
 - 	HyperKit

接下来，作者会采用两种安装方式：一种使用VM: VMware Fusion， 另外还有Docker两种方式来进行。

### 使用VMWare Fusion安装

其实很简单，只要一条命令就好了

#### 安装和启动-vmware driver
``` shell
minikube start --driver=vmware --memory=2048 --cpus=2 --image-repository='registry.cn-hangzhou.aliyuncs.com/google_containers'
```

> **特别说明**：在这里，作者使用了阿里云的镜像。如果没有缺省该参数，即不指定image-repository参数的话，会默认从gcr.io去下载镜像，改地址由于一些网络原因，是会被ban掉的
> 其实，也是有方法可以去gcr.io下载的，但这里就交给读者自己研究了。
> 但用指定阿里云的镜像的方法，会在下载速度上快很多，即使读者研究了方法去从gcr.io下载image，其速度也不是从阿里云下载可比的。
> 作为开发测试环境，我们希望的是快速完成搭建并使用，倒腾具体的网络下载相关的配置可以放到闲余时间研究。

很快，就会看到安装且启动完毕：

![minikube-vmware-aliyun](./minikube-vmware-aliyun.png)
比较有趣的是，可以看到，minikube使用了很多emoji表情表示了每个安装阶段的状态。这可能也是程序员可爱的一面，在其他的技术产品上，比如github，也有很多emoji表情在使用，读者有兴趣可以自己去找找。

#### 启动dashboard

也是很简单的一条命令

``` shell
 minikube dashboard &
```

> 说明：加上&是为了保持后台启动，以免关掉了执行这个命令的session导致相关进程被杀掉，也可以说防止为了保持dashboard一直运行，不得不长期开着一个session。

![minikube-dashboard](./minikube-dashboard.png)
这条命令会自动拉起浏览器打开上图所示链接，会看到k8s的dashboard。

![k8s dashboard](./k8s-dashboard.png)

## 使用docker driver安装

> 在更换driver之前，把原来的minikube stop并delete掉

嗯，还是简单的一条命令。其实是两条啦

``` shell
minikube stop
minikube delete
```

![minkube-stop](./minikube-stop.png)
在使用docker作为driver之前，要确认MacOS上已经安装好了docker desktop。

安装也很简单：

遵循这里的guide去做就OK了。[macos-docker](https://docs.docker.com/desktop/mac/install/)

作者在这里也去说明一下安装方法:

1. 去docker官网的链接，找到你的机器对应的安装，点击下载
[docker-desktop-install](https://docs.docker.com/desktop/mac/install/)


![select-macos-type](./select-macos-type.png)

   2. 之后，就和安装其他的MacOS软件一样，都是拖拽安装的。

![install-docker-desktop](./install-docker-desktop.png)

>如果有喜欢命令行安装的朋友，可以看这个链接 [install-docker-desktop-from-command-line](https://docs.docker.com/desktop/mac/install/#install-from-the-command-line)

安装完成之后，就可以启动docker desktop

> Tips: 如果MAC配置不高，比如作者的MAC，买的比较早一些，可以在启动docker desktop之前，去设置内存小一点

修改docker desktop的内存的步骤： 在MAC右上角的状态栏找到docker desktop->Preferences->按需修改内存和CPU的设置

![docker-desktop-config](./docker-desktop-config.png)


3. 启动了docker desktop之后，就可以用docker driver的方式用minikube来安装k8s了 

也很简单，就是一条命令：）

``` shell
 minikube start --memory=2048 --cpus=2 --image-repository='registry.cn-hangzhou.aliyuncs.com/google_containers'
```

![minikube-docker-install](./minikube-docker-install.png)

同使用vmware driver一样，我们start完成之后，需要启动dashboard。

![minikube-docker-dashboard-start](./minikube-docker-dashboard-start.png)


同样也会自动拉起浏览器打开dashboard的url

![minikube-docker-dashboard-url](./minikube-docker-dashboard-url.png)
## 查看k8s

这就和普通的k8s没什么两样了, 使用相应的命令查看即可

``` shell
kubectl get node                                                                                          
NAME       STATUS   ROLES                  AGE   VERSION
minikube   Ready    control-plane,master   15m   v1.23.3
```
