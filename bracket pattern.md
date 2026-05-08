---
title: "bracket pattern"
date: 2021-12-08
---

一种获取资源后要如果不用或者出错要及时释放的编程模式，简单分享一个下这种模式在各个语言的使用

# [[Haskell]]

[[bracket pattern|*bracket pattern*]]

# C++ RAII

# Python

``` python
class ContextManagedResource:
    def __enter__(self):
        self._resource = get_resource()
        return self._resource

    def __exit__(self, exc_type, exc_value, traceback):
        if exc_type is not None:
            #  handle exception here
            pass

        else:
            pass

        release_resource(self._resource)
        return True

with ContextManagedResource() as res:
    res.method()
    return "good"
```
