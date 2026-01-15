---
title: SDL2与SDL3的区别
date: 2026-01-14 07:22:43
tags:
 -C++
 -SDL2
 -SDL3
---
# SDL2与SDL3的区别

## SDL初始化
### SDL2 初始化
‘’‘C++
SDL_Init(SDL_INIT_EVERYTHING);
’‘’
### 修改后SDL3初始化，按需加载
’‘’C++
// 组合你真正需要的模块
if (!SDL_Init(SDL_INIT_VIDEO | SDL_INIT_AUDIO | SDL_INIT_EVENTS | SDL_INIT_JOYSTICK)) {
    SDL_Log("初始化失败: %s", SDL_GetError());
    return -1;
}
’‘’