---
title: 'shader: item wave'
date: 2025-08-31 16:48:10
tags:
  - godot
  - shader
categories:
  - Game
  - godot
cover: /images/posts/shader-item-wave/cover.png  # 站内图片
---
# create a wave item in shader (godot)

## let me show this
![preview](/images/posts/shader-item-wave/1.gif)

## using godot

## Shader Code
```Shader
shader_type canvas_item;

group_uniforms Sine;
uniform bool do_abs;
uniform bool do_quantize;
uniform float quantize_to : hint_range(0, 2, 0.1) = 1;
uniform vec2 sine_amplitude = vec2(0.0, 1.0); // 修改默认值为双向
uniform vec2 sine_speed = vec2(1.0, 1.0);     // 速度也可以双向

void vertex() {
    vec2 s = sin(TIME * sine_speed);
    if (do_abs) {
        s = abs(s);
    }
    VERTEX += s * sine_amplitude;
    if (do_quantize) {
        VERTEX = round(VERTEX / quantize_to);
        VERTEX *= quantize_to;
    }
}
```
