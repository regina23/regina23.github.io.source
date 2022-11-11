---
title: "Picgo + github搭建图床"
author: ["Regina"]
date: 2022-11-10T17:50:55+08:00
lastmod: 2022-11-10T17:50:55+08:00
draft: false
toc:
  enable: true
categories: [""]
tags: [""]
prev: /posts/hugo-deployment
---

### 1. Github设置

1.1 GitHub新建仓库用于存放图片

1.2 GitHub新建Token，权限选择repo、write:packages、delete:packages、admin:org、admin:public_key、admin:repo_hook、project

### 2. PicGo设置

picgo选择Github仓库，填写仓库名称、分支、token、路径（选填）等信息

### 3. MerkText设置

preference -> Image目录下选择使用picgo上传图片
