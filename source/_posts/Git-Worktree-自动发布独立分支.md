---
title: Git Worktree 自动发布独立分支
date: 2026-01-12 23:10:47
tags:
 - Git
categories:
 - 代码人生
---
在游戏开发中，我们经常需要将生成的文档（如 Doxygen）、构建报告或 Web 版游戏（Emscripten 编译出的 HTML）自动发布到独立的分支（如 gh-pages 或 docs 分支）。

为了不让这些生成的 HTML 文件污染你的源代码历史，最专业的方法是使用 git worktree。这种方法允许你在本地另一个文件夹中直接操作另一个分支，而无需来回切换（checkout）。

## 核心步骤：使用 Git Worktree
假设你的主分支是 main，目标分支是 gh-pages，HTML 文件在 dist/ 目录下。
## 1. 准备目标分支（只需做一次）
如果远程还没有这个分支，先创建一个孤儿分支：
```bash
git checkout --orphan gh-pages
git rm -rf .
git commit --allow-empty -m "Init gh-pages"
git push origin gh-pages
git checkout main
```
## 2. 将目标分支挂载到子目录
在主分支下，将 gh-pages 分支关联到一个临时文件夹（如 out-branch）：
```bash
# 这样 out-branch 文件夹里的内容就是 gh-pages 分支的代码
git worktree add out-branch gh-pages
```
## 2. 复制文件并提交
这一步通常写在你的构建脚本（如 .bat 或 Makefile）中：
```bash
# 1. 运行你的游戏构建命令，生成 html 到 dist/
# 2. 清空目标文件夹
rm -rf out-branch/* # 3. 将生成的 html 拷贝过去
cp -r dist/* out-branch/
# 4. 进入该文件夹提交并推送
cd out-branch
git add .
git commit -m "Deploy updated HTML"
git push origin gh-pages
cd ..
```

# 1.配置完成后更新提交
## 编译
```bash
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
## 同步
```bash
cp -r dist/* out-branch/
```
## 推送
```bash
cd out-branch
git add .
git commit -m "Update"
git push origin gh-pages
cd ..
```
# 2.自动化执行脚本
chmod +x deploy.sh // 权限
```
#!/bin/bash
set -e 

echo "正在编译最新版本..."
mkdir -p dist

# 1. 编译 (确保编译成功)
emcc src/*.cpp -o dist/index.html \
  -s USE_SDL=2 \
  -s USE_SDL_IMAGE=2 \
  -s USE_SDL_TTF=2 \
  -s USE_SDL_MIXER=2 \
  -s SDL2_IMAGE_FORMATS='["png"]' \
  -s SDL2_MIXER_FORMATS='["mp3","ogg"]' \
  --preload-file assets@assets \
  -O2

echo "正在同步到发布分支..."

# 2. 检查 worktree 状态
if [ ! -d "out-branch/.git" ] && [ ! -f "out-branch/.git" ]; then
    echo "错误: out-branch 文件夹不是有效的 Git Worktree。"
    echo "请尝试运行: git worktree add out-branch gh-pages"
    exit 1
fi

# 3. 使用 rsync 同步 (这是最安全的方法)
# --delete: 删除目标目录中多余的文件
# --exclude='.git': 绝对不触碰 git 核心管理文件
# 注意 dist/ 后面那个斜杠，代表同步目录内容而不是目录本身
rsync -av --delete --exclude='.git' dist/ out-branch/

echo "正在推送更新到远程..."
cd out-branch

# 4. 提交并推送
if [ -n "$(git status --porcelain)" ]; then
    git add .
    git commit -m "Update game build: $(date +'%Y-%m-%d %H:%M:%S')"
    git push origin gh-pages
    echo "✅ 远程分支更新成功！"
else
    echo "ℹ️ 内容无变化，跳过推送。"
fi

cd ..
```