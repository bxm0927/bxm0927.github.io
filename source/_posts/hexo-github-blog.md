---
title: 可能是最详细的 Hexo + GitHub Pages 搭建个人博客的教程
date: 2018-06-24 22:00:37
tags: [hexo, github, blog, 搭建个人博客]
categories: [Skills]
---

## 前言

> 个人博客地址：www.lovebxm.com
> 如果您在搭建的过程中遇到无法解决的问题，可与我交流探讨，QQ：80583600

我们为什么要写博客？一方面写博客可以用来监督自己，阶段性总结归纳所学的知识点，进而达到学以致用的目的；另一方面在于分享精神，在分享与交流的过程中，你的语言表达能力会逐渐变得简洁清晰，而且带有很强的逻辑性。

<!-- more -->

所以说，写博客是一个好习惯，希望大家能够坚持下来。最近刚好有空，就把自己个人博客搭建的全过程记录下来，希望能够帮助到一些朋友。当然了，搭建个人博客的方式有很多，比如自己写前后端、WordPress、Jekyll等等，这里我是用的是免费开源、简洁高效的“Hexo + GitHub Pages”来搭建。


大概流程如下：


1. 搭建 Node.js 环境
2. 搭建 Git 环境
3. GitHub 注册和配置
4. Hexo 安装
5. Hexo 配置
6. 关联 Hexo 与 GitHub
7. 将 GitHub Pages 地址解析到个人域名
8. Hexo 常规操作
9. 结束语


