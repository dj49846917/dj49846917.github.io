---
title: ssm框架的整合
date: 2021-07-22 14:00:45
tags:
  - ssm
  - java
type: java                                                                         # 标签、分类和友情链接三個页面需要配置(必填)
description: SSM框架，是Spring + Spring MVC + MyBatis的缩写，这个是继SSH之后，目前比较主流的Java EE企业级框架，适用于搭建各种大型的企业级应用系统。                   # 描述
keywords: spring                                                                       # 关键词，便于搜索
top_img: /images/spring/ssm.jpg             # 文章的顶部图片
aside: true                                                                         # 展示文章侧边栏(默认为true)
categories: 
  - 教程
  - spring
  - ioc                                                                # 文章标签
  - java
cover: /images/spring/ssm.jpg                 # 文章的缩略图（用在首页）
---
# SSM框架整合
## 准备工作
### 原始方式整合
  1. 在mysql中创建ssm数据库和account表
    ```
      CREATE TABLE `account` (
        `id` int(11) NOT NULL AUTO_INCREMENT,
        `name` varchar(50) DEFAULT NULL,
        `money` double DEFAULT NULL,
        PRIMARY KEY (`id`)
      ) ENGINE=InnoDB DEFAULT CHARSET=utf8;
    ```

### 创建maven工程
  1. 新建itheima_ssm的project, 并配置webapp文件夹，
  2. 在java下创建com.itheima.controller、service、domain、mapper、utils文件夹
  3. 在webapp文件夹下创建index.jsp, save.jsp
  4. 项目结构如下：![ssm整合](/images/spring/ssm2.jpg)

### 导入坐标
  * 在pom.xml中导入响应的坐标
    ```
      <!--spring相关-->
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
          <groupId>org.springframework</groupId>
          <artifactId>spring-test</artifactId>
          <version>5.3.6</version>
      </dependency>
      <dependency>
          <groupId>org.springframework</groupId>
          <artifactId>spring-webmvc</artifactId>
          <version>5.3.6</version>
      </dependency>
      <!--servlet和jsp-->
      <dependency>
          <groupId>javax.servlet</groupId>
          <artifactId>servlet-api</artifactId>
          <version>2.5</version>
      </dependency>
      <dependency>
          <groupId>javax.servlet.jsp</groupId>
          <artifactId>jsp-api</artifactId>
          <version>2.2</version>
      </dependency>
      <!--mybatis相关-->
      <dependency>
          <groupId>org.mybatis</groupId>
          <artifactId>mybatis</artifactId>
          <version>3.5.6</version>
      </dependency>
      <dependency>
          <groupId>org.mybatis</groupId>
          <artifactId>mybatis-spring</artifactId>
          <version>2.0.6</version>
      </dependency>
      <dependency>
          <groupId>mysql</groupId>
          <artifactId>mysql-connector-java</artifactId>
          <version>5.1.46</version>
      </dependency>
      <dependency>
          <groupId>com.mchange</groupId>
          <artifactId>c3p0</artifactId>
          <version>0.9.5.2</version>
      </dependency>
      <dependency>
          <groupId>junit</groupId>
          <artifactId>junit</artifactId>
          <version>4.12</version>
      </dependency>
      <dependency>
          <groupId>jstl</groupId>
          <artifactId>jstl</artifactId>
          <version>1.2</version>
      </dependency>
    ```

### 编写实体类
  * 在domain下新建Account.java的实体类
    ```
      package com.itheima.domain;
      public class Account {
          private Integer id;
          private String name;
          private Double money;
          public Integer getId() {
              return id;
          }
          public void setId(Integer id) {
              this.id = id;
          }
          public String getName() {
              return name;
          }
          public void setName(String name) {
              this.name = name;
          }
          public Double getMoney() {
              return money;
          }
          public void setMoney(Double money) {
              this.money = money;
          }
          @Override
          public String toString() {
              return "Account{" +
                      "id=" + id +
                      ", name='" + name + '\'' +
                      ", money=" + money +
                      '}';
          }
      }
    ```

### 编写mapper接口
  * 在mapper下新建AccountMapper.java的接口
    ```
      package com.itheima.mapper;
      import com.itheima.domain.Account;
      import java.util.List;
      public interface AccountMapper {
          // 保存账户信息
          void save(Account account);
          // 查询账户信息
          List<Account> findAll();
      }
    ```

