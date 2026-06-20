# Obsidian + GitHub Pages 博客搭建指南

本仓库是你的个人主页源码，站点地址：**https://huyf9.github.io**

## 架构概览

```text
Obsidian 写作  →  git push  →  GitHub Actions 构建  →  GitHub Pages 发布
```

- **博客引擎**：Jekyll + [Chirpy](https://github.com/cotes2020/jekyll-theme-chirpy) 主题
- **构建方式**：推送到 GitHub 后由 Actions 自动构建（本地无需安装 Ruby/Node）
- **写作工具**：Obsidian 直接打开本文件夹作为库

## 第一步：创建 GitHub 仓库

1. 登录 [GitHub](https://github.com)
2. 新建仓库，名称必须为 **`huyf9.github.io`**（与用户名一致才能作为个人主页）
3. 设为 **Public**，不要勾选「Add a README」

在本地执行（将远程地址换成你的仓库）：

```powershell
cd D:\blog\huyf9.github.io
git remote remove origin
git remote add origin https://github.com/Huyf9/huyf9.github.io.git
git add .
git commit -m "初始化个人主页与 Obsidian 博客"
git push -u origin main
```

## 第二步：启用 GitHub Pages

1. 打开仓库 **Settings → Pages**
2. **Build and deployment → Source** 选择 **GitHub Actions**
3. 推送代码后，在 **Actions** 标签页查看构建进度
4. 构建成功后访问 https://huyf9.github.io

## 第三步：用 Obsidian 打开仓库

1. 打开 Obsidian → **打开文件夹作为库**
2. 选择 `D:\blog\huyf9.github.io`
3. 进入 **设置 → 社区插件 → 关闭安全模式**
4. 安装并启用以下插件：
   - **Obsidian Git** — 在 Obsidian 内提交、推送代码
   - **Templater**（可选）— 使用 `templates/博客文章.md` 快速创建文章

### 配置 Obsidian Git

进入 **Obsidian Git 设置**，建议：

| 选项 | 建议值 |
|------|--------|
| Vault backup interval (minutes) | 10（自动提交间隔，0 为关闭） |
| Auto pull interval (minutes) | 10 |
| Pull updates on startup | 开启 |
| Disable push | 关闭 |

推送时若提示认证，使用 GitHub **Personal Access Token**（见下方）。

## 第四步：发布博客文章

### 文章存放位置

- 已发布文章：`_posts/`
- 草稿（不发布）：`_drafts/`（需自行创建该文件夹）
- 图片附件：`assets/img/`

### 文件命名

```text
YYYY-MM-DD-英文或拼音标题.md
```

示例：`_posts/2026-06-20-hello-world.md`

### 文章头部（Frontmatter）

```yaml
---
title: 文章标题
date: 2026-06-20 12:00:00 +0800
categories: [读书笔记]   # 主题，自己定义，如：技术笔记、随笔
tags: [心理学]           # 标签，可选，用于更细的标记
---
```

站点按 **主题**（`categories`）归档，不按年月浏览。侧边栏点「主题」可查看所有主题及文章。

可选：在 `_posts` 下按主题建子文件夹，例如 `_posts/读书笔记/2026-06-20-某书.md`。

### 发布流程

1. 在 `_posts` 新建或编辑 Markdown 文件
2. Obsidian 命令面板（`Ctrl+P`）→ **Obsidian Git: Commit all changes**
3. 再执行 **Obsidian Git: Push**
4. 等待 GitHub Actions 构建完成（约 1–2 分钟）

## GitHub 认证（推送时需要）

GitHub 已不支持密码推送，需使用 Personal Access Token：

1. GitHub → **Settings → Developer settings → Personal access tokens**
2. 选择 **Tokens (classic)** → **Generate new token**
3. 勾选 `repo` 权限
4. 推送时用户名填 `Huyf9`，密码处粘贴 Token

## 目录结构

```text
huyf9.github.io/
├── _posts/          # 博客文章（Obsidian 主要写作区）
├── _drafts/         # 草稿（自行创建，不会发布）
├── _tabs/           # 导航页（主题、标签、关于等）
├── assets/img/      # 图片资源
├── templates/       # Obsidian 文章模板
├── .obsidian/       # Obsidian 库配置
├── _config.yml      # 站点配置（标题、作者等）
└── .github/workflows/  # 自动部署流程
```

## 常见问题

**Q：Obsidian 的双链 `[[链接]]` 能在网站上用吗？**

不能直接被 Jekyll 识别。发布到博客的文章请使用标准 Markdown 链接 `[文字](url)`，或仅在本地笔记中使用双链。

**Q：修改了 `_config.yml` 没生效？**

配置变更需要重新 push 并等待 Actions 构建完成。

**Q：想换主题或站点标题？**

编辑 `_config.yml` 中的 `title`、`tagline`、`social` 等字段。

**Q：构建失败怎么办？**

到 GitHub 仓库 **Actions** 页查看错误日志，常见原因是文章 frontmatter 格式错误或文件名不符合规范。

## 参考链接

- [Chirpy 主题文档](https://github.com/cotes2020/jekyll-theme-chirpy/wiki)
- [Obsidian Git 插件](https://github.com/Vinzent03/obsidian-git)
- [GitHub Pages 文档](https://docs.github.com/en/pages)
