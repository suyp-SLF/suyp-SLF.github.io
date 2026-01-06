---
title: C++游戏开发日志
date: 2026-01-06 06:07:59
tags: 
 -Game
 -C++
---
##参考视频 B站ZiyuGameDev
## 头文件减少依赖，能够增加编译速度
```
// 向前声明，减少头文件以来，增加编译速度
//因为【预处理】部分会将头文件替换，所以时间上会减少很多。
struct SDL_Window;
struct SDL_Renderer;
// 在使用的时候再去.cpp文件读取头文件
```
## Cmake使用
###设置
先使用command + shift + p
使用Cmake Quick Start创建项目，Cmake会自动生成CMakeLists.txt文件，会辅助我们生成Makefile
```
cmake_minimum_required(VERSION 3.10.0) //安装的cmake版本号最低要求
project(StarLand VERSION 0.1.0 LANGUAGES C CXX) //项目名称，版本号，语言

add_executable(StarLand main.cpp)   //需要编译执行的文件，生成一个可执行文件
```
###Cmake作用，类似于Maven，添加需要加载的依赖以及设置
![Image Alt Text](/images/posts/C++游戏开发日志/1.png)
类似于这种需要加载库的时候报错，一般是Cmake没有加载对应的库，需要去CmakeLists.txt中添加对应的库。告诉系统文件位置。
```
//cmake 加载库的代码
include_directories(${CMAKE_SOURCE_DIR}/include) //加载头文件用于头文件
link_directories(${CMAKE_SOURCE_DIR}/lib) //链接库文件用于链接

target_link_libraries(StarLand SDL2main SDL2) //链接库文件,需要指定需要哪些库文件,也就是link_directories制定的文件路径
#brew 查找库文件位置 
brew --prefix sdl2
```
## C++（cpp-> 预处理 -> 编译 -> 链接 -> .exe）
### 预处理
就是处理头文件，将头文件中的内容替换到cpp文件中，然后编译成汇编文件
### 编译
将汇编文件编译成二进制文件，cmake制定需要编译的文件，使用第三方库需要指定库文件(.h)
### 链接
将二进制文件链接成可执行文件，cmake需要制定链接库文件，使用第三方库需要指定库文件(.lib .dll)

## SDL2 C++ Cmake配置
```
cmake_minimum_required(VERSION 3.10.0)
project(StarLand VERSION 0.1.0 LANGUAGES C CXX)

include_directories("/opt/homebrew/opt/sdl2/include")
link_directories("/opt/homebrew/opt/sdl2/lib")

add_executable(StarLand main.cpp)

target_link_libraries(StarLand SDL2 SDL2main)
```

##Cmake 跨平台逻辑
```
//可以参考这种
if(APPLE)
#mac 平台
include_directories("/opt/homebrew/opt/sdl2/include")
link_directories("/opt/homebrew/opt/sdl2/lib")
endif(WIN32)
#windows平台
include_directories("C:/SDL2-2.0.18/include")
link_directories("C:/SDL2-2.0.18/lib/x64")
elseif(UNIX)
# Linux平台
include_directories("/usr/include/SDL2")
link_directories("/usr/lib/x86_64-linux-gnu")
endif()

//或者这样,很多库的lib存在cmake文件，里面内置很多代码，可以直接使用
![Image Alt Text](/images/posts/C++游戏开发日志/2.png)

cmake_minimum_required(VERSION 3.10.0)
project(StarLand VERSION 0.1.0 LANGUAGES C CXX)

//include_directories("/opt/homebrew/opt/sdl2/include")
//link_directories("/opt/homebrew/opt/sdl2/lib")
find_package(SDL2 REQUIRED) //调用内置的cmake文件,上面的两句就不需要了


add_executable(StarLand main.cpp)

target_link_libraries(StarLand SDL2::SDL2)  //调用内置的cmake文件,需要添加::SDL2

```

