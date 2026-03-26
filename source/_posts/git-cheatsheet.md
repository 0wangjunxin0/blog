---
title: Git 常用命令速查手册
date: 2026-03-20 09:00:00
categories:
  - 工具
tags:
  - Git
  - 工具
  - 版本控制
cover: https://images.unsplash.com/photo-1618401471353-b98afee0b2eb?w=800&q=80
---

Git 是日常开发中最重要的工具之一。这篇文章整理了工作中最常用的 Git 命令，方便随时查阅。

<!-- more -->

## 基础配置

```bash
# 配置用户信息
git config --global user.name "Your Name"
git config --global user.email "your@email.com"

# 查看配置
git config --list
```

## 仓库操作

```bash
# 初始化仓库
git init

# 克隆远程仓库
git clone https://github.com/user/repo.git

# 克隆指定分支
git clone -b main https://github.com/user/repo.git
```

## 日常工作流

```bash
# 查看状态
git status

# 添加文件到暂存区
git add .             # 添加所有文件
git add file.txt      # 添加指定文件

# 提交
git commit -m "feat: add new feature"

# 推送到远程
git push origin main

# 拉取最新代码
git pull origin main
```

## 分支管理

```bash
# 查看所有分支
git branch -a

# 创建并切换分支
git checkout -b feature/new-feature

# 切换分支
git checkout main

# 合并分支
git merge feature/new-feature

# 删除分支
git branch -d feature/new-feature
```

## 撤销操作

```bash
# 撤销工作区修改（未 add）
git checkout -- file.txt

# 撤销暂存区（已 add，未 commit）
git reset HEAD file.txt

# 撤销最近一次提交（保留修改）
git reset --soft HEAD~1

# 查看提交历史
git log --oneline --graph
```

## 标签管理

```bash
# 打标签
git tag v1.0.0

# 推送标签
git push origin v1.0.0

# 删除标签
git tag -d v1.0.0
```

## Commit 规范

推荐使用 [Conventional Commits](https://www.conventionalcommits.org/) 规范：

| 类型 | 说明 |
|------|------|
| `feat` | 新功能 |
| `fix` | 修复 bug |
| `docs` | 文档变更 |
| `style` | 代码格式（不影响逻辑） |
| `refactor` | 代码重构 |
| `test` | 添加测试 |
| `chore` | 构建/工具变更 |

---

这些命令覆盖了 80% 的日常使用场景。遇到复杂情况，`git --help` 永远是你的朋友。
