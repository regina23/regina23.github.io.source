---
title: "Hugo博客搭建"
author: regina23
date: 2022-11-02T18:28:54+08:00
draft: false
next: /posts/hugo-deployment
---

官方文档：[Quick Start](https://gohugo.io/getting-started/quick-start/)

### 1. 安装Hugo

```
brew install hugo
hugo version
```

### 2. 创建一个新的网站

```
hugo new site quickstart
```

创建了`quickstart`的目录，目录结构为：

```
├── archetypes
│   └── default.md
├── config.toml         # 配置文件
├── content             # 文章所在目录
├── data                
├── layouts             # 网站布局
├── static              # 静态内容
└── themes              # 博客主题
```

### 3. 添加主题

主题参见[themes](https://themes.gohugo.io/)，下载主题并添加到配置文件，注意主题所在文件夹和配置中名称对应。

```
cd quickstart
git init
git submodule add https://github.com/dillonzq/LoveIt.git themes/LoveIt
echo theme = \"LoveIt\" >> config.toml
```

### 4. 新建内容页

新建文章可以手动添加文件至`/content/posts/`目录，也可以使用`new`命令。
文章`draft: true`表示草稿，不会被部署；文章完成后修改为`draft: false`。

```
hugo new posts/my-first-post.md
```

其中文章主要结构包括属性和正文，属性主要有：

```
---
title:                      # 文章标题
author:                     # 文章作者
description :               # 文章描述信息
date:                       # 文章编写日期
lastmod:                    # 文章修改日期

tags = [                    # 文章所属标签
    "文章标签1",
    "文章标签2"
]
categories = [              # 文章所属标签
    "文章分类1",
    "文章分类2",
]
keywords = [                # 文章关键词
    "Hugo",
    "static",
    "generator",
]

next: /posts/next           # 下一篇博客地址
prev: /posts/prev           # 上一篇博客地址
---
```

### 5. 启动Hugo服务

`-D`表示草稿也会被加载，浏览器访问http://localhost:1313/即可看到网站。

```
hugo server -D
```

### 6. 修改模版配置

修改新建文档的初始化配置archetypes/defeault.md：

```
---
title: "{{ replace .Name "-" " " | title }}"
author: ["Regina23"]
date: {{ .Date }}
lastmod: {{ .Date }}
draft: true
---
```

### 7. 修改项目配置

修改config.toml

```
baseURL = 'https://www.regina23.com/'
languageCode = zh-cn
title = "Regina23's Blog"
theme = "LoveIt"
```
