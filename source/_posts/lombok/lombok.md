---
title: lombok
date: 2021-06-08 11:18:15
tags:
  - springboot
  - lombok
  - java
type: java                                                                         # 标签、分类和友情链接三個页面需要配置(必填)
description: Lombok是一个 Java 库，可自动插入您的编辑器并构建工具，为您的 Java 增添趣味。永远不要再编写另一个getter或equals方法，通过一个注释，您的类就有一个功能齐全的构建器，自动化您的日志变量等等。                   # 描述
keywords: lombok                  # 关键词，便于搜索
top_img: /images/springboot/lombok.jpg             # 文章的顶部图片
aside: true                                                                         # 展示文章侧边栏(默认为true)
categories: 
  - 教程
  - lombok
  - springboot                                                                # 文章标签
  - java
cover: /images/springboot/lombok.jpg                 # 文章的缩略图（用在首页）
---

# 安装lombok
1. 引入jar包
  ```
    <dependency>
          <groupId>org.projectlombok</groupId>
          <artifactId>lombok</artifactId>
          <version>1.16.18</version>
          <scope>provided</scope>
    </dependency>

  ```
2. 启用lombok
  ```
    File => Settings => Build, Execution, Deployment => Compiler => Annotation Processors

    勾选 Enable annotation processing
  ```
3. 安装lombok插件
  ```
    File => Settings => Plugins, 搜索lombok下载
  ```
# 代码编写
## 构造函数相关：@AllArgsConstructor、@NoArgsConstructor、@RequiredArgsConstructor
   1. @AllArgsConstructor: 包含所有的构造函数
    ```
      # 新建UserInfo.class类

        @AllArgsConstructor //包含所有的构造函数
        public class UserInfo {
          private Long id;
          private String name;
          private String phone;
          private String address;
        }

      # 新建使用方法App.class
        public class App {
          public static void main(String[] args) {
            // 使用@AllArgsConstructor注解后，UserInfo类必须把所有的构造函数都写出来，否则报错
            UserInfo userInfo = new UserInfo(1L, "zhangsan", "17815182648", "2020");
          }
        }
    ```
      * 该注解的默认是public，如果要设置private提供参数access
        ```
          @AllArgsConstructor(access = AccessLevel.PRIVATE): 私有的构造函数
        ```

   2. @NoArgsConstructor: 生成无参构造函数
    ```
      # 新建UserInfo.class类

        @NoArgsConstructor // 生成无参构造函数
        public class UserInfo {
          private Long id;
          private String name;
          private String phone;
          private String address;
        }

      # 新建使用方法App.class
        public class App {
          public static void main(String[] args) {
            UserInfo userInfo = new UserInfo();
          }
        }
    ```

   3. @RequiredArgsConstructor: 生成必须通过构造函数传过来的属性
    ```
      # 新建UserInfo.class类

        @RequiredArgsConstructor // 生成必须通过构造函数传过来的属性
        public class UserInfo {
          private Long id;
          private String name;
          private String phone;
          private String address;
          /**
          * @RequiredArgsConstructor 包含两种属性
          *
          */
          @NonNull // 1. 当属性被加了@NonNull,则该属性会被包含在构造函数中
          private String notNullProp;
          // 2. final: 定义的时候初始化/构造函数中初始化，当加了final,则该属性会被包含在构造函数中
          private final String finalProp;
        }

      # 新建使用方法App.class
        public class App {
          public static void main(String[] args) {
            UserInfo userInfo = new UserInfo("123", "456");
          }
        }
    ```

## @Getter、@Setter
  * 自动生成成员变量的get和set方法
    ```
      # 新建UserInfo2.class类

        @Setter @Getter // 自动生成成员变量的get和set方法
        public class UserInfo2 {
            private Long id;
            private String name;
            private String phone;
            private String address;
        }

      # 新建使用方法App.class
        public class App {
          public static void main(String[] args) {
            UserInfo2 userInfo2 = new UserInfo2();
          }
        }
    ```
  
  * 如果要设置某个成员变量不生成get或set方法，使用value属性
    ```
      @Setter @Getter // 自动生成成员变量的get和set方法
      public class UserInfo2 {
        @Getter(value = AccessLevel.NONE)
        private Long id;
        private String name;
        private String phone;
        @Getter(value = AccessLevel.NONE) // none表示不生成get方法
        @Setter(value = AccessLevel.NONE) // none表示不生成set方法
        private String address;
      }
    ```

  * 如果自己写构造方法，优先级更高
    ```
      @Setter @Getter // 自动生成成员变量的get和set方法
      public class UserInfo2 {
          @Getter(value = AccessLevel.NONE)
          private Long id;
          private String name;
          private String phone;
          @Getter(value = AccessLevel.NONE) // none表示不生成get方法
          @Setter(value = AccessLevel.NONE) // none表示不生成set方法
          private String address;

          // 自己写的优先级更高
          public String getName() {
              return "我自己写的方法：" +name;
          }
      }
    ```

