---
title: spring整合springSecurity
date: 2021-08-04 09:28:30
tags:
  - spring Security
type: spring Security
description: Spring Security 是一个功能强大且高度可定制的身份验证和访问控制框架。
keywords: Spring Security
top_img: /images/springSecurity/log.jpg
aside: true
categories:
  - 教程
  - springSecurity
cover: /images/springSecurity/log.jpg
---

# 案例介绍
  * 项目基础代码在https://gitee.com/dujiang1992/spring-security-demo
## 案例效果图
### 启动项目进入首页
  * ![效果展示](/images/springSecurity/准备.jpg)

### 系统管理界面
  * ![效果展示](/images/springSecurity/准备2.jpg)

### 基础数据界面
  * ![效果展示](/images/springSecurity/准备3.jpg)

### 项目最终目录结构
  * ![效果展示](/images/springSecurity/准备4.jpg)
  
## 建表语句
  * 在mysql中新建一个security_authority的表，再执行下面的sql语句
    ```
      DROP TABLE IF EXISTS `sys_permission`;

      CREATE TABLE `sys_permission` (
        `ID` int(11) NOT NULL AUTO_INCREMENT COMMENT '编号',
        `permission_NAME` varchar(30) DEFAULT NULL COMMENT '菜单名称',
        `permission_url` varchar(100) DEFAULT NULL COMMENT '菜单地址',
        `parent_id` int(11) NOT NULL DEFAULT '0' COMMENT '父菜单id',
        PRIMARY KEY (`ID`)
      ) ENGINE=InnoDB DEFAULT CHARSET=utf8;

      DROP TABLE IF EXISTS `sys_role`;

      CREATE TABLE `sys_role` (
        `ID` int(11) NOT NULL AUTO_INCREMENT COMMENT '编号',
        `ROLE_NAME` varchar(30) DEFAULT NULL COMMENT '角色名称',
        `ROLE_DESC` varchar(60) DEFAULT NULL COMMENT '角色描述',
        PRIMARY KEY (`ID`)
      ) ENGINE=InnoDB AUTO_INCREMENT=2 DEFAULT CHARSET=utf8;

      DROP TABLE IF EXISTS `sys_role_permission`;

      CREATE TABLE `sys_role_permission` (
        `RID` int(11) NOT NULL COMMENT '角色编号',
        `PID` int(11) NOT NULL COMMENT '权限编号',
        PRIMARY KEY (`RID`,`PID`),

        KEY `FK_Reference_12` (`PID`),
        CONSTRAINT `FK_Reference_11` FOREIGN KEY (`RID`) REFERENCES `sys_role` (`ID`),
        CONSTRAINT `FK_Reference_12` FOREIGN KEY (`PID`) REFERENCES `sys_permission` (`ID`)
      ) ENGINE=InnoDB DEFAULT CHARSET=utf8;

      DROP TABLE IF EXISTS `sys_user`;

      CREATE TABLE `sys_user` (
        `id` int(11) NOT NULL AUTO_INCREMENT,
        `username` varchar(32) NOT NULL COMMENT '用户名称',
        `password` varchar(120) CHARACTER SET utf8 COLLATE utf8_general_ci NOT NULL COMMENT '密码',
        `status` int(1) DEFAULT '1' COMMENT '1开启0关闭',
        PRIMARY KEY (`id`)
      ) ENGINE=InnoDB AUTO_INCREMENT=2 DEFAULT CHARSET=utf8;

      DROP TABLE IF EXISTS `sys_user_role`;

      CREATE TABLE `sys_user_role` (
        `UID` int(11) NOT NULL COMMENT '用户编号',
        `RID` int(11) NOT NULL COMMENT '角色编号',
        PRIMARY KEY (`UID`,`RID`),
        KEY `FK_Reference_10` (`RID`),
        CONSTRAINT `FK_Reference_10` FOREIGN KEY (`RID`) REFERENCES `sys_role` (`ID`),
        CONSTRAINT `FK_Reference_9` FOREIGN KEY (`UID`) REFERENCES `sys_user` (`id`)
      ) ENGINE=InnoDB DEFAULT CHARSET=utf8;
    ```

## 页面部分所用技术简单说明
### 前端使用adminlTE
  * AdminLTE是一款基于Bootstrap的页面模板，可以快速构建出一套美观的后台管理页面。
  * 官网地址：https://adminlte.io/
  * 下载地址：https://github.com/ColorlibHQ/AdminLTE/releases

### 后台部分所用技术简单说明
  * 后台代码采用springmvc实现web层，spring控制业务层事务，mybatis操作数据库，这三个框架大家一定非常熟悉了，这里我就不再赘述！

# 初识权限管理
## 权限管理概念
  * 权限管理，一般指根据系统设置的安全规则或者安全策略，用户可以访问而且只能访问自己被授权的资源。权限管
理几乎出现在任何系统里面，前提是需要有用户和密码认证的系统。
{% note info %}
  在权限管理的概念中，有两个非常重要的名词：
  认证：通过用户名和密码成功登陆系统后，让系统得到当前用户的角色身份。
  授权：系统根据当前用户的角色，给其授予对应可以操作的权限资源
{% endnote %}

## 完成权限管理需要三个对象
  * 用户：主要包含用户名，密码和当前用户的角色信息，可实现认证操作。
  * 角色：主要包含角色名称，角色描述和当前角色拥有的权限信息，可实现授权操作。
  * 权限：权限也可以称为菜单，主要包含当前权限名称，url地址等信息，可实现动态展示菜单。
{% note info %}
  注：这三个对象中，用户与角色是多对多的关系，角色与权限是多对多的关系，用户与权限没有直接关系，二者是通过角色来建立关联关系的。
{% endnote %}

# 初识Spring Security
## Spring Security概念
  * Spring Security是spring采用AOP思想，基于servlet过滤器实现的安全框架。它提供了完善的认证机制和方法级的授权功能。是一款非常优秀的权限管理框架。

## Spring Security简单入门
  * Spring Security博大精深，设计巧妙，功能繁杂，一言难尽，咱们还是直接上代码吧！

### 创建web工程并导入jar包
{% note info %}
  Spring Security主要jar包功能介绍
  spring-security-core.jar
  核心包，任何Spring Security功能都需要此包。
  spring-security-web.jar
  web工程必备，包含过滤器和相关的Web安全基础结构代码。
  spring-security-config.jar
  用于解析xml配置文件，用到Spring Security的xml配置文件的就要用到此包。
  spring-security-taglibs.jar
  Spring Security提供的动态标签库，jsp页面可以用
{% endnote %}
  * 在pom.xml中导入对应的包
    ```
      <dependency>
          <groupId>org.springframework.security</groupId>
          <artifactId>spring-security-taglibs</artifactId>
          <version>5.1.5.RELEASE</version>
      </dependency>
      <dependency>
          <groupId>org.springframework.security</groupId>
          <artifactId>spring-security-config</artifactId>
          <version>5.1.5.RELEASE</version>
      </dependency>
    ```

