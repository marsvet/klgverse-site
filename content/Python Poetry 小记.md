---
tags: python
evl-share: "true"
---
### Poetry 的主要功能

1. 管理依赖，可以用 poetry.lock 锁定依赖
2. 管理虚拟环境（基于 virtualenv）
3. 打包、发布 Python 包

### 安装 Poetry

```bash
pip install poetry
```

### 常用命令

```bash
# 创建新 Poetry 项目
poetry new pkgname

# 已存在的非 Poetry 项目，将其变为一个 Poetry 项目（其实就只是创建一个 pyproject.toml）
poetry init

# 已存在的 Poetry 项目，安装依赖并创建虚拟环境
poetry install

# 在虚拟环境中运行命令
poetry run python main.py

# 包管理
poetry add fastapi@^0.92.0
poetry remove fastapi

# 开始测试
poetry run pytest

# 打包
poetry build

# 发布到 PyPI
poetry publish --username your_username --password your_password
```

### Poetry scripts

poetry scripts 跟 npm scripts 很像。

首先在 pyproject.toml 中定义脚本：
```toml
[tool.poetry.scripts]
dev = "scripts.server:dev"
prod = "scripts.server:prod"
```
然后执行 poetry install
这样就可以用 poetry run 运行了：
```bash
# 运行 scripts.server 模块中的 dev 函数
poetry run dev
```

原理是 poetry install 的时候会将 dev、prod 这些命令安装到虚拟环境的 bin 目录下。

可以看到功能不如 npm scripts 强大，npm scripts 可以定义任意命令比如：`node xxx.js`，而 poetry scripts 只能指定 python 模块中的某个函数。
貌似原因是开发者觉得这超出了 Poetry 的职责，这个功能不应该由 Poetry 实现，具体看：[Improve script section: support use of script files · Issue #2310 · python-poetry/poetry · GitHub](https://github.com/python-poetry/poetry/issues/2310)
PS：另一个包管理工具 pdm 实现了和 npm scripts 一样的功能。[[./Python 包和环境管理工具调研|Python 包和环境管理工具调研]]

典型使用场景：
- **启动开发服务器**： 你可以通过 `poetry run serve` 启动一个开发服务器，而无需每次都输入复杂的 Python 启动命令。
- **管理数据库迁移**： 可以通过自定义命令来执行数据库迁移或其他管理任务。
- **脚本化任务**： 比如，自动化测试、构建、部署等，可以通过脚本定义，然后通过命令行运行，避免手动操作。