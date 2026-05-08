---
title: "Discrete mathematics"
date: 2021-12-26
---

# Josephus 环

$f(n,k) = (f(n-1,k) + k) % n$

``` python
from functools import cache
@cache
def j(n, k):
    if n == 1:
        return 1
    ans = j(n-1,k) + k
    if ans < n :
        return ans
    return ans - n

return j(8,3)
```

```text
7
```
