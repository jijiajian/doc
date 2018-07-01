## hadoop 常用命令

1. 将hello文件传至hdfs根目录  
hadoop fs -put hello.txt /

2. 查看文件内容  
hadoop fs -text /hello.txt
hadoop fs -cat /test/childa/a.txt

3. 创建文件夹
hadoop fs -mkdir /test

4. 查看目录
hadoop fs -ls /

5. 创建子目录
hadoop fs -mkdir -p /test/childa

6. 递归查看目录  
hadoop fs -ls -R /

7. 将本地文件拷贝至hdfs
hadoop fs -copyFromLocal hello.txt /test/childa/a.txt

8. 将hdfs上的文件下载至本地
hadoop fs -get /test/childa/a.txt

9. 删除文件(目录)
hadoop fs -rm /hello.txt
hadoop fs -rm -R /test/childb


## java api
连接不上hadoop, 首先先确认防火墙是否关闭
[Hadoop 拒绝远程 9000 端口访问解决](https://blog.csdn.net/mzjwx/article/details/78547573)
