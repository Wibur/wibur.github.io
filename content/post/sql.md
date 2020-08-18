+++
author = "Vancer"
title = "MySQL索引"
date = "2020-07-21"
description = "MySQL 索引相關"
tags = [
    "sql技巧",
]
categories = [
    "sql",
]
series = ["Themes Guide"]
aliases = ["migrate-from-jekyl"]
+++

## 索引欄位介紹
![](https://i.imgur.com/bSUop9S.png)
* (cardinality)基數可以分為以下兩種類型 =>
低基數-列的所有值必須相同。
高基數-列的所有值都必須唯一。
如果我們在列上施加約束以限制重複值，則使用高基數的概念。
[相關資料](https://reurl.cc/g7op1p)

* index kind =>
1.Primary Key
2.Unique index
3.Normal index
4.Fulltext index
5.Clustered index
    Every InnoDB table has a special index called the clustered index where the data for the rows is stored. 
    When you define a PRIMARY KEY on your table, InnoDB uses it as the clustered index. 
6.Secondary index
    All indexes other than the clustered index are known as secondary indexes.

* index_type 索引的實現方法 => 
1. B-Tree(平衡多路搜索树)
    B-Treey优点:
    * 磁盘磁头读写移动次数少,与比较二叉树，深度较低，叶子节点存储数据，
2. R-Tree(Rectangle  Tree)
3. HASH(main menmory)
4. FULLTEXT(全文索引）

## 索引規則
* 模糊搜尋時 除則在最右邊 其他情況索引失效
* 索引中含不等於，or時 失效
* 索引列作計算 也會失效