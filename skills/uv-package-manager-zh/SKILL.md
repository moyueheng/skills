---
name: uv-package-manager
description: 精通 uv 包管理器，实现快速的 Python 依赖管理、虚拟环境和现代 Python 项目工作流。用于设置 Python 项目、管理依赖或使用 uv 优化 Python 开发工作流。
---

# UV 包管理器

全面指南：使用 uv（一个用 Rust 编写的极速 Python 包安装器和解析器）进行现代 Python 项目管理和依赖工作流。

## 何时使用此技能

- 快速设置新的 Python 项目
- 比 pip 更快地管理 Python 依赖
- 创建和管理虚拟环境
- 安装 Python 解释器
- 高效解决依赖冲突
- 从 pip/pip-tools/poetry 迁移
- 加速 CI/CD 流水线
- 管理单体仓库 Python 项目
- 使用锁文件实现可重现构建
- 优化包含 Python 依赖的 Docker 构建

## 核心概念

### 1. 什么是 uv？
- **超快速包安装器**：比 pip 快 10-100 倍
- **用 Rust 编写**：利用 Rust 的性能优势
- **pip 的直接替代品**：兼容 pip 工作流
- **虚拟环境管理器**：创建和管理虚拟环境
- **Python 安装器**：下载和管理 Python 版本
- **解析器**：高级依赖解析
- **锁文件支持**：可重现的安装

### 2. 主要特性
- 极快的安装速度
- 通过全局缓存节省磁盘空间
- 兼容 pip、pip-tools、poetry
- 全面的依赖解析
- 跨平台支持（Linux、macOS、Windows）
- 安装时无需 Python
- 内置虚拟环境支持

### 3. UV 与传统工具对比
- **vs pip**：快 10-100 倍，更好的解析器
- **vs pip-tools**：更快、更简单、更好的用户体验
- **vs poetry**：更快、少一些主观性、更轻量
- **vs conda**：更快、专注于 Python

## 安装

### 快速安装

```bash
# macOS/Linux
curl -LsSf https://astral.sh/uv/install.sh | sh

# Windows (PowerShell)
powershell -c "irm https://astral.sh/uv/install.ps1 | iex"

# 使用 pip（如果已有 Python）
pip install uv

# 使用 Homebrew (macOS)
brew install uv

# 使用 cargo（如果有 Rust）
cargo install --git https://github.com/astral-sh/uv uv
```

### 验证安装

```bash
uv --version
# uv 0.x.x
```

## 快速开始

### 创建新项目

```bash
# 创建带虚拟环境的新项目
uv init my-project
cd my-project

# 或在当前目录创建
uv init .

# init 命令会创建：
# - .python-version (Python 版本)
# - pyproject.toml (项目配置)
# - README.md
# - .gitignore
```

### 安装依赖

```bash
# 安装包（需要时会创建 venv）
uv add requests pandas

# 安装开发依赖
uv add --dev pytest black ruff

# 从 requirements.txt 安装
uv pip install -r requirements.txt

# 从 pyproject.toml 安装
uv sync
```

## 虚拟环境管理

### 模式 1：创建虚拟环境

```bash
# 使用 uv 创建虚拟环境
uv venv

# 使用特定 Python 版本创建
uv venv --python 3.12

# 使用自定义名称创建
uv venv my-env

# 创建包含系统站点包的环境
uv venv --system-site-packages

# 指定位置
uv venv /path/to/venv
```

### 模式 2：激活虚拟环境

```bash
# Linux/macOS
source .venv/bin/activate

# Windows (命令提示符)
.venv\Scripts\activate.bat

# Windows (PowerShell)
.venv\Scripts\Activate.ps1

# 或使用 uv run（无需激活）
uv run python script.py
uv run pytest
```

### 模式 3：使用 uv run

```bash
# 运行 Python 脚本（自动激活 venv）
uv run python app.py

# 运行已安装的 CLI 工具
uv run black .
uv run pytest

# 使用特定 Python 版本运行
uv run --python 3.11 python script.py

# 传递参数
uv run python script.py --arg value
```

## 包管理

### 模式 4：添加依赖

