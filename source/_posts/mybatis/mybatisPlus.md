---
title: mybatisPlus
date: 2021-07-26 16:32:03
tags:
  - mybatis-plus
  - mybatis
  - java
type: java                                                                         # 标签、分类和友情链接三個页面需要配置(必填)
description: MyBatis 是一款优秀的持久层框架，它支持自定义 SQL、存储过程以及高级映射。    # 描述
keywords: mybatis                  # 关键词，便于搜索
top_img: /images/mybatis/plus.jpg             # 文章的顶部图片
aside: true                                                                         # 展示文章侧边栏(默认为true)
categories: 
  - 教程
  - mybatis-plus
  - mybatis                                                              # 文章标签
  - java
cover: /images/mybatis/plus.jpg                 # 文章的缩略图（用在首页）
---

# 了解Mybatis-Plus
## Mybatis-Plus介绍
  * MyBatis-Plus（简称 MP）是一个 MyBatis 的增强工具，在 MyBatis 的基础上只做增强不做改变，为简化开发、提高效率而生。

## 代码以及文档
  * 文档地址：https://mybatis.plus/guide/
  * 源码地址：https://github.com/baomidou/mybatis-plus

## 特性
  * 无侵入：只做增强不做改变，引入它不会对现有工程产生影响，如丝般顺滑
  * 损耗小：启动即会自动注入基本 CURD，性能基本无损耗，直接面向对象操作
  * 强大的 CRUD 操作：内置通用 Mapper、通用 Service，仅仅通过少量配置即可实现单表大部分 CRUD 操作，更有强大的条件构造器，满足各类使用需求
  * 支持 Lambda 形式调用：通过 Lambda 表达式，方便的编写各类查询条件，无需再担心字段写错
  * 支持多种数据库：支持 MySQL、MariaDB、Oracle、DB2、H2、HSQL、SQLite、Postgre、SQLServer2005、SQLServer 等多种数据库
  * 支持主键自动生成：支持多达 4 种主键策略（内含分布式唯一 ID 生成器 - Sequence），可自由配置，完美解决主键问题
  * 支持 XML 热加载：Mapper 对应的 XML 支持热加载，对于简单的 CRUD 操作，甚至可以无 XML 启动
  * 支持 ActiveRecord 模式：支持 ActiveRecord 形式调用，实体类只需继承 Model 类即可进行强大的 CRUD 操 作
  * 支持自定义全局通用操作：支持全局通用方法注入（ Write once, use anywhere ）
  * 支持关键词自动转义：支持数据库关键词（order、key......）自动转义，还可自定义关键词
  * 内置代码生成器：采用代码或者 Maven 插件可快速生成 Mapper 、 Model 、 Service 、 Controller 层代码，支持模板引擎，更有超多自定义配置等您来使用
  * 内置分页插件：基于 MyBatis 物理分页，开发者无需关心具体操作，配置好插件之后，写分页等同于普通 List查询
  * 内置性能分析插件：可输出 Sql 语句以及其执行时间，建议开发测试时启用该功能，能快速揪出慢查询
  * 内置全局拦截插件：提供全表 delete 、 update 操作智能分析阻断，也可自定义拦截规则，预防误操作
  * 内置 Sql 注入剥离器：支持 Sql 注入剥离，有效预防 Sql 注入攻击

# 快速开始
  * 对于Mybatis整合MP有常常有三种用法，分别是Mybatis+MP、Spring+Mybatis+MP、Spring Boot+Mybatis+MP。
  
## 创建数据库以及表
  ```
    -- 创建测试表 
    CREATE TABLE `tb_user` ( 
      `id` bigint(20) NOT NULL AUTO_INCREMENT COMMENT '主键ID', 
      `user_name` varchar(20) NOT NULL COMMENT '用户名', 
      `password` varchar(20) NOT NULL COMMENT '密码', 
      `name` varchar(30) DEFAULT NULL COMMENT '姓名', 
      `age` int(11) DEFAULT NULL COMMENT '年龄', 
      `email` varchar(50) DEFAULT NULL COMMENT '邮箱', 
      PRIMARY KEY (`id`) 
    ) ENGINE=InnoDB AUTO_INCREMENT=1 DEFAULT CHARSET=utf8;

    -- 插入测试数据 
    INSERT INTO `tb_user` (`id`, `user_name`, `password`, `name`, `age`, `email`) VALUES ('1', 'zhangsan', '123456', '张三', '18', 'test1@itcast.cn'); 
    INSERT INTO `tb_user` (`id`, `user_name`, `password`, `name`, `age`, `email`) VALUES ('2', 'lisi', '123456', '李四', '20', 'test2@itcast.cn'); 
    INSERT INTO `tb_user` (`id`, `user_name`, `password`, `name`, `age`, `email`) VALUES ('3', 'wangwu', '123456', '王五', '28', 'test3@itcast.cn'); 
    INSERT INTO `tb_user` (`id`, `user_name`, `password`, `name`, `age`, `email`) VALUES ('4', 'zhaoliu', '123456', '赵六', '21', 'test4@itcast.cn'); 
    INSERT INTO `tb_user` (`id`, `user_name`, `password`, `name`, `age`, `email`) VALUES ('5', 'sunqi', '123456', '孙七', '24', 'test5@itcast.cn');
  ```

## 创建工程
  1. 新建itheima_mybatis_plus的project
  2. 在pom.xml中导入相应的坐标
    ```
      <!-- mybatis-plus插件依赖 -->
      <dependency>
          <groupId>com.baomidou</groupId>
          <artifactId>mybatis-plus</artifactId>
          <version>3.4.2</version>
      </dependency>
      <!-- MySql -->
      <dependency>
          <groupId>mysql</groupId>
          <artifactId>mysql-connector-java</artifactId>
          <version>5.1.46</version>
      </dependency>
      <!-- 连接池 -->
      <dependency>
          <groupId>com.alibaba</groupId>
          <artifactId>druid</artifactId>
          <version>1.1.10</version>
      </dependency>
      <!--简化bean代码的工具包-->
      <dependency>
          <groupId>org.projectlombok</groupId>
          <artifactId>lombok</artifactId>
          <version>1.18.20</version>
      </dependency>
      <dependency>
          <groupId>junit</groupId>
          <artifactId>junit</artifactId>
          <version>4.12</version>
      </dependency>
      <dependency>
          <groupId>org.slf4j</groupId>
          <artifactId>slf4j-log4j12</artifactId>
          <version>1.7.25</version>
      </dependency>
    ```

## Mybatis + MP
  * 下面演示，通过纯Mybatis与Mybatis-Plus整合。

### 创建子Module
  * 新建itheima_mybatis_plus_01的module, 并设置webapp文件夹
 
