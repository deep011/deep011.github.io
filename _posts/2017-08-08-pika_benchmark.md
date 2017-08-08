---
layout: post
title: Pika测试
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