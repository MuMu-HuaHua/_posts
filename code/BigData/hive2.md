---
title: OPeration Of Hive
categories: [Code,BD]
tags:
  - Hive
  - BD
date: 2022-07-20 18:30:09
banner_img: https://cdn.jsdelivr.net/gh/MuMu-HuaHua/picgo/202207201617927.jpg
toc: true
---
### 基操
启动hive，可以通过`/bin/hive`访问hive客户端，也可以通过beeline远程访问
-e 不进入 hive 的交互窗口执行 sql 语句  `bin/hive -e "select id from student;"`
-f 执行脚本中 sql 语句                 `bin/hive -f /opt/module/datas/hivef.sql`


