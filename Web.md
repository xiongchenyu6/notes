---
title: "Web"
date: 2021-11-20
---

# Broser

## Chrome Safety disable

- **Mac**

``` shell
open -a Google\ Chrome --args --disable-web-security --user-data-dir
```

- **Linux**

``` shell
chromium --args --disable-web-security --user-data-dir &
```

# Safety

## CRSF

- Double submit save both token and refresh token in the cookie and localstorage, and submit it with them both.

## CORS

- Have to be https
- Have to set the Allow ... Header
- Have OPTIONS operator sniffing
