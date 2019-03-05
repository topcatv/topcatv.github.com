title: 系统数据容量获取
date: 2019-01-14 17:26:31
tags: mysql,mongodb,数据容量
---

# 统计系统数据容量

## MySQL数据库

- 查看所有数据库容量大小
```sql
select table_schema                                 as '数据库',
       sum(table_rows)                              as '记录数',
       sum(truncate(data_length / 1024 / 1024, 2))  as '数据容量(MB)',
       sum(truncate(index_length / 1024 / 1024, 2)) as '索引容量(MB)'
from information_schema.tables
group by table_schema
order by sum(data_length) desc, sum(index_length) desc;
```
<!-- more -->

- 查看所有数据库各表容量大小
```sql
select table_schema                            as '数据库',
       table_name                              as '表名',
       table_rows                              as '记录数',
       truncate(data_length / 1024 / 1024, 2)  as '数据容量(MB)',
       truncate(index_length / 1024 / 1024, 2) as '索引容量(MB)'
from information_schema.tables
order by data_length desc, index_length desc;
```

- 查看指定数据库容量大小
```sql
select table_schema                                 as '数据库',
       sum(table_rows)                              as '记录数',
       sum(truncate(data_length / 1024 / 1024, 2))  as '数据容量(MB)',
       sum(truncate(index_length / 1024 / 1024, 2)) as '索引容量(MB)'
from information_schema.tables
where table_schema = 'mysql';
```

- 查看指定数据库各表容量大小
```sql
select table_schema                            as '数据库',
       table_name                              as '表名',
       table_rows                              as '记录数',
       truncate(data_length / 1024 / 1024, 2)  as '数据容量(MB)',
       truncate(index_length / 1024 / 1024, 2) as '索引容量(MB)'
from information_schema.tables
where table_schema = 'mysql'
order by data_length desc, index_length desc;
```

## MongoDB数据库

```javascript
db.stats();

{
    "db" : "xxx",   //当前数据库
    "collections" : 27,  //当前数据库多少表 
    "objects" : 18738550,  //当前数据库所有表多少条数据
    "avgObjSize" : 1153.54876188392, //每条数据的平均大小
    "dataSize" : 21615831152.0,  //所有数据的总大小
    "storageSize" : 23223312272.0,  //所有数据占的磁盘大小 
    "numExtents" : 121,
    "indexes" : 26,   //索引数 
    "indexSize" : 821082976,  //索引大小 
    "fileSize" : 25691160576.0,  //预分配给数据库的文件大小
    "nsSizeMB" : 16,
    "dataFileVersion" : {
        "major" : 4,
        "minor" : 5
    },
    "extentFreeList" : {
        "num" : 1,
        "totalSize" : 65536
    },
    "ok" : 1.0
}
```
默认返回数据单位是`bytes`

可以通过传参数，比如`db.stats(1024)`得到的是kb单位

`db.stats(1073741824);`得到的是G单位
```javascript
{
    "db" : "xxxx",
    "collections" : 27,
    "objects" : 18736680,
    "avgObjSize" : 1153.53257375373,
    "dataSize" : 20,  //所有数据的总大小
    "storageSize" : 21, //所有数据占的磁盘大小 
    "numExtents" : 121,
    "indexes" : 26,
    "indexSize" : 0,
    "fileSize" : 23,  //预分配给数据库的文件大小
    "nsSizeMB" : 16,
    "dataFileVersion" : {
        "major" : 4,
        "minor" : 5
    },
    "extentFreeList" : {
        "num" : 1,
        "totalSize" : 0
    },
    "ok" : 1.0
}
```

这里的`objects`以及`avgObjSize`还是`bytes`为单位的，不受参数影响