### mybatis实现查询User
  1. 在resources下新建log4j.properties文件
  ```
    log4j.rootLogger=DEBUG,A1
    log4j.appender.A1=org.apache.log4j.ConsoleAppender
    log4j.appender.A1.layout=org.apache.log4j.PatternLayout
    log4j.appender.A1.layout.ConversionPattern=[%t] [%c]-[%p] %m%n
  ```
  2. 在resources下新建jdbc.properties
    ```
      jdbc.driver=com.mysql.jdbc.Driver
      jdbc.url=jdbc:mysql://localhost:3306/mp
      jdbc.username=root
      jdbc.password=root
    ```
  3. 在java下新建com.itheima.pojo.User.java实体类
    ```
      package com.itheima.pojo;
      import lombok.AllArgsConstructor;
      import lombok.Data;
      import lombok.NoArgsConstructor;
      @Data
      @AllArgsConstructor
      @NoArgsConstructor
      public class User {
          private int id;
          private String username;
          private String password;
          private String name;
          private Integer age;
          private String email;
      }
    ```
  4. 在java下新建com.itheima.mapper.UserMapper.java的接口
    ```
      package com.itheima.mapper;
      import com.itheima.pojo.User;
      import java.util.List;
      public interface UserMapper {
          List<User> findAll();
      }
    ```
  5. 在resources下新建sqlMapConfig.xml
    ```
      <?xml version="1.0" encoding="UTF-8" ?>
      <!DOCTYPE configuration
              PUBLIC "-//mybatis.org//DTD Config 3.0//EN"
              "http://mybatis.org/dtd/mybatis-3-config.dtd">
      <configuration>
          <!--通过properties标签加载jdbc.properties-->
          <properties resource="jdbc.properties"></properties>
          <!--设置别名-->
          <typeAliases>
              <typeAlias type="com.itheima.pojo.User" alias="user"></typeAlias>
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
          <!--扫描mapper-->
          <mappers>
              <mapper resource="mapper/UserMapper.xml"></mapper>
          </mappers>
      </configuration>
    ```
  6. resources下新建mapper/UserMapper.xml
    ```
      <?xml version="1.0" encoding="UTF-8" ?>
      <!DOCTYPE mapper PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN" "http://mybatis.org/dtd/mybatis-3-mapper.dtd">
      <mapper namespace="com.itheima.mapper.UserMapper">
          <select id="findAll" resultType="user">
              select * from tb_user
          </select>
      </mapper>
    ```
  7. 编写测试方法
    ```
      package com.itheima.test;
      import com.itheima.mapper.UserMapper;
      import com.itheima.pojo.User;
      import org.apache.ibatis.io.Resources;
      import org.apache.ibatis.session.SqlSession;
      import org.apache.ibatis.session.SqlSessionFactory;
      import org.apache.ibatis.session.SqlSessionFactoryBuilder;
      import org.junit.Test;
      import java.io.IOException;
      import java.io.InputStream;
      import java.util.List;
      public class userText {
          @Test
          public void testUserList() throws IOException {
              InputStream resourceAsStream = Resources.getResourceAsStream("sqlMapConfig.xml");
              SqlSessionFactory sqlSessionFactory = new SqlSessionFactoryBuilder().build(resourceAsStream);
              SqlSession sqlSession = sqlSessionFactory.openSession();
              UserMapper mapper = sqlSession.getMapper(UserMapper.class);
              List<User> userList = mapper.findAll();
              for (User user : userList) {
                  System.out.println(user);
              }
          }
      }
    ```
  8. 测试结果
    ```
      [main] [com.itheima.mapper.UserMapper.findAll]-[DEBUG] ==>  Preparing: select * from tb_user
      [main] [com.itheima.mapper.UserMapper.findAll]-[DEBUG] ==> Parameters: 
      [main] [com.itheima.mapper.UserMapper.findAll]-[DEBUG] <==      Total: 5
      User(id=1, username=null, password=123456, name=张三, age=18, email=test1@itcast.cn)
      User(id=2, username=null, password=123456, name=李四, age=20, email=test2@itcast.cn)
      User(id=3, username=null, password=123456, name=王五, age=28, email=test3@itcast.cn)
      User(id=4, username=null, password=123456, name=赵六, age=21, email=test4@itcast.cn)
      User(id=5, username=null, password=123456, name=孙七, age=24, email=test5@itcast.cn)
    ```

## Mybatis+MP实现查询User
1. 在main/java/com/itheima/mapper/UserMapper.java中，将UserMapper继承BaseMapper，将拥有了BaseMapper中的所有方法
  ```
    package com.itheima.mapper;
    import com.baomidou.mybatisplus.core.mapper.BaseMapper;
    import com.itheima.pojo.User;
    import java.util.List;
    public interface UserMapper extends BaseMapper<User> {
        List<User> findAll();
    }
  ```
2. 编写测试方法，使用MP中的MybatisSqlSessionFactoryBuilder进程构建
  ```
    package com.itheima.test;
    import com.baomidou.mybatisplus.core.MybatisSqlSessionFactoryBuilder;
    import com.itheima.mapper.UserMapper;
    import com.itheima.pojo.User;
    import org.apache.ibatis.io.Resources;
    import org.apache.ibatis.session.SqlSession;
    import org.apache.ibatis.session.SqlSessionFactory;
    import org.junit.Test;
    import java.io.IOException;
    import java.io.InputStream;
    import java.util.List;
    public class userText2 {
        @Test
        public void testUserList() throws IOException {
            InputStream resourceAsStream = Resources.getResourceAsStream("sqlMapConfig.xml");
            SqlSessionFactory sqlSessionFactory = new MybatisSqlSessionFactoryBuilder().build(resourceAsStream);
            SqlSession sqlSession = sqlSessionFactory.openSession();
            UserMapper mapper = sqlSession.getMapper(UserMapper.class);
            // 使用mybatis-plus的查询方法
            List<User> userList = mapper.selectList(null);
            for (User user : userList) {
                System.out.println(user);
            }
        }
    }
  ```
3. 找到main/java/com/itheima/pojo/User.java实体类，添加@TableName注解
  ```
    package com.itheima.pojo;
    import com.baomidou.mybatisplus.annotation.TableName;
    import lombok.AllArgsConstructor;
    import lombok.Data;
    import lombok.NoArgsConstructor;
    @Data
    @AllArgsConstructor
    @NoArgsConstructor
    @TableName("tb_user") // 指定表名
    public class User {
        private int id;
        private String userName;
        private String password;
        private String name;
        private Integer age;
        private String email;
    }
  ```
4. 启动测试,如果没有第三步就会报错
  ```
    [main] [com.itheima.mapper.UserMapper.selectList]-[DEBUG] ==> Parameters: 
    [main] [com.itheima.mapper.UserMapper.selectList]-[DEBUG] <==      Total: 5
    User(id=1, userName=zhangsan, password=123456, name=张三, age=18, email=test1@itcast.cn)
    User(id=2, userName=lisi, password=123456, name=李四, age=20, email=test2@itcast.cn)
    User(id=3, userName=wangwu, password=123456, name=王五, age=28, email=test3@itcast.cn)
    User(id=4, userName=zhaoliu, password=123456, name=赵六, age=21, email=test4@itcast.cn)
    User(id=5, userName=sunqi, password=123456, name=孙七, age=24, email=test5@itcast.cn)
  ```

## Spring + Mybatis + MP
引入了Spring框架，数据源、构建等工作就交给了Spring管理。

### 创建子Module
1. 新建itheima_mybatis_plus_003的module，并配置webapp文件夹
2. 在pom.xml中添加相应的坐标
  ```
    <?xml version="1.0" encoding="UTF-8"?>
    <project xmlns="http://maven.apache.org/POM/4.0.0"
            xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
            xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
        <parent>
            <artifactId>itheima_mybatis_plus</artifactId>
            <groupId>com.itheima</groupId>
            <version>1.0-SNAPSHOT</version>
        </parent>
        <modelVersion>4.0.0</modelVersion>
        <artifactId>itheima_mybatis_plus_003</artifactId>
        <properties>
            <spring.version>5.3.6</spring.version>
        </properties>
        <dependencies>
            <dependency>
                <groupId>org.springframework</groupId>
                <artifactId>spring-test</artifactId>
                <version>${spring.version}</version>
            </dependency>
            <dependency>
                <groupId>org.springframework</groupId>
                <artifactId>spring-jdbc</artifactId>
                <version>${spring.version}</version>
            </dependency>
            <dependency>
                <groupId>org.springframework</groupId>
                <artifactId>spring-webmvc</artifactId>
                <version>${spring.version}</version>
            </dependency>
        </dependencies>
    </project>
  ```

### 实现查询User
1. 在resources下，新建jdbc.properties
  ```
    jdbc.driver=com.mysql.jdbc.Driver
    jdbc.url=jdbc:mysql://127.0.0.1:3306/mp
    jdbc.username=root
    jdbc.password=root
  ```
2. 在resources下，新建applicationContext.xml
  ```
    <?xml version="1.0" encoding="UTF-8"?>
    <beans xmlns="http://www.springframework.org/schema/beans"
          xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
          xmlns:context="http://www.springframework.org/schema/context"
          xsi:schemaLocation="
          http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd
          http://www.springframework.org/schema/context http://www.springframework.org/schema/context/spring-context.xsd
    ">
        <!--引入jdbc.properties-->
        <context:property-placeholder location="jdbc.properties" />
        <!--定义数据源-->
        <bean id="druidDataSource" class="com.alibaba.druid.pool.DruidDataSource" destroy-method="clone">
            <property name="driverClassName" value="${jdbc.driver}" />
            <property name="url" value="${jdbc.url}" />
            <property name="username" value="${jdbc.username}" />
            <property name="password" value="${jdbc.password}" />
        </bean>
        <!--这里使用MP提供的sqlSessionFactory，完成了Spring与MP的整合-->
        <bean id="sessionFactory" class="com.baomidou.mybatisplus.extension.spring.MybatisSqlSessionFactoryBean">
            <property name="dataSource" ref="druidDataSource" />
        </bean>
        <!--扫描mapper接口，使用的依然是Mybatis原生的扫描器-->
        <bean class="org.mybatis.spring.mapper.MapperScannerConfigurer">
            <property name="basePackage" value="com.itheima.mapper" />
        </bean>
    </beans>
  ```
3. 在main/java下新建com.itheima.pojo.User实例
  ```
    package com.itheima.pojo;
    import com.baomidou.mybatisplus.annotation.TableName;
    import lombok.AllArgsConstructor;
    import lombok.Data;
    import lombok.NoArgsConstructor;
    @Data
    @AllArgsConstructor
    @NoArgsConstructor
    @TableName("tb_user")
    public class User {
        private int id;
        private String userName;
        private String password;
        private String name;
        private Integer age;
        private String email;
    }
  ```
