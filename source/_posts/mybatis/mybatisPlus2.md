---
title: mybatisPlus2
date: 2021-07-26 16:35:03
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

# 初始代码准备
1. 创建一个itheima_springboot的project
2. 在pom.xml中导入相应的坐标
  ```
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
  ```
3. 在resources下新建log4j.properties
  ```
    log4j.rootLogger=DEBUG,A1
    log4j.appender.A1=org.apache.log4j.ConsoleAppender
    log4j.appender.A1.layout=org.apache.log4j.PatternLayout
    log4j.appender.A1.layout.ConversionPattern=[%t] [%c]-[%p] %m%n
  ```
4. 在java下新建com.itheima.pojo.User实体类
  ```
    package com.itheima.pojo;
    import lombok.AllArgsConstructor;
    import lombok.Data;
    import lombok.NoArgsConstructor;

    @Data
    @NoArgsConstructor
    @AllArgsConstructor
    public class User {
        private Long id;
        private String userName;
        private String password;
        private String name;
        private Integer age;
        private String email;
    }
  ```

5. 在java下新建com.itheima.mapper.UserMapper.java
  ```
    package com.itheima.mapper;
    import com.baomidou.mybatisplus.core.mapper.BaseMapper;
    import com.itheima.pojo.User;
    public interface UserMapper extends BaseMapper<User> {
    }
  ```
6. 在resources下新建mapper/UserMapper.xml
  ```
    <?xml version="1.0" encoding="UTF-8" ?>
    <!DOCTYPE mapper
            PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
            "http://mybatis.org/dtd/mybatis-3-mapper.dtd">
    <mapper namespace="com.itheima.mapper.UserMapper">

    </mapper>
  ```
7. 在resources下修改application.yml
  ```
    spring:
      datasource:
        driver-class-name: com.mysql.jdbc.Driver
        url: jdbc:mysql://127.0.0.1:3306/mp?useUnicode=true&characterEncoding=utf8&autoReconnect=true&allowMultiQueries=true&useSSL=false
        username: root
        password: root
    mybatis-plus:
      # 指定Mapper.xml文件的路径
      mapper-locations: classpath:mapper/UserMapper.xml
      # 实体对象的扫描包
      type-aliases-package: com.itheima.pojo
      # 配置表名(tb_user)
      global-config:
        db-config:
          table-prefix: tb_
  ```
8. 编写测试方法
  ```
    package com.itheima.test;
    import com.itheima.mapper.UserMapper;
    import com.itheima.pojo.User;
    import org.junit.Test;
    import org.junit.runner.RunWith;
    import org.springframework.beans.factory.annotation.Autowired;
    import org.springframework.boot.test.context.SpringBootTest;
    import org.springframework.test.context.junit4.SpringJUnit4ClassRunner;
    import java.util.List;
    @RunWith(SpringJUnit4ClassRunner.class)
    @SpringBootTest
    public class MyTest {
        @Autowired
        private UserMapper mapper;
        @Test
        public void selectMyList() {
            List<User> userList = mapper.selectList(null);
            for (User user : userList) {
                System.out.println(user);
            }
        }
    }
  ```

# ActiveRecord
{% note warning %}
  什么是ActiveRecord？
  ActiveRecord也属于ORM（对象关系映射）层，由Rails最早提出，遵循标准的ORM模型：表映射到记录，记录映射到对象，字段映射到对象属性。配合遵循的命名和配置惯例，能够很大程度的快速实现模型的操作，而且简洁易懂。

  ActiveRecord的主要思想是：
    1. 每一个数据库表对应创建一个类，类的每一个对象实例对应于数据库中表的一行记录；通常表的每个字段在类中都有相应的Field；
    2. ActiveRecord同时负责把自己持久化，在ActiveRecord中封装了对数据库的访问，即CURD;；
    3. ActiveRecord是一种领域模型(Domain Model)，封装了部分业务逻辑；
{% endnote %}

## 开启AR之旅
  * 在MP中，开启AR非常简单，只需要将实体对象继承Model即可。
    - 修改pojo.User实体类
      ```
        package com.itheima.pojo;
        import com.baomidou.mybatisplus.extension.activerecord.Model;
        import lombok.AllArgsConstructor;
        import lombok.Data;
        import lombok.NoArgsConstructor;
        @Data
        @NoArgsConstructor
        @AllArgsConstructor
        public class User extends Model<User> {
            private Long id;
            private String userName;
            private String password;
            private String name;
            private Integer age;
            private String email;
        }
      ```

