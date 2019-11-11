# Linux的使用

[TOC]

## 1.常用快捷键

中英文切换 win+空格

Ctrl+Alt+T打开终端，在终端中创建新标签：Shift+Ctrl+t

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





















