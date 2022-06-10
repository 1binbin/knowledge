# 数据库连接

## 非数据源

### 配置文件（jdbc.properties）

- 存放于resources文件夹中

  ```properties
  driver=com.mysql.cj.jdbc.Driver
  user=root
  password=
  url=jdbc:mysql://localhost:3306/goodsims
  ```

### 获取连接

```java
 public static Connection getConnection() throws Exception {
        Properties properties = new Properties();
        String path = JdbcConnection.class.getClassLoader().getResource("jdbc.properties").getPath();
        path = URLDecoder.decode(path, "utf-8");
        FileInputStream fileInputStream = new FileInputStream(path);
        properties.load(fileInputStream);
        String driver = properties.get("driver").toString();
        String user = properties.get("user").toString();
        String password = properties.get("password").toString();
        String url = properties.get("url").toString();
        Class.forName(driver);
        Connection connection = DriverManager.getConnection(url, user, password);
        return connection;
    }
```

## 数据源
- 在META-INF/context.xml中配置数据源
```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!-- 数据源 -->
<Context reloadble = "true">
  <!--    MySQL版本8.0.25-->
  <Resource
          name = "jdbc/goodsMall"
          type = "javax.sql.DataSource"
          maxTotal = "1000"
          maxIdle = "300"
          driverClassName = "com.mysql.jdbc.Driver"
          url = "jdbc:mysql://localhost:3306/数据库名"
          username = "用户名"
          password = "密码"
          maxWaitMillis = "80000"
  />
</Context>
```
- 获取连接
  - 在service中获取连接
```java
  public static Connection getConnection() throws Exception {
      Context context = new InitialContext();
      DataSource dataSource = (DataSource) context.lookup("java:comp/env/jdbc/goodsMall");
      Connection connection = dataSource.getConnection();
      return connection;
    }
```

