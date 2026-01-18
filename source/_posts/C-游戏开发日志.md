---
title: C++游戏开发日志
date: 2026-01-06 06:07:59
tags: 
 -Game
 -C++
---
## 参考视频 B站ZiyuGameDev
## 头文件减少依赖，能够增加编译速度
```
// 向前声明，减少头文件以来，增加编译速度
//因为【预处理】部分会将头文件替换，所以时间上会减少很多。
struct SDL_Window;
struct SDL_Renderer;
// 在使用的时候再去.cpp文件读取头文件
```
## Cmake使用
## 设置
```
先使用command + shift + p
使用Cmake Quick Start创建项目，Cmake会自动生成CMakeLists.txt文件，会辅助我们生成Makefile

cmake_minimum_required(VERSION 3.10.0) //安装的cmake版本号最低要求
project(StarLand VERSION 0.1.0 LANGUAGES C CXX) //项目名称，版本号，语言

add_executable(StarLand main.cpp)   //需要编译执行的文件，生成一个可执行文件
```
### Cmake作用，类似于Maven，添加需要加载的依赖以及设置
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
#### 简单配置
```
cmake_minimum_required(VERSION 3.10.0)
project(StarLand VERSION 0.1.0 LANGUAGES C CXX)

include_directories("/opt/homebrew/opt/sdl2/include")
link_directories("/opt/homebrew/opt/sdl2/lib")

add_executable(StarLand main.cpp)

target_link_libraries(StarLand SDL2 SDL2main)
```

### 简单方法 Cmake 跨平台逻辑
```
//可以参考这种
if(APPLE)
//mac 平台
include_directories("/opt/homebrew/opt/sdl2/include")
link_directories("/opt/homebrew/opt/sdl2/lib")
endif(WIN32)
//windows平台
include_directories("C:/SDL2-2.0.18/include")
link_directories("C:/SDL2-2.0.18/lib/x64")
elseif(UNIX)
//Linux平台
include_directories("/usr/include/SDL2")
link_directories("/usr/lib/x86_64-linux-gnu")
endif()
```
### 或者这样,很多库的lib存在cmake文件，里面内置很多代码，可以直接使用
```
![Image Alt Text](/images/posts/C++游戏开发日志/2.png)

cmake_minimum_required(VERSION 3.10.0)
project(StarLand VERSION 0.1.0 LANGUAGES C CXX)

//include_directories("/opt/homebrew/opt/sdl2/include")
//link_directories("/opt/homebrew/opt/sdl2/lib")
find_package(SDL2 REQUIRED) //调用内置的cmake文件,上面的两句就不需要了


add_executable(StarLand main.cpp)

target_link_libraries(StarLand SDL2::SDL2)  //调用内置的cmake文件,需要添加::SDL2,${SDL2_LIBRARIES}是更好的跨平台逻辑
```
## 引用错误
```
//重新设置
C/C++: Change Configuration Provider
//强制重置 IntelliSense 数据库
C/C++: Reset IntelliSense Database
```

## C++单例模式
```
//单例模式
class Singleton {
public:
    static Singleton& getInstance() {
        static Singleton instance;
        return instance;
    }
    void doSomething() {
        // do something
    }
private:
    Singleton() {} // 私有构造函数
    Singleton(const Singleton&) = delete; // 禁止拷贝构造函数
    Singleton& operator=(const Singleton&) = delete; // 禁止赋值操作符
};
//这里非常要注意，私有构造函数Singleton() {}， 大括号一定要有、一定要有、一定要有
//使用
Singleton& instance = Singleton::getInstance();
instance.doSomething();
```
## 无法引入头文件
```
// 添加可执行文件，查看cmake配置文件是不是没有配置
add_executable(${TARGET} 
                src/main.cpp
                src/Game.cpp
                src/SceneMain.cpp)
```

# 像素游戏设置 Renderer 和 Window (Nearest Neighbor)
```
// --- Renderer 属性设置 ---
SDL_PropertiesID ren_props = SDL_CreateProperties();
SDL_SetPointerProperty(ren_props, SDL_PROP_RENDERER_CREATE_WINDOW_POINTER, _window);
SDL_SetNumberProperty(ren_props, SDL_PROP_RENDERER_CREATE_PRESENTVSYNC_NUMBER, 1); // 必须开启 VSync 防止撕裂

_renderer = SDL_CreateRendererWithProperties(ren_props);
SDL_DestroyProperties(ren_props);

// --- 像素游戏特有设置 ---

// 1. 关闭线条的平滑处理（确保线条也是硬朗的像素点）
SDL_SetHint(SDL_HINT_RENDER_LINE_METHOD, "0"); 

// 2. 逻辑分辨率：这是像素游戏的“降准”核心
// 假设你的原始设计是 320x180 的低分辨率像素画
// 设置逻辑显示，SDL会自动帮你等比例拉伸到窗口大小，且保持像素锐利
SDL_SetRenderLogicalPresentation(_renderer, width, height, SDL_LOGICAL_PRESENTATION_INTEGER_SCALE);
```
# C++ vector删除元素，erase-remove方法，效率高 
```
_children.erase(std::remove(_children.begin(), _children.end(), child), _children.end())

std::remove(_children.begin(), _children.end(), child) // 将要删除的元素放到vector的末尾，并返回新的end迭代器
_children.erase(_children.begin(), _children.end()) // 删除所有要删除的元素
```
# C++ 继承
```
class Object
{
protected:
    Game &_game = Game::GetInstance();
}
class ObjectAffiliate : public Object
class ObjectAffiliate : Object
// 两种继承，第一个是正常继承，会调用Object的构造函数，第二个不会调用Object的构造函数
// 并且，第一个可以访问Object的私有成员，第二个不可以访问Object的私有成员
```