### 编写Service接口
  * 在service下新建AccountService的接口
    ```
      package com.itheima.service;
      import com.itheima.domain.Account;
      import java.util.List;
      public interface AccountService {
          void saveItem(Account account); // 保存账户信息
          List<Account> findAllList(); // 查询账户信息
      }
    ```
  * 在service下新建impl.AccountServiceImpl.java的实现类,去实现AccountService的方法
    ```
      package com.itheima.service.impl;
      import com.itheima.domain.Account;
      import com.itheima.service.AccountService;
      import java.util.List;
      public class AccountServiceImpl implements AccountService {
          public void saveItem(Account account) {
              
          }
          public List<Account> findAllList() {
              return null;
          }
      }
    ```

### 编写controller
  * 在controller下新建AccountController.java
    ```
      package com.itheima.controller;
      import com.itheima.domain.Account;
      import org.springframework.web.servlet.ModelAndView;
      public class AccountController {
          // 保存
          public String saveItem(Account account) {
              return null;
          }
          // 查询
          public ModelAndView findAllItems() {
              return null;
          }
      }
    ```

### 编写添加页面
  * 在webapp下新建save.jsp
    ```
      <%@ page contentType="text/html;charset=UTF-8" language="java" %>
      <html>
      <head>
          <title>Title</title>
      </head>
      <body>
          <h1>添加账户信息表单</h1>
          <form name="accountForm" method="post" action="${pageContext.request.contextPath}/account/saveItem">
              账户名称:<input type="text" name="name" /><br>
              账户金额:<input type="text" name="money" /><br>
              <input type="submit" value="保存" /><br>
          </form>
      </body>
      </html>
    ```

### 编写列表页面
  * 在webapp/WEB-INF下新建pages/accountList.jsp
    ```
      <%@ page contentType="text/html;charset=UTF-8" language="java" %>
      <html>
      <head>
          <title>Title</title>
      </head>
      <body>
          <h1>展示账户数据列表</h1>
          <table>
              <tr>
                  <th>账户id</th>
                  <th>账户名称</th>
                  <th>账户金额</th>
              </tr>
              <tr>
                  <td>1</td>
                  <td>zhangsan</td>
                  <td>5000</td>
              </tr>
          </table>
      </body>
      </html>
    ```

### 编写相应配置文件
  * Spring配置文件
    - 在resources下新建applicationContext.xml
      ```
        <?xml version="1.0" encoding="UTF-8"?>
        <beans xmlns="http://www.springframework.org/schema/beans"
              xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
              xmlns:context="http://www.springframework.org/schema/context"
              xmlns:aop="http://www.springframework.org/schema/aop"
              xmlns:tx="http://www.springframework.org/schema/tx"
              xsi:schemaLocation="
              http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd
              http://www.springframework.org/schema/context http://www.springframework.org/schema/context/spring-context.xsd
              http://www.springframework.org/schema/aop http://www.springframework.org/schema/aop/spring-aop.xsd
              http://www.springframework.org/schema/tx http://www.springframework.org/schema/tx/spring-tx.xsd
        ">
            
        </beans>
      ```
  * SpringMVC配置文件
    - 在resources下新建spring-mvc.xml
      ```
        <?xml version="1.0" encoding="UTF-8"?>
        <beans xmlns="http://www.springframework.org/schema/beans"
              xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
              xmlns:context="http://www.springframework.org/schema/context"
              xmlns:aop="http://www.springframework.org/schema/aop"
              xmlns:tx="http://www.springframework.org/schema/tx"
              xsi:schemaLocation="
              http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd
              http://www.springframework.org/schema/context http://www.springframework.org/schema/context/spring-context.xsd
              http://www.springframework.org/schema/aop http://www.springframework.org/schema/aop/spring-aop.xsd
              http://www.springframework.org/schema/tx http://www.springframework.org/schema/tx/spring-tx.xsd
        ">
            
        </beans>
      ```
  * Mybatis映射文件
    - 在resources下新建com/itheima/mapper/AccountMapper.xml
      ```
        <?xml version="1.0" encoding="UTF-8" ?>
        <!DOCTYPE mapper PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN" "http://mybatis.org/dtd/mybatis-3-mapper.dtd">
        <mapper>
            
        </mapper>
      ```
  * Mybatis核心文件
    - 在resource下新建sqlMapConfig.xml
      ```
        <?xml version="1.0" encoding="UTF-8" ?>
        <!DOCTYPE configuration PUBLIC "-//mybatis.org//DTD Config 3.0//EN" "http://mybatis.org/dtd/mybatis-3-config.dtd">
        <configuration>

        </configuration>
      ```
  * 数据库连接信息文件
    - 在resources下新建jdbc.properties
      ```
        jdbc.driver=com.mysql.jdbc.Driver
        jdbc.url=jdbc:mysql://localhost:3306/ssm
        jdbc.username=root
        jdbc.password=root
      ```
  * Web.xml文件
    - 修改resources下WEB-INF的web.xml文件
      ```
        <?xml version="1.0" encoding="UTF-8"?>
        <web-app xmlns="http://xmlns.jcp.org/xml/ns/javaee"
                xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
                xsi:schemaLocation="http://xmlns.jcp.org/xml/ns/javaee http://xmlns.jcp.org/xml/ns/javaee/web-app_4_0.xsd"
                version="4.0">
            <!--spring 监听器-->

            <!--springmvc的前端控制器-->

            <!--乱码过滤器-->
        </web-app>
      ```
  * 日志文件
    - 在resources下新建log4j.properties
      ```
        log4j.appender.stdout=org.apache.log4j.ConsoleAppender
        log4j.appender.stdout.Target=System.err
        log4j.appender.stdout.layout=org.apache.log4j.PatternLayout
        log4j.appender.stdout.layout.ConversionPattern=%d{ABSOLUTE} %5p %c{1}:%L - %m%n

        log4j.rootLogger=all, stdout
      ```

