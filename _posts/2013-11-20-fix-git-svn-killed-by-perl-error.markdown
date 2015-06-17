---
layout: post
title:  "git-svn遇到perl被kill掉"
date:   2013-11-20 06:07:15
description: "git-svn遇到perl被kill掉"
permalink: post/fix-git-svn-killed-by-perl-error
disqus:
  id: fix-git-svn-killed-by-perl-error
categories:
- blog
- git
---

在和同事一起开发一个svn管理的项目的时候，发现svn1.8和git1.8两个版本一起用会报错，无法使用：

```text
Error in `/usr/bin/perl’: double free or corruption (!prev): 0x00000000026fcd80
```

然后在网上找到了解决办法：
找到 /usr/share/perl5/vendor_perl/Git/SVN/Ra.pm
然后在 sub _auth_providers () { 上面添加几行： 

```perl
    END {
        $RA = undef;
    }
```

问题解决。
