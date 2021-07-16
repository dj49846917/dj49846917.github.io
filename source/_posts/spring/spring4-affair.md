---
title: spring4-事务
date: 2021-06-21 15:49:46
tags:
  - spring
  - 事务
  - java
type: java                                                                         # 标签、分类和友情链接三個页面需要配置(必填)
description: Spring是分层的JavaSE/EE应用full-stack轻量级开源框架                   # 描述
keywords: spring                                                                       # 关键词，便于搜索
top_img: /images/spring/logo.jpg             # 文章的顶部图片
aside: true                                                                         # 展示文章侧边栏(默认为true)
categories: 
  - 教程
  - spring
  - 事务                                                                # 文章标签
  - java
cover: /images/spring/logo.jpg                 # 文章的缩略图（用在首页）
---

# 编程式事务控制相关对象
## PlatformTransactionManager
  * PlatformTransactionManager 接口是 spring 的事务管理器，它里面提供了我们常用的操作事务的方法。
  * ![事务](/images/spring/事务.jpg)
{% note danger %}
  注意：PlatformTransactionManager 是接口类型，不同的 Dao 层技术则有不同的实现类，例如：Dao 层技术是jdbc 或 mybatis 时：org.springframework.jdbc.datasource.DataSourceTransactionManager Dao 层技术是hibernate时：org.springframework.orm.hibernate5.HibernateTransactionManager
{% endnote %}

## TransactionDefinition
  * TransactionDefinition 是事务的定义信息对象，里面有如下方法：
  * ![事务](/images/spring/事务2.jpg)
    1. 事务隔离级别
      * 设置隔离级别，可以解决事务并发产生的问题，如脏读、不可重复读和虚读。
        - ISOLATION_DEFAULT
        - ISOLATION_READ_UNCOMMITTED
        - ISOLATION_READ_COMMITTED
        - ISOLATION_REPEATABLE_READ
        - ISOLATION_SERIALIZABLE

    2. 事务传播行为
      * <div style="color: red">REQUIRED：如果当前没有事务，就新建一个事务，如果已经存在一个事务中，加入到这个事务中。一般的选择（默认值）</div>
      * <div style="color: red">SUPPORTS：支持当前事务，如果当前没有事务，就以非事务方式执行（没有事务）</div>
      * MANDATORY：使用当前的事务，如果当前没有事务，就抛出异常
      * REQUERS_NEW：新建事务，如果当前在事务中，把当前事务挂起。
      * NOT_SUPPORTED：以非事务方式执行操作，如果当前存在事务，就把当前事务挂起
      * NEVER：以非事务方式运行，如果当前存在事务，抛出异常
      * NESTED：如果当前存在事务，则在嵌套事务内执行。如果当前没有事务，则执行 REQUIRED 类似的操作
      超时时间：默认值是-1，没有超时限制。如果有，以秒为单位进行设置
      是否只读：建议查询时设置为只读

## TransactionStatus
  * TransactionStatus 接口提供的是事务具体的运行状态，方法介绍如下。
  * ![事务](/images/spring/事务3.jpg)

# 基于XML的声明式事务控制
## 什么是声明式事务控制
  * Spring 的声明式事务顾名思义就是**采用声明的方式来处理事务**。这里所说的声明，就是指在配置文件中声明，用在 Spring 配置文件中声明式的处理事务来代替代码式的处理事务。
  * 作用： 
    -  事务管理不侵入开发的组件。具体来说，业务逻辑对象就不会意识到正在事务管理之中，事实上也应该如此，因为事务管理是属于系统层面的服务，而不是业务逻辑的一部分，如果想要改变事务管理策划的话，也只需要在定义文件中重新配置即可
    - 在不需要事务管理的时候，只要在设定文件上修改一下，即可移去事务管理服务，无需改变代码重新编译，这样维护起来极其方便
{% note danger %}
  注意：Spring 声明式事务控制底层就是AOP。
{% endnote %}

## 声明式事务控制的实现
  * 声明式事务控制明确事项：
    - 谁是切点？
    - 谁是通知？
    - 配置切面？

