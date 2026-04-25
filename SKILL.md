---
name: blog-post-publisher
description: Use when writing, editing, publishing, or committing technical blog posts in the local Quarto tech-blog repository, especially troubleshooting writeups, tutorials, issue-resolution notes, reproducible workflows, or posts that need sanitized examples, validation, and GitHub Pages deployment.
---

# Blog Post Publisher

将技术问题、教程、排查记录或项目经验整理成 Quarto 博客文章，并按仓库工作流验证、提交、按需推送发布。

## 核心原则

- 先理解材料，再写文章；不要把聊天记录机械改写成博客。
- 每篇博客必须有清晰的问题线索、可复用的诊断逻辑、可执行的解决步骤。
- 默认保护隐私和凭据：不要发布密码、token、私钥、完整公钥、服务器真实内网 IP、邮箱密钥、订阅链接或任何 secrets。
- 提交前只暂存本次相关文件，避免把 `.DS_Store`、旧的脏文件、生成产物误提交。

## 定位博客仓库

- 本 skill 所在目录就是博客根目录；如果用户提供了仓库路径，以用户提供路径为准。
- 不要硬编码旧的绝对路径。从当前 `SKILL.md` 所在目录、用户给出的路径或 `git rev-parse --show-toplevel` 定位仓库。
- 文章目录固定为博客根目录下的 `posts/`。
- 站点由 Quarto 构建，主配置通常是 `_quarto.yml`。

常用定位命令：

```bash
git rev-parse --show-toplevel
git status --short --branch
ls -la posts
```

## 每篇博客的强制结构

每篇新博客都必须包含这四个一级内容模块，标题可以根据语境微调，但语义不能缺失：

1. **描述问题**：说明背景、现象、报错、影响范围、读者为什么需要关心。
2. **原因分析**：给出排查证据、关键日志、错误根因、为什么不是其他原因。
3. **解决方法**：提供可执行步骤、命令、配置片段、验证命令和注意事项。
4. **总结**：沉淀规律、检查清单、经验教训或后续建议。

推荐文章骨架：

```markdown
---
title: "清晰标题，包含关键词"
date: "YYYY-MM-DD"
categories: [分类 1, 分类 2]
tags: [标签 1, 标签 2]
description: "一句话说明文章解决什么问题。"
---

## 描述问题

## 原因分析

## 解决方法

## 总结
```

可按需要增加这些可选模块，但不能替代四个强制模块：

- `## 适用环境`
- `## 排查过程`
- `## 验证结果`
- `## 常见问题`
- `## 参考链接`

## 写作要求

- 标题要具体，包含技术关键词和问题关键词。
- Frontmatter 必须包含 `title`、`date`、`categories`、`tags`、`description`。
- 文件名使用 `YYYY-M-D-topic-slug.qmd`，例如 `2026-4-25-ssh-key-login-debug.qmd`。
- 使用 fenced code block，并标注语言：`bash`、`python`、`powershell`、`yaml`、`text` 等。
- 命令必须尽量可执行；如果需要替换变量，用 `<user>`、`<host>`、`your-token` 这类占位符。
- 对排查类文章，必须写明“观察到什么证据，因此得出什么判断”。
- 对方案类文章，必须给出验证命令或成功输出示例。
- 避免空泛总结；总结要能指导下次遇到同类问题时怎么做。

## 安全与脱敏检查

发布前必须扫描并人工确认：

- 不含明文密码、API token、cookie、订阅链接、private key、完整 public key。
- 不含不必要的真实服务器地址、内网 IP、用户名、邮箱或机器名。
- 不含用户未要求公开的项目路径、商业信息、实验数据或个人信息。
- 日志可以保留关键错误行，但要用 `...` 或 `<placeholder>` 替换敏感段。

建议检查：

```bash
rg -n "password|passwd|token|secret|BEGIN .*PRIVATE KEY|ssh-ed25519 AAAA|ssh-rsa AAAA|密码|私钥|密钥" posts/<new-post>.qmd
git diff --check -- posts/<new-post>.qmd
```

