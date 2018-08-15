---
layout:     post
title:      iOS APP提交审核被拒因Guideline 2.5.2 - Performance - Software Requirements
subtitle:   Guideline 2.5.2
date:       2018-08-15
author:     Kevin
header-img: img/post-bg-cook.jpg
catalog: true
tags:
    - iOS
---

## 前言

iOS APP提交审核被拒因Guideline 2.5.2 - Performance - Software Requirements


## Guideline 2.5.2

>关键词：Guideline 2.5.1 Guideline 2.5.2

### Guideline 2.5.1 

打开项目工程，全局搜索 prefs:root=   替换成 UIApplicationOpenSettingsURLString

### Guideline 2.5.2

网上各种百度 Google, 知道苹果不允许使用私有方法 dlopen(), dlsym(), respondstoselector动态方法, performselector: method_exchangeimplementations().项目中许多第三方不知道那个用到了,需要一个一个查,全局搜索是没有的,一般这两个方法都在第三方库打包到的. a 文件中,所以需要命令行查看:

1. cd .a上级文件夹目录 (找到.a文件的目录)2. nm -n xxx.a >> xxx.txt (将 静态库名.a 所用的函数名保存到 xxx.txt 文件中)

打开 xxx.txt 文件搜索dlopen, dlsym 这两个函数名如果有用到就比较危险了,将第三方库更新(有的第三方库最新版本可能是不行反而老版本可以,看情况)
