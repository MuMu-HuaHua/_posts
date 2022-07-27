---
title: 21年C题二周目
categories: [Statistic,Theory]
tags:
  - Statistic
  - python
  - Matlab
toc: true
date: 2022-07-28 17:30:20
update: 2022-07-29 9:30:20
latex: true
---
### 问题 && 前置条件
#### 问题
1. 量化分析各企业供货特征，建立反映保障企业生产重要性的数学模型，在此基础上确定50家最重要的供应商
2. 满足生产的需求应选择的供应商、针对这些供应商，制定 __未来 24 周每周最经济__ 的原材料订购方案，并 __据此制定损耗最少的转运方案__ 、__对订购方案和转运方案的实施效果进行分析__。
3. 现计划尽量多地采购 A 类和尽量少地采购 C 类原材料，以减少转运及仓储的成本，同时希望转运商的转运损耗率尽量少。__制定新的订购方案及转运方案，并分析方案的实施效果__
4. 该企业通过技术改造已具备了提高产能的潜力。根据现有原材料的供应商和转运商的实际情况，__确定该企业每周的产能提高有多少__，__给出未来 24 周的订购和转运方案__
   
#### 前置条件
该企业每年按 48周安排生产，需要提前制定24周的原材料订购和转运计划
- 产能&原料：产能为2.82万立方米，每立方米产品需消耗A类原材料0.6立方米，或B类原材料0.66立方米，或C类原材料0.72立方米
- 原材料库存量：大于等于两周
- 转运商：6000立方米/周
- 原料成本：A类和 B类原材料的采购单价分别比C类原材料高20%和10%
- 运输&存储成本：三者一致

### 思路



### 数据
#### 数据说明
附件格式：供应商/转运商ID+240周的对应数据
损耗率=(供货量−接收量)/供货量 x 100%
数值“0”表示没有运送。   

#### 数据处理


#### 结果导出
因为需要输出到表格文件中，所以需要代码处理，可以直接整理好后复制粘贴或者直接用代码解决
### 模型