### mybatis配置文件内容填充
  1. 修改resources/com/itheima/mapper/AccountMapper.xml
    ```
      <?xml version="1.0" encoding="UTF-8" ?>
      <!DOCTYPE mapper PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN" "http://mybatis.org/dtd/mybatis-3-mapper.dtd">
      <mapper namespace="com.itheima.mapper.AccountMapper">
          <insert id="save" parameterType="account">
              insert into account values(#{id}, #{name}, #{money})
          </insert>
          
          <select id="findAll" resultType="account">
              select * from account
          </select>
      </mapper>
    ```
  2. 修改resources/sqlMapConfig.xml
    ```
      <?xml version="1.0" encoding="UTF-8" ?>
      <!DOCTYPE configuration PUBLIC "-//mybatis.org//DTD Config 3.0//EN" "http://mybatis.org/dtd/mybatis-3-config.dtd">
      <configuration>
          <!--加载properties文件-->
          <properties resource="jdbc.properties"></properties>

          <!--设置别名-->
          <typeAliases>
              <!--扫描包之后，别名就是domain的类名，首字母大小写都可以-->
              <package name="com.itheima.domain"/>
          </typeAliases>

          <!--环境-->
          <environments default="development">
              <environment id="development">
                  <transactionManager type="JDBC"></transactionManager>
                  <dataSource type="POOLED">
                      <property name="driver" value="${jdbc.driver}"/>
                      <property name="url" value="${jdbc.url}"/>
                      <property name="username" value="${jdbc.username}"/>
                      <property name="password" value="${jdbc.password}"/>
                  </dataSource>
              </environment>
          </environments>

          <!--加载映射-->
          <mappers>
              <!--<mapper resource="com/itheima/mapper/AccountMapper.xml"></mapper>-->
              <package name="com.itheima.mapper"/>
          </mappers>
      </configuration>
    ```
