# 07月25日

Mybatis Generator是一个mybatis工具项目，用于生成mybatis的model,mapper,dao持久层代码。Mybatis Generator提供了maven plugin,ant target，java三种方式启动。现在主流的构建工具是gradle,虽然mybatis generator没有提供gradle的插件，但gradle可以调用ant任务，因此，gradle也能启动Mybatis Generator。

spring可以通过指定classpath*:与classpath:前缀加路径的方式从classpath加载文件,如bean的定义文件.classpath*:的出现是为了从多个jar文件中加载相同的文件.classpath:只能加载找到的第一个文件.

cn.nevertiree.domain文件夹和cn/nevertiree/domain虽然看起来一样，但是结构上大相径庭

4.
我这里总结了判断记录是否存在的常用方法： 

sql语句：select count(*) from tablename; 

然后读取count(*)的值判断记录是否存在。对于这种方法性能上有些浪费，我们只是想判断记录记录是否存在，没有必要全部都查出来。 

以下这个方法是我推荐的。 

sql语句：select 1 from tablename where col = col limit 1; 

然后读取语句执行所影响的行数。 

当然这里limit 1很重要。这要mysql找到一条记录后就不会在往下找了。这里执行所影响的行数不是0就是1，性能提高了不少。 

如果你用的是PDO,可以用rowCount(),很容易就都到执行所影响的行数。 

这里还有人可能会去读取sql语句查询到的记录，然后判断记录是否存在，从而判断记录是否存在。这个方法虽然可行，但对于我们的要求来说，还是有些浪费，我们不需要查询到的记录，所有性能上会有损失。这里不推荐。
