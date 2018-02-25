## 以管理员运行cmd/powershell

1. 解压, 配置my.ini(放在mysql的bin外层目录)
```
[mysql]
# 设置mysql客户端默认字符集
default-character-set=utf8 
[mysqld]
#设置3306端口
port = 3306 
# 设置mysql的安装目录
basedir=D:\\software\work\mysql\mysql-5.7.20-winx64
# 设置mysql数据库的数据的存放目录
datadir=D:\\software\work\mysql\data
# 允许最大连接数
max_connections=200
# 服务端使用的字符集默认为8比特编码的latin1字符集
character-set-server=utf8
# 创建新表时将使用的默认存储引擎
default-storage-engine=INNODB
```

2. 初始化mysql  
* 将mysql的bin目录路径追加到电脑的path环境变量
* 进入bin目录  mysqld –-initialize (生成data文件夹)
* mysqld install
* net start mysql (启动)


3. 更改mysql的ROOT 密码   
**好像是mysql 5.7默认会有一个root密码,可以先试试sql -uroot -p,不输密码直接回车,如果不需要输密码则可以跳过第3步,直接进行第4步**
* net stop mysql
* 进入bin目录, mysqld –skip-grant-tables(跳过他的权限表检查)
* 新开一个cmd,进入bin目录,输入mysql回车, 然后 use mysql;
* update user set authentication_string=password(‘1234’) where user=’root’ and Host = ‘localhost’;
* flush privileges;
* 关闭原来运行mysqld -–skip-grant-tables命令的cmd
* cmd再次进入bin目录 mysqld stop


4. 再次修改ROOT密码(不然无法操作,图形客户端也无法连接)
* mysql -uroot -p1234
* SET PASSWORD = PASSWORD('root1234');
* flush privileges;


安装过程遇到了一个问题,按照这个地址解决了  http://blog.csdn.net/u012465296/article/details/71157286

