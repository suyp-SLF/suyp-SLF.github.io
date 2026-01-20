---
title: C++游戏开发日志
date: 2026-01-06 06:07:59
tags: 
 -Game
 -C++
---
## 参考视频 B站ZiyuGameDev
## C++ 友元
//我声明了一个友元类，那么这个类就可以访问这个类的私有属性，类似于这样
```C++
class Spell;
class Collider : public ObjectAffiliate
{
protected:
    // 友元
    friend Spell;
    enum class Shape
    {
        CIRCLE,
        RECTANGLE
    };
    Shape _shape = Shape::CIRCLE;
};

Spell *Spell::addSpellChild(ObjectWorld *parent, const std::string &texture_path, glm::vec2 position, float scale, Anchor anchor)
{
    auto spell = new Spell();
    spell->init();
    spell->_anim = SpriteAnim::addSpriteAnimChild(spell, texture_path, anchor, scale);
    auto size = spell->_anim->getSize();
    //这里就可以访问私有的Shape
    spell->_collider = Collider::addColliderChild(spell, size, Anchor::CENTER, Collider::Shape::CIRCLE);
    spell->_anim->setIsLoop(false);
    spell->setPosition(position);
    if (parent) parent->addChild(spell);
    return spell;
}
```
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
```C++
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
```C++
// 添加可执行文件，查看cmake配置文件是不是没有配置
add_executable(${TARGET} 
                src/main.cpp
                src/Game.cpp
                src/SceneMain.cpp)
```

# 像素游戏设置 Renderer 和 Window (Nearest Neighbor)
```C++
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
```C++
_children.erase(std::remove(_children.begin(), _children.end(), child), _children.end())

std::remove(_children.begin(), _children.end(), child) // 将要删除的元素放到vector的末尾，并返回新的end迭代器
_children.erase(_children.begin(), _children.end()) // 删除所有要删除的元素
```
# C++ 继承
```C++
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

# C++ 随机数
```C++
#include <random>

// 随机数生成器
std::mt19937 _random_generator = std::mt19937(std::random_device{}()); 

// 生成一个[min, max)之间的随机浮点数
float randomFloat(float min, float max) { return std::uniform_real_distribution<float>(min, max)(_random_generator)}; 
// 生成一个[min, max)之间的随机整数
float randowInt(int min, int max) { return std::uniform_int_distribution<int>(min, max)(_random_generator); } 
// 生成一个[min, max)之间的随机二维向量
glm::vec2 randomVec2(float min, float max) { return glm::vec2(randomFloat(min, max), randomFloat(min, max)); } 
// 生成一个[min, max)之间的随机二维整数向量
glm::ivec2 randomIvec2(int min, int max) { return glm::ivec2(randowInt(min, max), randowInt(min, max)); } 
```

# 资源管理器
```C++
#ifndef ASSET_STORE_H
#define ASSET_STORE_H

#include <string>
#include <unordered_map> //无序map，性能比mao稍好
#include <SDL3/SDL.h>
#include <SDL3_mixer/SDL_mixer.h>
#include <SDL3_image/SDL_image.h>
#include <SDL3_ttf/SDL_ttf.h>

class AssetStore
{
private:
    SDL_Renderer *_renderer = nullptr; // 渲染器指针
    MIX_Mixer *_mixer = nullptr; // 音效指针
    std::unordered_map<std::string, SDL_Texture *> _textures;
    std::unordered_map<std::string, MIX_Audio *> _sounds;
    std::unordered_map<std::string, MIX_Audio *> _musics;
    std::unordered_map<std::string, TTF_Font *> _fonts;

    // 载入函数,返回地址
    SDL_Texture *loadTexture(const std::string &path);
    MIX_Audio *loadSound(const std::string &path);
    MIX_Audio *loadMusic(const std::string &path);
    TTF_Font *loadFont(const std::string &path, int size);

public:
    AssetStore(SDL_Renderer *renderer, MIX_Mixer *mixer) : _renderer(renderer), _mixer(mixer) {};
    ~AssetStore() = default;

