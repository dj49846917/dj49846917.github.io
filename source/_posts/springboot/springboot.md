---
title: springboot
date: 2021-05-06 09:07:44
tags: 
  - springboot
  - java
type: java                                                                         # 标签、分类和友情链接三個页面需要配置(必填)
description: pring Boot 是 Spring 家族中的一个全新的框架，用来简化 Spring 应用程序的创建和开发过程                   # 描述
keywords: springboot                                                                       # 关键词，便于搜索
top_img: https://ss0.bdstatic.com/70cFuHSh_Q1YnxGkpoWK1HF6hhy/it/u=2589588367,2632593097&fm=26&gp=0.jpg             # 文章的顶部图片
aside: true                                                                         # 展示文章侧边栏(默认为true)
categories: 
  - 教程
  - springboot                                                                # 文章标签
  - java
cover: https://ss0.bdstatic.com/70cFuHSh_Q1YnxGkpoWK1HF6hhy/it/u=2589588367,2632593097&fm=26&gp=0.jpg                 # 文章的缩略图（用在首页）
---

# springboot学习所遇问题
## springboot项目中，右侧没有mavenproject
  * 输入命令：shift + ctrl + A, 在输入Maven，选择ADD 

## 设置maven镜像
  * 在settings -> Build,Execution,Deployment -> Maven, 找到Maven home directory,打开该目录
  * 找到conf/settings.xml, 在mirrors下写入以下内容：
    ```
    <mirrors>
      <mirror>
        <id>nexus-aliyun</id>
        <mirrorOf>*</mirrorOf>
        <name>Nexus aliyun</name>
        <url>http://maven.aliyun.com/nexus/content/groups/public</url>
      </mirror>
    </mirrors>
    ```

## 更新jar包的最新版本
  ```
    <dependency>
			<groupId>javax.servlet.jsp</groupId>
			<artifactId>javax.servlet.jsp-api</artifactId>
			<version>[2.3.1,)</version>
		</dependency>
  ```
  * 以上注意version里的[2.3.1,)表示取2.3.1以上最新版本。当我这样写version之后，我的selenium框架里的jar包就会自动升级了，现在他们自己变成2.3.1版本了。

# 第1章Spring Boot 框架入门
## Spring Boot 简介
  * Spring Boot 是 Spring 家族中的一个全新的框架，它用来简化 Spring 应用程序的创建和开发过程，也可以说 Spring Boot 能简化我们之前采用 SpringMVC + Spring + MyBatis 框架进行开发的过程。

## Spring Boot 的特性
  * 能够快速创建基于 Spring 的应用程序
  * 能够直接使用 java main 方法启动内嵌的 Tomcat 服务器运行 Spring Boot 程序，不需
  要部署 war 包文件
  * 提供约定的 starter POM 来简化 Maven 配置，让 Maven 的配置变得简单
  * 自动化配置，根据项目的 Maven 依赖配置， Spring boot 自动配置 Spring、 Spring mvc
  等
  * 提供了程序的健康检查等功能
  * 基本可以完全不使用 XML 配置文件，采用注解配置

## Spring Boot 四大核心
  1. 自动配置
  2. 起步依赖
  3. Actuator
  4. 命令行界面

# 第2章Spring Boot 入门案例
## 第一个 SpringBoot 项目
### 开发步骤
项目名称： 001-springboot-first
  1. 创建一个 Module，选择类型为 Spring Initializr 快速构建
    * ![创建module](/images/springboot/创建module.jpg)
  
  2. 设置 GAV 坐标及 pom 配置信息
    * ![配置pom](/images/springboot/创建module2.jpg)

  3. 选择 Spring Boot 版本及依赖
    * ![选择版本](/images/springboot/创建module3.jpg)

  4. 设置模块名称、 Content Root 路径及模块文件的目录
    * ![文件目类](/images/springboot/创建module4.jpg)

  5. 项目创建完毕，如下
    * ![项目展示](/images/springboot/创建module5.jpg)

  6. 项目结构
    * static： 存放静态资源，如图片、 CSS、 JavaScript 等
    * templates：存放 Web 页面的模板文件
    * application.properties/application.yml 用于存放程序的各种依赖模块的配置信息，比如 服务端口，数据库连接配置等

