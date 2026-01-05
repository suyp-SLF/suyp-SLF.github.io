---
title: Exception Stack Trace Suppression (about print same log)
date: 2022-03-09 19:07:18
tags:
  - Java
  - Log
  - JVM
  - Server
categories:
  - Technology
cover: /images/cover.jpg  # 站内图片
---
# Exception Stack Trace Suppression
when JVM print so much same Log, it will not print this error.
It's good, but sometimes I can't find the issue.
## this is the JVM config that can close this.
```JVM
# close Exception Stack Trace Suppression
-XX:-OmitStackTraceInFastThrow
```