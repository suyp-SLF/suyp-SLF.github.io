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
## 随机数
```C++
#include <random>

// -- 随机数 -- 
std::mt19937 _random_generator = std::mt19937(std::random_device{}()); // 随机数生成器

// 随机数
float randomFloat(float min, float max) { return std::uniform_real_distribution<float>(min, max)(_random_generator);}// 生成一个[min, max)之间的随机浮点数
float randowInt(int min, int max) { return std::uniform_int_distribution<int>(min, max)(_random_generator); } // 生成一个[min, max)之间的随机整数
glm::vec2 randomVec2(glm::vec2 min, glm::vec2 max) { return glm::vec2(randomFloat(min.x, max.x), randomFloat(min.y, max.y)); } // 生成一个[min, max)之间的随机二维浮点数向量
glm::ivec2 randomIvec2(glm::ivec2 min, glm::ivec2 max) { return glm::ivec2(randowInt(min.x, max.x), randowInt(min.y, max.y)); } // 生成一个[min, max)之间的随机二维整数向量
```