## 入门案例
  1. 创建一个 Spring MVC 的 IndexController
    * ![入门案例](/images/springboot/入门案例.jpg)

  2. 在 IDEA 中右键，运行 Application 类中的 main 方法
    * 通过在控制台的输出，可以看到启动 SpringBoot 框架，会启动一个内嵌的 tomcat，端口号为 8080，上下文根为空
      - ![入门案例](/images/springboot/入门案例2.jpg)

    * 在浏览器中输入 http://localhost:8080/hello/world 访问
      - ![入门案例](/images/springboot/入门案例3.jpg)

## Spring Boot 的核心配置文件
  * Spring Boot 的核心配置文件用于配置 Spring Boot 程序，名字必须以 application 开始
### 核心配置格式
  1. .properties 文件（默认采用该文件）
    * 通过修改 application.properties 配置文件，在修改默认 tomcat 端口号及项目上下文件根键值对的 properties 属性文件配置方式
      - ![修改配置文件](/images/springboot/修改配置文件.jpg)

    * 配置完毕之后，启动浏览器测试
      - ![修改配置文件](/images/springboot/修改配置文件2.jpg)

  2. .yml 文件
    * yml 是一种 yaml 格式的配置文件，主要采用一定的空格、换行等格式排版进行配置。
    * yaml 是一种直观的能够被计算机识别的的数据序列化格式，容易被人类阅读， yaml 类似于 xml，但是语法比 xml 简洁很多，值与前面的冒号配置项必须要有一个空格， yml 后缀也可以使用 yaml 后缀
      * ![修改配置文件](/images/springboot/修改配置文件3.jpg)

    * 注意：当两种格式配置文件同时存在，使用的是.properties 配置文件

### 多环境配置
  * 为每个环境创建一个配置文件，命名必须以 application-环境标识.properties|yml, 启动即可
    - ![多环境](/images/springboot/多环境.jpg)
    
  * 在application.properties中：
    ```
      # 激活对应的环境
      spring.profiles.active=prod
    ```
  
  * 在application-dev.properties中：
    ```
      server.port=8082
      server.servlet.context-path=/app

      school.name=萱花中学
    ```
  
  * 在application-prod.properties中：
    ```
      server.port=8083
      server.servlet.context-path=/app

      school.name=永川中学
    ```

