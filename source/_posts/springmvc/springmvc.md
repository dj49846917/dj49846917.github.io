---
title: springmvc
date: 2021-06-22 15:34:50
tags:
  - spring
  - springmvc
  - java
type: java                                                                         # 标签、分类和友情链接三個页面需要配置(必填)
description: Spring是分层的JavaSE/EE应用full-stack轻量级开源框架                   # 描述
keywords: spring                                                                       # 关键词，便于搜索
top_img: /images/spring/logo.jpg             # 文章的顶部图片
aside: true                                                                         # 展示文章侧边栏(默认为true)
categories: 
  - 教程
  - spring
  - springmvc                                                                # 文章标签
  - java
cover: /images/spring/logo.jpg                 # 文章的缩略图（用在首页）
---

# Spring与Web环境集成
## 快速入门
  1. 创建一个itheima_spring_mvc的modules,并配置webapp文件夹
  2. 在pom.xml中导入spring-context坐标
    ```
      <dependency>
        <groupId>org.springframework</groupId>
        <artifactId>spring-context</artifactId>
        <version>5.3.6</version>
      </dependency>
    ```
  3. 在java下新建com.itheima.dao.UserDao的类
    ```
      package com.itheima.dao;
      public interface UserDao {
          public void save();
      }
    ```
  4. 在dao下新建UserDaoImpl的实现类，去实现save方法,添加@Repository注解
    ```
      package com.itheima.dao.impl;
      import com.itheima.dao.UserDao;
      import org.springframework.stereotype.Repository;

      @Repository("userDaoImpl")
      public class UserDaoImpl implements UserDao {
          public void save() {
              System.out.println("save running...");
          }
      }
    ```
  5. 在java下新建com.itheima.service.UserService
    ```
      package com.itheima.service;
      public interface UserService {
          public void search();
      }
    ```
  6. 在service下新建impl.UserServiceImpl的实现类，去实现search方法, 添加注解@Service和@Autowired
    ```
      package com.itheima.service.impl;
      import com.itheima.dao.UserDao;
      import com.itheima.service.UserService;
      import org.springframework.beans.factory.annotation.Autowired;
      import org.springframework.stereotype.Service;

      @Service("userServiceImpl")
      public class UserServiceImpl implements UserService {
          @Autowired
          private UserDao ud;
          public void search() {
              ud.save();
          }
      }
    ```
  7. 在resources下新建ApplicationContext.xml，配置组件扫描, 注意添加context命名空间
    ```
      <?xml version="1.0" encoding="UTF-8"?>
      <beans xmlns="http://www.springframework.org/schema/beans"
            xmlns:context="http://www.springframework.org/schema/context"
            xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
            xsi:schemaLocation="
            http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd
            http://www.springframework.org/schema/context http://www.springframework.org/schema/context/spring-context.xsd
      ">
          <context:component-scan base-package="com.itheima"></context:component-scan>
      </beans>
    ```
  8. 编写一个测试方法Test去测试save方法是否成功
    ```
      package com.itheima;
      import com.itheima.service.UserService;
      import org.springframework.context.ApplicationContext;
      import org.springframework.context.support.ClassPathXmlApplicationContext;

      public class Test {
          public static void main(String[] args) {
              ApplicationContext app = new ClassPathXmlApplicationContext("applicationContext.xml");
              UserService us = app.getBean(UserService.class);
              us.search();
          }
    ```

  9. 在pom.xml中添加javax.servlet-api、javax.servlet.jsp-api包
    ```
      <dependency>
        <groupId>javax.servlet</groupId>
        <artifactId>javax.servlet-api</artifactId>
        <version>4.0.1</version>
        <scope>provided</scope>
      </dependency>
      <dependency>
        <groupId>javax.servlet.jsp</groupId>
        <artifactId>javax.servlet.jsp-api</artifactId>
        <version>2.3.3</version>
        <scope>provided</scope>
      </dependency>
    ```
  10. 在java下，新建com.itheima.web.UserServlet
    ```
      package com.itheima.web;
      import com.itheima.service.UserService;
      import org.springframework.context.ApplicationContext;
      import org.springframework.context.support.ClassPathXmlApplicationContext;
      import javax.servlet.ServletException;
      import javax.servlet.http.HttpServlet;
      import javax.servlet.http.HttpServletRequest;
      import javax.servlet.http.HttpServletResponse;
      import java.io.IOException;

      public class UserServlet extends HttpServlet {
          @Override
          protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
              ApplicationContext app = new ClassPathXmlApplicationContext("applicationContext.xml");
              UserService us = app.getBean(UserService.class);
              us.search();
          }
      }
    ```
  11. 如果要启动servlet，还需要配置web.xml,路径在webapp/WEB-INF/web.xml

    ```
      <servlet>
        <servlet-name>UserServlet</servlet-name>
        <servlet-class>com.itheima.web.UserServlet</servlet-class>
      </servlet>
      <servlet-mapping>
        <servlet-name>UserServlet</servlet-name>
        <url-pattern>/userServlet</url-pattern>
      </servlet-mapping>
    ```
  12. 在webapp下添加index.jsp
    ```
      <html>
        <body>
          <h2>Hello World!</h2>
        </body>
      </html>
    ```
  13. 如果没有下载tomcat的，先去下载和配置tomcat
    1. 官网地址：https://tomcat.apache.org/
      * ![tomcat下载](/images/spring/tomcat下载.jpg)
    2. 配置环境变量
      * ![tomcat环境变量](/images/spring/tomcat.jpg)
      * ![tomcat环境变量](/images/spring/tomcat2.jpg)
    3. 找到你安装的tomcat/bin，点击startup.bat启动，输入localhost:8080看是否启动成功
    4. 在idea中，导入tomcat
      * 点击File => Settings => Build,Execution,Deployment => Application server, 点击加号，选择Tomcat Server, 在Tomcat Home中引入你下的tomcat路径点击确定，再点击Apply再点确定
      * ![tomcat](/images/spring/tomcat3.jpg)

  14. 启动tomcat
    * 点击idea右上角有个下拉框，选择Edit Configurations,如果左边没有tomcat就添加一个
      - ![启动tomcat](/images/spring/tomcat4.jpg)
      - ![启动tomcat](/images/spring/tomcat5.jpg)
    * 点击右边的加号，如果选项里面没有artifact，还需要添加artifact，在Files => Project Structure，点击Artifacts，点击加号，选择Web Application:Exploded
      - ![启动tomcat](/images/spring/tomcat6.jpg)
      - ![启动tomcat](/images/spring/tomcat7.jpg)
      - ![启动tomcat](/images/spring/tomcat8.jpg)
    * 添加好artifact后，再次点击idea右上角有个下拉框，选择Edit Configurations，点击加号选择artifact启动
      - ![启动tomcat](/images/spring/tomcat9.jpg)
  

