---
title: "MySQL 多表关联查询和多次单表查询哪个好？"
excerpt_separator: "<!--more-->"
categories:
  - Blog
tags:
  - MySQL
toc: true
toc_label: "目录"
toc_icon: "cog"
---

MySQL 多表关联查询和多次单表查询哪个好？很多高性能的应用都会对关联查询进行分解。简单地，可以对每一个表进行一次单表査询，然后将结果在应用程序中进行关联。

<!--more-->

例如，下面这个查询：

```mysql
mysql> SELECT FROM tag
    ->    JOIN tag_post ON tag_post,tag_id=tagid
    ->    JOIN post ON tag_post post_id=post.id
    -> WHERE tag.tag='mysql';
```

可以分解成下面这些查询来代替:

```mysql
mysql> SELECT * FROM tag WHERE tag='mysql';
mysql> SELECT * FROM tag_post WHERE tag_id=1234;
mysql> SELECT * FROM post WHERE post.id in (123,456,567,9098,8904);
```

## 分解关联查询的优势
- **让缓存的效率更高。** 许多应用程序可以方便地缓存单表查询对应的结果对象。对 MYSQL 的查询缓存来说，如果关联中的某个表发生了变化，
那么就无法使用查询缓存了，而拆分后，如果某个表很少改变，那么基于该表的查询就可以重复利用查询缓存结果了。
- **将查询分解后，执行单个查询可以减少锁的竟争。**
- **在应用层做关联，可以更容易对数据库进行拆分，更容易做到高性能和可扩展。**
- **查询本身效率也可能会有所提升。** 这个例子中，使用 `IN()` 代替关联査询，可以让 MYSQL 按照 ID 顺序进行査询，这可能比随机的关联要更高效。
- **可以减少冗余记录的查询。** 在应用层做关联査询，意味着对于某条记录应用只需要查询一次，而在数据库中做关联查询，则可能需要重复地访问一部分数据。从这点看，这样的重构还可能会减少网络和内存的消耗。
- 更进一步，这样做相当于在应用中实现了哈希关联，而不是使用 MYSQL 的嵌套循环关联。某些场景哈希关联的效率要高很多

> 本文摘选自[《高性能 MySQL》（第3版）](https://u.jd.com/6xAiou8)

![《高性能 MySQL》](https://i.loli.net/2021/07/07/B4TkKnQiyUGcEXN.jpg)