## 根据主键查询
  ```
    package com.itheima.test;
    import com.itheima.pojo.User;
    import org.junit.Test;
    import org.junit.runner.RunWith;
    import org.springframework.boot.test.context.SpringBootTest;
    import org.springframework.test.context.junit4.SpringJUnit4ClassRunner;
    @RunWith(SpringJUnit4ClassRunner.class)
    @SpringBootTest
    public class MyTest2 {
        @Test
        public void selectMyList() {
            User user = new User();
            user.setId(2L);
            User user1 = user.selectById();
            System.out.println(user1);
        }
    }
  ```

## 新增数据
  ```
    package com.itheima.test;
    import com.itheima.pojo.User;
    import org.junit.Test;
    import org.junit.runner.RunWith;
    import org.springframework.boot.test.context.SpringBootTest;
    import org.springframework.test.context.junit4.SpringJUnit4ClassRunner;
    @RunWith(SpringJUnit4ClassRunner.class)
    @SpringBootTest
    public class MyTestInsert {
        @Test
        public void insertMyList() {
            User user = new User();
            user.setEmail("dkdu477@qq.com");
            user.setPassword("123");
            user.setUserName("youzi");
            user.setAge(18);
            user.setName("柚子");
            boolean insert = user.insert();
            System.out.println(insert);
        }
    }
  ```

## 更新操作
  ```
    package com.itheima.test;
    import com.itheima.pojo.User;
    import org.junit.Test;
    import org.junit.runner.RunWith;
    import org.springframework.boot.test.context.SpringBootTest;
    import org.springframework.test.context.junit4.SpringJUnit4ClassRunner;
    @RunWith(SpringJUnit4ClassRunner.class)
    @SpringBootTest
    public class MyTestUpdate {
        @Test
        public void updateMyList() {
            User user = new User();
            user.setName("刘璋");
            user.setAge(24);
            user.setId(10L);
            boolean updateById = user.updateById();
            System.out.println(updateById);
        }
    }
  ```

## 删除操作
  ```
    package com.itheima.test;
    import com.itheima.pojo.User;
    import org.junit.Test;
    import org.junit.runner.RunWith;
    import org.springframework.boot.test.context.SpringBootTest;
    import org.springframework.test.context.junit4.SpringJUnit4ClassRunner;
    @RunWith(SpringJUnit4ClassRunner.class)
    @SpringBootTest
    public class MyTestDelete {
        @Test
        public void deleteMyList() {
            User user = new User();
            user.setId(10L);
            boolean updateById = user.deleteById();
            System.out.println(updateById);
        }
    }
  ```

## 根据条件查询
  ```
    package com.itheima.test;
    import com.baomidou.mybatisplus.core.conditions.query.QueryWrapper;
    import com.itheima.pojo.User;
    import org.junit.Test;
    import org.junit.runner.RunWith;
    import org.springframework.boot.test.context.SpringBootTest;
    import org.springframework.test.context.junit4.SpringJUnit4ClassRunner;
    import java.util.List;
    @RunWith(SpringJUnit4ClassRunner.class)
    @SpringBootTest
    public class MyTestCondition {
        @Test
        public void conditionMyList() {
            User user = new User();
            QueryWrapper<User> wrapper = new QueryWrapper<>();
            wrapper.gt("age", "20");
            List<User> userList = user.selectList(wrapper);
            for (User user1 : userList) {
                System.out.println(user1);
            }
        }
    }
  ```

# 自动填充功能
  * 有些时候我们可能会有这样的需求，插入或者更新数据时，希望有些字段可以自动填充数据，比如密码、version等。在MP中提供了这样的功能，可以实现自动填充。

## 添加@TableField注解
  1. 在pojo.User.java实体类中添加@TableField注解
    ```
      package com.itheima.pojo;
      import com.baomidou.mybatisplus.annotation.FieldFill;
      import com.baomidou.mybatisplus.annotation.TableField;
      import com.baomidou.mybatisplus.extension.activerecord.Model;
      import lombok.AllArgsConstructor;
      import lombok.Data;
      import lombok.NoArgsConstructor;
      @Data
      @NoArgsConstructor
      @AllArgsConstructor
      public class User extends Model<User> {
          private Long id;
          private String userName;
          @TableField(fill = FieldFill.INSERT) // insert时自动填充
          private String password;
          private String name;
          private Integer age;
          private String email;
      }
    ```