## ApplicationContext应用上下文获取方式
  * 应用上下文对象是通过**new ClasspathXmlApplicationContext(spring配置文件)**方式获取的，但是每次从容器中获得Bean时都要编写**new ClasspathXmlApplicationContext(spring配置文件),**这样的弊端是配置文件加载多次，应用上下文对象创建多次。

  * 在Web项目中，可以使用**ServletContextListener**监听Web应用的启动，我们可以在Web应用启动时，就加载Spring的配置文件，创建应用上下文对象**ApplicationContext**，在将其存储到最大的域**servletContext**域中，这样就可以在任意位置从域中获得应用上下文**ApplicationContext**对象了。
  
  * 针对快速入门代码的优化：
    1. 在上面快速入门的9~~10之间，在com.itheima下创建listener.ContextLoaderLister,去实现ServletontextListener的方法
      ```
        package com.itheima.listener;
        import org.springframework.context.ApplicationContext;
        import org.springframework.context.support.ClassPathXmlApplicationContext;

        import javax.servlet.ServletContext;
        import javax.servlet.ServletContextEvent;
        import javax.servlet.ServletContextListener;

        public class ContextLoaderListener implements ServletContextListener {
            public void contextInitialized(ServletContextEvent sce) {
                ApplicationContext app = new ClassPathXmlApplicationContext("applicationContext.xml");
                // 将Spring的应用上下文对象存储到ServletContext域中
                ServletContext servletContext = sce.getServletContext();
                // 第一个参数随便写，第二个是ApplicationContext
                servletContext.setAttribute("application", app);
                System.out.println("spring容器创建完毕");
            }
            
            public void contextDestroyed(ServletContextEvent sce) {

            }
        }
      ```
    2. 要使用监听器需要在web.xml中配置
      ```
        <listener>
            <listener-class>com.itheima.listener.ContextLoaderListener</listener-class>
        </listener>
      ```
    3. 修改com.itheima.web.UserServlet的代码，通过获取ServletContext对象，来获取app对象
      * 修改前：
        ```
          package com.itheima.web;
          import com.itheima.service.UserService;
          import org.springframework.context.ApplicationContext;
          import org.springframework.context.support.ClassPathXmlApplicationContext;
          import javax.servlet.ServletException;
          import javax.servlet.http.HttpServlet;
          import javax.servlet.http.HttpServletRequest;
          import javax.servlet.http.HttpServletResponse;
          import java.io.IOException;

          public class UserServlet extends HttpServlet {
              @Override
              protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
                  super.doGet(req, resp);
                  ApplicationContext app = new ClassPathXmlApplicationContext("applicationContext.xml");
                  UserService us = app.getBean(UserService.class);
                  us.search();
              }
          }
        ```
      
      * 修改后：
        ```
          package com.itheima.web;
          import com.itheima.service.UserService;
          import org.springframework.context.ApplicationContext;
          import javax.servlet.ServletContext;
          import javax.servlet.ServletException;
          import javax.servlet.http.HttpServlet;
          import javax.servlet.http.HttpServletRequest;
          import javax.servlet.http.HttpServletResponse;
          import java.io.IOException;
          public class UserServlet extends HttpServlet {
              @Override
              protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
                  // 获取ServletContext，通过getAttribute获取ApplicationContext对象
                  ServletContext servletContext = this.getServletContext();
                  ApplicationContext ac = (ApplicationContext) servletContext.getAttribute("application");
                  UserService us = ac.getBean(UserService.class);
                  us.search();
              }
          }
        ```

  * 在刚刚写的代码中，第一步的`ApplicationContext app = new ClassPathXmlApplicationContext("applicationContext.xml");`和`servletContext.setAttribute("application", app);`是写死的，第三步的修改后`application`也是写死的，可以抽取出来
    1. 在web.xml中配置全局初始化参数
      ```
        <!--配置初始化参数application.xml-->
        <context-param>
          <!--名字随便取-->
          <param-name>applicationContext</param-name>
          <param-value>applicationContext.xml</param-value>
        </context-param>
      ```
    2. 在ContextLoaderListener中获取web.xml中配置的context-param
      ```
        // 获取servletContext对象
        ServletContext servletContext = sce.getServletContext();
        // 读取web.xml中的全局参数
        String applicationContext = servletContext.getInitParameter("applicationContext");
        ApplicationContext app = new ClassPathXmlApplicationContext(applicationContext);
      ```

    3. 在listener下创建WebApplicationContextUtils的类，目的是将ContextLoaderListener中`servletContext.setAttribute("application", app);`和UserServlet中`servletContext.getAttribute("application");`写死的"application"抽离出来
      ```
        package com.itheima.listener;
        import org.springframework.context.ApplicationContext;
        import javax.servlet.ServletContext;
        public class WebApplicationContextUtils {
            public static ApplicationContext getWebApplicationContext(ServletContext servletContext) {
                return (ApplicationContext) servletContext.getAttribute("application");
            }
        }
      ```
    4. 在UserServlet中,修改代码
      * 修改前：
        ```
          protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
            // 获取ServletContext，通过getAttribute获取ApplicationContext对象
            ServletContext servletContext = this.getServletContext();
            ApplicationContext ac = (ApplicationContext) servletContext.getAttribute("application");
            UserService us = ac.getBean(UserService.class);
            us.search();
          }
        ```
      * 修改后：
        ```
          protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
            // 获取ServletContext，通过getAttribute获取ApplicationContext对象
            ServletContext servletContext = this.getServletContext();

            // 通过WebApplicationContextUtils获取ApplicationContext对象
            ApplicationContext ac = WebApplicationContextUtils.getWebApplicationContext(servletContext);
            UserService us = ac.getBean(UserService.class);
            us.search();
          }
        ```

