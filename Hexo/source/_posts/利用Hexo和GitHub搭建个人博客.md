---
title: 如何利用Hexo+GitHub搭建个人博客
tags:
  - Hexo
  - Butterfly
categories: 前端
description: 本文记录了无聊的作者如何利用Hexo+GitHub搭建了自己的博客（即本网站）以及在搭建过程中踩过的坑。
cover: pic/01.png
abbrlink: 3375b7de
date: 2022-12-17 00:00:00
updated: 2026-05-01 00:00:00
keywords:
top_img:
comments:
toc:
toc_number:
toc_style_simple:
copyright:
copyright_author:
copyright_author_href:
copyright_url:
copyright_info:
mathjax:
katex:
aplayer:
highlight_shrink:
aside:
---
<p style="text-indent:2em">
本人纯小白，因此本教程（记录贴）门槛为零，请放心食用~</p>
<p style="text-indent:2em">
本文使用的是Hexo 6.3.0 + Butterfly 4.5.1，不同版本之间可能有所差异，本文仅供参考！</p>

## 准备工作
1. 一台能联网的电脑，最好能使用魔法，以便快速访问[GitHub](https://github.com/)等网站。
2. 如果你没有接触过Markdown，建议使用Typora或在VSCode中安装Markdown插件来熟悉一下md的语法。
3. 会使用浏览器（/doge）

## 一、安装 Node.js 和 Git

Hexo 是基于 Node.js 的静态博客框架，Git 用于版本控制和部署。

### 1.1 安装 Node.js

前往 [Node.js 官网](https://nodejs.org/) 下载 LTS 版本并安装。安装完成后打开终端验证：

```bash
node -v
npm -v
```

如果能正常输出版本号，说明安装成功。

### 1.2 安装 Git

前往 [Git 官网](https://git-scm.com/) 下载对应系统的版本并安装。安装完成后验证：

```bash
git --version
```

### 1.3 配置 Git 用户信息

```bash
git config --global user.name "你的用户名"
git config --global user.email "你的邮箱"
```

## 二、注册 GitHub 并创建仓库

### 2.1 注册 GitHub 账号

前往 [GitHub](https://github.com/) 注册一个账号，过程略。

### 2.2 创建仓库

1. 点击右上角的 `+` → `New repository`
2. 仓库名必须为 `用户名.github.io`（例如我的是 `Auroraee.github.io`）
3. 选择 Public
4. 点击 Create repository

> **注意：** 仓库名必须和你的 GitHub 用户名完全一致，否则后续部署会失败。

## 三、安装并初始化 Hexo

### 3.1 安装 Hexo CLI

```bash
npm install -g hexo-cli
```

### 3.2 初始化博客

```bash
hexo init my-blog
cd my-blog
npm install
```

初始化完成后，目录结构大致如下：

```
my-blog/
├── _config.yml      # 站点配置文件
├── package.json     # 依赖管理
├── scaffolds/       # 文章模板
├── source/          # 资源文件夹
│   └── _posts/      # 文章存放处
└── themes/          # 主题文件夹
```

### 3.3 本地预览

```bash
hexo server
```

打开浏览器访问 `http://localhost:4000`，看到默认页面就说明成功了。

## 四、安装并配置 Butterfly 主题

### 4.1 安装主题

在博客根目录下执行：

```bash
npm install hexo-theme-butterfly
```

或者通过 Git 安装：

```bash
git clone -b master https://github.com/jerryc127/hexo-theme-butterfly.git themes/butterfly
```

然后安装主题依赖：

```bash
npm install hexo-renderer-pug hexo-renderer-stylus
```

### 4.2 启用主题

修改根目录下的 `_config.yml`：

```yaml
theme: butterfly
```

### 4.3 配置主题

Butterfly 主题推荐使用覆盖配置的方式。在根目录创建 `_config.butterfly.yml` 文件，把主题的 `_config.yml` 内容复制进去进行修改。这样更新主题时不会丢失自定义配置。

以下是一些常用配置：

**导航菜单：**

```yaml
menu:
  首页: / || fas fa-home
  归档: /archives/ || fas fa-archive
  标签: /tags/ || fas fa-tags
  分类: /categories/ || fas fa-folder-open
  关于: /about/ || fas fa-heart
```

**代码块样式：**

```yaml
highlight_theme: mac light
highlight_copy: true
highlight_lang: true
code_word_wrap: true
```

**深色模式：**

```yaml
darkmode:
  enable: true
  button: true
```

**数学公式支持（KaTeX）：**

```yaml
katex:
  enable: true
  per_page: false
```

同时需要安装渲染器：

```bash
npm install hexo-renderer-markdown-it @neilsustc/markdown-it-katex
```

并在 `_config.yml` 中添加：

```yaml
markdown:
  plugins:
    - plugin:
      name: '@neilsustc/markdown-it-katex'
      options:
        strict: false
```

## 五、撰写第一篇文章

在 `source/_posts/` 目录下创建 `.md` 文件，例如 `hello.md`：

```markdown
---
title: 我的第一篇博客
date: 2026-05-01 12:00:00
tags:
  - 随笔
categories: 日记
---

这是我的第一篇博客文章。

## 小标题

正文内容...
```

也可以使用命令快速创建：

```bash
hexo new post "文章标题"
```

## 六、部署到 GitHub Pages

### 6.1 安装部署插件

```bash
npm install hexo-deployer-git
```

### 6.2 配置部署信息

修改根目录 `_config.yml`：

```yaml
deploy:
  type: git
  repository: git@github.com:用户名/用户名.github.io.git
  branch: main
```

> **注意：** 使用 SSH 协议（`git@` 开头）需要先在 GitHub 添加 SSH Key。如果不想配置，可以使用 HTTPS 地址。

### 6.3 生成并部署

```bash
hexo clean && hexo generate && hexo deploy
```

或者使用简写：

```bash
hexo clean && hexo g -d
```

部署成功后，访问 `https://用户名.github.io` 即可看到你的博客。

## 七、绑定自定义域名（可选）

如果你有自己的域名，可以绑定到 GitHub Pages。

### 7.1 添加 CNAME 文件

在 `source/` 目录下创建 `CNAME` 文件，内容为你的域名：

```
blog.example.com
```

### 7.2 配置 DNS

在你的域名服务商处添加 CNAME 记录，指向 `用户名.github.io`。

### 7.3 修改站点配置

```yaml
url: https://blog.example.com
```

## 八、常用命令速查

| 命令 | 说明 |
|------|------|
| `hexo new post "标题"` | 新建文章 |
| `hexo server` | 本地预览 |
| `hexo clean` | 清除缓存 |
| `hexo generate` | 生成静态文件 |
| `hexo deploy` | 部署到远程 |
| `hexo clean && hexo g -d` | 清理 + 生成 + 部署 |

## 九、踩坑记录

### 9.1 部署后页面空白

检查 `_config.yml` 中的 `url` 是否配置正确，必须和实际访问地址一致。

### 9.2 主题样式异常

确保安装了主题所需的所有依赖，特别是 `hexo-renderer-pug` 和 `hexo-renderer-stylus`。

### 9.3 文章不显示

检查文章的 `front-matter` 中 `date` 是否正确，Hexo 默认不显示未来日期的文章（可在配置中设置 `future: true`）。

### 9.4 部署报错 `Permission denied`

SSH Key 未配置或配置错误。运行 `ssh -T git@github.com` 测试连接，如果失败则需要重新生成并添加 SSH Key。

---

<p style="text-indent:2em">
以上就是搭建 Hexo 博客的完整流程。如果遇到问题，善用搜索引擎和官方文档，大部分问题都能找到解决方案。祝你搭建顺利！</p>
