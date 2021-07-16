---
title: mybatis2
date: 2021-07-16 16:09:29
tags:
  - mybatis
  - java
type: java                                                                         # 标签、分类和友情链接三個页面需要配置(必填)
description: MyBatis 是一款优秀的持久层框架，它支持自定义 SQL、存储过程以及高级映射。    # 描述
keywords: mybatis                  # 关键词，便于搜索
top_img: /images/mybatis/log.jpg             # 文章的顶部图片
aside: true                                                                         # 展示文章侧边栏(默认为true)
categories: 
  - 教程
  - mybatis                                                              # 文章标签
  - java
cover: /images/mybatis/log.jpg                 # 文章的缩略图（用在首页）
---

# MyBatis的多表操作
## 代码准备
  1. 新建itheima_mybatis_005的module, 并配置webapp文件夹
  2. 在pom.xml中导入相应的坐标
    ```
      <dependency>
        <groupId>mysql</groupId>
        <artifactId>mysql-connector-java</artifactId>
        <version>5.1.46</version>
      </dependency>
      <dependency>
        <groupId>org.mybatis</groupId>
        <artifactId>mybatis</artifactId>
        <version>3.5.6</version>
      </dependency>
      <dependency>
        <groupId>junit</groupId>
        <artifactId>junit</artifactId>
        <version>4.12</version>
        <scope>test</scope>
      </dependency>
      <dependency>
        <groupId>log4j</groupId>
        <artifactId>log4j</artifactId>
        <version>1.2.17</version>
      </dependency>
    ```
  3. 在mysql的test数据库中新建user表，添加username和password，birthday字段
  4. 在java下新建com.itheima.domain.User.java实体类
    ```
      package com.itheima.domain;
      import java.util.Date;
      public class User {
        private int id;
        private String username;
        private String password;
        private Date birthday;
        public Date getBirthday() {
            return birthday;
        }
        public void setBirthday(Date birthday) {
            this.birthday = birthday;
        }
        public int getId() {
            return id;
        }
        public void setId(int id) {
            this.id = id;
        }
        public String getUsername() {
            return username;
        }
        public void setUsername(String username) {
            this.username = username;
        }
        public String getPassword() {
            return password;
        }
        public void setPassword(String password) {
            this.password = password;
        }
        @Override
        public String toString() {
            return "User{" +
                    "id=" + id +
                    ", username='" + username + '\'' +
                    ", password='" + password + '\'' +
                    ", birthday=" + birthday +
                    '}';
        }
      }
    ```
  5. 在java下新建com.itheima.mapper.UserMapper.java
    ```
      package com.itheima.mapper;
      public interface UserMapper {
      }
    ```
  6. 在resources下新建mapper/UserMapper.xml
    ```
      <?xml version="1.0" encoding="UTF-8" ?>
      <!DOCTYPE mapper PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN" "http://mybatis.org/dtd/mybatis-3-mapper.dtd">
      <mapper namespace="com.itheima.mapper.UserMapper">
          
      </mapper>
    ```
  7. 在resources下新建jdbc.properties
    ```
      jdbc.driver=com.mysql.jdbc.Driver
      jdbc.url=jdbc:mysql://localhost:3306/test
      jdbc.username=root
      jdbc.password=root
    ```
  8. 在resources下新建log4j.properties
    ```
      ### direct log messages to stdout ###
      log4j.appender.stdout=org.apache.log4j.ConsoleAppender
      log4j.appender.stdout.Target=System.out
      log4j.appender.stdout.layout=org.apache.log4j.PatternLayout
      log4j.appender.stdout.layout.ConversionPattern=%d{ABSOLUTE} %5p %c{1}:%L - %m%n

      ### direct messages to file mylog.log ###
      log4j.appender.file=org.apache.log4j.FileAppender
      log4j.appender.file.File=d:/upload/mylog.log
      log4j.appender.file.layout=org.apache.log4j.PatternLayout
      log4j.appender.file.layout.ConversionPattern=%d{ABSOLUTE} %5p %c{1}:%L - %m%n

      ### set log levels - for more verbose logging change 'info' to 'debug' ###

      log4j.rootLogger=debug, stdout
    ```
  9. 在resources下新建sqlMapConfig.xml
    ```
      <?xml version="1.0" encoding="UTF-8" ?>
      <!DOCTYPE configuration PUBLIC "-//mybatis.org//DTD Config 3.0//EN" "http://mybatis.org/dtd/mybatis-3-config.dtd">
      <configuration>
          <!--通过properties标签加载jdbc.properties-->
          <properties resource="jdbc.properties"></properties>

          <!--设置别名-->
          <typeAliases>
              <typeAlias type="com.itheima.domain.User" alias="user"></typeAlias>
          </typeAliases>

          <!--数据源环境-->
          <environments default="dev">
              <environment id="dev">
                  <transactionManager type="JDBC"></transactionManager>
                  <dataSource type="POOLED">
                      <property name="driver" value="${jdbc.driver}"/>
                      <property name="url" value="${jdbc.url}"/>
                      <property name="username" value="${jdbc.username}"/>
                      <property name="password" value="${jdbc.password}"/>
                  </dataSource>
              </environment>
          </environments>
          <!--加载映射文件-->
          <mappers>
            <mapper resource="mapper/UserMapper.xml"></mapper>
          </mappers>
      </configuration>
    ```