### spring和mvc配置文件内容填充
  1. 修改resources/applicationContext.xml
    ```
      <?xml version="1.0" encoding="UTF-8"?>
      <beans xmlns="http://www.springframework.org/schema/beans"
            xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
            xmlns:context="http://www.springframework.org/schema/context"
            xsi:schemaLocation="
            http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd
            http://www.springframework.org/schema/context http://www.springframework.org/schema/context/spring-context.xsd
      ">
          <!--组件扫描 扫描service和mapper-->
          <context:component-scan base-package="com.itheima">
              <!--排除controller的扫描-->
              <context:exclude-filter type="annotation" expression="org.springframework.stereotype.Controller"/>
          </context:component-scan>
      </beans>
    ```
  2. 修改resources/spring-mvc.xml
    ```
      <?xml version="1.0" encoding="UTF-8"?>
      <beans xmlns="http://www.springframework.org/schema/beans"
            xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
            xmlns:context="http://www.springframework.org/schema/context"
            xmlns:mvc="http://www.springframework.org/schema/mvc"
            xsi:schemaLocation="
                    http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd
                    http://www.springframework.org/schema/context http://www.springframework.org/schema/context/spring-context.xsd
                    http://www.springframework.org/schema/mvc http://www.springframework.org/schema/mvc/spring-mvc.xsd
              ">
          <!--组件扫描 主要扫描controller-->
          <context:component-scan base-package="com.itheima.controller"></context:component-scan>
          <!--配置mvc注解驱动-->
          <mvc:annotation-driven></mvc:annotation-driven>
          <!--内部资源视图解析器-->
          <bean class="org.springframework.web.servlet.view.InternalResourceViewResolver" id="resolver">
              <property name="suffix" value=".jsp"></property>
              <property name="prefix" value="/WEB-INF/pages/"></property>
          </bean>
          <!--开放静态资源访问权限-->
          <mvc:default-servlet-handler></mvc:default-servlet-handler>
      </beans>
    ```

### web.xml配置文件内容填充
  1. 修改webapp/WEB-INF/web.xml
    ```
      <?xml version="1.0" encoding="UTF-8"?>
      <web-app xmlns="http://xmlns.jcp.org/xml/ns/javaee"
              xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
              xsi:schemaLocation="http://xmlns.jcp.org/xml/ns/javaee http://xmlns.jcp.org/xml/ns/javaee/web-app_4_0.xsd"
              version="4.0">
          <!--spring 监听器-->
          <context-param>
              <param-name>contextConfigLocation</param-name>
              <param-value>classpath:applicationContext.xml</param-value>
          </context-param>
          <listener>
              <listener-class>org.springframework.web.context.ContextLoaderListener</listener-class>
          </listener>
          <!--springmvc的前端控制器-->
          <servlet>
              <servlet-name>DispatcherServlet</servlet-name>
              <servlet-class>org.springframework.web.servlet.DispatcherServlet</servlet-class>
              <init-param>
                  <param-name>contextConfigLocation</param-name>
                  <param-value>classpath:spring-mvc.xml</param-value>
              </init-param>
              <load-on-startup>1</load-on-startup>
          </servlet>
          <servlet-mapping>
              <servlet-name>DispatcherServlet</servlet-name>
              <url-pattern>/</url-pattern>
          </servlet-mapping>
          <!--乱码过滤器-->
          <filter>
              <filter-name>CharacterEncodingFilter</filter-name>
              <filter-class>org.springframework.web.filter.CharacterEncodingFilter</filter-class>
              <init-param>
                  <param-name>encoding</param-name>
                  <param-value>UTF-8</param-value>
              </init-param>
          </filter>
          <filter-mapping>
              <filter-name>CharacterEncodingFilter</filter-name>
              <url-pattern>/*</url-pattern>
          </filter-mapping>
      </web-app>
    ```

