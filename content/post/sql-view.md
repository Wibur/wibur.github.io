+++
author = "Vancer"
title = "MySQL 視圖應用"
date = "2020-08-11"
description = "MySQL 應用"
tags = [
    "sql技巧",
]
categories = [
    "sql",
]
series = ["Themes Guide"]
+++

## 介紹
視圖本身是虛擬的表，因此不會儲存資料，只負責儲存sql語句，但須注意資料量過大時會影響資料庫效能;用來把複雜的sql語句做包裝，進而達成保護真正sql語句的目的

## 視圖創建
```sql=
CREATE VIEW "視圖表名" AS "SQL SELECT 語句";
```
## 如何簡化
```sql=
像是下面的語句
SELECT
	count(*) AS count,
	sum( price ) AS sum_price 
FROM
	tb_goods

建立一個視圖
CREATE view tb_goods_view AS
SELECT
	count(*) AS count,
	sum( price ) AS sum_price 
FROM
	tb_goods

建立後只需下
SELECT * FROM view tb_goods_view
即可達成一樣的結果
```

## 可應用場景
當專案有需求做大量統計時，可以建立一張實體表搭配觸發器使用
```sql
1. 建立視圖
create view goods_total as select count(*), sum(price) from tb_goods
```
![](https://i.imgur.com/USwL9LT.png)
```sql
2. 建立一個儲存過程(sql的函式)
DROP PROCEDURE refresh_goods_now;
CREATE PROCEDURE refresh_goods_now ()
BEGIN
    TRUNCATE TABLE tb_goods_total;
    INSERT INTO tb_goods_total SELECT * FROM goods_total;
END;
';
```
![](https://i.imgur.com/Nph0pNU.png)
```sql
3. 在想要更新的時間點呼叫即可
CALL refresh_goods_now()
```
## 作法
簡單說就是將需要計算的資料，提前計算好統一存在另一張實體表，當需要統計的數值時，直接從表上取得預先計算好的值，不用每次都重新計算一遍

## Demo
![](https://i.imgur.com/jp1k8ep.png)
可以看到只是簡單的統計數量700W筆就已經需要將近兩秒
就更不用算上代碼執行網路延遲等時間~
![](https://i.imgur.com/uecbSY3.png)
再使用了上述方法後時間可以幾乎不記，但是此種做法就需要考量資料的顯示是否需要即時性，如不需要即時性，則可以搭配排程在特定時間呼叫函式做更新即可;
如果需要要即時性，則需要搭配事件或是觸發器來保證資料表內的資料永遠是最新的統計

## 補充
procedure和觸發器的區別

|          | 執行函式 |
| -------- | -------|
| procedure| 主動    |
| 觸發器    | 被動    |

## 參考資料
[SQL語法教學](https://www.1keydata.com/tw/sql/sql-create-view.html)


