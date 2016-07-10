# 07月10日

##关键词

Smarty

##1.Smarty

###引入：Smarty->libs

Smarty.class.php是该类库的主文件，可以调用其他所有的功能；
SmartyBC.class.php提升该类的上下兼容性

###实例化：new Smarty()

###配置：

1.$smarty -> left_delimiter = "{";//左定界符

2.$smarty -> right_delimiter = "}";//右定界符

>在左右定界符{}之中的语句都交给smarty处理

3.$smarty -> template_dir="tpl";//html模板地址

4.$smarty -> compile_dir="template_c";//模板编译生成的文件

>因为有些smarty语法与PHP不兼任，需要先编译成可以被PHP识别的语法；

5.$smarty -> cache_dir="cache";//缓存

>以下是开启cache的另外两个配置，但通常不用其缓存机制

>$smarty -> caching = true;//开启缓存

>$smarty -> cache_lifetime =120;//缓存时间

###主要方法：

1.$smarty -> assign('articletitle',"文章标题");//赋值变量

2.$smarty -> display("template_dir");//输入模板地址，编译并展示该模板效果

高斯小时候，老师让他算从一到一百的和，他计算了这个无穷级数的和，然后一个一个的减去从一百开始的所有自然数的和。
有一次高斯证明了一条公理，但他不喜欢它，所以他又证明了它是假命题。 
高斯用奥卡姆剃刀剃胡子。
有两个厨师都以擅长做炸鸡而闻名，两人的炸鸡各有千秋。一天甲向乙请教怎么才能把炸鸡做得更酥脆，乙拒绝了。后来甲主动公开了自己的炸鸡配方和流程，乙学习后改进了自己原来的做法，他的炸鸡越做越好吃。这时甲找到乙说，我的炸鸡配方是按GPL协议发布的，所以你现在必须公开你的新炸鸡配方了！

##2.JSON

阮一峰大神的[《数据类型和Json格式》](http://www.ruanyifeng.com/blog/2009/05/data_types_and_json.html)

从结构上看，所有的data可以分解成三种类型：

1.标量（scalar）：单独的String或numbers，比如"北京"这个单独的词；

2.序列（sequence）：若干个相关的数据按照一定顺序并列在一起，又叫做array或List，比如"北京，上海"；

3.映射（mapping）：名/值对（Name/value），即数据有一个名称，还有一个与之相对应的值，这又称作hash，比如"首都：北京"。

在Java中用[org.json](https://mvnrepository.com/artifact/org.json/json)来解析


###Json的规格

1.并列的数据之间用","分隔。

2.映射用":"表示。

3.并列数据的集合（数组）用[]表示。

4.映射的集合（对象）用{}表示。

>"北京市的面积为16800平方公里，常住人口1600万人。上海市的面积为6400平方公里，常住人口1800万。"

`[`

`　　{"城市":"北京","面积":16800,"人口":1600},`

`　　{"城市":"上海","面积":6400,"人口":1800}`

`]`

###3.MySql

###配置

修改编码方式

`sudo vim /etc/my.cnf`

`[mysql]`

`default-character-set=utf8`

>mysqld主要进行服务器端的配置

`[mysqld]`

`character-set-server=utf8`

###登录操控

-V 版本 -P 端口 -u 用户 -p 密码 -h 服务器名称

###退出操作

mysql>exit / quit / \q

###修改提示符

1.在登录时用 --prompt 【\h】 (\h代表服务器名字、\D代表日期、\d代表当前数据库、\u当前用户，也可以是其他名字)

2.在登录后用 prompt 【mysql>】（mysql>是想代替的名字）

###常用命令

SELECT VERSION();显示当前服务器版本
SELECT NOW();显示当前日期时间
SELECT USER();显示当前用户

##4.gitignore


`git rm --cached <added_file_to_undo>`