### 逻辑代码编写
  1. 修改main/java/com/itheima/service/impl/AccountServiceImpl.java
    ```
      package com.itheima.service.impl;
      import com.itheima.domain.Account;
      import com.itheima.mapper.AccountMapper;
      import com.itheima.service.AccountService;
      import org.apache.ibatis.io.Resources;
      import org.apache.ibatis.session.SqlSession;
      import org.apache.ibatis.session.SqlSessionFactory;
      import org.apache.ibatis.session.SqlSessionFactoryBuilder;
      import org.springframework.stereotype.Service;
      import java.io.IOException;
      import java.io.InputStream;
      import java.util.List;

      @Service("accountService")
      public class AccountServiceImpl implements AccountService {
          public void saveItem(Account account) {
              try {
                  InputStream resourceAsStream = Resources.getResourceAsStream("sqlMapConfig.xml");
                  SqlSessionFactory sqlSessionFactory = new SqlSessionFactoryBuilder().build(resourceAsStream);
                  SqlSession sqlSession = sqlSessionFactory.openSession(true);
                  AccountMapper mapper = sqlSession.getMapper(AccountMapper.class);
                  mapper.save(account);
                  sqlSession.close();
              } catch (IOException e) {
                  e.printStackTrace();
              }
          }
          public List<Account> findAllList() {
              try {
                  InputStream resourceAsStream = Resources.getResourceAsStream("sqlMapConfig.xml");
                  SqlSessionFactory sqlSessionFactory = new SqlSessionFactoryBuilder().build(resourceAsStream);
                  SqlSession sqlSession = sqlSessionFactory.openSession(true);
                  AccountMapper mapper = sqlSession.getMapper(AccountMapper.class);
                  List<Account> accountList = mapper.findAll();
                  sqlSession.close();
                  return accountList;
              } catch (IOException e) {
                  e.printStackTrace();
              }
              return null;
          }
      }
    ```
  2. 修改main/java/com/itheima/controller/AccountController.java
    ```
      package com.itheima.controller;
      import com.itheima.domain.Account;
      import com.itheima.service.AccountService;
      import org.springframework.beans.factory.annotation.Autowired;
      import org.springframework.stereotype.Controller;
      import org.springframework.web.bind.annotation.RequestMapping;
      import org.springframework.web.bind.annotation.ResponseBody;
      import org.springframework.web.servlet.ModelAndView;
      import java.util.List;

      @Controller
      @RequestMapping("/account")
      public class AccountController {
          @Autowired
          private AccountService accountService;
          // 保存
          // 不加produces, 保存成功这四个字会是乱码
          @RequestMapping(value = "/saveItem", produces = "text/html;charset=UTF-8")
          @ResponseBody
          public String saveItem(Account account) {
              accountService.saveItem(account);
              return "保存成功";
          }
          // 查询
          @RequestMapping("/findAll")
          public ModelAndView findAllItems() {
              List<Account> accountList = accountService.findAllList();
              ModelAndView modelAndView = new ModelAndView();
              modelAndView.addObject(accountList);
              modelAndView.setViewName("accountList");
              return modelAndView;
          }
      }
    ```
  3. 修改webapp/WEB-INF/pages/accountList.jsp
    ```
      <%@ page contentType="text/html;charset=UTF-8" language="java" %>
      <%@ taglib prefix="c" uri="http://java.sun.com/jsp/jstl/core" %>
      <html>
      <head>
          <title>Title</title>
      </head>
      <body>
          <h1>展示账户数据列表</h1>
          <table>
              <tr>
                  <th>账户id</th>
                  <th>账户名称</th>
                  <th>账户金额</th>
              </tr>
              <c:forEach items="${accountList}" var="item">
                  <tr>
                      <td>${item.id}</td>
                      <td>${item.name}</td>
                      <td>${item.money}</td>
                  </tr>
              </c:forEach>
          </table>
      </body>
      </html>
    ```
### 测试
  1. 启动tomcat，访问http://localhost:8080/account/findAll
  2. 访问http://localhost:8080/save.jsp

## Spring整合Mybatis
### 整合思路 
  * ![整合](/images/spring/ssm3.jpg)

