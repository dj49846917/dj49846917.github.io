---
title: shiro
date: 2021-10-26 14:19:11
tags:
---

# Shiro简介
## 什么是shiro
  * Apache Shiro是java的一个安全框架。Shiro可以非常容易的开发出足够好的应用，其不仅可以用在JavaSE环境，也可以用在JavaEE环境。Shiro可以帮助我们完成：认证、授权、加密、会话管理、与web集成、缓存等
  * Shiro官方网站：https://shiro.apache.org

## 为什么要学shiro
  * 既然shiro将安全认证相关的功能抽取出来组成一个框架。使用shiro就可以非常快速的完成认证、授权等功能的开发，降低系统成本。（开发和维护）
  * shiro使用广泛，shiro可以运行在web应用，非web应用，集群分布式应用中越来越多的用户开始使用shiro。

## 基本功能
  * ![基本功能](/images/shiro/基本功能.jpg)

### Authentication
  > 身份认证/登录，验证用户是不是拥有相应的身份

### Authorization
  > 授权，即权限授权，验证某个已认证的用户是否拥有某个权限：即判断用不是否能做事情，常见的如：验证某个用户是否拥有某个教师。或者细粒度的验证某个用户对某个资源是否具有某个权限

### Session Manager
  * 会话管理，即用户登录后就是一次护花，在没有退出之前，他的所有信息都在会话中；会话可以是普通javaSe环境的，也可以是如Web环境的；
  * **Cryptography:** 加密，保护数据的安全性，如密码加密存储到数据库，而不是明文存储；
  * **Web Support:** Web支持，可以非常容易的集成到web环境中
  * **Caching:** 缓存，比如用户登录后，其用户信息，拥有的角色/权限不必每次去查，这样可以提高效率；
  * **Concurrency:** shiro支持多线程应用的并发验证，即如在一个县城中开启另一个线程，能把权限自动传播出去；
  * **Testing:** 提供测试支持
  * **Run As:** 允许一个用户假装成另一个用户（如果他们允许）的身份进行访问；
  * **Remember Me:** 记住我，这个是非常常见的功能，及一次登录后，下次再来的话不用登录了
{% note danger %}
  说明: shiro不会去维护用户、维护权限；这些需要我们自己去设计/提供；然后通过相应的接口注入给shiro即可
{% endnote %}

# Shiro认证
## 基本概念
### 身份验证
  > 即在应用中谁能证明他就是他本人。一般提供如他们的身份ID，一些标识信息来表名他就说他本人，如通过身份证，用户名/密码来证明。
  > 在shiro中，用户需要提供principals（身份）和cradentials（证明）给shiro，从而应用能验证用户身份；

### principals
  > 身份，即主体的标识属性，可以是任何东西，如用户名、邮箱等，唯一即可。
  > 一个主体可以有多个principals，但只有一个Primary principals，一般是用户名/密码/手机号

### credentials
  > 证明/凭证，即只有主体知道的安全值，如密码/数字证书等
  > <span style="background-color: red;">最常见的principals和credentials组合就是用户名/密码了。接下来先进行一个基本的身份认证</span>

## 认证流程
  * ![认证流程](/images/shiro/认证流程.jpg)

## shiro入门程序工程 环境
  1. 新建一个名为shiro-test的空项目，再创建一个shiro-demo01的module
  2. 在pom.xml中导入坐标
    ```
      <dependency>
          <groupId>org.apache.shiro</groupId>
          <artifactId>shiro-core</artifactId>
          <version>1.3.2</version>
      </dependency>

      <dependency>
          <groupId>org.apache.shiro</groupId>
          <artifactId>shiro-web</artifactId>
          <version>1.3.2</version>
      </dependency>

      <dependency>
          <groupId>org.apache.shiro</groupId>
          <artifactId>shiro-spring</artifactId>
          <version>1.3.2</version>
      </dependency>
      <!-- 日志 -->
      <dependency>
          <groupId>org.slf4j</groupId>
          <artifactId>slf4j-api</artifactId>
          <version>1.7.32</version>
      </dependency>
      <dependency>
          <groupId>log4j</groupId>
          <artifactId>log4j</artifactId>
          <version>1.2.17</version>
      </dependency>
      <dependency>
          <groupId>org.slf4j</groupId>
          <artifactId>slf4j-log4j12</artifactId>
          <version>1.7.32</version>
      </dependency>
      <dependency>
          <groupId>junit</groupId>
          <artifactId>junit</artifactId>
          <version>4.13.2</version>
          <scope>test</scope>
      </dependency>
    ```
  3. 在src/main/resources下新建shiro.ini文件，注意创建方式选择properties
    ```
      # 配置用户
      [users]
      # 用户名=密码
      lucy=abc123
      lili=321
    ```
  4. 编写测试类，在test/java下新建com.shiro.test.ShiroTest.java
    ```
      package com.shiro;
      import org.apache.shiro.SecurityUtils;
      import org.apache.shiro.authc.IncorrectCredentialsException;
      import org.apache.shiro.authc.UnknownAccountException;
      import org.apache.shiro.authc.UsernamePasswordToken;
      import org.apache.shiro.config.IniSecurityManagerFactory;
      import org.apache.shiro.mgt.SecurityManager;
      import org.apache.shiro.subject.Subject;
      import org.junit.Test;
      public class ShiroTest {
          @Test
          public void shiroTest() {
              // 1.读取配置文件，创建安全管理器：SecurityManager
              IniSecurityManagerFactory factory = new IniSecurityManagerFactory("classpath:shiro.ini");
              SecurityManager securityManager = factory.createInstance();

              // 2.将工厂对象和当前线程绑定
              SecurityUtils.setSecurityManager(securityManager);

              // 3.从当前线程获取主体对象：Subject
              Subject subject = SecurityUtils.getSubject();

              // 4.判断是否认证
              boolean authenticated = subject.isAuthenticated();
              System.out.println("认证前："+authenticated);
              if(!authenticated) {
                  // 5.创建Token(令牌)：封装身份(账号)和凭证(密码)
                  UsernamePasswordToken usernamePasswordToken = new UsernamePasswordToken("lucy", "abc123");
                  try{
                      // 6. 认证(登录)
                      subject.login(usernamePasswordToken);
                      authenticated = subject.isAuthenticated();
                      System.out.println("认证后："+authenticated);
                  }catch (UnknownAccountException e) {
                      System.out.println("亲，账号不存在");
                  } catch(IncorrectCredentialsException e) {
                      System.out.println("亲，账号错误");
                  }
                  // 获取身份信息
                  Object principal = subject.getPrincipal();
                  System.out.println(principal);
                  // 退出认证
                  subject.logout();
              }
          }
      }
    ```

## 自定义realm
  > 什么是reaml？
  > Reaml: 领域，范围，通俗讲，你可以理解为一个特殊的dao,他主要用于对认证信息和授权信息的存取和操作，实际开发总主要调用开发者开发操作数据库相关service获取dao来进行授权认证
  > 将来实际开发需要realm从数据库中查询用户信息

