# PicBed
picture bed

个人图床


> 本文由 [简悦 SimpRead](http://ksria.com/simpread/) 转码， 原文地址 [cloud.tencent.com](https://cloud.tencent.com/developer/article/1651601)

> 经常写 Markdown 或者博客的同学，肯定都要用到图床。图床是什么呢？其实相当于一个存储图片的网站，类似百度云这样，不过上传图片到图床后可以直接通过外链进行访问...

**前言**
------

经常写 Markdown 或者博客的同学，肯定都要用到图床。图床是什么呢？其实相当于一个存储图片的网站，类似百度云这样，不过上传图片到图床后可以直接通过外链进行访问。

比如把本地一张`a.jpg`上传到图床后，便可以拿到一个链接：

https://www.xxx.com/img/a.jpg

然后点击这个链接就可以访问图片 a 了。今天来聊聊怎么搭建可靠的图床吧~

为什么会产生这个需求呢？因为小编经常写博文什么的，现在的做法是在简书上上传图片，然后把生成的图片链接放到 Markdown 文档上面，写好文档以后就可以批量复制到各大博客平台投稿了。

![](https://ask.qcloudimg.com/http-save/7256485/72pdiug98d.gif)

但是这样有个隐患：万一简书哪天挂掉了，那么我放到 csdn、cnbolgs 等这些平台的文章图片都会挂掉。即使简书一直维持现状，但万一哪天它不高兴了，做了个外链防盗（图片外链只能在本站显示），那同样会遇到上面的问题。

比如小编之前放在简书上的文章，复制到 csdn 上后。不知道怎么回事：

![](https://ask.qcloudimg.com/http-save/7256485/798yx66e0b.png?imageView2/2/w/2560/h/7000)

说多了都是泪。因此，趁早做好准备，免得以后出现问题就麻烦了。毕竟有些博客的图片只是随手一截，还真找不到备份……

**平台选择**
--------

现在也有蛮多的图床平台可以选择，常见的有 SM.MS 图床、腾讯云 [COS](https://cloud.tencent.com/product/cos?from=10680)、微博图床、GitHub 图床、七牛图床、Imgur 图床、阿里云 OSS、又拍云图床等。

![](https://ask.qcloudimg.com/http-save/7256485/5bn5pl2i8v.png?imageView2/2/w/2560/h/7000)

而这里边，SM.MS 和 Imgur 有免费版也有收费版，腾讯云、七牛、阿里云、又拍云都是收费的，微博图床据说已经挂了。其他小站的就不推荐了，因为指不定哪天就挂了。

那么，也就剩下 GitHub 安全、免费又可靠了（微软爸爸护着呢！）。现在微软接管了 GitHub 以后，貌似公有仓库已经不限个数了，而且单个仓库容量已经放宽至 2GB。这绝对够用了，不够就再建一个共有仓呗。最重要的还是免费，配合 [CDN](https://cloud.tencent.com/product/cdn?from=10680) 加速，访问也不成问题。嗯，就微软爸爸了！

![](https://ask.qcloudimg.com/http-save/7256485/ewfy0uqpbu.png?imageView2/2/w/2560/h/7000)

**工具选择**
--------

选择一个本地的上传工具是为了方便我们快速上传图片，获得图片外链。这里首选 picgo。

![](https://ask.qcloudimg.com/http-save/7256485/07j9osotku.png)

介绍和下载地址：

https://github.com/Molunerfinn/PicGo

这款小工具非常强大，其中最赞的就是那个剪切板图片上传功能，在 QQ 或者微信截图截好图片后，可以点击`剪切板图片上传`或者通过快捷键，它就会把当前剪切板中的图片上传到配置到图床中。可以看到上传所有图片，点击即可复制需要的图片外链格式：

![](https://ask.qcloudimg.com/http-save/7256485/s084cx5uu5.png)

![](https://ask.qcloudimg.com/http-save/7256485/8qcot7kd0z.png)

准备完毕就可以着手配置了。先去 GitHub，没有账号的先注册一个账号。

![](https://ask.qcloudimg.com/http-save/7256485/wrch3u36l2.jpeg)

**GitHub 配置**
-------------

**1. 创建 Repository**

鼠标移动到右上角，点击 "New repository" 按钮：

![](https://ask.qcloudimg.com/http-save/7256485/nfk43ixsyz.png)

填写相关信息，创建一个存储图片的仓库：

![](https://ask.qcloudimg.com/http-save/7256485/xz7qwsrhw9.png)

**2. 配置 token key**

生成一个 Token 用于操作 GitHub repository。回到主页，点击 "Settings" 按钮：

![](https://ask.qcloudimg.com/http-save/7256485/cpi8fqcjug.png)

进入页面后，点击 "Developer settings" 按钮

![](https://ask.qcloudimg.com/http-save/7256485/o92o341d9c.png)

点击 "Personal access tokens" 按钮，然后点击 Generate token：

![](https://ask.qcloudimg.com/http-save/7256485/3be5yg63m2.png)

填写描述，选择 "repo" 权限，然后拉到下面点击 "Generate token" 按钮

![](https://ask.qcloudimg.com/http-save/7256485/oqum3azudz.png)

![](https://ask.qcloudimg.com/http-save/7256485/vhmabvnt4k.png)

> 注意：创建成功后，会生成一串 token，这串 token 之后不会再显示，所以第一次看到的时候，可以用个小本本保存起来哦，忘记了只有重新生成，每次都不一样。

**Picgo 配置**
------------

拿到了刚刚记录了 GitHub token 后，打开 picgo，按照如下设置即可：

![](https://ask.qcloudimg.com/http-save/7256485/zi7uj8uzth.png)

`设定仓库名`按照 “账户名 / 仓库名的格式填写”，比如我的是：dengfaheng/image01。

`分支名`统一填写 “master”。

`设定Token`将之前的 Token 粘贴在这里。

`指定存储路径`留空。

`自定义域名`的作用是在上传图片后成功后，PicGo 会将 “自定义域名 + 上传的图片名” 生成的访问链接，放到剪切板上。默认留空也可以正常使用。这里为了使用 CDN 加快图片的访问速度，自定义域名我们按照这样去填写：

https://cdn.jsdelivr.net/gh/GitHub 用户名 / 仓库名

比如我的是：

https://cdn.jsdelivr.net/gh/dengfaheng/image01

点击确定后就配置完成了，我们截图后点击`剪切板图片上传`，如果上传成功，拿到的外链放到 Markdown 中正常访问，就 OK 啦。

![](https://ask.qcloudimg.com/http-save/7256485/2tly00713s.png)

![](https://ask.qcloudimg.com/http-save/7256485/zdsl3olpjn.png)

当然快捷上传的快捷键也可以到设置中进行配置。可以看到 GitHub 仓库中多了很多我们上传的图片。

![](https://ask.qcloudimg.com/http-save/7256485/bl5ux0qaql.png)

也可以在 picgo 中对上传的图片进行相关操作，不过这里的删除只是删除 picgo 中的图片而言，GitHub 上的不会删除哦。

![](https://ask.qcloudimg.com/http-save/7256485/8a7r0x68tb.png)

至于如何删除 GitHub 上的图片，emmm…… 那说来就话长了。。就不说了。大家还是不要删了，空间不够了再开个仓库即可。
