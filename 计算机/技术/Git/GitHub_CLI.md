**GitHub CLI**（命令行工具，简称 `gh`）是 GitHub 提供的一个命令行工具，可以在命令行中直接与 GitHub 交互，进行如仓库管理、拉取请求处理、问题追踪、发布管理等一系列操作。通过 GitHub CLI，你不需要通过浏览器，所有的 GitHub 操作都可以通过命令行完成。

### **GitHub CLI 的主要功能**

GitHub CLI 工具提供了一些非常有用的命令，可以大大简化日常的 GitHub 操作。以下是 GitHub CLI 的一些常见功能和命令：

#### 1. **管理仓库**

- **克隆仓库**：

  ```bash
  gh repo clone <owner>/<repo>
  ```

  克隆 GitHub 仓库到本地。

- **创建新仓库**：

  ```bash
  gh repo create <repo-name> --public
  ```

  在 GitHub 上创建新仓库，并选择是否公开或私有。

- **查看仓库信息**：

  ```bash
  gh repo view <owner>/<repo>
  ```

  查看仓库的详细信息，如描述、语言、最近活动等。

#### 2. **拉取请求（Pull Requests）**

- **查看拉取请求列表**：

  ```bash
  gh pr list
  ```

  显示当前仓库中所有的拉取请求。

- **创建一个新的拉取请求**：

  ```bash
  gh pr create --base <base-branch> --head <feature-branch> --title "Title" --body "Description"
  ```

  创建一个新的拉取请求，指定目标分支和源分支。

- **查看拉取请求详情**：

  ```bash
  gh pr view <pr-id>
  ```

  查看指定拉取请求的详细信息。

- **合并拉取请求**：

  ```bash
  gh pr merge <pr-id>
  ```

  合并指定的拉取请求。

#### 3. **问题追踪（Issues）**

- **查看问题列表**：

  ```bash
  gh issue list
  ```

  查看当前仓库的所有问题。

- **创建新问题**：

  ```bash
  gh issue create --title "Bug" --body "Details about the bug"
  ```

  创建一个新问题，填写标题和描述。

- **查看问题详情**：

  ```bash
  gh issue view <issue-id>
  ```

  查看指定问题的详细信息。

#### 4. **发布（Releases）**

- **查看发布信息**：

  ```bash
  gh release list
  ```

  查看所有发布版本。

- **创建新发布版本**：

  ```bash
  gh release create v1.0.0 ./path/to/asset.zip --title "First Release" --notes "Initial release"
  ```

  创建一个新的发布版本，并附加文件。

#### 5. **管理和查看 GitHub Actions**

- **查看 GitHub Actions 运行**：

  ```bash
  gh run list
  ```

  查看当前项目的所有 GitHub Actions 运行记录。

- **查看某个运行的详情**：

  ```bash
  gh run view <run-id>
  ```

- **重新触发 GitHub Actions**：

  ```bash
  gh run rerun <run-id>
  ```

#### 6. **管理团队和组织**

- **查看团队列表**：

  ```bash
  gh team list
  ```

  查看与 GitHub 组织相关的所有团队。

- **为团队创建仓库**：

  ```bash
  gh team repo add <team> <repo> --permission <permission>
  ```

  将仓库添加到 GitHub 组织中的某个团队。

#### 7. **GitHub Gist**

- 创建 Gist

  ```bash
  gh gist create <file1> <file2> --public
  ```

  创建公开的 Gist 文件，支持上传多个文件。

------

### **安装 GitHub CLI**

GitHub CLI 可以在 Windows、macOS 和 Linux 上安装，安装步骤如下：

#### 1. **Windows**（通过 PowerShell 或 Chocolatey）

使用 PowerShell 安装：

```bash
winget install --id GitHub.cli
```

或者使用 Chocolatey：

```bash
choco install gh
```

#### 2. **macOS**（通过 Homebrew）

如果你使用 Homebrew，可以运行以下命令：

```bash
brew install gh
```

#### 3. **Linux**（通过包管理器）

对于基于 Debian 的 Linux 发行版（如 Ubuntu）：

```bash
sudo apt install gh
```

对于基于 RPM 的发行版（如 Fedora）：

```bash
sudo dnf install gh
```

你还可以参考 [GitHub CLI 安装文档](https://cli.github.com/) 获取更多的安装方式。

------

### **配置 GitHub CLI**

安装完成后，你需要进行身份验证，以便 GitHub CLI 可以与 GitHub 帐户进行交互。

运行以下命令进行身份验证：

```bash
gh auth login
```

- 选择你要使用的认证方式（GitHub.com 或 GitHub Enterprise）。
- 选择认证方式（浏览器或 GitHub 令牌）。
- 按照提示完成认证流程。

完成认证后，GitHub CLI 将与你的 GitHub 账户连接，你可以开始使用命令行与 GitHub 交互。

------

### **GitHub CLI 常见命令总结**

| 命令                         | 说明                     |
| ---------------------------- | ------------------------ |
| `gh repo clone <repo>`       | 克隆仓库                 |
| `gh repo create <repo-name>` | 创建新仓库               |
| `gh pr list`                 | 查看拉取请求             |
| `gh pr create`               | 创建拉取请求             |
| `gh issue list`              | 查看问题列表             |
| `gh issue create`            | 创建新问题               |
| `gh release create`          | 创建发布                 |
| `gh run list`                | 查看 GitHub Actions 运行 |