### Spring Boot 自定义配置
  * @Value 注解
    1. 在核心配置文件 applicatin.properties 中，添加两个自定义配置项 school.name。在 IDEA 中可以看到这个属性不能被 SpringBoot 识别，背景是桔色的
      - ![自定义配置](/images/springboot/自定义配置.jpg)
  
    2. 在 IndexController 中定义属性，并使用@Value 注解或者自定义配置值，并对其方法进行测试
      ```
        @Controller
        public class IndexController {
            @Value("${school.location}")
            public String location;

            @RequestMapping(value = "/say")

            public @ResponseBody String say() {
                return "hello,world:" + school.getName() + "and" + location;
            }
        }
      ```

    3. 重新运行 Application，在浏览器中进行测试
      - ![自定义配置](/images/springboot/自定义配置2.jpg)

  * @ConfigurationProperties
    - 将整个文件映射成一个对象，用于自定义配置项比较多的情况
      1. 在 com.dj.app.web 包下创建 School 类，并为该类加上 Component 和 ConfigurationProperties 注解，并在 ConfigurationProperties 注解中添加属性 prefix，作用可以区分同名配置
        ```
          @Component
          @ConfigurationProperties(prefix = "school")
          public class School {
            private String name;
            public String getName() {
                return name;
            }
            public void setName(String name) {
                this.name = name;
            }
          }
        ```
      2. application.properties 配置文件
        ``` 
          school.name=永川中学
          school.location=456
        ```
      3. 在 IndexController 中注入 School 配置类
        ```
          @Autowired
          private School school;
        ```
      4. 修改 IndexController 类中的测试方法
        ```
          public @ResponseBody String say() {
            return "hello,world:" + school.getName() + "and" + location;
          }
        ```
      5. 运行测试
        ![自定义配置](/images/springboot/自定义配置2.jpg)

  * 警告解决
    {% note warning %}
    在 School 类中使用了 ConfigurationProperties 注解后， IDEA 会出现一个警告，不影响程序的执行
    * ![自定义配置](/images/springboot/自定义配置3.jpg)
    * 点击 open documentnation 跳转到网页，在网页中提示需要加一个依赖，我们将这个依赖拷贝，粘贴到 pom.xml 文件中
      ```
        <dependency>
          <groupId>org.springframework.boot</groupId>
          <artifactId>spring-boot-configuration-processor</artifactId>
          <optional>true</optional>
        </dependency>
      ```
    {% endnote %}

  * 中文乱码
    {% note warning %}
      如果在 SpringBoot 核心配置文件中有中文信息，会出现乱码：
      * 一般在配置文件中，不建议出现中文（注释除外）
      * 如果有，可以先转化为 ASCII 码，在Settings -> Editor -> File Encodings
        - ![自定义配置](/images/springboot/自定义配置4.jpg)
    {% endnote %}

  * 友情提示
    {% note warning %}
      大家如果是从其它地方拷贝的配置文件，一定要将里面的空格删干净
    {% endnote %}

# 第3章Spring Boot集成jsp
  1. 在 pom.xml 文件中配置以下依赖项
    ```
      <!--引入 Spring Boot 内嵌的 Tomcat 对 JSP 的解析包，不加解析不了 jsp 页面-->
      <!--如果只是使用 JSP 页面，可以只添加该依赖-->
      <dependency>
          <groupId>org.apache.tomcat.embed</groupId>
          <artifactId>tomcat-embed-jasper</artifactId>
      </dependency>

      <!--如果要使用 servlet 必须添加该以下两个依赖-->
      <!-- servlet 依赖的 jar 包-->
      <dependency>
          <groupId>javax.servlet</groupId>
          <artifactId>javax.servlet-api</artifactId>
      </dependency>

      <dependency>
          <groupId>javax.servlet.jsp</groupId>
          <artifactId>javax.servlet.jsp-api</artifactId>
          <version>2.3.1</version>
      </dependency>

      <!--如果使用 JSTL 必须添加该依赖-->
      <!--jstl 标签依赖的 jar 包 start-->
      <dependency>
          <groupId>javax.servlet</groupId>
          <artifactId>jstl</artifactId>
      </dependency>
    ```

  2. 在 pom.xml 的 build 标签中要配置以下信息
    * SpringBoot 要求 jsp 文件必须编译到指定的 META-INF/resources 目录下才能访问，否则访问不到。
      ```
        <!--SpringBoot 要求 jsp 文件必须编译到指定的 META-INF/resources 目录下才能访问，否则访问不到。其它官方已经建议使用模版技术（后面会课程会单独讲解模版技术）-->
          <resources>
              <resource>
                  <!--源文件位置-->
                  <directory>src/main/webapp</directory>

                  <!--指定编译到 META-INF/resources，该目录不能随便写-->
                  <targetPath>META-INF/resources</targetPath>

                  <!--指定要把哪些文件编译进去， **表示 webapp 目录及子目录， *.*表示所有文件-->
                  <includes>
                      <include>**/*.*</include>
                  </includes>
              </resource>
          </resources>
      ```

  3. 在application.properties文件配置 Spring MVC 的视图展示为jsp，这里相当于 Spring MVC 的配置
    ```
      #配置 SpringMVC 视图解析器
      #其中： / 表示目录为 src/main/webapp
      spring.mvc.view.prefix=/
      spring.mvc.view.suffix=.jsp
    ```

  4. 在 com.dj.app.web 包下创建 IndexController 类，并编写代码
    ```
      @RequestMapping(value="/abc")
      public String index(Model model) {
          model.addAttribute("age", "18");
          return "index";
      }
    ```

  5. 在 src/main 下创建一个 webapp 目录，然后在该目录下新建index.jsp 页面
    * 如果在webapp目录下右键，没有创建jsp的选项，可以在Project Structure中指定webapp为 Web Resource Directory
      - 快捷键Shift + Ctrl + A, 搜索Project Structure,点开
      - ![集成jsp](/images/springboot/集成jsp.jpg)
      - ![集成jsp](/images/springboot/集成jsp2.jpg)

  6. 启动
      - ![展示](/images/springboot/集成jsp3.jpg)

