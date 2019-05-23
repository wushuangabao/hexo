---
title: 利用GitHub和Hexo免费搭建个人博客
tags:
  - 技巧
  - 产出
categories:
  - 奇技淫巧
toc: false
date: 2019-05-16 22:50:28
---

## 1. 使用GitHub

GitHub可以简单看成是一个存放文件（主要是源代码文件）的云端仓库，它的核心功能是基于Git进行开发项目的版本管理，它允许你和你的团队随时随地 从仓库拉取、或者向仓库上传 代码文件。

### 1.1. 注册GitHub账号

登录[GitHub网址](https://github.com/)进行在线注册。（注册时，图片验证码可能要翻墙才能获取）

### 1.2. 创建GitHub Pages

Github Pages是GitHub的公共静态页面搭建托管服务。有了它，静态网站可以被免费托管在Github上。

你可以选择使用Github Pages默认提供的域名github.io或者自定义域名来发布站点。比如我的就是[wushuangabao.github.io](https://wushuangabao.github.io/)。

如何创建？[参考](https://www.cnblogs.com/lijiayi/p/githubpages.html)

GitHub Pages也是基于一个GitHub文件仓库，这个仓库里的文件可以随意新建、修改和删除。仓库中名为index.html的文件会作为自己站点的首页，也就是说只要懂得HTML语法，就能编写属于自己的页面了。

### 1.3. GitHub Desktop

习惯于使用Windows操作系统的用户，像我，一般是不习惯使用命令行来操作Git的，那就推荐下载[GitHub Desktop](https://desktop.github.com/)工具。用它点点鼠标就可以完成代码的上传（push）和拉取（pull）。

## 2. 使用Hexo

其实有了GitHub Pages，再手写网页文件，自己的网页就能够被人们访问了，那为什么还要用Hexo呢？

因为Hexo可以提供博客（blog）的模板，以后发表一篇博客只须创建一个Markdown文本文件，就可以用Hexo构建整个网站（博客型的）。

### 2.1. 安装Hexo

装[Hexo](https://hexo.io/zh-cn/)前提是要有[node.js](https://nodejs.org/en/)和[Git](https://git-scm.com/)环境，没有的话自行安装。（Git貌似也是使用GitHub应该具备的环境。）

然后根据[Hexo官方文档](https://hexo.io/zh-cn/docs/)进行安装即可。

### 2.2. 部署博客

根据[文档](https://hexo.io/zh-cn/docs/commands)提供的指令，我们可以通过Windows命令行（cmd）操作Hexo。反正有文档，如何**写作、生成、部署、换主题**，我就不写了。

### 2.3. Hexo-Client

不过还是觉得用命令行太麻烦，我搜了一下GitHub，果然找到了一款用[Electron](https://electronjs.org/)写的Windows桌面工具，叫[Hexo-Client](https://github.com/gaoyoubo/hexo-client/releases)。下载exe文件，安装。

依照Hexo-Client作者写的[帮助文档](https://www.mspring.org/2018/11/29/HexoClient%E4%BD%BF%E7%94%A8%E5%B8%AE%E5%8A%A9/)（这文档本身就属于一个GitHub Pages + Hexo构建的博客）配置完毕，以后发布博客就用它了。

## 3. 使用七牛云

由于MarkDown文件里插入图片用的是图片的URL链接，所以我们创建的这个博客需要有配套的云图片库。

而[七牛云](https://www.qiniu.com/)为个人用户提供免费的10G云空间，当然也可以作为图片库，[参考这里](https://www.jianshu.com/p/6a118a49ab1f)。（之前我是把GitHub Pages作为云图片库，然而现在改成Hexo构建博客，原来的图片不能继续放着了。）

## 4. 更换成NexT主题

Hexo生成的博客，默认的主题风格不好看怎么办？

我们可以换，也可以自己写自定义风格。据说用得最多的主题是NexT，可以尝试换一下。

- Hexo及其主题介绍，[别人写的个人博客之旅](https://blog.csdn.net/u011475210/article/details/79023429#jekyll-or-hexo)
- 简述上的，NexT主题的[常用配置参考](https://www.jianshu.com/p/3a05351a37dc)