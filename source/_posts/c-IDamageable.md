---
title: C# IDamageable
date: 2025-09-03 07:57:26
tags:
  - Godot
categories:
  - Game
  - C#
cover: /images/cover.jpg  # 站内图片
---
# Using IDamageable to check can be attacked
```C#
//using this interface check Node
public interface IDamageable
{
    void TakeDamage(float damage);
    bool IsDestroyed { get; }
}
// interface used
public partial class Enemy : CharacterBody2D, IDamageable {}
 ```

# when Attack check Node, and call the funciton
```C#
if (area is IDamageable damageable && !damageable.IsDestroyed)
{
    //can using this check, but IDamageable must have this method
    if (body.HasMethod("TakeDamage"))
    {
        //call the function
        body.Call("TakeDamage", 10);
    }
}


 ```