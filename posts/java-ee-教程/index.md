# Java EE 教程


<!--more-->

## 面向对象设计模式

### DAO 模式

DAO（Data Access Objects，数据访问对象）是指位于业务逻辑和持久化数据之间实现对持久化数据的访问。通俗来讲，就是将数据库操作都封装起来。

**DAO 模式的优势：**

1. 隔离了数据访问代码和业务逻辑代码。业务逻辑代码直接调用 DAO 方法即可，完全感觉不到数据库表的存在。分工明确，数据访问层代码变化不影响业务逻辑代码，这符合单一职能原则，降低了耦合性，提高了可复用性。

1. 隔离了不同数据库实现。采用面向接口编程，如果底层数据库变化，如由 MySQL 变成 Oracle 只要增加 DAO 接口的新实现类即可，原有 MySQL 实现不用修改。这符合“开-闭”原则。该原则降低了代码的耦合性，提高了代码扩展性和系统的可移植性。

**DAO 模式组成部分：**

1. DAO 接口。把对数据库的所有操作定义成抽象方法，可以提供多种实现。

```java
package org.example.dao;

import org.example.entity.Website;

import java.util.List;

public interface WebsiteDAO {
    public void insert(Website website) throws Exception;

    public void update(Website website) throws Exception;

    public void delete(int websiteId) throws Exception;

    public Website queryById(int websiteId) throws Exception;

    public List<Website> queryAll() throws Exception;
}
```

2. DAO 实现类。针对不同数据库给出 DAO 接口定义方法的具体实现。

```java
package org.example.dao.impl;

import org.example.dao.WebsiteDAO;
import org.example.entity.Website;
import org.example.utils.DBConnectionUtil;

import java.sql.PreparedStatement;
import java.sql.ResultSet;
import java.util.ArrayList;
import java.util.List;

public class WebsiteDAOImpl implements WebsiteDAO {
    @Override
    public void insert(Website website) throws Exception {
        String sql = "insert into websites (website_name, website_url, website_rank, website_country) values (?, ?, ?, ?)";
        PreparedStatement stmt = null;
        DBConnectionUtil util = null;
        try {
            util = new DBConnectionUtil();
            stmt = util.getConnection().prepareStatement(sql);
            stmt.setString(1, website.getWebsiteName());
            stmt.setString(2, website.getWebsiteUrl());
            stmt.setInt(3, website.getWebsiteRank());
            stmt.setString(4, website.getWebsiteCountry());
            stmt.executeUpdate();
            stmt.close();
        } catch (Exception e) {
            throw new Exception("操作出现异常");
        } finally {
            if (util != null) {
                util.close();
            }
        }
    }

    @Override
    public void update(Website website) throws Exception {
        String sql = "update websites set website_name=?, website_url=?, website_rank=?, website_country=? where website_id=?";
        PreparedStatement stmt = null;
        DBConnectionUtil util = null;
        try {
            util = new DBConnectionUtil();
            stmt = util.getConnection().prepareStatement(sql);
            stmt.setString(1, website.getWebsiteName());
            stmt.setString(2, website.getWebsiteUrl());
            stmt.setInt(3, website.getWebsiteRank());
            stmt.setString(4, website.getWebsiteCountry());
            stmt.setInt(5, website.getWebsiteId());
            stmt.executeUpdate();
            stmt.close();
        } catch (Exception e) {
            throw new Exception("操作出现异常");
        } finally {
            if (util != null) {
                util.close();
            }
        }
    }

    @Override
    public void delete(int websiteId) throws Exception {
        String sql = "delete from websites where website_id=?";
        PreparedStatement stmt = null;
        DBConnectionUtil util = null;
        try {
            util = new DBConnectionUtil();
            stmt = util.getConnection().prepareStatement(sql);
            stmt.setInt(1, websiteId);
            stmt.executeUpdate();
            stmt.close();
        } catch (Exception e) {
            throw new Exception("操作出现异常");
        } finally {
            if (util != null) {
                util.close();
            }
        }
    }

    @Override
    public Website queryById(int websiteId) throws Exception {
        String sql = "select website_id, website_name, website_url, website_rank, website_country from websites where website_id=?";
        PreparedStatement stmt = null;
        DBConnectionUtil util = null;
        Website website = null;
        try {
            util = new DBConnectionUtil();
            stmt = util.getConnection().prepareStatement(sql);
            stmt.setInt(1, websiteId);
            ResultSet rs = stmt.executeQuery();
            while (rs.next()) {
                website = new Website();
                website.setWebsiteId(rs.getInt("website_id"));
                website.setWebsiteName(rs.getString("website_name"));
                website.setWebsiteUrl(rs.getString("website_url"));
                website.setWebsiteRank(rs.getInt("website_rank"));
                website.setWebsiteCountry(rs.getString("website_country"));
            }
            rs.close();
            stmt.close();
        } catch (Exception e) {
            throw new Exception("操作出现异常");
        } finally {
            if (util != null) {
                util.close();
            }
        }
        return website;
    }

    @Override
    public List<Website> queryAll() throws Exception {
        String sql = "select website_id, website_name, website_url, website_rank, website_country from websites";
        PreparedStatement stmt = null;
        DBConnectionUtil util = null;
        List<Website> websites = null;
        try {
            util = new DBConnectionUtil();
            stmt = util.getConnection().prepareStatement(sql);
            ResultSet rs = stmt.executeQuery();
            websites = new ArrayList<>();
            while (rs.next()) {
                Website website = new Website();
                website.setWebsiteId(rs.getInt("website_id"));
                website.setWebsiteName(rs.getString("website_name"));
                website.setWebsiteUrl(rs.getString("website_url"));
                website.setWebsiteRank(rs.getInt("website_rank"));
                website.setWebsiteCountry(rs.getString("website_country"));
                websites.add(website);
            }
            rs.close();
            stmt.close();
        } catch (Exception e) {
            throw new Exception("操作出现异常");
        } finally {
            if (util != null) {
                util.close();
            }
        }
        return websites;
    }
}
```

