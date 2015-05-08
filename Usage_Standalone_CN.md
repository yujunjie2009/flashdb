### 适用场景 ###
数据量和访问量不大。也可用于快速入门实验。
<br />

### 一般用法 ###
方式一:
  * 直接在同一张表上进行检索和写操作。
  * 写入操作完成后，执行 <font color='green'>FLUSH TABLE xxx</font> 使检索可见。
方式二：
  * 建a、b两张结构相同的表，a表用于检索服务，b表用于写操作。
  * 在b表写操作完成后，先执行 <font color='green'>OPTIMIZE TABLE b</font> 进行优化
  * 再执行 <font color='green'>LOAD TABLE DATA a FROM LOCAL b</font> 使检索切换到新数据。
<br />

### 配置(flashdb.properties) ###
```
default_tokenizer_bean=IKTokenizer
rmi_port=1999
plugin_jar_files=commons-dbcp-1.4.jar,commons-pool-1.5.4.jar,mysql-connector-java-5.1.17.jar,flashdb-ik-4.7.2.jar
heart_beat_sql=select count(*) from sample_table
```

配置项说明:
  * default\_tokenizer\_bean: 默认的文本分词器(值为在plugin.spring.xml文件中的相应bean的id)。相关配置参考 http://code.google.com/p/flashdb/wiki/TokenizerConf_CN ;
  * rmi\_port: RMI通信协议的服务端口. 0表示不开放RMI通信协议服务(可通过httpinvoker/hessina通信协议访问);
  * plugin\_jar\_files: 扩展插件所需的jar文件名(逗号间隔). jar文件存放在/home/flashdb/plugin/目录下;
  * heart\_beat\_sql: 服务心跳监测时执行的sql. 心跳监测页 http://127.0.0.1:8080/flashdb/do/heartbeat.json