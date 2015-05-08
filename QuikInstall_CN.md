### 系统安装环境要求 ###
  * <a href='http://www.oracle.com/technetwork/java/javase/downloads/index.html'>Java SE JRE 1.6</a><br />
  * Web Server (如 <a href='http://tomcat.apache.org/'>Tomcat</a>、<a href='http://www.eclipse.org/jetty/'>Jetty</a>、<a href='http://www.ibm.com/software/websphere/'>Websphere</a>、...) <br />

### 系统安装 ###
1、进入<a href='http://code.google.com/p/flashdb/source/browse/jar'>下载页面</a>，下载所需文件，系统程序包<a href='http://flashdb.googlecode.com/svn/jar/flashdb-webserver-1.0.2.war'>flashdb-webserver-xxx.war</a>和系统数据包<a href='http://flashdb.googlecode.com/svn/jar/flashdb-data-1.0.2.zip'>flashdb-data-xxx.zip</a> ; <br />
2、将系统数据包flashdb-data-xxx.zip解压缩到本地目录 /home/flashdb <br />
3、在Web Server的JVM启动参数中添加以下内容,以设置FlashDB的工作目录:
```
-DFLASHDB_HOME=/home/flashdb -Dlogback.configurationFile=file:///home/flashdb/config/logback.xml
```
4、在Web Server的JVM启动参数中添加以下内容,以设置Java内存(具体数值可根据机器实际配置调整):
```
-Xms2048m
```
5、启动Web Server，部署flashdb-webserver-xxx.war, 建议上下文根使用 /flashdb <br />
6、访问FlashDB的首页(如 http://127.0.0.1:8080/flashdb/ )显示正常即可。<br />
7、推荐使用开源工具<a href='http://code.google.com/p/flashdb/wiki/SQuirreL_CN'>SQuirreL SQL Client</a>进行数据库操作.
<br />
### 编程开发 ###
  * 可按照普通的数据库开发方式进行。推荐使用spring-jdbc中的`JdbcTemplate`
    1. 注意: 所有写表操作(INSERT/UPDATE/DELETE)完成后，需执行SQL语句 FLUSH TABLE xxx ，否则写入的数据还在缓冲区中，查询检索不可见
  * 所需JDBC驱动包：<a href='http://flashdb.googlecode.com/svn/jar/flashdb-client-1.0.2.jar'>flashdb-client-xxx.jar</a>
> > 或者通过Maven引用:
```
<dependency>
  <groupId>com.boyunjian.flashdb</groupId>
  <artifactId>flashdb-client</artifactId>
  <version>1.0.2</version>
</dependency>
```