## 编写MyMetaObjectHandler
  * 在java下新建com.itheima.handle.MyMetaObjectHandler.java，实现MetaObjectHandler的insertFill方法
    ```
      package com.itheima.handle;
      import com.baomidou.mybatisplus.core.handlers.MetaObjectHandler;
      import org.apache.ibatis.reflection.MetaObject;
      import org.springframework.stereotype.Component;
      @Component
      public class MyMetaObjectHandler implements MetaObjectHandler {
          /**
          * 插入数据时填充
          * @param metaObject
          */
          @Override
          public void insertFill(MetaObject metaObject) {
              // 先获取到password的值，再进行判断，如果为空，就进行填充，如果不为空，就不做处理
              Object password = getFieldValByName("password", metaObject);
              if(password == null) {
                  setFieldValByName("password", "123", metaObject);
              }
          }
          /**
          * 更新数据时填充
          * @param metaObject
          */
          @Override
          public void updateFill(MetaObject metaObject) {
          }
      }
    ```

## 编写测试方法
  ```
    package com.itheima.test;
    import com.itheima.pojo.User;
    import org.junit.Test;
    import org.junit.runner.RunWith;
    import org.springframework.boot.test.context.SpringBootTest;
    import org.springframework.test.context.junit4.SpringJUnit4ClassRunner;
    @RunWith(SpringJUnit4ClassRunner.class)
    @SpringBootTest
    public class MyTestInsert2 {
        @Test
        public void insertMyList() {
            User user = new User();
            user.setEmail("555@qq.com");
            user.setUserName("baipiao");
            user.setAge(21);
            user.setName("白嫖");
            boolean insert = user.insert();
            System.out.println(insert);
        }
    }
  ```

# 逻辑删除
  * 开发系统时，有时候在实现功能时，删除操作需要实现逻辑删除，所谓逻辑删除就是将数据标记为删除，而并非真正的物理删除（非DELETE操作），查询时需要携带状态条件，确保被标记的数据不被查询到。这样做的目的就是避免数据被真正的删除。

## 修改表结构
  1. 为tb_user表增加deleted字段，用于表示数据是否被删除，1代表删除，0代表未删除。
    ```
      ALTER TABLE `tb_user` ADD COLUMN `deleted` int(1) NULL DEFAULT 0 COMMENT '1代表删除，0代表未删除' AFTER `email`;
    ```
  2. 同时，也修改pojo.User实体，增加deleted属性并且添加@TableLogic注解：
    ```
      package com.itheima.pojo;
      import com.baomidou.mybatisplus.annotation.FieldFill;
      import com.baomidou.mybatisplus.annotation.TableField;
      import com.baomidou.mybatisplus.annotation.TableLogic;
      import com.baomidou.mybatisplus.extension.activerecord.Model;
      import lombok.AllArgsConstructor;
      import lombok.Data;
      import lombok.NoArgsConstructor;
      @Data
      @NoArgsConstructor
      @AllArgsConstructor
      public class User extends Model<User> {
          private Long id;
          private String userName;
          @TableField(fill = FieldFill.INSERT) // insert时自动填充
          private String password;
          private String name;
          private Integer age;
          private String email;
          @TableLogic
          private int deleted;
      }
    ```

## 配置
  * 在application.properties中
    ```
      # 逻辑已删除值(默认为 1)
      mybatis-plus.global-config.db-config.logic-delete-value=1
      # 逻辑未删除值(默认为 0)
      mybatis-plus.global-config.db-config.logic-not-delete-value=0
    ```

## 编写测试方法
  ```
    package com.itheima.test;
    import com.itheima.pojo.User;
    import org.junit.Test;
    import org.junit.runner.RunWith;
    import org.springframework.boot.test.context.SpringBootTest;
    import org.springframework.test.context.junit4.SpringJUnit4ClassRunner;
    @RunWith(SpringJUnit4ClassRunner.class)
    @SpringBootTest
    public class MyTestDelete2 {
        @Test
        public void deleteMyList() {
            User user = new User();
            user.setId(11L);
            boolean updateById = user.deleteById();
            System.out.println(updateById);
        }
    }
  ```

## 测试结果
  * ![结果](/images/mybatis/plus3.jpg)

# 通用枚举
  * 解决了繁琐的配置，让 mybatis 优雅的使用枚举属性！

## 修改表结构
  ```
    ALTER TABLE `tb_user` ADD COLUMN `sex` int(1) NULL DEFAULT 1 COMMENT '1-男，2-女' AFTER `deleted`;
  ```

