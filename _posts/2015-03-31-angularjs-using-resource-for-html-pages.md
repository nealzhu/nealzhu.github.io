---
layout: post
title:  "Angularjs $resource 服务获取 HTML 页面的问题"
date:   2015-04-01 14:35:15
description: 使用 Angularjs $resource 服务获取 HTML 页面返回 JSON 格式对象的解决方案
categories:
- web development
- javascript
- angularjs
---

单页面应用越来越流行，Angularjs 的实践也是越来越火热，这不，公司的后台项目也用上了它。最近在用 $resource 服务的时候遇上了一点小问题，可能也未必算一个问题，但是分享一下还是可以的。

### $resource 是个神马？

$resource 是 AngularJS 中应对 RESTful 风格接口的一个服务，它底层使用了 $http 服务，然后封装了简易的上层接口使我们可以非常简单的和服务端的数据做交互。官方 API 在此：[$resource API][link0]（要翻墙别说我没告诉你哟）。因为没有拼接在 angularjs 本体文件中，所以用的时候需要再引入一个 resource 模块的 js 文件。

### 遇到什么问题了？

有个需求，后台可以上传一些页面模板，然后服务端可以把数据输出到模板中生成静态 HTML 文件，然后用户可以在后台预览生成的页面。服务端的接口直接输出的就是 text/html 类型的页面数据，一开始我以为是查看上传模板的代码，于是用 $resource 获取接口数据后直接展示在页面上，可是显示出来的数据却是一个对象：

    {"0":"<","1":"!","2":"D","3":"O","4":"C","5":"T","6":"Y","7":"P","8":"E","9":" ","10":"h","11":"t","12":"m","13":"l","14":">","15......此处省略 N 字节

太奇怪了！第一反应是难道接口有问题？立马被推翻，既然可以预览，那么肯定不是啥 JSON 对象之类。翻了下 network 面板（Chrome 调试），Response Headers 里 Content-Type 也是 text/html 没错啊，那这是个什么鬼？！

看来只有找文档问一问了。文档告诉我，transformResponse 和 responseType 这俩参数可能有戏。不过 responseType 是正确的，那么出路应当在 transformResponse 身上了。看了看 transformResponse 的介绍，它可以在响应到来后对响应体和响应头做些文章；默认情况下只会检测返回值是不是类似 JSON 的字符串，是的话立刻用 `angular.fromJson` 把它转换了返回给你；如果不需要，可以设置为 `[]` 空数组来阻止默认行为。

果然有戏！那就设置为空数组吧！升职加薪当上 CEO 迎娶白富美指日可待！

十秒后…… (／‵Д′)╯︵┴─┴页面上这是什么啊？！返回值为毛还是 JSON 啊？！transformResponse 你干活了吗？！……冷静，我需要冷静。一个声音告诉我：google 一下~好的。stackoverflow 上倒是有两个类似问题，可是回答不对。又一个声音告诉我：查下 github 上的 issue~好的。issue 好多，关于 $resource 的却没有说到这个问题。又一个声音告诉我：在 transformResponse 函数体里 `console.log` 一下~于是我试了试，这里的 data 是正常数据，一个安静美丽的字符串。看来问题不在 transformResponse 上啊。翻源码吧，我对自己说。

看了看底层 $http，是没问题的，那么就出在 $resource 自己身上了。果然，在文件底部找到了“猫腻”：

```javascript
    if (action.isArray) {
        value.length = 0;
        forEach(data, function(item) {
            if (typeof item === "object") {
                value.push(new Resource(item));
            } else {
                // Valid JSON values may be string literals, and these should not be converted
                // into objects. These items will not have access to the Resource prototype
                // methods, but unfortunately there
                value.push(item);
            }
        });
    } else {
        shallowClearAndCopy(data, value);
        value.$promise = promise;
    }
```

问题就出在 `shallowClearAndCopy` 这个函数身上。因为在 isArray 为 false 的情况下，$recourse 最终返回的是单个 resource 对象，所以最后会调用 `shallowClearAndCopy` 将服务端请求来的数据拷贝至这个 resource 对象身上，而请求的页面作为字符串—既不是对象也不是类似 JSON 的字符串—在 for..in 的循环中，被一个字节一个字节地拷贝到了 resource 对象身上，于是乎在页面上直接输出就成了之前的鬼样子。

知道问题所在，解决就不难了。只要在 transformResponse 函数中返回一个对象，把字符串作为其属性值保存，在业务代码的成功回调中取得该属性值在页面输出即可。

事后发现，需求搞错了啊，555~是预览不是查看模板代码，也是，不然为毛直接输出静态页面呢？看来是着急不仔细了，不过就接口来说可以做的扩展性更好一些（或者说就应保持同一风格的返回值，全部 JSON），返回值还是一个 JSON 字符串，内含模板代码以及预览的 URL，相对直接返回该静态页面来说，对今后新需求的接入会更方便一些。

本文纯属个人经验与探索，如有不足指出，欢迎指正，谢谢阅读。

[link0]: https://docs.angularjs.org/api/ngResource/service/$resource
[img0]: http://zqlabimg.b0.upaiyun.com/2014/12/github-pages_generator_1.png