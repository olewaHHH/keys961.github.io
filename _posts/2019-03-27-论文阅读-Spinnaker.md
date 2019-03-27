---
layout: post
title: "论文阅读-Spinnaker:利用Paxos构建可扩展、一致且高可用的数据存储"
author: "keys961"
comments: true
catalog: true
tags:
  - Distributed System
  - Paper
typora-root-url: ./
---

# 0. Abstract

Spinnaker是一个为大集群（普通机器）设计的单数据中心。它通过键的范围分区，保存3个副本，读写API支持事务（强一致读，时间轴一致性）。

Spinnaker基于Paxos进行复制，读性能甚至更快，写性能只比最终一致低5%~10%。

# 1. Introduction

面对大数据量，通常方法是*分区/分片*：

- 分区管理比较麻烦（尽管有自动的解决方案）
- 为了保证线性扩展，事务不能跨节点
- ...

为了高可用，通常解决方案是*主从复制*（通常会有一个从节点是同步复制，保证高可用的同时，维持一定的一致性）。不过这有局限性，下面会细讲：

## 1.1. 主从复制的局限性以及Paxos的应用

