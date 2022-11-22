---
title: "Picgo + github搭建图床"
author: "Regina"
date: 2022-11-10T17:50:55+08:00
lastmod: 2022-11-10T17:50:55+08:00
draft: false
toc:
  enable: true
prev: /posts/hugo-deployment
---

### 1. Github创建图床仓库

1.1 GitHub新建仓库用于存放图片

1.2 GitHub新建Token，权限选择repo、write:packages、delete:packages、admin:org、admin:public_key、admin:repo_hook、project

### 2. PicGo-Core安装和设置

2.1 安装Node.js

```
brew install node
```

2.2 安装PicGo-Core

```
// 全局安装
npm install picgo -g
```

2.3 修改PicGo-Core配置文件

通过以下命令进入交互式命令行，设置GitHub仓库名、分支名、token等信息。

```
picgo set uploader
```

或修改配置文件设置picBed，macOS默认配置文件位置为`~/.picgo/config.json`。

2.4 使用时间戳

安装插件

```
picgo install super-prefix
```

2.5 上传测试

使用`picgo upload xxx.png`上传图片，显示PicGo SUCCESS

![image-20221122203320672](https://raw.githubusercontent.com/regina23/Picture/main/blog/20221122203329.png)