### 配置web.xml
  * 找到webapp/WEB-INF/web.xml
    ```
      <web-app xmlns="http://java.sun.com/xml/ns/javaee"
      xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
      xsi:schemaLocation="http://java.sun.com/xml/ns/javaee
      http://java.sun.com/xml/ns/javaee/web-app_3_0.xsd"
      version="3.0">
          <display-name>Archetype Created Web Application</display-name>
          
          <!--Spring Security过滤器链，注意过滤器名称必须叫springSecurityFilterChain-->
          <filter>
              <!-- 名称springSecurityFilterChain固定，不能修改 -->
              <filter-name>springSecurityFilterChain</filter-name>
              <filter-class>org.springframework.web.filter.DelegatingFilterProxy</filter-class>
          </filter>
          <filter-mapping>
              <filter-name>springSecurityFilterChain</filter-name>
              <url-pattern>/*</url-pattern>
          </filter-mapping>
      </web-app>
    ```

### 向数据库中的sys_role表添加一条数据
  ```
    INSERT INTO `sys_role` VALUES ('6', 'ROLE_USER', '基本角色');
  ```

### 配置spring-security.xml
  * 在resources下新建spring-security.xml
    ```
      <?xml version="1.0" encoding="UTF-8"?>
      <beans xmlns="http://www.springframework.org/schema/beans"
            xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
            xmlns:security="http://www.springframework.org/schema/security"
            xsi:schemaLocation="
            http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd
            http://www.springframework.org/schema/security http://www.springframework.org/schema/security/spring-security.xsd
      ">
          <!-- 配置springSecurity -->
          <!--
              auto-config="true"  表示自动加载springSecurity配置文件
              use-expressions="true"  表示使用spring的el表达式来配置springSecurity
          -->
          <security:http auto-config="true" use-expressions="true">
              <!-- 拦截资源 -->
              <!--
                  "ROLE_USER"     是数据库sys_role里面的一条数据的ROLE_NAME
                  pattern="/**"   表示拦截所有资源(spring中两个*表示所有，第一个*表示路径，第二个*表示子目录以及后面的目录)
                  access="hasAnyAuthority('ROLE_USER')"   表示只有ROLE_USER角色才能访问资源
                  hasAnyAuthority()   可以多个角色
                  hasRole()   1个角色
              -->
              <security:intercept-url pattern="/**" access="hasAnyAuthority('ROLE_USER')" />
          </security:http>

          <!--设置Spring Security认证用户信息的来源-->
          <!-- springSecurity默认的认证必须是加密的，加上{noop} 表示不加密认证 -->
          <security:authentication-manager>
              <security:authentication-provider>
                  <security:user-service>
                      <security:user name="user" password="{noop}user"
                                    authorities="ROLE_USER" />
                      <security:user name="admin" password="{noop}admin"
                                    authorities="ROLE_ADMIN" />
                  </security:user-service>
              </security:authentication-provider>
          </security:authentication-manager>
      </beans>
    ```

### 将spring-security.xml配置文件引入到applicationContext.xml中
  * 找到resources下的applicationContext.xml
    ```
      <!--引入SpringSecurity主配置文件-->
      <import resource="classpath:spring-security.xml"/>
    ```

### 运行结果
  * 好了！开始启动项目了，万众期待看到index.jsp中的内容！
  * ![结果](/images/springSecurity/结果.jpg)
{% note info %}
  唉！？说好的首页呢！？为何生活不是我想象！？
  地址栏中login处理器谁写的！？这个带有歪果仁文字的页面哪来的！？这么丑！？我可以换了它吗！？
  稍安勿躁……咱们先看看这个页面源代码，真正惊心动魄的还在后面呢……
  这不就是一个普通的form表单吗？除了那个_csrf的input隐藏文件！
  注意！这可是你想使用自定义页面时，排查问题的一条重要线索！
  ![结果](/images/springSecurity/结果2.jpg)

  我们再去看看控制台发生了什么，这里我偷偷在项目中加了日志包和配置文件……
  惊不惊喜！？意不意外！？哪来这么多过滤器啊！？
  ![结果](/images/springSecurity/结果3.jpg)

  **最后，我们在这个登录页面上输入用户名user，密码user，点击Sign in，好了，总算再次看到首页了！**
{% endnote %}

# Spring Security过滤器链
## Spring Security常用过滤器介绍
  1. org.springframework.security.web.context.SecurityContextPersistenceFilter
    > 首当其冲的一个过滤器，作用之重要，自不必多言。
    > SecurityContextPersistenceFilter主要是使用SecurityContextRepository在session中保存或更新一个SecurityContext，并将SecurityContext给以后的过滤器使用，来为后续filter建立所需的上下文。
    > SecurityContext中存储了当前用户的认证以及权限信息。
  2. org.springframework.security.web.context.request.async.WebAsyncManagerIntegrationFilter
    > 此过滤器用于集成SecurityContext到Spring异步执行机制中的WebAsyncManager
  3. org.springframework.security.web.header.HeaderWriterFilter
    > 向请求的Header中添加相应的信息,可在http标签内部使用security:headers来控制
  4. org.springframework.security.web.csrf.CsrfFilter
    > csrf又称跨域请求伪造，SpringSecurity会对所有post请求验证是否包含系统生成的csrf的token信息，如果不包含，则报错。起到防止csrf攻击的效果。
  5. org.springframework.security.web.authentication.logout.LogoutFilter
    > 匹配URL为/logout的请求，实现用户退出,清除认证信息。
  6. org.springframework.security.web.authentication.UsernamePasswordAuthenticationFilter
    > 认证操作全靠这个过滤器，默认匹配URL为/login且必须为POST请求。
  7. org.springframework.security.web.authentication.ui.DefaultLoginPageGeneratingFilter
    > 如果没有在配置文件中指定认证页面，则由该过滤器生成一个默认认证页面。
  8. org.springframework.security.web.authentication.ui.DefaultLogoutPageGeneratingFilter
    > 由此过滤器可以生产一个默认的退出登录页面
  9. org.springframework.security.web.authentication.www.BasicAuthenticationFilter
    > 此过滤器会自动解析HTTP请求中头部名字为Authentication，且以Basic开头的头信息。
  10. org.springframework.security.web.savedrequest.RequestCacheAwareFilter
    > 通过HttpSessionRequestCache内部维护了一个RequestCache，用于缓存HttpServletRequest
  11. org.springframework.security.web.servletapi.SecurityContextHolderAwareRequestFilter
    > 针对ServletRequest进行了一次包装，使得request具有更加丰富的API
  12. org.springframework.security.web.authentication.AnonymousAuthenticationFilter
    > 当SecurityContextHolder中认证信息为空,则会创建一个匿名用户存入到SecurityContextHolder中。spring security为了兼容未登录的访问，也走了一套认证流程，只不过是一个匿名的身份。
  13. org.springframework.security.web.session.SessionManagementFilter
    > SecurityContextRepository限制同一用户开启多个会话的数量
  14. org.springframework.security.web.access.ExceptionTranslationFilter
    > 异常转换过滤器位于整个springSecurityFilterChain的后方，用来转换整个链路中出现的异常
  15. org.springframework.security.web.access.intercept.FilterSecurityInterceptor
    > 获取所配置资源访问的授权信息，根据SecurityContextHolder中存储的用户信息来决定其是否有权限。
{% note warning %}
  那么，是不是spring security一共就这么多过滤器呢？答案是否定的！随着spring-security.xml配置的添加，还会出现新的过滤器。
  那么，是不是spring security每次都会加载这些过滤器呢？答案也是否定的！随着spring-security.xml配置的修改，有些过滤器可能会被去掉。
{% endnote %}

