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

```shell
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

```shell
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