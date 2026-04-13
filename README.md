# Xiangyu CHEN Homepage

这是一个基于 Jekyll 的个人主页项目。当前推荐使用 Conda 创建独立环境 `homepage` 来安装 Ruby、Bundler 和 Jekyll 依赖，并用该环境生成和本地预览网页。

网页构建输出目录：

```text
_site/
```

本地预览地址：

```text
http://127.0.0.1:4000
```

## Conda 环境安装

以下命令默认在项目根目录运行：

```bash
cd /home/hkust/ChangerC77.github.io
```

本项目推荐使用 `conda run -n homepage ...` 执行命令，而不是依赖 `conda activate homepage`。这样可以避免当前 shell 中其它 Conda 环境残留变量影响 Ruby/Bundler。

### 1. 创建 Conda 环境

创建名为 `homepage` 的环境，并安装 Ruby `3.4.x`：

```bash
conda create -y -n homepage -c conda-forge ruby=3.4
```

检查 Ruby 版本：

```bash
conda run -n homepage ruby -v
```

当前机器上测试通过的版本是：

```text
ruby 3.4.8
```

如果 `homepage` 环境已经存在，可以跳过创建环境这一步。

### 2. 安装 Ruby 原生扩展编译工具

本项目依赖的一些 Ruby gems 需要本地编译，例如：

- `eventmachine`
- `ffi`
- `http_parser.rb`
- `bigdecimal`

因此需要给 `homepage` 环境安装编译器、`make` 和 `pkg-config`：

```bash
conda install -y -n homepage -c conda-forge c-compiler cxx-compiler make pkg-config
```

### 3. 安装 Bundler

`Gemfile.lock` 中锁定的 Bundler 版本是 `4.0.9`：

```bash
conda run -n homepage gem install bundler -v 4.0.9
```

检查 Bundler 版本：

```bash
conda run -n homepage bundle -v
```

期望输出：

```text
4.0.9
```

### 4. 安装 Jekyll 项目依赖

```bash
conda run -n homepage bundle install
```

这一步会根据 `Gemfile` 和 `Gemfile.lock` 安装 Jekyll 及相关插件。

## 生成网页

运行：

```bash
cd /home/hkust/ChangerC77.github.io
conda run -n homepage bundle exec jekyll build
```

构建成功后，终端中会看到类似输出：

```text
Destination: /home/hkust/ChangerC77.github.io/_site
Generating...
done
```

生成后的静态网页文件位于：

```text
_site/
```

## 本地预览

启动本地 Jekyll 服务：

```bash
cd /home/hkust/ChangerC77.github.io
conda run --no-capture-output -n homepage bundle exec jekyll serve --no-watch --host 127.0.0.1 --port 4000
```

然后在浏览器打开：

```text
http://127.0.0.1:4000
```

这个命令会一直运行，不会自动退出。这是正常现象，因为它正在前台运行本地网页服务器。

停止本地服务：

```text
Ctrl + C
```

如果 `4000` 端口被占用，可以换一个端口，例如：

```bash
conda run --no-capture-output -n homepage bundle exec jekyll serve --no-watch --host 127.0.0.1 --port 4010
```

然后访问：

```text
http://127.0.0.1:4010
```

## 常用命令

构建网页：

```bash
conda run -n homepage bundle exec jekyll build
```

本地预览：

```bash
conda run --no-capture-output -n homepage bundle exec jekyll serve --no-watch --host 127.0.0.1 --port 4000
```

查看 Ruby 和 Bundler 版本：

```bash
conda run -n homepage ruby -v
conda run -n homepage bundle -v
```

重新安装项目 Ruby 依赖：

```bash
conda run -n homepage bundle install
```

## 重要说明

- `jekyll serve` 看起来像是“卡住不动”，其实是在运行本地网页服务器。
- `--no-capture-output` 用来让 Jekyll 日志直接显示在终端中。
- `--no-watch` 用来关闭文件监听。在当前 Linux + Ruby `3.4.x` 环境中，旧版 Jekyll 依赖的 watch 路径可能触发兼容问题，所以推荐关闭 watch。
- 修改 `_config.yml` 后建议重启本地服务，因为 Jekyll 不一定会完整热更新配置文件。
- 类似 `libtinfo.so.6: no version information available` 的信息通常是当前 shell/Conda 环境警告，一般不是网页构建失败的根本原因。
- `run_server.sh` 仍保留在仓库中，但当前机器推荐使用本 README 中的 Conda 命令来构建和预览。