```bash
# 添加包（添加到 pyproject.toml）
uv add requests

# 添加带版本约束的包
uv add "django>=4.0,<5.0"

# 添加多个包
uv add numpy pandas matplotlib

# 添加开发依赖
uv add --dev pytest pytest-cov

# 添加可选依赖组
uv add --optional docs sphinx

# 从 git 添加
uv add git+https://github.com/user/repo.git

# 从 git 添加特定版本
uv add git+https://github.com/user/repo.git@v1.0.0

# 从本地路径添加
uv add ./local-package

# 添加可编辑的本地包
uv add -e ./local-package
```

### 模式 5：移除依赖

```bash
# 移除包
uv remove requests

# 移除开发依赖
uv remove --dev pytest

# 移除多个包
uv remove numpy pandas matplotlib
```

### 模式 6：升级依赖

```bash
# 升级特定包
uv add --upgrade requests

# 升级所有包
uv sync --upgrade

# 升级包到最新版本
uv add --upgrade requests

# 显示将要升级的内容
uv tree --outdated
```

### 模式 7：锁定依赖

```bash
# 生成 uv.lock 文件
uv lock

# 更新锁文件
uv lock --upgrade

# 锁定但不安装
uv lock --no-install

# 锁定特定包
uv lock --upgrade-package requests
```

## Python 版本管理

### 模式 8：安装 Python 版本

```bash
# 安装 Python 版本
uv python install 3.12

# 安装多个版本
uv python install 3.11 3.12 3.13

# 安装最新版本
uv python install

# 列出已安装版本
uv python list

# 查找可用版本
uv python list --all-versions
```

### 模式 9：设置 Python 版本

```bash
# 为项目设置 Python 版本
uv python pin 3.12

# 这会创建/更新 .python-version 文件

# 为命令使用特定 Python 版本
uv --python 3.11 run python script.py

# 使用特定版本创建 venv
uv venv --python 3.12
```

## 项目配置

### 模式 10：使用 uv 的 pyproject.toml

```toml
[project]
name = "my-project"
version = "0.1.0"
description = "我的精彩项目"
readme = "README.md"
requires-python = ">=3.8"
dependencies = [
    "requests>=2.31.0",
    "pydantic>=2.0.0",
    "click>=8.1.0",
]

[project.optional-dependencies]
dev = [
    "pytest>=7.4.0",
    "pytest-cov>=4.1.0",
    "black>=23.0.0",
    "ruff>=0.1.0",
    "mypy>=1.5.0",
]
docs = [
    "sphinx>=7.0.0",
    "sphinx-rtd-theme>=1.3.0",
]

[build-system]
requires = ["hatchling"]
build-backend = "hatchling.build"

[tool.uv]
dev-dependencies = [
    # 由 uv 管理的额外开发依赖
]

[tool.uv.sources]
# 自定义包源
my-package = { git = "https://github.com/user/repo.git" }
```

### 模式 11：在现有项目中使用 uv

```bash
# 从 requirements.txt 迁移
uv add -r requirements.txt

# 从 poetry 迁移
# 已有 pyproject.toml，只需使用：
uv sync

# 导出为 requirements.txt
uv pip freeze > requirements.txt

# 导出包含哈希的文件
uv pip freeze --require-hashes > requirements.txt
```

## 高级工作流程

### 模式 12：单体仓库支持

```bash
# 项目结构
# monorepo/
#   packages/
#     package-a/
#       pyproject.toml
#     package-b/
#       pyproject.toml
#   pyproject.toml (根目录)

# 根目录 pyproject.toml
[tool.uv.workspace]
members = ["packages/*"]

# 安装所有工作空间包
uv sync

# 添加工作空间依赖
uv add --path ./packages/package-a
```

### 模式 13：CI/CD 集成

```yaml
# .github/workflows/test.yml
name: 测试

on: [push, pull_request]

jobs:
  test:
    runs-on: ubuntu-latest

    steps:
      - uses: actions/checkout@v4

      - name: 安装 uv
        uses: astral-sh/setup-uv@v2
        with:
          enable-cache: true

      - name: 设置 Python
        run: uv python install 3.12

      - name: 安装依赖
        run: uv sync --all-extras --dev

      - name: 运行测试
        run: uv run pytest

      - name: 运行代码检查
        run: |
          uv run ruff check .
          uv run black --check .
```

### 模式 14：Docker 集成

```dockerfile
# Dockerfile
FROM python:3.12-slim

# 安装 uv
COPY --from=ghcr.io/astral-sh/uv:latest /uv /usr/local/bin/uv

# 设置工作目录
WORKDIR /app

# 复制依赖文件
COPY pyproject.toml uv.lock ./

# 安装依赖
RUN uv sync --frozen --no-dev

# 复制应用代码
COPY . .

# 运行应用
CMD ["uv", "run", "python", "app.py"]
```

