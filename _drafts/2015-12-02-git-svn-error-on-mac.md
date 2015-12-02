---
layout: post
title:  "OSX EI Caption 下 git svn 缺少 core.pm 的一种解决方法"
date:   2015-12-02 16:58:15
description: OSX EI Caption 下 git svn 缺少 core.pm 的一种解决方法
categories:
- OSX
- git
- svn
---

1、安装了command line tools，自带和dmg安装的git都缺少，常规方案（ln -s）没有用，因为ei caption禁止了命令行对部分系统文件的权限；
2、卸载掉dmg的git，用brew安装git以及subversion，问题解决。

官方 API 在此：[$resource API][link0]（要翻墙别说我没告诉你哟）。

[link0]: https://docs.angularjs.org/api/ngResource/service/$resource