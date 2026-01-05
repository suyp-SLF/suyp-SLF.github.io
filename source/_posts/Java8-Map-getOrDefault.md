---
title: Java8 Map .getOrDefault
date: 2024-04-10 04:13:20
tags:
  - Java
  - Java8
  - Map
categories:
  - Java
cover: /images/posts/Cover/Java.png  # 站内图片
---

```Java
Map<String, Integer> map = new HashMap<>();
map.put("apple", 10);
map.put("banana", 20);

// Key exists
int appleCount = map.getOrDefault("apple", 0); // Returns 10
System.out.println("Apple count: " + appleCount);


// Key does not exist
int orangeCount = map.getOrDefault("orange", 0); // Returns 0 (default)
System.out.println("Orange count: " + orangeCount);
```