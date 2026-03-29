# Xiangyu CHEN Homepage
## macOS 本地启动

当前项目推荐在 macOS 上使用 Ruby `3.4.1` 运行，项目所需版本记录在：

```text
.ruby-version
```

启动成功后可访问：

```text
http://127.0.0.1:4000
```

说明：

- `run_server.sh` 会启动 Jekyll 本地预览服务。
- 如果 LiveReload 默认端口被占用，脚本会自动尝试切换端口。
- 修改 `_config.yml` 后通常需要重启服务，因为 Jekyll 不会完整热更新配置文件。

### 方式一：macOS 推荐方案（Homebrew + rbenv）

#### 1. 安装 Xcode Command Line Tools

```bash
xcode-select --install
```

#### 2. 安装 Homebrew

如果尚未安装 Homebrew，可执行：

```bash
/bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"
```

安装完成后建议先确认：

```bash
brew -v
```

#### 3. 安装 Ruby 环境管理工具

```bash
brew install rbenv ruby-build
```

然后将 `rbenv` 初始化写入 shell 配置：

```bash
echo 'eval "$(rbenv init - zsh)"' >> ~/.zshrc
source ~/.zshrc
```

#### 4. 安装项目所需 Ruby

```bash
rbenv install 3.4.1
rbenv local 3.4.1
ruby -v
```

#### 5. 安装 Bundler 和项目依赖

```bash
gem install bundler -v 4.0.9
bundle install
```

#### 6. 启动项目

```bash
bash run_server.sh
```

### 方式二：macOS 可选方案（Conda）

如果你更习惯使用 Conda，也可以在 macOS 上按下面的方式启动：

```bash
conda create -n homepage -c conda-forge ruby=3.4
conda activate homepage
gem install bundler -v 4.0.9
bundle install
bash run_server.sh
```

### 方式三：已安装 Ruby 的 macOS 环境

如果你的 Mac 已经装好了兼容版本的 Ruby 和 Bundler，也可以直接执行：

```bash
bundle install
bash run_server.sh
```

## 常用命令

安装依赖：

```bash
bundle install
```

启动本地预览：

```bash
bash run_server.sh
```

检查构建：

```bash
bundle exec jekyll build
```

## 目录说明

### 核心文件

- `_pages/about.md`
  - 首页主要内容
  - 包括自我介绍、News、Educations、Publications、Internships、Honor and Awards、Skills

- `_config.yml`
  - 站点基础配置
  - 包括标题、作者信息、邮箱、头像、SEO、语言映射等

- `_data/navigation.yml`
  - 顶部导航栏顺序和跳转锚点

- `_includes/author-profile.html`
  - 左侧侧边栏个人信息的渲染逻辑
  - 包括头像、邮箱、GitHub、Google Scholar 等

- `_includes/masthead.html`
  - 顶部导航栏模板

- `_includes/head/custom.html`
  - 浏览器图标、额外样式、额外脚本

- `_layouts/default.html`
  - 全站默认页面布局

- `assets/css/accent-enhancements.css`
  - 当前主页的大部分自定义样式都在这里

- `assets/css/main.scss`
  - 主样式入口文件

- `run_server.sh`
  - 本地开发启动脚本

- `Gemfile` / `Gemfile.lock`
  - Ruby 与 Jekyll 依赖

### 资源文件

- `images/`
  - 页面中实际使用的图片资源
  - 当前 favicon 使用的是 `images/hkust.png`

- `assets/`
  - 字体、PDF、JS、CSS 等静态资源

- `docs/`
  - 文档和截图，不影响主页主体渲染

- `google_scholar_crawler/`
  - Google Scholar 相关脚本
  - 不是主页日常编辑的主要入口

## 文件修改对照表

### 1. 首页正文内容

主要修改文件：

```text
_pages/about.md
```

对应内容：

- 顶部自我介绍
- News
- Educations
- Publications
- Internships
- Honor and Awards
- Skills

### 2. 姓名、邮箱、头像和侧边栏信息

主要修改文件：

```text
_config.yml
```

主要字段：

- `title`
- `name`
- `author.name`
- `author.avatar`
- `author.bio`
- `author.location`
- `author.email`
- `author.github`
- `author.googlescholar`

如果你还想调整左侧侧边栏的显示逻辑，可以再查看：

```text
_includes/author-profile.html
```

### 3. 顶部导航栏顺序

主要修改文件：

```text
_data/navigation.yml
```

这里决定：

- 显示哪些导航项
- 导航顺序
- 每一项跳到哪个锚点

### 4. 导航栏显示文字

主要修改文件：

```text
_config.yml
```

位置：

```yaml
t:
  en:
```

这里控制例如：

- `about`
- `news`
- `educations`
- `publications`
- `internships`

### 5. favicon / 浏览器标签页图标

主要修改文件：

```text
images/hkust.png
_includes/head/custom.html
images/site.webmanifest
```

### 6. 全站样式

主要修改文件：

```text
assets/css/accent-enhancements.css
```

如果需要调整样式入口或主题基础结构，可继续查看：

```text
assets/css/main.scss
_sass/
```

### 7. 顶部导航栏模板结构

主要修改文件：

```text
_includes/masthead.html
```

### 8. 页面整体布局

主要修改文件：

```text
_layouts/default.html
```

## 当前仓库与原模板相比的说明

当前仓库已经做过一些定制，因此与原始模板并不完全一致：

- 博客系统相关内容已移除
- `_posts/` 已不再作为当前主页内容来源
- 首页内容集中在 `_pages/about.md`
- 浏览器图标已改为 `images/hkust.png`
- 左侧社交链接已做裁剪，只保留当前需要的项目

## 修改建议

如果只是更新主页内容，通常只需要修改下面 3 个文件：

```text
_pages/about.md
_config.yml
_data/navigation.yml
```

如果改完页面后没有立刻看到变化，优先检查两件事：

1. 本地服务是否需要重启，尤其是改了 `_config.yml`
2. 浏览器是否缓存了旧样式或旧 favicon

## License

本仓库保留原模板所使用的相关开源依赖许可，详情见 [LICENSE](./LICENSE)。
