---
title: C++ 工具函数
date: 2026-01-12 05:36:21
tags:
 -Game
 -C++
---
## C++单例模式
```
class Singleton {
public:
    static Singleton& getInstance() {
        static Singleton instance;
        return instance;
    }
    void doSomething() {
        // do something
    }
private:
    Singleton() {} // 私有构造函数
    Singleton(const Singleton&) = delete; // 禁止拷贝构造函数
    Singleton& operator=(const Singleton&) = delete; // 禁止赋值操作符
};
//使用
Singleton& instance = Singleton::getInstance();
instance.doSomething();
```
## 去除UTF_8字符串最后一个字符
```
//区分中文，英文
void SceneEnd::removeLastUTF8Char(std::string &str)
{
   if (str.empty()) return;

    // 从字符串末尾向前探测
    size_t lastPos = str.length() - 1;

    // UTF-8 的后续字节（Trailing bytes）都以 10 开头 (即 0x80 到 0xBF)
    // 我们一直向前找，直到找到该字符的起始字节
    while (lastPos > 0 && (str[lastPos] & 0xC0) == 0x80) {
        lastPos--;
    }

    // 此时 lastPos 指向最后一个字符的起始位置
    str.erase(lastPos);
}
```
## map排序
```
//按照int从大到小排序
std::map<int, std::string, std::greater<int>> rank;


```