## 定义枚举
  * 在java下新建com.itheima.enums.SexEnum.java的枚举类，去实现IEnum方法
    ```
      package com.itheima.enums;
      import com.baomidou.mybatisplus.core.enums.IEnum;
      public enum SexEnum implements IEnum<Integer> {
          MAN(1,"男"),
          WOMAN(2,"女");
          private int value;
          private String desc;
          SexEnum(int value, String desc) {
              this.value = value;
              this.desc = desc;
          }
          @Override
          public Integer getValue() {
              return this.value;
          }
          @Override
          public String toString() {
              return this.desc;
          }
      }
    ```

## 配置
  * 在application.properties中
    ```
      # 枚举包扫描
      mybatis-plus.type-enums-package=cn.itcast.mp.enums
    ```

## 修改实体
  * 在pojo.User.java实体类中
    ```
      package com.itheima.pojo;
      import com.baomidou.mybatisplus.annotation.FieldFill;
      import com.baomidou.mybatisplus.annotation.TableField;
      import com.baomidou.mybatisplus.annotation.TableLogic;
      import com.baomidou.mybatisplus.extension.activerecord.Model;
      import com.itheima.enums.SexEnum;
      import lombok.AllArgsConstructor;
      import lombok.Data;
      import lombok.NoArgsConstructor;
      @Data
      @NoArgsConstructor
      @AllArgsConstructor
      public class User extends Model<User> {
          private Long id;
          private String userName;
          @TableField(fill = FieldFill.INSERT) // insert时自动填充
          private String password;
          private String name;
          private Integer age;
          private String email;
          @TableLogic
          private int deleted;
          private SexEnum sex;
      }
    ```

## 编写测试方法
  ```
    package com.itheima.test;
    import com.itheima.enums.SexEnum;
    import com.itheima.pojo.User;
    import org.junit.Test;
    import org.junit.runner.RunWith;
    import org.springframework.boot.test.context.SpringBootTest;
    import org.springframework.test.context.junit4.SpringJUnit4ClassRunner;
    @RunWith(SpringJUnit4ClassRunner.class)
    @SpringBootTest
    public class MyTestInsert3 {
        @Test
        public void insertEnumMyList() {
            User user = new User();
            user.setEmail("888@qq.com");
            user.setUserName("yaosheng");
            user.setAge(23);
            user.setName("药神");
            user.setSex(SexEnum.WOMAN);
            boolean insert = user.insert();
            System.out.println(insert);
        }
    }
  ```
  
# 代码生成器
  * AutoGenerator 是 MyBatis-Plus 的代码生成器，通过 AutoGenerator 可以快速生成 Entity、Mapper、Mapper、XML、Service、Controller 等各个模块的代码，极大的提升了开发效率。

## 创建工程
  * 新建itheima_my_boot3的springboot project
  * 在pom.xml中导入相应的坐标
    ```
      <dependency>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter</artifactId>
      </dependency>
      <dependency>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-test</artifactId>
        <scope>test</scope>
      </dependency>
      <!--mybatis-plus的springboot支持-->
      <dependency>
        <groupId>com.baomidou</groupId>
        <artifactId>mybatis-plus-boot-starter</artifactId>
        <version>3.4.2</version>
      </dependency>
      <dependency>
        <groupId>com.baomidou</groupId>
        <artifactId>mybatis-plus-generator</artifactId>
        <version>3.1.1</version>
      </dependency>
      <dependency>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-freemarker</artifactId>
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
      <dependency>
        <groupId>org.projectlombok</groupId>
        <artifactId>lombok</artifactId>
      </dependency>
    ```