# SpringSecurity使用自定义认证页面
## 在SpringSecurity主配置文件中指定认证页面配置信息
  1. 修改resources/spring-security.xml
    ```
      <?xml version="1.0" encoding="UTF-8"?>
      <beans xmlns="http://www.springframework.org/schema/beans"
            xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
            xmlns:security="http://www.springframework.org/schema/security"
            xsi:schemaLocation="
            http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd
            http://www.springframework.org/schema/security http://www.springframework.org/schema/security/spring-security.xsd
      ">
          <!--直接释放无需经过SpringSecurity过滤器的静态资源-->
          <security:http pattern="/css/**" security="none"/>
          <security:http pattern="/img/**" security="none"/>
          <security:http pattern="/plugins/**" security="none"/>
          <security:http pattern="/failer.jsp" security="none"/>
          <security:http pattern="/favicon.ico" security="none"/>

          <!-- 配置springSecurity -->
          <!--
              auto-config="true"      表示自动加载springSecurity配置文件
              use-expressions="true"  表示使用spring的el表达式来配置springSecurity
          -->
          <security:http auto-config="true" use-expressions="true">
              <!-- 让认证页面可以匿名访问 -->
              <!--
                  access="permitAll()"    无条件允许访问
              -->
              <security:intercept-url pattern="/login.jsp" access="permitAll()"></security:intercept-url>

              <!-- 拦截资源 -->
              <!--
                  "ROLE_USER"                             是数据库sys_role里面的一条数据的ROLE_NAME
                  pattern="/**"                           表示拦截所有资源(spring中两个*表示所有，第一个*表示路径，第二个*表示子目录以及后面的目录)
                  access="hasAnyAuthority('ROLE_USER')"   表示只有ROLE_USER角色才能访问资源
                  hasAnyAuthority()                       可以多个角色
                  hasRole()                               1个角色
              -->
              <security:intercept-url pattern="/**" access="hasAnyAuthority('ROLE_USER')" />
              <!-- 配置认证信息 -->
              <!--
                  login-page="/login.jsp"                     自己的登录页面
                  login-processing-url="/login"               指定认证的处理器地址(也就是默认security启动的登录地址)
                  default-target-url="/index.jsp"             认证通过默认跳转的url路径，如果有路径这个就没用了
                  authentication-failure-url="/failer.jsp"    认证失败跳转的url路径
              -->
              <security:form-login login-page="/login.jsp"
                                  login-processing-url="/login"
                                  default-target-url="/index.jsp"
                                  authentication-failure-url="/failer.jsp" />
              <!-- 配置退出登录信息 -->
              <!--
                  logout-url="/logout"            指定退出的处理器地址(也就是默认security启动的退出地址)
                  logout-success-url="/login.jsp" 退出后跳转的url路径
              -->
              <security:logout logout-url="/logout" logout-success-url="/login.jsp"></security:logout>
          </security:http>

          <!--设置Spring Security认证用户信息的来源-->
          <!-- springSecurity默认的认证必须是加密的，加上{noop} 表示不加密认证 -->
          <security:authentication-manager>
              <security:authentication-provider>
                  <security:user-service>
                      <security:user name="user" password="{noop}user"
                                    authorities="ROLE_USER" />
                      <security:user name="admin" password="{noop}admin"
                                    authorities="ROLE_ADMIN" />
                  </security:user-service>
              </security:authentication-provider>
          </security:authentication-manager>
      </beans>
    ```
  2. 修改认证页面的请求地址: 修改webapp/login.jsp
    - 修改前：
      ```
        <form action="${pageContext.request.contextPath}/login.jsp" method="post">
      ```
    - 修改后：
      ```
        <form action="${pageContext.request.contextPath}/login" method="post">
      ```
  3. 再次启动项目后就可以看到自定义的酷炫认证页面了 ![结果](/images/springSecurity/结果4.jpg)
  4. 然后输入了用户名user，密码user，就出现了如下的界面：![结果](/images/springSecurity/结果5.jpg)
{% note danger %}
  403什么异常？这是SpringSecurity中的权限不足！这个异常怎么来的？还记得上面SpringSecurity内置认证页面源码中的那个_csrf隐藏input吗？问题就在这了！
{% endnote %}

## SpringSecurity的csrf防护机制
  * CSRF（Cross-site request forgery）跨站请求伪造，是一种难以防范的网络攻击方式。

### SpringSecurity中CsrfFilter过滤器说明
  * 自己的认证页面，请求方式为POST，但却没有携带token，所以才出现了403权限不足的异常。那么如何处理这个问题呢？
    - 方式一：直接禁用csrf，不推荐。
    - 方式二：在认证页面携带token请求。

### 禁用csrf防护机制
  * 在resources/spring-security.xml主配置文件中添加禁用crsf防护的配置。
    ```
      <security:http auto-config="true" use-expressions="true">
          <!-- 去掉csrf拦截的过滤器 -->
          <security:csrf disabled="true"></security:csrf>
      </security:http>
    ```
    
### 在认证页面携带token请求
  1. 修改webapp/login.jsp ![token](/images/springSecurity/token.jpg)
  2. 注：HttpSessionCsrfTokenRepository对象负责生成token并放入session域中。
  3. 去掉前面删除csrf拦截过滤器的配置

# SpringSecurity使用数据库数据完成认证
## 初步实现认证功能
### 让我们自己的UserService接口继承UserDetailsService
  1. 在数据库的sys_user表添加一条数据
    ```
      INSERT INTO `sys_user` VALUES ('4', 'xiaoming', '123', '1');
    ```
  2. 找到service下的UserService.java，去继承UserDetailsService
    ```
      public interface UserService extends UserDetailsService {
        public void save(SysUser user);
        public List<SysUser> findAll();
        public Map<String, Object> toAddRolePage(Integer id);
        public void addRoleToUser(Integer userId, Integer[] ids);
      }
    ```
### 编写loadUserByUsername业务
  * 修改service.UserServiceImpl.java
    ```
      /**
       * 认证业务
       * @param username  用户在浏览器输入的用户名
       * @return UserDetails  是springSecurity自己的用户对象
       * @throws UsernameNotFoundException
       */
      @Override
      public UserDetails loadUserByUsername(String username) throws UsernameNotFoundException {
          try {
              // 根据用户名做查询
              SysUser sysUser = userDao.findByName(username);
              if(sysUser == null) {
                  return null;
              }
              List<SimpleGrantedAuthority> authorities = new ArrayList<>();
              authorities.add(new SimpleGrantedAuthority("ROLE_USER"));
              // {noop}后面的密码，springSecurity会认为是原文
              UserDetails userDetails = new User(sysUser.getUsername(), "{noop}" + sysUser.getPassword(), authorities);
              return userDetails;
          } catch(Exception e) {
              e.printStackTrace();
          }
          // 认证失败
          return null;
      }
    ```

### 在SpringSecurity主配置文件中指定认证使用的业务对象
  * 修改resources/spring-security.xml
    - 修改前：
      ```
        <security:authentication-manager>
            <security:authentication-provider>
                <security:user-service>
                    <security:user name="user" password="{noop}user"
                                  authorities="ROLE_USER" />
                    <security:user name="admin" password="{noop}admin"
                                  authorities="ROLE_ADMIN" />
                </security:user-service>
            </security:authentication-provider>
        </security:authentication-manager>
      ```
    - 修改后：
      ```
        <security:authentication-manager>
            <!-- userServiceImpl是service.impl.UserServiceImpl.java取首字母小写 -->
            <security:authentication-provider user-service-ref="userServiceImpl">
            </security:authentication-provider>
        </security:authentication-manager>
      ```