## 一对一查询
### 一对一查询的模型
  * 用户表和订单表的关系为，一个用户有多个订单，一个订单只从属于一个用户
  * 一对一查询的需求：查询一个订单，与此同时查询出该订单所属的用户。 ![多表操作](/images/mybatis/多表操作.jpg)

### 一对一查询的语句
  * 对应的sql语句：select *  from orders o,user u where o.uid=u.id;
  * 查询的结果如下：

### 创建Order和User实体
  * 在domain里，新建Order.java实体类  
    ```
      package com.itheima.domain;
      import java.util.Date;
      public class Order {
          private int id;
          private Date orderTime;
          private double total;
          // 代表当前订单从属于哪一个用户
          private User user;
          public int getId() {
              return id;
          }
          public void setId(int id) {
              this.id = id;
          }
          public Date getOrderTime() {
              return orderTime;
          }
          public void setOrderTime(Date orderTime) {
              this.orderTime = orderTime;
          }
          public double getTotal() {
              return total;
          }
          public void setTotal(double total) {
              this.total = total;
          }
          public User getUser() {
              return user;
          }
          public void setUser(User user) {
              this.user = user;
          }
          @Override
          public String toString() {
              return "Order{" +
                      "id=" + id +
                      ", orderTime=" + orderTime +
                      ", total=" + total +
                      ", user=" + user +
                      '}';
          }
      }
    ```
  * 在test数据库中新建orders表，添加id、ordertime、total、uid字段。![order表](/images/mybatis/多表操作2.jpg)

### 创建OrderMapper接口
  * 在mapper下新建OrderMapper.java的接口，添加findAll方法
    ```
      package com.itheima.mapper;
      import com.itheima.domain.Order;
      import java.util.List;
      public interface OrderMapper {
          List<Order> findAll();
      }
    ```

### 在resources/mapper下新建OrderMapper.xml
  * 新建OrderMapper.xml
    ```
      <?xml version="1.0" encoding="UTF-8" ?>
      <!DOCTYPE mapper PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN" "http://mybatis.org/dtd/mybatis-3-mapper.dtd">
      <mapper namespace="com.itheima.mapper.OrderMapper">

      </mapper>
    ```
  * 在resources下的sqlMapConfig中引入OrderMapper.xml
    ```
      <!--加载映射文件-->
      <mappers>
          <mapper resource="mapper/UserMapper.xml"></mapper>
          <mapper resource="mapper/OrderMapper.xml"></mapper>
      </mappers>
    ```



