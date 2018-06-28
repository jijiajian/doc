虚拟机安装centos minimal版本

## 安装centos minimal版本

注意:

网络适配器设置为 桥接模式  
cd /etc/sysconfig/network-scripts/ 
vi ifcfg-ens33  
将里面的ONBOOT=no改为ONBOOT=yes,保存退出  
service network restart

### NAT

vi ifcfg-ens33 
```
# 改
BOOTPROTO=static
ONBOOT=yes

# 增
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
## 关闭防火墙
关闭防火墙（适用于centos7，低版本不适用）
分别执行如下两条命令：

systemctl stop firewalld.service
systemctl disable firewalld.service

## 关闭SELinux
vi /etc/selinux/config
将SELINUX=enforcing改为SELINUX=disabled 
重启


## jdk的安装

yum -y remove java-1.8.0-openjdk.x86_64
yum -y remove java-1.8.0-openjdk*

yum -y list java*


### jdk安装
wget http://download.oracle.com/otn-pub/java/jdk/8u171-b11/512cd62ec5174c3487ac17c61aaa89e8/jdk-8u171-linux-x64.rpm?AuthParam=1529848302_cb561570a55701f1a68ef4445e4f33bf

rpm -ivh jdk-8u171-b14-jdk-8u111-linux-x64.rpm

```
2. 配置环境变量
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
```

export JAVA_HOME=/opt/jdk1.8.0_171

## 修改主机名
vi /etc/sysconfig/network

NETWORKING=yes
HOSTNAME=hadoop01


vi /etc/hosts
localhost.localdomain -> hadoop01

vi /etc/hostname
hadoop01



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


export HADOOP_HOME=/usr/local/hadoop  
export PATH=$PATH:$HADOOP_HOME/bin:$HADOOP_HOME/sbin 


https://www.cnblogs.com/thousfeet/p/8618696.html

## hadoop

vi /etc/profile
export HADOOP_HOME=/usr/local/software/hadoop-3.0.3
export PATH=$PATH:$HADOOP_HOME/bin

hadoop目录/etc/hadoop/hadoop-env.sh
export JAVA_HOME=/usr/local/software/jdk1.8.0_171

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
        <value>hdfs://hadoop01:9000</value>
    </property>
    <!-- 指定hadoop运行时产生文件的存储路径 -->
    <property>
        <name>hadoop.tmp.dir</name>
        <value>/usr/local/hdpdata/tmp</value>
    </property>
</configuration>

### etc/hadoop/hdfs-site.xml 

<property>
    <name>dfs.namenode.name.dir</name>
    <value>/usr/local/hdpdata/hdfs/dfsname</value>
    <description>namenode上存储hdfs名字空间元数据 </description> 
</property>
<property>
    <name>dfs.datanode.data.dir</name>
    <value>/usr/local/hdpdata/hdfs/dfsdata</value>
    <description>datanode上数据块的物理存储位置</description>
</property>
<!-- 设置hdfs副本数量 -->
<property>
    <name>dfs.replication</name>
    <value>1</value>
</property>
<!-- 设置hdfs页面端口 -->
<property>
  <name>dfs.namenode.http-address</name>
  <value>0.0.0.0:50070</value>
</property>

bin/hdfs namenode -format
sbin/start-dfs.sh


http://192.168.0.5:50070

sbin/hadoop-daemon.sh start namenode


export HDFS_NAMENODE_USER=root
export HDFS_DATANODE_USER=root
export HDFS_SECONDARYNAMENODE_USER=root
export YARN_RESOURCEMANAGER_USER=root
export YARN_NODEMANAGER_USER=root


tencent://message/?uin=2112857847&Site=http://vps.shuidazhe.com&Menu=yes

待发布>待审核>审核不通过>优化中>扣费中>已关闭



# Java Setting
JAVA_HOME=/opt/jdk1.8.0_171
HADOOP_HOME=/opt/hadoop-3.1.0
PATH=$PATH:$JAVA_HOME/bin:$HADOOP_HOME/bin:$HADOOP_HOME/sbin
CLASSPATH=.:$JAVA_HOME/lib/dt.jar:$JAVA_HOME/lib/tools.jar
export JAVA_HOME HADOOP_HOME CLASSPATH PATH
