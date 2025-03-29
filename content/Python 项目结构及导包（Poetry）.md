---
tags: python
evl-share: "true"
---
项目结构：
```text
pkgname/            # 项目根目录
├── docs/           # 文档
├── charts/         # Helm Charts
├── pkgname/        # 项目主包（与项目同名）
│   ├── __init__.py    # 包的入口
│   ├── common/
│   │   ├── config/
│   │   ├── consts/
│   │   ├── middlewares/
│   │   ├── dependencies.py  # 依赖项，用于依赖注入
│   │   ├── logger.py
│   │   └── __init__.py
│   ├── controller/
│   ├── service/
│   ├── manager/
│   ├── repo/
│   ├── model/
│   │   ├── dto/
│   │   ├── bo/
│   │   └── entity/
│   ├── utils/         # 放置与项目完全无关的工具类函数或模块
│   │   ├── helper.py  # 例如：用于帮助其他模块的工具函数
│   │   └── ...
│   └── ...           
├── tests/             # 测试目录
│   ├── __init__.py    
│   ├── test_module1.py
│   ├── test_module2.py
│   └── ...
├── scripts/           # 项目使用的一些辅助脚本、启动脚本等
│   │   ├── server.py  # 启动命令
│   │   ├── build_plugin.sh
│   │   └── check_env.sh
├── .gitignore
├── Dockerfile
├── Jenkinsfile
├── pyproject.toml     # Poetry 管理的核心配置文件
├── poetry.lock        # 依赖锁定文件（自动生成，不要手动修改）
├── README.md          # 项目简介
└── LICENSE            # 许可证
```

这种项目结构下，导包很容易：
1. 对于第三方包，直接通过包名导入，比如 `from pydantic import BaseModel`，这个没什么说的。
2. pkgname 下的子孙模块互相导入，是通过根包名一直找下去，比如 controller 中某个模块导入 service 中某个模块：`from pkgname.service.xxx import xxx`
3. 某个子孙包，如果包内部的文件互相导入，可以使用相对路径，比如 `pkgname/common/__init__.py` 中导入 `pkgname/common/config/xxx.py` 时：`from .config import xxx`
4. tests 中的模块这样导入主包：`from pkgname.service.xxx import xxx`

这种项目结构，在运行时，当前工作目录需要是项目根目录，以及 IDE 打开时也要打开项目根目录，否则包的导入关系可能会失效。

另外，还有一种常见的目录结构是，在项目主包外加一层 src 目录：
```text
pkgname/            # 项目根目录
├── src/
│   ├── pkgname/    # 项目主包（与项目同名）
│   │   ├── controller/
│   │   ├── service/
│   │   └── ...
├── tests/          # 测试目录
└── ...
```
这样没有上面那种结构方便：
1. 在 IDE 中开发、调试时，需要将 src 设置为搜索路径（PYTHONPATH 环境变量）
2. 启动项目时（无论是直接 python xxx.py 启动，还是启动 uvicorn 等服务器），也需要设置 PYTHONPATH
3. 用 pytest 执行测试时，也需要设置 PYTHONPATH