**优化的多阶段构建：**

```dockerfile
# 多阶段 Dockerfile
FROM python:3.12-slim AS builder

# 安装 uv
COPY --from=ghcr.io/astral-sh/uv:latest /uv /usr/local/bin/uv

WORKDIR /app

# 安装依赖到 venv
COPY pyproject.toml uv.lock ./
RUN uv sync --frozen --no-dev --no-editable

# 运行时阶段
FROM python:3.12-slim

WORKDIR /app

# 从构建阶段复制 venv
COPY --from=builder /app/.venv .venv
COPY . .

# 使用 venv
ENV PATH="/app/.venv/bin:$PATH"

CMD ["python", "app.py"]
```

### 模式 15：锁文件工作流程

```bash
# 创建锁文件 (uv.lock)
uv lock

# 从锁文件安装（确切版本）
uv sync --frozen

# 更新锁文件但不安装
uv lock --no-install

# 在锁中升级特定包
uv lock --upgrade-package requests

# 检查锁文件是否最新
uv lock --check

# 导出锁文件为 requirements.txt
uv export --format requirements-txt > requirements.txt

# 导出包含哈希的文件（用于安全）
uv export --format requirements-txt --hash > requirements.txt
```

## 性能优化

### 模式 16：使用全局缓存

```bash
# UV 自动使用全局缓存，位置：
# Linux: ~/.cache/uv
# macOS: ~/Library/Caches/uv
# Windows: %LOCALAPPDATA%\uv\cache

# 清理缓存
uv cache clean

# 检查缓存大小
uv cache dir
```

### 模式 17：并行安装

```bash
# UV 默认并行安装包

# 控制并行度
uv pip install --jobs 4 package1 package2

# 不使用并行（顺序）
uv pip install --jobs 1 package
```

### 模式 18：离线模式

```bash
# 仅从缓存安装（无网络）
uv pip install --offline package

# 离线从锁文件同步
uv sync --frozen --offline
```

## 与其他工具比较

### uv vs pip

```bash
# pip
python -m venv .venv
source .venv/bin/activate
pip install requests pandas numpy
# 约 30 秒

# uv
uv venv
uv add requests pandas numpy
# 约 2 秒（快 10-15 倍）
```

### uv vs poetry

```bash
# poetry
poetry init
poetry add requests pandas
poetry install
# 约 20 秒

# uv
uv init
uv add requests pandas
uv sync
# 约 3 秒（快 6-7 倍）
```

### uv vs pip-tools

```bash
# pip-tools
pip-compile requirements.in
pip-sync requirements.txt
# 约 15 秒

# uv
uv lock
uv sync --frozen
# 约 2 秒（快 7-8 倍）
```

## 常见工作流程

### 模式 19：开始新项目

```bash
# 完整工作流程
uv init my-project
cd my-project

# 设置 Python 版本
uv python pin 3.12

# 添加依赖
uv add fastapi uvicorn pydantic

# 添加开发依赖
uv add --dev pytest black ruff mypy

# 创建结构
mkdir -p src/my_project tests

# 运行测试
uv run pytest

# 格式化代码
uv run black .
uv run ruff check .
```

### 模式 20：维护现有项目

```bash
# 克隆仓库
git clone https://github.com/user/project.git
cd project

# 安装依赖（自动创建 venv）
uv sync

# 安装包含开发依赖
uv sync --all-extras

# 更新依赖
uv lock --upgrade

# 运行应用
uv run python app.py

# 运行测试
uv run pytest

# 添加新依赖
uv add new-package

# 提交更新的文件
git add pyproject.toml uv.lock
git commit -m "添加 new-package 依赖"
```

## 工具集成

### 模式 21：预提交钩子

```yaml
# .pre-commit-config.yaml
repos:
  - repo: local
    hooks:
      - id: uv-lock
        name: uv lock
        entry: uv lock
        language: system
        pass_filenames: false

      - id: ruff
        name: ruff
        entry: uv run ruff check --fix
        language: system
        types: [python]

      - id: black
        name: black
        entry: uv run black
        language: system
        types: [python]
```

### 模式 22：VS Code 集成

