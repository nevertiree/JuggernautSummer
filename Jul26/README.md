# 07月26日

tomcat

部署war包

yum -y install iptables-services防火墙

打开防火墙,使外部能访问
/sbin/iptables -I INPUT -p tcp --dport 8080 -j ACCEPT
service iptables save
service iptables restart

或直接修改文件/etc/sysconfig/iptables
vi /etc/sysconfig/iptables
 -A INPUT -p tcp -m tcp --dport 8080 -j ACCEPT
service iptables restart



systemctl stop firewalld.service


 启动Tomcat
    # cd /usr/local/tomcat/bin
    # ./startup.sh


