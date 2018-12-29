---
layout:     post
date:       2018-12-29
title:      使用ssh连接github
cover:  https://siyuanlau.github.io/img/random/material-1.png
categories: 开发工具
tags:
    - github
    - ssh
---
使用ssh key的好处：不需要每次提交的时候都输入用户名密码。

<!--more-->
## 基本步骤
1. 在你的电脑上生成一个ssh key。
如何在本机上生成ssh key取决于你的git客户端，我的系统是windows10，使用的是tortoisegit。
如果你用的是github自家的[github desktop](https://desktop.github.com/)，可以参考[官方步骤](https://help.github.com/articles/connecting-to-github-with-ssh/)。


安装了tortoisegit后，它自带了两个ssh相关的程序：puttygen和pageant。可以在开始菜单的tortoisegit里面找到他们。puttygen用来生成ssh key。pageant用来管理key和作为一个ssh连接的代理。
运行puttygen程序即可生成key。生成以'ssh-rsa'开头的公钥字符串。将其复制、后面需要添加到github网站。
同时将私钥保存成pagent使用的ppk文件。

2. 再运行pageant添加代理。
运行pageant，将上一步保存的ppk私钥文件添加到ssh-agent。（ssh连接代理）

3. 在github网站后台将ssh key关联到你的账户。
4. 使用ssh形式的url连接github。
   github仓库的地址有两种形式：
   https: `https://github.com/hchaojie/xxx.git`
   ssh: `git@github.com:hchaojie/xxx.git`
   不要使用https的。


