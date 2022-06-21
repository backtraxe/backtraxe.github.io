# Java EE 教程


<!--more-->

## 面向对象设计模式

### DAO 模式

DAO（Data Access Objects，数据访问对象）是指位于业务逻辑和持久化数据之间实现对持久化数据的访问。通俗来讲，就是将数据库操作都封装起来。

**DAO 模式的优势：**

-

**DAO 模式组成部分：**

-

<br />

## Java 连接 MySQL

- 下载并在项目中导入`mysql-connector-java-8.0.29.jar`。

- 若是 Maven 项目，则在`pom.xml`中添加依赖。

```xml
<!-- pom.xml -->
<dependencies>
    <dependency>
        <groupId>mysql</groupId>
        <artifactId>mysql-connector-java</artifactId>
        <version>8.0.29</version>
    </dependency>
</dependencies>
```

```java
import java.sql.Connection;
import java.sql.DriverManager;
import java.sql.ResultSet;
import java.sql.Statement;

public class MySQLConnectionDemo {
    // MySQL 8.0 以下版本 - JDBC 驱动名及数据库 URL
    // static final String JDBC_DRIVER = "com.mysql.jdbc.Driver";
    static final String JDBC_DRIVER = "com.mysql.cj.jdbc.Driver";
    static final String DB_URL = "jdbc:mysql://localhost:3306/mysql_crash_course";

    public static void main(String[] args) {
        try {
            // 注册 JDBC 驱动
            Class.forName(JDBC_DRIVER);
            // 建立连接
            Connection conn = DriverManager.getConnection(DB_URL, "root", "12345678");
            // 执行 SQL
            Statement stmt = conn.createStatement();
            // // 增
            // ResultSet rs =
            // // 删
            // rs =
            // // 改
            // rs =
            // 查
            ResultSet rs = stmt.executeQuery("select * from vendors");
            while (rs.next()) {
                int vendId = rs.getInt("vend_id");
                String vendName = rs.getString("vend_name");
                System.out.println(vendId + " " + vendName);
            }
            // 关闭资源
            rs.close();
            stmt.close();
            conn.close();
        } catch (Exception e) {
            e.printStackTrace();
        }
    }
}
```
