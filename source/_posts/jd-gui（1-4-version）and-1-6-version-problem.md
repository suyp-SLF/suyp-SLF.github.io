---
title: jd-gui（1.4 version）and (1.6 version) problem
date: 2025-09-05 18:34:21
tags:
  - Java
  - jd-gui
  - compile
categories:
  - Technology
cover: /images/cover.jpg  # 站内图片
---
# 1. Introduce
compile software can't open
![software](/images/posts/jd-gui（1-4-version）and-1-6-version-problem/1.png)

# 2. I try to run this software
```Shell
#java执行jar文件
$ /usr/bin/java -jar /Users/suyp/TOOLS/jd-gui-1.4.0.jar
```
# 3. Error stack
Version problem, the new version using new function, but old Java can't allow

Caused by: java.lang.reflect.InaccessibleObjectException: Unable to make private java.lang.System() accessible: module java.base does not "opens java.lang" to unnamed module @768debd

# 4. Solve the problem
maybe computer update the Java Version, try to use old version Java run
## 4.1 find the Environment's Java Verion
```Shell 
$ java -version
openjdk version "22.0.2" 2024-07-16
OpenJDK Runtime Environment Zulu22.32+15-CA (build 22.0.2+9)
OpenJDK 64-Bit Server VM Zulu22.32+15-CA (build 22.0.2+9, mixed mode, sharing)
```
## 4.2 check versions list
```Shell 
/usr/libexec/java_home  -V
Matching Java Virtual Machines (2):
    22.0.2 (arm64) "Azul Systems, Inc." - "Zulu 22.32.15" /Library/Java/JavaVirtualMachines/zulu-22.jdk/Contents/Home
    1.8.0_322 (arm64) "Azul Systems, Inc." - "Zulu 8.60.0.21" /Library/Java/JavaVirtualMachines/zulu-8.jdk/Contents/Home
/Library/Java/JavaVirtualMachines/zulu-22.jdk/Contents/Home
```
## 4.3 reason: Both Java 22 and 8 version, defalult 22 Version
```Shell 
# setting Java_Home
$ open -e ~/.bash_profile

# add text
export JAVA_HOME=/Library/Java/JavaVirtualMachines/zulu-8.jdk/Contents/Home

# effect Setting
$ source ~/.bash_profile

#在执行就能够正常打开了
```
![software](/images/posts/jd-gui（1-4-version）and-1-6-version-problem/2.png)

