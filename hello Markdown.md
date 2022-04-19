```java

package com.bishop.dictionary;



import java.sql.Connection;
import java.sql.DriverManager;
import java.sql.PreparedStatement;
import java.sql.ResultSet;
import java.sql.SQLException;
import java.util.Properties;

public class DBconnection {
    public static final String URL = "jdbc:mysql://localhost:3306/dictionary?useSSL=false&serverTimezone=UTC&useUnicode=true&characterEncoding=utf-8&user=root&password=226757";

    private final String userName = "root";
    private final String password = "226757";
    private final String serverName = "localhost";
    private final int portNumber = 3306;
    private final String dbName = "dictionary";
    private final String tableName = "word";
    private PreparedStatement statement = null;

    public DBconnection() {
    }

    public Connection getConnection() {
        Connection conn = null;

        Properties connectionProps = new Properties();
        connectionProps.put("user", "root");
        connectionProps.put("password", "226757");

        try {
            Class.forName("com.mysql.cj.jdbc.Driver");
            conn = DriverManager.getConnection(URL);
            //System.out.println("数据库连接成功");
        } catch (SQLException var4) {
            System.err.println("Exception occured in method 'getConnection' in class 'DBConnection'");
            var4.printStackTrace();
        } catch (ClassNotFoundException e) {
            e.printStackTrace();
        }

        return conn;
    }


    public ResultSet getCn(String En) {
        String query = "SELECT cn FROM words WHERE en = '" + En + "'";
        ResultSet rs = null;

        try {
            this.statement = this.getConnection().prepareStatement(query, 1005);
            rs = this.statement.executeQuery();
        } catch (SQLException var5) {
            System.err.println("Exception occured in method 'getCn' in class 'DBConnection'");
            var5.printStackTrace();
        }

        return rs;
    }

    public int addWord(String En,String Cn) {
        String query = "insert into words values(null,'"+En+ "','"+Cn+"')";

        int rsadd = 0;

        try {
            this.statement = this.getConnection().prepareStatement(query, 1005);
            rsadd = this.statement.executeUpdate();
        } catch (SQLException var5) {
            System.err.println("Exception occured in method 'addWord' in class 'DBConnection'");
            var5.printStackTrace();
        }

        return rsadd;
    }
    public int delWord(String En) {
        String query = "DELETE FROM words WHERE en = '" + En + "'";
        int rsdel = 0;

        try {
            this.statement = this.getConnection().prepareStatement(query, 1005);
            rsdel = this.statement.executeUpdate();
        } catch (SQLException var5) {
            System.err.println("Exception occured in method 'delWord' in class 'DBConnection'");
            var5.printStackTrace();
        }

        return rsdel;
    }
}
```



##  hello Markdown

	## 二级标题

对于字体的设计使用#来设计为几级标题 #越多标题越大

字体加粗 字体两边分别两个星号*





*hello word*

**hello word**



***hello word***

~ hello word ~

hello word

## 引用

使用大于号 即可实现引用效果

> 学好数理化走遍天下都不怕

	## 分割线



三个减号 或者三个星号  即刻实现分割线效果



---



## 图片

![截图](https://img-blog.csdn.net/20180905111557512?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L2phbm55X2Zsb3dlcg==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)



图片使用        ![图片](C:\Users\ywjia\AppData\Roaming\Typora\typora-user-images\image-20220414200241484.png)

来使用图片

## 超链接

[点击进入博客  ](https://www.bilibili.com/)

使用[]（）来进行超链接操作





## 列表

可是使用数字 点 空格来实现有序

或者可以使用- 空格来实现点

1. hello
2. 你来了
3. 我要走了

- 

## 表格

进行插入表格

|      |      |      |
| ---- | ---- | ---- |
|      |      |      |
|      |      |      |
|      |      |      |

## 代码块

```java
public void main
    
```

