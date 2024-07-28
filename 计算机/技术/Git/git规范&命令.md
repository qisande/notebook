### 分支管理

1. **主分支（main/master）**：
   - 该分支始终保持稳定和可发布的状态。
   - 仅在经过充分测试后才将代码合并到主分支。
2. **开发分支（develop）**：
   - 该分支包含最新的开发代码，是一个进行中间集成的分支。
   - 通常，从 `develop` 分支创建特性分支（feature）、修复分支（bugfix）和发布分支（release）。
3. **特性分支（feature）**：
   - 每个新特性或功能在独立的分支上开发。
   - 命名约定：`feature/<description>`，例如 `feature/login-page`。
4. **修复分支（bugfix）**：
   - 用于修复开发过程中发现的错误。
   - 命名约定：`bugfix/<description>`，例如 `bugfix/fix-login-bug`。
5. **发布分支（release）**：
   - 当准备发布一个新的版本时，从 `develop` 分支创建一个发布分支。
   - 命名约定：`release/<version>`，例如 `release/1.0.0`。
6. **热修复分支（hotfix）**：
   - 用于修复生产环境中的紧急问题。
   - 从 `main` 分支创建，修复完成后合并回 `main` 和 `develop` 分支。
   - 命名约定：`hotfix/<description>`，例如 `hotfix/fix-production-bug`。

### 提交信息规范

1. **提交信息格式**：
   - 提交信息应该简洁明了，描述清楚本次提交的目的和变更内容。
   - 通常格式为：`<type>(<scope>): <subject>`，例如 `feat(auth): add login functionality`。
2. **提交类型**：
   - `feat`：新功能（feature）
   - `fix`：修复问题（bug fix）
   - `docs`：文档变更
   - `style`：代码格式（不影响功能，例如空格、格式化、缺少分号等）
   - `refactor`：代码重构（既不是修复 bug 也不是添加功能）
   - `test`：添加或修改测试
   - `chore`：构建过程或辅助工具的变动
3. **提交信息示例**：
   - `feat(auth): add login functionality`
   - `fix(auth): correct login validation error`
   - `docs(readme): update installation instructions`

### 代码审查（Code Review）

1. **拉取请求（Pull Request）**：
   - 所有合并到 `main` 和 `develop` 分支的代码应通过拉取请求（PR）进行。
   - 拉取请求应包含清晰的描述和变更内容的详细说明。
2. **审查过程**：
   - 由至少一个其他开发人员审查和批准。
   - 确保代码质量，遵循编码规范，检查潜在问题和优化建议。
3. **自动化检查**：
   - 配置自动化工具进行静态代码分析、测试和构建检查。

### 其他规范

1. **频繁提交**：
   - 频繁地提交代码，保持提交粒度小，便于回滚和追踪变更。
2. **合并策略**：
   - 使用 `rebase` 或 `merge` 合并分支，根据团队的工作流程选择合适的策略。
   - 避免过多的合并提交，保持提交历史简洁。
3. **标签（Tagging）**：
   - 在发布新版本时，使用标签标记版本号，例如 `v1.0.0`。
4. **Git Hooks**：
   - 配置 Git hooks，确保在提交代码前进行必要的检查和格式化。

### 示例工作流程

1. **创建特性分支**：

   ```bash
   git checkout develop
   git pull origin develop
   git checkout -b feature/login-page
   ```

2. **开发并提交代码**：

   ```bash
   git add .
   git commit -m "feat(auth): add login functionality"
   ```

3. **推送到远程仓库**：

   ```bash
   git push origin feature/login-page
   ```

4. **创建拉取请求**：

   - 在 GitHub 或 GitLab 等平台上创建一个拉取请求，描述本次变更，并请求代码审查。

5. **合并拉取请求**：

   - 审查通过后，将代码合并到 `develop` 分支。

### 常用 git 命令

1. **查看状态**

   ```bash
   git status
   ```

2. **查看分支**

   ```bash
   # 查询本地仓库+远程仓库+跟踪关系
   git branch -vv -a
   
   # 改变跟踪关系
   git branch --set-upstream-to=<shortname>/<remote_branch_name> <local_branch_name>
   
   # 如果你当前已经在 <local_branch_name> 上，可以省略本地分支分支名称：
   git branch --set-upstream-to=<shortname>/<remote_branch_name>
   
   # 列出所有本地分支
   git branch
   
   # 列出所有本地和远程分支
   git branch -a
   
   # 列出所有跟踪关系
   git branch -vv
   
   # 重命名当前分支
   git branch -m <new_branch_name>
   
   # 重命名特定分支
   git branch -m <old_branch_name> <new_branch_name>
   
   # 删除一个本地分支
   git branch -d <branch_name>
   
   # 如果分支没有被合并，可以使用 -D 选项强制删除
   git branch -D <branch_name>
   ```

