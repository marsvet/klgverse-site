---
tags: python/标准库
evl-share: "true"
---
contextvars 是 Python3.7 中增加的，支持协程，相当于 `threading.local()` 的升级版。
这个模块不仅仅给 aio 加入了 Context 的支持，也用来替代 `threading.local()`。

文档：[contextvars --- 上下文变量 — Python 3.11.4 文档](https://docs.python.org/zh-cn/3/library/contextvars.html)

基本使用：
```python
import asyncio
import contextvars

# 不同协程、线程中使用 client_addr_var 时，值是不一样的
# 但同一个协程、线程中获取到的值是一样的
client_addr_var = contextvars.ContextVar('client_addr')


def render_goodbye():
    # The address of the currently handled client can be accessed
    # without passing it explicitly to this function.

    client_addr = client_addr_var.get()    # 获取值
    return f'Good bye, client @ {client_addr}\n'.encode()


async def handle_request(reader, writer):
    addr = writer.transport.get_extra_info('socket').getpeername()
    client_addr_var.set(addr)              # 设置值

    # In any code that we call is now possible to get
    # client's address by calling 'client_addr_var.get()'.

    while True:
        line = await reader.readline()
        print(line)
        if not line.strip():
            break
        writer.write(line)

    writer.write(render_goodbye())
    writer.close()


async def main():
    srv = await asyncio.start_server(
        handle_request, '127.0.0.1', 8081)

    async with srv:
        await srv.serve_forever()


asyncio.run(main())

# To test it you can use telnet:
#     telnet 127.0.0.1 8081
```

FastAPI 使用 contextvars 可以实现类似 flask 的 g 的效果，但没有必要，显式大于隐式，还是显式传参更好。

contextvars 底层实现原理：[Python使用contextvars模块传递上下文的底层原理\_python contextvar\_聆听--风雨的博客-CSDN博客](https://blog.csdn.net/luchengtao11/article/details/126442670)

在 Python3.6 中使用 contextvars：
contextvars 实现了 PEP 567, 如果在 Python3.6 想使用可以用 [MagicStack/contextvars](https://github.com/MagicStack/contextvars) 这个向后移植库，它和标准库都是同一个作者写的，可以放心使用。
需要 pip 安装：`pip install contextvars`