## Spring提供获取应用上下文的工具
  * 上面的分析不用手动实现，Spring提供了一个监听器ContextLoaderListener就是对上述功能的封装，该监听器内部加载Spring配置文件，创建应用上下文对象，并存储到ServletContext域中，提供了一个客户端工具WebApplicationContextUtils供使用者获得应用上下文对象。
  * 所以我们需要做的只有两件事：
    1. 在web.xml中配置ContextLoaderListener监听器（导入spring-web坐标）
    2. 使用WebApplicationContextUtils获得应用上下文对象ApplicationContext

### 完整代码编写
  1. 创建一个itheima_spring_mvc3的modules,并配置webapp文件夹
  2. 在pom.xml中导入spring-context坐标
    ```
      <dependency>
          <groupId>org.springframework</groupId>
          <artifactId>spring-context</artifactId>
          <version>5.3.7</version>
      </dependency>
      <dependency>
          <groupId>javax.servlet.jsp</groupId>
          <artifactId>javax.servlet.jsp-api</artifactId>
          <version>2.3.3</version>
          <scope>provided</scope>
      </dependency>
      <dependency>
          <groupId>javax.servlet</groupId>
          <artifactId>javax.servlet-api</artifactId>
          <version>4.0.1</version>
          <scope>provided</scope>
      </dependency>
      <dependency>
          <groupId>org.springframework</groupId>
          <artifactId>spring-web</artifactId>
          <version>5.3.7</version>
      </dependency>
    ```
  3. 在java下新建com.itheima.dao.UserDao的类
    ```
      package com.itheima.dao;
      public interface UserDao {
          public void save();
      }
    ```
  4. 在dao下新建UserDaoImpl的实现类，去实现save方法,添加@Repository注解
    ```
      package com.itheima.dao.impl;
      import com.itheima.dao.UserDao;
      import org.springframework.stereotype.Repository;

      @Repository("userDaoImpl")
      public class UserDaoImpl implements UserDao {
          public void save() {
              System.out.println("save running...");
          }
      }
    ```
  5. 在java下新建com.itheima.service.UserService
    ```
      package com.itheima.service;
      public interface UserService {
          public void search();
      }
    ```
  6. 在service下新建impl.UserServiceImpl的实现类，去实现search方法, 添加注解@Service和@Autowired
    ```
      package com.itheima.service.impl;
      import com.itheima.dao.UserDao;
      import com.itheima.service.UserService;
      import org.springframework.beans.factory.annotation.Autowired;
      import org.springframework.stereotype.Service;

      @Service("userServiceImpl")
      public class UserServiceImpl implements UserService {
          @Autowired
          private UserDao ud;
          public void search() {
              ud.save();
          }
      }
    ```
  7. 在resources下新建ApplicationContext.xml，配置组件扫描, 注意添加context命名空间
    ```
      <?xml version="1.0" encoding="UTF-8"?>
      <beans xmlns="http://www.springframework.org/schema/beans"
            xmlns:context="http://www.springframework.org/schema/context"
            xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
            xsi:schemaLocation="
            http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd
            http://www.springframework.org/schema/context http://www.springframework.org/schema/context/spring-context.xsd
      ">
          <context:component-scan base-package="com.itheima"></context:component-scan>
      </beans>
    ```
  8. 编写一个测试方法Test去测试save方法是否成功
    ```
      package com.itheima;
      import com.itheima.service.UserService;
      import org.springframework.context.ApplicationContext;
      import org.springframework.context.support.ClassPathXmlApplicationContext;

      public class Test {
          public static void main(String[] args) {
              ApplicationContext app = new ClassPathXmlApplicationContext("applicationContext.xml");
              UserService us = app.getBean(UserService.class);
              us.search();
          }
    ```

  9. 在web.xml中配置监听器和全局初始参数
    ``` 
      <?xml version="1.0" encoding="UTF-8"?>
      <web-app xmlns="http://xmlns.jcp.org/xml/ns/javaee"
              xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
              xsi:schemaLocation="http://xmlns.jcp.org/xml/ns/javaee http://xmlns.jcp.org/xml/ns/javaee/web-app_4_0.xsd"
              version="4.0">
          <!--配置全局初始参数-->
          <context-param>
              <param-name>contextConfigLocation</param-name>
              <param-value>classpath:applicationContext.xml</param-value>
          </context-param>

          <!--配置监听器-->
          <listener>
              <listener-class>org.springframework.web.context.ContextLoaderListener</listener-class>
          </listener>

          <servlet>
              <servlet-name>userServlet</servlet-name>
              <servlet-class>com.itheima.web.UserWeb</servlet-class>
          </servlet>
          <servlet-mapping>
              <servlet-name>userServlet</servlet-name>
              <url-pattern>/user</url-pattern>
          </servlet-mapping>
      </web-app>
    ```

  10. 在java下新建com.iheima.web.UserWeb的类，去继承httpservlet
    ```
      package com.itheima.web;
      import com.itheima.service.UserService;
      import org.springframework.web.context.WebApplicationContext;
      import org.springframework.web.context.support.WebApplicationContextUtils;
      import javax.servlet.ServletContext;
      import javax.servlet.ServletException;
      import javax.servlet.http.HttpServlet;
      import javax.servlet.http.HttpServletRequest;
      import javax.servlet.http.HttpServletResponse;
      import java.io.IOException;
      public class UserWeb extends HttpServlet {
          @Override
          protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
              ServletContext servletContext= this.getServletContext();
              WebApplicationContext app = WebApplicationContextUtils.getWebApplicationContext(servletContext);
              UserService us = app.getBean(UserService.class);
              us.search();
          }
      }
    ```
  11. 启动tomcat，访问/user路径

