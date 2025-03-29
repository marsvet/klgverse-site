---
evl-share: "true"
---
以 web 框架为例，一个请求进来后，先到了 controller/handler，controller 要调用 service，service 要调用 dao，这种情况下，上下文信息应该如何传递呢？

比如我有一个需求，就是在 controller、service、dao 中记录日志时都记录 request_id，同一个请求的 request_id 应该是相同的。

此时一般有两种实现方式：
1. [[./Thread Local|Thread Local]] 或类似技术（隐式传递 Context）。
2. 像 Golang 那样显式传递 Context。

这两种方式各有优劣：
![[./resource/d9b3a6d0b34c9728d28064cf1f10286c_MD5.png|d9b3a6d0b34c9728d28064cf1f10286c_MD5.png]]
![[./resource/8fbf7b3d67504c3464df2ccc7c6f8f92_MD5.png|8fbf7b3d67504c3464df2ccc7c6f8f92_MD5.png]]

显式大于隐式，为了后期更好维护，应该尽可能显式传递，避免隐式传递。

[[./各编程语言中 Thread Local 和 Context 的使用情况|各编程语言中 Thread Local 和 Context 的使用情况]]