![image](http://img.imooc.com/568b80410001975704500260.jpg)



## 搭建 Node.js 环境

> 为什么要搭建 Node.js 环境？ ---- 因为 Hexo 博客系统是基于 Node.js 编写的，要运行在 Node.js 环境之上。

Node.js 是一个基于 Chrome V8 引擎的 JavaScript 运行环境，可以在非浏览器环境下，解释运行 JS 代码。

Node.js 的包管理器 npm（node package manager），是全球最大的开源库生态系统，安装 Node.js 时会自动安装 npm。我们之后要通过 npm 来安装 Hexo。

在 Node.js 官网：https://nodejs.org/en/ 下载最新稳定版，安装时保持默认设置即可，一路Next，安装很快就结束了。


最后，打开命令提示符，输入 `node -v`、`npm -v`，出现版本号则说明 Node.js 环境配置成功，第一步完成！！！

![](http://oph264zoo.bkt.clouddn.com/17-5-28/25738843.jpg)


## 搭建 Git 环境

> 为什么要搭建 Git 环境？ ---- 因为需要把本地的网页和文章等资源提交到远程Git仓库（GitHub）上。

Git 是一款免费、开源的分布式版本控制系统，用于敏捷高效地处理任何或小或大的项目。

在 Git 官网：https://git-scm.com/ 下载安装包，安装过程如下：

![](http://oph264zoo.bkt.clouddn.com/17-5-28/62582265.jpg)

桌面右键，打开 `Git Bush Here`，输入 `git --version`，出现版本号则说明 Git 环境配置成功，第二步完成！！！

![](http://oph264zoo.bkt.clouddn.com/17-5-28/13485948.jpg)


## GitHub 注册和配置

GitHub 是什么？GitHub 又名 GayHub，全球最大的同性交友网站。

（哈哈哈，开个玩笑 `^_^`）实际上，GitHub 是一个代码托管平台，因为只支持 Git 作为唯一的版本库格式进行托管，故名 GitHub。我们的博客最终是托管在 GitHub 上的。

Github Pages 是面向用户、组织和项目开放的公共静态页面搭建托管服务，站点可以被免费托管在 Github 上，你可以选择使用 Github Pages 提供的域名 github.io 或者自定义域名来发布站点。

首先，我们需要在 Github 官网进行注册：https://github.com

![image](http://img.mukewang.com/57eea87a0001c62213450582.png)

然后，创建仓库：Repository name 使用自己的用户名，每个用户只能建立一个，仓库名规则（注意：yourname 必须是你的用户名）：

```
yourname/yourname.github.io
```

比如我的仓库是这样创建的：

![](http://oph264zoo.bkt.clouddn.com/17-5-28/42622869.jpg)



最后，访问 https://yourname.github.io，比如我的是 https://bxm0927.github.io，如果可以正常访问，那么 Github 的配置已经结束了，第三步完成！！！

至此，搭建 Hexo 个人博客的前置环境配置已经完成，下面开始讲解 Hexo 搭建个人博客的核心操作。


## Hexo 安装

什么是 Hexo？Hexo 是一个快速、简洁且高效的博客框架，默认使用 Markdown 解析文章，在几秒内，即可利用靓丽的主题生成静态网页，只需一条指令即可部署到 GitHub Pages 或其他网站。

强烈建议读者花20分钟去读一读 Hexo 的官方文档：https://hexo.io/zh-cn/

![](http://oph264zoo.bkt.clouddn.com/17-5-28/49821520.jpg)


使用 npm 安装 Hexo（在这之前，请确保你已经成功安装了Node.js和Git）：

```
npm install hexo-cli -g
```

然后你将会看到下图所示信息，可能你会看到一个`WARN`，但是不用担心，这不会影响你的正常使用。

![](http://oph264zoo.bkt.clouddn.com/17-5-28/41219383.jpg)


查看Hexo的版本，`hexo version`，正确输出如下信息则代表 Hexo 安装成功。

![](http://oph264zoo.bkt.clouddn.com/17-5-29/81453389.jpg)




## Hexo 配置

安装 Hexo 完成后，执行下列命令来初始化 Hexo，用户名改成你自己的（我的是bxm0927），Hexo 将会在指定文件夹中新建博客系统所需要的文件。

```
hexo init bxm0927.github.io

cd bxm0927.github.io

npm install
```

新建完成后，目录如下（不同的版本可能目录有些不一样）：

```bash
.
├── .deploy         # 需要部署的文件
├── node_modules    # Hexo依赖
├── public          # 生成的静态网页文件
├── scaffolds       # 模板，当您新建文章时，Hexo 会根据 scaffold 来建立文件。
├── source          # 博客正文和其他源文件，404、favicon、CNAME 都应该放在这里
| ├── _drafts       #   草稿
| └── _posts        #   文章
├── themes          # 主题，Hexo 会根据主题来生成各种各样的静态页面。
├── _config.yml     # 全局配置文件，您可以在此配置大部分的参数。
└── package.json    # npm 依赖配置文件
```

![](http://oph264zoo.bkt.clouddn.com/18-6-24/88056415.jpg)


然后，在本地运行 Hexo：`hexo server`或`hexo s`


您的网站会在 http://localhost:4000 下启动，如果能够正常访问（如下图），则说明 Hexo 博客系统已经搭建起来了，但是目前只是在本地哦，别人是看不到的。

在下面步骤中，我们要把这个本地博客系统部署到 Github 上，让外网的人能够访问到你的博客。

![](http://oph264zoo.bkt.clouddn.com/17-5-28/34921926.jpg)


> 常见问题：执行hexo server提示找不到该指令\
> 解决办法：在 Hexo 3.0 后server被单独出来了，需要安装server，安装的命令如下：npm install hexo -server --save


## 关联 Hexo 与 GitHub

> 这一步对于不熟悉GitHub的朋友来说可能有些繁琐，慢慢来，不要着急~

### 配置 SSH

1. 生成 SSH

首先，使用SSH让本地Git项目与远程的GitHub建立起连接，方便传输文件。执行下面命令生成SSH公私钥，一路回车（记得输入你自己的邮箱地址哦~）：

```
ssh-keygen -t rsa -C "80583600@qq.com"
```


2. 添加 SSH Key 到 GitHub

用记事本打开 `C:\Users\bxm09\.ssh\id_rsa.pub`，此文件里面内容为刚才生成的公公钥，准确的复制这个文件的内容，粘贴到 https://github.com/settings/ssh 的“new SSH key”中

3. 测试

输入下面的命令，看看SSH是否配置成功，`git@github.com`的部分不要修改：

```
ssh -T git@github.com
```

如果是下面的反馈：


```
The authenticity of host 'github.com (207.97.227.239)' can't be established.
RSA key fingerprint is 16:27:ac:a5:76:28:2d:36:63:1b:56:4d:eb:df:a6:48.
Are you sure you want to continue connecting (yes/no)?
```


不要紧张，输入yes就好，然后会看到：

```
Hi aierui! You've successfully authenticated, but GitHub does not provide shell access.
```

### 配置 Git 个人信息

现在你已经可以通过 SSH 链接到 GitHub 了，还有一些个人信息需要完善的。
Git 会根据用户的名字和邮箱来记录提交。GitHub 也是用这些信息来做权限的处理。

输入下面的代码进行个人信息的设置（把名称和邮箱替换成你自己的）：

```
git config --global user.name "bxm0927"
git config --global user.email "80583600@qq.com"
```

### 配置 Deployment

在`_config.yml`文件中，找到`Deployment`，然后按照如下修改：

> 注意：1. 用户名改成你自己的，2. 冒号后面记得空一格

```
# Deployment
## Docs: https://hexo.io/docs/deployment.html
deploy:
  type: git
  repo: git@github.com:bxm0927/bxm0927.github.io.git
  branch: master
```

### 将本地文件提交到 GitHub Pages

> 来了来了来了~


```bash
# 本地预览
hexo s

# 删除旧的静态文件，即 public 文件
hexo clean

# 生成新的静态文件，即 public 文件（或者 hexo g）
hexo generate

# 部署到远程站点，即 GitHub（或者 hexo d）
hexo deploy

# 当然你也可以执行下面的组合命令
hexo d -g
```



在浏览器中输入 https://bxm0927.github.io （用户名改成你自己的），可以正常访问则说明 Hexo 与 GitHub 已经成功关联了，哇哇哇哇哇哇，开心死你了，不要忘了回来给我点赞哟 ~


### 常见问题

1. 若上面操作失败，则需要提前安装一个扩展：

```
npm install hexo-deployer-git --save
```

2. 如果在执行 `hexo d` 后,出现 `error deployer not found:github` 的错误（如下），则是因为没有设置好 public key 所致，重新详细设置即可。


```
Permission denied (publickey).
fatal: Could not read from remote repository.
Please make sure you have the correct access rights
and the repository exists.
```


3. 怎么避免 Markdown 文件（.md）被解析？

Hexo 原理就是在执行`hexo generate`时会在本地先把博客生成的一套静态html页面，然后放到public文件夹中，再执行`hexo deploy`时将其复制到.deploy文件夹中。

Github 通常建议同时附上README.md项目说明文档，但是 hexo 默认情况下会把所有 Markdown 文件（.md）文件解析成html文件，所以即使你在线生成了 README. md，它也会在你下一次部署时被删去。

那么怎么解决呢？

在执行`hexo deploy`前把在本地写好的README.md文件复制到.deploy文件夹中，再去执行`hexo deploy`。



## 将 GitHub Pages 地址解析到个人域名

> 其实当上一步结束之后，我们的博客也基本上完成了，能发博文，别人也能够通过外网域名访问到你的站点，但是~


看着博客的域名是GitHub的二级域名，总有一种寄人篱下的感觉，为了让这个小窝看起来更加正式，我们可以花几十块钱去买一个自己的域名，然后将其绑定自己的域名上，进行该绑定过程，其实就是一个重定向的过程。这里我使用的是阿里云的万网域名服务：



在 GitHub 仓库的根目录下建立一个 `CNAME` 的文本文件(注意：没有扩展名)，文件里面只能输入一个你的域名，不能加`http://`

```
www.xxx.com
```


注意到时候CNAME一定是在你Github项目的master根目录下，你也可以建立一个CNAME的文本文件，放到Hexo-->public目录下，因为到时候同步上去的是这个public文件夹。

进入[阿里云域名解析地](https://dc.aliyun.com/tcparse/dns.htm)址，添加解析：

1. 记录类型选择`CNAME`
2. 主机记录填`www`
3. 解析线路选择`默认`
4. 记录值填`yourname.github.io`
5. TTL值为`10`分钟
6. 再添加一个解析，记录类型`A`
7. 主机记录填`www`
8. 解析线路选择`默认`
9. 记录值填你GitHub 的ip地址（在cmd中ping：）

```
ping bxm0927.github.com
```

![](http://oph264zoo.bkt.clouddn.com/17-5-29/79989003.jpg)


点击保存，等 1 分钟，访问下你自己的域名，一切就ok了。

域名绑定成功，域名解析成功，因此你在浏览中输入 www.lovebxm.com，或 lovebxm.com 就可以访问到博客了，输入 bxm0927.github.io 会重定向到  www.lovebxm.com。过程：www 的方式，会先解析成 http://xxxx.github.io，然后根据 CNAME 再变成 www

**注意**：CNAME文件在下次 `hexo deploy `的时候就消失了，需要重新创建，这样就很繁琐

方法一：每次 `hexo d` 之后，就去 GitHub 仓库根目录新建 CNAME文件

方法二：在 `hexo g` 之后， `hexo d` 之前，把CNAME文件复制到 "\public\" 目录下面，里面写入你要绑定的域名。

方法三（推荐）：将需要上传至github的内容放在source文件夹，例如CNAME、favicon.ico、images等，这样在 hexo d 之后就不会被删除了。

方法四：通过安装插件实现永久保留

```
$ npm install hexo-generator-cname --save
```

之后在_config.yml中添加一条

```
plugins:
- hexo-generator-cname
```

需要注意的是：如果是在github上建立的CNAME文件，需要先clone到本地，然后安装插件，在deploy上去即可。CNAME只允许一个域名地址。

**注意1**：每次生成的 CNAME 都是 yoursite.com 怎么解决？

修改 _config.yml
```
url: http://www.lovebxm.com
root: /
permalink: :year/:month/:day/:title/
permalink_defaults:
```



## Hexo 常规操作

### 发表一篇文章

运行下面的命令创建一个新文章文件，然后，会在本地博客文件夹 `source\_posts` 文件夹下我们可以看到新建的 markdown 文件。

```
hexo new "文章标题"
```



文章编辑好之后，可以执行本地预览、部署等命令：

```bash
# 本地预览
hexo s

# 删除旧的静态文件，即 public 文件
hexo clean

# 生成新的静态文件，即 public 文件（或者 hexo g）
hexo generate

# 部署到远程站点，即 GitHub（或者 hexo d）
hexo deploye

# 当然你也可以执行下面的组合命令
hexo d -g
```


### 显示部分文章内容

如果在博客文章列表中不想全文显示，可以在文章中增加 `<!-- more -->`, 在这后面的内容就不会显示在列表。

```
<!-- more -->
```

### 更改主题

- 官方主题库：https://hexo.io/themes/
- 有哪些好看的 Hexo 主题？：https://www.zhihu.com/question/24422335

Hexo主题非常丰富，hexo3.0使用的默认主题是landscape，我推荐搭建使用 Next 为主题，好看且文档详细。详情请阅读 Next 的官方文档（ http://theme-next.iissnan.com/ ），5 分钟快速安装。

> 再提示一点，大家可以修改一步就`hexo s`在本地看下效果。


### 添加插件

添加 feed 插件和 sitemap 网站地图，有助于 SEO。

切换到你本地的 hexo 目录 ，输入以下命令

```
npm install hexo-generator-feed -save
npm install hexo-generator-sitemap -save
```

修改 `_config.yml`，增加以下内容


```
# Extensions
Plugins:
- hexo-generator-feed
- hexo-generator-sitemap
#Feed Atom
feed:
  type: atom
  path: atom.xml
  limit: 20
#sitemap
sitemap:
  path: sitemap.xml
```


再执行以下命令，部署到远端：

```
hexo d -g
```

配完之后，就可以访问 https://bxm0927.github.io/atom.xml 和 https://bxm0927.github.io/sitemap.xml ，发现这两个文件已经成功生成了。

### 添加404 页面

GitHub Pages 自定义404页面非常容易，直接在根目录下创建自己的`404.html`就可以。但是自定义404页面仅对绑定顶级域名的项目才起作用，GitHub默认分配的二级域名是不起作用的，使用`hexo s`在本机调试也是不起作用的。

![](http://oph264zoo.bkt.clouddn.com/17-5-29/38118493.jpg)

其实，404页面可以做更多有意义的事，来做个404公益项目吧。

推荐使用腾讯公益404 http://www.qq.com/404/ ：

```
<html>
<head>
    <meta charset="UTF-8">
    <title>404</title>
</head>
<body>
<br><!--
<!DOCTYPE HTML>
<html>
<head>
    <meta charset="UTF-8" />
    <title>公益404 | 不如</title>
</head>
<body>
#404 Not found By Bruce
<h1>404 Page Not Found</h1>
--><br><script type="text/javascript" src="http://www.qq.com/404/search_children.js" charset="utf-8"></script><br><!--
公益404介接入地址
益云公益404 http://yibo.iyiyun.com/Index/web404
腾讯公益404 http://www.qq.com/404
失蹤兒童少年資料管理中心404 http://404page.missingkids.org.tw
-->
<br>
</body>
</html>
```

复制上面代码，贴粘到目录下新建的404.html即可！

我的404页面配置如下：

```
<html>
<head>
  <meta http-equiv="content-type" content="text/html;charset=utf-8;"/>
  <meta http-equiv="X-UA-Compatible" content="IE=edge,chrome=1" />
  <meta name="robots" content="all" />
  <meta name="robots" content="index,follow"/>
</head>
<body>

<script type="text/javascript" src="https://www.qq.com/404/search_children.js"
        charset="utf-8" homePageUrl="gdutxiaoxu.github.io"
        homePageName="回到我的主页">
</script>

</body>
</html>
```

### 多PC同步管理博客

很多人可能家里一台笔记本，公司一个台式机，想两个同时管理博客，同时达到备份的博客主题、文章、配置的目的。下面就介绍一下用github来备份博客并同步博客。

1. A电脑备份博客内容到github

配置.gitignore文件。进入博客目录文件夹下，找到此文件，用sublime text 打开，在最后增加两行内容/.deploy_git和/public

2. 初始化仓库。

在博客根目录下，在git bash下依次执行git init和git remote add origin <server> <server>为远程仓库地址。

3. 同步到远程仓库。

gitbash下依次执行以下命令

```
git add . #添加目录下所有文件

git commit -m "更新说明" #提交并添加更新说明

git push -u origin master #推送更新到远程仓库
```

4. B电脑拉下远程仓库文件

在B电脑上同样先安装好node、git、ssh、hexo，然后建好hexo文件夹，安装好插件，（然后选做：将备份到远程仓库的文件及文件夹删除），然后执行以下命令：

```
git init

git remote add origin <server>

git fetch --all

git reset --hard origin/master
```

5. 发布博客后同步

在B电脑发布完博客之后，记得将博客备份同步到远程仓库
执行以下命令：

```
git add .

#可以用git master 查看更改内容

git commit -m "更新信息"

git push -u origin master  #以后每次提交可以直接git push
```

平时同步管理
每次想写博客时，先执行：git pull进行同步更新。发布完文章后同样按照上面的 发布博客后同步 同步到远程仓库。

### 中文乱码问题

在 md 文件中写中文内容，发布出来后为乱码，原因是 md 的编码不对，将 md 文件另存为`UTF-8`编码的文件即可解决问题。

## 结束语

建站的系统有很多，如：

- [Hexo + GitHub Pages](https://hexo.io/zh-cn/)
- [Jekyll + GitHub Pages](http://jekyll.com.cn/)
- [WordPress + 服务器 + 域名](https://cn.wordpress.org/)
- [DeDeCMS + 服务器 + 域名](http://www.dedecms.com/)
- ...


使用 Hexo + GitHub Pages 建站，有优点也有缺点：
- GitHub Pages 不支持数据库管理，所以你只能做静态页面的博客，不能像其他博客（如 WordPress）那样通过数据库管理自己的博客内容。
- 但是，GitHub Pages 无需购置服务器，免服务器费的同时还能做负载均衡，github pages有300M免费空间。
- 个人博客真的有必要用数据库吗？答案是否定的。博客静态化，评论记录使用第三方的 [网易云跟帖](https://gentie.163.com/info.html) 就可以了。静态的博客更有利于搜索引擎蜘蛛爬取，轻量化的感觉真的很好。
- 通过 Hexo 你可以轻松地使用 Markdown 编写文章，非常符合我的口味。Markdown 真的是专门针对程序员开发的语言啊，现在感觉没有 Markdown 什么都不想写。什么富文本编辑器，什么word，太麻烦了！而且样式都好丑！效率太低！


推荐几个很好用的在线 Markdown 编辑器：
- 作业部落：https://www.zybuluo.com/mdeditor
- 马克飞象：https://maxiang.io


推荐一个图床：[极简图床 + chrome 插件 + 七牛空间](https://jiantuku.com/#/)，七牛云储存提供10G的免费空间,以及每月10G的流量，存放个人博客外链图片最好不过了，七牛云储存还有各种图形处理功能、缩略图、视频存放速度也给力（非打广告）。

以上。
