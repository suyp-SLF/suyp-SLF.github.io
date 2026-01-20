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
```C++
SDL_Init(SDL_INIT_EVERYTHING);
```
### 修改后SDL3初始化，按需加载
```C++
// 组合你真正需要的模块
if (!SDL_Init(SDL_INIT_VIDEO | SDL_INIT_AUDIO | SDL_INIT_EVENTS | SDL_INIT_JOYSTICK)) {
    SDL_Log("初始化失败: %s", SDL_GetError());
    return -1;
}
```

### 修改后SDL3好用的文本显示
```C++
// 新文本绘制
    _textEngine = TTF_CreateRendererTextEngine(_renderer);
    // 加载字体
    _font = TTF_OpenFont("assets/font/VonwaonBitmap-16px.ttf", 24);
    if (!_font)
    {
        // 关键：打印出具体错误原因，比如 "No such file or directory"
        SDL_Log("字体加载失败！错误原因: %s\n", SDL_GetError());
        SDL_Log("当前工作目录路径，请确认 assets 文件夹是否在此目录下。");
    }
    _text = TTF_CreateText(_textEngine, _font, "FPS: 60", 0);
void Game::drawFPS(const glm::vec2 &position, const SDL_FColor color)
{
    if (!_text)
        return;

    // 1. 【重中之重】将渲染颜色设为纯白
    // 几何渲染会把这个颜色和文字纹理相乘。如果是黑色，字就消失了。
    SDL_SetRenderDrawColor(_renderer, 255, 255, 255, 255);

    // 2. 显式开启混合模式
    SDL_SetRenderDrawBlendMode(_renderer, SDL_BLENDMODE_BLEND);

    // 3. 设置文字内部颜色（转化为 0-255）
    TTF_SetTextColor(_text,
                     (Uint8)(color.r * 255),
                     (Uint8)(color.g * 255),
                     (Uint8)(color.b * 255),
                     (Uint8)(color.a * 255));

    // 4. 执行绘制
    if (!TTF_DrawRendererText(_text, position.x, position.y))
    {
        SDL_Log("TTF_DrawRendererText 失败: %s", SDL_GetError());
    }

    // 5. 绘制完后把颜色改回黑色（为了不影响你的 drawGrid 逻辑）
    SDL_SetRenderDrawColor(_renderer, 0, 0, 0, 255);
}

void Game::update(float dt)
{

    //更新
    if (_text)
    {
        char buffer[64];
        // dt * 1000 得到毫秒数
        float ms = dt * 1000.0f;
        float fps = (dt > 0) ? (1.0f / dt) : 0;

        SDL_snprintf(buffer, sizeof(buffer), "FPS: %.1f  Time: %.2f ms", fps, ms);

        // 更新 SDL3_ttf 文本内容
        TTF_SetTextString(_text, buffer, 0);
    }
    _current_scene->update(dt);
}
```