4. 在java下新建com.itheima.mapper.UserMapper.java接口,继承MybatisPlus的BaseMapper
  ```
    package com.itheima.mapper;
    import com.baomidou.mybatisplus.core.mapper.BaseMapper;
    import com.itheima.pojo.User;
    public interface UserMapper extends BaseMapper<User> {
    }
  ```
5. 编写测试方法
  ```
    package com.itheima.test;
    import com.itheima.mapper.UserMapper;
    import com.itheima.pojo.User;
    import org.junit.Test;
    import org.junit.runner.RunWith;
    import org.springframework.beans.factory.annotation.Autowired;
    import org.springframework.test.context.ContextConfiguration;
    import org.springframework.test.context.junit4.SpringJUnit4ClassRunner;
    import java.util.List;
    @RunWith(SpringJUnit4ClassRunner.class)
    @ContextConfiguration(locations = "classpath:applicationContext.xml")
    public class userTest {
        @Autowired
        private UserMapper userMapper;
        @Test
        public void testList() {
            List<User> userList = userMapper.selectList(null);
            for (User user : userList) {
                System.out.println(user);
            }
        }
    }
  ```
6. 测试结果
  ```
    User(id=1, userName=zhangsan, password=123456, name=张三, age=18, email=test1@itcast.cn)
    User(id=2, userName=lisi, password=123456, name=李四, age=20, email=test2@itcast.cn)
    User(id=3, userName=wangwu, password=123456, name=王五, age=28, email=test3@itcast.cn)
    User(id=4, userName=zhaoliu, password=123456, name=赵六, age=21, email=test4@itcast.cn)
    User(id=5, userName=sunqi, password=123456, name=孙七, age=24, email=test5@itcast.cn)
  ```

## SpringBoot + Mybatis + MP
  * 使用SpringBoot将进一步的简化MP的整合，需要注意的是，由于使用SpringBoot需要继承parent，所以需要重新创
建工程，并不是创建子Module。

### 创建工程
  1. 新建一个itheima_mp_springboot的project
  2. 在pom.xml中导入相应的坐标
    ```
      <?xml version="1.0" encoding="UTF-8"?>
      <project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
        xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 https://maven.apache.org/xsd/maven-4.0.0.xsd">
        <modelVersion>4.0.0</modelVersion>
        <parent>
          <groupId>org.springframework.boot</groupId>
          <artifactId>spring-boot-starter-parent</artifactId>
          <version>2.5.3</version>
          <relativePath/> <!-- lookup parent from repository -->
        </parent>
        <groupId>com.itheima</groupId>
        <artifactId>itheima_my_boot</artifactId>
        <version>0.0.1-SNAPSHOT</version>
        <name>itheima_my_boot</name>
        <description>Demo project for Spring Boot</description>
        <properties>
          <java.version>1.8</java.version>
        </properties>
        <dependencies>
          <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter</artifactId>
          </dependency>
          <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-test</artifactId>
            <scope>test</scope>
          </dependency>
          <dependency>
            <groupId>junit</groupId>
            <artifactId>junit</artifactId>
            <scope>test</scope>
          </dependency>
          <dependency>
            <groupId>org.projectlombok</groupId>
            <artifactId>lombok</artifactId>
            <optional>true</optional>
          </dependency>
          <dependency>
            <groupId>com.baomidou</groupId>
            <artifactId>mybatis-plus-boot-starter</artifactId>
            <version>3.4.2</version>
          </dependency>
          <dependency>
            <groupId>mysql</groupId>
            <artifactId>mysql-connector-java</artifactId>
            <version>5.1.46</version>
          </dependency>
          <dependency>
            <groupId>org.slf4j</groupId>
            <artifactId>slf4j-log4j12</artifactId>
          </dependency>
        </dependencies>
        <build>
          <plugins>
            <plugin>
              <groupId>org.springframework.boot</groupId>
              <artifactId>spring-boot-maven-plugin</artifactId>
            </plugin>
          </plugins>
        </build>
      </project>
    ```
### 编写log4j.properties
  * 在resources下新建log4j.properties
    ```
      log4j.rootLogger=DEBUG,A1
      log4j.appender.A1=org.apache.log4j.ConsoleAppender
      log4j.appender.A1.layout=org.apache.log4j.PatternLayout
      log4j.appender.A1.layout.ConversionPattern=[%t] [%c]-[%p] %m%n
    ```

### 编写application.properties
  * 在resources下修改application.properties
    ```
      spring.datasource.driver-class-name=com.mysql.jdbc.Driver
      spring.datasource.url=jdbc:mysql://127.0.0.1:3306/mp
      spring.datasource.username=root
      spring.datasource.password=root
    ```

### 编写pojo
  * 在java下新建com.itheima.pojo.User.java实体类
    ```
      package com.itheima.pojo;
      import com.baomidou.mybatisplus.annotation.TableName;
      import lombok.AllArgsConstructor;
      import lombok.Data;
      import lombok.NoArgsConstructor;
      @Data
      @NoArgsConstructor
      @AllArgsConstructor
      @TableName("tb_user")
      public class User {
          private Long id;
          private String userName;
          private String password;
          private String name;
          private Integer age;
          private String email;
      }
    ```

### 编写mapper
  * 在java下新建com.itheima.mapper.UserMapper.java，继承BaseMapper
    ```
      package com.itheima.mapper;
      import com.baomidou.mybatisplus.core.mapper.BaseMapper;
      import com.itheima.pojo.User;
      public interface UserMapper extends BaseMapper<User> {
      }
    ```

### 修改启动类
  * 找到java.com.itheima下的ItHeimaMyBootApplication.java启动类，添加@MapperScan注解
    ```
      package com.itheima;
      import org.mybatis.spring.annotation.MapperScan;
      import org.springframework.boot.SpringApplication;
      import org.springframework.boot.autoconfigure.SpringBootApplication;
      @MapperScan("com.itheima.mapper")
      @SpringBootApplication
      public class ItheimaMyBootApplication {
        public static void main(String[] args) {
          SpringApplication.run(ItheimaMyBootApplication.class, args);
        }
      }
    ```

### 编写测试用例
  * 在test文件夹下新建java.com.itheima.myTest.java
    ```
      package com.itheima;
      import com.itheima.mapper.UserMapper;
      import com.itheima.pojo.User;
      import org.junit.Test;
      import org.junit.runner.RunWith;
      import org.springframework.beans.factory.annotation.Autowired;
      import org.springframework.boot.test.context.SpringBootTest;
      import org.springframework.test.context.junit4.SpringRunner;
      import java.util.List;
      @RunWith(SpringRunner.class)
      @SpringBootTest
      public class myTest {
          @Autowired
          private UserMapper userMapper;
          @Test
          public void listTest() {
              List<User> userList = userMapper.selectList(null);
              for (User user : userList) {
                  System.out.println(user);
              }
          }
      }
    ```

### 测试
  ```
    User(id=1, userName=zhangsan, password=123456, name=张三, age=18, email=test1@itcast.cn)
    User(id=2, userName=lisi, password=123456, name=李四, age=20, email=test2@itcast.cn)
    User(id=3, userName=wangwu, password=123456, name=王五, age=28, email=test3@itcast.cn)
    User(id=4, userName=zhaoliu, password=123456, name=赵六, age=21, email=test4@itcast.cn)
    User(id=5, userName=sunqi, password=123456, name=孙七, age=24, email=test5@itcast.cn)
  ```

# 通用的CRUD
## 插入操作
### 方法定义
  ```
    /**
    * 插入一条记录
    *
    * @param entity 实体对象
    */
    int insert(T entity);
  ```

### 测试用例
  ```
    package com.itheima;
    import com.itheima.mapper.UserMapper;
    import com.itheima.pojo.User;
    import org.junit.Test;
    import org.junit.runner.RunWith;
    import org.springframework.beans.factory.annotation.Autowired;
    import org.springframework.boot.test.context.SpringBootTest;
    import org.springframework.test.context.junit4.SpringRunner;
    import java.util.List;
    @RunWith(SpringRunner.class)
    @SpringBootTest
    public class insertTest {
        @Autowired
        private UserMapper mapper;
        @Test
        public void listTest() {
            User user = new User();
            user.setAge(18);
            user.setEmail("821084785@qq.com");
            user.setName("王梅");
            user.setUserName("wangmei");
            user.setPassword("123");
            // 返回的result是受影响的行数，并不是自增后的id
            int count = mapper.insert(user);
            System.out.println("result:" + count);
            // 自增后的id会回填到对象中
            System.out.println(user.getId());
        }
    }
  ```
  * 测试结果：发现id没有自增长。![测试结果](/images/mybatis/plus2.jpg)

