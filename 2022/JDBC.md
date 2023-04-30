> `java.sql` **JDBC 2.0**  包含的接口和类采用传统的C/S体系结构设计思想.主要功能针对基本数据库编程服务,如生成连接,执行语句以及准备语句和运行批处理语句.也有一些高级功能如批处理更新,可滚动结果集,事务隔离以及SQL数据类型.
> `javax.sql` **JDBC 3.0 之后**
> 引入了JDBC编程方面一些主要的体系结构改变,并且为连接管理,分布式事务处理和老式连接提供了更好的抽象.这个包也引入了容器管理的连接缓冲池,分布式事务以及行集(
> rowset).



# `java.sql`

主要成员:

- DriverManager：此完全实现的类将应用程序连接到由数据库 URL 指定的数据源。当此类首次try构建连接时，它将自动加载在 Classpath 中找到的所有 JDBC 4.0
  驱动程序。请注意，您的应用程序必须手动加载 4.0 版之前的所有 JDBC 驱动程序。
-
DataSource：该interface优于DriverManager，因为它允许有关底层数据源的详细信息对您的应用程序透明。设置DataSource对象的属性，使其代表特定的数据源。有关更多信息，请参见[连接数据源对象](https://www.docs4dev.com/docs/zh/java/java8/tutorials/jdbc-basics-sqldatasources.html)
。有关使用DataSource类开发应用程序的更多信息，请参见最新的* [Java EE 教程](https://docs.oracle.com/javaee/6/tutorial/doc/) *。
- _Statement 普通无参数 SQL_
- _PreparedStatement 预处理 SQL_
- _CallableStatement 存储过程_

<br />
