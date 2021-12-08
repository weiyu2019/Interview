# redis

redis的瓶颈在内存和网络带宽而不是cpu

基于此，优化就需要在前面两个方面

单线程——减少上下文切换和资源竞争redis3.0

多线程——优化网络IO



几种数据结构

字符串——set、get、setnx、incr；应用场景

哈希表——hset

集合——sadd、scard、应用场景：随机抽奖、共同关注的人

zset——zadd、ZRANK、ZREVRANK 应用场景：排行榜，热搜

bitmap—— setbit、getbit 应用场景：是否登录、签到统计 （底层实际是string ）

hyperloglog—— 应用场景：不精准统计

GEO——应用场景：经纬度统计

![image-20211031102939567](image-20211031102939567.png)





## 应用案例

1、以抖音vcr最新的留言评价为案例，所有评论需要两个功能，按照时间排序+分页显示，能够排序+分页显示的redis数据结构是什么合适？

List——用Lpush命令可以将最新的评论放到队头，但是会导致分页显示第二页的时候出现第一页的数据

Zset——ZRANK/ZREVRANK对元素进行排序



2、天猫网站首页亿级UV的Redis统计方案

hset——1.5亿的访问量，一天就得占2G，一个月60G，内存顶不住

hyperloglog——专门用于基数统计



## 布隆过滤器

它实际上是一个很长的二进制数组+一系列随机hash算法映射函数，主要用于**判断一个元素是否在集合**中。

解决**缓存穿透**——当有新的请求时，先到布隆过滤器中查询是否存在：如果布隆过滤器中不存在该条数据则直接返回；如果布隆过滤器中已存在，才去查询缓存redis，如果redis里没查询到则穿透到Mysql数据库

不能删除元素。
因为删掉元素会导致误判率增加，因为hash冲突同一个位置可能存的东西是多个共有的，
你删除一个元素的同时可能也把其它的删除了。



![image-20211031010326753](image-20211031010326753.png)



缓存击穿——双缓存机制：主A从B、先更B再A、先查A再查B、B的过期时间多于A



## 分布式锁

redis实现分布式锁

setnx+lua脚本

- 保证一个客户端只能删除自己相关的锁
- 设置过期时间并且保证锁的过期大于业务执行时间发（续约问题）

集群环境下，容错率公式：2*x +  1