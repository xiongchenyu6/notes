---
title: "cache"
id: "658943fb-d978-4bac-be4c-835eb7086311"
date: "2021-12-27 Mon 21:28"
---

# CPU

- write back
- write through

# Cache aside

```plantuml
@startuml
!pragma useVerticalIf on
start
if (request) is (write) then
:update database;
:invalid cache;
else (read)
if (Whether hit the cache?) is (Yes) then
:read cache;
else (No)
:read db;
:update cache;
endif
#00FF00:return data;
endif
stop
@enduml
```

![](i/cache-ahead.jpg)

## 缓存不一致补偿

### 业务代码重试

```plantuml
@startuml
!pragma useVerticalIf on
start
:write req;
:update database;
:update cache;
while (delete success?) is (false)
  :add to mq;
  :retry;
endwhile
stop
@enduml
```

![](i/cache-retry.jpg)

### 数据库 binlog 消费

### Data Transmission Service

云服务商的同步

# Read through

```plantuml
@startuml
!pragma useVerticalIf on
start
:read request;
partition  #lightGreen "Acess Layer" {
if (Whether hit the cache?) is (Yes) then
:read cache;
else (No)
:read db;
:update cache;
endif
#00FF00:return data;
}
stop
@enduml
```

![](i/read-through.jpg)

# Write through

适合写操作较多，并且对一致性要求较高的场景

```plantuml
@startuml
!pragma useVerticalIf on
start
:write request;
partition "access layer" {
:update database;
:update cache;
}
stop
@enduml
```

![](i/write-through.jpg)

# Write behind

电商秒杀

```plantuml
@startuml
!pragma useVerticalIf on
start
:write request;
partition "access layer" {
:update cache;
:async update database;
}
stop
@enduml
```

![](i/write-behind.jpg)

# Write around

```plantuml
@startuml
!pragma useVerticalIf on
start
if (request) is (write) then
:update database;
else (read)
if (Whether hit the cache?) is (Yes) then
:read cache;
else (No)
:read db;
:update cache with timeout;
endif
#00FF00:return data;
endif
stop
@enduml
```

![](i/write-around.jpg)

# conclusion

In scenarios with more reads and less writes, you can choose to use the "Cache-Aside combined with consumer database logs for compensation" solution. For scenarios with more writes, you can choose to use the "Write-Through combined with distributed locks" solution, which is extreme for more writes. In the scene, you can choose to adopt the "Write-Behind" scheme.