# 5. Following problem
I find having something other error, It just using in shell command. When double click the Application, it will also have problem.
Or whenn open a new Shell, it also have this question.
## 5.1 find issue is the setting file is different
Mac Book using Zsh Shell(macOS Catalina version and later version), the old version using Bash
```Shell
#check the using shell
echo $SHELL

#if output
/bin/zsh 
#modify file: ~/.zshrc
-----------------------------
#if output
/bin/bash 
#modify file: ~/.bash_profile
```
I find my Mac using Zsh, so I use .zshrc setting file, so when i close shell the setting source file can't effect.
so the question reson is found.
## 5.2 find issue double click also using 22 version
### 5.2.1 check
the double click using javalauncher.app, this app just using version java 22, that make my confused.
I found the reson in internet, say .jar file running is another envirment, so the setting is also can't effect.
so just uninstall the version 22 java. Okkk, I will alse using version 22 java.
the solution is create a script like that
```Shell
# 1. 创建一个启动脚本（固定使用 Java 8）/create a run script (just using Java 8)
echo '#!/bin/sh
exec /Library/Java/JavaVirtualMachines/zulu-8.jdk/Contents/Home/bin/java -jar "$@"' > ~/java8-launcher.sh

# 2. 赋予执行权限 / Auth
chmod +x ~/java8-launcher.sh

# 3. 右键任意 .jar 文件 → 显示简介 → "打开方式" → 选择其他... /using this .sh script to run the .jar file
# 4. 选择刚才创建的脚本 ~/java8-launcher.sh，并点击"全部更改" /run
```
### 5.2.2 stack error
```Shell
Exception in thread "main" java.lang.ExceptionInInitializerError
at java.base/jdk.internal.misc.Unsafe.ensureClassInitialized0(Native Method)
at java.base/jdk.internal.misc.Unsafe.ensureClassInitialized(Unsafe.java:1160)
at java.base/jdk.internal.reflect.MethodHandleAccessorFactory.ensureClassInitialized(MethodHandleAccessorFactory.java:340)
at java.base/jdk.internal.reflect.MethodHandleAccessorFactory.newMethodAccessor(MethodHandleAccessorFactory.java:71)
at java.base/jdk.internal.reflect.ReflectionFactory.newMethodAccessor(ReflectionFactory.java:154)
at java.base/java.lang.reflect.Method.acquireMethodAccessor(Method.java:726)
at java.base/java.lang.reflect.Method.invoke(Method.java:577)
at org.codehaus.groovy.reflection.CachedMethod.invoke(CachedMethod.java:90)
at groovy.lang.MetaMethod.doMethodInvoke(MetaMethod.java:324)
at groovy.lang.MetaClassImpl.getProperty(MetaClassImpl.java:1845)
at groovy.lang.MetaClassImpl.getProperty(MetaClassImpl.java:3735)
at org.codehaus.groovy.runtime.callsite.ClassMetaClassGetPropertySite.getProperty(ClassMetaClassGetPropertySite.java:48)
at org.codehaus.groovy.runtime.callsite.AbstractCallSite.callGetProperty(AbstractCallSite.java:291)
at org.jd.gui.service.configuration.ConfigurationXmlPersisterProvider.getConfigFile(ConfigurationXmlPersisterProvider.groovy:21)
at org.jd.gui.service.configuration.ConfigurationXmlPersisterProvider.<clinit>(ConfigurationXmlPersisterProvider.groovy:18)
at java.base/jdk.internal.misc.Unsafe.ensureClassInitialized0(Native Method)
at java.base/jdk.internal.misc.Unsafe.ensureClassInitialized(Unsafe.java:1160)
at java.base/jdk.internal.reflect.MethodHandleAccessorFactory.ensureClassInitialized(MethodHandleAccessorFactory.java:340)
at java.base/jdk.internal.reflect.MethodHandleAccessorFactory.newConstructorAccessor(MethodHandleAccessorFactory.java:103)
at java.base/jdk.internal.reflect.ReflectionFactory.newConstructorAccessor(ReflectionFactory.java:173)
at java.base/java.lang.reflect.Constructor.acquireConstructorAccessor(Constructor.java:549)
at java.base/java.lang.reflect.Constructor.newInstanceWithCaller(Constructor.java:499)
at java.base/java.lang.reflect.Constructor.newInstance(Constructor.java:486)
at org.codehaus.groovy.reflection.CachedConstructor.invoke(CachedConstructor.java:77)
at org.codehaus.groovy.runtime.callsite.ConstructorSite$ConstructorSiteNoUnwrapNoCoerce.callConstructor(ConstructorSite.java:102)
at org.codehaus.groovy.runtime.callsite.CallSiteArray.defaultCallConstructor(CallSiteArray.java:57)
at org.codehaus.groovy.runtime.callsite.AbstractCallSite.callConstructor(AbstractCallSite.java:230)
at org.codehaus.groovy.runtime.callsite.AbstractCallSite.callConstructor(AbstractCallSite.java:234)
at org.jd.gui.service.configuration.ConfigurationPersisterService.<init>(ConfigurationPersisterService.groovy:10)
at java.base/jdk.internal.reflect.DirectConstructorHandleAccessor.newInstance(DirectConstructorHandleAccessor.java:62)
at java.base/java.lang.reflect.Constructor.newInstanceWithCaller(Constructor.java:502)
at java.base/java.lang.reflect.Constructor.newInstance(Constructor.java:486)
	at org.codehaus.groovy.reflection.CachedConstructor.invoke(CachedConstructor.java:77)
	at org.codehaus.groovy.runtime.callsite.ConstructorSite$ConstructorSiteNoUnwrapNoCoerce.callConstructor(ConstructorSite.java:102)
	at org.codehaus.groovy.runtime.callsite.CallSiteArray.defaultCallConstructor(CallSiteArray.java:57)
	at org.codehaus.groovy.runtime.callsite.AbstractCallSite.callConstructor(AbstractCallSite.java:230)
	at org.codehaus.groovy.runtime.callsite.AbstractCallSite.callConstructor(AbstractCallSite.java:234)
	at org.jd.gui.service.configuration.ConfigurationPersisterService.<clinit>(ConfigurationPersisterService.groovy)
	at java.base/jdk.internal.misc.Unsafe.ensureClassInitialized0(Native Method)
	at java.base/jdk.internal.misc.Unsafe.ensureClassInitialized(Unsafe.java:1160)
	at java.base/jdk.internal.reflect.MethodHandleAccessorFactory.ensureClassInitialized(MethodHandleAccessorFactory.java:340)
	at java.base/jdk.internal.reflect.MethodHandleAccessorFactory.newMethodAccessor(MethodHandleAccessorFactory.java:71)
	at java.base/jdk.internal.reflect.ReflectionFactory.newMethodAccessor(ReflectionFactory.java:154)
	at java.base/java.lang.reflect.Method.acquireMethodAccessor(Method.java:726)
	at java.base/java.lang.reflect.Method.invoke(Method.java:577)
	at org.codehaus.groovy.reflection.CachedMethod.invoke(CachedMethod.java:90)
	at groovy.lang.MetaMethod.doMethodInvoke(MetaMethod.java:324)
	at groovy.lang.MetaClassImpl.getProperty(MetaClassImpl.java:1845)
	at groovy.lang.MetaClassImpl.getProperty(MetaClassImpl.java:3735)
	at org.codehaus.groovy.runtime.callsite.ClassMetaClassGetPropertySite.getProperty(ClassMetaClassGetPropertySite.java:48)
	at org.codehaus.groovy.runtime.callsite.AbstractCallSite.callGetProperty(AbstractCallSite.java:291)
	at org.jd.gui.App.main(App.groovy:26)
Caused by: java.lang.reflect.InaccessibleObjectException: Unable to make private java.lang.System() accessible: module java.base does not "opens java.lang" to unnamed module @768debd
  #the reason
	at java.base/java.lang.reflect.AccessibleObject.throwInaccessibleObjectException(AccessibleObject.java:388)
	at java.base/java.lang.reflect.AccessibleObject.checkCanSetAccessible(AccessibleObject.java:364)
	at java.base/java.lang.reflect.AccessibleObject.checkCanSetAccessible(AccessibleObject.java:312)
	at java.base/java.lang.reflect.Constructor.checkCanSetAccessible(Constructor.java:194)
	at java.base/java.lang.reflect.Constructor.setAccessible(Constructor.java:187)
	at org.codehaus.groovy.reflection.CachedConstructor$1.run(CachedConstructor.java:41)
	at java.base/java.security.AccessController.doPrivileged(AccessController.java:319)
	at org.codehaus.groovy.reflection.CachedConstructor.<init>(CachedConstructor.java:39)
	at org.codehaus.groovy.reflection.CachedClass$2.initValue(CachedClass.java:76)
	at org.codehaus.groovy.reflection.CachedClass$2.initValue(CachedClass.java:66)
	at org.codehaus.groovy.util.LazyReference.getLocked(LazyReference.java:46)
	at org.codehaus.groovy.util.LazyReference.get(LazyReference.java:33)
	at org.codehaus.groovy.reflection.CachedClass.getConstructors(CachedClass.java:265)
	at groovy.lang.MetaClassImpl.<init>(MetaClassImpl.java:215)
	at groovy.lang.MetaClassImpl.<init>(MetaClassImpl.java:225)
	at groovy.lang.MetaClassRegistry$MetaClassCreationHandle.createNormalMetaClass(MetaClassRegistry.java:168)
	at groovy.lang.MetaClassRegistry$MetaClassCreationHandle.createWithCustomLookup(MetaClassRegistry.java:158)
	at groovy.lang.MetaClassRegistry$MetaClassCreationHandle.create(MetaClassRegistry.java:141)
	at org.codehaus.groovy.reflection.ClassInfo.getMetaClassUnderLock(ClassInfo.java:250)
	at org.codehaus.groovy.reflection.ClassInfo.getMetaClass(ClassInfo.java:282)
	at org.codehaus.groovy.runtime.metaclass.MetaClassRegistryImpl.getMetaClass(MetaClassRegistryImpl.java:255)
	at org.codehaus.groovy.runtime.InvokerHelper.getMetaClass(InvokerHelper.java:872)
	at org.codehaus.groovy.runtime.callsite.CallSiteArray.createCallStaticSite(CallSiteArray.java:72)
	at org.codehaus.groovy.runtime.callsite.CallSiteArray.createCallSite(CallSiteArray.java:159)
	at org.codehaus.groovy.runtime.callsite.CallSiteArray.defaultCall(CallSiteArray.java:45)
	at org.codehaus.groovy.runtime.callsite.AbstractCallSite.call(AbstractCallSite.java:108)
	at org.codehaus.groovy.runtime.callsite.AbstractCallSite.call(AbstractCallSite.java:120)
	at org.jd.gui.service.platform.PlatformService.initOS(PlatformService.groovy:19)
	at org.jd.gui.service.platform.PlatformService$initOS.callCurrent(Unknown Source)
	at org.codehaus.groovy.runtime.callsite.CallSiteArray.defaultCallCurrent(CallSiteArray.java:49)
	at org.codehaus.groovy.runtime.callsite.AbstractCallSite.callCurrent(AbstractCallSite.java:149)
	at org.codehaus.groovy.runtime.callsite.AbstractCallSite.callCurrent(AbstractCallSite.java:153)
	at org.jd.gui.service.platform.PlatformService.<init>(PlatformService.groovy:12)
	at java.base/jdk.internal.reflect.DirectConstructorHandleAccessor.newInstance(DirectConstructorHandleAccessor.java:62)
	at java.base/java.lang.reflect.Constructor.newInstanceWithCaller(Constructor.java:502)
	at java.base/java.lang.reflect.Constructor.newInstance(Constructor.java:486)
	at org.codehaus.groovy.reflection.CachedConstructor.invoke(CachedConstructor.java:77)
	at org.codehaus.groovy.runtime.callsite.ConstructorSite$ConstructorSiteNoUnwrapNoCoerce.callConstructor(ConstructorSite.java:102)
	at org.codehaus.groovy.runtime.callsite.CallSiteArray.defaultCallConstructor(CallSiteArray.java:57)
	at org.codehaus.groovy.runtime.callsite.AbstractCallSite.callConstructor(AbstractCallSite.java:230)
	at org.codehaus.groovy.runtime.callsite.AbstractCallSite.callConstructor(AbstractCallSite.java:234)
	at org.jd.gui.service.platform.PlatformService.<clinit>(PlatformService.groovy)
	... 52 more
2025-04-04 03:53:38.423 java[71892:77972757] +[IMKClient subclass]: chose IMKClient_Modern
2025-04-04 03:53:38.423 java[71892:77972757] +[IMKInputSession subclass]: chose IMKInputSession_Modern
```

The issue is JD-GUI 1.4 using new version Java 9+, 
Caused by: java.lang.reflect.InaccessibleObjectException: Unable to make private java.lang.System() accessible: module java.base does not "opens java.lang" to unnamed module @768debd