# SpringMVC的简介
## 代码准备，编写一个UserDao和UserService
  1. 创建一个itheima_spring_mvc4的modules,并配置webapp文件夹
  2. 在pom.xml中导入spring-context坐标
    ```
      <dependency>
          <groupId>org.springframework</groupId>
          <artifactId>spring-context</artifactId>
          <version>5.3.7</version>
      </dependency>
      <dependency>
          <groupId>javax.servlet.jsp</groupId>
          <artifactId>javax.servlet.jsp-api</artifactId>
          <version>2.3.3</version>
          <scope>provided</scope>
      </dependency>
      <dependency>
          <groupId>javax.servlet</groupId>
          <artifactId>javax.servlet-api</artifactId>
          <version>4.0.1</version>
          <scope>provided</scope>
      </dependency>
      <dependency>
          <groupId>org.springframework</groupId>
          <artifactId>spring-web</artifactId>
          <version>5.3.7</version>
      </dependency>
    ```
  3. 在java下新建com.itheima.dao.UserDao的类
    ```
      package com.itheima.dao;
      public interface UserDao {
          public void save();
      }
    ```
  4. 在dao下新建UserDaoImpl的实现类，去实现save方法,添加@Repository注解
    ```
      package com.itheima.dao.impl;
      import com.itheima.dao.UserDao;
      import org.springframework.stereotype.Repository;

      @Repository("userDaoImpl")
      public class UserDaoImpl implements UserDao {
          public void save() {
              System.out.println("save running...");
          }
      }
    ```
  5. 在java下新建com.itheima.service.UserService
    ```
      package com.itheima.service;
      public interface UserService {
          public void search();
      }
    ```
  6. 在service下新建impl.UserServiceImpl的实现类，去实现search方法, 添加注解@Service和@Autowired
    ```
      package com.itheima.service.impl;
      import com.itheima.dao.UserDao;
      import com.itheima.service.UserService;
      import org.springframework.beans.factory.annotation.Autowired;
      import org.springframework.stereotype.Service;

      @Service("userServiceImpl")
      public class UserServiceImpl implements UserService {
          @Autowired
          private UserDao ud;
          public void search() {
              ud.save();
          }
      }
    ```
  7. 在resources下新建ApplicationContext.xml，配置组件扫描, 注意添加context命名空间
    ```
      <?xml version="1.0" encoding="UTF-8"?>
      <beans xmlns="http://www.springframework.org/schema/beans"
            xmlns:context="http://www.springframework.org/schema/context"
            xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
            xsi:schemaLocation="
            http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd
            http://www.springframework.org/schema/context http://www.springframework.org/schema/context/spring-context.xsd
      ">
          <context:component-scan base-package="com.itheima.dao"></context:component-scan>
          <context:component-scan base-package="com.itheima.service"></context:component-scan>
      </beans>
    ```
  8. 编写一个测试方法Test去测试save方法是否成功
    ```
      package com.itheima;
      import com.itheima.service.UserService;
      import org.springframework.context.ApplicationContext;
      import org.springframework.context.support.ClassPathXmlApplicationContext;

      public class Test {
          public static void main(String[] args) {
              ApplicationContext app = new ClassPathXmlApplicationContext("applicationContext.xml");
              UserService us = app.getBean(UserService.class);
              us.search();
          }
    ```
