---
title: "Hugo博客自动部署"
author: Regina23
date: 2022-11-03T14:21:52+08:00
draft: false
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

## 3. GitHub Actions自动发布

### 3.1 新建远程仓库和Hugo根目录关联

GitHubPages托管+手动发布静态文件比较繁琐，因此选择GitHub Action进行自动化发布。

新建仓库，例如`regina23.github.io.source`，将本地Hugo根目录`qiuckstart`关联到该仓库并推送。

```
cd qiuckstart
git init 
git remote add origin git@github.com:regina23/regina23.github.io.source.git
git add .
git commit -m "init"
git push origin master
```

### 3.2 配置GitHub Actions

在远程仓库新建GitHub Actions->Hugo，会在目录`/.github/workflows/`下，以`.yml`为后缀新建配置文件。

![](/Users/reginaren/Library/Application%20Support/marktext/images/2022-11-04-16-28-59-image.png)

```
# Sample workflow for building and deploying a Hugo site to GitHub Pages
name: deploy 

on:
  # Runs on pushes targeting the default branch
  push:
    branches: ["master"]

  # Allows you to run this workflow manually from the Actions tab
  workflow_dispatch:

# Sets permissions of the GITHUB_TOKEN to allow deployment to GitHub Pages
permissions:
  contents: read
  pages: write
  id-token: write

# Allow one concurrent deployment
concurrency:
  group: "pages"
  cancel-in-progress: true

# Default to bash
defaults:
  run:
    shell: bash

jobs:
  # Build job
  build:
    runs-on: ubuntu-latest
    env:
      HUGO_VERSION: 0.102.3
    steps:
      - name: Install Hugo CLI
        run: |
          wget -O ${{ runner.temp }}/hugo.deb https://github.com/gohugoio/hugo/releases/download/v${HUGO_VERSION}/hugo_extended_${HUGO_VERSION}_Linux-64bit.deb \
          && sudo dpkg -i ${{ runner.temp }}/hugo.deb
      - name: Checkout
        uses: actions/checkout@v3
        with:
          submodules: true
      - name: Setup Pages
        id: pages
        uses: actions/configure-pages@v2
      - name: Build with Hugo
        env:
          # For maximum backward compatibility with Hugo modules
          HUGO_ENVIRONMENT: production
          HUGO_ENV: production
        run: |
          hugo \
            --minify \
            --baseURL "${{ steps.pages.outputs.base_url }}/"
      - name: Upload artifact
        uses: actions/upload-pages-artifact@v1
        with:
          path: ./public

  # Deployment job
  deploy:
    environment:
      name: github-pages
      url: ${{ steps.deployment.outputs.page_url }}
    runs-on: ubuntu-latest
    needs: build
    steps:
      - name: Deploy to GitHub Pages
        id: deployment
        uses: actions/deploy-pages@v1
```