# 第4章springboot集成mybaties
## 数据库准备
  1. 下载mysql数据库，并安装
  2. 下载Navcat，并安装
  3. 使用navcat连接mysql
    * ![连接mysql](/images/springboot/navcat连接mysql.jpg)
    * ![连接mysql](/images/springboot/navcat连接mysql2.jpg)
  4. 右键点击新建数据库springboot
    * ![连接mysql](/images/springboot/navcat连接mysql3.jpg)
  5. 右键点击springboot表，选择运行SQL文件，将准备好的sql文件引入，文件放在images/springboot文件夹下
    * ![连接mysql](/images/springboot/navcat连接mysql4.jpg)
    * 文件内容为：
      ```
        drop table if exists t_student;
        create table t_student 
        (
          id                   int(10)                        not null auto_increment,
          name                 varchar(20)                    null,
          age                  int(10)                        null,
          constraint PK_T_STUDENT primary key clustered (id)
        );

        insert into t_student(name,age) values("zhangsan",25);
        insert into t_student(name,age) values("lisi",28);
        insert into t_student(name,age) values("wangwu",23);
        insert into t_student(name,age) values("Tom",21);
        insert into t_student(name,age) values("Jck",55);
        insert into t_student(name,age) values("Lucy",27);
        insert into t_student(name,age) values("zhaoliu",75);
      ```

## 配置pom.xml中的相关jar依赖
  * 在pom.xml中，添加mysql
    ```
      <!--mybatis依赖-->
      <dependency>
          <groupId>org.mybatis.spring.boot</groupId>
          <artifactId>mybatis-spring-boot-starter</artifactId>
          <version>1.3.2</version>
      </dependency>
      <!--mysql驱动-->
      <dependency>
          <groupId>mysql</groupId>
          <artifactId>mysql-connector-java</artifactId>
      </dependency>
    ```
## 在application.yml中配置数据源
  ```
    server:
      # 配置内嵌 Tomcat 端口号
      port: 8081
      servlet:
        # 配置项目上下文根
        context-path: /app
    # 配置数据库的连接信息
    # 注意这里的驱动类有变化
    spring:
      datasource:
        driver-class-name: com.mysql.cj.jdbc.Driver
        url: jdbc:mysql://localhost:3306/springboot?useUnicode=t
          rue&characterEncoding=UTF-8&useJDBCCompliantTimezoneShift=true&useLegacyD
          atetimeCode=false&serverTimezone=GMT%2B8
        username: root
        password: root
  ```

