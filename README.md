# Sandrew's Base

这是我的个人博客源码仓库，站点基于 [Hugo](https://gohugo.io/) 构建，使用 [hugo-theme-stack](https://github.com/CaiJimmy/hugo-theme-stack) 主题，并通过 GitHub Actions 自动部署到 GitHub Pages。

在线访问地址：

```text
https://sandrewzq.github.io
```

## 技术栈

- 静态站点生成器：Hugo
- 主题：hugo-theme-stack
- 内容格式：Markdown
- 部署方式：GitHub Actions
- 托管平台：GitHub Pages

## 本地开发

### 1. 安装 Hugo

本项目建议使用 Hugo Extended 版本。

Windows 可以使用 winget 安装：

```powershell
winget install Hugo.Hugo.Extended
```

安装完成后检查版本：

```powershell
hugo version
```

### 2. 拉取主题 submodule

主题通过 Git submodule 管理。首次克隆仓库后，需要初始化 submodule：

```powershell
git submodule update --init --recursive
```

如果主题目录缺失或为空，也可以重新执行上面的命令。

### 3. 启动本地预览

在项目根目录运行：

```powershell
hugo server -D
```

默认访问地址：

```text
http://localhost:1313/
```

其中 `-D` 表示包含草稿内容，即 frontmatter 中 `draft: true` 的文章也会显示。

### 4. 本地构建

生成静态文件：

```powershell
hugo --gc --minify
```

构建结果会输出到：

```text
public/
```

`public/` 是 Hugo 自动生成的静态站点成品目录，不需要手动维护。

## 常用命令

```powershell
# 初始化主题
git submodule update --init --recursive

# 本地预览，包含草稿
hugo server -D

# 本地预览，指定端口
hugo server -D --port 1314

# 构建生产静态文件
hugo --gc --minify

# 查看 Hugo 版本
hugo version
```

## 目录结构

```text
.
├── .github/workflows/      # GitHub Actions 自动部署配置
├── archetypes/             # Hugo 内容模板
├── assets/                 # 站点资源文件
├── content/                # 博客文章、页面、工具页等 Markdown 内容
├── data/                   # Hugo 数据文件
├── layouts/                # 自定义布局模板
├── static/                 # 静态资源，会原样复制到站点
├── themes/                 # Hugo 主题目录，当前使用 hugo-theme-stack
├── config.yaml             # Hugo 站点主配置
├── README.md               # 项目说明文档
└── public/                 # Hugo 构建产物，本地生成，不建议提交
```

## 内容说明

主要内容放在 `content/` 目录下：

```text
content/
├── post/                   # 博客文章
├── page/                   # 独立页面，例如关于、工具收集、友链等
├── categories/             # 分类页面
└── _index.md               # 首页相关内容
```

新增文章时，可以在 `content/post/` 下创建新的文章目录或 Markdown 文件。

常见文章结构示例：

```text
content/post/example-post/
├── index.md
└── image.png
```

## 部署说明

项目使用 GitHub Actions 自动部署。

工作流文件：

```text
.github/workflows/hugo.yml
```

部署流程：

1. 推送代码到 `master` 分支
2. GitHub Actions 安装 Hugo
3. 拉取主题 submodule
4. 执行 Hugo 构建
5. 上传 `public/` 目录
6. 发布到 GitHub Pages

正常情况下，只需要执行：

```powershell
git add .
git commit -m "更新内容"
git push
```

推送后，GitHub Actions 会自动完成部署。

## 构建产物说明

`public/` 是 Hugo 构建生成的目录，里面是最终部署到 GitHub Pages 的静态 HTML、CSS、JS、图片等文件。

注意：

- 不要手动编辑 `public/` 里的文件
- 不建议把 `public/` 提交到 Git
- 修改博客内容时，应修改 `content/`、`config.yaml` 或主题相关文件
- 重新运行 Hugo 后，`public/` 会自动更新

## 主题说明

当前主题为：

```text
hugo-theme-stack
```

主题目录：

```text
themes/hugo-theme-stack
```

如果主题目录为空，通常是 submodule 没有初始化，执行：

```powershell
git submodule update --init --recursive
```

## 维护注意事项

- 修改文章内容时，优先编辑 `content/` 目录
- 修改站点配置时，编辑 `config.yaml`
- 修改自动部署流程时，编辑 `.github/workflows/hugo.yml`
- 不要直接修改 `public/` 中的构建结果
- 删除或移动内容页前，应确认不会影响已有链接
- 推送后可以在 GitHub Actions 页面查看构建和部署状态

## License

文章内容和站点资源请以仓库实际声明为准。

主题 `hugo-theme-stack` 遵循其原项目许可证。
