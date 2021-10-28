# redis

redis的瓶颈在内存和网络带宽而不是cpu

基于此，优化就需要在前面两个方面

单线程——减少上下文切换和资源竞争redis3.0

多线程——优化网络IO



几种数据结构

字符串——set、get、setnx、incr；应用场景

哈希表——hset

集合——sadd、scard、应用场景：随机抽奖、共同关注的人

zset——zadd、应用场景：排行榜，热搜

