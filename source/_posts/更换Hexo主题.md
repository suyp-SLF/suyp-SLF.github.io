---
title: 更换Hexo主题
date: 2026-01-12 20:59:59
tags:
 -Hexo
 -Blog
---
## 更换主题可以直接采用Hexo官方提供的主题，也可以自己制作主题，这里主要介绍如何更换主题。

### 1. 下载主题

在Hexo官网的主题页面，找到你喜欢的主题，点击进入主题页面，然后点击下载按钮，下载主题压缩包。


### 2. 解压主题
将下载的主题压缩包解压到Hexo博客的`themes`文件夹下。
重要：这里的文件名需要修改为主题的名称，例如`fluid`。
![view](/images/posts/更换Hexo主题/1.png)


### 3. 修改配置文件

打开Hexo博客的配置文件`_config.yml`，找到`theme`字段，将其修改为你要使用的主题名称。
![view](/images/posts/更换Hexo主题/2.png)
![view](/images/posts/更换Hexo主题/3.png)

### 4. 重新启动Hexo服务器


