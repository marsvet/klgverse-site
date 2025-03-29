---
tags: python
evl-share: "true"
---
最基本的就是 pip + requirements.txt + virtualenv，或者使用 Python 自带的 venv 代替 virtualenv
这种方式比较古老和基础，使用最广泛，但不太好用，尤其是 requirements.txt 问题很多。

以前还用过 pipenv，不是很好用，尤其是 lock，不但很慢而且不可靠，pipenv 的 lock 可能无法完全锁死依赖版本，导致项目在一段时间后跑不起来。
pipenv 虽然是 Python 官方推荐的，但用的人没那么多，有很多更优秀的。

根据 Github 上的 Star 数量，目前 Star 最多的是 uv，其次是 [Poetry](https://github.com/python-poetry/poetry)。
uv 出来的比较晚，功能还不稳定，还没发布 1.0 版本。
因此 Poetry 可以算得上是最常用的了。或许它目前不是最好用的，但它是使用最广泛的，它虽然不完美，但可以打 90 分以上了。

有一个工具 pdm，使用方法跟 Poetry 很像，还吸收了一些 npm 的优势，功能上可以说很好用，不过速度比较慢。
pdm 作者是 PyPa 成员、Pipenv 目前主要的维护者之一，而且是个中国人。
但 pdm 发布几年来，一直不温不火，Star 数不多，可以推测用的人较少。
pdm 的 scripts 类似 npm scripts，比 Poetry 的 scripts 更灵活，但这不是用 pdm 的理由，不要因为一个小功能的优势就改用 pdm，使用者数量更重要。

uv 速度很快，功能强大，不但能管理包和虚拟环境，还可以管理 Python 版本，正在向工程管理工具的方向发展，是一个更高一层的工具。
但 uv 目前功能还不稳定，还没发布 1.0 版本，就暂时不要用在正式项目中了，等发布 1.0 再说。期待 uv 的大一统。
简记：Python 包和项目管理工具 uv

rye 是 flask 作者 mitsuhiko 开发的，他觉得 Python 的研发生态链比起 rust 实在是太烂了，自己搞了一个工具关注整个研发生命周期，使用 独立 Python 解释器 + venv + pip + pyproject.toml 等既有生态来管理。
rye 也是用 Rust 开发的。作者的意思是这个工具主要用于验证思路，还不能算是一个稳定可靠、能投入生产环境的工具。
关于 Python 开发者体验和 rye 本身，mitsuhiko 在 rye 创始之初就有个 discussion ，值得一读：[Should Rye Exist? · astral-sh/rye · Discussion #6 · GitHub](https://github.com/astral-sh/rye/discussions/6)
后来开发 uv 的团队 astral 跟 mitsuhiko 沟通，接管了 rye 项目。
最近 uv 增加了很多 rye 的功能，那么最终 uv 貌似会往代替 rye 的方向发展？maybe，继续跟踪。

综上：
目前还是先使用 Poetry，简记：[[./Python Poetry 小记|Python Poetry 小记]]
然后持续跟踪 uv，等 uv 开发差不多了，考虑开始使用 uv