## 代码开发
### 使用 Mybatis 反向工程生成接口、映射文件以及实体 bean
  1. 在根路径下新建GeneratorMapper.xml，并写入以下代码:
    ```
      <?xml version="1.0" encoding="UTF-8"?>
      <!DOCTYPE generatorConfiguration
              PUBLIC "-//mybatis.org//DTD MyBatis Generator Configuration 1.0//EN"
              "http://mybatis.org/dtd/mybatis-generator-config_1_0.dtd">
      <generatorConfiguration>
          <!-- 指定连接数据库的 JDBC 驱动包所在位置，指定到你本机的完整路径 -->
          <classPathEntry location="C:\Users\lip\.m2\repository\mysql\mysql-connector-java\5.1.38\mysql-connector-java-5.1.38.jar"/>
          <!-- 配置 table 表信息内容体， targetRuntime 指定采用 MyBatis3 的版本 -->
          <context id="tables" targetRuntime="MyBatis3">
              <!-- 抑制生成注释，由于生成的注释都是英文的，可以不让它生成 -->
              <commentGenerator>
                  <property name="suppressAllComments" value="true" />
              </commentGenerator>
              <!-- 配置数据库连接信息 -->
              <jdbcConnection driverClass="com.mysql.cj.jdbc.Driver"
                              connectionURL="jdbc:mysql://localhost:3306/springboot"
                              userId="root"
                              password="root">
              </jdbcConnection>
              <!-- 生成 model 类， targetPackage 指定 model 类的包名， targetProject 指定
              生成的 model 放在 eclipse 的哪个工程下面-->
              <javaModelGenerator targetPackage="com.dj.app.springboot.model"
                                  targetProject="src/main/java">
                  <property name="enableSubPackages" value="false" />
                  <property name="trimStrings" value="false" />
              </javaModelGenerator>
              <!-- 生成 MyBatis 的 Mapper.xml 文件， targetPackage 指定 mapper.xml 文件的
              包名， targetProject 指定生成的 mapper.xml 放在 eclipse 的哪个工程下面 -->
              <sqlMapGenerator targetPackage="com.dj.app.springboot.mapper"
                              targetProject="src/main/java">
                  <property name="enableSubPackages" value="false" />
              </sqlMapGenerator>
              <!-- 生成 MyBatis 的 Mapper 接口类文件,targetPackage 指定 Mapper 接口类的包
              名， targetProject 指定生成的 Mapper 接口放在 eclipse 的哪个工程下面 -->
              <javaClientGenerator type="XMLMAPPER"
                                  targetPackage="com.dj.app.springboot.mapper" targetProject="src/main/java">
                  <property name="enableSubPackages" value="false" />
              </javaClientGenerator>
              <!-- 数据库表名及对应的 Java 模型类名 -->
              <table tableName="t_student" domainObjectName="Student"
                    enableCountByExample="false"
                    enableUpdateByExample="false"
                    enableDeleteByExample="false"
              enableSelectByExample="false"
              selectByExampleQueryId="false"/>
          </context>
      </generatorConfiguration>
    ```
  {% note warning %}
    注意:
      ➢ 如果使用高版本， 驱动类变为： com.mysql.cj.jdbc.Driver
      ➢ url 后面应该加属性 nullCatalogMeansCurrent=true，否则生成有问题
      当前版本 MySQL 数据库为 5.5.60
  {% endnote %}

  2. 在pom.xml中，添加 mysql 反向工程依赖
    ```
      <!--mybatis 代码自动生成插件-->
      <plugin>
          <groupId>org.mybatis.generator</groupId>
          <artifactId>mybatis-generator-maven-plugin</artifactId>
          <version>1.3.6</version>
          <configuration>
              <!--配置文件的位置-->
              <configurationFile>GeneratorMapper.xml</configurationFile>
              <verbose>true</verbose>
              <overwrite>true</overwrite>
          </configuration>
      </plugin
    ```
  3. 在右侧Maven,找到项目中的Plugins/mybatis-generator/mybatis-generator:generate, 双击红色选中命令，生成相关文件
    * ![集成mybatis](/images/springboot/集成mybatis.jpg)
    * ![集成mybatis](/images/springboot/集成mybatis2.jpg)
    