```json
// .vscode/settings.json
{
  "python.defaultInterpreterPath": "${workspaceFolder}/.venv/bin/python",
  "python.terminal.activateEnvironment": true,
  "python.testing.pytestEnabled": true,
  "python.testing.pytestArgs": ["-v"],
  "python.linting.enabled": true,
  "python.formatting.provider": "black",
  "[python]": {
    "editor.defaultFormatter": "ms-python.black-formatter",
    "editor.formatOnSave": true
  }
}
```

## 故障排除

### 常见问题

```bash
# 问题：找不到 uv
# 解决：添加到 PATH 或重新安装
echo 'export PATH="$HOME/.cargo/bin:$PATH"' >> ~/.bashrc

# 问题：Python 版本错误
# 解决：显式锁定版本
uv python pin 3.12
uv venv --python 3.12

# 问题：依赖冲突
# 解决：检查解析过程
uv lock --verbose

# 问题：缓存问题
# 解决：清理缓存
uv cache clean

# 问题：锁文件不同步
# 解决：重新生成
uv lock --upgrade
```

## 最佳实践

### 项目设置

1. **始终使用锁文件**确保可重现性
2. **使用 .python-version 锁定 Python 版本**
3. **将开发依赖与生产依赖分离**
4. **使用 uv run**而不是手动激活 venv
5. **将 uv.lock 提交到版本控制**
6. **在 CI 中使用 --frozen**确保一致构建
7. **利用全局缓存**提高速度
8. **对单体仓库使用工作空间**
9. **为兼容性导出 requirements.txt**
10. **保持 uv 更新**以获取最新功能

### 性能提示

```bash
# 在 CI 中使用冻结安装
uv sync --frozen

# 尽可能使用离线模式
uv sync --offline

# 并行操作（自动）
# uv 默认这样做

# 跨环境重用缓存
# uv 全局共享缓存

# 使用锁文件跳过解析
uv sync --frozen  # 跳过解析
```

## 迁移指南

### 从 pip + requirements.txt 迁移

```bash
# 之前
python -m venv .venv
source .venv/bin/activate
pip install -r requirements.txt

# 之后
uv venv
uv pip install -r requirements.txt
# 或更好的方式：
uv init
uv add -r requirements.txt
```

### 从 Poetry 迁移

```bash
# 之前
poetry install
poetry add requests

# 之后
uv sync
uv add requests

# 保留现有 pyproject.toml
# uv 会读取 [project] 和 [tool.poetry] 部分
```

### 从 pip-tools 迁移

```bash
# 之前
pip-compile requirements.in
pip-sync requirements.txt

# 之后
uv lock
uv sync --frozen
```

## 命令参考

### 基本命令

```bash
# 项目管理
uv init [PATH]              # 初始化项目
uv add PACKAGE              # 添加依赖
uv remove PACKAGE           # 移除依赖
uv sync                     # 安装依赖
uv lock                     # 创建/更新锁文件

# 虚拟环境
uv venv [PATH]              # 创建 venv
uv run COMMAND              # 在 venv 中运行

# Python 管理
uv python install VERSION   # 安装 Python
uv python list              # 列出已安装的 Python
uv python pin VERSION       # 锁定 Python 版本

# 包安装（pip 兼容）
uv pip install PACKAGE      # 安装包
uv pip uninstall PACKAGE    # 卸载包
uv pip freeze               # 列出已安装的包
uv pip list                 # 列出包

# 工具
uv cache clean              # 清理缓存
uv cache dir                # 显示缓存位置
uv --version                # 显示版本
```

## 资源

- **官方文档**：https://docs.astral.sh/uv/
- **GitHub 仓库**：https://github.com/astral-sh/uv
- **Astral 博客**：https://astral.sh/blog
- **迁移指南**：https://docs.astral.sh/uv/guides/
- **与其他工具比较**：https://docs.astral.sh/uv/pip/compatibility/

## 最佳实践总结

1. **所有新项目使用 uv** - 从 `uv init` 开始
2. **提交锁文件** - 确保可重现构建
3. **锁定 Python 版本** - 使用 .python-version
4. **使用 uv run** - 避免手动激活 venv
5. **利用缓存** - 让 uv 管理全局缓存
6. **在 CI 中使用 --frozen** - 确切重现
7. **保持 uv 更新** - 快速发展的项目
8. **使用工作空间** - 适用于单体仓库项目
9. **为兼容性导出** - 需要时生成 requirements.txt
10. **阅读文档** - uv 功能丰富且不断演进