## SpringMVC概述
  * SpringMVC 是一种基于 Java 的实现 MVC 设计模型的请求驱动类型的轻量级 Web 框架，属于SpringFrameWork 的后续产品，已经融合在 Spring Web Flow 中。
  * SpringMVC 已经成为目前最主流的MVC框架之一，并且随着Spring3.0 的发布，全面超越 Struts2，成为最优秀的 MVC 框架。它通过一套注解，让一个简单的 Java 类成为处理请求的控制器，而无须实现任何接口。同时它还支持 RESTful 编程风格的请求。

## SpringMVC快速入门
  * 需求：客户端发起请求，服务器端接收请求，执行逻辑并进行视图跳转。
  * 开发步骤：
    1. 导入SpringMVC相关坐标
    2. 配置SpringMVC核心控制器DispathcerServlet
    3. 创建Controller类和视图页面
    4. 使用注解配置Controller类中业务方法的映射地址
    5. 配置SpringMVC核心文件 spring-mvc.xml
    6. 客户端发起请求测试

### 导入SpringMVC相关坐标
  1. 在pom.xml中添加spring-mvc坐标
    ```
      <dependency>
        <groupId>org.springframework</groupId>
        <artifactId>spring-webmvc</artifactId>
        <version>5.3.7</version>
      </dependency>
    ```