### 测试
  * 启动项目，输入用户名：xiaoming；密码：123; 提示登录成功

## 优化上面的代码，全部替换成数据库的数据
  1. 点击用户管理，修改角色，点击保存，发现报403
  2. 修改webapp/pages/user-role-add.jsp的代码
    ```
      <%@taglib uri="http://www.springframework.org/security/tags" prefix="security" %>

      <form action="${pageContext.request.contextPath}/user/addRoleToUser.do" method="post">
				<security:csrfInput/>
    ```
  3. 再次点击保存按钮，此时数据库中的sys_user_role就有了一条数据 ![结果](/images/springSecurity/结果6.jpg)
  4. 修改service.impl.UserServiceImpl.java
    ```
       /**
        * 认证业务
        * @param username  用户在浏览器输入的用户名
        * @return UserDetails  是springSecurity自己的用户对象
        * @throws UsernameNotFoundException
        */
       @Override
       public UserDetails loadUserByUsername(String username) throws UsernameNotFoundException {
           try {
               // 根据用户名做查询
               SysUser sysUser = userDao.findByName(username);
               if(sysUser == null) {
                   return null;
               }
               List<SimpleGrantedAuthority> authorities = new ArrayList<>();
               // 查询角色信息
               List<SysRole> roles = sysUser.getRoles();
               for (SysRole role : roles) {
                   authorities.add(new SimpleGrantedAuthority(role.getRoleName()));
               }
               // {noop}后面的密码，springSecurity会认为是原文
               UserDetails userDetails = new User(sysUser.getUsername(), "{noop}" + sysUser.getPassword(), authorities);
               return userDetails;
           } catch(Exception e) {
               e.printStackTrace();
           }
           // 认证失败
           return null;
       }
    ```
  5. 重新启动项目，输入用户名：xiaoming；密码：123; 提示登录成功

## 加密认证
### 在IOC容器中提供加密对象
  1. 找到spring-security.xml，修改其中的代码
    ```
      <!-- 把加密对象放到ioc容器中 -->
      <bean id="bCryptPasswordEncoder" class="org.springframework.security.crypto.bcrypt.BCryptPasswordEncoder" />

      <!--设置Spring Security认证用户信息的来源-->
      <security:authentication-manager>
          <!-- userServiceImpl是service.impl.UserServiceImpl.java取首字母小写 -->
          <security:authentication-provider user-service-ref="userServiceImpl">
              <!--指定认证使用的加密对象-->
              <security:password-encoder ref="bCryptPasswordEncoder" />
          </security:authentication-provider>
      </security:authentication-manager>
    ```

### 修改认证方法
  * 找到service.impl.UserServiceImpl.java, 把代码中的`{noop}`去掉
    ```
      /**
        * 认证业务
        * @param username  用户在浏览器输入的用户名
        * @return UserDetails  是springSecurity自己的用户对象
        * @throws UsernameNotFoundException
        */
       @Override
       public UserDetails loadUserByUsername(String username) throws UsernameNotFoundException {
           try {
               // 根据用户名做查询
               SysUser sysUser = userDao.findByName(username);
               if(sysUser == null) {
                   return null;
               }
               List<SimpleGrantedAuthority> authorities = new ArrayList<>();
               // 获取角色信息
               List<SysRole> roles = sysUser.getRoles();
               for (SysRole role : roles) {
                   authorities.add(new SimpleGrantedAuthority(role.getRoleName()));
               }
               // {noop}后面的密码，springSecurity会认为是原文
               UserDetails userDetails = new User(sysUser.getUsername(), sysUser.getPassword(), authorities);
               return userDetails;
           } catch(Exception e) {
               e.printStackTrace();
           }
           // 认证失败
           return null;
       }
    ```

### 修改添加用户的操作
  * 找到service.impl.UserServiceImpl.java, 添加加密操作
    ```
      @Service
      @Transactional
      public class UserServiceImpl implements UserService {
          @Autowired
          private UserDao userDao;
          @Autowired
          private RoleService roleService;
          // 注入BCryptPasswordEncoder
          @Autowired
          private BCryptPasswordEncoder passwordEncoder;
          @Override
          public void save(SysUser user) {
               // 将密码加密
              user.setPassword(passwordEncoder.encode(user.getPassword()));
              userDao.save(user);
          }
          ...
      }
    ```




# 设置用户状态
## 判断认证用户的状态
  * 用户认证业务里，我们封装User对象时，选择了三个构造参数的构造方法，其实还有另一个构造方法：
    ```
      public User(String username, String password, boolean enabled, boolean accountNonExpired, boolean credentialsNonExpired, boolean accountNonLocked, Collection<? extends GrantedAuthority> authorities) {
        ...
    }
    ```
  * 可以看到，这个构造方法里多了四个布尔类型的构造参数，其实我们使用的三个构造参数的构造方法里这四个布尔值默认都被赋值为了true，那么这四个布尔值到底是何意思呢？
    - boolean enabled 是否可用
    - boolean accountNonExpired 账户是否失效
    - boolean credentialsNonExpired 秘密是否失效
    - boolean accountNonLocked 账户是否锁定

### 修改业务代码
  * 修改service.impl.UserServiceImpl.java
    ```
      @Override
      public UserDetails loadUserByUsername(String username) throws UsernameNotFoundException {
          try {
              // 根据用户名做查询
              SysUser sysUser = userDao.findByName(username);
              if(sysUser == null) {
                  return null;
              }
              List<SimpleGrantedAuthority> authorities = new ArrayList<>();
              // 获取角色信息
              List<SysRole> roles = sysUser.getRoles();
              for (SysRole role : roles) {
                  authorities.add(new SimpleGrantedAuthority(role.getRoleName()));
              }
              UserDetails userDetails = new User(
                      sysUser.getUsername(),    // 用户名
                      sysUser.getPassword(),    // 密码
                      sysUser.getStatus() == 1, // 是否可用
                      true,                     // 账户是否失效 
                      true,                     // 秘密是否失效
                      true,                     // 账户是否锁定
                      authorities);
              return userDetails;
          } catch(Exception e) {
              e.printStackTrace();
          }
          // 认证失败
          return null;
      }
    ```
  * 此刻，只有用户状态为1的用户才能成功通过认证！

### 测试
  * 启动项目，在数据库中找状态为1的账号能登录进去（xiaoming, 123），找状态为0的就户提示登录失败

## 注销功能
  1. 修改resources/pages/header.jsp
    * 修改前：
      ```
       <div class="pull-right">
          <a href="${pageContext.request.contextPath}/login.jsp"
            class="btn btn-default btn-flat">注销</a>
        </div>
      ```
    * 修改后：
      ```
        <%@ taglib prefix="security" uri="http://www.springframework.org/security/tags"%>

        <div class="pull-right">
          <form action="${pageContext.request.contextPath}/logout" method="post">
            <security:csrfInput/>
            <input type="submit" value="注销">
          </form>
        </div>
      ```
  2. 启动项目，点击右上角的退出按钮测试

# remember me
## 记住我功能页面代码
  > 注意name和value属性的值不要写错哦！
  1. 找到webapp/login.jsp, 在翻看源码后发现，要想实现remember me的功能，必须有一个单选或者复选框，name为"remember-me"(不能是其他值)，value为yes、true、on、1这4个中的一个
    * ![remember](/images/springSecurity/remember.jpg)

