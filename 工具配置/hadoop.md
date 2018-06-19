虚拟机安装centos minimal版本

## 安装centos minimal版本

注意:

网络适配器设置为 桥接模式  
cd /etc/sysconfig/network-scripts/  
vi ifcfg-ens33  
将里面的ONBOOT=no改为ONBOOT=yes,保存退出  
service network restart


## jdk的安装

yum -y list java*

1. yum  install  java-1.8.0-openjdk   java-1.8.0-openjdk-devel

2. 配置环境变量
vi /etc/profile

在末尾插入如下代码(java home 根据你装的jdk版本填写)
```
# Java Setting
JAVA_HOME=/usr/lib/jvm/jre-1.8.0-openjdk-1.8.0.171-8.b10.el7_5.x86_64
PATH=$PATH:$JAVA_HOME/bin  
CLASSPATH=.:$JAVA_HOME/lib/dt.jar:$JAVA_HOME/lib/tools.jar  
export JAVA_HOME  CLASSPATH  PATH 
```
3. 执行如下命令使设置生效  
source  /etc/profile


## 其他

mkdir 新建文件夹

rm 文件名 (删除文件)
tar -xzvf 文件名  解压至当前文件夹

wget http://mirror.bit.edu.cn/apache/hadoop/common/hadoop-3.1.0/hadoop-3.1.0-src.tar.gz 


修改主机名  vi /etc/hostname


https://www.cnblogs.com/thousfeet/p/8618696.html