### 修改User对象
  * 修改pojo里面的User实体类，添加@TableId注解,设置id自增长
    ```
      package com.itheima.pojo;
      import com.baomidou.mybatisplus.annotation.IdType;
      import com.baomidou.mybatisplus.annotation.TableId;
      import com.baomidou.mybatisplus.annotation.TableName;
      import lombok.AllArgsConstructor;
      import lombok.Data;
      import lombok.NoArgsConstructor;
      @Data
      @NoArgsConstructor
      @AllArgsConstructor
      @TableName("tb_user")
      public class User {
          @TableId(type = IdType.AUTO)
          private Long id;
          private String userName;
          private String password;
          private String name;
          private Integer age;
          private String email;
      }
    ```

### @TableField
  * 在MP中通过@TableField注解可以指定字段的一些属性，常常解决的问题有2个：
    1. 对象中的属性名和字段名不一致的问题（非驼峰）
    2. 对象中的属性字段在表中不存在的问题
      ```
        package com.itheima.pojo;
        import com.baomidou.mybatisplus.annotation.IdType;
        import com.baomidou.mybatisplus.annotation.TableField;
        import com.baomidou.mybatisplus.annotation.TableId;
        import com.baomidou.mybatisplus.annotation.TableName;
        import lombok.AllArgsConstructor;
        import lombok.Data;
        import lombok.NoArgsConstructor;
        @Data
        @NoArgsConstructor
        @AllArgsConstructor
        @TableName("tb_user")
        public class User {
            @TableId(type = IdType.AUTO)
            private Long id;
            @TableField(select = false) // 不加入查询字段
            private String userName;
            private String password;
            private String name;
            private Integer age;
            @TableField(value = "email") // 解决字段名不一致
            private String mail;
            @TableField(exist = false) // 该字段在数据表中不存在
            private String address;
        }
      ```

## 更新操作
  * 在MP中，更新操作有2种，一种是根据id更新，另一种是根据条件更新。

### 根据id更新
  1. 方法定义：
    ```
      /**
      * 根据 ID 修改
      *
      * @param entity 实体对象
      */
      int updateById(@Param(Constants.ENTITY) T entity);
    ```
  2. 测试：
    ```
      package com.itheima;
      import com.itheima.mapper.UserMapper;
      import com.itheima.pojo.User;
      import org.junit.Test;
      import org.junit.runner.RunWith;
      import org.springframework.beans.factory.annotation.Autowired;
      import org.springframework.boot.test.context.SpringBootTest;
      import org.springframework.test.context.junit4.SpringRunner;
      @RunWith(SpringRunner.class)
      @SpringBootTest
      public class updateTest {
          @Autowired
          private UserMapper mapper;
          @Test
          public void listTest() {
              User user = new User();
              user.setId(6L);
              user.setName("王麻子");
              user.setUserName("wangmazi");
              //根据id更新，更新不为null的字段
              mapper.updateById(user);
          }
      }
    ```

### 根据条件更新
  1. 方法定义：
    ```
      /**
      * 根据 whereEntity 条件，更新记录
      *
      * @param entity 实体对象 (set 条件值,可以为 null)
      * @param updateWrapper 实体对象封装操作类（可以为 null,里面的 entity 用于生成 where 语句）
      */
      int update(@Param(Constants.ENTITY) T entity, @Param(Constants.WRAPPER) Wrapper<T>
      updateWrapper);
    ```
  2. 测试用例：
    ```
      package com.itheima;
      import com.baomidou.mybatisplus.core.conditions.query.QueryWrapper;
      import com.itheima.mapper.UserMapper;
      import com.itheima.pojo.User;
      import org.junit.Test;
      import org.junit.runner.RunWith;
      import org.springframework.beans.factory.annotation.Autowired;
      import org.springframework.boot.test.context.SpringBootTest;
      import org.springframework.test.context.junit4.SpringRunner;
      @RunWith(SpringRunner.class)
      @SpringBootTest
      public class updateTest2 {
          @Autowired
          private UserMapper mapper;
          @Test
          public void listTest() {
              User user = new User();
              user.setAge(22); //更新的字段
              // 更新的条件
              QueryWrapper<User> wrapper = new QueryWrapper<>();
              wrapper.eq("id", 7);
              // 执行更新操作
              int result = mapper.update(user, wrapper);
              System.out.println("result:" + result);
          }
      }
    ```

  3. 或者，通过UpdateWrapper进行更新
    ```
      package com.itheima;
      import com.baomidou.mybatisplus.core.conditions.update.UpdateWrapper;
      import com.itheima.mapper.UserMapper;
      import com.itheima.pojo.User;
      import org.junit.Test;
      import org.junit.runner.RunWith;
      import org.springframework.beans.factory.annotation.Autowired;
      import org.springframework.boot.test.context.SpringBootTest;
      import org.springframework.test.context.junit4.SpringRunner;
      @RunWith(SpringRunner.class)
      @SpringBootTest
      public class updateTest3 {
          @Autowired
          private UserMapper mapper;
          @Test
          public void listTest() {
              User user = new User();
              //更新的条件以及字段
              UpdateWrapper<User> wrapper = new UpdateWrapper<>();
              wrapper.eq("id", 6).set("age", 23);
              //执行更新操作
              int result = mapper.update(null, wrapper);
              System.out.println("result = " + result);
          }
      }
    ```

## 删除操作
### deleteById
  1. 方法定义：
    ```
      /**
      * 根据 ID 删除
      *
      * @param id 主键ID
      */
      int deleteById(Serializable id);
    ```
  2. 测试用例：
    ```
      package com.itheima;
      import com.itheima.mapper.UserMapper;
      import org.junit.Test;
      import org.junit.runner.RunWith;
      import org.springframework.beans.factory.annotation.Autowired;
      import org.springframework.boot.test.context.SpringBootTest;
      import org.springframework.test.context.junit4.SpringRunner;
      @RunWith(SpringRunner.class)
      @SpringBootTest
      public class deleteTest {
          @Autowired
          private UserMapper mapper;
          @Test
          public void listTest() {
              int result = mapper.deleteById(4L);
              System.out.println("result:" + result);
          }
      }
    ```

### deleteByMap
  1. 方法定义
    ```
      /**
      * 根据 columnMap 条件，删除记录
      *
      * @param columnMap 表字段 map 对象
      */
      int deleteByMap(@Param(Constants.COLUMN_MAP) Map<String, Object> columnMap);
    ```
  2. 测试用例：
    ```
      package com.itheima;
      import com.itheima.mapper.UserMapper;
      import org.junit.Test;
      import org.junit.runner.RunWith;
      import org.springframework.beans.factory.annotation.Autowired;
      import org.springframework.boot.test.context.SpringBootTest;
      import org.springframework.test.context.junit4.SpringRunner;
      import java.util.HashMap;
      import java.util.Map;
      @RunWith(SpringRunner.class)
      @SpringBootTest
      public class deleteTest2 {
          @Autowired
          private UserMapper mapper;
          @Test
          public void listTest() {
              Map<String, Object> item = new HashMap<>();
              item.put("age", 21L);
              item.put("user_name", "wangmazi");

              // 将item中的元素设置为删除的条件，多个之间为and关系
              int result = mapper.deleteByMap(item);
              System.out.println("result:" + result);
          }
      }
    ```

### delete
  1. 方法定义：
    ```
      /**
      * 根据 entity 条件，删除记录
      *
      * @param wrapper 实体对象封装操作类（可以为 null）
      */
      int delete(@Param(Constants.WRAPPER) Wrapper<T> wrapper);
    ```
  2. 测试用例：
    ```
      package com.itheima;
      import com.baomidou.mybatisplus.core.conditions.query.QueryWrapper;
      import com.itheima.mapper.UserMapper;
      import com.itheima.pojo.User;
      import org.junit.Test;
      import org.junit.runner.RunWith;
      import org.springframework.beans.factory.annotation.Autowired;
      import org.springframework.boot.test.context.SpringBootTest;
      import org.springframework.test.context.junit4.SpringRunner;
      import java.util.HashMap;
      import java.util.Map;
      @RunWith(SpringRunner.class)
      @SpringBootTest
      public class deleteTest3 {
          @Autowired
          private UserMapper mapper;
          @Test
          public void listTest() {
              User user = new User();
              user.setAge(22);
              user.setName("xinxin11");

              // 将实体对象进行包装，包装为操作条件
              QueryWrapper<User> wrapper = new QueryWrapper<>(user);
              int result = mapper.delete(wrapper);
              System.out.println("result:" + result);
          }
      }
    ```