如果命令有命中，不要机械删除；判断是否是安全说明文本还是泄露内容。泄露内容必须脱敏后再继续。

## 工作流程

### 1. 收集材料

- 读取用户给出的上下文、日志、命令输出、计划文件或相关源码。
- 查看最近 1-3 篇文章，匹配现有风格和 Quarto frontmatter。
- 如果信息不足，优先基于已有证据写清楚边界；不要编造版本号、错误原因或验证结果。

### 2. 写文章

- 先确定标题、slug、分类和标签。
- 按“描述问题 → 原因分析 → 解决方法 → 总结”的顺序组织正文。
- 排查类文章应包含“失败现象、关键证据、根因、修复、验证”。
- 教程类文章也要解释问题或需求的来源、方案选择原因、执行步骤和结果确认。

### 3. 验证 Quarto

优先用临时副本渲染，避免真实仓库的 `_site` 或缓存文件被误改：

```bash
PREVIEW_DIR="/tmp/blog-preview-$(date +%Y%m%d%H%M%S)"
mkdir -p "$PREVIEW_DIR"
rsync -a --exclude .git ./ "$PREVIEW_DIR"/
cd "$PREVIEW_DIR"
quarto render
```

然后检查生成页面中是否包含新文章标题：

```bash
rg -n "文章标题关键词" _site/index.html _site/posts/index.html _site/posts
```

如果必须在真实仓库中渲染：

- 先记录 `git status --short`。
- 渲染后检查是否产生非预期 `_site` 变更。
- 未经用户要求，不要提交 `_site` 生成产物。

### 4. Git 提交

提交前先同步远端并检查脏文件：

```bash
git status --short --branch
git fetch origin main
git merge --ff-only origin/main
```

只暂存本次相关文件：

```bash
git add posts/<new-post>.qmd
# 如果本次任务是修改 skill，才暂存 SKILL.md
git add SKILL.md
```

提交前必须确认暂存区：

```bash
git diff --cached --name-only
git diff --cached --check
git diff --cached --stat
```

如果暂存区包含 `.DS_Store`、无关文章、旧的用户改动或 `_site` 生成产物，先取消暂存并重新选择文件。

提交信息使用清晰中文：

```bash
git commit -m "feat: 添加 XXX 教程"
git commit -m "docs: 完善博客发布 skill"
```

### 5. 发布

- 用户明确要求“发布到网站”“推送”“上线”时，执行 `git push origin main`。
- 只要求“提交”时，默认完成本地 git commit；如上下文表明该仓库通过 push 触发部署，再推送前说明或确认。
- 推送后可检查 GitHub Actions；如果 `gh` 未登录，就提供 Actions 页面链接和本地 push 结果，不要伪造部署成功状态。

## 常见错误

| 错误 | 正确做法 |
|---|---|
| 只写解决命令，没有原因分析 | 写清楚证据链：现象、日志、排除项、根因 |
| 把聊天里的密码或 key 原样放进文章 | 全部替换为占位符，并说明已脱敏 |
| 普通 `ssh user@host` 成功就写“免密成功” | 用禁用密码的验证命令证明 |
| `git add .` | 只 `git add` 本次文章或本次 skill 文件 |
| 渲染真实仓库后误提交 `_site` | 用 `/tmp` 副本预览，或明确排除生成产物 |
| push 失败仍说已发布 | 区分“本地已提交”和“远端已发布/部署完成” |

## 交付说明

最终回复要包含：

- 文章标题或 skill 修改摘要。
- 文件路径。
- commit hash。
- 是否已 push；如果已 push，给出仓库或网站链接。
- 验证方式：Quarto 渲染、敏感信息扫描、git 暂存区检查等。
- 仍然存在的无关 dirty 文件，避免用户误以为工作区完全干净。
