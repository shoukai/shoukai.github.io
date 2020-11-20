---
layout: post
title:  "开源软件：Feign 01 整体概览"
subtitle: "Feign 源码解析系列"
date:   2019-05-09 00:59:00
author: "Shoukai Huang"
header-img: 'cdn.apframework.com/cca353d706148a4631bf373bb3cb32c9.jpg'
header-mask: 0.4
tags: Feign 开源软件
---

## 学习过程

学习过程结合之前的文档进行开展：开源项目源码学习过程

## 相关文档

1 使用文档：清楚目标项目的使用方法、功能列表；

* [Feign 功能介绍](/2019/05/10/feign-source-2/)
* [Spring Cloud Feign](https://spring.io/projects/spring-cloud-openfeign)

2 架构文档：一个系统可以(在重大的系统中也确实如此)同时出多种不同的构架类型. 以不同的方式检查同一系统，分析系统的不同部分，或使用不同级别的分解, 都有可能发现不同的构架类型

3 对比选型：寻找同类竞品的对比文档，清楚目标项目的优势与特色

* Feign、Okhttp、RestTemplate
* gRPC、dubbo

4 社区讨论：顺着讨论思路，一个问题点切入，便于快速进入状态并找到归属感

* [github/issue](https://github.com/OpenFeign/feign/issues)
* [Gitter：Netflix/feign](https://gitter.im/OpenFeign/feign?utm_source=badge&utm_medium=badge&utm_campaign=pr-badge&utm_content=badge)

## 相关过程

安装/运行
* github clone
* mvn install
* idea debug

## 原理

* MethodHandle 方法解析
* Type 接口解析
* 动态代理

## 测试

* [example-github](https://github.com/OpenFeign/feign/blob/master/example-github/src/main/java/example/github/GitHubExample.java)
* [example-wikipedia](https://github.com/OpenFeign/feign/blob/master/example-wikipedia/src/main/java/example/wikipedia/WikipediaExample.java)

## 阅读

* [Feign源码 初始化](/2019/05/11/feign-source-3/)
* [Feign源码 接口调用](/2019/05/12/feign-source-4/)
* [Feign源码 核心类及接口](/2019/05/13/feign-source-5/)

## 核心类拆解

![](http://cdn.apframework.com/85011d4db84aede4c60800122ae1d422.jpg)