### deleteBatchIds
  1. 方法定义：
    ```
      /**
      * 删除（根据ID 批量删除）
      *
      * @param idList 主键ID列表(不能为 null 以及 empty)
      */
      int deleteBatchIds(@Param(Constants.COLLECTION) Collection<? extends Serializable>
      idList);
    ```
  2. 测试案例：
    ```
      package com.itheima;
      import com.itheima.mapper.UserMapper;
      import org.junit.Test;
      import org.junit.runner.RunWith;
      import org.springframework.beans.factory.annotation.Autowired;
      import org.springframework.boot.test.context.SpringBootTest;
      import org.springframework.test.context.junit4.SpringRunner;
      import java.util.Arrays;
      @RunWith(SpringRunner.class)
      @SpringBootTest
      public class deleteTest4 {
          @Autowired
          private UserMapper mapper;
          @Test
          public void listTest() {
              //根据id集合批量删除
              int result = mapper.deleteBatchIds(Arrays.asList(1L, 5L));
              System.out.println("result:" + result);
          }
      }
    ```

## 查询操作
  * MP提供了多种查询操作，包括根据id查询、批量查询、查询单条数据、查询列表、分页查询等操作。

### selectById
  1. 方法定义：
    ```
      /**
      * 根据 ID 查询
      *
      * @param id 主键ID
      */
      T selectById(Serializable id);
    ```
  2. 测试用例：
    ```
      package com.itheima;
      import com.itheima.mapper.UserMapper;
      import com.itheima.pojo.User;
      import org.junit.Test;
      import org.junit.runner.RunWith;
      import org.springframework.beans.factory.annotation.Autowired;
      import org.springframework.boot.test.context.SpringBootTest;
      import org.springframework.test.context.junit4.SpringRunner;
      @RunWith(SpringRunner.class)
      @SpringBootTest
      public class selectTest {
          @Autowired
          private UserMapper mapper;
          @Test
          public void listTest() {
              User user = mapper.selectById(2L);
              System.out.println(user);
          }
      }
    ```

### selectBatchIds
  1. 方法定义：
    ```
      /**
      * 查询（根据ID 批量查询）
      *
      * @param idList 主键ID列表(不能为 null 以及 empty)
      */
      List<T> selectBatchIds(@Param(Constants.COLLECTION) Collection<? extends Serializable>
      idList);
    ```
  2. 测试用例：
    ```
      package com.itheima;
      import com.itheima.mapper.UserMapper;
      import com.itheima.pojo.User;
      import org.junit.Test;
      import org.junit.runner.RunWith;
      import org.springframework.beans.factory.annotation.Autowired;
      import org.springframework.boot.test.context.SpringBootTest;
      import org.springframework.test.context.junit4.SpringRunner;
      import java.util.Arrays;
      import java.util.List;
      @RunWith(SpringRunner.class)
      @SpringBootTest
      public class selectTest2 {
          @Autowired
          private UserMapper mapper;
          @Test
          public void listTest() {
              List<User> userList = mapper.selectBatchIds(Arrays.asList(2L, 3L));
              for (User user : userList) {
                  System.out.println(user);
              }
          }
      }
    ```

### selectOne
  1. 方法定义：
    ```
      /**
      * 根据 entity 条件，查询一条记录
      *
      * @param queryWrapper 实体对象封装操作类（可以为 null）
      */
      T selectOne(@Param(Constants.WRAPPER) Wrapper<T> queryWrapper);
    ```
  2. 测试用例：
    ```
      package com.itheima;
      import com.baomidou.mybatisplus.core.conditions.query.QueryWrapper;
      import com.itheima.mapper.UserMapper;
      import com.itheima.pojo.User;
      import org.junit.Test;
      import org.junit.runner.RunWith;
      import org.springframework.beans.factory.annotation.Autowired;
      import org.springframework.boot.test.context.SpringBootTest;
      import org.springframework.test.context.junit4.SpringRunner;
      import java.util.Arrays;
      import java.util.List;
      @RunWith(SpringRunner.class)
      @SpringBootTest
      public class selectTest3 {
          @Autowired
          private UserMapper mapper;
          @Test
          public void listTest() {
              QueryWrapper<User> wrapper = new QueryWrapper<>();
              wrapper.eq("name", "李四");

              //根据条件查询一条数据，如果结果超过一条会报错
              User user = mapper.selectOne(wrapper);
              System.out.println(user);
          }
      }
    ```

### selectCount
  1. 方法定义：
    ```
      /**
      * 根据 Wrapper 条件，查询总记录数
      *
      * @param queryWrapper 实体对象封装操作类（可以为 null）
      */
      Integer selectCount(@Param(Constants.WRAPPER) Wrapper<T> queryWrapper);
    ```
  2. 测试用例：
    ```
      package com.itheima;
      import com.baomidou.mybatisplus.core.conditions.query.QueryWrapper;
      import com.itheima.mapper.UserMapper;
      import com.itheima.pojo.User;
      import org.junit.Test;
      import org.junit.runner.RunWith;
      import org.springframework.beans.factory.annotation.Autowired;
      import org.springframework.boot.test.context.SpringBootTest;
      import org.springframework.test.context.junit4.SpringRunner;
      @RunWith(SpringRunner.class)
      @SpringBootTest
      public class selectTest4 {
          @Autowired
          private UserMapper mapper;
          @Test
          public void listTest() {
              QueryWrapper<User> wrapper = new QueryWrapper<>();
              wrapper.gt("age", 20);
              //根据条件查询一条数据，如果结果超过一条会报错
              Integer count = mapper.selectCount(wrapper);
              System.out.println(count);
          }
      }
    ```

### selectList
  1. 方法定义：
    ```
      /**
      * 根据 entity 条件，查询全部记录
      *
      * @param queryWrapper 实体对象封装操作类（可以为 null）
      */
      List<T> selectList(@Param(Constants.WRAPPER) Wrapper<T> queryWrapper);
    ```
  2. 测试案例：
    ```
      package com.itheima;
      import com.baomidou.mybatisplus.core.conditions.query.QueryWrapper;
      import com.itheima.mapper.UserMapper;
      import com.itheima.pojo.User;
      import org.junit.Test;
      import org.junit.runner.RunWith;
      import org.springframework.beans.factory.annotation.Autowired;
      import org.springframework.boot.test.context.SpringBootTest;
      import org.springframework.test.context.junit4.SpringRunner;
      import java.util.List;
      @RunWith(SpringRunner.class)
      @SpringBootTest
      public class selectTest5 {
          @Autowired
          private UserMapper mapper;
          @Test
          public void listTest() {
              QueryWrapper<User> wrapper = new QueryWrapper<>();
              wrapper.gt("age", 20);

              //根据条件查询一条数据，如果结果超过一条会报错
              List<User> userList = mapper.selectList(wrapper);
              for (User user : userList) {
                  System.out.println(user);
              }
          }
      }
    ```

### selectPage
  1. 方法定义：
    ```
      /**
      * 根据 entity 条件，查询全部记录（并翻页）
      *
      * @param page 分页查询条件（可以为 RowBounds.DEFAULT）
      * @param queryWrapper 实体对象封装操作类（可以为 null）
      */
      IPage<T> selectPage(IPage<T> page, @Param(Constants.WRAPPER) Wrapper<T> queryWrapper);
    ```
  2. 在java下新建com.itheima.MybatisPlusConfig.java, 配置分页插件：
    ```
      package com.itheima;
      import com.baomidou.mybatisplus.extension.plugins.PaginationInterceptor;
      import org.mybatis.spring.annotation.MapperScan;
      import org.springframework.context.annotation.Bean;
      import org.springframework.context.annotation.Configuration;
      @Configuration
      @MapperScan("cn.itcast.mp.mapper") //设置mapper接口的扫描包
      public class MybatisPlusConfig {
          /**
          * 分页插件
          */
          @Bean
          public PaginationInterceptor paginationInterceptor() {
              return new PaginationInterceptor();
          }
      }
    ```
  3. 测试案例：
    ```
      package com.itheima;
      import com.baomidou.mybatisplus.core.conditions.query.QueryWrapper;
      import com.baomidou.mybatisplus.core.metadata.IPage;
      import com.baomidou.mybatisplus.extension.plugins.pagination.Page;
      import com.itheima.mapper.UserMapper;
      import com.itheima.pojo.User;
      import org.junit.Test;
      import org.junit.runner.RunWith;
      import org.springframework.beans.factory.annotation.Autowired;
      import org.springframework.boot.test.context.SpringBootTest;
      import org.springframework.test.context.junit4.SpringRunner;
      import java.util.List;
      @RunWith(SpringRunner.class)
      @SpringBootTest
      public class selectTest6 {
          @Autowired
          private UserMapper mapper;
          @Test
          public void listTest() {
              QueryWrapper<User> wrapper = new QueryWrapper<User>();
              wrapper.gt("age", 20); //年龄大于20岁
              Page<User> page = new Page<>(1,1);
              //根据条件查询数据
              IPage<User> iPage = mapper.selectPage(page, wrapper);
              System.out.println("数据总条数：" + iPage.getTotal());
              System.out.println("总页数：" + iPage.getPages());
              List<User> users = iPage.getRecords();
              for (User user : users) {
                  System.out.println("user = " + user);
              }
          }
      }
    ```