## 在java下新建com.itheima.generator.MyGenerator.java
  ```
    package com.itheima.generator;
    import java.util.ArrayList;
    import java.util.List;
    import java.util.Scanner;
    import com.baomidou.mybatisplus.core.exceptions.MybatisPlusException;
    import com.baomidou.mybatisplus.core.toolkit.StringPool;
    import com.baomidou.mybatisplus.core.toolkit.StringUtils;
    import com.baomidou.mybatisplus.generator.AutoGenerator;
    import com.baomidou.mybatisplus.generator.InjectionConfig;
    import com.baomidou.mybatisplus.generator.config.DataSourceConfig;
    import com.baomidou.mybatisplus.generator.config.FileOutConfig;
    import com.baomidou.mybatisplus.generator.config.GlobalConfig;
    import com.baomidou.mybatisplus.generator.config.PackageConfig;
    import com.baomidou.mybatisplus.generator.config.StrategyConfig;
    import com.baomidou.mybatisplus.generator.config.TemplateConfig;
    import com.baomidou.mybatisplus.generator.config.po.TableInfo;
    import com.baomidou.mybatisplus.generator.config.rules.NamingStrategy;
    import com.baomidou.mybatisplus.generator.engine.FreemarkerTemplateEngine;
    /**
    * <p>
    * mysql 代码生成器演示例子
    * </p>
    */
    public class MysqlGenerator {

        /**
        * <p>
        * 读取控制台内容
        * </p>
        */
        public static String scanner(String tip) {
            Scanner scanner = new Scanner(System.in);
            StringBuilder help = new StringBuilder();
            help.append("请输入" + tip + "：");
            System.out.println(help.toString());
            if (scanner.hasNext()) {
                String ipt = scanner.next();
                if (StringUtils.isNotEmpty(ipt)) {
                    return ipt;
                }
            }
            throw new MybatisPlusException("请输入正确的" + tip + "！");
        }

        /**
        * RUN THIS
        */
        public static void main(String[] args) {
            // 代码生成器
            AutoGenerator mpg = new AutoGenerator();

            // 全局配置
            GlobalConfig gc = new GlobalConfig();
            String projectPath = System.getProperty("user.dir");
            gc.setOutputDir(projectPath + "/src/main/java");
            gc.setAuthor("itcast");
            gc.setOpen(false);
            mpg.setGlobalConfig(gc);

            // 数据源配置
            DataSourceConfig dsc = new DataSourceConfig();
            dsc.setUrl("jdbc:mysql://127.0.0.1:3306/mp?useUnicode=true&useSSL=false&characterEncoding=utf8");
            // dsc.setSchemaName("public");
            dsc.setDriverName("com.mysql.jdbc.Driver");
            dsc.setUsername("root");
            dsc.setPassword("root");
            mpg.setDataSource(dsc);

            // 包配置
            PackageConfig pc = new PackageConfig();
            pc.setModuleName(scanner("模块名"));
            pc.setParent("com.itheima.generator");
            mpg.setPackageInfo(pc);

            // 自定义配置
            InjectionConfig cfg = new InjectionConfig() {
                @Override
                public void initMap() {
                    // to do nothing
                }
            };
            List<FileOutConfig> focList = new ArrayList<>();
            focList.add(new FileOutConfig("/templates/mapper.xml.ftl") {
                @Override
                public String outputFile(TableInfo tableInfo) {
                    // 自定义输入文件名称
                    return projectPath + "/itheima-generator/src/main/resources/mapper/" + pc.getModuleName()
                            + "/" + tableInfo.getEntityName() + "Mapper" + StringPool.DOT_XML;
                }
            });
            cfg.setFileOutConfigList(focList);
            mpg.setCfg(cfg);
            mpg.setTemplate(new TemplateConfig().setXml(null));

            // 策略配置
            StrategyConfig strategy = new StrategyConfig();
            strategy.setNaming(NamingStrategy.underline_to_camel);
            strategy.setColumnNaming(NamingStrategy.underline_to_camel);
    //        strategy.setSuperEntityClass("com.baomidou.mybatisplus.samples.generator.common.BaseEntity");
            strategy.setEntityLombokModel(true);
    //        strategy.setSuperControllerClass("com.baomidou.mybatisplus.samples.generator.common.BaseController");
            strategy.setInclude(scanner("表名"));
            strategy.setSuperEntityColumns("id");
            strategy.setControllerMappingHyphenStyle(true);
            strategy.setTablePrefix(pc.getModuleName() + "_");
            mpg.setStrategy(strategy);
            // 选择 freemarker 引擎需要指定如下加，注意 pom 依赖必须有！
            mpg.setTemplateEngine(new FreemarkerTemplateEngine());
            mpg.execute();
        }
    }
  ```
## 测试
  * 执行MyGenerator.java里面的方法 ![结果](/images/mybatis/plus4.jpg)
  * 生成代码 ![结果](/images/mybatis/plus5.jpg)

# MybatisX 快速开发插件
  * MybatisX 是一款基于 IDEA 的快速开发插件，为效率而生。
  * 安装方法：打开 IDEA，进入 File -> Settings -> Plugins -> Browse Repositories，输入 mybatisx 搜索并安装。
  * 功能：
    - Java 与 XML 调回跳转
    - Mapper 方法自动生成 XML