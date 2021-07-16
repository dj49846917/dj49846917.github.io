---
title: spring3-jdbcTemplate
date: 2021-06-18 15:22:44
tags:
  - spring
  - jdbc
  - java
type: java                                                                         # 标签、分类和友情链接三個页面需要配置(必填)
description: Spring是分层的JavaSE/EE应用full-stack轻量级开源框架                   # 描述
keywords: spring                                                                       # 关键词，便于搜索
top_img: /images/spring/logo.jpg             # 文章的顶部图片
aside: true                                                                         # 展示文章侧边栏(默认为true)
categories: 
  - 教程
  - spring 
  - jdbc                                                               # 文章标签
  - java
cover: /images/spring/logo.jpg                 # 文章的缩略图（用在首页）
---

# Spring JdbcTemplate基本使用
## JdbcTemplate概述
它是spring框架中提供的一个对象，是对原始繁琐的Jdbc API对象的简单封装。spring框架为我们提供了很多的操作模板类。例如：操作关系型数据的JdbcTemplate和HibernateTemplate，操作nosql数据库的RedisTemplate，操作消息队列的JmsTemplate等等。

## JdbcTemplate开发步骤
  1. 导入spring-jdbc和spring-tx坐标
  2. 创建数据库表和实体
  3. 创建JdbcTemplate对象
  4. 执行数据库操作

### 导入spring-jdbc和spring-tx坐标
  1. 新建itheima_spring_jdbc的module,并配置webapp文件夹
  2. 在pom.xml中，导入spring-jdbc、spring-tx、junit、mysql-connector-java、druid坐标
    ```
      <dependency>
          <groupId>org.springframework</groupId>
          <artifactId>spring-jdbc</artifactId>
          <version>5.3.6</version>
      </dependency>
      <dependency>
          <groupId>org.springframework</groupId>
          <artifactId>spring-tx</artifactId>
          <version>5.3.6</version>
      </dependency>
      <dependency>
          <groupId>mysql</groupId>
          <artifactId>mysql-connector-java</artifactId>
          <version>5.1.46</version>
      </dependency>
      <dependency>
          <groupId>junit</groupId>
          <artifactId>junit</artifactId>
          <version>4.12</version>
      </dependency>
      <dependency>
          <groupId>com.alibaba</groupId>
          <artifactId>druid</artifactId>
          <version>1.1.10</version>
      </dependency>
    ```

### 创建数据库表和实体
  1. 在mysql中，创建Account表，并添加两个字段name和money
    * ![jdbc表](/images/spring/jdbc表.jpg)

  2. 在test/java下，创建com.itheima.domain.Account的实体类(要和数据库中的Account表，字段完全一样)
    ```
      package com.itheima.domain;
      public class Account {
          private String name;
          private double money;
          @Override
          public String toString() {
              return "Account{" +
                      "name='" + name + '\'' +
                      ", money=" + money +
                      '}';
          }
          public String getName() {
              return name;
          }
          public void setName(String name) {
              this.name = name;
          }
          public double getMoney() {
              return money;
          }
          public void setMoney(double money) {
              this.money = money;
          }
      }
    ```

### 创建JdbcTemplate对象
  1. 在test/java下创建com.itheima.jdbc.JdbcTemplateTest测试类

### 执行数据库操作
  1. 在JdbcTemplateTest测试类中：
    ```
      package com.itheima.jdbc;
      import com.alibaba.druid.pool.DruidDataSource;
      import org.junit.Test;
      import org.springframework.jdbc.core.JdbcTemplate;

      public class JdbcTemplateTest {
          @Test
          // 测试jdbcTemplate开发步骤
          public void test1() {
              // 1. 创建数据源对象
              DruidDataSource dataSource = new DruidDataSource();
              dataSource.setDriverClassName("com.mysql.jdbc.Driver");
              dataSource.setUrl("jdbc:mysql://localhost:3306/test");
              dataSource.setUsername("root");
              dataSource.setPassword("root");
              // 2. 创建JdbcTemplate对象
              JdbcTemplate template = new JdbcTemplate();
              // 3. 设置数据源给JdbcTemplate
              template.setDataSource(dataSource);
              // 执行操作
              template.update("insert into account values(?,?)", "zhangsan", 5000.00);
          }
      }
    ```

## Spring产生JdbcTemplate对象
  * 在pom.xml中导入spring-context，我们可以将JdbcTemplate的创建权交给Spring，将数据源DataSource的创建权也交给Spring，在Spring容器内部将数据源DataSource注入到JdbcTemplate模版对象中，配置如下：
    ```
      <bean id="druidDataSource" class="com.alibaba.druid.pool.DruidDataSource">
          <property name="driverClassName" value="com.mysql.jdbc.Driver"></property>
          <property name="url" value="jdbc:mysql://localhost:3306/test"></property>
          <property name="username" value="root"></property>
          <property name="password" value="root"></property>
      </bean>

      <bean id="template" class="org.springframework.jdbc.core.JdbcTemplate">
          <!--这个dataSource是JDBCTemplate的setDataSource方法-->
          <property name="dataSource" ref="druidDataSource"></property>
      </bean>
    ```

  * 编写测试方法
    ```
      package com.itheima.jdbc;
      import org.junit.Test;
      import org.springframework.context.ApplicationContext;
      import org.springframework.context.support.ClassPathXmlApplicationContext;
      import org.springframework.jdbc.core.JdbcTemplate;

      public class JdbcTemplateTest2 {
          @Test
          // 测试jdbcTemplate开发步骤
          public void test1() {
              ApplicationContext app = new ClassPathXmlApplicationContext("applicationContext.xml");
              JdbcTemplate template = (JdbcTemplate) app.getBean("template");
              template.update("insert into account values(?,?)", "lisi", 400);
          }
      }
    ```

