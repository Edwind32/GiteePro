# GiteePro

> 使用 Git 与 Gitee 交互的完整指南

## 目录

- [简介](#简介)
- [环境准备](#环境准备)
- [连接方式](#连接方式)
  - [HTTPS 方式](#https-方式)
  - [SSH 方式](#ssh-方式)
- [基本操作](#基本操作)
  - [克隆仓库](#克隆仓库)
  - [初始化本地仓库并关联远程](#初始化本地仓库并关联远程)
  - [查看远程仓库](#查看远程仓库)
  - [提交更改](#提交更改)
  - [推送代码](#推送代码)
  - [拉取更新](#拉取更新)
- [分支管理](#分支管理)
- [常见工作流](#常见工作流)
- [常见问题](#常见问题)

---

## 简介

[Gitee](https://gitee.com)（码云）是国内领先的代码托管平台，完全兼容 Git 协议。你可以使用标准的 Git 命令与 Gitee 上的远程仓库进行交互，包括克隆、推送、拉取等操作。

---

## 环境准备

1. **安装 Git**

   - Windows：从 [https://git-scm.com](https://git-scm.com) 下载并安装。
   - macOS：`brew install git`
   - Linux（Debian/Ubuntu）：`sudo apt install git`

2. **配置全局用户信息**

   ```bash
   git config --global user.name "你的用户名"
   git config --global user.email "你的邮箱@example.com"
   ```

   > 这些信息会出现在你的每次提交记录中，应与 Gitee 账号保持一致。

---

## 连接方式

### HTTPS 方式

HTTPS 是最简单的连接方式，适合偶尔使用或不方便配置 SSH 的场景。

**克隆仓库：**

```bash
git clone https://gitee.com/用户名/仓库名.git
```

推送时需要输入 Gitee 的用户名和密码（或个人访问令牌）。

**使用凭证缓存（避免重复输入密码）：**

```bash
# 临时缓存（默认 15 分钟）
git config --global credential.helper cache

# 永久保存（明文存储，注意安全）
git config --global credential.helper store
```

---

### SSH 方式

SSH 方式更安全，配置完成后无需每次输入密码，推荐用于日常开发。

**第一步：生成 SSH 密钥对**

```bash
ssh-keygen -t ed25519 -C "你的邮箱@example.com"
```

> 如果系统不支持 ed25519，可以使用：`ssh-keygen -t rsa -b 4096 -C "你的邮箱@example.com"`

按提示操作，密钥默认保存在 `~/.ssh/id_ed25519`（私钥）和 `~/.ssh/id_ed25519.pub`（公钥）。

**第二步：将公钥添加到 Gitee**

1. 查看公钥内容：

   ```bash
   cat ~/.ssh/id_ed25519.pub
   ```

2. 登录 Gitee，进入 **设置 → SSH 公钥**，将公钥内容粘贴并保存。

**第三步：验证连接**

```bash
ssh -T git@gitee.com
```

若返回 `Hi 用户名! You've successfully authenticated...` 则配置成功。

**使用 SSH 克隆仓库：**

```bash
git clone git@gitee.com:用户名/仓库名.git
```

---

## 基本操作

### 克隆仓库

将 Gitee 上的远程仓库克隆到本地：

```bash
# HTTPS
git clone https://gitee.com/用户名/仓库名.git

# SSH
git clone git@gitee.com:用户名/仓库名.git

# 克隆到指定目录
git clone git@gitee.com:用户名/仓库名.git 本地目录名
```

---

### 初始化本地仓库并关联远程

如果你有一个已有的本地项目，想推送到 Gitee 上的新建仓库：

```bash
# 在项目目录中初始化 Git
cd 你的项目目录
git init

# 关联 Gitee 远程仓库
git remote add origin git@gitee.com:用户名/仓库名.git

# 添加文件、提交、推送
git add .
git commit -m "初始化项目"
git push -u origin main
```

---

### 查看远程仓库

```bash
# 列出所有远程仓库
git remote -v

# 修改远程地址（例如从 HTTPS 切换到 SSH）
git remote set-url origin git@gitee.com:用户名/仓库名.git
```

---

### 提交更改

```bash
# 查看当前状态
git status

# 将文件添加到暂存区
git add 文件名          # 添加指定文件
git add .              # 添加所有更改

# 提交到本地仓库
git commit -m "提交说明"

# 修改最近一次提交信息
git commit --amend -m "新的提交说明"
```

---

### 推送代码

```bash
# 推送到远程仓库（首次使用 -u 建立跟踪关系）
git push -u origin main

# 后续推送
git push

# 推送指定分支
git push origin 分支名

# 强制推送（推荐使用 --force-with-lease 而非 --force）
# --force-with-lease 会在远程有新提交时拒绝推送，防止覆盖他人的工作
git push --force-with-lease origin 分支名
```

---

### 拉取更新

```bash
# 拉取并合并远程最新代码
git pull

# 拉取指定分支
git pull origin 分支名

# 先拉取后变基（保持提交历史整洁）
git pull --rebase origin main
```

---

## 分支管理

```bash
# 查看本地分支
git branch

# 查看所有分支（包含远程）
git branch -a

# 创建并切换到新分支
git checkout -b 新分支名

# 或使用新命令（Git 2.23+）
git switch -c 新分支名

# 切换分支
git checkout 分支名
git switch 分支名

# 推送新分支到 Gitee
git push -u origin 新分支名

# 合并分支（先切换到目标分支）
git checkout main
git merge 功能分支名

# 删除已合并的本地分支
git branch -d 分支名

# 删除远程分支
git push origin --delete 分支名
```

---

## 常见工作流

### 功能开发工作流

```bash
# 1. 从主分支拉取最新代码
git checkout main
git pull origin main

# 2. 创建功能分支
git checkout -b feature/新功能名称

# 3. 开发并提交代码
git add .
git commit -m "feat: 添加新功能"

# 4. 推送功能分支到 Gitee
git push -u origin feature/新功能名称

# 5. 在 Gitee 上发起 Pull Request，请求合并到主分支
```

### 同步 Fork 仓库

```bash
# 添加原始仓库为上游
git remote add upstream git@gitee.com:原作者/仓库名.git

# 拉取上游更新
git fetch upstream

# 将上游主分支合并到本地主分支
git checkout main
git merge upstream/main

# 推送到你的 Fork 仓库
git push origin main
```

---

## 常见问题

**Q: 推送时提示 `rejected` 错误？**

A: 通常是因为远程有你本地没有的提交。先执行 `git pull` 合并后再推送。

**Q: 如何忽略不想提交的文件？**

A: 在项目根目录创建 `.gitignore` 文件，写入需要忽略的文件或目录路径，例如：

```
node_modules/
*.log
.env
dist/
```

**Q: HTTPS 克隆速度慢？**

A: 推荐使用 SSH 方式，或者配置 Git 代理。国内访问 Gitee 通常比 GitHub 快很多。

**Q: 如何查看提交历史？**

```bash
git log --oneline --graph --all
```

**Q: 误提交敏感信息怎么办？**

A: 立即修改密码/密钥，然后使用 `git rebase -i` 清理提交历史，或使用 [`git-filter-repo`](https://github.com/newren/git-filter-repo)（推荐，`git filter-branch` 已废弃）从历史中彻底删除敏感内容，再强制推送。**永远不要将密码、Token 等敏感信息提交到仓库。**

---

## 参考资料

- [Gitee 官方帮助文档](https://gitee.com/help)
- [Git 官方文档](https://git-scm.com/doc)
- [Pro Git 书籍（中文版）](https://git-scm.com/book/zh/v2)
