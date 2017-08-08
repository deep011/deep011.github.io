---
layout: post
title: Redis,Mc压测工具
---

## 简述

**vire-benchmark**是一个用于压测KV系统的工具（目前支持redis协议和memcached协议）。

由于redis-benchmark只能测出像redis这样单线程KV系统的极限QPS，为了能压测出多线程/多进程模型的KV系统的性能数据，我基于redis-benchmark进行了简单的二次开发。

主要的新feature如下：

- 支持多线程
- 支持memcached协议
- 支持field随机
- 增加了默认测试用例

## 安装

```shell
git clone https://github.com/vipshop/vire.git
cd vire
autoreconf -fvi
./configure
make
tests/vire-benchmark -h
```

## 使用

**vire-benchmark**的使用几乎和redis-benchmark一样，只是多了几个选项：

- -T，指定压测工具的线程数，可以压出更高的QPS。
- -m，使用memcached协议进行测试。
- -f，用法如-r，用于控制hash，set中field的随机数量。
- -S，指定只测试相关类型的命令，比如 server,string,hash,list,set,sortedset。

```
Usage: vire-benchmark [-h <host>] [-p <port>] [-c <clients>] [-n <requests]> [-k <boolean>]

 -h <hostname>      Server hostname (default 127.0.0.1)
 -p <port>          Server port (default 6379)
 -s <socket>        Server socket (overrides host and port)
 -a <password>      Password for Redis Auth
 -c <clients>       Number of parallel connections (default 100)
 -n <requests>      Total number of requests (default 1000000)
 -T <threads>       Threads count to run (default 2)
 -d <size>          Data size of SET/GET/... value in bytes (default 16)
 -dbnum <db>        SELECT the specified db number (default 0)
 -k <boolean>       1=keep alive 0=reconnect (default 1)
 -r <keyspacelen>   Use random keys for SET/GET/INCR/... (default 10000)
  Using this option the benchmark will expand the string __rand_key__
  inside an argument with a 12 digits number in the specified range
  from 0 to keyspacelen-1. The substitution changes every time a command
  is executed. Default tests use this to hit random keys in the
  specified range.
 -f <fieldspacelen>   Use random fields for SADD/HSET/... (default 100)
  Using this option the benchmark will expand the string __rand_field__
  inside an argument with a 14 digits number in the specified range
  from 0 to fieldspacelen-1. The substitution changes every time a command
  is executed. Default tests use this to hit random fields in the
  specified range.
 -P <numreq>        Pipeline <numreq> requests. Default 1 (no pipeline).
 -e                 If server replies with errors, show them on stdout.
                    (no more than 1 error per second is displayed)
 -q                 Quiet. Just show query/sec values
 --csv              Output in CSV format
 -l                 Loop. Run the tests forever
 -t <tests>         Only run the comma separated list of tests. The test
                    names are the same as the ones produced as output.
 -S <types>         Only run the comma separated list of the redis special types commands.
                    The type names are like 'server,string,hash,list,set,sortedset'.
 -I                 Idle mode. Just open N idle connections and wait.
 -m                 Use memcached protocol. This option is used for testing memcached.
 --noinline         Not test redis inline commands.

Examples:

 Run the benchmark with the default configuration against 127.0.0.1:6379:
   $ vire-benchmark

 Use 20 parallel clients, for a total of 100k requests, against 192.168.1.1:
   $ vire-benchmark -h 192.168.1.1 -p 6379 -n 100000 -c 20

 Fill 127.0.0.1:6379 with about 1 million keys only using the SET test:
   $ vire-benchmark -t set -n 1000000 -r 100000000

 Benchmark 127.0.0.1:6379 for a few commands producing CSV output:
   $ vire-benchmark -t ping,set,get -n 100000 --csv

 Benchmark a specific command line:
   $ vire-benchmark -r 10000 -n 10000 eval 'return redis.call("ping")' 0

 Fill a list with 10000 random elements:
   $ vire-benchmark -r 10000 -n 10000 lpush mylist __rand_field__

 On user specified command lines __rand_key__ and __rand_field__ are replaced
 with a random integer with a range of values selected by the -r and -f option.
```