3. **切换分支**

   ```bash
   切换到现有分支
   git checkout <branch_name>
   
   # 基于当前分支，创建并切换到新分支
   git checkout -b <new_branch_name>
   
   # 基于特定提交，创建新分支
   git checkout -b <new_branch_name> <commit>
   ```

4. **添加到暂存区**

   ```bash
   # 添加单个文件
   git add <file_path>
   
   # 添加多个文件
   git add <file1> <file2> <file3>
   
   # 添加所有更改的文件
   git add .
   ```

5. **提交到本地仓库**

   ```bash
   # 添加提交信息
   git commit -m "<commit_message>"
   ```

6. **远程仓库别名**

   ```bash
   # 查看所有远程仓库的详细信息
   git remote -v
   
   # 添加远程仓库
   # 将远程仓库唯一的URL<url> 映射成为 在本地仓库中对远程仓库起的别名<shortname>
   # 只负责映射！它不会产生下载或上传的流量
   git remote add <shortname> <url>
   
   # 删除远程仓库
   git remote remove <shortname>
   
   # 重命名远程仓库
   git remote rename <old_shortname> <new_shortname>
   
   # 更改远程仓库的 URL
   git remote set-url <shortname> <new_url>
   
   # 查看远程仓库的详细信息
   git remote show <shortname>
   
   # 列出所有远程仓库的引用
   git ls-remote
   
   # 列出名为 origin 的远程仓库的引用
   git ls-remote origin
   ```

7. **推送到远程仓库**

   ```bash
   # 本地分支第一次push
   git push -u <remote_repository_shortname> <local_branch_name>
   
   # 第二次push，当前分支曾经必须被“4大跟踪远程分支命令”远程跟踪到远程仓库的远程分支，可以简写为如下
   git push
   
   # 推送本地分支到远程分支
   git push <remote_repository_shortname> <local_branch_name>:<remote_branch_name>
   
   # 可以省略远程分支名
   # 一般Git使用者会省略<远程分支名>这个参数，所以Git会默认把<本地分支名>设置为<远程分支名>
   git push <remote_repository_shortname> <local_branch_name>
   
   # 删除一个远程分支
   git push <remote_repository_shortname> --delete <remote_branch_name>
   
   # 推送标签
   git push <remote_repository_shortname> <tag_name>
   
   # 强制推送
   git push --force <remote_repository_shortname> <branch_name>
   ```

   ![image-20240726120844564](https://images73.oss-cn-beijing.aliyuncs.com/img/image-20240726120844564.png)

8. **从远程仓库拉取代码**

   ```bash
   # 从远程仓库拉取更改并合并到当前分支
   # 这将从默认的远程仓库（通常是 origin）的当前分支拉取更新并合并到本地当前分支。
   git pull
   
   # 指定远程仓库和分支
   # 从远程仓库 shortname 的 branch_name 分支拉取更新并合并到本地当前分支：
   git pull <remote_repository_shortname> <remote_branch_name>
   ```

9. **合并分支**

   ```bash
   # 将指定分支合并到当前分支
   git merge <branch_name>
   ```

10. **查看变化**

    ```bash
    # 显示工作目录与暂存区之间的差异
    git diff
    
    # 显示暂存区与最新提交之间的差异
    # 显示已暂存但尚未提交的更改。
    git diff --cached
    
    # 显示工作目录与最新提交之间的差异
    git diff HEAD
    
    # 显示两个提交之间的差异
    git diff <commit1> <commit2>
    ```

### 其他 git 命令

- 查看配置信息

  ```bash
  # 设置全局用户名
  git config --global user.name "Your Name"
  
  # 设置全局邮箱
  git config --global user.email "your.email@example.com"
  
  # 设置项目级用户名
  git config user.name "Your Name"
  
  # 设置项目级邮箱
  git config user.email "your.email@example.com"
  
  # 查看当前项目的配置
  git config --list
  
  # 查看全局配置
  git config --global --list
  
  # 查看系统配置
  git config --system --list
  ```

- 初始化本地仓库

  ```bash
  git init
  ```

- 查看日志

  ```bash
  # 显示提交历史
  git log
  
  # 显示简化的提交历史，每个提交占一行
  git log --oneline
  
  # 显示最近的 5 次提交
  git log -5
  
  # 显示最近的 10 次提交
  git log -n 10
  
  # 仅显示提交信息中包含特定关键字的提交
  git log --grep="fix"
  
  # 显示每次提交的统计信息
  git log --stat
  ```

- 回退修改 || 回退添加 || 回退提交 || 回退到xxx提交

  ```bash
  # # 不保留更改
  git checkout .
  # 保留更改
  git reset --mixed
  # 保留更改
  git reset --soft
  
  # 重置到上一个提交，保留更改
  git reset --mixed HEAD~1
  # 重置到上一个提交，保留更改
  git reset --soft HEAD~1
  # 重置到上一个提交，不保留更改
  git reset --hard HEAD~1
  ```