# 文档地址

https://dev.mysql.com/doc/refman/5.7/en/innodb-change-buffer.html

# 基本概念

插入缓冲的使用需要满足以下两个条件：
- 索引是辅助索引。
- 索引不是唯一的。

# 日志信息

```
-------------------------------------
INSERT BUFFER AND ADAPTIVE HASH INDEX
-------------------------------------
Ibuf: size 1, free list len 3332, seg size 3334, 95757 merges
merged operations:
 insert 82346, delete mark 38205, delete 6810
discarded operations:
 insert 0, delete mark 0, delete 0
Hash table size 6374777, node heap has 2960 buffer(s)
12013.44 hash searches/s, 676.37 non-hash searches/s
```

**seg size**显示了当前插入缓冲的大小为3334 * 16KB，大约为52MB，**free list len**代表了空闲列表的长度，**size**代表了已经合并记录页的数量。下面一行可能是我们真正关心的，因为它显示了提高性能了。Inserts代表插入的记录数，merged recs代表合并的页的数量， merges代表合并的次数。merges∶merged recs大约为3∶1，代表插入缓冲将对于非聚集索引页的IO请求大约降低了3倍。
