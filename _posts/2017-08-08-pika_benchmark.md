---
layout: post
title: Pika性能测试
---

## 简述

Pika是360DBA和基础架构组联合开发的类Redis存储系统，完全支持Redis协议。

这次测试的目的是从不同维度展示pika的性能表现。

## 开测

### 测试环境

**CPU型号**：Intel(R) Xeon(R) CPU E5-2690 v4 @ 2.60GHz

**CPU线程数**：56

**MEMORY**：256G

**DISK**：3T flash

**NETWORK**：10000baseT/Full * 2

### 测试一

#### 测试目的

测试在pika不同worker线程数量下，其QPS上限。

#### 测试条件

pika数据容量：800G

value：128字节

#### 测试结果

说明：横轴Pika线程数，纵轴QPS，value为128字节

<img src="/public/images/pika_benchmark/pika_threads_test.png" width="1000px" />

#### 结论

从以上测试图可以看出，pika的worker线程数设置为20-24比较划算。

### 测试二

#### 测试目的

测试在最佳worker线程数（20线程）下，pika的rrt表现。

#### 测试条件

**pika数据容量**：800G

**value**：128字节

#### 测试结果

```c
====== GET ======
  10000000 requests completed in 23.10 seconds
  200 parallel clients
  3 bytes payload
  keep alive: 1
99.89% <= 1 milliseconds
100.00% <= 2 milliseconds
100.00% <= 3 milliseconds
100.00% <= 5 milliseconds
100.00% <= 6 milliseconds
100.00% <= 7 milliseconds
100.00% <= 7 milliseconds
432862.97 requests per second
```

```c
====== SET ======
  10000000 requests completed in 36.15 seconds
  200 parallel clients
  3 bytes payload
  keep alive: 1
91.97% <= 1 milliseconds
99.98% <= 2 milliseconds
99.98% <= 3 milliseconds
99.98% <= 4 milliseconds
99.98% <= 5 milliseconds
99.98% <= 6 milliseconds
99.98% <= 7 milliseconds
99.98% <= 9 milliseconds
99.98% <= 10 milliseconds
99.98% <= 11 milliseconds
99.98% <= 12 milliseconds
99.98% <= 13 milliseconds
99.98% <= 16 milliseconds
99.98% <= 18 milliseconds
99.99% <= 19 milliseconds
99.99% <= 23 milliseconds
99.99% <= 24 milliseconds
99.99% <= 25 milliseconds
99.99% <= 27 milliseconds
99.99% <= 28 milliseconds
99.99% <= 34 milliseconds
99.99% <= 37 milliseconds
99.99% <= 39 milliseconds
99.99% <= 40 milliseconds
99.99% <= 46 milliseconds
99.99% <= 48 milliseconds
99.99% <= 49 milliseconds
99.99% <= 50 milliseconds
99.99% <= 51 milliseconds
99.99% <= 52 milliseconds
99.99% <= 61 milliseconds
99.99% <= 63 milliseconds
99.99% <= 72 milliseconds
99.99% <= 73 milliseconds
99.99% <= 74 milliseconds
99.99% <= 76 milliseconds
99.99% <= 83 milliseconds
99.99% <= 84 milliseconds
99.99% <= 88 milliseconds
99.99% <= 89 milliseconds
99.99% <= 133 milliseconds
99.99% <= 134 milliseconds
99.99% <= 146 milliseconds
99.99% <= 147 milliseconds
100.00% <= 203 milliseconds
100.00% <= 204 milliseconds
100.00% <= 208 milliseconds
100.00% <= 217 milliseconds
100.00% <= 218 milliseconds
100.00% <= 219 milliseconds
100.00% <= 220 milliseconds
100.00% <= 229 milliseconds
100.00% <= 229 milliseconds
276617.50 requests per second
```

#### 结论

get/set 响应时间 99.9%都在2ms以内。

### 测试三

#### 测试目的

在pika最佳的worker线程数下，查看各命令的极限QPS。

#### 测试条件

**pika的worker线程数**：20
**key数量**：10000
**field数量**：100（list除外）
**value**：128字节
**命令执行次数**：1000万（lrange除外）

#### 测试结果

```c
PING_INLINE: 548606.50 requests per second
PING_BULK: 544573.31 requests per second
SET: 231830.31 requests per second
GET: 512163.91 requests per second
INCR: 230861.56 requests per second
MSET (10 keys): 94991.12 requests per second
LPUSH: 196093.81 requests per second
RPUSH: 195186.69 requests per second
LPOP: 131156.14 requests per second
RPOP: 152292.77 requests per second
LPUSH (needed to benchmark LRANGE): 196734.20 requests per second
LRANGE_100 (first 100 elements): 50705.12 requests per second
LRANGE_300 (first 300 elements): 16745.16 requests per second
LRANGE_450 (first 450 elements): 6787.94 requests per second
LRANGE_600 (first 600 elements): 3170.38 requests per second
SADD: 160885.52 requests per second
SPOP: 128920.80 requests per second
HSET: 180209.41 requests per second
HINCRBY: 153364.81 requests per second
HINCRBYFLOAT: 141095.47 requests per second
HGET: 506791.00 requests per second
HMSET (10 fields): 27777.31 requests per second
HGETALL: 109059.58 requests per second
ZADD: 120583.62 requests per second
ZREM: 161689.33 requests per second
PFADD: 6153.47 requests per second
PFCOUNT: 28312.57 requests per second
PFADD (needed to benchmark PFMERGE): 6166.37 requests per second
PFMERGE: 6007.09 requests per second
```

#### 结论

整体表现很不错，个别命令表现较弱（LRANGE，PFADD，PFMERGE）。
