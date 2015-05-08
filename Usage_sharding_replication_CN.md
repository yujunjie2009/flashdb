### 适用场景 ###
数据量很大, 同时查询访问量也很大。
<br />

### 一般用法 ###
  * 一台sharding master服务器加n台sharding\_slaves为一组。
  * 一组服务器用于写操作，其他服务器组用于检索服务。
  * 所有服务器上的表名及表结构相同。唯一的区别是sharding master服务器上的建表参数需加上 <font color='green'>ENGINE 'shard'</font>
  * 所有外部应用程序的JDBC连接指向相应的sharding master服务器;
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

replication_master=192.168.0.10
replication_slaves=192.168.0.11,192.168.0.12,192.168.0.13
```

配置项说明:
  * 各配置项的含义，参见<a href='http://code.google.com/p/flashdb/wiki/Usage_Standalone_CN'>单机</a>、<a href='http://code.google.com/p/flashdb/wiki/Usage_sharding_CN'>数据分片</a>、<a href='http://code.google.com/p/flashdb/wiki/Usage_replication_CN'>主从复制</a>;
  * 各服务器的配置项 sharding\_master/sharding\_slaves/replication\_master/replication\_slaves 的值不同，需根据自身在各组中的角色进行相应配置；
  * 各组服务器的分片服务器数应保持一致;
  * 每组服务器的sharding\_slaves分片顺序应保持一一对应;
注意: 所有Web服务器的端口和上下文根应保持一致.