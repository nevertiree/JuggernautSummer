# 07月14日

##关键词

JDBC MySQL用户配置

##1.MySQL用户配置

1.进入用户管理数据库

```sql

use mysql;

```
2.创建新用户

```sql

create user user_example IDENTIFIED by 'password_example';//将纯文本密码加密作为散列值存储;

```

3.查看全部用户 确认创建情况

```sql

select host,user,password from user ;

```
4.赋予新用户权限

```sql

grant select,insert,update,delete on database_name.* to user_example;

grant create on database_name to user_example;

```
5.确认用户权限

```sql

show grants for user_example;

```

##2.Gradle

在build.gradle中加入代码后需要在命令行中`gradle copyJars`才有用

##3.JSON

[完全理解Gson](http://www.importnew.com/16630.html)

##4.单元测试

##5.断点调试