### 在web包下创建 StudentController,并写入
  ```
    package com.dj.app.springboot.web;

    import com.dj.app.springboot.model.Student;
    import com.dj.app.springboot.service.StudentService;
    import org.springframework.beans.factory.annotation.Autowired;
    import org.springframework.stereotype.Controller;
    import org.springframework.web.bind.annotation.RequestMapping;
    import org.springframework.web.bind.annotation.ResponseBody;

    @Controller
    public class StudentController {
        // 注入业务层
        @Autowired
        private StudentService studentService;

        @RequestMapping(value="/student")
        public @ResponseBody Object student(Integer id) {
            Student student = studentService.queryStudentById(id);
            return student;
        }
    }
  ```

### 在 service 包下创建 service 接口并编写代码
  ```
    package com.dj.app.springboot.service;
    import com.dj.app.springboot.model.Student;
    public interface StudentService {
        /**
        * 根据学生ID查询详情
        * @param id
        * @return
        */
        Student queryStudentById(Integer id);
    }
  ```

### 在service.impl包下创建service接口并编写代码
  ```
    package com.dj.app.springboot.service.impl;

    import com.dj.app.springboot.mapper.StudentMapper;
    import com.dj.app.springboot.model.Student;
    import com.dj.app.springboot.service.StudentService;
    import org.springframework.beans.factory.annotation.Autowired;
    import org.springframework.stereotype.Service;

    @Service
    public class StudentServiceImpl implements StudentService {
        @Autowired
        private StudentMapper studentMapper;

        @Override
        public Student queryStudentById(Integer id) {
            Student student = studentMapper.selectByPrimaryKey(id);
            return student;
        }
    }
  ```
{% note warning %}
  注意：如果在 web 中导入 service 存在报错，可以尝试进行如下配置解决
  ![集成mybatis](/images/springboot/集成mybatis3.jpg)
  
  ➢ 在 Mybatis 反向工程生成的 StudentMapper 接口上加一个 Mapper 注解
  @Mapper 作用： mybatis 自动扫描数据持久层的映射文件及 DAO 接口的关系
    ```
      @Mapper
      public interface StudentMapper {
    ```

  ➢ 默认情况下， Mybatis 的 xml 映射文件不会编译到 target 的 class 目录下，所以我们需要在 pom.xml 文件中配置 resource
    ```
      <!--手动指定文件夹为resources-->
      <resources>
        <resource>
          <directory>src/main/java</directory>
          <includes>
            <include>**/*.xml</include>
          </includes>
        </resource>
      </resources>
    ```
{% endnote %}

### 启动 Application 应用，浏览器访问测试运行
  * ![集成mybatis](/images/springboot/集成mybatis4.jpg)
  
## DAO 其它开发方式
  1. 在运行的主类上添加注解包扫描@MapperScan("com.abc.springboot.mapper")
    * 注释掉 StudentMapper 接口上的@Mapper 注解
      ```
        // @Mapper // 扫描dao接口到spring容器
        public interface StudentMapper {
      ```

    * 在运行主类 Application 上加@MapperScan("com.abc.springboot.mapper")
      ```
        @MapperScan(basePackages = "com.dj.app.springboot.mapper")
        public class Application {
      ```

  2. 将接口和映射文件分开
    * 在resources目录下新建目录mapper存放映射文件，将StudentMapper.xml文件移到resources/mapper目录下
      * ![集成mybatis](/images/springboot/集成mybatis5.jpg)
    
    * 在 application.yml 配置文件中指定映射文件的位置，这个配置只有接口和映射文件不在同一个包的情况下，才需要指定
      ```
        mybatis:
          mapper-locations: classpath:mapper/*.xml
      ```
    
    * 这样，pom.xml中手动指定文件夹为resources就可以删掉了
      ```
        <!--手动指定文件夹为resources-->
        <resources>
          <resource>
            <directory>src/main/java</directory>
            <includes>
              <include>**/*.xml</include>
            </includes>
          </resource>
        </resources>
      ```
