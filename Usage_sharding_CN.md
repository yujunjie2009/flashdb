### 适用场景 ###
数据量很大, 查询访问量适中。
<br />

### 一般用法 ###
  * 主控服务器用于整体读写控制，分片服务器用于执行实际的存储和检索。
  * 所有服务器上的表名及表结构相同。唯一的区别是主控服务器上的建表参数需加上 <font color='green'>ENGINE 'shard'</font>
  * 所有外部应用程序的JDBC连接指向主控服务器;
<br />

### 基本实现原理 ###
  * 写入操作时，主控服务器计算主键字段值(主键多字段时,取第一个字段值)的哈希，根据哈希值将实际的写操作发送到相应的分片服务器上。
  * 检索读取时，主控服务器将相同的检索请求同时发送到各分片服务器上，然后再汇总结果返回。
<br />

### 配置(flashdb.properties) ###
```
default_tokenizer_bean=IKTokenizer
rmi_port=1999
plugin_jar_files=commons-dbcp-1.4.jar,commons-pool-1.5.4.jar,mysql-connector-java-5.1.17.jar,flashdb-ik-4.7.2.jar
heart_beat_sql=select count(*) from sample_table

web_port=8080
web_context=/flashdb/

sharding_master=10.19.0.10
sharding_slaves=10.19.0.11,10.19.0.12,10.19.0.13
default_shard_count=6
```

配置项说明:
  * web\_port: Web服务器的端口;
  * web\_context: flashdb-webserver-xxx.war在Web中部署的上下文根;
  * sharding\_master: 主控服务器的IP地址;
  * sharding\_slaves: 分片服务器的IP地址(逗号间隔)
  * default\_shard\_count: 数据分片数,一般配置为分片服务器数的n倍
注意: 所有Web服务器的端口和上下文根应保持一致.