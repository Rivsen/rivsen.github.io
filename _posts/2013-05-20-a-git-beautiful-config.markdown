---
layout: post
title:  "git的两个漂亮的设置"
date:   2013-05-20 03:27:00
description: "git的两个漂亮的设置"
permalink: post/a-git-beautiful-config
disqus:
  id: a-git-beautiful-config
categories:
- blog
- git
---

一个是当你执行git命令的时候显示成彩色，linux中默认是没有的

```text
[color]
ui = true
```

还有一个是查看历史提交的线性关系，终端的哦，不是gui里面的～这条是一个朋友给我的

```text
[alias]
xtree = log –graph –pretty=format:'%C(yellow)%h%C(cyan)%d%Creset %Cgreen%an%Creset: %s %Cblue(%ar)%Creset'
```
