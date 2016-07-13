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

>MBG是Mybatis代码生成器。它可以生成Mybatis各个版本的代码，可以内省数据库的表（或多个表）然后生成可以用来访问（多个）表的基础对象。这样和数据库表进行交互时不需要创建对象和配置文件。MBG解决了对数据库操作有最大影响的一些简单的CRUD操作。但用户仍然需要对联合查询和存储过程手写SQL和对象。

[MBG快速入门指南](http://generator.sturgeon.mopaas.com/quickstart.html)

1.创建并填写适当的配置文件

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

3.创建或修改的标准MyBatis配置文件来使用新生成的代码

[运行MBG后的任务](http://generator.sturgeon.mopaas.com/afterRunning.html)


