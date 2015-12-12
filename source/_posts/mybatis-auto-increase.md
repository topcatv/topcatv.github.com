title: Mybatis里如何在insert后获得主键值
date: 2013-09-12 21:11:53
categories: [mybatis]
tags: [java,mybatis,db2]
---

在mybatis的sql配置文件里的insert标签里添加（**DB2适用**）
``` xml
<selectKey resultClass="long" keyProperty="_id">
    SELECT IDENTITY_VAL_LOCAL() FROM SYSIBM.SYSDUMMY1
</selectKey>
```
<!-- more -->

这里的`keyProperty`指的是传入参数中的属性

**Oracle适用**
``` xml
<selectKey resultClass="long" order="BEFORE" keyProperty="_id">
  SELECT SEQ_TEST.NEXTVAL FROM DUAL
</selectKey>
```

**Mysql适用**
``` xml
<selectKey resultClass="long" order="BEFORE" keyProperty="_id">
  SELECT LAST_INSERT_ID() AS id
</selectKey>
```