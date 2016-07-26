# 07月18日

##关键字

##1.MySQL增加列

查看表的字段信息：desc 表名;

查看表的所有信息：show create table 表名;

添加主键约束：alter table 表名 add constraint 主键 （形如：PK_表名） primary key 表名(主键字段);

添加外键约束：alter table 从表 add constraint 外键（形如：FK_从表_主表） foreign key 从表(外键字段) references 主表(主键字段);

删除主键约束：alter table 表名 drop primary key;

删除外键约束：alter table 表名 drop foreign key 外键（区分大小写）;

修改表名：alter table t_book rename to bbb;

添加列：alter table 表名 add column 列名 varchar(30);

删除列：alter table 表名 drop column 列名;

修改列名MySQL： alter table bbb change nnnnn hh int;

修改列名SQLServer：exec sp_rename't_student.name','nn','column';

修改列名Oracle：alter table bbb rename column nnnnn to hh int;

修改列属性：alter table t_book modify name varchar(22);

##2.MySQL插入优化

对于一些数据量较大的系统，面临的问题除了是查询效率低下，还有一个很重要的问题就是插入时间长。当导入的数据量较大时，插入操作耗费的时间相当可观。因此，提高大数据量系统的MySQL insert效率是很有必要的。

1. 一条SQL语句插入多条数据。

常用的插入语句如：
```sql

INSERT INTO `insert_table` (`datetime`, `uid`, `content`, `type`) VALUES ('0', 'userid_0', 'content_0', 0);

INSERT INTO `insert_table` (`datetime`, `uid`, `content`, `type`) VALUES ('1', 'userid_1', 'content_1', 1);

```

修改成：

```

INSERT INTO `insert_table` (`datetime`, `uid`, `content`, `type`) VALUES 
    ('0', 'userid_0', 'content_0', 0), ('1', 'userid_1', 'content_1', 1);

```

Java实现：

```java

connection.setAutoCommit(false);// 关闭自动提交，默认情况下每执行一条sql提交一次
PreparedStatement statement = connection.prepareStatement("INSERT INTO insert_table VALUES(?, ?)"); 

//记录1
statement.setString(1, "2012-12-27 11:11:11"); 
statement.setString(2, "userid_0");
statement.setString(3, "content_0");
statement.setInt(4, 0);
statement.addBatch();

//记录2
statement.setString(1, "2012-12-27 12:12:12"); 
statement.setString(2, "userid_1"); 
statement.setString(3, "content_1"); 
statement.setInt(4, 1);
statement.addBatch(); 

...

//批量执行上面语句
int [] counts = statement.executeBatch();

//Commit
connection.commit();

```

修改后的插入操作能够提高程序的插入效率。这里第二种SQL执行效率高的主要原因有两个，一是减少SQL语句解析的操作， 只需要解析一次就能进行数据的插入操作，二是SQL语句较短，可以减少网络传输的IO。

2. 在事务中进行插入处理。
把插入修改成：
```sql

START TRANSACTION;
INSERT INTO `insert_table` (`datetime`, `uid`, `content`, `type`) VALUES ('0', 'userid_0', 'content_0', 0); 
INSERT INTO `insert_table` (`datetime`, `uid`, `content`, `type`) VALUES ('1', 'userid_1', 'content_1', 1);
...
COMMIT;

```

使用事务可以提高数据的插入效率，这是因为进行一个INSERT操作时，MySQL内部会建立一个事务，在事务内进行真正插入处理。通过使用事务可以减少数据库执行插入语句时多次“创建事务，提交事务”的消耗，所有插入都在执行后才进行提交操作。

注意事项：
1. SQL语句是有长度限制，在进行数据合并在同一SQL中务必不能超过SQL长度限制，通过max_allowed_packe配置可以修改，默认是1M。
2. 事务需要控制大小，事务太大可能会影响执行的效率。MySQL有innodb_log_buffer_size配置项，超过这个值会日志会使用磁盘数据，这时，效率会有所下降。所以比较好的做法是，在事务大小达到配置项数据级前进行事务提交。
3. DML语句可以采用事务处理，DDL则不支持事务，实际上若对DDL使用事务，只有最后一条真正执行。

##3.Java equals ==

equals和==的区别本质上是基本数据类型变量和非基本数据类型变量的区别。

1. ==

8种基本数据类型（float,double,byte,short,int,long,char,boolean）的变量直接存储的是“值”，因此在用关系操作符==来进行比较时，比较的就是 “值” 本身。例如 `int n=3;int m=3;　`变量n和变量m都是直接存储的"3"这个值，所以用==比较的时候结果是true。

非基本数据类型（引用类型）的变量存储的并不是 “值”本身，而是于其关联的对象在内存中的地址。` String str1 = new String("hello");String str2 = new String("hello");`变量str1中存储的是它指向的对象在内存中的存储地址，并不是“值”本身，也就是说并不是直接存储的字符串"hello"。这里面的引用和C/C++中的指针很类似。因此在用==对str1和str2进行第一次比较时，得到的结果是false。因此它们分别指向的是不同的对象，也就是说它们实际存储的内存地址不同。

2. equals

equals方法是基类Object中的方法，因此对于所有的继承于Object的类都会有该方法。