## 开启remember me过滤器
  * 找到resources.spring-security.xml，添加过滤器
    ```
      <!--设置可以用spring的el表达式配置Spring Security并自动生成对应配置组件（过滤器）-->
      <security:http auto-config="true" use-expressions="true">
        <!--开启remember me过滤器，设置token存储时间为60秒-->
        <security:remember-me token-validity-seconds="60"/>
      </security:http>
    ```
  * 测试
    - 启动项目，左下角勾选remember me，输入账号密码登录进去后，1分钟内，关闭浏览器再次访问就不用跳转到登录页面了

## remember me安全性分析
  * 记住我功能方便是大家看得见的，但是安全性却令人担忧。因为Cookie毕竟是保存在客户端的，很容易盗取，而且cookie的值还与用户名、密码这些敏感数据相关，虽然加密了，但是将敏感信息存在客户端，还是不太安全。那么这就要提醒喜欢使用此功能的，用完网站要及时手动退出登录，清空认证信息。
  * 此外，SpringSecurity还提供了remember me的另一种相对更安全的实现机制 :在客户端的cookie中，仅保存一个无意义的加密串（与用户名、密码等敏感数据无关），然后在db中保存该加密串-用户信息的对应关系，自动登录时，用cookie中的加密串，到db中验证，如果通过，自动登录才算通过。

## 持久化remember me信息
  1. 创建一张表，注意这张表的名称和字段都是固定的，不要修改。
    ```
      CREATE TABLE `persistent_logins` (
        `username` varchar(64) NOT NULL,
        `series` varchar(64) NOT NULL,
        `token` varchar(64) NOT NULL,
        `last_used` timestamp NOT NULL,
        PRIMARY KEY (`series`)
      ) ENGINE=InnoDB DEFAULT CHARSET=utf8
    ```
  2. 修改resources/spring-security.xml
    ```
      <!--
          开启remember me过滤器，
          data-source-ref="dataSource" 指定数据库连接池
          token-validity-seconds="60" 设置token存储时间为60秒 可省略
          remember-me-parameter="remember-me" 指定记住的参数名 可省略
      -->
      <security:remember-me data-source-ref="dataSource"
                            token-validity-seconds="60"
                            remember-me-parameter="remember-me"/>
    ```
  3. 测试：输入账号密码，点击remember me，登录后，persistent_logins表就会多出一条数据

# 显示当前认证用户名
  * 找到webapp/pages/header.jsp
    ```
      <span class="hidden-xs">
          <security:authentication property="principal.username" />
      </span>
      或者
      <span class="hidden-xs">
          <security:authentication property="name" />
      </span>
    ```

# 授权准备工作
  > 为了模拟授权操作，咱们临时编写两个业务功能：
  1. contoller.OrderController.java
    ```
      package com.itheima.controller;
      import org.springframework.stereotype.Controller;
      import org.springframework.web.bind.annotation.RequestMapping;
      @Controller
      @RequestMapping("/order")
      public class OrderController {
          @RequestMapping("/findAll")
          public String findAll(){
              return "order-list";
          }
      }
    ```
  2. controller.ProductController.java
    ```
      package com.itheima.controller;
      import org.springframework.stereotype.Controller;
      import org.springframework.web.bind.annotation.RequestMapping;
      @Controller
      @RequestMapping("/product")
      public class ProductController {
          @RequestMapping("/findAll")
          public String findAll(){
              return "product-list";
          }
      }
    ```
  3. webapp/pages/aside.jsp
    ```
      <ul class="treeview-menu">
          <li id="system-setting"><a
                  href="${pageContext.request.contextPath}/product/findAll">
              <i class="fa fa-circle-o"></i> 产品管理
          </a></li>
          <li id="system-setting"><a
                  href="${pageContext.request.contextPath}/order/findAll">
              <i class="fa fa-circle-o"></i> 订单管理
          </a></li>
      </ul>
    ```

# 动态展示菜单
  * 在webapp/pages/aside.jsp对每个菜单通过SpringSecurity标签库指定访问所需角色
    ```
      <%@ taglib prefix="security" uri="http://www.springframework.org/security/tags" %>

      <ul class="treeview-menu">
          <!-- 产品模块还需要产品角色，注意这里还需再次指定超级管理员 -->
          <security:authorize access="hasAnyRole('ROLE_ADMIN','ROLE_PRODUCT')">
              <li id="system-setting"><a
                      href="${pageContext.request.contextPath}/product/findAll">
                  <i class="fa fa-circle-o"></i> 产品管理
              </a></li>
          </security:authorize>
           <!-- 订单模块还需要订单角色，注意这里还需再次指定超级管理员 -->
          <security:authorize access="hasAnyRole('ROLE_ORDER', 'ROLE_ADMIN')">
              <li id="system-setting"><a
                      href="${pageContext.request.contextPath}/order/findAll">
                  <i class="fa fa-circle-o"></i> 订单管理
              </a></li>
          </security:authorize>
      </ul>
    ```
  * 点击页面中的用户管理，给用户添加对应的角色
  * 测试： 启动项目，分别用不同的账户登录，看到的菜单不同 ![结果](/images/springSecurity/结果7.jpg)
{% note danger %}
  那么问题来了，是不是现在已经授权成功了呢？答案是否定的！你可以试试直接去访问订单的http请求地址：
  我们发现xiaoming其实是可以操作订单模块的，只是没有把订单功能展示给xiaoming而已！
  总结一句：页面动态菜单的展示只是为了用户体验，并未真正控制权限！
  ![结果](/images/springSecurity/结果8.jpg)
{% endnote %}

# 授权操作
  > 说明：SpringSecurity可以通过注解的方式来控制类或者方法的访问权限。注解需要对应的注解支持，若注解放在controller类中，对应注解支持应该放在mvc配置文件中，因为controller类是有mvc配置文件扫描并创建的，同理，注解放在service类中，对应注解支持应该放在spring配置文件中。由于我们现在是模拟业务操作，并没有service业务代码，所以就把注解放在controller类中了。

## 开启授权的注解支持
> 这里给大家演示三类注解，但实际开发中，用一类即可！
  * 找到resources/spring-mvc.xml，添加springSecurity的注解支持
    ```
      <!--
          开启权限控制注解支持
          jsr250-annotations="enabled"表示支持jsr250-api的注解，需要jsr250-api的jar包
          pre-post-annotations="enabled"表示支持spring表达式注解
          secured-annotations="enabled"这才是SpringSecurity提供的注解
      -->
      <security:global-method-security jsr250-annotations="enabled"
                                      pre-post-annotations="enabled"
                                      secured-annotations="enabled"/>
    ```
{% note warning %}
  如果把这段配置放到spring-security.xml中，是不起作用的，因为，在controller中的注解是放到spring-mvc.xml中的
{% endnote %}