## @ToString
  * 生成toString的成员方法
    ```
      # 新建UserInfo3.class类

        @Setter
        @Getter
        @ToString // 生成toString方法
        public class UserInfo3 {
            private Long id;
            private String name;
            private String phone;
            private String address;
        }

      # 新建使用方法App.class
        public class App {
          public static void main(String[] args) {
            UserInfo3 userInfo3 = new UserInfo3();
            userInfo3.setId(1L);
            userInfo3.setAddress("渝中区");
            userInfo3.setName("张三");
            userInfo3.setPhone("17815182648");
            System.out.println(userInfo3);
            // 打印结果：UserInfo3(id=1, name=张三, phone=17815182648, address=渝中区)
          }
        }
    ```

  * 可接收参数：of、exclude
    - of: 包含的元素
      ```
        @ToString(of = {"id", "name"}) // 生成toString方法, 包含id和name
        public class UserInfo3 {
            private Long id;
            private String name;
            private String phone;
            private String address;
        }
      ```
    - exclude: 排除的元素
      ```
        @ToString(exclude = {"name"}) // 生成toString方法, 排除name
        public class UserInfo3 {
            private Long id;
            private String name;
            private String phone;
            private String address;
        }
      ```

    - callSuper: 指定调用父类的toString + 本类的toString
      ```
        @ToString(callSuper = true)
        public class UserInfo3 {
            private Long id;
            private String name;
            private String phone;
            private String address;
        }
      ```

## @EqualsAndHashCode
  * 生成equals和hashcode方法
    ```
      # 新建UserInfo4.class类
        @Setter
        @Getter
        @ToString
        @EqualsAndHashCode // 生成equals和hashcode方法
        public class UserInfo4 {
          private Long id;
          private String name;
          private String phone;
          private String address;
        }

      # 新建使用方法App.class
        public class App {
          public static void main(String[] args) {
            UserInfo4 userInfo4 = new UserInfo4();
            userInfo4.setId(1L);
            userInfo4.setAddress("渝中区");
            userInfo4.setName("张三");
            userInfo4.setPhone("17815182648");

            UserInfo4 userInfo4_2 = new UserInfo4();
            userInfo4_2.setId(1L);
            userInfo4_2.setAddress("渝中区");
            userInfo4_2.setName("张三");
            userInfo4_2.setPhone("17815182648");

            Set<UserInfo4> set = new HashSet<>();
            set.add(userInfo4);
            set.add(userInfo4_2);
            System.out.println(set.size());
            // 输出结果：1
          }
        }
    ```
  * 可选参数：of
    - of: 只根据of的参数生成equals和hashcode方法
      ```
        # 新建UserInfo4.class类

          @Setter
          @Getter
          @ToString
          @EqualsAndHashCode(of = {"id"}) // 只使用id,生成equals和hashcode方法
          public class UserInfo4 {
              private Long id;
              private String name;
              private String phone;
              private String address;
          }

        # 此时，如果不改变id的值，set.size()输出的值始终是1
      ```

## @Data
  * 相当于@Setter + @Getter + @EqualsAndHashCode + @ToString
    ```
      # 新建UserInfoData.class类
        @Data // 相当于@Setter + @Getter + @EqualsAndHashCode + @ToString
        public class UserInfoData {
            private Long id;
            private String name;
            private String phone;
            private String address;
        }

      # 新建使用方法App.class
        public class App {
          public static void main(String[] args) {
            UserInfoData userInfoData = new UserInfoData();
            userInfoData.setId(1L);
            userInfoData.setAddress("渝中区");
            userInfoData.setName("张三");
            userInfoData.setPhone("1781518268");
            System.out.println(userInfoData);
          }
        }
    ```

  * 如果要指定比如equals方法按照id去重等，在@Data后面再写个@EqualsAndHashCode覆盖即可
    ```
      @Data
      @EqualsAndHashCode(of = {"id"})
      public class UserInfoData {
        private Long id;
        private String name;
        private String phone;
        private String address;
      }
    ```

## @Accessors
  * 可选参数：chain、fluent
    - chain：开启链式编程，可以直接使用成员方法
      ```
        # 新建UserInfoAccessors.class类
          @Data
          @Accessors(chain = true) // 开启链式编程,需要依赖@Data或者@Setter等
          public class UserInfoAccessors {
              private Long id;
              private String name;
              private String phone;
              private String address;
          }

        # 新建使用方法App.class
          public class App {
            public static void main(String[] args) {
              UserInfoAccessors userInfoAccessors = new UserInfoAccessors();
              // 链式编程
              userInfoAccessors.setId(2L).setName("李四").setAddress("渝中区").setPhone("15310902437");
              System.out.println(userInfoAccessors);
            }
          }
      ```

    - fluent: 开启链式编程，可以直接使用属性名做方法名
      ```
        # 新建UserInfoAccessors.class类
          @Data
          @Accessors(fluent = true) // 链式编程，直接使用属性名做方法名
          public class UserInfoAccessors {
              private Long id;
              private String name;
              private String phone;
              private String address;
          }

        # 新建使用方法App.class
          public class App {
            public static void main(String[] args) {
              UserInfoAccessors userInfoAccessors = new UserInfoAccessors();
              // 链式编程
              userInfoAccessors.id(3L).address("小什字").phone("15310902437").name("王麻子");
              System.out.println(userInfoAccessors);
            }
          }
      ```

