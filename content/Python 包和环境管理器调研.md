---
tags: python
evl-share: "true"
---
最基本的就是 pip + requirements.txt + virtualenv
比较古老，使用最广泛，但不太好用，尤其是 requirements.txt 问题很多

以前还用过 pipenv，不是很好用，现在虽然是 Python 官方推荐的，但用的人没那么多，有很多更优秀的。

其他有很多第三方的包管理器，各有各的好处和问题，根据 Github 上的 Star 数量，目前最常用的是 Poetry。
或许它目前不是最好用的，但它是使用最广泛的，它虽然不完美，但可以打 90 分以上了，好像有其他一两个比它更好用一点，但 Star 没它多。

所以最后就确定用 [Poetry](https://github.com/python-poetry/poetry) 了。
简记：[[./Python Poetry 小记|Python Poetry 小记]]

貌似 [pdm](https://github.com/pdm-project/pdm) 比 Poetry 更好用，但目前 pdm 诞生时间还比较短，Github Star 离 Poetry 还有不小差距，继续观察，继续使用 Poetry，以后如果 pdm 超过 Poetry 再考虑使用它。
pdm 的 scripts 类似 npm scripts，比 Poetry 的 scripts 更灵活，但这不是用 pdm 的理由，不要因为一个小功能的优势就改用 pdm，Poetry 是使用最广泛的，这才是最重要的。

Flask 作者开发了一个 rye，结合依赖管理、环境管理、Python版本管理，可以说是功能最完善的。参考了 rust 的依赖管理、环境管理工具实现的，Star 涨得非常快。
但作者的意思是这个工具主要用于验证思路，还不能算是一个稳定可靠、能投入生产环境的工具，所以还是先不用了，继续观察。