### 在web.xml配置SpringMVC的核心控制器
  1. 在web.xml配置SpringMVC核心控制器DispathcerServlet
    ```
      <servlet>
        <servlet-name>DispatcherServlet</servlet-name>
        <servlet-class>org.springframework.web.servlet.DispatcherServlet</servlet-class>
        <!--服务器启动时加载-->
        <load-on-startup>1</load-on-startup>
      </servlet>
      <!--servlet映射地址-->
      <servlet-mapping>
        <servlet-name>DispatcherServlet</servlet-name>
        <url-pattern>/</url-pattern>
      </servlet-mapping>
    ```

### 创建Controller类和视图页面
  1. 在java下新建com.itheima.controller.UserController
    ```
      package com.itheima.controller;
      public class UserController {
        public String testController() {
          System.out.println("controller running...");
          // 返回的视图
          return "test.jsp";
        }
      }
    ```
  2. 在webapp文件夹下新建test.jsp
    ```
      <%@ page contentType="text/html;charset=UTF-8" language="java" %>
      <html>
      <head>
          <title>Title</title>
      </head>
      <body>
          <h2>我是测试页面</h2>
      </body>
      </html>
    ```

### 使用注解配置Controller类中业务方法的映射地址(@Controller和@RequestMapping)
  1. 在UserController中添加注解
    ```
      package com.itheima.controller;
      import org.springframework.stereotype.Controller;
      import org.springframework.web.bind.annotation.RequestMapping;
      @Controller
      public class UserController {
          @RequestMapping("/test")
          public String testController() {
              System.out.println("controller running...");
              // 返回的视图
              return "test.jsp";
          }
      }
    ```

