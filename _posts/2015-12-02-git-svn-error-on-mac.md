---
layout: post
title:  "OSX EI Capitan 下 git svn 缺少 Core.pm 的一种解决方法"
date:   2015-12-02 16:58:15
description: OSX EI Capitan 下 git svn 缺少 Core.pm 的一种解决方法
categories:
- OSX
- git
- svn
---

最近由于种种原因需要远程办公，公司是 svn 管理项目，但是家里的 Mac 因为没有什么好的 svn 的 GUI 管理软件，所以打算用 git 替代，因为有 `git svn` 这种命令的存在，所以应该不是问题。

找了一个 svn 公共库用于测试，`git svn clone xxx` 敲上去却报了错：

    Can't locate SVN/Core.pm in @INC (@INC contains: /usr/local/Cellar/git/2.6.2/lib...)

怎么回事？没太看懂，先搜索一下解决方案。谷歌大神果然没有让人失望，从 OSX Mavericks 开始就有人提这个问题，那时候倒还好解决，安装完整 xCode 或者 command line tools 之后把 Perl 库里的几个目录做下软链就可以解决了，具体只要搜索报错的核心提示就可以得到这些回答了。

但是在我这不起作用！为啥？因为升级了 OSX EI Capitan！执行软链命令的时候被告知：“Operation not permitted”。一查之下，原来 EI Capitan 禁止了部分系统目录的修改权限，`sudo` 也不管用了，至少手动在命令行里执行是不管用了。

因为我的 git 没用系统自带而是通过 dmg 方式安装的，结果悲催的发现不仅两个版本的 git 都缺少 Core.pm，而且我一直都在用 /usr/bin/git 这个自带的玩意……于是卸载了 dmg 的 git，用 brew 安装省点事吧。此时突然想到，既然是跟 svn 相关，也许安装 subversion 会起到一定的帮助，再用 brew 安装 subversion 之后执行 `git svn clone xxx`，成功了。

又可以继续愉快的玩耍了，需要 GUI 管理工具的话，sourcetree 吧，支持 Windows/OSX 双平台，开源，免费！