redis-understand.md
---
---

ref：https://xiaolincoding.com/redis/

## 一、数据类型

### 1. 基础数据类型

对redis来说，所有的key都是字符串类型，因此这里主要看的是支持的值类型。redis支持五中数据类型，分别是：

- string
- hash
- list
- set
- sorted set

### 2. 底层的数据结构实现

redis为了实现操作时的多态，实现了一套对象结构（redisObject）。所以redis每种对象的实际包含了两部分：对象结构（redisObject） 、 实际的数据结构。

对象结构中，主要保存的是对象的元信息，包括：
- 数据类型：4bit
- 实际数据编码类型：4bit
- lru或lfu：24bit（8bit频率，16bit时间）
- 引用计数：32bit
- 实际数据结构的指针：32bit

在有了对象结构实现多态的基础上，redis实现了多种底层的数据结构来支持上面提到的哪些数据类型。每种数据类型会根据当时的使用情况，选用具体的数据结构实现。

- 简单动态字符串：sds
- 压缩列表：ziplist
- 快表：quicklist
- 字典/哈希表：dict
- 整数集：IntSet
- 跳表：ZSkipList

#### 2-1 简单动态字符串 - sds

### 3. 特殊数据类型

## 二、打满之后的key清理策略(内存淘汰策略)
所有的redis内存淘汰策略共分为两类：
- 不进行数据淘汰（redis 3.0的默认策略，再提交会报错）
- 进行数据淘汰
  - 在设置了过期时间的数据范围进行淘汰
    - volatile-random：随机淘汰设置了过期时间的任意键值；
    - volatile-ttl：优先淘汰更早过期的键值。
    - volatile-lru（Redis3.0 之前，默认的内存淘汰策略）：淘汰所有设置了过期时间的键值中，最久未使用的键值；
    - volatile-lfu（Redis 4.0 后新增的内存淘汰策略）：淘汰所有设置了过期时间的键值中，最少使用的键值；
  - 在所有数据范围内进行淘汰：
    - allkeys-random：随机淘汰任意键值;
    - allkeys-lru：淘汰整个键值中最久未使用的键值；
    - allkeys-lfu（Redis 4.0 后新增的内存淘汰策略）：淘汰整个键值中最少使用的键值。

## 三、布隆过滤器的实现及优缺点

布隆过滤器一般基于bitmap实现，

问题：
1. 有误差
2. 无法删除数据，只能重新构建