## 在注解支持对应类或者方法上添加注解
  1. 找到controller.ProductController.java, 添加`@Secured`注解
      ```
        package com.itheima.controller;
        import org.springframework.security.access.annotation.Secured;
        import org.springframework.stereotype.Controller;
        import org.springframework.web.bind.annotation.RequestMapping;
        @Controller
        @RequestMapping("/product")
        public class ProductController {
            // 添加角色，参考webapp/pages/aside.jsp
            @Secured({"ROLE_ADMIN", "ROLE_PRODUCT"})
            @RequestMapping("/findAll")
            public String findAll(){
                return "product-list";
            }
        }
      ```
  2. 找到controller.OrderController.java，添加`@Secured`注解
    ```
      package com.itheima.controller;
      import org.springframework.security.access.annotation.Secured;
      import org.springframework.stereotype.Controller;
      import org.springframework.web.bind.annotation.RequestMapping;
      @Controller
      @RequestMapping("/order")
      public class OrderController {
          @Secured({"ROLE_ORDER", "ROLE_ADMIN"})
          @RequestMapping("/findAll")
          public String findAll(){
              return "order-list";
          }
      }
    ```
  3. 测试：启动项目，使用用户名：xiaoming; 密码123；登录进去，他只有查看产品菜单的权限，访问订单的url，会提示403 ![结果](/images/springSecurity/结果9.jpg)
  
# 权限不足异常处理
> 大家也发现了，每次权限不足都出现403页面，着实难堪！体会一下：![结果](/images/springSecurity/结果9.jpg)
  * 解决办法如下：

## 方式一：在spring-security.xml配置文件中处理
  * 不推荐，因为，每一个都要配置。比如说403，404
  ```
    <!--设置可以用spring的el表达式配置Spring Security并自动生成对应配置组件（过滤器）-->
    <security:http auto-config="true" use-expressions="true">
        <!--省略其它配置-->
        <!--403异常处理-->
        <security:access-denied-handler error-page="/403.jsp"/>
    </security:http>
  ```
  * 测试：启动项目，使用用户名：xiaoming; 密码123；登录进去，他只有查看产品菜单的权限，访问订单的url，跳转到403页面（完毕）

## 方式二：在webapp/WEB-INF/web.xml中处理
  * 不推荐，因为如果不是web工程，就用不了，比如说前后端分离，单比方法一要好
  ```
    <error-page>
        <error-code>403</error-code>
        <location>/403.jsp</location>
    </error-page>
    <error-page>
        <error-code>404</error-code>
        <location>/404.jsp</location>
    </error-page>
  ```

## 方式三：编写异常处理器
  * 在controller下新建advice.HandlerControllerException.java
    ```
      package com.itheima.controller;
      import org.springframework.security.access.AccessDeniedException;
      import org.springframework.web.bind.annotation.ControllerAdvice;
      import org.springframework.web.bind.annotation.ExceptionHandler;
      @ControllerAdvice
      public class handleControllerException2 {
          //只有出现AccessDeniedException异常才调转403.jsp页面
          @ExceptionHandler(AccessDeniedException.class)
          public String handlerException(){
              return "forward:/403.jsp";
          }
          //只有出现AccessDeniedException异常才调转403.jsp页面
          @ExceptionHandler(RuntimeException.class)
          public String runtimeHandlerException(){
              return "forward:/500.jsp";
          }
      }
    ```