### 配置SpringMVC核心文件 spring-mvc.xml
  1. 在resources下新建spring-mvc.xml，专门配置controller里面的注解
    ```
      <?xml version="1.0" encoding="UTF-8"?>
      <beans xmlns="http://www.springframework.org/schema/beans"
            xmlns:context="http://www.springframework.org/schema/context"
            xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
            xsi:schemaLocation="
            http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd
            http://www.springframework.org/schema/context http://www.springframework.org/schema/context/spring-context.xsd
      ">
          <!--controller的组件扫描-->
          <context:component-scan base-package="com.itheima.controller"></context:component-scan>
      </beans>
    ```
  2. 配置完之后需要在web.xml中的servlet加载
    * ![mvc](/images/spring/mvc.jpg)
    ```
      <?xml version="1.0" encoding="UTF-8"?>
      <web-app xmlns="http://xmlns.jcp.org/xml/ns/javaee"
              xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
              xsi:schemaLocation="http://xmlns.jcp.org/xml/ns/javaee http://xmlns.jcp.org/xml/ns/javaee/web-app_4_0.xsd"
              version="4.0">
          <!--配置SpringMVC的前端控制器-->
          <servlet>
              <servlet-name>DispatcherServlet</servlet-name>
              <servlet-class>org.springframework.web.servlet.DispatcherServlet</servlet-class>
              <init-param>
                  <param-name>contextConfigLocation</param-name>
                  <param-value>classpath:spring-mvc.xml</param-value>
              </init-param>
              <!--服务器启动时加载-->
              <load-on-startup>1</load-on-startup>
          </servlet>
          <!--servlet映射地址-->
          <servlet-mapping>
              <servlet-name>DispatcherServlet</servlet-name>
              <url-pattern>/</url-pattern>
          </servlet-mapping>
      </web-app>
    ```

### 客户端发起请求测试
  * 访问地址：localhost:8080/test
  * ![mvc](/images/spring/mvc2.jpg)

# SpringMVC的组件解析
## SpringMVC的执行流程
  1. 用户发送请求至前端控制器DispatcherServlet。
  2. DispatcherServlet收到请求调用HandlerMapping处理器映射器。
  3. 处理器映射器找到具体的处理器(可以根据xml配置、注解进行查找)，生成处理器对象及处理器拦截器(如果有则生成)一并返回给DispatcherServlet。
  4. DispatcherServlet调用HandlerAdapter处理器适配器。
  5. HandlerAdapter经过适配调用具体的处理器(Controller，也叫后端控制器)。
  6. Controller执行完成返回ModelAndView。
  7. HandlerAdapter将controller执行结果ModelAndView返回给DispatcherServlet。
  8. DispatcherServlet将ModelAndView传给ViewReslover视图解析器。
  9. ViewReslover解析后返回具体View。
  10. DispatcherServlet根据View进行渲染视图（即将模型数据填充至视图中）。DispatcherServlet响应用户。