## @Builder
  * 使用该注解之后，代码变成了见证者的设计模式
    ```
      # 新建UserInfoBuilder.class类
        @Builder
        public class UserInfoBuilder {
            private Long id;
            private String name;
            private String phone;
            private String address;
        }

      # 新建使用方法App.class
        public static void main(String[] args) {
          UserInfoBuilder.UserInfoBuilderBuilder builder = UserInfoBuilder.builder();
          builder.id(4L).address("江南").name("赵武").phone("13638348793");
          System.out.println(builder);
          // 输出结果：UserInfoBuilder.UserInfoBuilderBuilder(id=4, name=赵武, phone=13638348793, address=江南)
        }
    ```
  
## @Slf4j
  * 记录日志
    - 为了测试，在pom.xml添加logback的依赖
      ```
        <!--添加log-back日志组件-->
        <dependency>
          <groupId>ch.qos.logback</groupId>
          <artifactId>logback-classic</artifactId>
          <version>1.2.3</version>
        </dependency>
      ```

    - 没用使用@Slf4j注解的写法：
      ```
        # 新建UserInfoService.class类
          package com.dj.app.log;

          import com.dj.app.UserInfo;
          import org.slf4j.Logger;
          import org.slf4j.LoggerFactory;

          import java.util.ArrayList;
          import java.util.List;

          public class UserInfoService {
              private static final Logger LOGGER = LoggerFactory.getLogger(UserInfoService.class);
              public List<UserInfo> getAll() {
                  LOGGER.info("进入getAll方法11");
                  return new ArrayList<>();
              }
          }

        # 新建使用方法App.class
          package com.dj.app.log;

          public class App {
              public static void main(String[] args) {
                  UserInfoService userInfoService = new UserInfoService();
                  userInfoService.getAll();
              }
          }
      ```

    - 使用@Slf4j注解后：
      ```
        # 新建UserInfoService.class类
          package com.dj.app.log;

          import com.dj.app.UserInfo;
          import lombok.extern.slf4j.Slf4j;

          import java.util.ArrayList;
          import java.util.List;

          @Slf4j
          public class UserInfoService {
              public List<UserInfo> getAll() {
                  log.info("进入getAll方法11");
                  return new ArrayList<>();
              }
          }
      ```
## @Log
  * 记录日志，用法和@Slf4j完全一样，它更通用
    - 为了测试，在pom.xml添加logback的依赖
      ```
        <!--添加log-back日志组件-->
        <dependency>
          <groupId>ch.qos.logback</groupId>
          <artifactId>logback-classic</artifactId>
          <version>1.2.3</version>
        </dependency>
      ```

    - 没用使用@Log注解的写法：
      ```
        # 新建UserInfoService.class类
          package com.dj.app.log;

          import com.dj.app.UserInfo;
          import org.slf4j.Logger;
          import org.slf4j.LoggerFactory;

          import java.util.ArrayList;
          import java.util.List;

          public class UserInfoService {
              private static final Logger LOGGER = LoggerFactory.getLogger(UserInfoService.class);
              public List<UserInfo> getAll() {
                  LOGGER.info("进入getAll方法11");
                  return new ArrayList<>();
              }
          }

        # 新建使用方法App.class
          package com.dj.app.log;

          public class App {
              public static void main(String[] args) {
                  UserInfoService userInfoService = new UserInfoService();
                  userInfoService.getAll();
              }
          }
      ```

    - 使用@Log注解后：
      ```
        # 新建UserInfoService.class类
        package com.dj.app.log;

        import com.dj.app.UserInfo;
        import lombok.extern.java.Log;

        import java.util.ArrayList;
        import java.util.List;

        @Log
        public class UserInfoService {
            public List<UserInfo> getAll() {
                log.info("进入getAll方法11");
                return new ArrayList<>();
            }
        }
      ```

## val
  * 类似js中的var，可以推算变量的类型
    ```
      package com.dj.app.log;
      import lombok.val;
      import java.util.HashMap;

      public class App {
          public static void main(String[] args) {
            // 使用val推算map的类型为HashMap
            val map = new HashMap<String, String>();
            map.put("name", "张三");
            map.put("age", "18");
            System.out.println(map.size());
          }
      }
    ```

## @Cleanup
  * 简化io流，关闭的相关操作
    ```
      package com.dj.app.io;
      import lombok.Cleanup;

      import java.io.FileWriter;
      import java.io.IOException;

      public class Demo {
          public static void main(String[] args) throws IOException {
              @Cleanup
              FileWriter fw = new FileWriter("d:\\a.txt");
              fw.write("hello,world1");
              fw.flush();
          }
      }
    ```