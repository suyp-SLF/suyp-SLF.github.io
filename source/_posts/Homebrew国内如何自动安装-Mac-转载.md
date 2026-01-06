---
title: Homebrew国内如何自动安装 Mac 转载
date: 2026-01-06 07:44:32
tags: 
 -Homebrew
 -Mac
---
## 自动安装脚本，也可以更新源
```
// 安装脚本
/bin/zsh -c "$(curl -fsSL https://gitee.com/cunkai/HomebrewCN/raw/master/Homebrew.sh)"
//卸载脚本
/bin/zsh -c "$(curl -fsSL https://gitee.com/cunkai/HomebrewCN/raw/master/HomebrewUninstall.sh)"
```