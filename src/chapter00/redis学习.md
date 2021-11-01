# redis学习

## 一、数据结构与对象

## SDS

1. 定义

   simple dynamic string 简单动态字符串

   ```c
   struct sdshdr{
       //记录buf数组中已使用的字节数量
       //等于sds所保存字符串的长度
       int len;
       //记录buf数组中未使用字节的数量
       int free;
       //字节数组，用于保存字符
       char buf[]
   }
   ```

2. 优化点
   - 常数复杂度获取字符串长度，直接读取len的值
   - 空间预分配——分配可用空间+未使用的空间
   - 惰性空间释放——不会立即释放，而是记录到free字段中



## 链表List

1. 每个链表使用一个list结构来表示，这个结构带有表头指针、表尾指针以及链表长度等信息
2. redis的链表是无环链表



## 字典Map

1. redis中的字典使用哈希表作为底层实现，一个**哈希表**里面可以有多个**哈希表节点**，而每个哈希表节点就保存了一个键值对；另外，每个字典带有两个哈希表，一个平时使用(**ht[0]**)，另一个仅在进行rehash时使用(**ht[1]**)

2. 哈希表结构

   ```c
   typedef struct dictht{
       //哈希表节点数组
       dictEntry **table;
       
       //哈希表大小
       unsigned long size;
       
       //哈希表大小掩码，用于计算索引值
       //总是等于size - 1
       unsigned long sizemask;
       
       //该哈希表已有节点的数量
       unsigned long used;
   }dictht;
   ```

![image-20210618173620369](C:\Users\weiyu02\AppData\Roaming\Typora\typora-user-images\image-20210618173620369.png)

3. 哈希表节点

   ```c
   typedef struct dictEntry{
       //键
       void *key;
       
       //值
       union{
           void *val;
           uint64_tu64;
           int64_ts64;
       }v;
       struct dictEntry *next;
   }dictEntry;
   ```

4. 字典

   ```c
   typedef struct dict {
       //类型特定函数
       dictType *type;
       
       //私有数据
       void *privdata;
       
       //哈希表
       dictht ht[2];
       
       //rehash索引
       //当rehash不在进行时，值为-1
       int trehashidx;
   }dict;
   ```

   ![image-20210618174448570](C:\Users\weiyu02\AppData\Roaming\Typora\typora-user-images\image-20210618174448570.png)

5. 哈希冲突

   使用链地址法来解决哈希冲突

6. rehash

   - 为字典的ht[1]分配空间——扩容时，空间大小为 大于等于 ht[0].used * 2 的 2的n次幂；缩容时，大小为 大于等于 ht[0].used 的 2的n次幂
   - 将保存在ht[0]中的所有键值对rehash到ht[1]上面：rehash指的是重新计算键的哈希值和索引值，然后将键值对放置到ht[1]哈希表的指定位置上
   - 当迁移完成后，释放ht[0]，并将ht[1]设置为ht[0]，并在ht[1]分配一个空白哈希表



## 跳表skiplist

1. redis使用skiplist来作为有序集合键的底层实现之一

   ![image-20210618182059056](C:\Users\weiyu02\AppData\Roaming\Typora\typora-user-images\image-20210618182059056.png)

2. zskiplistNode

   ```c
   typedef struct zskiplistNode{
       
   }
   ```

   