# 配置
  * 在MP中有大量的配置，其中有一部分是Mybatis原生的配置，另一部分是MP的配置，详情：https://mybatis.plus/config/

## 基本配置
### configLocation
  * MyBatis 配置文件位置，如果您有单独的 MyBatis 配置，请将其路径配置到 configLocation 中。 MyBatis
Configuration 的具体内容请参考MyBatis 官方文档：
  * **Spring Boot：**
    ```
      mybatis-plus.config-location = classpath:mybatis-config.xml
    ```
  * **Spring MVC：**
    ```
      <bean id="sqlSessionFactory" class="com.baomidou.mybatisplus.extension.spring.MybatisSqlSessionFactoryBean">
        <property name="configLocation" value="classpath:mybatis-config.xml"/>
      </bean>
    ```

#### springboot：
  1. 在resources下新建mybatis-config.xml
    ```
      <?xml version="1.0" encoding="UTF-8" ?>
      <!DOCTYPE configuration
              PUBLIC "-//mybatis.org//DTD Config 3.0//EN"
              "http://mybatis.org/dtd/mybatis-3-config.dtd">
      <configuration>
          <plugins>
              <plugin interceptor="com.baomidou.mybatisplus.extension.plugins.PaginationInterceptor"></plugin>
          </plugins>
      </configuration>
    ```
  2. 在resources下的application.properties中引入
    ```
      # 指定全局的配置文件
      mybatis-plus.config-location=classpath:mybatis-config.xml
    ```
  3. 编写测试方法
    ```
      package com.itheima;
      import com.baomidou.mybatisplus.core.conditions.query.QueryWrapper;
      import com.baomidou.mybatisplus.core.metadata.IPage;
      import com.baomidou.mybatisplus.extension.plugins.pagination.Page;
      import com.itheima.mapper.UserMapper;
      import com.itheima.pojo.User;
      import org.junit.Test;
      import org.junit.runner.RunWith;
      import org.springframework.beans.factory.annotation.Autowired;
      import org.springframework.boot.test.context.SpringBootTest;
      import org.springframework.test.context.junit4.SpringRunner;
      import java.util.List;
      @RunWith(SpringRunner.class)
      @SpringBootTest
      public class selectTest6 {
          @Autowired
          private UserMapper mapper;
          @Test
          public void listTest() {
              QueryWrapper<User> wrapper = new QueryWrapper<User>();
              wrapper.gt("age", 20); //年龄大于20岁
              Page<User> page = new Page<>(1,1);
              //根据条件查询数据
              IPage<User> iPage = mapper.selectPage(page, wrapper);
              System.out.println("数据总条数：" + iPage.getTotal());
              System.out.println("总页数：" + iPage.getPages());
              List<User> users = iPage.getRecords();
              for (User user : users) {
                  System.out.println("user = " + user);
              }
          }
      }
    ```

#### springmvc：
  1. 在resources下新建mybatis-config.xml
    ```
      <?xml version="1.0" encoding="UTF-8" ?>
      <!DOCTYPE configuration
              PUBLIC "-//mybatis.org//DTD Config 3.0//EN"
              "http://mybatis.org/dtd/mybatis-3-config.dtd">

      <configuration>

      </configuration>
    ```
  2. 在applicationContext.xml中引入
    ```
      <!--这里使用MP提供的sqlSessionFactory，完成了Spring与MP的整合-->
      <bean id="sessionFactory" class="com.baomidou.mybatisplus.extension.spring.MybatisSqlSessionFactoryBean">
          <property name="configLocation" value="classpath:sqlMapConfig.xml" />
      </bean>
    ```

### mapperLocations
  * MyBatis Mapper 所对应的 XML 文件位置，如果您在 Mapper 中有自定义方法（XML 中有自定义实现），需要进行该配置，告诉 Mapper 所对应的 XML 文件位置。
  * **Spring Boot：**
    ```
      mybatis-plus.mapper-locations = classpath*:mybatis/*.xml
    ```
  * **Spring MVC：**
    ```
      <bean id="sqlSessionFactory" class="com.baomidou.mybatisplus.extension.spring.MybatisSqlSessionFactoryBean">
        <property name="mapperLocations" value="classpath*:mybatis/*.xml"/>
      </bean>
    ```

#### springboot：
  1. 在resources下新建mapper/UserMapper.xml,并写入
    ```
      <?xml version="1.0" encoding="UTF-8" ?>
      <!DOCTYPE mapper
              PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
              "http://mybatis.org/dtd/mybatis-3-mapper.dtd">
      <mapper namespace="com.itheima.mapper.UserMapper">
          <select id="findUserById" resultType="com.itheima.pojo.User">
              select * from tb_user where id = #{id}
          </select>
      </mapper>
    ```
  2. 在com.itheima.mapper.UserMapper.java中添加findUserById接口方法
    ```
      package com.itheima.mapper;
      import com.baomidou.mybatisplus.core.mapper.BaseMapper;
      import com.itheima.pojo.User;
      public interface UserMapper extends BaseMapper<User> {
          User findUserById(Long id);
      }
    ```
  3. 在application.properties中引入
    ```
      # 指定Mapper.xml文件的路径
      mybatis-plus.mapper-locations=classpath:mapper/UserMapper.xml
    ```
  4. 编写测试方法
    ```
      package com.itheima;
      import com.itheima.mapper.UserMapper;
      import com.itheima.pojo.User;
      import org.junit.Test;
      import org.junit.runner.RunWith;
      import org.springframework.beans.factory.annotation.Autowired;
      import org.springframework.boot.test.context.SpringBootTest;
      import org.springframework.test.context.junit4.SpringRunner;
      @RunWith(SpringRunner.class)
      @SpringBootTest
      public class selectTest {
          @Autowired
          private UserMapper mapper;
          @Test
          public void listTest2() {
              User user = mapper.findUserById(2L);
              System.out.println(user);
          }
      }
    ```

#### SpringMVC：
  1. 在resources下新建mapper/UserMapper.xml
    ```
      <?xml version="1.0" encoding="UTF-8" ?>
      <!DOCTYPE mapper PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN" "http://mybatis.org/dtd/mybatis-3-mapper.dtd">
      <mapper namespace="com.itheima.mapper.UserMapper">
          <select id="selectUsersFromId" resultType="com.itheima.pojo.User">
              select * from tb_user where id = #{id}
          </select>
      </mapper>
    ```
  2. 在com.itheima.mapper.UserMapper.java中添加selectUsersFromId方法
    ```
      package com.itheima.mapper;
      import com.baomidou.mybatisplus.core.mapper.BaseMapper;
      import com.itheima.pojo.User;
      public interface UserMapper extends BaseMapper<User> {
          User selectUsersFromId(int id);
      }
    ```
  3. 在resources下的applicationContext.xml中引入mapper
    ```
      <!--这里使用MP提供的sqlSessionFactory，完成了Spring与MP的整合-->
      <bean id="sessionFactory" class="com.baomidou.mybatisplus.extension.spring.MybatisSqlSessionFactoryBean">
          <property name="dataSource" ref="druidDataSource" />
          <property name="mapperLocations" value="classpath*:mapper/UserMapper.xml" />
      </bean>
    ```
  4. 编写测试方法
    ```
      package com.itheima.test;
      import com.itheima.mapper.UserMapper;
      import com.itheima.pojo.User;
      import org.junit.Test;
      import org.junit.runner.RunWith;
      import org.springframework.beans.factory.annotation.Autowired;
      import org.springframework.test.context.ContextConfiguration;
      import org.springframework.test.context.junit4.SpringJUnit4ClassRunner;
      @RunWith(SpringJUnit4ClassRunner.class)
      @ContextConfiguration(locations = "classpath:applicationContext.xml")
      public class userTest2 {
          @Autowired
          private UserMapper userMapper;
          @Test
          public void testList() {
              User user = userMapper.selectUsersFromId(7);
              System.out.println(user);
          }
      }
    ```

### typeAliasesPackage
  * MyBaits 别名包扫描路径，通过该属性可以给包中的类注册别名，注册后在 Mapper 对应的 XML 文件中可以直接使用类名，而不用使用全限定的类名（即 XML 中调用的时候不用包含包名）。
  * **Spring boot:**
    ```
      mybatis-plus.type-aliases-package = cn.itcast.mp.pojo
    ```
  * **Spring MVC:**
    ```
      <bean id="sqlSessionFactory" class="com.baomidou.mybatisplus.extension.spring.MybatisSqlSessionFactoryBean">
        <property name="typeAliasesPackage" value="com.baomidou.mybatisplus.samples.quickstart.entity"/>
      </bean>
    ```

