---
title: "Hugo博客自动部署"
author: regina23
date: 2022-11-03T14:21:52+08:00
draft: true
prev: posts/hugo-setup
---

## 1. 准备工作

### 1.1 新建GitHub仓库

新建仓库名称为`username.github.io`格式，并选择默认的Public。

![](/Users/reginaren/Library/Application%20Support/marktext/images/2022-11-03-17-59-53-image.png)

### 1.2 购买域名

在[Cloudflare](https://www.cloudflare.com/zh-cn/)注册账号，选择注册域名，搜索想注册的域名然后购买完成注册。

![](/Users/reginaren/Library/Application%20Support/marktext/images/2022-11-03-14-35-10-image.png)

### 1.3 域名托管

选择Websites里面的域名进入域名管理页面，添加DNS记录，选择CNAME，添加子域名www，指向GitHub Pages地址。

![](/Users/reginaren/Library/Application%20Support/marktext/images/2022-11-03-15-02-24-image.png)

![](/Users/reginaren/Library/Application%20Support/marktext/images/2022-11-03-17-54-05-image.png)

## 2. GitHub Pages托管

### 2.1 修改本地配置

修改本地站点配置quickstart/config.toml，将baseURL改为自己的域名。

```
baseURL = 'https://www.regina23.com/'
languageCode = 'zh-cn'
title = "Regina23's Blog"
theme = "LoveIt"
```

### 2.2 本地仓库关联到远程仓库

Hugo 默认将生成的静态网页文件存放在 `qiuckstart/public/` 目录下，将该目录初始化，并关联到远程仓库。

```
cd qiuckstart/public
git init 
git remote add origin git@github.com:regina23/regina23.github.io.git
```

本地构建并检查文章内容，无误后推送到远程仓库。

```
cd qiuckstart
hugo
hugo server

git add .
git commit -m "add pages"
git push origin master
```

### 2.2 远程仓库设置域名

进入1.1新建的GitHub仓库，在Settings -> Pages确认部署的分支并保存，然后配置自定义域名并保存。

![](/Users/reginaren/Library/Application%20Support/marktext/images/2022-11-03-18-04-45-image.png)

此时，打开域名网址可以看到博客以及部署上去了。

## 3. GitHub Action自动发布
