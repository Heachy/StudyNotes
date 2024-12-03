# Git 概述

**目录**

1. [Git操作](git/git操作)
2. [Action](git/action)

**简介**

Git 是一个分布式版本控制系统，用于有效、高速地处理从小到大的项目版本管理。

**安装**

- **Linux/Mac**: 通过包管理器安装，例如 `sudo apt-get install git`。
- **Windows**: 下载并安装 [Git for Windows](https://gitforwindows.org/)。

**配置**

```shell
git config --global user.name "Your Name"
git config --global user.email "youremail@example.com"
```

**常用命令**

- **初始化仓库**:
  ```shell
  git init
  ```

- **克隆远程仓库**:
  ```shell
  git clone [url]
  ```

- **添加文件到暂存区**:
  ```shell
  git add .
  ```

- **提交更改**:
  ```shell
  git commit -m "commit message"
  ```

- **推送到远程仓库**:
  ```shell
  git push origin main
  ```

- **拉取远程仓库的更改**:
  ```shell
  git pull origin main
  ```

- **查看分支**:
  ```shell
  git branch
  ```

- **创建并切换到新分支**:
  ```shell
  git checkout -b new-branch
  ```

**GitHub Action**

GitHub Action 是一个自动化工具，它允许你自动执行软件开发流程。

**基本流程**

1. **定义工作流**: 在你的仓库中创建一个 `.github/workflows` 目录，并在其中创建一个 YAML 文件来定义你的工作流。
2. **触发工作流**: 工作流可以由各种事件触发，如推送代码、打开 PR 或者定时执行。
3. **执行任务**: 工作流中的每个任务都在隔离的环境中运行，可以执行测试、部署代码等操作。

**示例工作流**

```yaml
name: CI

on: [push]

jobs:
  build:
    runs-on: ubuntu-latest
    steps:
    - uses: actions/checkout@v2
    - name: Run a one-line script
      run: echo Hello, world!
```

这个工作流在每次推送到仓库时都会运行，执行一个简单的脚本。

---

以上就是 Git 的基本操作和 GitHub Action 的简单介绍。希望这能帮助你更好地管理和自动化你的代码开发流程。
