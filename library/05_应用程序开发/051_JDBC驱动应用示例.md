## JDBC驱动应用示例  
本章将介绍GBase 8s的JDBC的基础的操作演示。  

## 示例环境介绍  
| 软件 | 版本 |
| --- | --- |
| JDBC驱动 | gbasedbtjdbc_3.5.1.jar |
| JDK | 1.8 |

## JDBC驱动下载  
JDBC驱包的名称一般为：gbasedbtjdbc_3.5.1.jar  
官方下载地址：[https://www.gbase.cn/download/gbase-8s-1?category=DRIVER_PACKAGE](https://www.gbase.cn/download/gbase-8s-1?category=DRIVER_PACKAGE)  

## 编写JAVA文件  

- 编写示例使用的java文件：JdbcSample.java  

```java  
import java.sql.*;

public class JdbcSample {
    public static String DRIVER_CLASSNAME = "com.gbasedbt.jdbc.Driver";
    public static String DRIVER_URL="jdbc:gbasedbt-sqli://192.168.80.70:9088/testdb:GBASEDBTSERVER=gbase01;DB_LOCALE=zh_CN.utf8;CLIENT_LOCALE=zh_CN.utf8;IFX_LOCK_MODE_WAIT=10";
    public static String DRIVER_USER="gbasedbt";
    public static String DRIVER_PASS="GBase123$%";
    public static void main(String[] args) throws ClassNotFoundException, SQLException {
        Connection connection = null;
        PreparedStatement preparedStatement = null;
        ResultSet resultSet = null;
        String sqlstr = "";

        Class.forName(DRIVER_CLASSNAME);
        connection = DriverManager.getConnection(DRIVER_URL,DRIVER_USER,DRIVER_PASS);
        System.out.println("Connection succeed!");

        sqlstr = "drop table if exists company";
        preparedStatement = connection.prepareStatement(sqlstr);
        preparedStatement.executeUpdate();
        System.out.println("drop table company succeed!");

        sqlstr = "create table company(coid serial,coname varchar(255),coaddr varchar(255), primary key(coid))";
        preparedStatement = connection.prepareStatement(sqlstr);
        preparedStatement.executeUpdate();
        System.out.println("create table company succeed!");

        sqlstr = "insert into company values(0,?,?),(0,?,?)";
        preparedStatement = connection.prepareStatement(sqlstr);
        preparedStatement.setString(1,"GBase");
        preparedStatement.setString(2,"TJ");
        preparedStatement.setString(3,"GBase BeiJing");
        preparedStatement.setString(4,"BJ");
        preparedStatement.executeUpdate();
        System.out.println("insert table company succeed!");

        sqlstr = "select * from company";
        preparedStatement = connection.prepareStatement(sqlstr);
        resultSet = preparedStatement.executeQuery();
        while(resultSet.next()){
            System.out.println(resultSet.getObject(1) + "\t" + resultSet.getObject(2) + "\t" + resultSet.getObject(3));
        }
        System.out.println("select table company succeed!");

        resultSet.close();
        preparedStatement.close();
        connection.close();
    }
}
```

其中：  

| **属性名称** | **属性说明** | **示例值** |
| --- | --- | --- |
| DRIVER_CLASSNAME | 数据库驱动类名称 | com.gbasedbt.jdbc.Driver |
| DRIVER_URL | 数据库连接字符串 | jdbc:gbasedbt-sqli://192.168.80.70:9088/testdb:GBASEDBTSERVER=gbase01;DB_LOCALE=zh_CN.utf8;CLIENT_LOCALE=zh_CN.utf8;IFX_LOCK_MODE_WAIT=10 |
| DRIVER_USER | 数据库连接用户 | gbasedbt |
| DRIVER_PASS | 用户使用的密码 | GBase123$% |

注：DRIVER_URL使用 安装部署 完成时的信息。  

- 编译JdbcSample.java  

```shell
javac JdbcSample.java
```

生成JdbcSample.classs

- JDBC测试  

```text
[gbasedbt@node2 ~]$ java -cp .:gbasedbtjdbc_3.5.1.jar JdbcSample
Connection succeed!
drop table company succeed!
create table company succeed!
insert table company succeed!
1       GBase   TJ
2       GBase BeiJing   BJ
select table company succeed!
```

注：-cp 指定当前目录 . 和 gbasedbtjdbc_3.5.1.jar 为CLASSPATH
