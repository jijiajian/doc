虚拟机安装centos minimal版本

## 安装centos minimal版本

注意:

网络适配器设置为 桥接模式  
cd /etc/sysconfig/network-scripts/  
vi ifcfg-ens33  
将里面的ONBOOT=no改为ONBOOT=yes,保存退出  
service network restart


## jdk的安装

yum -y remove java-1.8.0-openjdk.x86_64
yum -y remove java-1.8.0-openjdk*

yum -y list java*


### jdk安装
wget http://download.oracle.com/otn-pub/java/jdk/8u171-b11/512cd62ec5174c3487ac17c61aaa89e8/jdk-8u171-linux-x64.rpm?AuthParam=1529848302_cb561570a55701f1a68ef4445e4f33bf

rpm -ivh jdk-8u171-b14-jdk-8u111-linux-x64.rpm

```
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
```



## 其他

mkdir 新建文件夹

rm 文件名 (删除文件)
tar -xzvf 文件名  解压至当前文件夹

wget http://mirror.bit.edu.cn/apache/hadoop/common/hadoop-3.1.0/hadoop-3.1.0-src.tar.gz 
wget http://mirror.bit.edu.cn/apache/hadoop/common/hadoop-3.0.3/hadoop-3.0.3.tar.gz 

wget http://download.oracle.com/otn-pub/java/jdk/8u171-b11/512cd62ec5174c3487ac17c61aaa89e8/jdk-8u171-linux-x64.tar.gz?AuthParam=1529848047_75008054f60c98981f52349eada4feed



```
rm (选项)(参数)

选项
-d：直接把欲删除的目录的硬连接数据删除成0，删除该目录；
-f：强制删除文件或目录；
-i：删除已有文件或目录之前先询问用户；
-r或-R：递归处理，将指定目录下的所有文件与子目录一并处理；
--preserve-root：不对根目录进行递归操作；
-v：显示指令的详细执行过程。
```


修改主机名  vi /etc/hostname


https://www.cnblogs.com/thousfeet/p/8618696.html

## hadoop

vi /etc/profile
export HADOOP_HOME=/usr/local/hadoop/hadoop-3.0.3
export PATH=$PATH:$HADOOP_HOME/bin 

hadoop目录/etc/hadoop/hadoop-env.sh
export JAVA_HOME=/usr/java/latest

export HDFS_NAMENODE_SECURE_USER=root
export HDFS_DATANODE_SECURE_USER=root
export HDFS_SECONDARYNAMENODE_USER=root


### ssh免登陆
ssh-keygen -t dsa -P '' -f ~/.ssh/id_dsa  
cat ~/.ssh/id_dsa.pub >> ~/.ssh/authorized_keys  
chmod 0600 ~/.ssh/authorized_keys 


### etc/hadoop/core-site.xml

<configuration>  
    <!-- 指定HDFS老大（namenode）的通信地址 -->  
    <property>  
        <name>fs.defaultFS</name>  
        <value>hdfs://localhost:9000</value>  
    </property>  
    <!-- 指定hadoop运行时产生文件的存储路径 -->  
    <property>  
        <name>hadoop.tmp.dir</name>  
        <value>/usr/hadoop/tmp</value>  
    </property>  
</configuration>  

### etc/hadoop/hdfs-site.xml 

<property>
    <name>dfs.name.dir</name>
    <value>/usr/hadoop/hdfs/name</value>
    <description>namenode上存储hdfs名字空间元数据 </description> 
</property>

<property>
    <name>dfs.data.dir</name>
    <value>/usr/hadoop/hdfs/data</value>
    <description>datanode上数据块的物理存储位置</description>
</property>
<!-- 设置hdfs副本数量 -->
<property>
    <name>dfs.replication</name>
    <value>1</value>
</property>
<!-- 设置hdfs页面端口 -->
<property>
  <name>dfs.http.address</name>
  <value>0.0.0.0:50070</value>
</property>

bin/hdfs namenode -format
sbin/start-dfs.sh


http://192.168.0.5:50070