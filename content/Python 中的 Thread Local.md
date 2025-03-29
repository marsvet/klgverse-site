---
evl-share: "true"
---
## threading.local()

在 Python 中，标准库提供了 [[./Thread Local|Thread Local]] 能力：`threading.local()`
但这个只能用于传统的多线程编程，无法用于协程。

## flask 中的 Thread Local

在 flask 中，可以通过 g 来在同一请求的不同函数中共享数据，这其实也是 Thread Local。
当然，除了 g，flask 还在很多地方用到了 Thread Local，比如 request。

不过 flask 不直接使用 threading.local()，而是自己实现，除了支持线程还支持 Greenlet 协程。

[python - flask 上下文的实现 - \_Zhao - SegmentFault 思否](https://segmentfault.com/a/1190000004223296)

## contextvars

上面说了，threading.local() 不支持协程，所以 Python3.7 中增加了 contextvars。
具体内容参见：[[./Python 标准库 - contextvars|Python 标准库 - contextvars]]
