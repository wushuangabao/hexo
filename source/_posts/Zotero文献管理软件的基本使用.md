---
title: Zotero文献管理软件的基本使用
tags:
  - 技巧
  - 科研
categories:
  - 奇技淫巧
toc: false
date: 2019-06-19 19:40:59
---

## 介绍与安装

我曾经在[微信公号](http://mp.weixin.qq.com/s?__biz=MzI1MzEyODQyMA==&mid=2708133016&idx=1&sn=0ba2974967de9ebb5a168c874a6bd704&chksm=cd474f13fa30c6059a7e3a80bb213b9108c821db9825c74f289bcb3c45c86f4fde47993f2250&mpshare=1&scene=23&srcid=#rd)中比较过几款主流的文献管理工具。其中 Zotero 作为开源软件，功能扩展性极强。

<!--more-->

我一开始了解到 Zotero 是因为看了阳志平的[入门六篇](https://www.douban.com/group/topic/45562674/)，里面不但介绍了Zotero 的使用，还介绍了很多配套工具。

如何安装无须多讲，直接上官网[传送门](https://www.zotero.org/support/word_processor_plugin_installation)。

此外，使用之前还须要[注册](https://www.zotero.org/user/register/)一个账号，用来在不同设备之间同步你的文献库（另外还可以创建 group 进行团队协作，可以关注其他用户）。


## 在浏览器中使用 Zotero

选择 Zotero 菜单栏中的工具 > 安装浏览器插件，会打开浏览器（Firefox, Chrome 等）并进入插件浏览器下载页面。

打开本地的 Zotero 软件，再配合 Zotero 的浏览器插件，你可以直接从你打开的网页上爬取文献资料的信息（标题、作者、摘要等）。

阳志平入门六篇的[第一篇](https://www.yangzhiping.com/tech/zotero1.html)就介绍了6种向 Zotero 导入文献的方法。不过我最常用的还是下图这种：

![导入豆瓣图书信息](http://m.qpic.cn/psb?/V11Tp57c2B9kPO/QiV5wb1EnTqdWuKohkD7qoIDK1F6870aZjAZAje5WZY!/b/dD4BAAAAAAAA&bo=OQPfAQAAAAADB8Y!&rf=viewer_4)

**注：**最近我发现，先打开浏览器再打开 Zotero 的情况下，导入文献会出错，须要重启浏览器。另外 Zotero 无法识别万方数据库的文献元数据（可能因为网页改版了），国内的文献还是去 CNKI 搜索吧，而且在 CNKI 还能支持批量导入……

顺带一提，Chrome 浏览器拥有丰富的插件，其中我经常使用的有：
- TamperMonkey，允许用户自己编写、共享插件脚本，不少 geek 写了许多很实用的脚本，比如解析网上的 VIP 视频、免费下载百度文库等；
- 谷歌访问助手，允许浏览器直接访问 Google 主页以及 Google scholar 等网站；

（由于 QQ 浏览器使用 Chrome 内核，我个人体验是：以上插件在 QQ 浏览器基本上也可以使用。我用 QQ 主要是因为一直在用 QQ 账号同步网页收藏夹。）

## 在 Word 中使用 Zotero

在 Word 中使用 Zotero 可以方便地插入参考文献的引用。

只要你想引用的文献已经添加到 Zotero 文库中，就可以直接插入 Word 文档的任意位置，并在指定位置、按指定样式，自动生成该参考文献的信息。即使引文插入的位置有先后变化，只要刷新一下就会自动调整。

是不是很激动人心？如果你的 Windows 系统安装了 Zotero 和 Word，只需要再按下面2个步骤操作，就可以做到上述神奇的功能。

### 安装 Zotero Word 插件

如果你在你的 Word 中找不到 ZOTERO 插件，可以参考[官方文档](https://www.zotero.org/support/word_processor_plugin_manual_installation)进行手动安装。

Windows 用户的插件安装步骤如下：
1. 打开 Zotero 的安装文件夹（默认路径为`C:\Program Files (x86)\Zotero`）
2. 在安装文件夹中，依次打开`extensions\zoteroWinWordIntegration@zotero.org\install`，里面有2个文件 Zotero.dotm (用于 Word 2007 及以上版本) 以及 Zotero.dot (用于旧版本的 Word)
3. 找到 Word 的 startup 文件夹（对于 XP 以后的操作系统，路径为 `C:\Users\你的系统用户名\AppData\Roaming\Microsoft\Word\STARTUP`，如果没有 STARTUP 文件夹的话自己创建一个）
4. 复制一份 Zotero.dotm 或 Zotero.dot 文件（取决于你的 Word 版本）到 Word 的 Startup 文件夹
5. 打开或重启 Word，你会发现工具面板多了一块内容，如图：

![Word 中的 Zotero 插件](http://m.qpic.cn/psb?/V11Tp57c2B9kPO/m4xFU4yRplBvfx3zYEem*X2kLjW31lMeLq3Z0IN6fV0!/b/dDABAAAAAAAA&bo=RQJ1AAAAAAADBxA!&rf=viewer_4)

### 添加中文文献引用样式

Zotero 默认的引文格式中找不到中文标准 GB 样式，如何添加呢？

在 Word 插件的 Document Preference 中，有“添加引文样式…”字样，点它就相当于打开了 Zotero 软件的首选项设置，如下图：

![首选项-引文](https://upload-images.jianshu.io/upload_images/5727621-ada62e18161b6e5c.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/616/format/webp)

点“获取其他样式…”，然后搜索`Chinese Std GB/T 7714-2005 (numeric, Chinese)`，将它添加到样式管理器中，就可以选用中文标准的引文格式了。