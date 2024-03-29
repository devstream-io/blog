---
title: "使用 KubeKey 搭建 Kubernetes/KubeSphere 环境的\"心路(累)历程\""
subtitle: "给 KubeKey 的十几个意见、建议"
author: "胡涛 | Daniel Hu"
authorLink: https://www.danielhu.cn
tags: ["kubekey", "kubesphere", "kubernetes", "中文文章"]
categories: ["Kubernetes", "中文文章"]
date: 2022-06-08

draft: false
description: "使用 KubeKey 搭建 Kubernetes/KubeSphere 环境的\"心路(累)历程\""
resources:
- name: "featured-image"
  src: "banner.jpeg"
keywords:
- k8s
- KubeKey
- Kubernetes
---

## 今天要干嘛？

今天我要给 `KubeKey` 挑个刺！

身为一个 [KubeSphere Community Member](https://github.com/orgs/kubesphere/people?query=Daniel)，至今为止我居然没有用过 [KubeKey](https://github.com/kubesphere/kubekey)，是不是很过分？说出来都觉得没脸在 KubeSphere 社区立足啊！

想当年开始玩 [KubeSphere](https://github.com/kubesphere) 时，每走一步我都觉得“不和谐”。虽说 KubeSphere 早已经有了足够的知名度和大量的企业用户，但是我总能挑出“刺”，天天给 KubeSphere 社区提意见建议……

没错，最终他们“受不了”了，决定邀请我加入 KubeSphere 社区，成为一名光荣的 Member！

现在我自己也搞开源社区了。自从开始管理 [DevStream](https://github.com/devstream-io) 开源社区后，我基本就没有精力参与 KubeSphere 社区了。哎，一颗躁动的心，一双不安分的手！我得做点啥，但是开发我是没精力参与了，要不，发挥一下我的“臭毛病” - 晚期的强迫症和极致的细节洞察力，去挑挑刺吧！

没错！

我决定试用一下 KubeKey！一方面把这些“刺”反馈给 KubeSphere 社区，帮助他们进一步完善 KubeKey 的使用体验！另外一方面在这个过程中熟悉 KubeKey 的用法，看下能不能找到 DevStream 和 KubeSphere 协作的点，比如用 DevStream 简化 KubeSphere 的安装部署和配置过程。

## 在哪里干？

KubeSphere 社区给了我一个开发机，一台 Linux vm，酷！我就在这个 Linux vm 上“干”它吧！

## 从哪里开始干？

这还不简单，[README 文档](https://github.com/kubesphere/kubekey)呀！

### 快速开干！

我们在文档里找 Quick Start，没错，有，大致长这样：

{{< figure title="KubeKey Quick Start" src="kubekey-1.png" >}}

**开炮！**

{{< figure title="KubeKey Installation 1" src="kubekey-2.png" >}}

看到这个日志，是不是看着特别像“no errors, no warnings”，一派祥和，歌舞升平，马上可以用 `kubectl` 命令看下崭新的 Kubernetes 集群了！（不要和我杠单节点 k8s 环境是不是集群，官方称之为“单节点集群”）

我就不演示 `kubectl get xxx` 了，结果惨不忍睹！我们仔细看这个输出日志，对，细品，有没有发现这里用了几行**非 error** 级别的日志告诉我们需要先安装：

- conntrack
- socat

> 给 KubeKey 的建议1：这里是不是用 error level logs 更合理一些？

> 给 KubeKey 的建议2：能不能日志里直接告诉用户需要先安装 conntrack 和 socat，并且同时打印出安装命令？（我知道 centos 和 ubuntu 安装方式不一样，不过可选的命令集合其实很有限，kk 获取系统类型也很容易，给用户更友好的提示其实很容易做到）

> 给 KubeKey 的建议3：表格里那么多项没有 y 的，到底哪些是必须有的哪些是不必须的？这个结果让用户看得心慌：我需要先安装那么多东西才能开始用 kk？望而却步啊！

### 解决依赖问题再继续干！

回到文档，其实仔细看可以在[这里](https://github.com/kubesphere/kubekey#requirements-and-recommendations)发现前置依赖，就是这段文字太长了，让人有点没耐心仔细看完。

{{< figure title="conntrack installation" src="conntrack.png" >}}

还是 Google 大法好用！我们照着这个思路执行下面命令：

```sh
yum -y install conntrack-tools
yum -y install socat
```

不出意外的话这一步很难失败。对，如果你失败了，那就是意外。真的失败了，那就是 `yum` 配置问题了，明白怎么处理吧？

然后继续执行 `./kk create cluster`，看下能不能得到和谐的输出。（记得如果环境不够“科学”，要先执行`export KKZONE=cn`，否则你会在某一步卡半天，卡到怀疑人生，无法判断是网络太慢还是网络不通）

> 给 KubeKey 的建议4：当命令卡住的时候，比如镜像下载太慢时，有没有超时逻辑？或者能不能定时输出一个日志告诉用户“没有卡死”？或者显示一下下载的进度条？总之不要长时间没有日志，用户会忍不住去 Ctrl+c，然后一脸迷茫，下一步干啥？

{{< figure title="KubeKey Installation 2" src="kubekey-3.png" >}}

比刚才和谐了，不过第一行日志很不和谐。

如果 `Failed` 影响大，比如会让某个功能不可用，那么应该用 `error` 级别，这样我会尝试去修复；反之 `WARN` 级别的意思就是“你可以忽略，忽略也能用”。OK，我选择忽略。（我不一定真的忽略，但是很多用户肯定会这样想。）

> 给 KubeKey 的建议5：尽量避免 WARN 日志，尤其是 WARN 配合 Failed 一起用，让用户又觉得这个错误很严重，又觉得可以忽略。

接着就看网络环境了。

顺利的话，最后可以看到这样的日志：

```sh
Installation is complete.

Please check the result using the command:

	kubectl get pod -A
```

让我们执行 `kubectl get pod -A`，看下“豌豆荚”们是不是正常：

```sh
$ kubectl get pod -A
NAMESPACE     NAME                                       READY   STATUS    RESTARTS   AGE
kube-system   calico-kube-controllers-75ddb95444-6dg9b   1/1     Running   0          30m
kube-system   calico-node-rqqrk                          1/1     Running   0          30m
kube-system   coredns-5495dd7c88-9hlrp                   1/1     Running   0          30m
kube-system   coredns-5495dd7c88-nlp5d                   1/1     Running   0          30m
kube-system   kube-apiserver-i-hmqwjh0m                  1/1     Running   0          30m
kube-system   kube-controller-manager-i-hmqwjh0m         1/1     Running   0          30m
kube-system   kube-proxy-x69p9                           1/1     Running   0          30m
kube-system   kube-scheduler-i-hmqwjh0m                  1/1     Running   0          30m
kube-system   nodelocaldns-vd77n                         1/1     Running   0          30m
```

我勒个乖乖，Amazing 啊！清一色的 Running、1/1 且 0 restarts，堪称完美！

## 如何干翻重来？

装好了，然后呢？

干翻重来吧，因为直接执行 `./kk create cluster` 并不带 KubeSphere。我们体验过 k8s 的安装过程了，整体还是挺简单易用的。下面试下带 KubeSphere 的玩法吧。

干翻操作：

```sh
./kk delete cluster
```

执行完这个命令后，k8s 集群会被删掉。当然，对应的 kubectl 和 docker images 我不希望被删掉，事实证明 KubeKey 也不会把它们删掉。对，我验证了：

{{< figure title="kubekey images" src="images.png" >}}

## 连着 KubeSphere 一起干！

命令是如此的简单，只需要加一个 flag 就够了：

```sh
./kk create config --with-kubesphere
```

### 干不过，输了。

结果不美丽啊：

{{< figure title="KubeSphere Installation 1" src="kubesphere-1.png" >}}

我。。。

你。。。

你给我一个 `WARN` 后就直接挂了？

> 给 KubeKey 的建议6：WARN 日志前面提到了，这里再次印证我的观点，日志级别要严谨，WARN 要少用。

这个错误，，，看起来是要 Google 一会了。但是我懒啊！这不就是和 `docker` 安装方式有关嘛（我猜可能和版本也有关系），我卸掉 docker，让 KubeKey 来安装总行了吧！

> 给 KubeKey 的建议7：如果你们知道这个错误的解决方案，请把方法（或者参考资料链接）贴到日志里，不要期待用户可以轻松 Google 一下解决。当然，我不能接受你说我解决不了，哈哈，不过现在是晚上10:30分，我想快点打完收工了，不查了。

其实这个机子本来没有 docker，为什么我会提前安装呢？因为我勤劳？不存在的。我是被这段内容误导的：

{{< figure title="KubeKey Installation Docs" src="docs-1.png" >}}

看这个缩进级别，你瞟一眼，是不是最显眼的是下面的 Note？Note 里说先安装 docker，然后假如你很熟悉 docker，是不是就顺手先装了 docker 在继续看文档？

对，我就跪在了这里。装了 docker 才发现，这个 Note 是针对 *Build Binary from Source Code* 的 Note。。。

> 给 KubeKey 的建议8：这里的文档也优化下吧，让 Note 看起来不那么像全局 Note。

### 重整旗鼓，继续干！

上来就是一套组合拳：

```sh
yum remove docker
./kk create cluster --with-kubesphere
```

不出我所料，错误绕过去了！

对，我就是这么心安理得，可以忍住不去查前面这个错误是怎么回事，我选择绕过后晚上还是可以睡得香喷喷！我相信 KubeSphere 社区过几天会告诉我答案的。

{{< figure title="KubeSphere Installation 2" src="kubesphere-2.png" >}}

截图看不到效果，大家脑补一下上面这个箭头会左右移动，对，动态的！太可爱了吧！明天我要去问下谁写的这个代码，真是个可爱的程序员！

在等这个箭头停下来的时间里，再给 KubeKey 加一个建议吧：

{{< figure title="KubeKey Installation Docs" src="docs-2.png" >}}

> 给 KubeKey 的建议9：这里的文档也优化下吧，风格一致更优雅，要么都写一个“示例”版本，要么都用[version]占位。

{{< figure title="KubeSphere Installation 3" src="yes.png" >}}

> 给 KubeKey 的建议10：y 和 yes 在用户意图里绝对是一模一样，要不要兼容下？包括大小写（我没有测试大小写是不是兼容）

{{< figure title="KubeSphere Installation 4" src="ks-installer.png" >}}

> 给 KubeKey 的建议11：如果一次安装长时间卡住，可能是网络问题。这时候如果 Ctrl+c 然后重新执行安装，会出现这种情况，没错，两个 ks-installer。当然，我知道需要先 delete 一下就能绕过去，但是或许 kk 能够识别到用户没有 delete 并且给出提示，那就更“优雅”了。

### 再次重整旗鼓，继续干！

由于我用的云主机，前面安装过程太久，我去洗了个澡，回来后终端断开连接了。没错，再次进去的时候我忘记执行 `export KKZONE=cn` 了，所以失败了一次。再次执行的时候忘记 delete 了，所以再失败了一次。

于是，我要 delete 了：

{{< figure title="KubeSphere Installation 5" src="kubesphere-3.png" >}}

惨不忍睹，惨绝人寰啊！！！

你给我装的 docker 呀，怎么还能有这个错误？

同样，你告诉我 WARN，我是不会上心的。

无视

and

继续！

{{< figure title="KubeSphere Installation 6" src="kubesphere-4.png" >}}

好玩的事情是，这次看着错误信息似乎，，，和前面没啥区别呀。。。但是没有直接退出程序，而是继续往下走了！好吧，我是不会去研究这个错误的！

又到了这个“可爱到爆”的箭头了，继续漫长的等待吧～

{{< figure title="KubeSphere Installation 7" src="kubesphere-5.png" >}}

不对劲啊，为什么那么久，是不是挂了？

好吧，唤醒一下我沉睡多年（准确说是大半年）的 k8s 排错技能！

我看到了 ks-installer pod 的错误 events 信息：

{{< figure title="KubeSphere Installation 8" src="kubesphere-6.png" >}}

有意思，这是个大 bug 了吧，镜像不存在？我得去问一下内部人员：

{{< figure title="KubeSphere Installation 9" src="kubesphere-7.png" >}}

> 给 KubeKey 的建议11：对着文档走不通安装过程，这算大 bug 了，不用我细说怎么 fix 了吧。

### 一鼓作气，再而衰，三而竭，竭前最后挣扎着干一次

没力气打“组合拳”了，一条简单的命令，然后小拳拳敲个回车：

```sh
./kk create cluster --with-kubesphere v3.2.1
```

{{< figure title="KubeSphere Installation 10" src="kubesphere-8.png" >}}

好家伙，一把给我整到了晚上11点。。。我不要面子的啊，我在家办公还要加班啊，我是朝十晚四午休三小时的男人啊……

不过说实话，看到这个熟悉的日志，还是很亲切的！毕竟去年不知道看了多少遍这个日志！（别不信，我是深度玩过的，有[视频](https://www.danielhu.cn/speech-cic-2021/)为证）

继续看下 pod

{{< figure title="KubeSphere Installation 11" src="kubesphere-9.png" >}}

很和谐！Amazing 啊！

（其实没有特别和谐，restarts 不是全0。不过没关系，我知道这不影响功能，只是安装过程还有优化的空间而已。）

### 你想看最后的 Dashboard ？我也想！

不过我没有 ELB，焦急等待中……（委托大佬给我配置 ELB 去了）

半小时后……

登录页面来了！

{{< figure title="KubeSphere Login" src="login.png" >}}

进去后，熟悉的感觉，熟悉的味道！

{{< figure title="KubeSphere Dashboard" src="dashboard.png" >}}

后面我一定要再写几篇文章介绍下 KubeSphere 怎么玩！当年那一堆笔记，那些付在 KubeSphere 上的青春，不写点啥可惜了！

## 最后的总结

1. [官方文档](https://kubesphere.io/docs/installing-on-linux/introduction/kubekey/)比 GitHub 的 README 更适合“普通用户”；
2. 好困啊！！！我要睡觉了，总结啥嘛，没啥好总结的。

## 最后的最后

转载请注明出处！

更多我的文章，请移步我的个人网站：<https://www.danielhu.cn>

更多 DevStream 团队的文章，请移步 DevStream 博客网站：<https://blog.devstream.io>

想要及时收到我的最新文章，请关注我的个人公众号：**胡说云原生**
