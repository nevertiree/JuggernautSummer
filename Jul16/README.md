# 07月16日

##关键词

MySQL正则表达式 MySQL数据导出

##1.MySQL正则表达式

[MySQL正则表达](http://www.cnblogs.com/way_testlife/archive/2010/09/17/1829567.html)式仅支持多数正则表达式实现的一个很小的子集

1. 基本字符匹配

```sql

SELECT prod_name FROM products WHERE prod_name REGEXP '1000' ORDER BY prod_name;

```

2. OR匹配

```sql

SELECT prod_name FROM products WHERE prod_name REGEXP '1000|2000' ORDER BY prod_name;

```

##2.MySQL数据导出