#### Spring boot：
  1. 在application.properties中
    ```
      # 实体对象的扫描包
      mybatis-plus.type-aliases-package=com.itheima.pojo
    ```
  2. 修改mapper/UserMapper.xml的resultType
    * 修改前：
      ```
        <mapper namespace="com.itheima.mapper.UserMapper">
            <select id="findUserById" resultType="com.itheima.pojo.User">
                select * from tb_user where id = #{id}
            </select>
        </mapper>
      ```
    * 修改后：
      ```
        <mapper namespace="com.itheima.mapper.UserMapper">
            <select id="findUserById" resultType="User">
                select * from tb_user where id = #{id}
            </select>
        </mapper>
      ```
  3. 编写测试方法
    ```
      package com.itheima.test;
      import com.itheima.mapper.UserMapper;
      import com.itheima.pojo.User;
      import org.junit.Test;
      import org.junit.runner.RunWith;
      import org.springframework.beans.factory.annotation.Autowired;
      import org.springframework.test.context.ContextConfiguration;
      import org.springframework.test.context.junit4.SpringJUnit4ClassRunner;
      @RunWith(SpringJUnit4ClassRunner.class)
      @ContextConfiguration(locations = "classpath:applicationContext.xml")
      public class userTest2 {
          @Autowired
          private UserMapper userMapper;
          @Test
          public void testList() {
              User user = userMapper.selectUsersFromId(7);
              System.out.println(user);
          }
      }
    ```

#### Spring MVC：
  1. 在resources/applicationContext.xml中,添加typeAliasesPackage
    ```
      <!--这里使用MP提供的sqlSessionFactory，完成了Spring与MP的整合-->
      <bean id="sessionFactory" class="com.baomidou.mybatisplus.extension.spring.MybatisSqlSessionFactoryBean">
          <property name="typeAliasesPackage" value="com.itheima.pojo" />
      </bean>
    ```
  2. 修改mapper/UserMapper.xml的resultType
    * 修改前：
      ```
        <mapper namespace="com.itheima.mapper.UserMapper">
            <select id="findUserById" resultType="com.itheima.pojo.User">
                select * from tb_user where id = #{id}
            </select>
        </mapper>
      ```
    * 修改后：
      ```
        <mapper namespace="com.itheima.mapper.UserMapper">
            <select id="findUserById" resultType="User">
                select * from tb_user where id = #{id}
            </select>
        </mapper>
      ```
  3. 编写测试方法
    ```
      package com.itheima.test;
      import com.itheima.mapper.UserMapper;
      import com.itheima.pojo.User;
      import org.junit.Test;
      import org.junit.runner.RunWith;
      import org.springframework.beans.factory.annotation.Autowired;
      import org.springframework.test.context.ContextConfiguration;
      import org.springframework.test.context.junit4.SpringJUnit4ClassRunner;
      @RunWith(SpringJUnit4ClassRunner.class)
      @ContextConfiguration(locations = "classpath:applicationContext.xml")
      public class userTest2 {
          @Autowired
          private UserMapper userMapper;
          @Test
          public void testList() {
              User user = userMapper.selectUsersFromId(7);
              System.out.println(user);
          }
      }
    ```

## 进阶配置
  * 本部分（Configuration）的配置大都为 MyBatis 原生支持的配置，这意味着您可以通过 MyBatis XML 配置文件的形
式进行配置。

### mapUnderscoreToCamelCase
  * 类型：boolean
  * 默认值：true
  * 是否开启自动驼峰命名规则（camel case）映射，即从经典数据库列名 A_COLUMN（下划线命名） 到经典 Java 属
性名 aColumn（驼峰命名） 的类似映射。

{% note warning %}
  注意：
    此属性在 MyBatis 中原默认值为 false，在 MyBatis-Plus 中，此属性也将用于生成最终的 SQL 的 select body如果您的数据库命名符合规则无需使用 @TableField 注解指定数据库字段名
{% endnote %}

  * springboot: 在application.properties中
    ```
      #关闭自动驼峰映射，该参数不能和mybatis-plus.config-location同时存在
      mybatis-plus.configuration.map-underscore-to-camel-case=false
    ```

### cacheEnabled
  * 类型： boolean
  * 默认值： true
  * 全局地开启或关闭配置文件中的所有映射器已经配置的任何缓存，默认为 true。
  * springboot: 在application.properties中:
    ```
      mybatis-plus.configuration.cache-enabled=false
    ```

## DB 策略配置
### idType
  * 类型：com.baomidou.mybatisplus.annotation.IdType
  * 默认值：ID_WORKER（用于比如说设置id的自增长）
  * 全局默认主键类型，设置后，即可省略实体对象中的@TableId(type = IdType.AUTO)配置。
  * **SpringBoot：**
    ```
      # 全局的id生成策略
      mybatis-plus.global-config.db-config.id-type=auto
    ```
  * **SpringMVC：**
    ```
      <!--这里使用MP提供的sqlSessionFactory，完成了Spring与MP的整合-->
      <bean id="sessionFactory" class="com.baomidou.mybatisplus.extension.spring.MybatisSqlSessionFactoryBean">
          <property name="dataSource" ref="druidDataSource" />
          <!--全局的id生成策略-->
          <property name="globalConfig">
              <bean class="com.baomidou.mybatisplus.core.config.GlobalConfig">
                  <property name="dbConfig">
                      <bean class="com.baomidou.mybatisplus.core.config.GlobalConfig$DbConfig">
                          <property name="idType" value="AUTO" />
                      </bean>
                  </property>
              </bean>
          </property>
      </bean>
    ```

### tablePrefix
  * 类型： String
  * 默认值： null
  * 表名前缀，全局配置后可省略@TableName()配置。
  * **SpringBoot：**
    ```
      # 全局的表名前缀
      mybatis-plus.global-config.db-config.table-prefix=tb_
    ```
  * **SpringMVC：**
    ```
      <bean id="sqlSessionFactory" class="com.baomidou.mybatisplus.extension.spring.MybatisSqlSessionFactoryBean">
        <property name="dataSource" ref="dataSource"/>
        <property name="globalConfig">
          <bean class="com.baomidou.mybatisplus.core.config.GlobalConfig">
            <property name="dbConfig">
              <bean class="com.baomidou.mybatisplus.core.config.GlobalConfig$DbConfig">
                <property name="idType" value="AUTO"/>
                <property name="tablePrefix" value="tb_"/>
              </bean>
            </property>
          </bean>
        </property>
      </bean>
    ```

# 条件构造器
## allEq(查询)
### 查询全部
  ```
    allEq(Map<R, V> params)
    allEq(Map<R, V> params, boolean null2IsNull)
    allEq(boolean condition, Map<R, V> params, boolean null2IsNull)
  ```
  - 举例：
    ```
      import com.baomidou.mybatisplus.core.conditions.query.QueryWrapper;
      import com.itheima.mapper.UserMapper;
      import com.itheima.pojo.User;
      import org.junit.Test;
      import org.junit.runner.RunWith;
      import org.springframework.beans.factory.annotation.Autowired;
      import org.springframework.test.context.ContextConfiguration;
      import org.springframework.test.context.junit4.SpringJUnit4ClassRunner;
      import java.util.HashMap;
      import java.util.List;
      import java.util.Map;
      @RunWith(SpringJUnit4ClassRunner.class)
      @ContextConfiguration(locations = "classpath:applicationContext.xml")
      public class selectTest {
          @Autowired
          private UserMapper userMapper;
          @Test
          public void testInsertItem() {
              Map<String,Object> params = new HashMap<String,Object>();
              params.put("name", "李四");
              params.put("age", "20");
              params.put("password", null);
              QueryWrapper<User> wrapper = new QueryWrapper<User>();
              // 相当于sql: SELECT * FROM tb_user WHERE password IS NULL AND name = ? AND age = ?
              wrapper.allEq(params);
              List<User> userList = userMapper.selectList(wrapper);
              for (User user : userList) {
                  System.out.println("user" + user);
              }
          }

          @Test
          public void testInsertItem2() {
              Map<String,Object> params = new HashMap<String,Object>();
              params.put("name", "李四");
              params.put("age", "20");
              params.put("password", null);
              QueryWrapper<User> wrapper = new QueryWrapper<User>();
              // 相当于sql: SELECT * FROM tb_user WHERE name = ? AND age = ?
              wrapper.allEq(params, false);
              List<User> userList = userMapper.selectList(wrapper);
              for (User user : userList) {
                  System.out.println("user" + user);
              }
          }
      }
    ```