3. 实体类。用于存放与传输对象数据。

```java
package org.example.entity;

public class Website {
    public Website(int websiteId, String websiteName, String websiteUrl, int websiteRank, String websiteCountry) {
        this.websiteId = websiteId;
        this.websiteName = websiteName;
        this.websiteUrl = websiteUrl;
        this.websiteRank = websiteRank;
        this.websiteCountry = websiteCountry;
    }

    public Website() {
    }

    @Override
    public String toString() {
        return "Website{" +
                "websiteId=" + websiteId +
                ", websiteName='" + websiteName + '\'' +
                ", websiteUrl='" + websiteUrl + '\'' +
                ", websiteRank=" + websiteRank +
                ", websiteCountry='" + websiteCountry + '\'' +
                '}';
    }

    public int getWebsiteId() {
        return websiteId;
    }

    public void setWebsiteId(int websiteId) {
        this.websiteId = websiteId;
    }

    public String getWebsiteName() {
        return websiteName;
    }

    public void setWebsiteName(String websiteName) {
        this.websiteName = websiteName;
    }

    public String getWebsiteUrl() {
        return websiteUrl;
    }

    public void setWebsiteUrl(String websiteUrl) {
        this.websiteUrl = websiteUrl;
    }

    public int getWebsiteRank() {
        return websiteRank;
    }

    public void setWebsiteRank(int websiteRank) {
        this.websiteRank = websiteRank;
    }

    public String getWebsiteCountry() {
        return websiteCountry;
    }

    public void setWebsiteCountry(String websiteCountry) {
        this.websiteCountry = websiteCountry;
    }

    private int websiteId;
    private String websiteName;
    private String websiteUrl;
    private int websiteRank;
    private String websiteCountry;
}
```

4. 数据库连接和关闭工具类。避免了数据库连接和关闭代码的重复使用，方便修改。

```java
package org.example.utils;

import java.sql.Connection;
import java.sql.DriverManager;

public class MySQLConnectionUtil {
    private static final String DB_DRIVER = "com.mysql.cj.jdbc.Driver";
    private static final String DB_URL = "jdbc:mysql://localhost:3306/demo";
    private static final String DB_USER = "root";
    private static final String DB_PASSWORD = "12345678";
    private Connection connection = null;

    public MySQLConnectionUtil() {
        try {
            Class.forName(DB_DRIVER);
            this.connection = DriverManager.getConnection(DB_URL, DB_USER, DB_PASSWORD);
        } catch (Exception e) {
            System.out.println("加载驱动失败！");
        }
    }

    public Connection getConnection() {
        return connection;
    }

    public void close() {
        try {
            connection.close();
        } catch (Exception e) {
            System.out.println("数据库连接关闭失败！");
        }
    }
}
```

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
    // MySQL 8.0 以下版本
    // private static final String JDBC_DRIVER = "com.mysql.jdbc.Driver";
    private static final String JDBC_DRIVER = "com.mysql.cj.jdbc.Driver";
    private static final String DB_URL = "jdbc:mysql://localhost:3306/demo";

    public static void main(String[] args) {
        try {
            // 注册 JDBC 驱动
            Class.forName(JDBC_DRIVER);
            // 建立连接
            Connection conn = DriverManager.getConnection(DB_URL, "root", "12345678");
            // 执行 SQL
            Statement stmt = conn.createStatement();
            ResultSet rs = stmt.executeQuery("select * from websites");
            while (rs.next()) {
                int websiteId = rs.getInt("website_id");
                String websiteName = rs.getString("website_name");
                System.out.println(websiteId + " " + websiteName);
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
