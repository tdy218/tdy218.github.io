---
layout: post
title:  "使用rotatelogs和CATALINA_OUT_CMD环境变量限制Tomcat标准输出日志catalina.out文件大小"
date:   2023-04-24 16:43:44 +0800
comments: true
categories: [tomcat]
---

### 1. Tomcat标准输出日志catalina.out切割环境变量CATALINA_OUT_CMD

从 Tomcat 以下版本， Tomcat 支持使用 CATALINA_OUT_CMD 环境变量来定义一个重定向 Tomcat 进程 stdout、stderr 日志的命令，以此来控制 catalina.out 日志的轮询。

```
Fixed in:
- master for 10.0.0-M6 onwards
- 9.0.x for 9.0.36 onwards
- 8.5.x for 8.5.56 onwards
- 7.0.x for 7.0.105 onwards
```

详见：https://bz.apache.org/bugzilla/show_bug.cgi?id=64430



### 2. 应用

常见的日志轮询的工具有：cronolog、logrotate、rotatelogs

其中 cronolog 好多年已经不更新了，不推荐使用，logrotate 只能按照一定频率去检查日志文件大小的方式来限制，存在一定的不足（在检查周期内，目标日志文件可能出现突发增长的情况），所以下面推荐使用 Apache httpd 自带的 rotatelogs 来进行限制，rotatelogs 支持实时文件大小检查。

