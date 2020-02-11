# Redis的基本数据结构

## 1. 概述

Redis中数据存储是键值对的形式,其中值的形式有多种,可以是String,List,SET,HASH,Sorted Set的形式

## 2. 键(key)
1. keys *:可以找到该库中所有的key,其中keys后面的内容为匹配模式
2. EXISTS key:用于判断库中是否存在该key
3. move key 库号:将key移动到另一个库中
4. expire key 秒数:设置某个key的有效时间
5. ttl key:查看某个key还有多久过期,-1表示不过期
6. type key:查看该key的类型
7. del key:直接删除某个键

## 3. 值(value)

### String

**单值单value**

1. append key value:在key对应的value后面追加value
2. strlen key:查看某个key对应value的长度
3. Incr/decr/incrby/decrby 对value的值减去对应value的值

    - Incr key:将key对应的value的值加上1
    - decr key:将key对应的value的值减去1
    - incrby key number:将key对应的value的值加上number所指定的值
    - decrby key number:将key对应的value的值减去number所指定的值
    
4. getrange key beginindex endindex:获取key对应value的一部分的值
5. setrange key index value:将key对应的value从某个下标开始赋值

        127.0.0.1:6379> GETRANGE k3 0 -1
        "444"
        127.0.0.1:6379> SETRANGE k3 1 22
        (integer) 3
        127.0.0.1:6379> get k3
        "422"

6. setex key 时间 value:在设置key-value时也设置了超时时间
7. setnx key value:如果key已经存在,那么不会对其覆盖
8. mset k1 v1 k2 v2 ...:多个键值对的存储
9. mget k1 k2 ..:同时获取多个key对应的value
10. msetnx k1 v1 k2 v2:即使其中只有一个key存在,那么此次赋值也会失败

### List

**底层的数据结构是链表,模式时单值多value**

1. LPUSH key value(数据列表):可以想象成是从左往右来压栈,所以在链表的头部的是最后压入的值  
2. RPUSH key value(数据列表):可以想象成是从右往左来压栈,所以在链表的头部的是最后压入的值
3. IRANGE key begin end:获取对应key的链表中的数据,其中begin和end指定开始结束的指针,如果end值为-1,则表明到链表最后
4. LPOP key:将链表的左边元素弹出
5. RPOP key:将链表的右边元素弹出
6. LINDEX key 下标:获取链表对应下标的值
7. LLEN key:获取对应链表的长度
8. Lren key N value:删除链表中N个值为value的节点
9. LTRIM key begin end:截取链表的某段值再赋给该key
10. RPOPLPUSH 源列表 目标链表:从源链表右边弹出再从左链表左边压入
11. LSET key index value:将key链表中index索引所指定的值用value来替代
12. LINSERT key before/after 值1 值2:在链表中第一个出现值1的位置(前面/后面)加入值2

### SET集合

**模式时当值多value**

1. sadd key 元素列表
2. SMEMBERS key:展示该key对应的set中所有的元素
3. SISMEMBER key value:value是否在该key对应的set集合中
4. SCARD:获取set集合元素中元素的个数
5. SREM key value:删除key中对应的集合的元素value
6. SPOP key:随机移除set集合中某个元素
7. smove key1 key2 key1对应set中某个值:将这个值放到key2对应的set中
8. 集合操作

    - sdiff key1 key2:将key1的元素减去key2元素(差集)
    - sinter:交集
    - sunion:并集
    
### HASH(哈希)

**值的形式是一个键值对**

1. hset key 键值对:将键值对存储到这个key对应的hash表中
2. hget key 键:获取key对应的hash表中对应键的值
3. Hmset key 多个键值对:获取key对应的hash表,然后往其中插入多个键值对
4. HGETALL key:获取该key对应的hash表中所有的键值对
5. HDEL key 键:删除键对应的hash表中对应键的键值对
6. HLEN 键:获取该键对应的hash表对应的键值对的数目
7. HEXISTS KEY 键:判断key对应的hash表中是否有该键的存在
8. hkeys key:获取key对应的hash表中所有的key
9. hvals key:获取key对应的hash表中所有的val
10. HINCRBY key 键值 value:获得key对应的hash中对应键值的值,然后加上value
11. HINCRBYFLOAT:跟上面函数的功能相似,不过是可以加上浮点值
12. Hsetnx key 键值对:在key对应的hash表中加入对应的键值对

### Sorted Set

**value同样为键值对,键为score,值为value**

1. zadd key 键值对:为key对应的sorted set中插入(score,value)
2. ZRANGE key beginindex endindex:获取该范围内所有的value,加上withscores可以将score的值也打印出来
3. ZRANGEBYSCORE  key 开始score 结束score:用于获取key对应的hash表的score范围内的key
4. zrevRANGEBYSCORE  key 大score 小score:获取从大到小的score的value

    - limit 开始坐标 个数:用于限制查找的开始坐标及查找个数
    
5. zrem key value:将set集合中某个值删去
6. ZCARD key:获取key对应的hash表的键值对的个数
7. ZCOUNT key score区间:在这个区间范围的元素的个数
8. ZRANK key value:可以获取这个value的索引的下标
9. ZSCORE key value:得到对应value对应的score
10. zrevrank  key value:逆序后获得这个value的下标
11. zrevrange key beginindex endindex:先逆序再将按序将数据展示出来