## 从零重建环境

如果 `homepage` 环境已经损坏，或者想重新创建，可以执行：

```bash
conda env remove -y -n homepage
conda create -y -n homepage -c conda-forge ruby=3.4
conda install -y -n homepage -c conda-forge c-compiler cxx-compiler make pkg-config
conda run -n homepage gem install bundler -v 4.0.9
conda run -n homepage bundle install
```

然后重新构建：

```bash
conda run -n homepage bundle exec jekyll build
```

## 目录说明

### 核心文件

- `_pages/about.md`
  - 首页主要内容。
  - 包括自我介绍、News、Educations、Publications、Internships、Honor and Awards、Skills。

- `_config.yml`
  - 站点基础配置。
  - 包括标题、作者信息、邮箱、头像、SEO、语言映射等。

- `_data/navigation.yml`
  - 顶部导航栏顺序和跳转锚点。

- `_includes/author-profile.html`
  - 左侧侧边栏个人信息的渲染逻辑。
  - 包括头像、邮箱、GitHub、Google Scholar 等。

- `_includes/masthead.html`
  - 顶部导航栏模板。

- `_includes/head/custom.html`
  - 浏览器图标、额外样式、额外脚本。

- `_layouts/default.html`
  - 全站默认页面布局。

- `assets/css/accent-enhancements.css`
  - 当前主页的大部分自定义样式。

- `assets/css/main.scss`
  - 主样式入口文件。

- `Gemfile` / `Gemfile.lock`
  - Ruby 与 Jekyll 依赖。

### 资源目录

- `images/`
  - 页面中实际使用的图片资源。

- `assets/`
  - CSS、JavaScript、字体、PDF 等静态资源。

- `docs/`
  - 文档和截图。

- `video/`
  - 页面使用的视频资源。

- `CV/`
  - 简历 PDF 文件。

- `certificate/`
  - 证书和奖项文件。

- `google_scholar_crawler/`
  - Google Scholar 相关脚本。
  - 日常编辑主页和 Jekyll 构建不需要运行它。

## 修改位置对照表

### 首页正文内容

主要修改：

```text
_pages/about.md
```

### 姓名、邮箱、头像和侧边栏信息

主要修改：

```text
_config.yml
```

常见字段：

```yaml
title
name
author.name
author.avatar
author.bio
author.location
author.email
author.github
author.googlescholar
```

如果需要调整左侧侧边栏显示逻辑，可以查看：

```text
_includes/author-profile.html
```

### 顶部导航栏顺序

主要修改：

```text
_data/navigation.yml
```

这里决定显示哪些导航项、导航顺序，以及每一项跳到哪个锚点。

### 导航栏显示文字

主要修改：

```text
_config.yml
```

位置：

```yaml
t:
  en:
```

### favicon / 浏览器标签页图标

主要查看：

```text
_includes/head/custom.html
images/
```

## 常见问题

### 运行 `serve` 后终端一直不返回

这是正常的。本地预览命令会启动一个网页服务器，所以它会一直运行：

```bash
conda run --no-capture-output -n homepage bundle exec jekyll serve --no-watch --host 127.0.0.1 --port 4000
```

打开浏览器访问：

```text
http://127.0.0.1:4000
```

停止服务时按：

```text
Ctrl + C
```

### 端口被占用

换一个端口即可：

```bash
conda run --no-capture-output -n homepage bundle exec jekyll serve --no-watch --host 127.0.0.1 --port 4010
```

然后访问：

```text
http://127.0.0.1:4010
```

### `bundle install` 编译失败

如果报错中出现缺少编译器、`make`、`pkg-config`，或者 native extension 编译失败，重新安装编译工具后再运行 `bundle install`：

```bash
conda install -y -n homepage -c conda-forge c-compiler cxx-compiler make pkg-config
conda run -n homepage bundle install
```

## License

本仓库保留原模板所使用的相关开源依赖许可，详情见 [LICENSE](./LICENSE)。
