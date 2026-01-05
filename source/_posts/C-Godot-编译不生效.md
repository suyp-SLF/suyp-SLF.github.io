---
title: C# Godot 编译不生效
date: 2026-01-04 08:10:27
tags:
  - Godot
categories:
  - Game
cover: /images/cover.jpg  # 站内图片
---
# 2026/01/05 看一下现象
![Attack](/images/posts/C-Godot-编译不生效/1.png)
![Attack](/images/posts/C-Godot-编译不生效/2.png)
## 这是两个不一样的地方，重新编译不管用。现在只能把方法放进全局逻辑里面才会编译新的逻辑，并且删掉就会重新复现。
## 另外C# 如果使用数组队列没有new的话，也不会提示报错，并且像我这样卡死。