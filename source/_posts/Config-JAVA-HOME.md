---
title: Config JAVA_HOME
date: 2024-04-04 01:27:22
tags:
  - Java
  - JAVA_HOME
categories:
  - Java
cover: /images/posts/Cover/Java.png  # 站内图片
---

# MAC版本
jdk位置/where
一般JDK 位置 :(tip：很多命令需要sudo，不然查不到)
～/Library/Java/JavaVirtualMachines/

# 打开配置文件
open -e ~/.zshrc
open -e ~/.bash_profile

# 切换配置JAVA_HOME/change version
在配置文件添加JAVA_HOME,可以切换或者配置Java版本。
JAVA_HOME = /Library/Java/JavaVirtualMachines/zulu-8.jdk/Contents/Home
![change version](/images/posts/Config-JAVA-HOME/1.png)

# *使配置生效/active config
配置玩的文件通过命令使配置生效：
source ~/.bash_profile

# 查询所有版本/search all versions
/usr/libexec/java_home  -V
![search all versions](/images/posts/Config-JAVA-HOME/2.png)

# 进阶解决方案 \ high leve solution
```Shell
手动切换JDK版本

通过修改 ~/.bash_profile文件修改JAVA_HOME，如果没有这个文件则需要新建一个。alias是自定义命令别名，修改配置文件如下：
########################
export JAVA_8_HOME=$(/usr/libexec/java_home -v1.8)
export JAVA_9_HOME=$(/usr/libexec/java_home -v9)
export JAVA_10_HOME=$(/usr/libexec/java_home -v10)
export JAVA_11_HOME=$(/usr/libexec/java_home -v11)

alias java8='export JAVA_HOME=$JAVA_8_HOME'
alias java9='export JAVA_HOME=$JAVA_9_HOME'
alias java10='export JAVA_HOME=$JAVA_10_HOME'
alias java11='export JAVA_HOME=$JAVA_11_HOME'
#默认是Java 11
########################

执行下面命令，让文件生效
source ~/.bash_profile

切换JDK版本就执行对应命令别名，比如，我要切换成JDK9，就直接输入命令
java9
```