## 声明式事务控制的实现
  1. 新建itheima_spring_shiwu的module,并配置webapp文件夹
  2. 在pom.xml中添加spring-context、druid、spring-tx、spring-jdbc、aspectjweaver、mysql-connector-java包
    ```
      <dependency>
            <groupId>org.springframework</groupId>
            <artifactId>spring-context</artifactId>
            <version>5.3.6</version>
        </dependency>
        <dependency>
            <groupId>org.aspectj</groupId>
            <artifactId>aspectjweaver</artifactId>
            <version>1.9.4</version>
        </dependency>
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
            <groupId>com.alibaba</groupId>
            <artifactId>druid</artifactId>
            <version>1.1.10</version>
        </dependency>
        <dependency>
            <groupId>mysql</groupId>
            <artifactId>mysql-connector-java</artifactId>
            <version>5.1.46</version>
        </dependency>
    ```
  3. 在java下新建com.itheima.dao.AccountDao的接口，并写入out和in方法
    ```
      package com.itheima.dao;

      public interface AccountDao {
          public void out(String outMan,double money);
          public void in(String inMan, double money);
      }
    ```
  4. 在dao下新建impl.AccountDaoImpl的实现类，去实现out和in方法
    ```
      package com.itheima.dao.impl;

      import com.itheima.dao.AccountDao;
      import org.springframework.jdbc.core.JdbcTemplate;

      public class AccountDaoImpl implements AccountDao {
          private JdbcTemplate jt;

          public void setJt(JdbcTemplate jt) {
              this.jt = jt;
          }

          public void out(String outMan, double money) {
              jt.update("update account set money=money-? where name=?",money,outMan);
          }

          public void in(String inMan, double money) {
              jt.update("update account set money=money+? where name=?",money,inMan);
          }
      }
    ```
  5. 在java下新建com.itheima.domain.Account的实例类
    ```
      package com.itheima.domain;

      public class Account {
          private String name;
          private double money;

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
  6. 在java下新建com.itheima.service.AccountService的接口
    ```
      package com.itheima.service;
      public interface AccountService {
          public void transfer(String outMan,String inMan,double money);
      }
    ```
  7. 在service下新建impl.AccountServiceImpl的实现类
    ```
      package com.itheima.service.impl;
      import com.itheima.dao.AccountDao;
      import com.itheima.service.AccountService;
      public class AccountServiceImpl implements AccountService {
          private AccountDao ad;
          public void setAd(AccountDao ad) {
              this.ad = ad;
          }
          public void transfer(String outMan, String inMan, double money) {
              ad.out(outMan,money);
              ad.in(inMan,money);
          }
      }
    ```
  8. 在resources下新建applicationContext.xml
    * 添加aop和tx的命名空间
      - ![事务4](/images/spring/事务4.jpg)

    * 配置事务增强
      ```
        <!--配置平台事务管理器-->
        <bean id="transactionManager" class="org.springframework.jdbc.datasource.DataSourceTransactionManager">
            <property name="dataSource" ref="druidDataSource"></property>
        </bean>

        <!--通知  事务的增强-->
        <tx:advice id="txAdvice" transaction-manager="transactionManager">
            <!--设置事务的属性信息的-->
            <tx:attributes>
                <tx:method name="update*" isolation="REPEATABLE_READ" propagation="REQUIRED" read-only="true"/>
                <tx:method name="*"/>
            </tx:attributes>
        </tx:advice>
      ```

    * 配置事务 AOP 织入
      ```
        <!--配置事务的aop织入-->
        <aop:config>
            <aop:pointcut id="pointCut" expression="execution(* com.itheima.service.impl.*.*(..))"/>
            <aop:advisor advice-ref="txAdvice" pointcut-ref="pointCut"></aop:advisor>
        </aop:config>
      ```

    * 相关配置
      ```
        <bean id="druidDataSource" class="com.alibaba.druid.pool.DruidDataSource">
            <property name="driverClassName" value="com.mysql.jdbc.Driver"></property>
            <property name="url" value="jdbc:mysql://localhost:3306/test"></property>
            <property name="username" value="root"></property>
            <property name="password" value="root"></property>
        </bean>

        <!--配置jdbcTemplate-->
        <bean id="jdbcTemplate" class="org.springframework.jdbc.core.JdbcTemplate">
            <property name="dataSource" ref="druidDataSource"></property>
        </bean>

        <!--dao层-->
        <bean id="accountDao" class="com.itheima.dao.impl.AccountDaoImpl">
            <property name="jt" ref="jdbcTemplate"></property>
        </bean>

        <!--目标对象 内部的方法就是切点-->
        <bean id="accountService" class="com.itheima.service.impl.AccountServiceImpl">
            <property name="ad" ref="accountDao"></property>
        </bean>

        <!--配置平台事务管理器-->
        <bean id="transactionManager" class="org.springframework.jdbc.datasource.DataSourceTransactionManager">
            <property name="dataSource" ref="druidDataSource"></property>
        </bean>

        <!--通知  事务的增强-->
        <tx:advice id="txAdvice" transaction-manager="transactionManager">
            <!--设置事务的属性信息的-->
            <tx:attributes>
                <tx:method name="update*" isolation="REPEATABLE_READ" propagation="REQUIRED" read-only="true"/>
                <tx:method name="*"/>
            </tx:attributes>
        </tx:advice>

        <!--配置事务的aop织入-->
        <aop:config>
            <aop:pointcut id="pointCut" expression="execution(* com.itheima.service.impl.*.*(..))"/>
            <aop:advisor advice-ref="txAdvice" pointcut-ref="pointCut"></aop:advisor>
        </aop:config>
      ```

  9. 在java下新建com.itheima.controller.AccountController的测试方法
    ```
      package com.itheima.controller;
      import com.itheima.service.AccountService;
      import org.springframework.context.ApplicationContext;
      import org.springframework.context.support.ClassPathXmlApplicationContext;

      public class AccountController {
          public static void main(String[] args) {
              ApplicationContext app = new ClassPathXmlApplicationContext("applicationContext.xml");
              AccountService as = app.getBean(AccountService.class);
              as.transfer("zhangsan", "lisi", 300.5);
          }
      }
    ```
    
## 切点方法的事务参数的配置
```
  <!--通知  事务的增强-->
  <tx:advice id="txAdvice" transaction-manager="transactionManager">
      <!--设置事务的属性信息的-->
      <tx:attributes>
          <tx:method name="update*" isolation="REPEATABLE_READ" propagation="REQUIRED" read-only="true"/>
          <tx:method name="*"/>
      </tx:attributes>
  </tx:advice>
