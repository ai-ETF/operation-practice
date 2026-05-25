# operation-practice

这个仓库用于团队成员练习 Git 协作流程，采用 Git Flow 分支管理策略。

详细的分支保护规则请查看 [BRANCH_RULES.md](BRANCH_RULES.md)。
推荐阅读: [git 是什么？](https://git-scm.com/book/zh/v2/%E8%B5%B7%E6%AD%A5-Git-%E6%98%AF%E4%BB%80%E4%B9%88%EF%BC%9F)

## 1 第一次开发（本地还没有仓库）

- 克隆远程仓库到本地

```bash
# 如果没有配置 ssh 使用：
git clone https://github.com/ai-ETF/operation-practice.git
# 如果配置了 ssh：
git clone git@github.com:ai-ETF/operation-practice.git 
```

```bash
cd operation-practice # 进入仓库目录
git checkout dev # 切换到 dev 分支
git pull origin dev # 拉取远程 dev 分支的最新代码
```

- 由于默认分支就是 `dev` 分支，所以拉取下来就直接再 `dev` 分支上。

## 2 每次开发的完整流程

### 2.1 拉取最新代码并创建功能分支

```bash
git checkout dev # 切换到 dev 分支
git pull origin dev # 拉取远程 dev 分支的最新代码
git checkout -b feature/你的功能名 # 基于当前分支创建并切换到新的功能分支
```

> ⚠️ 先创建分支再写代码，不要在 dev 上直接改代码。

### 2.2 写代码，然后提交

```bash
git add . # 添加所有修改的文件到暂存区
git commit -m "feat: 简要描述你做了什么" # 将暂存区的修改提交到本地仓库
```

这步虽然很简单，commit message 要写好却很难，所以要求，统一使用 `/git-commit`这条 Claude command 来完成这个步骤。

- 在 claude 对话窗中输入：

```claude
/git-commit 
```


### 2.3 推送到远程仓库

```bash
git push -u origin feature/你的功能名 # 将本地功能分支推送到远程仓库，并设置上游跟踪分支
```

- 【写代码 -> 提交】这个步骤可以反复多次，待到相对满意了再推送到远程仓库


### 2.4 提交 Pull Request

1. 打开 GitHub 仓库页面：`https://github.com/ai-ETF/operation-practice`
2. 点击 **Pull requests** 页面，点击 **New pull request**
![alt text](image.png)
3. **base 选择 `dev`**，compare 选择你的功能分支
![alt text](image-1.png)
4. 填写标题和说明，点击 **Create pull request**
5. 等待队友审核通过后合并



### 2.5 清理本地分支

PR 合并后，删除本地的功能分支：

```bash
git checkout dev # 切换回 dev 分支
git branch -d feature/你的功能名 # 删除本地的功能分支
```

- 下次开发注意一定要执行：

```bash
git pull origin dev # 拉取远程 dev 分支的最新代码
```