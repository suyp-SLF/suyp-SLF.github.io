---
title: How to create a simple blog（如何创建一个简单的个人博客）
date: 2025-08-31 05:10:35
tags:
  - Hexo
  - Blog
categories:
  - Blog
cover: /images/cover.jpg  # 站内图片
---
for who want to create a blog。

## Using: Hexo, github account, MacBook, NodeJs, 

## check: NodeJs Installed

```Shell
node -v
npm -v
```

## Install Hexo
```Shell
npm install -g hexo-cli
```

## checkout Installed
```Shell
hexo --version
```

## find a file path to init
```Shell
hexo init my-blog  # "my-blog" is your blog name
cd my-blog
npm install
```

## local start up
```Shell
hexo server # simple version: hexo s
```
## Github
```Shell
npm install hexo-deployer-git --save # install deployer plugin
# config file _config.yml
deploy:
  type: git
  repo: https://github.com/(githubname)/(reponame).github.io.git # reponame better same as githubname
  branch: main （tip in you can create a new branch to save html page, use main branch save code）
  
  这里以使用一个其他的分支(gh-pages)保存页面，主分支保存编写的文章，这样可以随时拉取
  ![Image Alt Text](/images/posts/How-to-create-a-simple-blog/1.png)
  还需要更改这里的配置
# init 
hexo clean
hexo generate
hexo deploy
```

## login deployer
```Shell
Username for 'https://github.com': (githubusername)
Password for 'https://()@github.com':(Using token blew get)

GitHub: Settings -> Developer settings -> Personal access tokens -> Tokens (classic)
"Generate new token"
Note : Hexo-Blog-Deploy
Expiration: No expiration
Select scopes : repo (All), workflow 
```

## restart deployer
```Shell
hexo clean && hexo deploy -g
```

## Blog website URL
```Shell
https://(githubname).github.io
```

## error zsh: command not found: hexo
```Shell
npm config get prefix # find config path
vim ~/.zshrc # edit zshrc config file
export PATH="$PATH:/opt/homebrew/bin"  # add a new line using hexo path
source ~/.zshrc # effect config
```

## useful Code
```Shell
# create a blog, Path: source/_posts/my-first-blog.md
hexo new "my first blog"

# title blog
---
title: (title)
date: 2023-10-27 14:00:00       # time
updated: 2023-10-28 10:00:00    # update time（option）
tags: [tag1, tag2, tag3]      # tags
categories: name             # categories
cover: images/cover.jpg         # coverpic（option）
toc: true                       # content（option）
comments: true                  # comments（option）
---
```

## Markdown Syntax Cheat Sheet（<span style="color:red">Markdown Code</span>）
```Shell
Headers
# H1 Header
## H2 Header
### H3 Header
#### H4 Header
##### H5 Header
###### H6 Header
```
> Headers
# H1 Header
## H2 Header
### H3 Header
#### H4 Header
##### H5 Header
###### H6 Header



```Shell
Text Formatting
**Bold Text** or __Bold Text__
*Italic Text* or _Italic Text_
***Bold and Italic*** or ___Bold and Italic___
~~Strikethrough Text~~
`Inline Code`
```
> Text Formatting
**Bold Text** or __Bold Text__
*Italic Text* or _Italic Text_
***Bold and Italic*** or ___Bold and Italic___
~~Strikethrough Text~~
`Inline Code`



```Shell
Unordered List:
- Item 1
- Item 2
  - Subitem 2.1
  - Subitem 2.2
- Item 3
```
> Unordered List:
- Item 1
- Item 2
  - Subitem 2.1
  - Subitem 2.2
- Item 3



```Shell
Ordered List:
1. First item
2. Second item
3. Third item
```
> Ordered List:
1. First item
2. Second item
3. Third item



```Shell
Links & Images
[Link Text](https://example.com)
[Reference Link][1]
![Image Alt Text](image-url.jpg)
![Reference Image][2]
[1]: https://example.com
[2]: image-url.jpg
```
> Links & Images
[Link Text](https://example.com)
[Reference Link][1]
![Image Alt Text](/images/posts/How-to-create-a-simple-blog/icon.svg)
![Reference Image][2]
[1]: https://example.com
[2]: /images/posts/How-to-create-a-simple-blog/icon.svg



```Shell
Tables
| Header 1 | Header 2 | Header 3 |
|----------|----------|----------|
| Cell 1   | Cell 2   | Cell 3   |
| Cell 4   | Cell 5   | Cell 6   |
```
> Tables
| Header 1 | Header 2 | Header 3 |
|----------|----------|----------|
| Cell 1   | Cell 2   | Cell 3   |
| Cell 4   | Cell 5   | Cell 6   |



```Shell
Blockquotes
> This is a blockquote
> 
> With multiple lines
> > Nested blockquote
```
> Blockquotes
> This is a blockquote
> 
> With multiple lines
> > Nested blockquote



```Shell
Horizontal Rules
---
***
___
```
> Horizontal Rules
---
***
___




```Shell
style="color:blue" -> <span style="color:blue">Blue</span>
style="color:green" -> <span style="color:green">Green</span>
style="color:#ff00ff" -> <span style="color:#ff00ff">Pink</span>
style="color:rgb(255, 165, 0)" -> <span style="color:rgb(255, 165, 0)">(RGB)</span>
```
style="color:blue" -> <span style="color:blue">Blue</span>
style="color:green" -> <span style="color:green">Green</span>
style="color:#ff00ff" -> <span style="color:#ff00ff">Pink</span>
style="color:rgb(255, 165, 0)" -> <span style="color:rgb(255, 165, 0)">(RGB值)</span>