# 总结
## springSecurity的快速入门
  1. 在pom.xml中，导入相应的坐标
    ```
      <dependency>
          <groupId>org.springframework.security</groupId>
          <artifactId>spring-security-taglibs</artifactId>
          <version>5.1.5.RELEASE</version>
      </dependency>
      <dependency>
          <groupId>org.springframework.security</groupId>
          <artifactId>spring-security-config</artifactId>
          <version>5.1.5.RELEASE</version>
      </dependency>
    ```
  2. 新建一个security_authority的数据库，并执行以下脚本 ![quick](/images/springSecurity/quick.jpg)
    ```
      DROP TABLE IF EXISTS `sys_permission`;

      CREATE TABLE `sys_permission` (
        `ID` int(11) NOT NULL AUTO_INCREMENT COMMENT '编号',
        `permission_NAME` varchar(30) DEFAULT NULL COMMENT '菜单名称',
        `permission_url` varchar(100) DEFAULT NULL COMMENT '菜单地址',
        `parent_id` int(11) NOT NULL DEFAULT '0' COMMENT '父菜单id',
        PRIMARY KEY (`ID`)
      ) ENGINE=InnoDB DEFAULT CHARSET=utf8;

      DROP TABLE IF EXISTS `sys_role`;

      CREATE TABLE `sys_role` (
        `ID` int(11) NOT NULL AUTO_INCREMENT COMMENT '编号',
        `ROLE_NAME` varchar(30) DEFAULT NULL COMMENT '角色名称',
        `ROLE_DESC` varchar(60) DEFAULT NULL COMMENT '角色描述',
        PRIMARY KEY (`ID`)
      ) ENGINE=InnoDB AUTO_INCREMENT=2 DEFAULT CHARSET=utf8;

      DROP TABLE IF EXISTS `sys_role_permission`;

      CREATE TABLE `sys_role_permission` (
        `RID` int(11) NOT NULL COMMENT '角色编号',
        `PID` int(11) NOT NULL COMMENT '权限编号',
        PRIMARY KEY (`RID`,`PID`),

        KEY `FK_Reference_12` (`PID`),
        CONSTRAINT `FK_Reference_11` FOREIGN KEY (`RID`) REFERENCES `sys_role` (`ID`),
        CONSTRAINT `FK_Reference_12` FOREIGN KEY (`PID`) REFERENCES `sys_permission` (`ID`)
      ) ENGINE=InnoDB DEFAULT CHARSET=utf8;

      DROP TABLE IF EXISTS `sys_user`;

      CREATE TABLE `sys_user` (
        `id` int(11) NOT NULL AUTO_INCREMENT,
        `username` varchar(32) NOT NULL COMMENT '用户名称',
        `password` varchar(120) CHARACTER SET utf8 COLLATE utf8_general_ci NOT NULL COMMENT '密码',
        `status` int(1) DEFAULT '1' COMMENT '1开启0关闭',
        PRIMARY KEY (`id`)
      ) ENGINE=InnoDB AUTO_INCREMENT=2 DEFAULT CHARSET=utf8;

      DROP TABLE IF EXISTS `sys_user_role`;

      CREATE TABLE `sys_user_role` (
        `UID` int(11) NOT NULL COMMENT '用户编号',
        `RID` int(11) NOT NULL COMMENT '角色编号',
        PRIMARY KEY (`UID`,`RID`),
        KEY `FK_Reference_10` (`RID`),
        CONSTRAINT `FK_Reference_10` FOREIGN KEY (`RID`) REFERENCES `sys_role` (`ID`),
        CONSTRAINT `FK_Reference_9` FOREIGN KEY (`UID`) REFERENCES `sys_user` (`id`)
      ) ENGINE=InnoDB DEFAULT CHARSET=utf8;

      CREATE TABLE `persistent_logins` (
        `username` varchar(64) NOT NULL,
        `series` varchar(64) NOT NULL,
        `token` varchar(64) NOT NULL,
        `last_used` timestamp NOT NULL,
        PRIMARY KEY (`series`)
      ) ENGINE=InnoDB DEFAULT CHARSET=utf8

      INSERT INTO `sys_role` VALUES ('6', 'ROLE_USER', '基本角色');
      INSERT INTO `sys_role` VALUES ('7', 'ROLE_PRODUCT', '管理产品');
      INSERT INTO `sys_role` VALUES ('8', 'ROLE_ADMIN', '超级管理员');
      INSERT INTO `sys_role` VALUES ('9', 'ROLE_ORDER', '订单管理');

      INSERT INTO `sys_user` VALUES ('4', 'xiaoming', '$2a$10$T1JPLs1zhydvidYXkQCoU.zEEVB4FbtUuhhEvJWEXtAq8CxRH4PXK', '1');
      INSERT INTO `sys_user` VALUES ('14', 'dujiang', '$2a$10$5pc0nZR5KfE6vMwGEXl.KOoqehyAa4eFKqoGuow3qlr.M.3p9ui3G', '1');

      INSERT INTO `sys_user_role` VALUES ('4', '6');
      INSERT INTO `sys_user_role` VALUES ('14', '6');
      INSERT INTO `sys_user_role` VALUES ('4', '7');
      INSERT INTO `sys_user_role` VALUES ('14', '9');
    ```
  3. 修改webapp/login.jsp
    ```
      <%@ taglib prefix="security" uri="http://www.springframework.org/security/tags"%>

      <form action="${pageContext.request.contextPath}/login" method="post">
        <security:csrfInput/>
    ```
  4. 修改webapp/pages/header.jsp
    ```
      <%@ taglib prefix="security" uri="http://www.springframework.org/security/tags"%>

      <div class="pull-right">
        <form action="${pageContext.request.contextPath}/logout" method="post">
          <security:csrfInput/>
          <input type="submit" value="注销">
        </form>
      </div>
    ```
  5. 修改webapp/pages/user-role-add.jsp
    ```
      <%@taglib uri="http://www.springframework.org/security/tags" prefix="security" %>

      <form action="${pageContext.request.contextPath}/user/addRoleToUser.do" method="post">
        <security:csrfInput/>
        <!-- 正文区域 -->
        <section class="content">
    ```
  6. 修改webapp/pages/role-add.jsp
    ```
      <%@taglib uri="http://www.springframework.org/security/tags" prefix="security" %>

      <form action="${pageContext.request.contextPath}/role/save.do" method="post">
				<security:csrfInput/>
    ```
  7. 修改webapp/pages/user-add.jsp
    ```
      <%@ taglib prefix="security" uri="http://www.springframework.org/security/tags" %>

      <form action="${pageContext.request.contextPath}/user/save.do" method="post">
				<security:csrfInput/>

      <select class="form-control select2" style="width: 100%"
        name="status">
        <option value="0">关闭</option>
        <option value="1">开启</option>
      </select>
    ```
  8. 修改webapp/pages/aside.jsp，添加菜单对应的角色
    ```
      <%@ taglib prefix="security" uri="http://www.springframework.org/security/tags" %>

      <ul class="treeview-menu">
          <!-- 产品模块还需要产品角色，注意这里还需再次指定超级管理员 -->
          <security:authorize access="hasAnyRole('ROLE_ADMIN','ROLE_PRODUCT')">
              <li id="system-setting"><a
                      href="${pageContext.request.contextPath}/product/findAll">
                  <i class="fa fa-circle-o"></i> 产品管理
              </a></li>
          </security:authorize>
          <security:authorize access="hasAnyRole('ROLE_ORDER', 'ROLE_ADMIN')">
              <li id="system-setting"><a
                      href="${pageContext.request.contextPath}/order/findAll">
                  <i class="fa fa-circle-o"></i> 订单管理
              </a></li>
          </security:authorize>
      </ul>
    ```
  9.  修改dao.UserDao.java
    ```
      @Insert("insert into sys_user (username, password, status) values (#{username}, #{password}, #{status})")
      public void save(SysUser user);
    ```
  10. 修改webapp/WEB-INF/web.xml
    ```
      <!--Spring Security过滤器链，注意过滤器名称必须叫springSecurityFilterChain-->
      <filter>
          <filter-name>springSecurityFilterChain</filter-name>
          <filter-class>org.springframework.web.filter.DelegatingFilterProxy</filter-class>
      </filter>
      <filter-mapping>
          <filter-name>springSecurityFilterChain</filter-name>
          <url-pattern>/*</url-pattern>
      </filter-mapping>
    ```
  11. 在resources下新建spring-security.xml
    ```
      <?xml version="1.0" encoding="UTF-8"?>
      <beans xmlns="http://www.springframework.org/schema/beans"
            xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
            xmlns:security="http://www.springframework.org/schema/security"
            xsi:schemaLocation="
            http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd
            http://www.springframework.org/schema/security http://www.springframework.org/schema/security/spring-security.xsd
      ">
          <!--直接释放无需经过SpringSecurity过滤器的静态资源-->
          <security:http pattern="/css/**" security="none"/>
          <security:http pattern="/img/**" security="none"/>
          <security:http pattern="/plugins/**" security="none"/>
          <security:http pattern="/failer.jsp" security="none"/>
          <security:http pattern="/favicon.ico" security="none"/>

          <!-- 配置springSecurity -->
          <!--
              auto-config="true"      表示自动加载springSecurity配置文件
              use-expressions="true"  表示使用spring的el表达式来配置springSecurity
          -->
          <security:http auto-config="true" use-expressions="true">
              <!-- 让认证页面可以匿名访问 -->
              <!--
                  access="permitAll()"    无条件允许访问
              -->
              <security:intercept-url pattern="/login.jsp" access="permitAll()"></security:intercept-url>

              <!-- 拦截资源 -->
              <!--
                  "ROLE_USER"                             是数据库sys_role里面的一条数据的ROLE_NAME
                  pattern="/**"                           表示拦截所有资源(spring中两个*表示所有，第一个*表示路径，第二个*表示子目录以及后面的目录)
                  access="hasAnyAuthority('ROLE_USER')"   表示只有ROLE_USER角色才能访问资源
                  hasAnyAuthority()                       可以多个角色
                  hasRole()                               1个角色
              -->
              <security:intercept-url pattern="/**" access="hasAnyAuthority('ROLE_USER')" />
              <!-- 配置认证信息 -->
              <!--
                  login-page="/login.jsp"                     自己的登录页面
                  login-processing-url="/login"               指定认证的处理器地址(也就是默认security启动的登录地址)
                  default-target-url="/index.jsp"             认证通过默认跳转的url路径，如果有路径这个就没用了
                  authentication-failure-url="/failer.jsp"    认证失败跳转的url路径
              -->
              <security:form-login login-page="/login.jsp"
                                  login-processing-url="/login"
                                  default-target-url="/index.jsp"
                                  authentication-failure-url="/failer.jsp" />
              <!-- 配置退出登录信息 -->
              <!--
                  logout-url="/logout"            指定退出的处理器地址(也就是默认security启动的退出地址)
                  logout-success-url="/login.jsp" 退出后跳转的url路径
              -->
              <security:logout logout-url="/logout" logout-success-url="/login.jsp"></security:logout>
              <!-- 开启remember me过滤器，设置token存储的时间为60秒 -->
              <security:remember-me token-validity-seconds="60"
                                    remember-me-parameter="remember-me"
                                    data-source-ref="dataSource" />
          </security:http>

          <!-- 把加密对象放到ioc容器中 -->
          <bean id="bCryptPasswordEncoder" class="org.springframework.security.crypto.bcrypt.BCryptPasswordEncoder" />

          <!--设置Spring Security认证用户信息的来源-->
          <!-- springSecurity默认的认证必须是加密的，加上{noop} 表示不加密认证 -->
          <security:authentication-manager>
              <!-- userServiceImpl是service.impl.UserServiceImpl.java取首字母小写 -->
              <security:authentication-provider user-service-ref="userServiceImpl">
                  <security:password-encoder ref="bCryptPasswordEncoder" />
              </security:authentication-provider>
          </security:authentication-manager>
      </beans>
    ```
  8. 在resources/applicationContext.xml中引入spring-security.xml
    ```
      <!--引入spring-security文件-->
      <import resource="classpath:spring-security.xml" />
    ```
  9. 修改resources/spring-mvc.xml
    ```
      <?xml version="1.0" encoding="UTF-8"?>
      <beans xmlns="http://www.springframework.org/schema/beans"
              xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
              xmlns:context="http://www.springframework.org/schema/context"
              xmlns:security="http://www.springframework.org/schema/security"
              xmlns:mvc="http://www.springframework.org/schema/mvc"
              xsi:schemaLocation="http://www.springframework.org/schema/beans
                http://www.springframework.org/schema/beans/spring-beans.xsd
                http://www.springframework.org/schema/context
                http://www.springframework.org/schema/context/spring-context.xsd
                http://www.springframework.org/schema/security
                http://www.springframework.org/schema/security/spring-security.xsd
                http://www.springframework.org/schema/mvc
                http://www.springframework.org/schema/mvc/spring-mvc.xsd">

          <context:component-scan base-package="com.itheima.controller"/>

          <mvc:annotation-driven/>

          <bean class="org.springframework.web.servlet.view.InternalResourceViewResolver">
              <property name="prefix" value="/pages/"/>
              <property name="suffix" value=".jsp"/>
          </bean>

          <mvc:default-servlet-handler/>

          <!--
              开启权限控制注解支持
              jsr250-annotations="enabled"表示支持jsr250-api的注解，需要jsr250-api的jar包
              pre-post-annotations="enabled"表示支持spring表达式注解
              secured-annotations="enabled"这才是SpringSecurity提供的注解
          -->
          <security:global-method-security jsr250-annotations="enabled"
                                          pre-post-annotations="enabled"
                                          secured-annotations="enabled"/>

      </beans>
    ```
  10. 修改service.UserService.java，去继承UserDetailsService
    ```
      package com.itheima.service;
      import com.itheima.domain.SysUser;
      import org.springframework.security.core.userdetails.UserDetailsService;
      import java.util.List;
      import java.util.Map;
      public interface UserService extends UserDetailsService {
          ...
      }
    ```
  11. 修改service.impl.UserServiceImpl.java
    ```
      package com.itheima.service.impl;
      import com.itheima.dao.UserDao;
      import com.itheima.domain.SysRole;
      import com.itheima.domain.SysUser;
      import com.itheima.service.RoleService;
      import com.itheima.service.UserService;
      import org.springframework.beans.factory.annotation.Autowired;
      import org.springframework.security.core.authority.SimpleGrantedAuthority;
      import org.springframework.security.core.userdetails.User;
      import org.springframework.security.core.userdetails.UserDetails;
      import org.springframework.security.core.userdetails.UsernameNotFoundException;
      import org.springframework.security.crypto.bcrypt.BCryptPasswordEncoder;
      import org.springframework.stereotype.Service;
      import org.springframework.transaction.annotation.Transactional;
      import java.util.ArrayList;
      import java.util.HashMap;
      import java.util.List;
      import java.util.Map;
      @Service
      @Transactional
      public class UserServiceImpl implements UserService {
          @Autowired
          private UserDao userDao;

          @Autowired
          private RoleService roleService;

          // 注入BCryptPasswordEncoder
          @Autowired
          private BCryptPasswordEncoder passwordEncoder;

          @Override
          public void save(SysUser user) {
              System.out.println(user);
              // 将密码加密
              user.setPassword(passwordEncoder.encode(user.getPassword()));
              userDao.save(user);
          }
          @Override
          public List<SysUser> findAll() {
              return userDao.findAll();
          }
          @Override
          public Map<String, Object> toAddRolePage(Integer id) {
              List<SysRole> allRoles = roleService.findAll();
              List<Integer> myRoles = userDao.findRolesByUid(id);
              Map<String, Object> map = new HashMap<>();
              map.put("allRoles", allRoles);
              map.put("myRoles", myRoles);
              return map;
          }
          @Override
          public void addRoleToUser(Integer userId, Integer[] ids) {
              userDao.removeRoles(userId);
              for (Integer rid : ids) {
                  userDao.addRoles(userId, rid);
              }
          }
          /**
          * 认证业务
          * @param username  用户在浏览器输入的用户名
          * @return UserDetails  是springSecurity自己的用户对象
          * @throws UsernameNotFoundException
          */
          @Override
          public UserDetails loadUserByUsername(String username) throws UsernameNotFoundException {
              try {
                  // 根据用户名做查询
                  SysUser sysUser = userDao.findByName(username);
                  if(sysUser == null) {
                      return null;
                  }
                  List<SimpleGrantedAuthority> authorities = new ArrayList<>();
                  // 获取角色信息
                  List<SysRole> roles = sysUser.getRoles();
                  for (SysRole role : roles) {
                      authorities.add(new SimpleGrantedAuthority(role.getRoleName()));
                  }
                  // {noop}后面的密码，springSecurity会认为是原文
                  UserDetails userDetails = new User(
                          sysUser.getUsername(),
                          sysUser.getPassword(),
                          sysUser.getStatus() == 1,
                          true,
                          true,
                          true,
                          authorities);
                  return userDetails;
              } catch(Exception e) {
                  e.printStackTrace();
              }
              // 认证失败
              return null;
          }
      }
    ```
  12. 找到controller.ProductController.java, 添加`@Secured`注解
      ```
        package com.itheima.controller;
        import org.springframework.security.access.annotation.Secured;
        import org.springframework.stereotype.Controller;
        import org.springframework.web.bind.annotation.RequestMapping;
        @Controller
        @RequestMapping("/product")
        public class ProductController {
            // 添加角色，参考webapp/pages/aside.jsp
            @Secured({"ROLE_ADMIN", "ROLE_PRODUCT"})
            @RequestMapping("/findAll")
            public String findAll(){
                return "product-list";
            }
        }
      ```
  13. 找到controller.OrderController.java，添加`@Secured`注解
    ```
      package com.itheima.controller;
      import org.springframework.security.access.annotation.Secured;
      import org.springframework.stereotype.Controller;
      import org.springframework.web.bind.annotation.RequestMapping;
      @Controller
      @RequestMapping("/order")
      public class OrderController {
          @Secured({"ROLE_ORDER", "ROLE_ADMIN"})
          @RequestMapping("/findAll")
          public String findAll(){
              return "order-list";
          }
      }
    ```
  14. 在controller下新建advice.HandlerControllerException.java
    ```
      package com.itheima.controller;
      import org.springframework.security.access.AccessDeniedException;
      import org.springframework.web.bind.annotation.ControllerAdvice;
      import org.springframework.web.bind.annotation.ExceptionHandler;
      @ControllerAdvice
      public class handleControllerException2 {
          //只有出现AccessDeniedException异常才调转403.jsp页面
          @ExceptionHandler(AccessDeniedException.class)
          public String handlerException(){
              return "forward:/403.jsp";
          }
          //只有出现AccessDeniedException异常才调转403.jsp页面
          @ExceptionHandler(RuntimeException.class)
          public String runtimeHandlerException(){
              return "forward:/500.jsp";
          }
      }
    ```
  15. 启动tomcat, 输入用户名：xiaoming; 密码：123，提示登录成功
  




