---
layout: post
title:  "AB基准测试"
subtitle: "微服务性能测试整理系列（一）"
date:   2018-03-30 8:00:00
background: 'http:/\/oo6gt25nl.bkt.clouddn.com/01.jpg'
---

# WRK基准测试

### 1 安装过程

服务器环境：qingcloud Ubuntu 16.04

**Nginx 安装**

```
sudo apt-get install nginx
```

**ApacheBench 安装**

命令介绍： [ab - Apache HTTP server benchmarking tool](http://httpd.apache.org/docs/2.4/programs/ab.html)

```
sudo apt-get install apache2-utils  
```

### 2 测试及优化

**2.1 测试（未优化，100并发5w请求）**

使用命令

```
ab -c 100 -n 50000 http://127.0.0.1/index.html
```

结果集：

```
Server Software:        nginx/1.10.3
Server Hostname:        172.20.0.10
Server Port:            80

Document Path:          /
Document Length:        612 bytes

Concurrency Level:      100
Time taken for tests:   3.464 seconds
Complete requests:      50000
Failed requests:        0
Total transferred:      42700000 bytes
HTML transferred:       30600000 bytes
Requests per second:    14432.26 [#/sec] (mean)
Time per request:       6.929 [ms] (mean)
Time per request:       0.069 [ms] (mean, across all concurrent requests)
Transfer rate:          12036.28 [Kbytes/sec] received

Connection Times (ms)
              min  mean[+/-sd] median   max
Connect:        0    0   0.5      0       9
Processing:     2    7   1.4      7      15
Waiting:        2    7   1.4      7      15
Total:          3    7   1.2      7      15

Percentage of the requests served within a certain time (ms)
  50%      7
  66%      7
  75%      7
  80%      7
  90%      8
  95%      8
  98%     11
  99%     12
 100%     15 (longest request)

```

**2.2 测试（未优化，1k并发5w请求）**

使用命令

```
ab -c 1000 -n 50000 http://127.0.0.1/
```

结果集

```
Server Software:        nginx/1.10.3
Server Hostname:        172.20.0.10
Server Port:            80

Document Path:          /
Document Length:        612 bytes

Concurrency Level:      1000
Time taken for tests:   13.875 seconds
Complete requests:      50000
Failed requests:        0
Total transferred:      42700000 bytes
HTML transferred:       30600000 bytes
Requests per second:    3603.53 [#/sec] (mean)
Time per request:       277.506 [ms] (mean)
Time per request:       0.278 [ms] (mean, across all concurrent requests)
Transfer rate:          3005.28 [Kbytes/sec] received

Connection Times (ms)
              min  mean[+/-sd] median   max
Connect:        0   14 118.3      0    3004
Processing:     2   60 514.5      9   13847
Waiting:        2   60 514.5      9   13847
Total:          4   74 529.6      9   13862

Percentage of the requests served within a certain time (ms)
  50%      9
  66%      9
  75%      9
  80%     10
  90%     12
  95%     24
  98%   1007
  99%   1681
 100%  13862 (longest request)
```

**2.3 测试（未优化，1w并发5w请求）**

使用命令

```
ab -c 10000 -n 50000 http://127.0.0.1/
```

结果集：

```
This is ApacheBench, Version 2.3 <$Revision: 1706008 $>
Copyright 1996 Adam Twiss, Zeus Technology Ltd, http://www.zeustech.net/
Licensed to The Apache Software Foundation, http://www.apache.org/

Benchmarking 127.0.0.1 (be patient)
socket: Too many open files (24)
```



**测试（未优化，1k并发5w请求）**









