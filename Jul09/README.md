# 07月09日

##关键词

爬虫 HttpClient OkHttp htmlparser Shell

***

##1.爬虫

参考文档

1.[零基础写Java知乎爬虫之准备工作](http://www.jb51.net/article/57189.htm)

2.[零基础写Java知乎爬虫之先拿百度首页练练手](http://www.jb51.net/article/57193.htm)

3.[零基础写Java知乎爬虫之进阶篇](http://www.jb51.net/article/57207.htm)

>宽度优先爬虫的基本流程：

>1.把解析出的链接和 Visited 表中的链接进行比较，若 Visited 表中不存在此链接， 表示其未被访问过;

>2.把链接放入 TODO 表中;

>3.处理完毕后，从 TODO 表中取得一条链接，直接放入 Visited 表中;

>4.针对这个链接所表示的网页，继续上述过程。如此循环往复;

4.[零基础写Java知乎爬虫之获取知乎编辑推荐内容](http://www.jb51.net/article/57197.htm)

5.[零基础写Java知乎爬虫之抓取知乎答案](http://www.jb51.net/article/57203.htm)

6.[零基础写Java知乎爬虫之将抓取的内容存储到本地](http://www.jb51.net/article/57206.htm)

http://gkcx.eol.cn/soudaxue/querySchoolSpecialty.html?zycengci=%E6%9C%AC%E7%A7%91&argspecialtyname=%E5%93%B2%E5%AD%A6&argkeyword=%E5%93%B2%E5%AD%A6&form=form

##2.Shell(Bash)

Bash的变量都是默认为字符串型的，由字母或者下划线打头，中间只能由数字、字母和下划线

Bash变量类型有以下几类：字符串型、整型、浮点和日期型

Bash变量的分类：

###1.用户自定义变量

定义变量：变量名=变量值（等号前后不能有空格，如果有空格就认为是系统命令，字符串用引号包围）

调用变量：$变量名（在定义变量的时候不需要添加$，但是在调用的时候要用$）

变量叠加：x=123 x="$x"456 ==> x=123456

变量查看：set+变量名 （系统中全部生效的变量） -u 调用未声明的变量时会报错（默认情况下不会）

变量删除：unset+变量名（变量名前不加$）

###2.环境变量（保存系统操作环境中与i环境相关的数据）

环境变量是全局变量，用户自定义变量是局部变量。

声明环境变量：export 变量名=变量值 

查看环境变量：env  删除同自定义变量

常用环境变量==>PATH变量:系统查找命令的路径

###3.位置参数变量；用来向脚本当中传递参数或数据。变量名不能自定义，变量作用是固定的；

###4.预定义变量；变量名和作用都是固定的；