    void clean(); // 清理函数
    // 获取函数
    SDL_Texture *getTexture(const std::string &name);
    MIX_Audio *getSound(const std::string &name);
    MIX_Audio *getMusic(const std::string &name);
    TTF_Font *getFont(const std::string &name, int font_size);
};

#endif // ASSET_STORE_H

#include "asset_store.h"

void AssetStore::clean()
{
    for (auto &pair : _textures)
    {
        SDL_DestroyTexture(pair.second);
    }
    _textures.clear();

    for (auto &pair : _sounds)
    {
        MIX_DestroyAudio(pair.second);
    }
    _sounds.clear();

    for (auto &pair : _musics)
    {
        MIX_DestroyAudio(pair.second);
    }
    _musics.clear();

    for (auto &pair : _fonts)
    {
        TTF_CloseFont(pair.second);
    }
    _fonts.clear();
}

SDL_Texture *AssetStore::loadTexture(const std::string &_file_path)
{
    SDL_Texture *texture = IMG_LoadTexture(_renderer, _file_path.c_str());
    if (texture == nullptr)
    {
        SDL_LogError(SDL_LOG_CATEGORY_APPLICATION, "加载图片失败: %s", SDL_GetError());
        return nullptr;
    }
    //_textures[_file_path] = texture;
    _textures.emplace(_file_path, texture);
    return texture;
}

MIX_Audio *AssetStore::loadSound(const std::string &_file_path)
{
    MIX_Audio *sound = MIX_LoadAudio(_mixer, _file_path.c_str(), false);
    if (sound == nullptr)
    {
        SDL_LogError(SDL_LOG_CATEGORY_APPLICATION, "加载音频失败: %s", SDL_GetError());
        return nullptr;
    }
    _sounds.emplace(_file_path, sound);
    return sound;
}

MIX_Audio *AssetStore::loadMusic(const std::string &_file_path)
{
    MIX_Audio *music = MIX_LoadAudio(_mixer, _file_path.c_str(), false);
    if (music == nullptr)
    {
        SDL_LogError(SDL_LOG_CATEGORY_APPLICATION, "加载背景音乐失败: %s", SDL_GetError());
        return nullptr;
    }
    _musics.emplace(_file_path, music);
    return music;
}

TTF_Font *AssetStore::loadFont(const std::string &_file_path, int font_size)
{
    TTF_Font *font = TTF_OpenFont(_file_path.c_str(), font_size);
    if (font == nullptr)
    {
        SDL_LogError(SDL_LOG_CATEGORY_APPLICATION, "加载字体失败: %s", SDL_GetError());
        return nullptr;
    }
    _fonts.emplace(_file_path + std::to_string(font_size), font);
    return font;
}



SDL_Texture *AssetStore::getTexture(const std::string &file_path)
{
    // _textures找到name，返回it
    auto it = _textures.find(file_path);
    if (it == _textures.end())
    {
        return loadTexture(file_path);
    }
    else
    {
        return it->second;
    }
}

MIX_Audio *AssetStore::getSound(const std::string &file_path)
{
    // _sounds找到name，返回it
    auto it = _sounds.find(file_path);
    if (it == _sounds.end())
    {
        return loadSound(file_path);
    }
    else
    {
        return it->second;
    }
}

MIX_Audio *AssetStore::getMusic(const std::string &file_path)
{
    // _musics找到name，返回it
    auto it = _musics.find(file_path);
    if (it == _musics.end())
    {
        return loadMusic(file_path);
    }
    else
    {
        return it->second;
    }
}

TTF_Font *AssetStore::getFont(const std::string &file_path, int font_size)
{
    // _fonts找到name，返回it
    auto it = _fonts.find(file_path + std::to_string(font_size));
    if (it == _fonts.end())
    {
        return loadFont(file_path, font_size);
    }
    else
    {
        return it->second;
    } ff
}
```