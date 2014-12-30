---
layout: post
title:  "Github Pages 快速搭建个人博客指南"
date:   2014-12-30 16:24:30
description: 本文记录了快速使用 Github pages 搭建个人博客的方法，也是本博客搭建的过程，不涉及复杂的优化或者自定义内容。
categories:
- blog
- Github
- jekyll
---

本文记录了快速使用 Github Pages 搭建个人博客的方法，也是本博客搭建的过程，不涉及复杂的优化或者自定义内容。这些是以后慢慢学习的内容。

### Github Pages 简介

Github 为用户提供了 [Github Pages][link0] 服务，可直接通过 Github 仓库来建立/管理用户的个人网站或者是项目主页。是的，它是免费的，而且生成的是静态页面。虽有一定的局限，但是对于需要一个可以记录、发布自己想法的人来说，已经够用了。

### 如何搭建个人博客

Wow，很简单，首先你需要有一个 Github 账号，接下来，在账号下建立一个仓库，名称是你的用户名加上 `.github.io`，例如：username.github.io。

然后……静待 20 分钟左右，大功告成！不信？请在仓库下新建一个 index.html 或者别的什么都行，同步到 Github 之后访问 username.github.io/index.html……你会看到你想要的一切。

此时此刻您可能正在腹诽，就这么简陋吗？难道要我自己搞定整个静态站点？！当然不可能，Github 支持了一个“好玩意”。

#### “好玩意”

吾名 jekyll，吾乃静态网站生成器，吾有特定目录结构，通过吾的配置文件可以对汝的网站进行多样的设置，什么主题、固定链接不在话下。使用吾需要 Ruby、RubyGems、NodeJS（或其他 Javascript 运行环境）的支持，不过 Github 都已准备妥帖。

于是乎，在 username.github.io 仓库下，参照 jekyll 官方说明，建立相应的目录和文件并进行一系列的配置，就可以实现一个有自己主题、便于管理、发布内容方便的网站了，接下来你就可以专注于内容而非其他的“俗物”了。

不过本文为了突出一个快字，当然不会让大家花时间在研究 jekyll 上了，这里有两种方法快速搭建符合 jekyll 结构的网站。

第一种请看下图，在 Github 网站上打开博客仓库 username.github.io，在 setting 中找到自动生成器，点击后 Github 就会以一个向导的形式为你创建出合格的站点了，当然还有一些默认的主题供予选择，真是贴心到爆……

![自动生成步骤一][img0]

![自动生成步骤二][img1]

第二种是直接上这：[http://jekyllthemes.org/][link1]，这里全是基于 jekyll 的各种主题，完全符合 jekyll 的接口，随便选一款下载下来，拷贝进自己的仓库里，把 `_config.yml` 里一些基本设置修修改改后同步到 Github 上，网站也就好啦，之后要汉化要改进就慢慢修改吧。

### 如何发布

所有的文章都在 `_posts` 文件夹中。这些文件可以用 [Markdown][link2] 编写，也可以用 [Textile][link3] 格式编写。只要文件中有 YAML 头信息，它们就会从源格式转化成 HTML 页面，从而成为你的静态网站的一部分。

发表一篇新文章，你所需要做的就是在 `_posts` 文件夹中创建一个新的文件。文件名的命名非常重要。Jekyll 要求一篇文章的文件名遵循下面的格式：

    年-月-日-标题.xxx

例如：

    2014-12-29-新年将至.md
    2014-12-30-new-years-eve-is-awesome.md
    2014-12-31-how-to-write-a-blog.textile

看不懂这些是什么东西？没关系，当你以前面两种方式建立好网站后，`_posts` 文件夹中应该有一些示例文章，参考一下吧。

### 自定义域名

通过以上种种建好的网站，默认的访问地址就是 username.github.io。作为无欲无求的你来说应该就到此为止了，不过 Github 也不是这么强求的网站，你也可以使用自定义的域名来访问它，甚至基于某些配置你还可以享受 Github 提供的 CDN 服务。

Github 提供了这几种方式供你设置自定义域名：

1. 顶级域名，A 记录设置到 192.30.252.153 以及 192.30.252.154 这两个 IP 上
2. 子域名，设置 CNAME 记录到 username.github.io
3. 顶级域名，设置 ALIAS 或者 ANAME 记录到 username.github.io

其中2、3两种方式可以享受到 Github 提供的 CDN 服务，国内 ping 一般延迟在 150ms 以内。方法3中提到的两种记录，可惜目前国内没有发现支持的 DNS 服务商，国外倒是有个把付费的提供商分别支持其中的一种（例如 [dnsimple][link4] 和 [dnsmadeeasy][link5]）。

DNS 设置好后，不要忘记一件事情，在 username.github.io 仓库下新建一个 CNAME 文件，没有后缀名，只有一行内容，就是准备解析过来的域名，例如：

    www.zqlab.org

同步好后你就能在上一节自动生成器上方看到你发布的网站访问地址了。

### 参考网站

[Github 官方指南][link6]
[jekyll 官网][link7]
[jekyll 中文网][link8]


[link0]: https://pages.github.com/
[link1]: http://jekyllthemes.org/
[link2]: http://wowubuntu.com/markdown/
[link3]: http://qt-project.org/wiki/TextileSyntax_SimplifiedChinese
[link4]: https://dnsimple.com/
[link5]: http://www.dnsmadeeasy.com/
[link6]: https://help.github.com/categories/github-pages-basics/
[link7]: http://jekyllrb.com/
[link8]: http://jekyllcn.com/

[img0]: http://zqlabimg.b0.upaiyun.com/2014/12/github-pages_generator_1.png
[img1]: http://zqlabimg.b0.upaiyun.com/2014/12/github-pages_generator_2.png