## SpringMVC组件解析
  1. <div style="color: red">前端控制器：DispatcherServlet</div>
    * 用户请求到达前端控制器，它就相当于 MVC 模式中的 C，DispatcherServlet 是整个流程控制的中心，由它调用其它组件处理用户的请求，DispatcherServlet 的存在降低了组件之间的耦合性。
  2. <div style="color: red">处理器映射器：HandlerMapping</div>
    * HandlerMapping 负责根据用户请求找到 Handler 即处理器，SpringMVC 提供了不同的映射器实现不同的映射方式，例如：配置文件方式，实现接口方式，注解方式等。
  3. <div style="color: red">处理器适配器：HandlerAdapter</div>
    * 通过 HandlerAdapter 对处理器进行执行，这是适配器模式的应用，通过扩展适配器可以对更多类型的处理器进行执行。
  4. <div style="color: red">处理器：Handler</div>
    * 它就是我们开发中要编写的具体业务控制器。由 DispatcherServlet 把用户请求转发到 Handler。由Handler 对具体的用户请求进行处理。
  5. <div style="color: red">视图解析器：View Resolver</div>
    * View Resolver 负责将处理结果生成 View 视图，View Resolver 首先根据逻辑视图名解析成物理视图名，即具体的页面地址，再生成 View 视图对象，最后对 View 进行渲染将处理结果通过页面展示给用户。
  6. <div style="color: red">视图：View</div>
    * SpringMVC 框架提供了很多的 View 视图类型的支持，包括：jstlView、freemarkerView、pdfView等。最常用的视图就是 jsp。一般情况下需要通过页面标签或页面模版技术将模型数据通过页面展示给用户，需要由程序员根据业务需求开发具体的页面

## SpringMVC注解解析
### @RequestMapping
  * 作用：用于建立请求 URL 和处理请求方法之间的对应关系
  * 位置：
    - 类上，请求URL 的第一级访问目录。此处不写的话，就相当于应用的根目录
    - 方法上，请求 URL 的第二级访问目录，与类上的使用@ReqquestMapping标注的一级目录一起组成访问虚拟路径
  * 属性：
    - <div style="color: red">value：</div>用于指定请求的URL。它和path属性的作用是一样的
    - <div style="color: red">method：</div>用于指定请求的方式
    - <div style="color: red">params：</div>用于指定限制请求参数的条件。它支持简单的表达式。要求请求参数的key和value必须和配置的一模一样
  * 例如：
    - <div style="color: red">params = {"accountName"}，</div>表示请求参数必须有accountName
    - <div style="color: red">params = {"moeny!100"}，</div>表示请求参数中money不能是100
  
  * 测试：
    1. 在controller.UserController中：
      ```
        package com.itheima.controller;
        import org.springframework.stereotype.Controller;
        import org.springframework.web.bind.annotation.RequestMapping;
        import org.springframework.web.bind.annotation.RequestMethod;

        @Controller
        //相当于地址栏加了一层/user
        @RequestMapping("/user")
        public class UserController {
            // method表示请求类型，params表示访问该路径必须携带的参数，这里是username和password
            @RequestMapping(value = "/test", method = RequestMethod.GET, params = {"username", "password"})
            public String testController() {
                System.out.println("controller running...");
                // 返回的视图
                return "/test.jsp";
            }
        }
      ```
    2. 访问http://localhost:8080/user/test?username=zhangsan&password=123
      - * ![mvc](/images/spring/mvc2.jpg)

## 视图解析器
  * 我们可以通过属性注入的方式修改视图的的前后缀
    - ![mvc5](/images/spring/mvc5.jpg)
  * 举例：在webapp.WEB_INF下新建jsp文件夹，把test.jsp放到jsp文件夹中，如果不改前后缀，代码应这样写
    - ![mvc3](/images/spring/mvc3.jpg)
  * 在spring-mvc.xml中配置视图解析器
    ```
      <!--配置内部资源视图解析器-->
      <bean id="view" class="org.springframework.web.servlet.view.InternalResourceViewResolver">
          <!--配置前缀和后缀/jsp/test.jsp可以变成test-->
          <property name="prefix" value="/jsp/"></property>
          <property name="suffix" value=".jsp"></property>
      </bean>
    ``` 
  * 在controller.UserController中
    - ![mvc4](/images/spring/mvc4.jpg)








