# 07月09日

##关键词

爬虫 HttpClient OkHttp htmlparser

***

##1.爬虫

参考文档

1.[零基础写Java知乎爬虫之准备工作](http://www.jb51.net/article/57189.htm)

2.[零基础写Java知乎爬虫之先拿百度首页练练手](http://www.jb51.net/article/57193.htm)

3.[零基础写Java知乎爬虫之进阶篇](http://www.jb51.net/article/57207.htm)

>宽度优先爬虫的基本流程：

1.把解析出的链接和 Visited 表中的链接进行比较，若 Visited 表中不存在此链接， 表示其未被访问过;

2.把链接放入 TODO 表中;

3.处理完毕后，从 TODO 表中取得一条链接，直接放入 Visited 表中;

4.针对这个链接所表示的网页，继续上述过程。如此循环往复;

>

4.[零基础写Java知乎爬虫之抓取知乎答案](http://www.jb51.net/article/57203.htm)

5.[零基础写Java知乎爬虫之将抓取的内容存储到本地](http://www.jb51.net/article/57206.htm)

