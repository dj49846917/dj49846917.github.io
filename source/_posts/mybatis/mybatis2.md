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
  * 对应的sql语句：SELECT o.ordertime,o.total,o.id oid,u.id uid,u.username,u.`password`,u.birthday FROM `order` o, `user` u WHERE o.uid = u.id;
  * 查询的结果如下：

### 创建Order和User实体
  * 在domain里，新建Order.java实体类  
    ```
      package com.itheima.domain;
      import java.util.Date;
      public class Order {
          private int id;
          private Date ordertime;
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
              return ordertime;
          }
          public void setOrderTime(Date ordertime) {
              this.ordertime = ordertime;
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
                      ", ordertime=" + ordertime +
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

### 配置OrderMapper.xml
  * 在resources/mapper下新建OrderMapper.xml，并配置findAll方法
    ```
      <?xml version="1.0" encoding="UTF-8" ?>
      <!DOCTYPE mapper PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN" "http://mybatis.org/dtd/mybatis-3-mapper.dtd">
      <mapper namespace="com.itheima.mapper.OrderMapper">
        <resultMap id="orderMap" type="order">
          <!--手动指定字段与实体属性的映射关系
              column: 数据表的字段名称
              property: 实体的属性名称
          -->
          <id column="oid" property="id"></id>
          <result column="ordertime" property="ordertime"></result>
          <result column="total" property="total"></result>
          <result column="uid" property="user.id"></result>
          <result column="username" property="user.username"></result>
          <result column="password" property="user.password"></result>
          <result column="birthday" property="user.birthday"></result>
        </resultMap>
        <select id="findAll" resultMap="orderMap">
            SELECT o.ordertime,o.total,o.id oid,u.id uid,u.username,u.`password`,u.birthday FROM `order` o, `user` u WHERE o.uid = u.id
        </select>
      </mapper>
    ```
  * 写法二，使用association
    ```
      <?xml version="1.0" encoding="UTF-8" ?>
      <!DOCTYPE mapper PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN" "http://mybatis.org/dtd/mybatis-3-mapper.dtd">
      <mapper namespace="com.itheima.mapper.OrderMapper">
          <resultMap id="orderMap" type="order">
              <!--手动指定字段与实体属性的映射关系
                  column: 数据表的字段名称
                  property: 实体的属性名称
              -->
              <id column="oid" property="id"></id>
              <result column="ordertime" property="ordertime"></result>
              <result column="total" property="total"></result>
              <!--
                  property: 当前实体(Order)中的属性名称(private User user)
                  javaType: 当前实体(order)中的属性的类型(com.itheima.domain.User别名为user)
              -->
              <association property="user" javaType="user">
                  <result column="uid" property="id"></result>
                  <result column="username" property="username"></result>
                  <result column="password" property="password"></result>
                  <result column="birthday" property="birthday"></result>
              </association>
          </resultMap>

          <select id="findAll" resultMap="orderMap">
              SELECT o.ordertime,o.total,o.id oid,u.id uid,u.username,u.`password`,u.birthday FROM `order` o, `user` u WHERE o.uid = u.id
          </select>
      </mapper>
    ```
  * 在resources下的sqlMapConfig中引入OrderMapper.xml
    ```
      <!--设置别名-->
      <typeAliases>
          <typeAlias type="com.itheima.domain.User" alias="user"></typeAlias>
          <typeAlias type="com.itheima.domain.Order" alias="order"></typeAlias>
      </typeAliases>

      <!--加载映射文件-->
      <mappers>
          <mapper resource="mapper/UserMapper.xml"></mapper>
          <mapper resource="mapper/OrderMapper.xml"></mapper>
      </mappers>
    ```

### 编写测试方法
  * 在test下新建com.itheima.test.OrderTest.java的测试类
    ```
      package com.itheima.test;
      import com.itheima.domain.Order;
      import com.itheima.mapper.OrderMapper;
      import org.apache.ibatis.io.Resources;
      import org.apache.ibatis.session.SqlSession;
      import org.apache.ibatis.session.SqlSessionFactory;
      import org.apache.ibatis.session.SqlSessionFactoryBuilder;
      import org.junit.Test;
      import java.io.IOException;
      import java.io.InputStream;
      import java.util.List;
      public class OrderTest {
          @Test
          public void order1Test() throws IOException {
              InputStream resourceAsStream = Resources.getResourceAsStream("sqlMapConfig.xml");
              SqlSessionFactory sqlSessionFactory = new SqlSessionFactoryBuilder().build(resourceAsStream);
              SqlSession sqlSession = sqlSessionFactory.openSession();
              OrderMapper mapper = sqlSession.getMapper(OrderMapper.class);
              List<Order> orderList = mapper.findAll();
              for (Order order : orderList) {
                  System.out.println(order);
              }
          }
      }
    ```

## 一对多查询
### 一对多查询
  * 用户表和订单表的关系为，一个用户有多个订单，一个订单只从属于一个用户
  * 一对多查询的需求：查询一个用户，与此同时查询出该用户具有的订单 ![一对多](/images/mybatis/多表操作3.jpg)

### 一对多查询的语句
  * 对应的sql语句：SELECT o.ordertime,o.total,o.id oid,u.id uid,u.username,u.`password`,u.birthday FROM `order` o, `user` u WHERE o.uid = u.id; ![一对多](/images/mybatis/多表操作4.jpg)

### 修改domain下的User实体
  ```
    package com.itheima.domain;
    import java.util.Date;
    import java.util.List;
    public class User {
        private int id;
        private String username;
        private String password;
        private Date birthday;
        // 当前用户存在哪些订单
        private List<Order> orderList;
        @Override
        public String toString() {
            return "User{" +
                    "id=" + id +
                    ", username='" + username + '\'' +
                    ", password='" + password + '\'' +
                    ", birthday=" + birthday +
                    ", orderList=" + orderList +
                    '}';
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
        public Date getBirthday() {
            return birthday;
        }
        public void setBirthday(Date birthday) {
            this.birthday = birthday;
        }
        public List<Order> getOrderList() {
            return orderList;
        }
        public void setOrderList(List<Order> orderList) {
            this.orderList = orderList;
        }
    }
  ```

### 在UserMapper接口中添加findAllList方法
  ```
    package com.itheima.mapper;
    import com.itheima.domain.User;
    import java.util.List;
    public interface UserMapper {
        List<User> findAllList();
    }
  ```

### 在UserMapper.xml中配置findAllList方法
  ```
    <?xml version="1.0" encoding="UTF-8" ?>
    <!DOCTYPE mapper PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN" "http://mybatis.org/dtd/mybatis-3-mapper.dtd">
    <mapper namespace="com.itheima.mapper.UserMapper">
        <resultMap id="userMap" type="user">
            <id column="uid" property="id"></id>
            <result column="username" property="username"></result>
            <result column="password" property="password"></result>
            <result column="birthday" property="birthday"></result>
            <!--配置集合信息
                property: 集合名称
                ofType: 当前集合中的数据类型
            -->
            <collection property="orderList" ofType="order">
                <id column="oid" property="id"></id>
                <result column="ordertime" property="ordertime"></result>
                <result column="total" property="total"></result>
            </collection>
        </resultMap>

        <select id="findAllList" resultMap="userMap">
            SELECT o.ordertime,o.total,o.id oid,u.id uid,u.username,u.`password`,u.birthday FROM `order` o, `user` u WHERE o.uid = u.id
        </select>
    </mapper>
  ```

### 编写测试方法
  ```
    package com.itheima.test;
    import com.itheima.domain.User;
    import com.itheima.mapper.UserMapper;
    import org.apache.ibatis.io.Resources;
    import org.apache.ibatis.session.SqlSession;
    import org.apache.ibatis.session.SqlSessionFactory;
    import org.apache.ibatis.session.SqlSessionFactoryBuilder;
    import org.junit.Test;
    import java.io.IOException;
    import java.io.InputStream;
    import java.util.List;
    public class UserTest {
        @Test
        public void orderTest() throws IOException {
            InputStream resourceAsStream = Resources.getResourceAsStream("sqlMapConfig.xml");
            SqlSessionFactory sqlSessionFactory = new SqlSessionFactoryBuilder().build(resourceAsStream);
            SqlSession sqlSession = sqlSessionFactory.openSession();
            UserMapper mapper = sqlSession.getMapper(UserMapper.class);
            List<User> userList = mapper.findAllList();
            for (User user : userList) {
                System.out.println(user);
            }
        }
    }
  ```

## 多对多查询的模型
  * 用户表和角色表的关系为，一个用户有多个角色，一个角色被多个用户使用
  * 多对多查询的需求：查询用户同时查询出该用户的所有角色 ![多对多](/images/mybatis/多表操作5.jpg)

### 多对多查询的语句
  * 对应的sql语句：select u.id userId,r.id roleId,u.username,u.`password`,u.birthday,r.roleName,r.roleDesc from `user` u, sys_user_role ur, sys_role r WHERE u.id = ur.userId AND ur.roleId = r.id;
  * 查询的结果如下: ![多对多](/images/mybatis/多表操作5.jpg)

### 创建sys_role,sys_user_role表
  ```
    -- 创建sys_user_role表
    DROP TABLE IF EXISTS `sys_user_role`;
    CREATE TABLE `sys_user_role` (
      `userId` bigint(20) NOT NULL,
      `roleId` bigint(20) NOT NULL,
      PRIMARY KEY (`userId`,`roleId`),
      KEY `roleId` (`roleId`),
      CONSTRAINT `sys_user_role_ibfk_1` FOREIGN KEY (`userId`) REFERENCES `sys_user` (`id`),
      CONSTRAINT `sys_user_role_ibfk_2` FOREIGN KEY (`roleId`) REFERENCES `sys_role` (`id`)
    ) ENGINE=InnoDB DEFAULT CHARSET=utf8;

    INSERT INTO `sys_user_role` VALUES ('1', '1');
    INSERT INTO `sys_user_role` VALUES ('1', '2');
    INSERT INTO `sys_user_role` VALUES ('2', '2');
    INSERT INTO `sys_user_role` VALUES ('2', '3');

    -- 创建sys_role表
    DROP TABLE IF EXISTS `sys_role`;
    CREATE TABLE `sys_role` (
      `id` bigint(20) NOT NULL AUTO_INCREMENT,
      `roleName` varchar(50) DEFAULT NULL,
      `roleDesc` varchar(50) DEFAULT NULL,
      PRIMARY KEY (`id`)
    ) ENGINE=InnoDB AUTO_INCREMENT=15 DEFAULT CHARSET=utf8;

    -- ----------------------------
    -- Records of sys_role
    -- ----------------------------
    INSERT INTO `sys_role` VALUES ('1', '院长', '负责全面工作');
    INSERT INTO `sys_role` VALUES ('2', '研究员', '课程研发工作');
    INSERT INTO `sys_role` VALUES ('3', '讲师', '授课工作');
    INSERT INTO `sys_role` VALUES ('4', '助教', '协助解决学生的问题');
  ```

### 创建Role实体类
  * 在domain下新建Role.java实体类
    ```
      package com.itheima.domain;
      public class Role {
          private int id;
          private String roleName;
          private String roleDesc;
          @Override
          public String toString() {
              return "Role{" +
                      "id=" + id +
                      ", roleName='" + roleName + '\'' +
                      ", roleDesc='" + roleDesc + '\'' +
                      '}';
          }
          public int getId() {
              return id;
          }
          public void setId(int id) {
              this.id = id;
          }
          public String getRoleName() {
              return roleName;
          }
          public void setRoleName(String roleName) {
              this.roleName = roleName;
          }
          public String getRoleDesc() {
              return roleDesc;
          }
          public void setRoleDesc(String roleDesc) {
              this.roleDesc = roleDesc;
          }
      }
    ```

### 修改User实体类
  * 在domain/User.java中，添加角色列表
    ```
      package com.itheima.domain;
      import java.util.Date;
      import java.util.List;
      public class User {
          private int id;
          private String username;
          private String password;
          private Date birthday;
          // 当前用户具备哪些角色
          private List<Role> roleList;
          @Override
          public String toString() {
              return "User{" +
                      "id=" + id +
                      ", username='" + username + '\'' +
                      ", password='" + password + '\'' +
                      ", birthday=" + birthday +
                      ", roleList=" + roleList +
                      '}';
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
          public Date getBirthday() {
              return birthday;
          }
          public void setBirthday(Date birthday) {
              this.birthday = birthday;
          }
          public List<Role> getRoleList() {
              return roleList;
          }
          public void setRoleList(List<Role> roleList) {
              this.roleList = roleList;
          }
      }
    ```

### 在UserMapper.java中添加findUserAndRoleAll方法
  ```
    package com.itheima.mapper;
    import com.itheima.domain.User;
    import java.util.List;
    public interface UserMapper {
        List<User> findUserAndRoleAll();
    }
  ```

### 配置findUserAndRoleAll方法
  * 在resources/mapper/UserMapper.xml中，配置findUserAndRoleAll方法
    ```
      <?xml version="1.0" encoding="UTF-8" ?>
      <!DOCTYPE mapper PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN" "http://mybatis.org/dtd/mybatis-3-mapper.dtd">
      <mapper namespace="com.itheima.mapper.UserMapper">
          <resultMap id="userAndRole" type="user">
              <!--user的信息-->
              <id column="userid" property="id"></id>
              <result column="username" property="username"></result>
              <result column="password" property="password"></result>
              <result column="birthday" property="birthday"></result>
              <!--user内部的roleList信息-->
              <collection property="roleList" ofType="role">
                  <id column="roleId" property="id"></id>
                  <result column="roleName" property="roleName"></result>
                  <result column="roleDesc" property="roleDesc"></result>
              </collection>
          </resultMap>
          <select id="findUserAndRoleAll" resultMap="userAndRole">
              select u.id userId,r.id roleId,u.username,u.`password`,u.birthday,r.roleName,r.roleDesc from `user` u, sys_user_role ur, sys_role r WHERE u.id = ur.userId AND ur.roleId = r.id;
          </select>
      </mapper>
    ```
  * 在resources/sqlMapConfig.xml中，添加role实体类的别名
    ```
      <!--设置别名-->
      <typeAliases>
          <typeAlias type="com.itheima.domain.User" alias="user"></typeAlias>
          <typeAlias type="com.itheima.domain.Order" alias="order"></typeAlias>
          <typeAlias type="com.itheima.domain.Role" alias="role"></typeAlias>
      </typeAliases>
    ```

### 编写测试方法
  ```
    package com.itheima.test;
    import com.itheima.domain.User;
    import com.itheima.mapper.UserMapper;
    import org.apache.ibatis.io.Resources;
    import org.apache.ibatis.session.SqlSession;
    import org.apache.ibatis.session.SqlSessionFactory;
    import org.apache.ibatis.session.SqlSessionFactoryBuilder;
    import org.junit.Test;
    import java.io.IOException;
    import java.io.InputStream;
    import java.util.List;
    public class UserTest {
        @Test
        public void orderTest() throws IOException {
            InputStream resourceAsStream = Resources.getResourceAsStream("sqlMapConfig.xml");
            SqlSessionFactory sqlSessionFactory = new SqlSessionFactoryBuilder().build(resourceAsStream);
            SqlSession sqlSession = sqlSessionFactory.openSession();
            UserMapper mapper = sqlSession.getMapper(UserMapper.class);
            List<User> userList = mapper.findAllList();
            for (User user : userList) {
                System.out.println(user);
            }
        }
    }
  ```

## 知识小结
### MyBatis多表配置方式：
  ```
    一对一配置：使用<resultMap>做配置
    一对多配置：使用<resultMap>+<collection>做配置
    多对多配置：使用<resultMap>+<collection>做配置
  ```

# MyBatis的注解开发
## MyBatis的常用注解
  * 这几年来注解开发越来越流行，Mybatis也可以使用注解开发方式，这样我们就可以减少编写Mapper
映射文件了。我们先围绕一些基本的CRUD来学习，再学习复杂映射多表操作。
  * @Insert：实现新增
  * @Update：实现更新
  * @Delete：实现删除
  * @Select：实现查询
  * @Result：实现结果集封装
  * @Results：可以与@Result 一起使用，封装多个结果集
  * @One：实现一对一结果集封装
  * @Many：实现一对多结果集封装

## 代码准备
  1. 新建itheima_mybatis.006_anno的module，并配置webapp文件夹
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
      import com.itheima.domain.User;
      import java.util.List;
      public interface UserMapper {
          void addItem(User user);
          void updateItem(User user);
          void deleteItem(int id);
          User findById(int id);
          List<User> findAll();
      }
    ```
  6. 在resources下新建mapper/UserMapper.xml
    ```
      <?xml version="1.0" encoding="UTF-8" ?>
      <!DOCTYPE mapper PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN" "http://mybatis.org/dtd/mybatis-3-mapper.dtd">
      <mapper namespace="com.itheima.mapper.UserMapper">
          <insert id="addItem" parameterType="user">
              insert into user values(#{id},#{username},#{password},#{birthday})
          </insert>
          <update id="updateItem" parameterType="user">
              update user set username=#{username},password=#{password} where id=#{id}
          </update>
          <delete id="deleteItem" parameterType="int">
              delete from user where id=#{id}
          </delete>
          <select id="findById" resultMap="user" parameterType="int">
              select * from user where id=#{id}
          </select>
          <select id="findAll" resultMap="user">
              select * from user
          </select>
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
  10. 编写测试方法
    ```
      package com.itheima.test;
      import com.itheima.domain.User;
      import com.itheima.mapper.UserMapper;
      import org.apache.ibatis.io.Resources;
      import org.apache.ibatis.session.SqlSession;
      import org.apache.ibatis.session.SqlSessionFactory;
      import org.apache.ibatis.session.SqlSessionFactoryBuilder;
      import org.junit.Test;
      import java.io.IOException;
      import java.io.InputStream;
      import java.util.Date;
      import java.util.List;
      public class mybatisTest {
          @Test
          public void findAllTest() throws IOException {
              InputStream resourceAsStream = Resources.getResourceAsStream("sqlMapConfig.xml");
              SqlSessionFactory sqlSessionFactory = new SqlSessionFactoryBuilder().build(resourceAsStream);
              SqlSession sqlSession = sqlSessionFactory.openSession();
              UserMapper mapper = sqlSession.getMapper(UserMapper.class);
              List<User> userList = mapper.findAll();
              for (User user : userList) {
                  System.out.println(user);
              }
              sqlSession.close();
          }

          @Test
          public void findByIdTest2() throws IOException {
              InputStream resourceAsStream = Resources.getResourceAsStream("sqlMapConfig.xml");
              SqlSessionFactory sqlSessionFactory = new SqlSessionFactoryBuilder().build(resourceAsStream);
              SqlSession sqlSession = sqlSessionFactory.openSession();
              UserMapper mapper = sqlSession.getMapper(UserMapper.class);

              User byId = mapper.findById(1);
              System.out.println(byId);
              sqlSession.close();
          }

          @Test
          public void addItemTest3() throws IOException {
              InputStream resourceAsStream = Resources.getResourceAsStream("sqlMapConfig.xml");
              SqlSessionFactory sqlSessionFactory = new SqlSessionFactoryBuilder().build(resourceAsStream);
              SqlSession sqlSession = sqlSessionFactory.openSession();
              UserMapper mapper = sqlSession.getMapper(UserMapper.class);

              User user = new User();
              user.setUsername("dj");
              user.setPassword("123456");
              user.setBirthday(new Date());
              mapper.addItem(user);
              sqlSession.commit();
              sqlSession.close();
          }

          @Test
          public void updateItemTest4() throws IOException {
              InputStream resourceAsStream = Resources.getResourceAsStream("sqlMapConfig.xml");
              SqlSessionFactory sqlSessionFactory = new SqlSessionFactoryBuilder().build(resourceAsStream);
              SqlSession sqlSession = sqlSessionFactory.openSession();
              UserMapper mapper = sqlSession.getMapper(UserMapper.class);

              User user = new User();
              user.setId(7);
              user.setUsername("dj2");
              user.setPassword("666");
              user.setBirthday(new Date());
              mapper.updateItem(user);
              sqlSession.commit();
              sqlSession.close();
          }

          @Test
          public void deleteItemTest4() throws IOException {
              InputStream resourceAsStream = Resources.getResourceAsStream("sqlMapConfig.xml");
              SqlSessionFactory sqlSessionFactory = new SqlSessionFactoryBuilder().build(resourceAsStream);
              SqlSession sqlSession = sqlSessionFactory.openSession();
              UserMapper mapper = sqlSession.getMapper(UserMapper.class);
              mapper.deleteItem(7);
              sqlSession.commit();
              sqlSession.close();
          }
      }
    ```

## MyBatis的增删改查
  1. 删除resources/mapper/UserMapper.xml
  2. 去掉sqlMapConfig.xml中引入UserMapper.xml的部分
    ```
      <mappers>
          <mapper resource="mapper/UserMapper.xml"></mapper>
      </mappers>
    ```
  3. 修改mapper/UserMapper.java中的代码, 添加注解
    ```
      package com.itheima.mapper;
      import com.itheima.domain.User;
      import org.apache.ibatis.annotations.Delete;
      import org.apache.ibatis.annotations.Insert;
      import org.apache.ibatis.annotations.Select;
      import org.apache.ibatis.annotations.Update;
      import java.util.List;
      public interface UserMapper {
          @Insert("insert into user values(#{id},#{username},#{password},#{birthday})")
          void addItem(User user);
          @Update("update user set username=#{username},password=#{password} where id=#{id}")
          void updateItem(User user);
          @Delete("delete from user where id=#{id}")
          void deleteItem(int id);
          @Select("select * from user where id=#{id}")
          User findById(int id);
          @Select("select * from user")
          List<User> findAll();
      }
    ```
  4. 找到sqlMapConfig.xml，扫描注解
    ```
      <mappers>
        <package name="com.itheima.mapper"/>
      </mappers>
    ```
  5. 编写测试方法
    ```
      package com.itheima.test;
      import com.itheima.domain.User;
      import com.itheima.mapper.UserMapper;
      import org.apache.ibatis.io.Resources;
      import org.apache.ibatis.session.SqlSession;
      import org.apache.ibatis.session.SqlSessionFactory;
      import org.apache.ibatis.session.SqlSessionFactoryBuilder;
      import org.junit.Before;
      import org.junit.Test;
      import java.io.IOException;
      import java.io.InputStream;
      import java.util.Date;
      import java.util.List;
      public class mybatisTest {
          private UserMapper mapper;
          @Before
          public void beforeTest() throws IOException {
              InputStream resourceAsStream = Resources.getResourceAsStream("sqlMapConfig.xml");
              SqlSessionFactory sqlSessionFactory = new SqlSessionFactoryBuilder().build(resourceAsStream);
              SqlSession sqlSession = sqlSessionFactory.openSession();
              mapper = sqlSession.getMapper(UserMapper.class);
          }
          @Test
          public void findAllTest() throws IOException {
              List<User> userList = mapper.findAll();
              for (User user : userList) {
                  System.out.println(user);
              }
          }
          @Test
          public void findByIdTest2() throws IOException {
              User byId = mapper.findById(1);
              System.out.println(byId);
          }
          @Test
          public void addItemTest3() throws IOException {
              User user = new User();
              user.setUsername("dj");
              user.setPassword("123456");
              user.setBirthday(new Date());
              mapper.addItem(user);
          }
          @Test
          public void updateItemTest4() throws IOException {
              User user = new User();
              user.setId(7);
              user.setUsername("dj2");
              user.setPassword("666");
              user.setBirthday(new Date());
              mapper.updateItem(user);
          }
          @Test
          public void deleteItemTest4() throws IOException {
              mapper.deleteItem(7);
          }
      }
    ```

## MyBatis的注解实现复杂映射开发
  * 实现复杂关系映射之前我们可以在映射文件中通过配置`<resultMap>`来实现，使用注解开发后，我们可以使用@Results注解，@Result注解，@One注解，@Many注解组合完成复杂关系的配置 ![注解](/images/mybatis/注解.jpg) ![注解](/images/mybatis/注解2.jpg)

## 一对一查询
### 一对一查询的模型
  * 用户表和订单表的关系为，一个用户有多个订单，一个订单只从属于一个用户
  * 一对一查询的需求：查询一个订单，与此同时查询出该订单所属的用户。 ![多表操作](/images/mybatis/多表操作.jpg)

### 一对一查询的语句
  * 对应的sql语句：SELECT o.ordertime,o.total,o.id oid,u.id uid,u.username,u.`password`,u.birthday FROM `order` o, `user` u WHERE o.uid = u.id;
  * 查询的结果如下：

### 创建Order和User实体
  * 在domain里，新建Order.java实体类  
    ```
      package com.itheima.domain;
      import java.util.Date;
      public class Order {
          private int id;
          private Date ordertime;
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
              return ordertime;
          }
          public void setOrderTime(Date ordertime) {
              this.ordertime = ordertime;
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
                      ", ordertime=" + ordertime +
                      ", total=" + total +
                      ", user=" + user +
                      '}';
          }
      }
    ```
  * 在test数据库中新建orders表，添加id、ordertime、total、uid字段。![order表](/images/mybatis/多表操作2.jpg)

### 创建OrderMapper接口
  * 在mapper下新建OrderMapper.java的接口，添加findAll方法,并添加注解
    ```
      package com.itheima.mapper;
      import com.itheima.domain.Order;
      import org.apache.ibatis.annotations.Result;
      import org.apache.ibatis.annotations.Results;
      import org.apache.ibatis.annotations.Select;
      import java.util.List;
      public interface OrderMapper {
          @Select("SELECT o.ordertime,o.total,o.id oid,u.id uid,u.username,u.`password`,u.birthday FROM `order` o, `user` u WHERE o.uid = u.id;")
          @Results({
                  @Result(column = "oid", property = "id"),
                  @Result(column = "ordertime", property = "ordertime"),
                  @Result(column = "total", property = "total"),
                  @Result(column = "uid", property = "user.id"),
                  @Result(column = "username", property = "user.username"),
                  @Result(column = "password", property = "user.password"),
          })
          List<Order> findAll();
      }
    ```

  * 写法二：使用@One注解, 需要事先UserMapper.java中有findById方法，没有就新建一个
    ```
      package com.itheima.mapper;
      import com.itheima.domain.Order;
      import com.itheima.domain.User;
      import org.apache.ibatis.annotations.One;
      import org.apache.ibatis.annotations.Result;
      import org.apache.ibatis.annotations.Results;
      import org.apache.ibatis.annotations.Select;
      import java.util.List;
      public interface OrderMapper {
          @Select("select * from orders")
          @Results({
                  @Result(column = "id", property = "id"),
                  @Result(column = "ordertime", property = "ordertime"),
                  @Result(column = "total", property = "total"),
                  @Result(
                          column = "uid", // 根据哪个字段去查询user表的数据
                          property = "user", // 要封装的属性名称
                          javaType = User.class, // 要封装的实体类型
                          // select属性 代表查询哪个接口的方法获取数据
                          one = @One(select = "com.itheima.mapper.UserMapper.findById")
                  )
          })
          List<Order> findAll();
      }
    ```

### 编写测试方法
  ```
    package com.itheima.test;
    import com.itheima.domain.Order;
    import com.itheima.mapper.OrderMapper;
    import org.apache.ibatis.io.Resources;
    import org.apache.ibatis.session.SqlSession;
    import org.apache.ibatis.session.SqlSessionFactory;
    import org.apache.ibatis.session.SqlSessionFactoryBuilder;
    import org.junit.Before;
    import org.junit.Test;
    import java.io.IOException;
    import java.io.InputStream;
    import java.util.List;
    public class mybatisTest2 {
        private OrderMapper mapper;
        @Before
        public void beforeTest() throws IOException {
            InputStream resourceAsStream = Resources.getResourceAsStream("sqlMapConfig.xml");
            SqlSessionFactory sqlSessionFactory = new SqlSessionFactoryBuilder().build(resourceAsStream);
            SqlSession sqlSession = sqlSessionFactory.openSession();
            mapper = sqlSession.getMapper(OrderMapper.class);
        }
        @Test
        public void findAllTest() throws IOException {
            List<Order> orderList = mapper.findAll();
            for (Order order : orderList) {
                System.out.println(order);
            }
        }
    }
  ```
  
## 一对多查询
### 一对多查询
  * 用户表和订单表的关系为，一个用户有多个订单，一个订单只从属于一个用户
  * 一对多查询的需求：查询一个用户，与此同时查询出该用户具有的订单 ![一对多](/images/mybatis/多表操作3.jpg)

### 一对多查询的语句
  * 对应的sql语句：SELECT o.ordertime,o.total,o.id oid,u.id uid,u.username,u.`password`,u.birthday FROM `order` o, `user` u WHERE o.uid = u.id; ![一对多](/images/mybatis/多表操作4.jpg)

### 修改domain下的User实体
  ```
    package com.itheima.domain;
    import java.util.Date;
    import java.util.List;
    public class User {
        private int id;
        private String username;
        private String password;
        private Date birthday;
        // 当前用户存在哪些订单
        private List<Order> orderList;
        @Override
        public String toString() {
            return "User{" +
                    "id=" + id +
                    ", username='" + username + '\'' +
                    ", password='" + password + '\'' +
                    ", birthday=" + birthday +
                    ", orderList=" + orderList +
                    '}';
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
        public Date getBirthday() {
            return birthday;
        }
        public void setBirthday(Date birthday) {
            this.birthday = birthday;
        }
        public List<Order> getOrderList() {
            return orderList;
        }
        public void setOrderList(List<Order> orderList) {
            this.orderList = orderList;
        }
    }
  ```

### 在UserMapper接口中添加findUserAndOrderAll方法
  * 在OrderMapper.java中，添加fingById方法
    ```
      @Select("select * from orders where uid = #{uid}")
      List<Order> findById(int uid);
    ```
  * 在UserMapper接口中添加findUserAndOrderAll方法,并添加注解
    ```
      package com.itheima.mapper;
      import com.itheima.domain.User;
      import org.apache.ibatis.annotations.*;
      import java.util.List;
      public interface UserMapper {
        @Results({
          @Result(id = true, column = "id", property = "id"),
          @Result(column = "username", property = "username"),
          @Result(column = "password", property = "password"),
          @Result(column = "birthday", property = "birthday"),
          @Result(
            column = "id",
            property = "orderList",
            javaType = List.class,
            many = @Many(select = "com.itheima.mapper.OrderMapper.findById")
          )
        })
        List<User> findUserAndOrderAll();
      }
    ```

### 编写测试方法
  ```
    package com.itheima.test;
    import com.itheima.domain.User;
    import com.itheima.mapper.UserMapper;
    import org.apache.ibatis.io.Resources;
    import org.apache.ibatis.session.SqlSession;
    import org.apache.ibatis.session.SqlSessionFactory;
    import org.apache.ibatis.session.SqlSessionFactoryBuilder;
    import org.junit.Before;
    import org.junit.Test;
    import java.io.IOException;
    import java.io.InputStream;
    import java.util.List;
    public class mybatisTest3 {
        private UserMapper mapper;
        @Before
        public void beforeTest() throws IOException {
            InputStream resourceAsStream = Resources.getResourceAsStream("sqlMapConfig.xml");
            SqlSessionFactory sqlSessionFactory = new SqlSessionFactoryBuilder().build(resourceAsStream);
            SqlSession sqlSession = sqlSessionFactory.openSession();
            mapper = sqlSession.getMapper(UserMapper.class);
        }
        @Test
        public void findAllTest() throws IOException {
            List<User> userList = mapper.findUserAndOrderAll();
            for (User user : userList) {
                System.out.println(user);
            }
        }
    }
  ```

## 多对多查询的模型
  * 用户表和角色表的关系为，一个用户有多个角色，一个角色被多个用户使用
  * 多对多查询的需求：查询用户同时查询出该用户的所有角色 ![多对多](/images/mybatis/多表操作5.jpg)

### 多对多查询的语句
  * 对应的sql语句：select u.id userId,r.id roleId,u.username,u.`password`,u.birthday,r.roleName,r.roleDesc from `user` u, sys_user_role ur, sys_role r WHERE u.id = ur.userId AND ur.roleId = r.id;
  * 查询的结果如下: ![多对多](/images/mybatis/多表操作5.jpg)

### 创建sys_role,sys_user_role表
  ```
    -- 创建sys_user_role表
    DROP TABLE IF EXISTS `sys_user_role`;
    CREATE TABLE `sys_user_role` (
      `userId` bigint(20) NOT NULL,
      `roleId` bigint(20) NOT NULL,
      PRIMARY KEY (`userId`,`roleId`),
      KEY `roleId` (`roleId`),
      CONSTRAINT `sys_user_role_ibfk_1` FOREIGN KEY (`userId`) REFERENCES `sys_user` (`id`),
      CONSTRAINT `sys_user_role_ibfk_2` FOREIGN KEY (`roleId`) REFERENCES `sys_role` (`id`)
    ) ENGINE=InnoDB DEFAULT CHARSET=utf8;

    INSERT INTO `sys_user_role` VALUES ('1', '1');
    INSERT INTO `sys_user_role` VALUES ('1', '2');
    INSERT INTO `sys_user_role` VALUES ('2', '2');
    INSERT INTO `sys_user_role` VALUES ('2', '3');

    -- 创建sys_role表
    DROP TABLE IF EXISTS `sys_role`;
    CREATE TABLE `sys_role` (
      `id` bigint(20) NOT NULL AUTO_INCREMENT,
      `roleName` varchar(50) DEFAULT NULL,
      `roleDesc` varchar(50) DEFAULT NULL,
      PRIMARY KEY (`id`)
    ) ENGINE=InnoDB AUTO_INCREMENT=15 DEFAULT CHARSET=utf8;

    -- ----------------------------
    -- Records of sys_role
    -- ----------------------------
    INSERT INTO `sys_role` VALUES ('1', '院长', '负责全面工作');
    INSERT INTO `sys_role` VALUES ('2', '研究员', '课程研发工作');
    INSERT INTO `sys_role` VALUES ('3', '讲师', '授课工作');
    INSERT INTO `sys_role` VALUES ('4', '助教', '协助解决学生的问题');
  ```

### 创建Role实体类
  * 在domain下新建Role.java实体类
    ```
      package com.itheima.domain;
      public class Role {
          private int id;
          private String roleName;
          private String roleDesc;
          @Override
          public String toString() {
              return "Role{" +
                      "id=" + id +
                      ", roleName='" + roleName + '\'' +
                      ", roleDesc='" + roleDesc + '\'' +
                      '}';
          }
          public int getId() {
              return id;
          }
          public void setId(int id) {
              this.id = id;
          }
          public String getRoleName() {
              return roleName;
          }
          public void setRoleName(String roleName) {
              this.roleName = roleName;
          }
          public String getRoleDesc() {
              return roleDesc;
          }
          public void setRoleDesc(String roleDesc) {
              this.roleDesc = roleDesc;
          }
      }
    ```

### 修改User实体类
  * 在domain/User.java中，添加角色列表
    ```
      package com.itheima.domain;
      import java.util.Date;
      import java.util.List;
      public class User {
          private int id;
          private String username;
          private String password;
          private Date birthday;
          // 当前用户具备哪些角色
          private List<Role> roleList;
          @Override
          public String toString() {
              return "User{" +
                      "id=" + id +
                      ", username='" + username + '\'' +
                      ", password='" + password + '\'' +
                      ", birthday=" + birthday +
                      ", roleList=" + roleList +
                      '}';
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
          public Date getBirthday() {
              return birthday;
          }
          public void setBirthday(Date birthday) {
              this.birthday = birthday;
          }
          public List<Role> getRoleList() {
              return roleList;
          }
          public void setRoleList(List<Role> roleList) {
              this.roleList = roleList;
          }
      }
    ```

### 在UserMapper接口中添加findUserAndRoleAll方法
  * 在com.itheima.mapper下新建RoleMapper接口，并添加findListById方法
    ```
      package com.itheima.mapper;
      import com.itheima.domain.Role;
      import org.apache.ibatis.annotations.Select;
      import java.util.List;
      public interface RoleMapper {
          @Select("SELECT * from sys_user_role ur, sys_role r WHERE ur.roleId = r.id AND ur.userId = #{uid}")
          List<Role> findListById(int uid);
      }
    ```
  * 在UserMapper接口中添加findUserAndRoleAll方法
    ```
      package com.itheima.mapper;
      import com.itheima.domain.User;
      import org.apache.ibatis.annotations.*;
      import java.util.List;
      public interface UserMapper {
          @Select("select * from user")
          @Results({
                  @Result(id = true, column = "id", property = "id"),
                  @Result(column = "username", property = "username"),
                  @Result(column = "password", property = "password"),
                  @Result(column = "birthday", property = "birthday"),
                  @Result(
                          column = "id",
                          property = "roleList",
                          javaType = List.class,
                          many = @Many(select = "com.itheima.mapper.RoleMapper.findListById")
                  )
          })
          List<User> findUserAndRoleAll();
      }
    ```

### 编写测试方法
  ```
    package com.itheima.test;
    import com.itheima.domain.User;
    import com.itheima.mapper.UserMapper;
    import org.apache.ibatis.io.Resources;
    import org.apache.ibatis.session.SqlSession;
    import org.apache.ibatis.session.SqlSessionFactory;
    import org.apache.ibatis.session.SqlSessionFactoryBuilder;
    import org.junit.Before;
    import org.junit.Test;
    import java.io.IOException;
    import java.io.InputStream;
    import java.util.List;
    public class mybatisTest4 {
        private UserMapper mapper;
        @Before
        public void beforeTest() throws IOException {
            InputStream resourceAsStream = Resources.getResourceAsStream("sqlMapConfig.xml");
            SqlSessionFactory sqlSessionFactory = new SqlSessionFactoryBuilder().build(resourceAsStream);
            SqlSession sqlSession = sqlSessionFactory.openSession();
            mapper = sqlSession.getMapper(UserMapper.class);
        }
        @Test
        public void findAllTest() throws IOException {
            List<User> userList = mapper.findUserAndRoleAll();
            for (User user : userList) {
                System.out.println(user);
            }
        }
    }
  ```









