# 07月11日

##关键词

阿里云 SSH yum servlet Tomcat 端口 WEB-INF JSP web程序的请求响应模式

##1.登录服务器

`ssh <ip>`

##2.服务器记住密码

1.进入客户端的ssh目录（如果没有就建立一个`mkdir ~/.ssh`）

`cd ~/.ssh/`

制作RSA密码

`ssh-keygen -t rsa`

将RSA公钥发给服务器的./ssh目录(如果服务器上没有.ssh文件夹，需要先登录服务器进行创建。)

`scp id_rsa.pub username@server:~/.ssh/id_rsa.pub`

2.切换到服务器上

`ssh username@server`

进入服务器的ssh目录

`cd ~/.ssh`

把接受到的公钥内容添加到认证公钥表

`cat id_rsa.pub >> authorized_keys`

删除公钥文件

`rm id_rsa.pub`

##3.配置LAMP环境

1.安装LAMP

`yum -y install mysql mysql-client mysql-server mysql-devel mysql*`

`yum -y install httpd`

`yum -y install php php-mysql php-gd libjpeg* php-ldap php-odbc php-pear php-xml php-xmlrpc php-mbstring php-bcmath php-mhash`


2.配置LAMP

`systemctl enable httpd`

`systemctl start httpd`

[无法用systemctl将mysqld.service设置为启动项](http://www.doraemonext.com/archives/538.html)

mysqld.service is a "virtual" unit – it doesn't exist on the filesystem, it's just part of systemd's compatibility layer. You can start it and systemd will run the legacy /etc/rc.d/mysqld initscript, but you cannot systemctl enable it because you need a real .service file which could be symlinked into the proper place. You can write such a unit yourself and put it in /etc/systemd/system/mysqld.service:

`[Unit]`

`Description=MySQL Server`

`After=network.target`


`[Service]`

`ExecStart=/usr/bin/mysqld --defaults-file=/etc/mysql/my.cnf --datadir=/var/lib/mysql --socket=/var/run/mysqld/mysqld.sock` 

`User=mysql`

`Group=mysql`

`WorkingDirectory=/usr`


`[Install]`

`WantedBy=multi-user.target`

创建并撰写以上文件以后使用

`systemctl enable mysql.service`

`systemctl start mysql.service`

##4.创建yum仓库(以163源为例)

查看CentOS系统版本

`lsb_release-a`

[CentOS镜像使用帮助](http://mirrors.163.com/.help/centos.html)

首先备份/etc/yum.repos.d/CentOS-Base.repo

`mv /etc/yum.repos.d/CentOS-Base.repo /etc/yum.repos.d/CentOS-Base.repo.backup`

下载对应版本repo文件, 放入/etc/yum.repos.d/

`wget http://mirrors.163.com/.help/CentOS7-Base-163.repo`

运行以下命令生成缓存

`yum clean all`

`yum makecache`

##5.Java开发Android后台

使用J2EE技术或者Java Web技术开发一个Web服务器，服务器返回Json数据，android客户端解析json数据，使用http协议和服务器通信，android有相应模块和API。用Servlet搭建Web服务，Serlvet映射一个URL，Android请求这个URL，Servlet处理请求，然后就是Java编程，Web分层、JDBC等技术。服务器返回的JSON，Android来解析。

具体技术：

>1.Java Servlet作为Web服务的处理入口；用户若想用发一个动态web资源(即开发一个Java程序向浏览器输出数据)，需要写一个Java类实现servlet接口，并部署到web服务器中。Servlet可以通过“响应-请求”编程模型来访问。

>2.Java编程编写业务处理程序；

>3.JDBC访问数据库；

>4.Android端的HTTP模块，API；

>5.Android端解析JSON数据；

>6.Servlet或者Java端生成JSON数据；

可以接触各种框架、EJB技术了。Spring、SpringMVC、Struts、Hibernate，甚至NOSQL、分布式、负载、node.js、模板技术等等。

##6.Tomcat的基本框架

Tomcat -> Container -> Engine -> HOST -> Servlet -> Context -> Web工程

##7.Servlet

手工编写第一个Servlet程序的大体步骤(可以用idea直接new一个Servlet)

1.继承HttpServlet抽象类 

>Servlet接口（三个方法对应三个生命周期init() serivce() & destroy()）--> GenericServlet抽象类（与协议无关）--> HttpServlet抽象类

IntelliJ IDEA 

Gradle

`dependencies{  `

`    compile 'org.springframework:spring-webmvc:3.2.5.RELEASE'  `

`    compile 'com.fasterxml.jackson.core:jackson-databind:2.3.0'  `

`    providedCompile 'org.apache.tomcat:tomcat-servlet-api:8.0.0-RC5'  `

`}  `

compile(
            "org.springframework:spring-context:4.0.5.RELEASE",
            "org.springframework:spring-web:4.0.5.RELEASE",
            "org.springframework:spring-webmvc:4.0.5.RELEASE"
    )
    testCompile("org.springframework:spring-test:4.0.5.RELEASE")
    runtime("jstl:jstl:1.2")

2.重写HttoServlet的doGet()和doPost()

在Intellij idea中快速重写父类方法。鼠标左击以确定代码插入的位置，使用快捷键CTRL+O，会弹出窗口让选择某个方法

3.在web.xml中注册Servlet

在File->Project Structure->Modules中生成web.xml文件

`<servlet>`与`<servlet-mapping>`

><servlet>标签中可以添加<loadon-startup>标签

##8.Idea


##9.端口查看

1.88请换为你的apache需要的端口，如：8080

`netstat -lnp|grep 88`

`lsof -i tcp:8080`

2.查看进程的详细信息

`ps 1777`

3.杀掉进程

`kill -9 1777`

##10.JSP

WEB-INF该目录是web的安全目录，客户端无法访问，只有服务端可以访问的目录。web.xml是项目部署文件。classes文件夹放置*.class。lib存放jar包。

虚拟路径？？

可以修改Tomcat的默认端口

JSP页面元素构成：指令 注释 表达式 脚本 声明  静态内容

###1.指令元素：

1.page指令：通常位于JSP页面的顶端，一个页面可以同时有多个page指令

`<%@ language="java" import="" contentType="text/html" %>`

2.include指令：将外部文件嵌入到当前JSP中并解析

3.taglib指令：设置用户自定义的标签库

###2.注释

1.HTML注释：`<!--html注释-->`客户端在F12中可见

2.JSP注释：`<%--jsp注释--%>`客户端不可见

3.JSP脚本注释：`/* */`

###3.脚本

>在JSP中执行的Java代码 `<% 脚本 %>`

###4.声明

>在JSP页面中定义变量或者方法`<%! 声明 %>`

###5.表达式

`<%= 表达式 %>` 表达式不用以分号结束（而声明和脚本都需要用分号结束）

JSP的声明周期？？

JSP内置对象 out request/response session application ...Page pageContext exception config

>JSP内置对象是web容器创建的一组对象，不使用new关键字就可以使用；

##11.请求响应模式

用户发送请求request 服务器给用户响应response

out对象是JspWriter类的实例，是向客户端输出内容的常用对象。println() clear()

###12.SpringMVC