```
  * 其中，`<tx:method>`代表切点方法的事务参数的配置，例如：`<tx:method name="transfer" isolation="REPEATABLE_READ" propagation="REQUIRED" timeout="-1" read-only="false"/>`
    - name：切点方法名称
    - isolation:事务的隔离级别
    - propogation：事务的传播行为
    - timeout：超时时间
    - read-only：是否只读

# 基于注解的声明式事务控制
## 快速入门
  1. 新建itheima_spring_shiwu2的module,并配置webapp文件夹
  2. 在pom.xml中添加spring-context、druid、spring-tx、spring-jdbc、aspectjweaver、mysql-connector-java包
    ```
      <dependency>
            <groupId>org.springframework</groupId>
            <artifactId>spring-context</artifactId>
            <version>5.3.6</version>
        </dependency>
        <dependency>
            <groupId>org.aspectj</groupId>
            <artifactId>aspectjweaver</artifactId>
            <version>1.9.4</version>
        </dependency>
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
            <groupId>com.alibaba</groupId>
            <artifactId>druid</artifactId>
            <version>1.1.10</version>
        </dependency>
        <dependency>
            <groupId>mysql</groupId>
            <artifactId>mysql-connector-java</artifactId>
            <version>5.1.46</version>
        </dependency>
    ```
  3. 在java下，新建com.itheima.domain.Account的实例类
    ```
      package com.itheima.domain;
      public class Account {
        private String name;
        private double money;
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

  4. 在java下，新建com.itheima.dao.AccountDao的接口
    ```
      package com.itheima.dao;
      public interface AccountDao {
          public void out(String outMan, double money);
          public void in(String inMan, double money);
      }
    ```

  5. 在dao下新建impl.AccountDaoImpl的实现类,并使用@Repository和@Autowired注解
    ```
      package com.itheima.dao.impl;
      import com.itheima.dao.AccountDao;
      import org.springframework.beans.factory.annotation.Autowired;
      import org.springframework.jdbc.core.JdbcTemplate;
      import org.springframework.stereotype.Repository;

      @Repository("accountDaoImpl")
      public class AccountDaoImpl implements AccountDao {
          @Autowired
          private JdbcTemplate jt;
          public void out(String outMan, double money) {
              jt.update("update account set money=money-? where name=?",money,outMan);
          }
          public void in(String inMan, double money) {
              jt.update("update account set money=money+? where name=?",money,inMan);
          }
      }
    ```
  6. 在resources下新建applicationContext.xml，并context和tx的命名空间,添加组件扫描，和jdbcTemplate配置
    ```
      <?xml version="1.0" encoding="UTF-8"?>
      <beans xmlns="http://www.springframework.org/schema/beans"
            xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
            xmlns:context="http://www.springframework.org/schema/context"
            xmlns:tx="http://www.springframework.org/schema/tx"
            xsi:schemaLocation="
            http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd
            http://www.springframework.org/schema/context http://www.springframework.org/schema/context/spring-context.xsd
            http://www.springframework.org/schema/tx http://www.springframework.org/schema/tx/spring-tx.xsd
      ">
          <!--配置druid-->
          <bean id="druidDataSource" class="com.alibaba.druid.pool.DruidDataSource">
              <property name="driverClassName" value="com.mysql.jdbc.Driver"></property>
              <property name="url" value="jdbc:mysql://localhost:3306/test"></property>
              <property name="username" value="root"></property>
              <property name="password" value="root"></property>
          </bean>
          <!--添加jdbcTemplate-->
          <bean id="jdbc" class="org.springframework.jdbc.core.JdbcTemplate">
              <property name="dataSource" ref="druidDataSource"></property>
          </bean>
      </beans>
    ```
  7. 在java下新建com.itheima.service.AccountService的接口
    ```
      package com.itheima.service;
      public interface AccountService {
          public void transfer(String outMan,String inMan,double money);
      }
    ```
  8. 在service下，新建impl.AccountServiceImpl的实现类，去实现transfer方法，添加注解@Service、@Autowired、@Transactional
    ```
      package com.itheima.service.impl;
      import com.itheima.dao.AccountDao;
      import com.itheima.service.AccountService;
      import org.springframework.beans.factory.annotation.Autowired;
      import org.springframework.stereotype.Service;
      import org.springframework.transaction.annotation.Isolation;
      import org.springframework.transaction.annotation.Propagation;
      import org.springframework.transaction.annotation.Transactional;

      @Service("accountService")
      public class AccountServiceImpl implements AccountService {
          @Autowired
          private AccountDao ad;

          @Transactional(isolation = Isolation.READ_COMMITTED,propagation = Propagation.REQUIRED)
          public void transfer(String outMan, String inMan, double money) {
              ad.out(outMan, money);
              int id = 1/0;
              ad.in(inMan, money);
          }
      }
    ```
  9. 在applicationContext.xml中添加事物的注解驱动和配置平台事务管理器
    ```
      <!--配置事务管理器-->
      <bean id="tranfer" class="org.springframework.jdbc.datasource.DataSourceTransactionManager">
        <property name="dataSource" ref="druidDataSource"></property>
      </bean>
      <!--事物的注解驱动-->
      <tx:annotation-driven transaction-manager="tranfer"></tx:annotation-driven>
    ```
  10. 在java下新建com.itheima.controller.AccountController的测试类
    ```
      package com.itheima;
      import com.itheima.service.AccountService;
      import org.springframework.context.ApplicationContext;
      import org.springframework.context.support.ClassPathXmlApplicationContext;

      public class AccountController {
          public static void main(String[] args) {
              ApplicationContext app = new ClassPathXmlApplicationContext("applicationContext.xml");
              AccountService as = app.getBean(AccountService.class);
              as.transfer("zhangsan", "lisi", 1000);
          }
      }
    ```

## 注解配置声明式事务控制解析
  1. 使用 @Transactional 在需要进行事务控制的类或是方法上修饰，注解可用的属性同 xml 配置方式，例如隔离级别、传播行为等。
  2. 注解使用在类上，那么该类下的所有方法都使用同一套注解参数配置。
  3. 使用在方法上，不同的方法可以采用不同的事务参数配置。
  4. Xml配置文件中要开启事务的注解驱动`<tx:annotation-driven />`