### 将SqlSessionFactory配置到Spring容器中
  1. 修改resources/sqlMapConfig.xml
    * 修改前：
      ```
        <?xml version="1.0" encoding="UTF-8" ?>
        <!DOCTYPE configuration PUBLIC "-//mybatis.org//DTD Config 3.0//EN" "http://mybatis.org/dtd/mybatis-3-config.dtd">
        <configuration>
            <!--加载properties文件-->
            <properties resource="jdbc.properties"></properties>
            <!--设置别名-->
            <typeAliases>
                <!--扫描包之后，别名就是domain的类名，首字母大小写都可以-->
                <package name="com.itheima.domain"/>
            </typeAliases>
            <!--环境-->
            <environments default="development">
                <environment id="development">
                    <transactionManager type="JDBC"></transactionManager>
                    <dataSource type="POOLED">
                        <property name="driver" value="${jdbc.driver}"/>
                        <property name="url" value="${jdbc.url}"/>
                        <property name="username" value="${jdbc.username}"/>
                        <property name="password" value="${jdbc.password}"/>
                    </dataSource>
                </environment>
            </environments>
            <!--加载映射-->
            <mappers>
                <!--<mapper resource="com/itheima/mapper/AccountMapper.xml"></mapper>-->
                <package name="com.itheima.mapper"/>
            </mappers>
        </configuration>
      ```
    * 修改后：
      ```
        <?xml version="1.0" encoding="UTF-8" ?>
        <!DOCTYPE configuration PUBLIC "-//mybatis.org//DTD Config 3.0//EN" "http://mybatis.org/dtd/mybatis-3-config.dtd">
        <configuration>
            <!--设置别名-->
            <typeAliases>
                <!--扫描包之后，别名就是domain的类名，首字母大小写都可以-->
                <package name="com.itheima.domain"/>
            </typeAliases>
        </configuration>
      ```

  2. 修改resources/applicationContext.xml
    * 修改前：
      ```
        <?xml version="1.0" encoding="UTF-8"?>
        <beans xmlns="http://www.springframework.org/schema/beans"
              xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
              xmlns:context="http://www.springframework.org/schema/context"
              xsi:schemaLocation="
              http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd
              http://www.springframework.org/schema/context http://www.springframework.org/schema/context/spring-context.xsd
        ">
            <!--组件扫描 扫描service和mapper-->
            <context:component-scan base-package="com.itheima">
                <!--排除controller的扫描-->
                <context:exclude-filter type="annotation" expression="org.springframework.stereotype.Controller"/>
            </context:component-scan>
        </beans>
      ```
    * 修改后：
      ```
        <?xml version="1.0" encoding="UTF-8"?>
        <beans xmlns="http://www.springframework.org/schema/beans"
              xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
              xmlns:context="http://www.springframework.org/schema/context"
              xsi:schemaLocation="
              http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd
              http://www.springframework.org/schema/context http://www.springframework.org/schema/context/spring-context.xsd
        ">
            <!--组件扫描 扫描service和mapper-->
            <context:component-scan base-package="com.itheima">
                <!--排除controller的扫描-->
                <context:exclude-filter type="annotation" expression="org.springframework.stereotype.Controller"/>
            </context:component-scan>
            <!--加载properties文件-->
            <context:property-placeholder location="classpath:jdbc.properties"></context:property-placeholder>
            <!--配置数据源信息-->
            <bean id="dataSourceC3P0" class="com.mchange.v2.c3p0.ComboPooledDataSource">
                <property name="driverClass" value="${jdbc.driver}"></property>
                <property name="jdbcUrl" value="${jdbc.url}"></property>
                <property name="user" value="${jdbc.username}"></property>
                <property name="password" value="${jdbc.password}"></property>
            </bean>
            <!--配置sessionFactory-->
            <bean id="sqlSessionFactory" class="org.mybatis.spring.SqlSessionFactoryBean">
                <property name="dataSource" ref="dataSourceC3P0"></property>
                <!--加载mybatis核心文件-->
                <property name="configLocation" value="classpath:sqlMapConfig.xml"></property>
            </bean>
            <!--扫描mapper所在的包 为mapper创建实现类-->
            <bean class="org.mybatis.spring.mapper.MapperScannerConfigurer">
                <property name="basePackage" value="com.itheima.mapper"></property>
            </bean>
        </beans>
      ```
  3. 修改main/java/com/itheima/service/impl/AccountServiceImpl.java
    * 修改前：
      ```
        package com.itheima.service.impl;
        import com.itheima.domain.Account;
        import com.itheima.mapper.AccountMapper;
        import com.itheima.service.AccountService;
        import org.apache.ibatis.io.Resources;
        import org.apache.ibatis.session.SqlSession;
        import org.apache.ibatis.session.SqlSessionFactory;
        import org.apache.ibatis.session.SqlSessionFactoryBuilder;
        import org.springframework.stereotype.Service;
        import java.io.IOException;
        import java.io.InputStream;
        import java.util.List;
        @Service("accountService")
        public class AccountServiceImpl2 implements AccountService {
            public void saveItem(Account account) {
                try {
                    InputStream resourceAsStream = Resources.getResourceAsStream("sqlMapConfig.xml");
                    SqlSessionFactory sqlSessionFactory = new SqlSessionFactoryBuilder().build(resourceAsStream);
                    SqlSession sqlSession = sqlSessionFactory.openSession(true);
                    AccountMapper mapper = sqlSession.getMapper(AccountMapper.class);
                    mapper.save(account);
                    sqlSession.close();
                } catch (IOException e) {
                    e.printStackTrace();
                }
            }
            public List<Account> findAllList() {
                try {
                    InputStream resourceAsStream = Resources.getResourceAsStream("sqlMapConfig.xml");
                    SqlSessionFactory sqlSessionFactory = new SqlSessionFactoryBuilder().build(resourceAsStream);
                    SqlSession sqlSession = sqlSessionFactory.openSession(true);
                    AccountMapper mapper = sqlSession.getMapper(AccountMapper.class);
                    List<Account> accountList = mapper.findAll();
                    sqlSession.close();
                    return accountList;
                } catch (IOException e) {
                    e.printStackTrace();
                }
                return null;
            }
        }
      ```
    * 修改后：
      ```
        package com.itheima.service.impl;
        import com.itheima.domain.Account;
        import com.itheima.mapper.AccountMapper;
        import com.itheima.service.AccountService;
        import org.springframework.beans.factory.annotation.Autowired;
        import org.springframework.stereotype.Service;
        import java.util.List;
        @Service("accountService")
        public class AccountServiceImpl implements AccountService {
            @Autowired
            private AccountMapper accountMapper;
            public void saveItem(Account account) {
                accountMapper.save(account);
            }
            public List<Account> findAllList() {
                return accountMapper.findAll();
            }
        }
      ```

