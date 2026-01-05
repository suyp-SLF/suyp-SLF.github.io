---
title: unzip/zip unix command (.tar、.gz、.tar.gz、.zip)
date: 2023-03-20 21:09:38
tags:
  - Unix
  - Command
categories:
  - Unix
cover: /images/cover.jpg  # 站内图片
---
# .tar
#解包 // unpack
tar xvf FileName.tar
#打包 // pack
tar cvf FileName.tar DirName

# .gz
#解压1 // Unzip
gunzip FileName.gz
#解压2 // Unzip
gzip -d FileName.gz
#压缩 // zip
gzip FileName

# .tar.gz
#解压 // Unzip
tar zxvf FileName.tar.gz
#压缩 // zip
tar zcvf FileName.tar.gz DirName

#  .zip
#解压 // Unzip
unzip FileName.zip
#压缩 // zip
zip FileName.zip DirName