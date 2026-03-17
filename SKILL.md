---
name: blog-post-publisher
description: Generate technical blog posts and optionally commit or push them to the tech-blog repository.
version: 2.0.0
author: bin
---

# Blog Post Publisher Skill

> 自动将用户的技术分享需求转化为博客文章，并提交到本地 tech-blog 仓库。

---

## 📋 技能信息

```yaml
name: blog-post-publisher
version: 2.0.0
description: 技术博客文章生成与发布工具
author: bin
location: ~/.openclaw/workspace/skills/blog-post-publisher
```

---

## 🎯 核心功能

### 1️⃣ 文章生成
- 用户提供一个主题或想法
- 自动生成结构完整的技术文章
- 遵循 Quarto Markdown 格式规范

### 2️⃣ 内容规范化
- 统一的文件命名（`YYYY-M-D-主题.qmd`）
- 标准化的 Frontmatter（标题、日期、分类、标签）
- 一致的写作风格和质量

### 3️⃣ Git 工作流
- 自动添加到 Git 仓库
- 提交信息规范化
- 可选推送到远程仓库

---

## 📁 项目路径

**重要：** 本技能已集成到 OpenClaw workspace，博客项目也在同一位置。

- **技能目录：** `~/.openclaw/workspace/skills/blog-post-publisher/`
- **博客根目录：** `~/.openclaw/workspace/skills/blog-post-publisher/`
- **文章目录：** `~/.openclaw/workspace/skills/blog-post-publisher/posts/`
- **配置文件：** `~/.openclaw/workspace/skills/blog-post-publisher/_quarto.yml`
- **样式文件：** `~/.openclaw/workspace/skills/blog-post-publisher/styles.css`

---

## 📝 工作流程

### 1. 接收任务

用户会给你一个主题，例如：
- "写一个 post，介绍今天安装的终端工具"
- "写一篇关于 Python 虚拟环境的教程"
- "记录一下今天解决的那个 Git 问题"

### 2. 分析项目结构

```bash
# 查看博客项目结构
ls -la ~/.openclaw/workspace/skills/blog-post-publisher/
ls -la ~/.openclaw/workspace/skills/blog-post-publisher/posts/

# 查看现有文章格式（学习写作风格）
cat ~/.openclaw/workspace/skills/blog-post-publisher/posts/最新文章.qmd
```

### 3. 撰写文章

**文章格式（Quarto Markdown）：**

```markdown
---
title: "文章标题"
date: "2026-XX-XX"
categories: [分类 1, 分类 2]
tags: [标签 1, 标签 2]
description: "一句话描述"
---

## 问题背景/引言
说明为什么写这篇文章，解决什么问题

## 适用环境
- 操作系统
- 软件版本
- 项目类型

## 解决方案/教程内容
分步骤说明，包含代码示例

## 常见问题
Q&A 格式

## 总结
表格对比、建议等

## 参考链接
相关文档、GitHub 仓库等
```

**写作要点：**
- ✅ 标题清晰，包含关键词
- ✅ 代码块标注语言（```bash, ```python 等）
- ✅ 使用表格对比
- ✅ 提供可执行的命令
- ✅ 包含常见问题解答

### 4. 保存文章

```bash
# 文件命名格式：YYYY-M-D-主题.qmd
# 例如：2026-3-14-modern-terminal-tools.qmd

cat > ~/.openclaw/workspace/skills/blog-post-publisher/posts/2026-3-14-主题.qmd << 'EOF'
[文章内容]
EOF
```

### 5. Git 提交并推送

```bash
cd ~/.openclaw/workspace/skills/blog-post-publisher

# 查看状态
git status

# 添加新文章
git add posts/2026-3-14-主题.qmd

# 提交（清晰说明变更）
git commit -m "feat: 添加 [主题] 教程"

# 推送
git push origin main
```

### 6. 通知用户

告诉用户：
- ✅ 文章已发布
- 📝 文章标题和位置
- 🔗 GitHub 仓库链接（如果可以访问）

---

## 🛠️ 工具依赖

- **Quarto**：博客静态站点生成器
- **Git**：版本控制
- **GitHub**：远程仓库

---

## 🎯 示例任务

### 示例 1：工具介绍

**用户输入：**
> 写一个 post，把今天我们刚安装的"Ghostty、Yazi、Lazygit、Zoxide"工具，写一个新手教程指导

**执行步骤：**
1. 收集工具信息（功能、配置、快捷键）
2. 按照格式撰写文章
3. 保存为 `2026-3-14-modern-terminal-tools.qmd`
4. Git 提交推送

### 示例 2：问题解决方案

**用户输入：**
> 记录一下今天解决的 Git 忽略.pyc 文件的问题

**执行步骤：**
1. 回顾问题背景和解决方案
2. 按照格式撰写文章
3. 保存为 `2026-3-4-git_ignore_pyc.qmd`
4. Git 提交推送

### 示例 3：技术对比

**用户输入：**
> 写一篇前端后端服务器对比的文章

**执行步骤：**
1. 收集对比信息（功能、性能、使用场景）
2. 用表格展示对比
3. 保存文章
4. Git 提交推送

---

## ⚠️ 注意事项

1. **日期格式：** 使用 `2026-03-14`（YYYY-MM-DD）
2. **文件名：** 使用 `2026-3-14-主题.qmd`（短横线分隔）
3. **提交信息：** 清晰说明变更（`feat: 添加 XXX 教程`）
4. **网络问题：** 如果 push 失败，告知用户本地已提交
5. **文章质量：** 确保代码示例可执行，步骤清晰

---

## 🔄 未来改进

- [ ] 自动构建并预览（`quarto render`）
- [ ] 自动检查拼写错误
- [ ] 自动生成摘要
- [ ] 支持多语言文章
- [ ] 自动添加标签和分类

---

## 📊 版本历史

- **v2.0.0** (2026-03-15): 迁移到 workspace 目录，规范化管理
- **v1.0.0** (2026-03-14): 初始版本

---

*创建时间：2026-03-14*  
*最后更新：2026-03-15*
