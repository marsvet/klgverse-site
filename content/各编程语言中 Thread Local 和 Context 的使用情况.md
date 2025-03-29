---
evl-share: "true"
---
## Python Web 框架

Python 的 web 框架中，Flask 实现了 [[./Thread Local|Thread Local]]，Django、Sanic、FastAPI 都没有实现，需要显式传参。

Django：
```python
from django.http import HttpResponse

def index(request):
	text = request.GET.get('text')
	return HttpResponse(f'Text is {text}')
```

Sanic：
```python
from sanic import response

app = Sanic()

@app.route('/')
async def index(request):
	text = request.args.get('text')
	return response.text(f'Text is {text}')
```

FastAPI：
```python
from fastapi import Request

@app.get("/")
async def index(request: Request):
	pass
```

虽然这些框架原生不支持 Thread Local，但我们也可以自己用 [[./Python 标准库 - contextvars|Python 标准库 - contextvars]] 实现，但不推荐这样做，显式大于隐式，还是显式传参更好。

## Go

Go 中，直接就没有 Thread Local 这个技术。
Go 传参全都是显式传递 Context。

Go 的设计哲学之一就是显式大于隐式，Go 官方根本不会让 Thread Local（准确来说是 goroutine local）存在，因为 Go 中无法获取到 goroutine id，所以这个技术实现不了。

不过貌似仍然有人通过特殊方法实现了 goroutine local storage：
[GitHub - jtolio/gls: Goroutine local storage](https://github.com/jtolio/gls)
但这种只能学学思路，肯定不能在生产环境使用。

## Java

Java 中主要是使用 ThreadLocal 这个类。

Java 生态我不太了解，貌似现在使用 ThreadLocal 的人也越来越少。
