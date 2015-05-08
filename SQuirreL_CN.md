1、下载<a href='http://squirrel-sql.sourceforge.net/'>SQuirreL SQL Client</a>安装包，根据提示进行安装;<br />
2、下载FlashDB的<a href='http://flashdb.googlecode.com/svn/jar/flashdb-client-1.0.2.jar'>JDBC驱动包</a>，并复制到SQuirreL安装目录的lib子目录下 <br />
3、启动SQuirreL，参照下图方式配置FlashDB的JDBC驱动 <br />
> <img src='http://flashdb.googlecode.com/svn/wiki/image_cn/SQuirreL-AddDriver.png' /><br />
> <img src='http://flashdb.googlecode.com/svn/wiki/image_cn/SQuirreL-DriverConf.png' /><br />
```
com.lingbobu.flashdb.jdbc.JdbcDriver
```
4、参照下图方式配置JDBC连接:<br />
> <img src='http://flashdb.googlecode.com/svn/wiki/image_cn/SQuirreL-AliasAdd.png' /><br />
> <img src='http://flashdb.googlecode.com/svn/wiki/image_cn/SQuirreL-AliasConf.png' /><br />
```
jdbc:FlashDB:rmi://127.0.0.1:1999/FlashDB
```
5、连接后，左侧显示如下图即表示成功。 <br />
> <img src='http://flashdb.googlecode.com/svn/wiki/image_cn/SQuirreL-AliasOpened.png' /><br />