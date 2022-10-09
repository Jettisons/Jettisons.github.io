---
title: java日志
categories:
- tech
---

日志框架技术：JUL、Logback、Log4j、Log4j2

日志门面技术：JCL、SLF4j

每一种日志框架都有自己单独的API，要使用对应的框架就要使用对应的API

使用日志门面技术可以屏蔽日志框架技术的不同，可在项目中随意改动日志框架 

<!-- more -->

## JUL

java.util.logging 

Java原生日志框架

## Log4j

Apache的一个开源项目

## Logback

由Log4j之父做的另一个开源项目

业界乘坐log4j的后浪，一个可靠、通用且灵活的java日志框架

## Log4j2

Log4j官方的第二个版本，各方面与Logback相似

具有插件式结构、配置文件优化等特征

Spring Boot1.4版本后不再支持log4j

## 日志门面技术

可以屏蔽日志框架的不同，只要日志门面做的好，随意换另外一个日志框架，应用程序不需要修改任意一行代码就可以直接上线

### SLF4J

中间使用桥接器和日志框架桥接





