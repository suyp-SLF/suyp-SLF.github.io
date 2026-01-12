---
title: C/C++游戏转为网页
date: 2026-01-12 21:33:53
tags:
---
## 安装emscripten
```
//Emscripten 是一个将 C/C++ 代码编译为 WebAssembly (Wasm) 的完整工具链
//卸载原来的
brew uninstall emscripten
unalias emcc
//安装官方sdk
cd ~
git clone https://github.com/emscripten-core/emsdk.git
cd emsdk
./emsdk install latest
./emsdk activate latest
//激活
source "/Users/NAME/emsdk/emsdk_env.sh"
//第一次配置
emcc -v
//到路径下直接生成，SDL2的
# 首先创建文件夹（Windows 使用 mkdir，Linux/Mac 使用 mkdir -p）
mkdir -p dist

# 运行 emcc，修改 -o 路径
emcc src/*.cpp -o dist/index.html \
  -s USE_SDL=2 \
  -s USE_SDL_IMAGE=2 \
  -s USE_SDL_TTF=2 \
  -s USE_SDL_MIXER=2 \
  -s SDL2_IMAGE_FORMATS='["png"]' \
  -s SDL2_MIXER_FORMATS='["mp3","ogg"]' \
  --preload-file assets@assets \
  -O2
```
## 编写与编译代码
### 基础编译 (生成 JavaScript)
```
emcc main.cpp -o main.js
```
### 完整编译 (生成可运行网页)
```
emcc main.cpp -o index.html
```
### 常用编译选项

选项,说明
-O3,优化级别：极致优化代码体积和运行速度。
-s WASM=1,明确指定输出 WebAssembly（默认即为1）。
-s NO_EXIT_RUNTIME=1,保持运行时：防止 main 函数执行完后环境被销毁。
### 卸载
```
brew uninstall emscripten
```

# 修改C++代码
对于 C++游戏，直接移植到 Emscripten (EMS) 最核心的问题是：浏览器环境不允许 C++ 代码像在桌面端那样使用 while(true) 死循环。如果你的代码里有死循环，浏览器的主线程就会被永久阻塞，导致画面永远无法渲染（黑屏）。

你需要按照以下步骤修改你的 C++ 代码，使其符合 EMS 的异步架构：
## 1. 修改 C++ 主循环结构
```
//错误写法
// 这会导致浏览器卡死黑屏
while (gameRunning) {
    handleEvents();
    update();
    render();
    SDL_Delay(16); 
  
}
//EMS 正确写法
#include <SDL2/SDL.h>
#ifdef __EMSCRIPTEN__
#include <emscripten.h>
#endif

// 将原本死循环里的逻辑抽离成一个函数
void main_loop() {
    // 1. 处理输入
    // 2. 更新逻辑
    // 3. 渲染画面
}

int main(int argc, char* argv[]) {
    // ... 你的初始化代码 (SDL_Init, Window, Renderer) ...

#ifdef __EMSCRIPTEN__
    // 参数说明：回调函数, 帧率(0为使用浏览器请求动画帧), 是否模拟无限循环
    emscripten_set_main_loop(main_loop, 0, 1);
#else
    // 桌面端原本的死循环
    while (gameRunning) {
        main_loop();
        SDL_Delay(16);
    }
#endif

    return 0;
}

//增加一个静态函数
static void mainLoopWrapper(void* arg);
```
## 2. 需要增加头文件

```
#ifdef __EMSCRIPTEN__
#include <emscripten.h>
#endif
```
## 3. 重新编译的
```
emcc src/*.cpp -o index.html \
  -s USE_SDL=2 \
  -s USE_SDL_IMAGE=2 \
  -s USE_SDL_TTF=2 \
  -s USE_SDL_MIXER=2 \
  -s SDL2_IMAGE_FORMATS='["png"]' \
  -s SDL2_MIXER_FORMATS='["mp3","ogg"]' \
  --preload-file assets@assets \
  -s ASSERTIONS=1 \
  -s SAFE_HEAP=1 \
  -s ALLOW_MEMORY_GROWTH=1 \
  -O2
```