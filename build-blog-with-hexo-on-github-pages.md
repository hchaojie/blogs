---
layout:     post
date:       2018-12-25
title:      使用hexo在github pages上面创建个人博客
cover:  https://siyuanlau.github.io/img/random/material-4.png
categories: 前端开发
tags:
    - blog
    - Hexo
    - github pages
---
流行的建站工具有三款：
1. [jekyll](https://jekyllrb.com/)，基于ruby语言。
2. [hugo](https://gohugo.io/)，基于go语言。
3. [hexo](https://hexo.io)，基于node.js。

其中hexo在国内用得比较多，有大量的主题。查看github主页，提交最多的是一个台湾人，应该就是创始人了。

<!--more-->

流行的建站工具有三款：
1. [jekyll](https://jekyllrb.com/)，基于ruby语言。
2. [hugo](https://gohugo.io/)，基于go语言。
3. [hexo](https://hexo.io)，基于node.js。

其中hexo在国内用得比较多，有大量的主题。查看github主页，提交最多的是一个台湾人，应该就是创始人了。

### 前提
1. 安装git(windows的话可以使用[tortoise git](https://tortoisegit.org/))
2. 安装node.js

### hexo安装和建站步骤
参考[官方文档](https://hexo.io/zh-cn/docs/)

1. 安装hexo
```
npm install hexo-cli -g
```

2. 建站
```
hexo init hchaojie-blog
cd hchaojie-blog
npm install
```

3. 生成静态页面
```
hexo g
```

实际上是把`<项目根目录>\source\_posts` 里面的markdown格式生成了对应的html页面。
生成的文件在`<项目根目录>\public`文件夹里面。

4. 本地运行
```
hexo server
```
可以通过地址http://localhost:4000来访问。

### 使用主题
默认生成的页面比较丑，你可以到官网选择喜欢的主题：
[主题列表](https://hexo.io/themes/)
[10个最受欢迎的主题](https://en.abnerchou.me/Blog/5c00ca67/)
[hexo-theme-yilia](https://github.com/litten/hexo-theme-yilia)
[hexo-theme-material](https://github.com/viosey/hexo-theme-material)
[hexo-theme-material-x](https://github.com/xaoxuu/hexo-theme-material-x/)

我使用的是hexo-theme-yilia，使用步骤：
1. 安装
```
cd themes
git clone https://github.com/litten/hexo-theme-yilia.git yilia
```
克隆完成之后，将`<项目根目录>\themes\material`里面的`_config.template.yml`复制一份并重命名为`_config.yml`。

2. 配置
修改`<项目根目录>\_config.yml`
```
# theme: landscape 这个是默认的主题，使用#号注释掉
theme: material
```

3. 使用`hexo generate`重新生成页面
   
4. 错误处理
这个主题的代码很久没有更新，安装的时候很可能出错。我安装的时候，报了一个错：

```
D:\code\hchaojie-blog\themes\material\layout\_widget\dnsprefetch.ejs:12
    10| <% } %>
    11|
 >> 12| <% if(theme.comment.use === "changyan") { %>
    13|     <link rel="dns-prefetch" href="https://changyan.sohu.com"/>
    14| <% } else if(theme.comment.use === "livere") { %>
    15|     <link rel="dns-prefetch" href="https://cdn-city.livere.com"/>

Cannot read property 'startsWith' of null
```
找到dnsprefetch.ejs这个文件，看了一眼21行大概是theme.comment.use为空，
只要修改`<>项目根目录\themes\material\_config.yml`，给use变量设置一个空值即可。
```
comment:
    use: ''
```


### 发布到github网站
前提：你需要有一个github账号。
基本步骤：创建一个xxx.github.io的仓库，然后把你的静态页面放到仓库的根目录就可以了。
其中xxx是你github的账号名。比如我的是`hchaojie`，那么我的仓库就是：`hchaojie.github.io`，我的个人博客访问地址就是`https://hchaojie.github.io/`

也可以使用hexo的自动发布功能：
1. 修改`<项目根目录>\_config.yml`
```
deploy:
  type: github
  repository: git@github.com:hchaojie/hchaojie.github.io.git
  branch: master
```
注意仓库路径是ssh形式的（以git@开头，而不是https@开头），并且要配置ssh key自动登陆。

2. 每次文章更新后的发布步骤：
```
hexo clean
hexo generate
hexo deploy
```