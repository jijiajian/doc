虚拟机安装centos minimal版本,网上教程很多

## centos 配置网络

1. 网络配置

`网络适配器设置为 桥接模式 `

cd /etc/sysconfig/network-scripts/ 
vi ifcfg-ens33(后面的数字不一定是33)

将里面的ONBOOT=no改为ONBOOT=yes,保存退出  

service network restart

`网络适配器设置为 NAT模式`

vi ifcfg-ens33
修改下面两项
```
BOOTPROTO=static
ONBOOT=yes
```

增加下面几项
```
# NAT设置里网关
GATEWAY=192.168.80.2
# 本机ip
IPADDR=192.168.80.66:50070
# 子网掩码
NETMASK=255.255.255.0
# DNS
DNS1=114.114.114.114
DNS2=8.8.8.8
```

2. 关闭防火墙
关闭防火墙（适用于centos7，低版本不适用）
分别执行如下两条命令：

systemctl stop firewalld.service
systemctl disable firewalld.service

3. 关闭SELinux

vi /etc/selinux/config

将SELINUX=enforcing改为SELINUX=disabled 

重启

## jdk安装

1. 将jdk安装包放到/opt路径下,执行下面命令解压到当前目录  
tar -xzvf jdk-8u171-linux-x64.tar.gz

1. 配置环境变量
vi /etc/profile

在末尾插入如下代码(java home 根据你装的jdk版本填写)

```
# Java Setting
JAVA_HOME=/opt/jdk1.8.0_171
PATH=$PATH:$JAVA_HOME/bin
CLASSPATH=.:$JAVA_HOME/lib/dt.jar:$JAVA_HOME/lib/tools.jar
export JAVA_HOME  CLASSPATH  PATH
```
3. 执行如下命令使设置生效  
source  /etc/profile

4. java -version 验证是否配置成功

## hadoop安装([参考官方安装教程](http://hadoop.apache.org/docs/r2.9.1/hadoop-project-dist/hadoop-common/SingleCluster.html))

1. 将安装包放到/opt文件夹下,解压至当前目录  
tar -xzvf hadoop-2.9.1.tar.gz
2. 配置环境变量
将之前的jdk配置改为如下配置(加入hadoop的配置)
```
# Java and hadoop setting
JAVA_HOME=/opt/jdk1.8.0_171
HADOOP_HOME=/opt/hadoop-2.9.1
PATH=$PATH:$JAVA_HOME/bin:$HADOOP_HOME/bin:$HADOOP_HOME/sbin
CLASSPATH=.:$JAVA_HOME/lib/dt.jar:$JAVA_HOME/lib/tools.jar
export JAVA_HOME HADOOP_HOME CLASSPATH PATH
```
3. ssh免登陆

```
ssh-keygen -t dsa -P '' -f ~/.ssh/id_dsa  
cat ~/.ssh/id_dsa.pub >> ~/.ssh/authorized_keys  
chmod 0600 ~/.ssh/authorized_keys 
```
使用 ssh localhost 测试是否设置成功

4. 配置hadoop-env.sh  
vi /opt/hadoop-2.9.1/etc/hadoop/hadoop-env.sh

```
# 与之前配置jdk保持一致
export JAVA_HOME=/opt/jdk1.8.0_171
```

若是使用root用户,还需加入以下内容
```
export HDFS_NAMENODE_SECURE_USER=root
export HDFS_DATANODE_SECURE_USER=root
export HDFS_SECONDARYNAMENODE_USER=root
```


5. 配置core-site.xml  
vi /opt/hadoop-2.9.1/etc/hadoop/core-site.xml

```
    <!-- 指定HDFS老大（namenode）的通信地址 -->
    <property>
        <name>fs.defaultFS</name>
        <value>hdfs://localhost:9000</value>
    </property>
```

6. 配置hdfs-site.xml  
vi /opt/hadoop-2.9.1/etc/hadoop/hdfs-site.xml

```
    <!-- 设置hdfs副本数量 伪分布式设置为1-->
    <property>
        <name>dfs.replication</name>
        <value>1</value>
    </property>
```
7. 格式化namenode后启动
```
cd /opt/hadoop-2.9.1
bin/hdfs namenode -format
sbin/start-dfs.sh
```
之后可以通过jps命令查看是否启动成功
也可以访问
http://192.168.80.66:50070

8. Yarn 配置 etc/hadoop/mapred-site.xml

<property>
    <name>mapreduce.framework.name</name>
    <value>yarn</value>
</property>

9. Yarn 配置 etc/hadoop/yarn-site.xml  
<property>
    <name>yarn.nodemanager.aux-services</name>
    <value>mapreduce_shuffle</value>
</property>

10. 启动并运行官方测试例子
- sbin/start-yarn.sh
- jps(查看相关进程是否启动, 若没有去看相应的log: cd logs)
- 访问 http://192.168.0.5:8088/cluster 查看yarn 调度状况
- 进入例子目录  cd share/hadoop/mapreduce/
- hadoop jar hadoop-mapreduce-examples-2.9.1.jar wordcount /hello.txt /output
- 查看结果  hadoop fs -cat /output/part-r-00000
