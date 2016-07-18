# 07月13日

##关键词

Spring Controller Service DAO MyBatis && MyBatis Generator

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

##6.MyBatis Generator(MBG)

[MBG中文介绍](http://generator.sturgeon.mopaas.com/)

>MBG是Mybatis代码生成器。它可以生成Mybatis各个版本的代码，可以内省数据库的表（或多个表）然后生成可以用来访问（多个）表的基础对象。这样和数据库表进行交互时不需要创建对象和配置文件(前提是你已经建立好了数据表，MBG才有可能从你的数据表中读出构造)。MBG解决了对数据库操作有最大影响的一些简单的CRUD操作。但用户仍然需要对联合查询和存储过程手写SQL和对象。

[MBG快速入门指南](http://generator.sturgeon.mopaas.com/quickstart.html)

1.创建并填写适当的配置文件

先把经常使用的常量写入generator.properties

```

jdbc.driverLocation=???/mysql-connector-java-5.1.34.jar

jdbc.driverClass=com.mysql.jdbc.Driver

jdbc.connectionURL=jdbc:mysql://localhost:????/??

jdbc.userId=root

jdbc.password=password

```
配置文件的基本架构如下

```xml

<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE generatorConfiguration PUBLIC "-//mybatis.org//DTD MyBatis Generator Configuration 1.0//EN" "http://mybatis.org/dtd/mybatis-generator-config_1_0.dtd">

<generatorConfiguration>
	
	<properties resource="generator.properties"></properties>

	<classPathEntry location="${jdbc.driverLocation}"/>

	<context id="default" targetRuntime="MyBatis3">
	
	        <commentGenerator>
		
		<jdbcConnection>

		<javaTypeResolver>

		<javaModelGenerator>

		<sqlMapGenerator>

		<javaClientGenerator>

		<table>

	</context>

</generatorConfiguration>

```

配置文件推荐存储在/src/main/resources/generatorConfig.xml位置，并且至少完成一下几点配置：

>1.<jdbcConnection> 元素定义如何连接目标数据库

```xml

<jdbcConnection driverClass="com.mysql.jdbc.Driver" 
	connectionURL="jdbc:mysql://localhost:port/something" 
	userId="root" 
	password="password">
</jdbcConnection>

```

>2.<javaModelGenerator> 元素来指定生成Java模型对象所属的包

```xml

<javaModelGenerator targetPackage="com.nostudy.domain" targetProject="src/main/java">
	<!-- 是否对model添加 构造函数 -->
	<property name="constructorBased" value="true"/>
            
	<!-- 是否允许子包，即targetPackage.schemaName.tableName -->
	<property name="enableSubPackages" value="false"/>

	<!-- 建立的Model对象是否 不可改变  即生成的Model对象不会有 setter方法，只有构造方法 -->
	<property name="immutable" value="true"/>

	<!-- 是否对类CHAR类型的列的数据进行trim操作 -->
	<property name="trimStrings" value="true"/>
</javaModelGenerator>

```

>3.<sqlMapGenerator> 元素来指定生成每个数据表的映射文件(Mapper)所属的包和的目标项目

```xml

<sqlMapGenerator targetPackage="com.nostudy.domain" targetProject="src/main/java">
	<property name="enableSubPackages" value="false"/>
</sqlMapGenerator>

```

>4.<javaClientGenerator> 元素来指定目标包和目标项目生成的客户端接口和类(如果不想生成Java客户端代码可以忽略)

```xml

<javaClientGenerator targetPackage="com.nostudy.business.dao" targetProject="src/main/java" type="MIXEDMAPPER">
	<property name="enableSubPackages" value=""/>

	<!--定义Maper.java 源代码中的ByExample() 方法的可视性，可选的值有：public;private;protected;default；如果targetRuntime="MyBatis3",此参数被忽略-->
	<property name="exampleMethodVisibility" value=""/>

	<!--方法名计数器(this property is ignored if the target runtime is MyBatis3.) -->
	<property name="methodNameCalculator" value=""/>

	<!--为生成的接口添加父接口-->
	<property name="rootInterface" value=""/>

</javaClientGenerator>

```

>5.<table>要生成的表

更改tableName和domainObjectName就可以

```xml
	<table tableName="NoUser"
		domainObjectName=""
		enableCountByExample="false"
		enableUpdateByExample="false"
		enableDeleteByExample="false"
		enableSelectByExample="false"
		selectByExampleQueryId="false"/>

```
其他部分请见[配置文件具体参考样式](http://generator.sturgeon.mopaas.com/configreference/xmlconfig.html)

2.命令行运行MBG

详细内容见[从命令行运行MBG](http://generator.sturgeon.mopaas.com/running/runningFromCmdLine.html)

```shell

java -jar mybatis-generator-core-x.x.x.jar -configfile \filepath\generatorConfig.xml -overwrite

```

>如果想保留已存在的Java文件，可以忽略-overwrite。MBG会用唯一的名字保存新生成的文件（eg:MyClass.java.1)。

[Gradle自动构建地址](http://mvnrepository.com/artifact/org.mybatis.generator/mybatis-generator-core)

3.创建或修改的标准MyBatis配置文件

主要的任务是[创建或修改MapperConfig.xml文件](http://generator.sturgeon.mopaas.com/afterRunning.html)

(目前暂时不考虑)


##7.MGB生成文件解析

在generatorConfig.xml的<table/>标签中声明的TableName会在MBG运行后自动生成三种文件：

>1.TableNameMapper.java

>2.TableName.java

>3.TableNameMapper.xml

##8.MySQL5.6安装

建议在root下操作

1.检查删除之前RPM安装的MySQL

```shell
rpm -qa | grep MySQL

rpm -qa | grep mysql

rpm -e ???? —nodeps

```

2.检查删除之前YUM安装的MySQL

```shell

yum remove mysql 

```

3.检查删除全部的MySQL

```shell

whereis mysql

rm -rf ????

```

4.开始RPM安装

[RPM下载地址](http://ftp.ntu.edu.tw/pub/MySQL/Downloads/MySQL-5.6/)

```shell

cd /usr/local/src

wget [下载地址](http://ftp.ntu.edu.tw/pub/MySQL/Downloads/MySQL-5.6/MySQL-5.6.15-1.linux_glibc2.5.x86_64.rpm-bundle.tar)

tar -xvf MySQL-5.6.15-1.linux_glibc2.5.x86_64.rpm-bundle.tar

rpm -ivh MySQL-client-5.6.15-1.linux_glibc2.5.x86_64.rpm

rpm -ivh MySQL-server-5.6.15-1.linux_glibc2.5.x86_64.rpm

初始化时，MySQL会自动生成密码并且保存在'/root/.mysql_secret'，需要先`cat /root/.mysql_secret`查看

A RANDOM PASSWORD HAS BEEN SET FOR THE MySQL root USER !You will find that password in '/root/.mysql_secret'.

在没有更改初始密码之前，除了`SET PASSWORD`命令以外不允许其他操作

You must change that password on your first connect,no other statement but 'SET PASSWORD' will be accepted.

Also, the account for the anonymous user has been removed.

此外在运行`/usr/bin/mysql_secure_installation`完成安全设置

In addition, you can run:  /usr/bin/mysql_secure_installation

which will also give you the option of removing the test database.
This is strongly recommended for production servers.

在`/usr/my.cnf`是配置文档，以后需要在其中修改配置

New default config file was created as /usr/my.cnf and
will be used by default by the server when you start it.
You may edit this file to change server settings

```

5.启动和配置

```shell

/sbin/chkconfig mysql on  启动mysql

systemctl restart mysql 重启mysql

```

在/usr/my.cnf中修改数据库配置

```shell

[mysqld]
character-set-server = utf8
collation-server = utf8_general_ci
default-storage-engine = INNODB

```

稍后登录后用`show variables like 'char%';`查看

6.修改密码

```sql

SET PASSWORD = PASSWORD('????')

```

7.安全操作

```shell

/usr/bin/mysql_secure_installation

```

一定要删除test数据库(程序提示YES/NO)
