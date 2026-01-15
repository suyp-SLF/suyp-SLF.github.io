---
title: C++用Cmake编译
date: 2026-01-13 07:31:37
tags:
 -C++
 -Cmake
categories:
 - C++
---

# C++用Cmake编译

## 1. Cmake简介
CMake是一个跨平台的安装（编译）工具，可以用简单的语句来描述所有平台的安装(编译过程)。他能够输出各种各样的makefile或者project文件，能测试编译器所支持的C++特性,类似UNIX下的automake。只是 CMake 的组态档取名为 CMakeLists.txt。Cmake 并不直接构建出最终的软件，而是生成标准的构建文件（如 Unix 的 Makefile 或 Windows Visual C++ 的 projects/workspaces），然后再调用指定的编译器进行处理。
<!-- more -->
## 2. Cmake编译
### 打开下载的git代码，用Cmake build。
![view](/images/posts/C-用Cmake编译/1.png)
### build完之后生成build文件夹，进入build文件夹，找到输出文件路径编译到需要的位置
![view](/images/posts/C-用Cmake编译/2.png)
![view](/images/posts/C-用Cmake编译/3.png)

