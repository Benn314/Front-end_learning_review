# Git快速配置教学

## 配置 Git SSH

当我们拿到一台新电脑的时候，应该如何快速将Git跟我们远程Github仓库进行绑定呢？

1. [前往Git官网进行下载](https://git-scm.com/)（Mac OS下载二进制文件即可）

2. 本地 Git 配置

   1. 建议与旧电脑的 Git 配置保持一致，查找用户名和邮箱📮

      ```bash
      # 在旧电脑查看已配置的 Git 信息
      git config --list
      
      # 在新电脑配置 Git 用户名和邮箱
      git config --global user.name "Your Name"
      git config --global user.email "your.email@example.com"
      ```

   2. 生成 SSH 密钥

      ```bash
      ssh-keygen -t rsa -b 4096 -C "your.email@example.com"
      ```

      将 `"your.email@example.com"` 替换为你的邮箱地址。按照提示一路回车，使用默认配置生成 SSH 密钥

3. 添加 SSH 到 Github

   1. 复制生成的 SSH 公钥

      ```bash
      pbcopy < ~/.ssh/id_rsa.pub
      ```

   2. 登录到你的 GitHub 帐户，在顶部导航栏中选择 "Settings"
   3. 在左侧菜单中选择 "SSH and GPG keys"
   4. 点击 "New SSH key"
   5. 在 "Title" 输入一个描述性的标题，例如 "Mac SSH Key"
   6. 在 "Key" 文本框中粘贴刚才复制的 SSH 公钥
   7. 点击 "Add SSH key"

4. 验证 SSH 链接是否成功

   ```bash
   ssh -T git@github.com
   ```

   如果连接成功，会显示一条欢迎消息

​	

## 本地仓库绑定远程仓库（便捷版）

- 新建远程仓库
- `git clone 项目地址`（选择git的地址，不要选择http版本）
- 将本地仓库的文件移动到clone下来的项目中（如果你的公私🔑绑定了，remote这一步不用做）
- 打开 `VSCode` ，进入clone下来的项目中，在源代码管理中直接填写 commit 信息，打开提交按钮右边的下拉更多标识，直接点击 `提交和推送`
- 插件下载：建议在 `VSCode` 中可以下载 `Gitlens插件`，写代码的时候有提示Git历史提交信息并且网络不好的时候可以增加push的成功率（强制提交，不过尽量不要强制提交～）

​	

## 本地仓库绑定远程仓库（传统版）

> 下面演示的是Win的操作，不过跟Mac大差不差（Mac直接在 `终端` 操作就好了，无需 `git bash`）

1. 在本地仓库右键选择 `git bash ` 进入bash命令行工具

2. 执行 `git init` 使仓库变成一个git仓库

3. 如果你远程仓库是 `main` 分支，本地 `git init` 后是 `master` 分支，需要你先切换分支，执行 `git checkout main` 切换到 `main` 分支，如果提示没有该分支，可以执行 `git branch -M main` 将 `master` 分支直接重命名为 `main` 分支

   1. `git branch`：查看当前仓库中有哪些分支，在列表中，当前活动的分支将以 `*` 标记
   2. `git branch -r`：查看远程仓库中的分支列表

4. 将本地仓库与远程仓库绑定，执行 `git remote add origin git@github.com:<你的用户名>/<仓库名>.git` (SSH)

5. 绑定成功后，从 `main` 分支拉去License文件到本地仓库，使用 `git pull origin main` 指令(第一次拉取的话加上origin main)

6. 拉取成功后，上传自己本地仓库的文件到远程仓库中（你可以新建. gitignore 文件来剔除你不想要上传的文件），第一次上传执行以下指令进行上传
 ```bash
 git add . # . 为添加所有文件，你也可以添加指定1文件
 git commit -m "First commit" # 上传的message信息（就是左边的"First commit"）不得为空，
 git push -u origin main 
 # 第一次上传需要加上 -u origin main 之后上传只需要在指定分支执行 git push即可
 ```

7. 检测一下你的Github是否真的上传成功，之后上传的话可以直接在 VScode 中进行

​	

## 面试题

### git中merge和rebase区别

在Git中，merge（合并）和rebase（变基）是两种常用的分支合并策略。它们的主要区别在于合并的方式和结果。

1. Merge（合并）：
   - 合并操作会**创建一个新的合并提交**（merge commit），**将两个或多个分支的修改合并到一个新的分支或当前分支**。
   - 合并会保留各个分支的提交历史，合并提交会包含所有合并的分支的改动，并保留每个分支的独立性。
   - 合并操作适合用于多人协作的项目或在需要保留分支独立性的情况下。
2. Rebase（变基）：
   - 变基操作**将当前分支的提交移动到另一个分支的末端**，通过重新应用提交来整合分支的修改。
   - 变基会重写提交历史，创建新的提交对象，并将当前分支的提交应用到目标分支的末端。
   - 变基操作可以**使提交历史更加线性和整洁**，**减少合并提交的数量**。
   - 变基操作适合用于个人开发或需要保持整洁的提交历史的项目。

选择使用merge还是rebase取决于项目的需求和团队的开发流程。**在多人协作时，merge更常见**，而**在个人开发或维护干净的提交历史时，rebase更常用**。需要注意的是，**变基操作会改变提交历史**，因此在与他人共享分支时需要小心使用，以免引起混淆或冲突。

​	

## 未完待续...

以下知识点请自行学习🧐

分支：

- 切换分支
- 拉取远程分支
- 上传本地分支

冲突：

- 本地和云端发生冲突怎么解决🤔️
- 提交错误信息到远程仓库，我应该如何撤销？

提交信息：

- 提交信息一定要写清楚做了什么呀！
  - `git commit -m “提交信息”`

拉取：

- `git pull` 再一起与本地修改文件一起提交