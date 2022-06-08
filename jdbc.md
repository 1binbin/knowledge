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

