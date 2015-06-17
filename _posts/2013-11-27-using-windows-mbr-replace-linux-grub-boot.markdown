---
layout: post
title:  "“修复mbr引导”（其实我想说，如何干掉grub引导然后使用Windows引导。。。"
date:   2013-11-27 03:29:25
description: "“修复mbr引导”（其实我想说，如何干掉grub引导然后使用Windows引导。。。"
permalink: post/using-windows-mbr-replace-linux-grub-boot
disqus:
  id: using-windows-mbr-replace-linux-grub-boot
categories:
- blog
---

找一张win7的安装盘，启动之，到启动完成，显示安装界面的时候（当然咱们不需要安装啦），按shift+f10，会调出来一个cmd，输入 bootrec /fixmbr 然后回车，mbr又“活灵活现”的出现在你的电脑上了～
