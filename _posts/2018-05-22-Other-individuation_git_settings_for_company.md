---
layout: post
title:  "【其他】为公司项目个性化 Git 设置"
date:   2018-05-22 20:15:36
categories: Other
---

公司最近决定把所有项目全部迁至 Git 仓库，我们用的是 Gogs（一个开源方案）。



我在 Gogs 上注册帐户，接下来我要去设置我的 ssh 密钥。



当我打开的本地的 id_rsa.pub 文件（之前我用 Github，已经生成过 ssh 密钥，不会的请点[这里](https://www.jianshu.com/p/9317a927e844)），发现密钥最后有我的邮箱地址，这个邮箱地址是我注册 Github 时的邮箱地址。



心里有一丝不情愿，不想把这个 ssh 公钥输入到 Gogs 仓库中，因为这个 ssh 密钥当时是为 Github 生成的。



虽然不情愿，但为了不影响进度，我还是把 这个ssh 公钥，输入到 Gogs 仓库的 ssh 密钥设置中。



接下来，提交代码，一切都正常，但当我查看提交历史时，我发现我的每次提交用户名是我自己设置的一个英文名，这下就尴尬了，这个英文名本来是为了保护自己的隐私，专门给 Github 设置的，如果用这个英文名，那在公司就很不方便了。



联想到上边 Gogs 的 ssh 密钥问题，我决定针对公司的 Gogs 仓库设置专门的 ssh 密钥和 commit 用户名。



中间过程不细说，我请教了同事和百度后，总结了操作方法。

### 生成另一个 ssh 密钥

打开 git 命令行，输入以下指令：

> ssh-keygen -t rsa -C "youremail@email.com" -f ~/.ssh/your_new_ssh 

回车后就能生成一个新的 ssh 密钥，一个是 your_new_ssh（私钥），另一个是your_new_ssh.pub（公钥）。

这下 .ssh 文件夹中就有两对公钥和私钥了，需要有一个规则，指明 Github 用哪个，Gogs 用哪个。在 .ssh 中新建一个 config 文件（无后缀）,在里边写上以下内容：

```
Host github.com
	HostName github.com
	PreferredAuthentications publickey
	IdentityFile ~/.ssh/id_rsa

Host 你的Gogs地址
	HostName 你的Gogs地址
	PreferredAuthentications publickey
	IdentityFile ~/.ssh/your_new_ssh
```

这样就 OK 了，Github 访问用 id_rsa，Gogs 用 your_new_ssh。

### 指定 commit 时的用户名

这个也很简单，在 公司项目的文件夹下设置，这样可以确保只作用于公司项目。

> git config user.name "your_name"