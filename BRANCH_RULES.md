# 分支保护规则说明

`main` 和 `dev` 是团队共用的分支。如果有人直接在上面改代码，很容易覆盖别人的工作，导致代码丢失或冲突。所以我们设了两条规则：**所有改动必须通过 PR（Pull Request）提交，且至少需要 1 人审核通过才能合并。**

## 简单理解

```
❌ 不能做的事：直接往 main 或 dev 上写代码、推送代码
✅ 正确的做法：在自己的功能分支上写，然后提 PR 让别人审核后再合并
```

## 分支总览

我们采用 **Git Flow** 分支管理策略，团队中存在三种角色不同的分支：

- **main 分支**：存放经过验证的稳定代码，对应线上可发布的状态。这个分支上的每一行代码都经过了完整的审核流程，永远不要直接在上面修改。
- **dev 分支**：开发集成分支，所有功能开发完成后先合到这里。dev 上的代码是"最新但可能还在测试中"的版本，是每个人创建功能分支的起点。
- **feature/xxx 分支**：你个人的临时工作分支，从 dev 拉出来，完成后通过 PR 合回 dev。命名示例：`feature/data-fetch`、`feature/user-login`、`feature/sing`。合并审核通过后会被删除。

三者之间的关系：`main ← dev ← feature/xxx`，代码从右往左流动，每一步都需要 PR 审核。

| 分支 | 用途 | 保护 | 谁能直接 push |
|------|------|------|-------------|
| `main` | 稳定的线上代码 | 严格保护 | 无人可以 |
| `dev` | 开发集成分支 | 基础保护 | 无人可以 |
| `feature/*` | 个人功能分支 | 无保护 | 创建者自由 push |

## 你每次开发的完整流程

```bash
# 1. 从 dev 拉取最新代码
git checkout dev
git pull origin dev

# 2. 先创建功能分支，再写代码！（不要在 dev 上写代码）
#    分支名用 feature/功能描述，如 feature/add-readme
git checkout -b feature/add-readme

# 3. 写代码...写完后提交
git add .
git commit -m "feat: 添加了说明文档"

# 4. 推送你的分支到远程
git push -u origin feature/add-readme

# 5. 去 GitHub 页面提 PR：feature/add-readme → dev
#    等队友审核通过后合并，合并后删除这个功能分支
```

> ⚠️ **为什么必须先切分支再写代码？**
> 在 dev 上写代码容易误提交到 dev 分支（虽然 push 会被拦截，但本地 dev 会被污染，后续操作容易混乱）。养成"先切分支再动手"的习惯可以避免这个问题。

## PR 审核说明

- 合并到 `main` 或 `dev` 的 PR 需要 **至少 1 人审核通过** 才能合并
- 如果 PR 提交后又有新的 commit push 上去，之前的审核会自动失效，需要重新审核
- PR 必须基于目标分支（开发的话是 dev 分支）的最新代码，如果有冲突需要先解决



## 常见问题

**Q: 我 push 时报错 "protected branch hook declined"？**
A: 你正在尝试直接推送到 main 或 dev，请切回自己的功能分支再 push。

**Q: PR 审核被驳回了怎么办？**
A: 根据审核意见修改代码，重新 commit 并 push，PR 会自动更新。

**Q: 我的 PR 有冲突怎么办？**
A: 在本地把 dev 的最新代码合并进来，解决冲突后重新 push：
```bash
git checkout feature/你的分支名
git pull origin dev          # 拉取 dev 最新代码
# 解决冲突后
git add .
git commit -m "fix: 解决冲突"
git push
```