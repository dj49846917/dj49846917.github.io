---
title: mybatis
date: 2021-07-15 16:32:56
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

# Mybatis简介
## 什么是Mybatis
  * mybatis 是一个优秀的基于java的持久层框架，它内部封装了jdbc，使开发者只需要关注sql语句本身，而不需要花费精力去处理加载驱动、创建连接、创建statement等繁杂的过程。
  * mybatis通过xml或注解的方式将要执行的各种 statement配置起来，并通过java对象和statement中sql的动态参数进行映射生成最终执行的sql语句。
  * 最后mybatis框架执行sql并将结果映射为java对象并返回。采用ORM思想解决了实体和数据库映射的问题，对jdbc 进行了封装，屏蔽了jdbc api 底层访问细节，使我们不用与jdbc api 打交道，就可以完成对数据库的持久化操作。

# MyBatis的快速入门
## MyBatis开发步骤
1. 添加MyBatis的坐标
2. 创建user数据表
3. 编写User实体类 
4. 编写映射文件UserMapper.xml
5. 编写核心文件SqlMapConfig.xml
6. 编写测试类

### 添加MyBatis的坐标
1. 创建一个itheima_mybatis_001的module,并配置webapp文件夹
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
  ```

### 创建user数据表
在mysql的test数据库中新建user表，添加username和password字段

### 编写User实体类
  * 在java下新建com.itheima.domain.User.java实体类
    ```
      package com.itheima.domain;
      public class User {
          private int id;
          private String username;
          private String password;
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
                      '}';
          }
      }
    ```

### 编写映射文件UserMapper.xml
  * 在resources下新建com/itheima/mapper/UserMapper.xml
    ```
      <?xml version="1.0" encoding="UTF-8" ?>
      <!DOCTYPE mapper PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN" "http://mybatis.org/dtd/mybatis-3-mapper.dtd">
      <mapper namespace="userMapper">
          <!--select查询语句, resultType: 查询的结果放到哪个位置-->
          <select id="findAll" resultType="com.itheima.domain.User">
              select * from user
          </select>
      </mapper>
    ```

### 编写核心文件SqlMapConfig.xml
  * 在resources下新建sqlMapConfig.xml
    ```
      <?xml version="1.0" encoding="UTF-8" ?>
      <!DOCTYPE configuration PUBLIC "-//mybatis.org//DTD Config 3.0//EN" "http://mybatis.org/dtd/mybatis-3-config.dtd">
      <configuration>
          <!--数据源环境-->
          <environments default="dev">
              <environment id="dev">
                  <transactionManager type="JDBC"></transactionManager>
                  <dataSource type="POOLED">
                      <property name="driver" value="com.mysql.jdbc.Driver"/>
                      <property name="url" value="jdbc:mysql://localhost:3306/test"/>
                      <property name="username" value="root"/>
                      <property name="password" value="root"/>
                  </dataSource>
              </environment>
          </environments>
      </configuration>
    ```

### 编写测试类
  * 在test/java下新建com.itheima.test.MyBatisTest的测试类，启动
    ```
      package com.itheima.test;
      import com.itheima.domain.User;
      import org.apache.ibatis.io.Resources;
      import org.apache.ibatis.session.SqlSession;
      import org.apache.ibatis.session.SqlSessionFactory;
      import org.apache.ibatis.session.SqlSessionFactoryBuilder;
      import org.junit.Test;
      import java.io.IOException;
      import java.io.InputStream;
      import java.util.List;
      public class MyBatisTest {
          @Test
          public void test1() throws IOException {
              // 获取核心配置文件
              InputStream resource = Resources.getResourceAsStream("sqlMapConfig.xml");
              // 获取sqlSession工厂对象
              SqlSessionFactory sqlSessionFactory = new SqlSessionFactoryBuilder().build(resource);
              // 获得sqlSession对象
              SqlSession sqlSession = sqlSessionFactory.openSession();
              // 执行sql语句
              List<User> userList = sqlSession.selectList("userMapper.findAll");
              //打印结果
              System.out.println(userList);
              //释放资源
              sqlSession.close();
          }
      }
    ```

# MyBatis的映射文件概述
![概述](/images/mybatis/概述.jpg)

# MyBatis映射文件深入
## 代码准备
1. 新建itheima_mybatis_003的module,并配置webapp文件夹
2. 在pom.xml中导入响应的坐标
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
3. 在mysql的test数据库中新建user表，添加username和password字段
4. 在java下新建com.itheima.domain.User.java实体类
  ```
    package com.itheima.domain;
    public class User {
        private int id;
        private String username;
        private String password;
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
                    '}';
        }
    }
  ```
5. 在resources下新建mapper/UserMapper.xml
  ```
    <?xml version="1.0" encoding="UTF-8" ?>
    <!DOCTYPE mapper PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN" "http://mybatis.org/dtd/mybatis-3-mapper.dtd">
    <mapper namespace=""></mapper>
  ```
6. 在resources下新建jdbc.properties
  ```
    jdbc.driver=com.mysql.jdbc.Driver
    jdbc.url=jdbc:mysql://localhost:3306/test
    jdbc.username=root
    jdbc.password=root
  ```
7. 在resources下新建log4j.properties
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
8. 在resources下新建sqlMapConfig.xml
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

## 动态sql语句
### 动态 SQL之`<if>`
我们根据实体类的不同取值，使用不同的 SQL语句来进行查询。比如在 id如果不为空时可以根据id查询，如果username 不同空时还要加入用户名作为条件。这种情况在我们的多条件组合查询中经常会碰到。
  1. 在java下创建com.itheima.mapper.UserMapper的接口
    ```
      package com.itheima.mapper;
      import com.itheima.domain.User;
      import java.util.List;
      public interface UserMapper {
          List<User> findByCondition(User user);
      }
    ```
  2. 在userMapper.xml中配置UserMapper接口
    ```
      <?xml version="1.0" encoding="UTF-8" ?>
      <!DOCTYPE mapper PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN" "http://mybatis.org/dtd/mybatis-3-mapper.dtd">
      <mapper namespace="com.itheima.mapper.UserMapper">
          <select id="findByCondition" resultType="user" parameterType="user">
              select * from user
              <where>
                  <if test="id!=0">
                      and id=#{id}
                  </if>
                  <if test="username!=null">
                      and username=#{username}
                  </if>
                  <if test="password!=null">
                      and password=#{password}
                  </if>
              </where>
          </select>
      </mapper>
    ```
  3. 在test下新建com.itheima.mapperTest的测试类
    ```
      package com.itheima;
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
      public class mapperTest {
          @Test
          public void userTest() throws IOException {
              InputStream resourceAsStream = Resources.getResourceAsStream("sqlMapConfig.xml");
              SqlSessionFactory sqlSessionFactory = new SqlSessionFactoryBuilder().build(resourceAsStream);
              SqlSession sqlSession = sqlSessionFactory.openSession();
              UserMapper mapper = sqlSession.getMapper(UserMapper.class);

              // 模拟条件user
              User condition = new User();
              condition.setId(1);
              // condition.setUsername("zhangsan");
              // condition.setPassword("123");

              List<User> userList = mapper.findByCondition(condition);
              System.out.println(userList);
          }
      }
    ```

### 动态SQL之`<foreach>`
  1. 在src/main/java/com/itheima/mapper/UserMapper.java，添加findByIds方法
    ```
      package com.itheima.mapper;
      import com.itheima.domain.User;
      import java.util.List;
      public interface UserMapper {
          List<User> findByCondition(User user);
          List<User> findByIds(List<Integer> ids);
      }
    ```
  2. 在resources/mapper/UserMapper.xml中配置findByIds
    ```
      <select id="findByIds" parameterType="list" resultType="user">
        select * from user
        <where>
          <foreach collection="list" open="id in(" close=")" item="id" separator=",">
            #{id}
          </foreach>
        </where>
      </select>
    ```
  3. 编写测试方法
    ```
      package com.itheima;
      import com.itheima.domain.User;
      import com.itheima.mapper.UserMapper;
      import org.apache.ibatis.io.Resources;
      import org.apache.ibatis.session.SqlSession;
      import org.apache.ibatis.session.SqlSessionFactory;
      import org.apache.ibatis.session.SqlSessionFactoryBuilder;
      import org.junit.Test;
      import java.io.IOException;
      import java.io.InputStream;
      import java.util.ArrayList;
      import java.util.List;

      public class mapperTest {
          @Test
          public void userTest2() throws IOException {
            InputStream resourceAsStream = Resources.getResourceAsStream("sqlMapConfig.xml");
            SqlSessionFactory sqlSessionFactory = new SqlSessionFactoryBuilder().build(resourceAsStream);
            SqlSession sqlSession = sqlSessionFactory.openSession();
            UserMapper mapper = sqlSession.getMapper(UserMapper.class);
            // 模拟ids的数据
            List<Integer> ids = new ArrayList<Integer>();
            ids.add(1);
            ids.add(4);
            List<User> byIds = mapper.findByIds(ids);
            System.out.println(byIds);
          }
      }
    ```

## SQL片段抽取
  * Sql 中可将重复的 sql 提取出来，使用时用 include 引用即可，最终达到 sql 重用的目的
  * 在UserMapper.xml中，可以把相同的select * from user提取出来
    - 提取前：
      ```
        <?xml version="1.0" encoding="UTF-8" ?>
        <!DOCTYPE mapper PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN" "http://mybatis.org/dtd/mybatis-3-mapper.dtd">
        <mapper namespace="com.itheima.mapper.UserMapper">
            <select id="findByCondition" resultType="user" parameterType="user">
                select * from user
                <where>
                    <if test="id!=0">
                        and id=#{id}
                    </if>
                    <if test="username!=null">
                        and username=#{username}
                    </if>
                    <if test="password!=null">
                        and password=#{password}
                    </if>
                </where>
            </select>

            <select id="findByIds" parameterType="list" resultType="user">
                select * from user
                <where>
                    <foreach collection="list" open="id in(" close=")" item="id" separator=",">
                        #{id}
                    </foreach>
                </where>
            </select>
        </mapper>
      ```
    - 提取后：
      ```
        <?xml version="1.0" encoding="UTF-8" ?>
        <!DOCTYPE mapper PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN" "http://mybatis.org/dtd/mybatis-3-mapper.dtd">
        <mapper namespace="com.itheima.mapper.UserMapper">
            <!--sql语句抽取-->
            <sql id="selectUser">select * from user</sql>
            <select id="findByCondition" resultType="user" parameterType="user">
                --引入sql
                <include refid="selectUser"></include>
                <where>
                    <if test="id!=0">
                        and id=#{id}
                    </if>
                    <if test="username!=null">
                        and username=#{username}
                    </if>
                    <if test="password!=null">
                        and password=#{password}
                    </if>
                </where>
            </select>
            <select id="findByIds" parameterType="list" resultType="user">
                <include refid="selectUser"></include>
                <where>
                    <foreach collection="list" open="id in(" close=")" item="id" separator=",">
                        #{id}
                    </foreach>
                </where>
            </select>
        </mapper>
      ```

## 知识小结
### MyBatis映射文件配置:
  ```
    <select>：查询
    <insert>：插入
    <update>：修改
    <delete>：删除
    <where>：where条件
    <if>：if判断
    <foreach>：循环
    <sql>：sql片段抽取
  ```

# MyBatis的增删改查操作
## MyBatis的插入数据操作
### 编写UserMapper映射文件
  ```
    <mapper namespace="userMapper">
      <insert id="add" parameterType="com.itheima.domain.User">
        insert into user values(#{id},#{username},#{password})
      </insert>
    </mapper>
  ```

### 编写插入实体User的代码
  * 在test/java/com/itheima/test下新建MyBatisAddTest测试类
    ```
      package com.itheima.test;
      import com.itheima.domain.User;
      import org.apache.ibatis.io.Resources;
      import org.apache.ibatis.session.SqlSession;
      import org.apache.ibatis.session.SqlSessionFactory;
      import org.apache.ibatis.session.SqlSessionFactoryBuilder;
      import org.junit.Test;
      import java.io.IOException;
      import java.io.InputStream;
      import java.util.List;
      public class MyBatisAddTest {
          @Test
          public void test1() throws IOException {
              // 模拟user对象
              User user = new User();
              user.setUsername("wangmazi");
              user.setPassword("123");

              // 获取核心配置文件
              InputStream resource = Resources.getResourceAsStream("sqlMapConfig.xml");
              // 获取sqlSession工厂对象
              SqlSessionFactory sqlSessionFactory = new SqlSessionFactoryBuilder().build(resource);
              // 获得sqlSession对象
              SqlSession sqlSession = sqlSessionFactory.openSession();
              // 执行sql语句
              int insert = sqlSession.insert("userMapper.add", user);
              System.out.println(insert);
              // mybatis执行更新操作 提交事务
              sqlSession.commit();
              //释放资源
              sqlSession.close();
          }
      }
    ```

### 插入操作注意问题
  1. 插入语句使用insert标签
  2. 在映射文件中使用parameterType属性指定要插入的数据类型
  3. Sql语句中使用#{实体属性名}方式引用实体中的属性值
  4. 插入操作使用的API是sqlSession.insert(“命名空间.id”,实体对象);
  5. 插入操作涉及数据库数据变化，所以要使用sqlSession对象显示的提交事务，即sqlSession.commit() 

## MyBatis的修改数据操作
### 编写UserMapper映射文件
  ```
    <mapper namespace="userMapper">
      <update id="update" parameterType="com.itheima.domain.User">
        update user set username=#{username},password=#{password} where id=#{id}
      </update>
    </mapper>
  ```
### 编写修改实体User的代码
  * 在test/java/com/itheima/test下新建MyBatisEditTest测试类
    ```
      package com.itheima.test;
      import com.itheima.domain.User;
      import org.apache.ibatis.io.Resources;
      import org.apache.ibatis.session.SqlSession;
      import org.apache.ibatis.session.SqlSessionFactory;
      import org.apache.ibatis.session.SqlSessionFactoryBuilder;
      import org.junit.Test;
      import java.io.IOException;
      import java.io.InputStream;
      public class MyBatisEditTest {
          @Test
          public void test1() throws IOException {
              // 模拟user对象
              User user = new User();
              user.setId(2);
              user.setUsername("linmei");
              user.setPassword("555");

              // 获取核心配置文件
              InputStream resource = Resources.getResourceAsStream("sqlMapConfig.xml");
              // 获取sqlSession工厂对象
              SqlSessionFactory sqlSessionFactory = new SqlSessionFactoryBuilder().build(resource);
              // 获得sqlSession对象
              SqlSession sqlSession = sqlSessionFactory.openSession();
              // 执行sql语句
              int update = sqlSession.update("userMapper.update", user);
              System.out.println(update);
              // mybatis执行更新操作 提交事务
              sqlSession.commit();
              //释放资源
              sqlSession.close();
          }
      }
    ```

### 修改操作注意问题
  1. 修改语句使用update标签
  2. 修改操作使用的API是sqlSession.update(“命名空间.id”,实体对象);

## MyBatis的删除数据操作
### 编写UserMapper映射文件
  ```
    <mapper namespace="userMapper">
      <delete id="delete" parameterType="java.lang.Integer">
        delete from user where id=#{id}
      </delete>
    </mapper>
  ```

### 编写删除数据的代码
  * 在test/java/com/itheima/test下新建MyBatisDeleteTest测试类
    ```
      package com.itheima.test;
      import org.apache.ibatis.io.Resources;
      import org.apache.ibatis.session.SqlSession;
      import org.apache.ibatis.session.SqlSessionFactory;
      import org.apache.ibatis.session.SqlSessionFactoryBuilder;
      import org.junit.Test;
      import java.io.IOException;
      import java.io.InputStream;
      public class MyBatisDeleteTest {
          @Test
          public void test1() throws IOException {
              // 获取核心配置文件
              InputStream resource = Resources.getResourceAsStream("sqlMapConfig.xml");
              // 获取sqlSession工厂对象
              SqlSessionFactory sqlSessionFactory = new SqlSessionFactoryBuilder().build(resource);
              // 获得sqlSession对象
              SqlSession sqlSession = sqlSessionFactory.openSession();
              // 执行sql语句
              int delete = sqlSession.delete("userMapper.delete", 2);
              System.out.println(delete);
              // mybatis执行更新操作 提交事务
              sqlSession.commit();
              //释放资源
              sqlSession.close();
          }
      }
    ```

### 删除操作注意问题
  1. 删除语句使用delete标签
  2. Sql语句中使用#{任意字符串}方式引用传递的单个参数
  3. 删除操作使用的API是sqlSession.delete(“命名空间.id”,Object);

## 知识小节
{% note danger %}
增删改查映射配置与API：
查询数据： List`<User>` userList = sqlSession.selectList("userMapper.findAll");
    `<select id="findAll" resultType="com.itheima.domain.User">`
      select * from User
    `</select>`
添加数据： sqlSession.insert("userMapper.add", user);
    `<insert id="add" parameterType="com.itheima.domain.User">`
      insert into user values(#{id},#{username},#{password})
    `</insert>`
修改数据： sqlSession.update("userMapper.update", user);
    `<update id="update" parameterType="com.itheima.domain.User">`
      update user set username=#{username},password=#{password} where id=#{id}
    `</update>`
删除数据：sqlSession.delete("userMapper.delete",3);
    `<delete id="delete" parameterType="java.lang.Integer">`
      delete from user where id=#{id}
    `</delete>`
{% endnote %}

# MyBatis的核心配置文件概述
## MyBatis核心配置文件层级关系
![关系](/images/mybatis/relation.jpg)

## MyBatis常用配置解析
### environments标签
  * 数据库环境的配置，支持多环境配置
    - ![核心配置文件](/images/mybatis/核心配置文件.jpg)
  * 其中，事务管理器（transactionManager）类型有两种：
    - JDBC：这个配置就是直接使用了JDBC 的提交和回滚设置，它依赖于从数据源得到的连接来管理事务作用域。
    - MANAGED：这个配置几乎没做什么。它从来不提交或回滚一个连接，而是让容器来管理事务的整个生命周期（比如 JEE 应用服务器的上下文）。 默认情况下它会关闭连接，然而一些容器并不希望这样，因此需要将 closeConnection 属性设置为 false 来阻止它默认的关闭行为。
  * 其中，数据源（dataSource）类型有三种：
    - UNPOOLED：这个数据源的实现只是每次被请求时打开和关闭连接。
    - POOLED：这种数据源的实现利用“池”的概念将 JDBC 连接对象组织起来。
    - JNDI：这个数据源的实现是为了能在如 EJB 或应用服务器这类容器中使用，容器可以集中或在外部配置数据源，然后放置一个 JNDI 上下文的引用。

### mapper标签
  * 该标签的作用是加载映射的，加载方式有如下几种：
    - 使用相对于类路径的资源引用，例如：<mapper resource="org/mybatis/builder/AuthorMapper.xml"/>
    - 使用完全限定资源定位符（URL），例如：<mapper url="file:///var/mappers/AuthorMapper.xml"/>
    - 使用映射器接口实现类的完全限定类名，例如：<mapper class="org.mybatis.builder.AuthorMapper"/>
    - 将包内的映射器接口实现全部注册为映射器，例如：<package name="org.mybatis.builder"/>

### Properties标签
  * 实际开发中，习惯将数据源的配置信息单独抽取成一个properties文件，该标签可以加载额外配置的properties文件
    - 在resources下新建jdbc.properties
      ```
        jdbc.driver=com.mysql.jdbc.Driver
        jdbc.url=jdbc:mysql://localhost:3306/test
        jdbc.username=root
        jdbc.password=root
      ```
    - 在sqlMapConfig.xml中引入jdbc.properties
      ```
        <?xml version="1.0" encoding="UTF-8" ?>
        <!DOCTYPE configuration PUBLIC "-//mybatis.org//DTD Config 3.0//EN" "http://mybatis.org/dtd/mybatis-3-config.dtd">
        <configuration>
            <!--通过properties标签加载jdbc.properties-->
            <properties resource="jdbc.properties"></properties>
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
                <mapper resource="com/itheima/mapper/UserMapper.xml"></mapper>
            </mappers>
        </configuration>
      ```

### typeAliases标签
  * 类型别名是为Java 类型设置一个短的名字。原来的类型名称配置如下![typeAliases标签](/images/mybatis/核心配置文件2.jpg)
  * 在sqlMapConfig.xml中添加typeAliases标签，将userMapper.xml中的`com.itheima.domain.User`修改成别名`user`
    - 在sqlMapConfig.xml中, 注意typeAliases的顺序，不然也会报错
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
                <mapper resource="com/itheima/mapper/UserMapper.xml"></mapper>
            </mappers>
        </configuration>
      ```

    - 在userMapper.xml中, 将`<select id="findAll" resultType="com.itheima.domain.User">`改为`<select id="findAll" resultType="user">`
      ```
        <?xml version="1.0" encoding="UTF-8" ?>
        <!DOCTYPE mapper PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN" "http://mybatis.org/dtd/mybatis-3-mapper.dtd">
        <mapper namespace="userMapper">
            <!--插入操作-->
            <insert id="add" parameterType="user">
                insert into user values(#{id}, #{username}, #{password})
            </insert>
            <!--删除操作-->
            <delete id="delete" parameterType="user">
                delete from user where id=#{id}
            </delete>
            <!--更新操作-->
            <update id="update" parameterType="user">
                update user set username=#{username}, password=#{password} where id=#{id}
            </update>
            <!--查询操作 resultType: 查询的结果放到哪个位置-->
            <select id="findAll" resultType="user">
                select * from user
            </select>
        </mapper>
      ```
  * 上面我们是自定义的别名，mybatis框架已经为我们设置好的一些常用的类型的别名
    - ![typeAliases标签](/images/mybatis/核心配置文件3.jpg)

# MyBatis核心配置文件深入
## 代码准备
1. 新建itheima_mybatis_004的module,并配置webapp文件夹
2. 在pom.xml中导入响应的坐标
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
5. 在java下新建com.itheima.mapper.UserMapper.java中，添加save方法
    ```
      package com.itheima.mapper;
      import com.itheima.domain.User;
      import java.util.List;
      public interface UserMapper {
          void save(User user);
          List<User> findById(int id);
      }
    ```
6. 在resources下新建mapper/UserMapper.xml,配置save方法
    ```
      <?xml version="1.0" encoding="UTF-8" ?>
      <!DOCTYPE mapper PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN" "http://mybatis.org/dtd/mybatis-3-mapper.dtd">
      <mapper namespace="com.itheima.mapper.UserMapper">
          <insert id="save" parameterType="user">
              insert into user values(#{id}, #{username}, #{password}, #{birthday})
          </insert>
          <select id="findById" parameterType="int" resultType="user">
              select * from user where id = ${id}
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
10. 编写测试方法,在test/java下新建com.itheima.userTest.java
  ```
    package com.itheima;
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
    public class userTest {
        @Test
        public void userTest2() throws IOException {
            InputStream resourceAsStream = Resources.getResourceAsStream("sqlMapConfig.xml");
            SqlSessionFactory sqlSessionFactory = new SqlSessionFactoryBuilder().build(resourceAsStream);
            SqlSession sqlSession = sqlSessionFactory.openSession();
            UserMapper mapper = sqlSession.getMapper(UserMapper.class);
            // 创建User
            User user = new User();
            user.setUsername("ceshi");
            user.setPassword("123456");
            user.setBirthday(new Date());
            // 执行保存操作
            mapper.save(user);
            sqlSession.commit();
            sqlSession.close();
        }

        @Test
        public void userTest3() throws IOException {
            InputStream resourceAsStream = Resources.getResourceAsStream("sqlMapConfig.xml");
            SqlSessionFactory sqlSessionFactory = new SqlSessionFactoryBuilder().build(resourceAsStream);
            SqlSession sqlSession = sqlSessionFactory.openSession();
            UserMapper mapper = sqlSession.getMapper(UserMapper.class);

            List<User> byId = mapper.findById(6);
            System.out.println(byId);
        }
    }
  ```

## typeHandlers标签
  * 无论是 MyBatis 在预处理语句（PreparedStatement）中设置一个参数时，还是从结果集中取出一个值时， 都会用类型处理器将获取的值以合适的方式转换成 Java 类型。下表描述了一些默认的类型处理器（截取部分）。
  * ![typeHandlers标签](/images/mybatis/核心配置文件4.jpg)
  * 你可以重写类型处理器或创建你自己的类型处理器来处理不支持的或非标准的类型。具体做法为：实现 org.apache.ibatis.type.TypeHandler 接口， 或继承一个很便利的类 org.apache.ibatis.type.BaseTypeHandler， 然后可以选择性地将它映射到一个JDBC类型。例如需求：一个Java中的Date数据类型，我想将之存到数据库的时候存成一个1970年至今的毫秒数，取出来时转换成java的Date，即java的Date与数据库的varchar毫秒值之间转换。
  * 开发步骤：
    1. 定义转换类继承类BaseTypeHandler`<T>`
    2. 覆盖4个未实现的方法，其中setNonNullParameter为java程序设置数据到数据库的回调方法，getNullableResult为查询时 mysql的字符串类型转换成 java的Type类型的方法
    3. 在MyBatis核心配置文件中进行注册
    4. 测试转换是否正确

### 定义转换类继承类BaseTypeHandler`<T>`
  1. 在java下新建com.itheima.handle.DateTypeHandle,继承BaseTypeHandler
    ```
      package com.itheima.handle;
      import org.apache.ibatis.type.BaseTypeHandler;
      import java.util.Date;
      public class DateTypeHandle extends BaseTypeHandler<Date> {
      }
    ```

### 覆盖4个未实现的方法，其中setNonNullParameter为java程序设置数据到数据库的回调方法，getNullableResult为查询时 mysql的字符串类型转换成 java的Type类型的方法
  ```
    package com.itheima.handle;
    import org.apache.ibatis.type.BaseTypeHandler;
    import org.apache.ibatis.type.JdbcType;
    import java.sql.CallableStatement;
    import java.sql.PreparedStatement;
    import java.sql.ResultSet;
    import java.sql.SQLException;
    import java.util.Date;
    public class DateTypeHandle extends BaseTypeHandler<Date> {
        // 将java类型 转换成 数据库需要的类型
        public void setNonNullParameter(PreparedStatement preparedStatement, int i, Date date, JdbcType jdbcType) throws SQLException {
            long time = date.getTime();
            preparedStatement.setLong(i, time);
        }
        // 将数据库中类型 转换成 java类型
        // String参数 要转换的字段
        // ResultSet 查询出的结果集
        public Date getNullableResult(ResultSet resultSet, String s) throws SQLException {
            long aLong = resultSet.getLong(s);
            Date date = new Date(aLong);
            return date;
        }
        // 将数据库中类型 转换成 java类型
        public Date getNullableResult(ResultSet resultSet, int i) throws SQLException {
            long aLong = resultSet.getLong(i);
            Date date = new Date(aLong);
            return date;
        }
        // 将数据库中类型 转换成 java类型
        public Date getNullableResult(CallableStatement callableStatement, int i) throws SQLException {
            long aLong = callableStatement.getLong(i);
            Date date = new Date(aLong);
            return date;
        }
    }
  ```

### 在MyBatis核心配置文件中进行注册
  * 在resources下的sqlMapConfig.xml中，配置类型转换器
    ```
      <!--注册类型处理器-->
      <typeHandlers>
          <typeHandler handler="com.itheima.handle.DateTypeHandle"></typeHandler>
      </typeHandlers>
    ```
  
### 测试
  * 再回到test/com/itheima/userTest.java中，启动

## plugins标签
  * MyBatis可以使用第三方的插件来对功能进行扩展，分页助手PageHelper是将分页的复杂操作进行封装，使用简单的方式即可获得分页的相关数据
  * 开发步骤：
    1. 导入通用PageHelper的坐标
    2. 在mybatis核心配置文件中配置PageHelper插件
    3. 测试分页数据获取

### 导入通用PageHelper的坐标
  1. 在mapper.UserMapper.java中添加findAll的查询方法
    ```
      List<User> findAll();
    ```
  2. 在resources.mapper.UserMapper.xml中，配置findAll方法
    ```
      <select id="findAll" resultType="user">
          select * from user
      </select>
    ```
  3. 测试方法
    ```
      @Test
      public void userTest4() throws IOException {
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
    ```
  4. 在pom.xml中导入PageHelper的坐标(注意版本)
    ```
      <dependency>
          <groupId>com.github.pagehelper</groupId>
          <artifactId>pagehelper</artifactId>
          <version>3.7.5</version>
      </dependency>
      <dependency>
          <groupId>com.github.jsqlparser</groupId>
          <artifactId>jsqlparser</artifactId>
          <version>0.9.1</version>
      </dependency>
    ```

### 在mybatis核心配置文件中配置PageHelper插件
  * 在resources/sqlMapConfig.xml中，配置PageHelper插件
    ```
      <!--配置分页助手插件-->
      <plugins>
          <plugin interceptor="com.github.pagehelper.PageHelper">
              <!-- 指定方言 -->
              <property name="dialect" value="mysql"/>
          </plugin>
      </plugins>
    ```

### 测试分页数据获取
  * 在test下新建com.itheima.userTest2.java
    ```
      package com.itheima;
      import com.github.pagehelper.PageHelper;
      import com.github.pagehelper.PageInfo;
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
      public class userTest2 {
          @Test
          public void userTest4() throws IOException {
              InputStream resourceAsStream = Resources.getResourceAsStream("sqlMapConfig.xml");
              SqlSessionFactory sqlSessionFactory = new SqlSessionFactoryBuilder().build(resourceAsStream);
              SqlSession sqlSession = sqlSessionFactory.openSession();
              UserMapper mapper = sqlSession.getMapper(UserMapper.class);
              // 设置分页相关参数 当前页+每页显示的条数
              PageHelper.startPage(1, 3);
              List<User> userList = mapper.findAll();
              for (User user : userList) {
                  System.out.println(user);
              }
              // 获取与分页相关参数
              PageInfo<User> pageInfo = new PageInfo<User>(userList);
              System.out.println("总条数："+pageInfo.getTotal());
              System.out.println("总页数："+pageInfo.getPages());
              System.out.println("当前页："+pageInfo.getPageNum());
              System.out.println("每页显示条数："+pageInfo.getPageSize());
              System.out.println("上一页："+pageInfo.getPrePage());
              System.out.println("下一页："+pageInfo.getNextPage());
              System.out.println("是否第一页："+pageInfo.isIsFirstPage());
              System.out.println("是否最后一页："+pageInfo.isIsLastPage());
              sqlSession.close();
          }
      }
    ```

## 知识小结
### MyBatis核心配置文件常用标签：
1. properties标签：该标签可以加载外部的properties文件
2. typeAliases标签：设置类型别名
3. environments标签：数据源环境配置标签
4. typeHandlers标签：配置自定义类型处理器
5. plugins标签：配置MyBatis的插件

# MyBatis的相应API
## SqlSession工厂构建器SqlSessionFactoryBuilder
  * 常用API：SqlSessionFactory  build(InputStream inputStream)
  * 通过加载mybatis的核心文件的输入流的形式构建一个SqlSessionFactory对象
    ```
      String resource = "org/mybatis/builder/mybatis-config.xml"; 
      InputStream inputStream = Resources.getResourceAsStream(resource); 
      SqlSessionFactoryBuilder builder = new SqlSessionFactoryBuilder(); 
      SqlSessionFactory factory = builder.build(inputStream);
    ```
  * 其中， Resources 工具类，这个类在 org.apache.ibatis.io 包中。Resources 类帮助你从类路径下、文件系统或一个 web URL 中加载资源文件。

## SqlSession工厂对象SqlSessionFactory
  * SqlSessionFactory 有多个个方法创建 SqlSession 实例。常用的有如下两个：![api](/images/mybatis/api.jpg)

## SqlSession会话对象
  * SqlSession 实例在 MyBatis 中是非常强大的一个类。在这里你会看到所有执行语句、提交或回滚事务和获取映射器实例的方法。
  * 执行语句的方法主要有：
    ```
      <T> T selectOne(String statement, Object parameter) 
      <E> List<E> selectList(String statement, Object parameter) 
      int insert(String statement, Object parameter) 
      int update(String statement, Object parameter) 
      int delete(String statement, Object parameter)
    ```
  * 操作事务的方法主要有： 
    ```
      void commit()  
      void rollback()
    ```

# MyBatis的Dao层实现
## 代理开发方式
### 代理开发方式介绍
  * 采用 Mybatis 的代理开发方式实现 DAO 层的开发，这种方式是我们后面进入企业的主流。
  Mapper 接口开发方法只需要程序员编写Mapper 接口（相当于Dao 接口），由Mybatis 框架根据接口定义创建接口的动态代理对象，代理对象的方法体同上边Dao接口实现类方法。
  Mapper 接口开发需要遵循以下规范：
    1. Mapper.xml文件中的namespace与mapper接口的全限定名相同
    2. Mapper接口方法名和Mapper.xml中定义的每个statement的id相同
    3. Mapper接口方法的输入参数类型和mapper.xml中定义的每个sql的parameterType的类型相同
    4. Mapper接口方法的输出参数类型和mapper.xml中定义的每个sql的resultType的类型相同

### 编写UserMapper接口
![mapper接口](/images/mybatis/dao层.jpg)

### 测试代理方式
1. 新建itheima_mybatis_002的module,并配置webapp文件夹
2. 在pom.xml中导入相应坐标
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
    </dependency>
  ```
3. 在mysql test数据库中新建一个user表，有username和password2个字段
4. 在java下新建com.itheima.domain.User.java实体类
  ```
    package com.itheima.domain;
    public class User {
        private int id;
        private String username;
        private String password;
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
                    '}';
        }
    }
  ```
5. 编写映射文件UserMapper.xml
  * 在resources下新建com/itheima/mapper/UserMapper.xml
    ```
      <?xml version="1.0" encoding="UTF-8" ?>
      <!DOCTYPE mapper PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN" "http://mybatis.org/dtd/mybatis-3-mapper.dtd">
      <mapper namespace="userMapper">
          <!--select查询语句, resultType: 查询的结果放到哪个位置-->
          <select id="findAll" resultType="com.itheima.domain.User">
              select * from user
          </select>
      </mapper>
    ```
6. 在resources下新建jdbc.properties
  ```
    jdbc.driver=com.mysql.jdbc.Driver
    jdbc.url=jdbc:mysql://localhost:3306/test
    jdbc.username=root
    jdbc.password=root
  ```
7. 在resources下新建sqlMapConfig.xml
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
            <mapper resource="com/itheima/mapper/UserMapper.xml"></mapper>
        </mappers>
    </configuration>
  ```
8. 在java下创建com.itheima.dao.UserMapper的接口
  ```
    package com.itheima.dao;
    import com.itheima.domain.User;
    import java.util.List;
    public interface UserMapper {
        List<User> findAll();
        User findById(int id);
    }
  ```
9. 修改UserMapper.xml中的代码
  ```
    <?xml version="1.0" encoding="UTF-8" ?>
    <!DOCTYPE mapper PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN" "http://mybatis.org/dtd/mybatis-3-mapper.dtd">
    <mapper namespace="com.itheima.dao.UserMapper">
        <!--select查询语句, resultType: 查询的结果放到哪个位置-->
        <select id="findAll" resultType="user">
            select * from user
        </select>

        <select id="findById" parameterType="int" resultType="user">
            select * from user where id = #{id}
        </select>
    </mapper>
  ```
10. 编写测试方法，在java下新建com.itheima.service.ServiceDemo.java
  ```
    package com.itheima.service;
    import com.itheima.dao.UserMapper;
    import com.itheima.domain.User;
    import org.apache.ibatis.io.Resources;
    import org.apache.ibatis.session.SqlSession;
    import org.apache.ibatis.session.SqlSessionFactory;
    import org.apache.ibatis.session.SqlSessionFactoryBuilder;
    import java.io.IOException;
    import java.io.InputStream;
    import java.util.List;

    public class ServiceDemo {
        public static void main(String[] args) throws IOException {
            InputStream resourceAsStream = Resources.getResourceAsStream("sqlMapConfig.xml");
            SqlSessionFactory sqlSessionFactory = new SqlSessionFactoryBuilder().build(resourceAsStream);
            SqlSession sqlSession = sqlSessionFactory.openSession();
            UserMapper mapper = sqlSession.getMapper(UserMapper.class);
            List<User> all = mapper.findAll();
            System.out.println(all);
            User id = mapper.findById(1);
            System.out.println(id);
        }
    }
  ```
## 知识小结
  * MyBatis的Dao层实现的两种方式：
    - 手动对Dao进行实现：传统开发方式
    - 代理方式对Dao进行实现：
      ```
        UserMapper userMapper = sqlSession.getMapper(UserMapper.class);
      ```








