---
title: Markdown 入门详细教程
date: 2018-06-26 08:53:05
tags: [Markdown, 有道云笔记]
categories: [Skills]
---

## Markdown 简介

- Markdown 中文官网：http://www.markdown.cn
- 【简明版】有道云笔记Markdown指南：http://note.youdao.com/iyoudao/?p=2411
- 【进阶版】有道云笔记Markdown指南：http://note.youdao.com/iyoudao/?p=2445

Markdown 是一种简单、易读易写的「标记语言」，用来写博客、做笔记非常方便，通常为程序员群体所用。

<!-- more -->

Markdown 的语法十分简单，常用的标记符号不超过十个，用于日常写作记录绰绰有余，不到半小时就能完全掌握。用户可以使用一些标记符号以最小的输入代价生成极富表现力的文档（譬如您正在阅读的这份文档就是通过 Markdown 书写的）。

![](http://oph264zoo.bkt.clouddn.com/18-6-26/25081922.jpg)


## 安利一波 Markdown 工具

> 注意：不同平台的 Markdown 语法会有些许差别。

Markdown 编辑器（支持实时同步预览）：

- VSCode
- Sublime + Markdown 插件（MarkdownPreview）

Markdown 在线编辑器：

- CSDN 博客：https://blog.csdn.net
- 简书：https://www.jianshu.com
- 马克飞象：https://maxiang.io
- 作业部落：https://www.zybuluo.com/mdeditor


笔记工具：

- 有道云笔记：https://note.youdao.com
- 印象笔记：https://www.yinxiang.com
- 为知笔记：http://www.wiz.cn
- OneNote：https://www.onenote.com



## Markdown 基本语法

Markdown 的标记语法非常之多，并且不同平台的 Markdown 语法会有些许差别，但是常用的标记符号不超过十个，且平台间基本通用。

常用标记：标题、粗体、斜体、删除线、下划线、高亮、引用、分割线、链接与图片

不常用标记：列表、待办事项、表格、代码高亮、流程图、序列图、甘特图、数学公式

### 标题

![image](http://note.youdao.com/iyoudao/wp-content/uploads/2016/09/2-0-%E6%A0%87%E9%A2%98.png)


### 粗体和斜体

![image](http://note.youdao.com/iyoudao/wp-content/uploads/2016/09/2-3-%E7%B2%97%E4%BD%93%E6%96%9C%E4%BD%93.png)



### 引用

![image](http://note.youdao.com/iyoudao/wp-content/uploads/2016/09/2-2%E5%BC%95%E7%94%A8.png)


### 链接与图片

![image](http://note.youdao.com/iyoudao/wp-content/uploads/2016/09/2-4%E9%93%BE%E6%8E%A5%E4%B8%8E%E5%9B%BE%E7%89%87.png)



### 分割线

![image](http://note.youdao.com/iyoudao/wp-content/uploads/2016/09/2-5-%E5%88%86%E5%89%B2%E7%BA%BF.png)



### 列表

![image](http://note.youdao.com/iyoudao/wp-content/uploads/2016/09/2-1-%E5%88%97%E8%A1%A8.png)


### 待办事项

![image](http://note.youdao.com/iyoudao/wp-content/uploads/2016/09/1-2%E5%BE%85%E5%8A%9E%E4%BA%8B%E9%A1%B9.png)


### 表格

![image](http://note.youdao.com/iyoudao/wp-content/uploads/2016/09/1-3%E8%A1%A8%E6%A0%BC.png)


### 代码高亮：

![image](http://note.youdao.com/iyoudao/wp-content/uploads/2016/09/1-1%E4%BB%A3%E7%A0%81%E9%AB%98%E4%BA%AE.png)


### 流程图

![image](http://note.youdao.com/iyoudao/wp-content/uploads/2016/09/%E6%B5%81%E7%A8%8B%E5%9B%BE.png)



### 甘特图

![image](http://note.youdao.com/iyoudao/wp-content/uploads/2016/09/%E7%94%981.png)


## Markdown 使用技巧

### 表情

可以在 HTML、Markdown 中使用下面的表情，微软自带的输入法可以输入这些表情：

📦 🚀 🐠 ✂️ 🔥 🚨 ✏️ 🛠 💅 👌 ✨ 💯 ⚛️ 📄

![](http://oph264zoo.bkt.clouddn.com/18-1-11/163002.jpg)


### 目录

有道云支持 Markdown 目录，使用 `[TOC]`


### 控制图片大小和位置


```
<div align="center">
  <img width="65" height="75" src=""/>
</div>
```

### 图文混排

```
<img align="right" src=""/>

这是一个示例图片。

图片显示在 N 段文字的右边。

N 与图片高度有关。

刷屏行。

刷屏行。

到这里应该不会受影响了，本行应该延伸到了图片的正下方，所以我要足够长才能确保不同的屏幕下都看到效果。
```

### 在表格单元格里换行

借助于 HTML 里的 `<br />` 实现。

```
| Header1 | Header2                          |
|---------|----------------------------------|
| item 1  | 1. one<br />2. two<br />3. three |
```

### 老子今天不加班.mp3

```
<audio controls="controls" autoplay="true" src="https://oijmns1ch.qnssl.com/antiwork_today.mp3"></audio>
```

