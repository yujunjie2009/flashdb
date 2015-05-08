### 适用场景 ###
查询访问量很大, 数据量适中。这是大多数搜索系统适用的模式。
<br />

### 一般用法 ###
  * 主服务器用于写操作，从服务器用于检索服务。
  * 所有服务器上的表名及表结构相同。
  * 在主服务器上写操作完成后，先执行 <font color='green'>OPTIMIZE TABLE xxx</font> 进行优化。
  * 再执行 <font color='green'>RUNON REPLICATION SLAVES NOPARALLEL <strong>LOAD TABLE DATA xxx FROM REPLICATION MASTER</strong></font> 使各个从服务器同步切换到新数据;
<br />

### 配置(flashdb.properties) ###
```
default_tokenizer_bean=IKTokenizer
rmi_port=1999
plugin_jar_files=commons-dbcp-1.4.jar,commons-pool-1.5.4.jar,mysql-connector-java-5.1.17.jar,flashdb-ik-4.7.2.jar
heart_beat_sql=select count(*) from sample_table

web_port=8080
web_context=/flashdb/

replication_master=192.168.0.10
replication_slaves=192.168.0.11,192.168.0.12,192.168.0.13
```

配置项说明:
  * web\_port: Web服务器的端口;
  * web\_context: flashdb-webserver-xxx.war在Web中部署的上下文根;
  * replication\_master: 主服务器的IP地址;
  * replication\_slaves: 从服务器的IP地址(逗号间隔)
注意: 所有Web服务器的端口和上下文根应保持一致.