### 过滤查询
  ```
    allEq(BiPredicate<R, V> filter, Map<R, V> params)
    allEq(BiPredicate<R, V> filter, Map<R, V> params, boolean null2IsNull)
    allEq(boolean condition, BiPredicate<R, V> filter, Map<R, V> params, boolean
    null2IsNull)
  ```
  * 举例：
    ```
      import com.baomidou.mybatisplus.core.conditions.query.QueryWrapper;
      import com.itheima.mapper.UserMapper;
      import com.itheima.pojo.User;
      import org.junit.Test;
      import org.junit.runner.RunWith;
      import org.springframework.beans.factory.annotation.Autowired;
      import org.springframework.test.context.ContextConfiguration;
      import org.springframework.test.context.junit4.SpringJUnit4ClassRunner;
      import java.util.HashMap;
      import java.util.List;
      import java.util.Map;
      @RunWith(SpringJUnit4ClassRunner.class)
      @ContextConfiguration(locations = "classpath:applicationContext.xml")
      public class selectTest2 {
          @Autowired
          private UserMapper userMapper;
          @Test
          public void testInsertItem() {
              Map<String,Object> params = new HashMap<String,Object>();
              params.put("age", "28");
              params.put("name", "张毅");
              QueryWrapper<User> wrapper = new QueryWrapper<User>();
              // 相当于sql: SELECT * FROM tb_user WHERE name = ? AND age = ?
              wrapper.allEq((k, v) -> (k.equals("name") || k.equals("age")),params);
              List<User> userList = userMapper.selectList(wrapper);
              for (User user : userList) {
                  System.out.println("user" + user);
              }
          }
      }
    ```

## 基本比较操作
### eq
  * 等于 =
### ne
  * 不等于 <>
### gt
  * 大于 >
### ge
  * 大于等于 >=
### lt
  * 小于 <
### le
  * 小于等于 <=
### between
  * BETWEEN 值1 AND 值2
### notBetween
  * NOT BETWEEN 值1 AND 值2
### in
  * 字段 IN (value.get(0), value.get(1), ...)
### 测试用例：
  ```
    import com.baomidou.mybatisplus.core.conditions.query.QueryWrapper;
    import com.itheima.mapper.UserMapper;
    import com.itheima.pojo.User;
    import org.junit.Test;
    import org.junit.runner.RunWith;
    import org.springframework.beans.factory.annotation.Autowired;
    import org.springframework.test.context.ContextConfiguration;
    import org.springframework.test.context.junit4.SpringJUnit4ClassRunner;
    import java.util.HashMap;
    import java.util.List;
    import java.util.Map;
    @RunWith(SpringJUnit4ClassRunner.class)
    @ContextConfiguration(locations = "classpath:applicationContext.xml")
    public class selectTest3 {
        @Autowired
        private UserMapper userMapper;
        @Test
        public void testInsertItem() {
            QueryWrapper<User> wrapper = new QueryWrapper<User>();
            wrapper.gt("age", 20)
                    .eq("password", 123);
            List<User> userList = userMapper.selectList(wrapper);
            for (User user : userList) {
                System.out.println("user" + user);
            }
        }
    }
  ```

## 模糊查询
### like
  * LIKE '%值%'
  * 例: like("name", "王") ---> name like '%王%'
### notLike
  * NOT LIKE '%值%'
  * 例: notLike("name", "王") ---> name not like '%王%'
### likeLeft
  * LIKE '%值'
  * 例: likeLeft("name", "王") ---> name like '%王'
### likeRight
  * LIKE '值%'
  * 例: likeRight("name", "王") ---> name like '王%'

### 测试用例
  ```
    import com.baomidou.mybatisplus.core.conditions.query.QueryWrapper;
    import com.itheima.mapper.UserMapper;
    import com.itheima.pojo.User;
    import org.junit.Test;
    import org.junit.runner.RunWith;
    import org.springframework.beans.factory.annotation.Autowired;
    import org.springframework.test.context.ContextConfiguration;
    import org.springframework.test.context.junit4.SpringJUnit4ClassRunner;
    import java.util.List;
    @RunWith(SpringJUnit4ClassRunner.class)
    @ContextConfiguration(locations = "classpath:applicationContext.xml")
    public class selectTest4 {
        @Autowired
        private UserMapper userMapper;
        @Test
        public void testInsertItem() {
            QueryWrapper<User> wrapper = new QueryWrapper<User>();
            // 相当于：SELECT id,user_name,password,name,age,email FROM tb_user WHERE name LIKE ?
            // Parameters: %张%(String)
            wrapper.like("name", "张");
            List<User> userList = userMapper.selectList(wrapper);
            for (User user : userList) {
                System.out.println("user" + user);
            }
        }
    }
  ```

## 排序
### orderBy
  * 排序：ORDER BY 字段, ...
  * 例: orderBy(true, true, "id", "name") ---> order by id ASC,name ASC
### orderByAsc
  * 排序：ORDER BY 字段, ... ASC
  * 例: orderByAsc("id", "name") ---> order by id ASC,name ASC
### orderByDesc
  * 排序：ORDER BY 字段, ... DESC
  * 例: orderByDesc("id", "name") ---> order by id DESC,name DESC

### 测试用例
  ```
    import com.baomidou.mybatisplus.core.conditions.query.QueryWrapper;
    import com.itheima.mapper.UserMapper;
    import com.itheima.pojo.User;
    import org.junit.Test;
    import org.junit.runner.RunWith;
    import org.springframework.beans.factory.annotation.Autowired;
    import org.springframework.test.context.ContextConfiguration;
    import org.springframework.test.context.junit4.SpringJUnit4ClassRunner;
    import java.util.List;
    @RunWith(SpringJUnit4ClassRunner.class)
    @ContextConfiguration(locations = "classpath:applicationContext.xml")
    public class selectTest5 {
        @Autowired
        private UserMapper userMapper;
        @Test
        public void testInsertItem() {
            QueryWrapper<User> wrapper = new QueryWrapper<User>();
            // 相当于sql: SELECT id,user_name,password,name,age,email FROM tb_user ORDER BY age DESC
            wrapper.orderByDesc("age");
            List<User> userList = userMapper.selectList(wrapper);
            for (User user : userList) {
                System.out.println("user" + user);
            }
        }
    }
  ```

## 逻辑查询
### or
  * 拼接 OR
  * 主动调用 or 表示紧接着下一个方法不是用 and 连接!(不调用 or 则默认为使用 and 连接)
### and
  * AND 嵌套
  * 例: and(i -> i.eq("name", "李白").ne("status", "活着")) ---> and (name = '李白' and status<> '活着')

### 测试案例
  ```
    import com.baomidou.mybatisplus.core.conditions.query.QueryWrapper;
    import com.itheima.mapper.UserMapper;
    import com.itheima.pojo.User;
    import org.junit.Test;
    import org.junit.runner.RunWith;
    import org.springframework.beans.factory.annotation.Autowired;
    import org.springframework.test.context.ContextConfiguration;
    import org.springframework.test.context.junit4.SpringJUnit4ClassRunner;
    import java.util.List;
    @RunWith(SpringJUnit4ClassRunner.class)
    @ContextConfiguration(locations = "classpath:applicationContext.xml")
    public class selectTest6 {
        @Autowired
        private UserMapper userMapper;
        @Test
        public void testInsertItem() {
            QueryWrapper<User> wrapper = new QueryWrapper<User>();
            // 相当于sql: SELECT id,user_name,password,name,age,email FROM tb_user WHERE name = ? OR age = ?
            wrapper.eq("age", 22).or().eq("name", "张毅");
            List<User> userList = userMapper.selectList(wrapper);
            for (User user : userList) {
                System.out.println("user" + user);
            }
        }
    }
  ```

## select 
  * 在MP查询中，默认查询所有的字段，如果有需要也可以通过select方法进行指定字段。
### 测试案例
  ```
    import com.baomidou.mybatisplus.core.conditions.query.QueryWrapper;
    import com.itheima.mapper.UserMapper;
    import com.itheima.pojo.User;
    import org.junit.Test;
    import org.junit.runner.RunWith;
    import org.springframework.beans.factory.annotation.Autowired;
    import org.springframework.test.context.ContextConfiguration;
    import org.springframework.test.context.junit4.SpringJUnit4ClassRunner;
    import java.util.List;
    @RunWith(SpringJUnit4ClassRunner.class)
    @ContextConfiguration(locations = "classpath:applicationContext.xml")
    public class selectTest7 {
        @Autowired
        private UserMapper userMapper;
        @Test
        public void testInsertItem() {
            QueryWrapper<User> wrapper = new QueryWrapper<User>();
            // 相当于sql: SELECT id,name,age FROM tb_user WHERE name = ? OR age = ?
            wrapper.gt("age", 20)
                    .or()
                    .eq("name", "张毅")
                    .select("id", "name", "age");
            List<User> userList = userMapper.selectList(wrapper);
            for (User user : userList) {
                System.out.println("user" + user);
            }
        }
    }
  ```
   