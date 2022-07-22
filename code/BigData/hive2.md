---
title: OPeration Of Hive
categories: [Code,BD]
tags:
  - Hive
  - BD
date: 2022-07-21 18:30:09
banner_img: https://cdn.jsdelivr.net/gh/MuMu-HuaHua/picgo/202207201617927.jpg
toc: true
---
<!--more-->
### 基操
启动hive，可以通过`/bin/hive`访问hive客户端，也可以通过beeline远程访问

#### HIVE SQL
-e 不进入 hive 的交互窗口执行 sql 语句  `bin/hive -e "select id from student;"`
-f 执行脚本中 sql 语句                 `bin/hive -f /opt/module/datas/hivef.sql`

##### HIVE SQL执行流程
数据来源、过滤数据、查询数据、显示数据
``` sql
from     join    # (left/right/inner/outner)
on
where
group by
having
select
distinct
order by
limit
union/union all
```
##### 注意事项
1.  列裁剪和分区裁剪
    1.  列剪裁
    2.  分区剪裁
        1.  对分区表进行分区字段进行过滤，不做分区操作Hive将做全表扫描
        2.  `select 字段A，字段B from table where 分区字段 = 20220101 and id = ’aaa‘;`
2. 表连接
   1. 不要用错主键
   2. 小表在前、大表在后
   3. 建立中间表（join操作较多时）
   4. 尽早过滤数据，在子查询或者中间表中过滤数据，同时join后面不要跟where条件，因为这样会导致匹配的时候全表join
   5. ``` SQL
      # 错误示范
      select ... from table1 A
      inner join table2 B
      on A.key = B.key
      where A.id>1 and A.id<10
      and B.date > '2022-01-01' and B.date < '2022-07-01';
      # 标准示范

      
      SELECT  A.col1,A.col2,B.col3,B.col4
      FROM(SELECT col1,col2
           FROM database.table1
           WHERE id >1 and id < 10 >) A
      INNER JOIN ( SELECT col3,col4
           FROM database.table2 
           WHERE date > '2022-01-01' and date < '2022-07-01') B
      on A.key = B.key;
      ```
3. 避免数据倾斜
4. 避免笛卡尔积
5. order by
   1. 使用order by必须带着limit ，因为oder by是全局排序，速度非常慢
   2. 或使用sort by 代替order by
6. group by
   1. distinct 使用group by 替代
   2. 比如要对某列去除计数，不要使用count(distinct)， 要使用 group by
7. 规范
   1. 代码规范及缩进
   2. 注释