## JdbcTemplate的常用操作
### 修改操作
  * 需导入spring-test包
    ```
      package com.itheima.jdbc;
      import org.junit.Test;
      import org.junit.runner.RunWith;
      import org.springframework.beans.factory.annotation.Autowired;
      import org.springframework.jdbc.core.JdbcTemplate;
      import org.springframework.test.context.ContextConfiguration;
      import org.springframework.test.context.junit4.SpringJUnit4ClassRunner;

      @RunWith(SpringJUnit4ClassRunner.class)
      @ContextConfiguration("classpath:applicationContext.xml")
      public class JdbcTemplateTest3 {
          @Autowired
          private JdbcTemplate template;

          @Test
          // 测试修改操作
          public void TestUpdate() {
              template.update("update account set money=? where name=?", 600, "lisi");
          }
      }
    ```

### 删除操作
  ```
    package com.itheima.jdbc;

    import org.junit.Test;
    import org.junit.runner.RunWith;
    import org.springframework.beans.factory.annotation.Autowired;
    import org.springframework.jdbc.core.JdbcTemplate;
    import org.springframework.test.context.ContextConfiguration;
    import org.springframework.test.context.junit4.SpringJUnit4ClassRunner;

    @RunWith(SpringJUnit4ClassRunner.class)
    @ContextConfiguration("classpath:applicationContext.xml")
    public class JdbcTemplateTest3 {
        @Autowired
        private JdbcTemplate template;

        @Test
        // 测试删除操作
        public void TestDelete() {
            template.update("delete from account where name=?","lisi");
        }
    }
  ```

### 测试查询操作
  1. 查询所有数据(使用query方法)
    ```
      package com.itheima.jdbc;

      import com.itheima.domain.Account;
      import org.junit.Test;
      import org.junit.runner.RunWith;
      import org.springframework.beans.factory.annotation.Autowired;
      import org.springframework.jdbc.core.BeanPropertyRowMapper;
      import org.springframework.jdbc.core.JdbcTemplate;
      import org.springframework.test.context.ContextConfiguration;
      import org.springframework.test.context.junit4.SpringJUnit4ClassRunner;

      import java.util.List;

      @RunWith(SpringJUnit4ClassRunner.class)
      @ContextConfiguration("classpath:applicationContext.xml")
      public class JdbcTemplateTest4 {
          @Autowired
          private JdbcTemplate template;

          @Test
          // 测试查询所有操作
          public void TestQueryAll() {
              // 这个Account是创建的实体类，和数据库字段一一对应的
              List<Account> list = template.query("select * from account", new BeanPropertyRowMapper<Account>(Account.class));
              System.out.println(list.toString());
          }
      }
    ```

  2. 查询单个数据操作(使用queryForObject方法)
    ```
      @Test
      // 测试查询单个对象操作
      public void TestQueryOne() {
          // 这个Account是创建的实体类，和数据库字段一一对应的
          Account account = template.queryForObject("select * from account", new BeanPropertyRowMapper<Account>(Account.class), "zhangsan");
          System.out.println(account.toString());
      }
    ```

  3. 测试查询单个简单数据操作(聚合查询)
    ```
      package com.itheima.jdbc;

      import com.itheima.domain.Account;
      import org.junit.Test;
      import org.junit.runner.RunWith;
      import org.springframework.beans.factory.annotation.Autowired;
      import org.springframework.jdbc.core.BeanPropertyRowMapper;
      import org.springframework.jdbc.core.JdbcTemplate;
      import org.springframework.test.context.ContextConfiguration;
      import org.springframework.test.context.junit4.SpringJUnit4ClassRunner;

      import java.util.List;

      @RunWith(SpringJUnit4ClassRunner.class)
      @ContextConfiguration("classpath:applicationContext.xml")
      public class JdbcTemplateTest4 {
          @Autowired
          private JdbcTemplate template;

          @Test
          // 查询money总数（聚合查询）
          public void TestQuerySum() {
              Double aLong = template.queryForObject("select sum(money) from account", Double.class);
              System.out.println(aLong);
          }
      }
    ```

## 知识要点
  1. 导入spring-jdbc和spring-tx坐标
  2. 创建数据库表和实体
  3. 创建JdbcTemplate对象
    ```
      JdbcTemplate jdbcTemplate = new JdbcTemplate();
      jdbcTemplate.setDataSource(dataSource);
    ```
  4. 执行数据库操作
    * 更新操作：
      ```
        jdbcTemplate.update (sql,params)
      ```
        
    * 查询操作：
      ```
        jdbcTemplate.query (sql,Mapper,params)
        jdbcTemplate.queryForObject(sql,Mapper,params)
      ```
     


  