### 添加声明式事务(aop)
  * 修改applicationContext.xml
    ```
      <?xml version="1.0" encoding="UTF-8"?>
      <beans xmlns="http://www.springframework.org/schema/beans"
            xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
            xmlns:context="http://www.springframework.org/schema/context"
            xmlns:tx="http://www.springframework.org/schema/tx"
            xmlns:aop="http://www.springframework.org/schema/aop"
            xsi:schemaLocation="
            http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd
            http://www.springframework.org/schema/context http://www.springframework.org/schema/context/spring-context.xsd
            http://www.springframework.org/schema/tx http://www.springframework.org/schema/tx/spring-tx.xsd
            http://www.springframework.org/schema/aop http://www.springframework.org/schema/aop/spring-aop.xsd
      ">
          <!--组件扫描 扫描service和mapper-->
          <context:component-scan base-package="com.itheima">
              <!--排除controller的扫描-->
              <context:exclude-filter type="annotation" expression="org.springframework.stereotype.Controller"/>
          </context:component-scan>
          <!--加载properties文件-->
          <context:property-placeholder location="classpath:jdbc.properties"></context:property-placeholder>
          <!--配置数据源信息-->
          <bean id="dataSourceC3P0" class="com.mchange.v2.c3p0.ComboPooledDataSource">
              <property name="driverClass" value="${jdbc.driver}"></property>
              <property name="jdbcUrl" value="${jdbc.url}"></property>
              <property name="user" value="${jdbc.username}"></property>
              <property name="password" value="${jdbc.password}"></property>
          </bean>
          <!--配置sessionFactory-->
          <bean id="sqlSessionFactory" class="org.mybatis.spring.SqlSessionFactoryBean">
              <property name="dataSource" ref="dataSourceC3P0"></property>
              <!--加载mybatis核心文件-->
              <property name="configLocation" value="classpath:sqlMapConfig.xml"></property>
          </bean>
          <!--扫描mapper所在的包 为mapper创建实现类-->
          <bean class="org.mybatis.spring.mapper.MapperScannerConfigurer">
              <property name="basePackage" value="com.itheima.mapper"></property>
          </bean>
          <!--平台事务管理器-->
          <bean class="org.springframework.jdbc.datasource.DataSourceTransactionManager" id="transactionManager">
              <property name="dataSource" ref="dataSourceC3P0"></property>
          </bean>
          <!--配置事务增强-->
          <tx:advice id="advice">
              <tx:attributes>
                  <tx:method name="*" />
              </tx:attributes>
          </tx:advice>
          <!--事务的aop织入-->
          <aop:config>
              <aop:advisor advice-ref="advice" pointcut="execution(* com.itheima.service.impl.*.*(..))"></aop:advisor>
          </aop:config>
      </beans>
    ```

### 测试
  1. 启动tomcat，访问http://localhost:8080/account/findAll
  2. 访问http://localhost:8080/save.jsp