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

### **安装java**

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



### **安装mysql**

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

### **安装Jenkins**

/usr/local/jenkins

```shell
https://pkg.jenkins.io/redhat-stable/
rpm -ih jenkins-2.73.2-1.1.noarch.rpm
service jenkins start
```

```sehll
JAVA_HOME=/usr/local/jdk8
/usr/local/maven/apache-maven-3.6.2
/usr/local/maven/apache-maven-3.6.2

mvn -v 
echo $MAVEN_HOME
echo $JAVA_HOME
```

restart.sh放在了/soft/jenkins目录中

### jenkins的jar保存目录

```txt
/var/lib/jenkins/workspace
clean install -Dmaven.test.skip=true -Ptest
```

```shell
cd /home/admin/Jenkins-in #根据自己stop.sh、replace.sh脚本地址写
sh stop.sh
sh replace.sh
BUILD_ID=dontKillMe nohup java -jar /home/admin/workspace/personal-0.0.1-SNAPSHOT.jar &
#根据自己jar包的名称、地址修改



stop.sh-------------------
# 将应用停止
#stop.sh
#!/bin/bash
echo "Stopping SpringBoot Application"
pid=`ps -ef | grep personal-0.0.1-SNAPSHOT.jar | grep -v grep | awk '{print $2}'`
if [ -n "$pid" ]
then
   kill -9 $pid
fi

#此处personal-0.0.1-SNAPSHOT.jar根据自己的jar包名称修改

replace.sh-------------------
#replace.sh 用于将上次构建的结果备份，然后将新的构建结果移动到合适的位置
#!/bin/bash
# 先判断文件是否存在，如果存在，则备份
file="/www/server/workspace/autumn-0.0.1-SNAPSHOT.jar"
if [ -f "$file" ]
then
   mv /home/admin/workspace/personal-0.0.1-SNAPSHOT.jar.`date +%Y%m%d%H%M%S`
fi
mv /root/.jenkins/workspace/hello/target/personal-0.0.1-SNAPSHOT.jar /home/admin/workspace/personal-0.0.1-SNAPSHOT.jar

#此处 /home/admin/workspace/personal-0.0.1-SNAPSHOT.jar根据自己实际jar包名称和路径修改
```

不让jenkins杀死进程

```shell
key=BUILD_ID

 value=allow_to_run_as_daemon start_my_service
```

### maven构建

```shell
#只打包不执行单元测试
# mvn package -Dmaven.test.skip=true

# 打包:跳过单元测试;每次构建检查更新
mvn package -Dmaven.test.skip=true -U -e -X
```

### 部署docker

```shell
#操作/项目路径(Dockerfile存放的路劲)
BASE_PATH=/soft/platform
cd $BASE_PATH
# 源jar路径  
#docker 镜像/容器名字或者jar名字 这里都命名为这个
SERVER_NAME=platform
SERVER_PORT=9091
#容器id
CID=$(docker ps | grep "$SERVER_NAME" | awk '{print $1}')
#镜像id
IID=$(docker images | grep "$SERVER_NAME" | awk '{print $3}')
DATE=`date +%Y%m%d%H%M`
# 构建docker镜像
function build(){
    if [ -n "$IID" ]; then
        echo "存在$SERVER_NAME镜像，IID=$IID"
    else
        echo "不存在$SERVER_NAME镜像，开始构建镜像"
            cd $BASE_PATH
        docker build -t $SERVER_NAME .
    fi
}
# 运行docker容器
function run(){
    backup
    transfer
    build
    if [ -n "$CID" ]; then
        echo "存在$SERVER_NAME容器，CID=$CID,重启docker容器 ..."
            docker restart $SERVER_NAME
        echo "$SERVER_NAME容器重启完成"
    else
        echo "不存在$SERVER_NAME容器，docker run创建容器..."
            docker run --name $SERVER_NAME -v $BASE_PATH:$BASE_PATH -d -p $SERVER_PORT:$SERVER_PORT $SERVER_NAME
        echo "$SERVER_NAME容器创建完成"
    fi
}
#入口
run  
```





























