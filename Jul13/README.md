# 07月13日

##关键词

Spring Controller Service DAO MyBatis 

##1.后台整体架构理解

数据交互流程图

>用户上传信息 --> Web --> Tomcat服务器 --> Spring Controller --> Spring Service --> DAO(MyBatis) --> DB(MySql)

##2.Spring Controller

1.@Autowired --> Service Interface

2.Value Object --> Encryted ClientInfoBaseVO --> Encryted Base VO

byte[]

3.执行service业务逻辑并得到返回结果

4.把执行结果返回到Map中

```
key="success" value="boolean"

key="res" value="List<>"

```

5.GsonUtil.toJson(Map)结果返回标准的JSON值；

##3.Spring Service

1.@Autowired --> Mapper(relate to the database Table)

2.业务方法中执行mapper类（DAO）的DML和DQL语句

3.向Controller返回结果

##4.Data Access Object(DAO)

由db的table自动生成MyBatis配置

在表名对应的Mapper中进行修改（Mapper中已经有一些自动生成的SQL语句，注意区分）

##5.Object Relationship Model(ORM)

数据库的基本结构是table，table的基本结构是row；

Java的基本结构是class，class的基本结构是object；

故ORM意味着每一个Object对应table中的row，通过这种方式快速完成数据存储；

以add业务为例

1.new Java Object (工具生成domain类)

2.定义Mapper中的SQL语句（自行添加）；

3.MyBatis自动执行，返回有几条语句收到了影响；（update && delete 同）

4.查询时通过SQL从db中取出多行，由mybatis根据Mapper中的SQL语句执行

5.根据mapper.xml把记录返回成VO对象；
