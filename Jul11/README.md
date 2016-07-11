# 07月11日

##关键词

阿里云 SSH 

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

>1.Java Servlet作为Web服务的处理入口；

>2.Java编程编写业务处理程序；

>3.JDBC访问数据库；

>4.Android端的HTTP模块，API；

>5.Android端解析JSON数据；

>6.Servlet或者Java端生成JSON数据；

可以接触各种框架、EJB技术了。Spring、SpringMVC、Struts、Hibernate，甚至NOSQL、分布式、负载、node.js、模板技术等等。
