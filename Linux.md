# Linux的使用

[TOC]

## 1.常用快捷键

中英文切换 win+空格

Ctrl+Alt+T打开终端，在终端中创建新标签：Shift+Ctrl+t

git add . && git commit -m "mysql" && git push

## 2.命令

查看java安装目录：

rpm -qa | grep java

whereis java

卸载已经有的java

 rpm -e --nodeps 名字

yum -y remove 名字

**安装java**

- 下载tar包
- 解压tar -zxvf jdk-8u111-linux-x64.tar.gz
- 移动 mv jdk1.8.0_111 /usr/java/
- 配置环境变量vi /etc/profile

```shell
export JAVA_HOME=/usr/java/jdk1.8.0_141
export JRE_HOME=${JAVA_HOME}/jre
export CLASSPATH=.:%{JAVA_HOME}/lib:%{JRE_HOME}/lib
export PATH=${JAVA_HOME}/bin:$PATH
```

- 重新编译source /etc/profile

wget --no-cookies --no-check-certificate --header "Cookie: gpw_e24=http%3A%2F%2Fwww.oracle.com%2F; oraclelicense=accept-securebackup-cookie" "http://download.oracle.com/otn-pub/java/jdk/8u141-b15/336fa29ff2bb4ef291e347e091f7f4a7/jdk-8u141-linux-x64.tar.gz"



**安装mysql**

<https://dev.mysql.com/downloads/mysql/5.5.html>

- 下载 tar包

wget https://cdn.mysql.com//archives/mysql-5.5/mysql-5.5.49.tar.gz

- 解压tar -zxvf mysql-5.5.49.tar.gz

`rpm -qa | grep postfix`
`rpm -qa | grep mariadb`

卸载postfix和mariadb-libs

rpm -e --nodeps postfix-2.10.1-6.el7.x86_64

rpm -e --nodeps mariadb-libs-5.5.52-1.el7.x86_64

```shell
再创建用户和用户组
#groupadd mysql
#useradd -r -g mysql mysql

将安装目录所有者及所属组改为mysql ，这个根据自己的目录来
#chown -R mysql.mysql /usr/mysql

在mysql目录下创建data文件夹
#mkdir data

初始化数据库
#/usr/mysql/bin/mysql_install_db --user=mysql --basedir=/usr/mysql/ --datadir=/usr/mysql/data --initialize

```



第二种

<https://www.cnblogs.com/brianzhu/p/8575243.html>

```shell
wget -i -c http://dev.mysql.com/get/mysql57-community-release-el7-10.noarch.rpm
yum -y install mysql57-community-release-el7-10.noarch.rpm
yum -y install mysql-community-server
```

```
启动
systemctl start  mysqld.service
systemctl status mysqld.service
grep "password" /var/log/mysqld.log
mysql -uroot -p     # 回车后会提示输入密码
ALTER USER 'root'@'localhost' IDENTIFIED BY 'z?guwrBhH7p>';

SHOW VARIABLES LIKE 'validate_password%';
 set global validate_password_policy=LOW; 
set global validate_password_policy=0;

set global validate_password_length=1;
ALTER USER 'root'@'localhost' IDENTIFIED BY '123456';

grant all on *.* to root@'%' identified by '123456';
```

```shell
原因：没有开放3306端口。

执行一下命令：

systemctl stop firewalld
systemctl mask firewalld

并且安装iptables-services：
yum install iptables-services

设置开机启动：
systemctl enable iptables

systemctl stop iptables
systemctl start iptables
systemctl restart iptables
systemctl reload iptables

保存设置：
service iptables save

OK，再试一下应该就好使了

开放某个端口 在/etc/sysconfig/iptables里添加

模仿22端口开发3306

-A RH-Firewall-1-INPUT -m state --state NEW -m tcp -p tcp --dport 3306 -j ACCEPT

```

```shell
ping ip
telnet ip 端口
```







