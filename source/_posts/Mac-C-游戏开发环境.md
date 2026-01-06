---
title: '# Mac C++游戏开发环境'
date: 2026-01-06 04:49:03
tags: 
  - Godot
categories:
  - Game
---
# Mac C++游戏开发环境
## C++编译器（Clang）
```
xcode-select --install //安装
```

## Cmake安装（编译）
```
brew install cmake //安装
cmake --version //验证
```
## sdl2安装（图形，窗口）
```
brew install sdl2 sdl2_image sdl2_mixer sdl2_ttf
```
## sdl3安装（图形，窗口）
```
brew install sdl3 //安装
brew install sdl3_image //图片
brew install sdl3_mixer //音频播放和混音
brew info sdl3 //验证
brew info sdl3_image
brew info sdl3_mixer 
```
## glm安装（数学计算）
```
brew install glm //安装
brew info glm //验证
```
## nlohmann/json安装（一个流行的 C++ JSON 库）
```
brew install nlohmann-json //安装
brew info nlohmann-json //验证
```
## spdlog（spdlog 是一个快速的 C++ 日志库）
```
brew install spdlog //安装
brew info spdlog //验证
```