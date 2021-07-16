---
title: springmvc2
date: 2021-06-30 17:11:36
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

# SpringMVC的数据相应
## SpringMVC的数据响应方式
  1. 页面跳转
    * 直接返回字符串
    * 通过ModelAndView对象返回
  2. 回写数据 
    * 直接返回字符串
    * 返回对象或集合    

## 页面跳转
  1. 返回字符串形式
    * ![mvc跳转](/images/spring/mvc跳转.jpg)
  2. 返回ModelAndView对象
    * 写法一：在controller下，新建UserController2.java，通过addObject设置model数据，setViewName设置视图名称
      ```
        package com.itheima.controller;
        import org.springframework.stereotype.Controller;
        import org.springframework.web.bind.annotation.RequestMapping;
        import org.springframework.web.bind.annotation.RequestMethod;
        import org.springframework.web.servlet.ModelAndView;

        @Controller
        //相当于地址栏加了一层/user
        @RequestMapping("/user")
        public class UserController2 {
            @RequestMapping(value = "/test2", method = RequestMethod.GET, params = {"username"})
            public ModelAndView testController() {
                System.out.println("controller running...");
                /**
                * Model: 模型 作用封装数据
                * View: 视图 作用展示数据
                */
                ModelAndView modelAndView = new ModelAndView();
                // 设置模型数据
                modelAndView.addObject("username", "zhangsan");
                // 设置视图名称
                modelAndView.setViewName("test");
                return modelAndView;
            }
        }
      ```

    * 写法二：在controller下，新建UserController3.java，直接在方法里使用ModelAndView对象
      ```
        package com.itheima.controller;
        import org.springframework.stereotype.Controller;
        import org.springframework.web.bind.annotation.RequestMapping;
        import org.springframework.web.bind.annotation.RequestMethod;
        import org.springframework.web.servlet.ModelAndView;

        @Controller
        @RequestMapping("/user")
        public class UserController3 {
            @RequestMapping(value = "/test3", method = RequestMethod.GET, params = {"username"})
            public ModelAndView testController(ModelAndView modelAndView) {
                modelAndView.addObject("username", "zhangsan");
                modelAndView.setViewName("test");
                return modelAndView;
            }
        }
      ```

    * 写法三：在controller下，新建UserController4.java，直接在方法里使用Model对象，通过addAttribute设置模型，返回一个字符串的视图名称
      ```
        package com.itheima.controller;
        import org.springframework.stereotype.Controller;
        import org.springframework.ui.Model;
        import org.springframework.web.bind.annotation.RequestMapping;
        import org.springframework.web.bind.annotation.RequestMethod;

        @Controller
        @RequestMapping("/user")
        public class UserController4 {
            @RequestMapping(value = "/test4", method = RequestMethod.GET, params = {"username"})
            public String testController(Model model) {
                model.addAttribute("username", "mazi");
                return "test";
            }
        }
      ```
    * 在webapp/WEB-INF/jsp/test.jsp里, 可以使用controller里model设置的username
      ```
        <%@ page contentType="text/html;charset=UTF-8" language="java" %>
        <html>
          <head>
              <title>Title</title>
          </head>
          <body>
              <h2>我是测试页面 ${username}</h2>
          </body>
        </html>
      ```
    * 测试：输入http://localhost:8080/user/test2?username=zhangsan, 展示页面
      - ![测试](/images/spring/mvc6.jpg)
  3. 向request域存储数据
    * 在controller下，新建UserController5.java，直接在方法里使用HttpServletRequest对象，通过setAttribute设置模型，返回一个字符串的视图名称
      ```
        package com.itheima.controller;
        import org.springframework.stereotype.Controller;
        import org.springframework.web.bind.annotation.RequestMapping;
        import org.springframework.web.bind.annotation.RequestMethod;
        import javax.servlet.http.HttpServletRequest;
        @Controller
        @RequestMapping("/user")
        public class UserController5 {
            @RequestMapping(value = "/test5", method = RequestMethod.GET, params = {"username"})
            public String testController(HttpServletRequest request) {
                request.setAttribute("username", "123");
                return "test";
            }
        }
      ```

## 回写数据
### 直接返回字符串
  1. 通过SpringMVC框架注入的response对象，使用response.getWriter().print(“hello world”) 回写数据，此时不需要视图跳转，业务方法返回值为void。
    * 在controller下，新建UserController6.java，直接在方法里使用HttpServletResponse对象，通过response.getWriter().print打印一个字符串
      ```
        package com.itheima.controller;
        import org.springframework.stereotype.Controller;
        import org.springframework.web.bind.annotation.RequestMapping;
        import org.springframework.web.bind.annotation.RequestMethod;
        import javax.servlet.http.HttpServletResponse;
        import java.io.IOException;
        @Controller
        @RequestMapping("/user")
        public class UserController6 {
            @RequestMapping(value = "/test6", method = RequestMethod.GET, params = {"username"})
            public void testController(HttpServletResponse response) throws IOException {
                response.getWriter().print("hello,world");
            }
        }
      ```

  2. 将需要回写的字符串直接返回，但此时需要通过@ResponseBody注解告知SpringMVC框架，方法返回的字符串不是跳转是直接在http响应体中返回。
    * 在controller下，新建UserController7.java，使用@ResponseBody注解，返回一个字符串
      ```
        package com.itheima.controller;
        import org.springframework.stereotype.Controller;
        import org.springframework.web.bind.annotation.RequestMapping;
        import org.springframework.web.bind.annotation.RequestMethod;
        import org.springframework.web.bind.annotation.ResponseBody;
        @Controller
        @RequestMapping("/user")
        @ResponseBody
        public class UserController7 {
            @RequestMapping(value = "/test7", method = RequestMethod.GET, params = {"username"})
            public String testController() {
              return "yes, mvc";
            }
        }
      ```
  3. 返回json格式的字符串
    1. 在pom.xml中导入json转换工具jackson
      ```
        <dependency>
          <groupId>com.fasterxml.jackson.core</groupId>
          <artifactId>jackson-core</artifactId>
          <version>2.12.3</version>
        </dependency>
      ```

    2. 在java下创建com.itheima.domain.User的实体类
      ```
        package com.itheima.domain;
        public class User {
          private String name;
          private String age;
          public String getName() {
              return name;
          }
          public void setName(String name) {
              this.name = name;
          }
          public String getAge() {
              return age;
          }
          public void setAge(String age) {
              this.age = age;
          }
          @Override
          public String toString() {
              return "User{" +
                      "name='" + name + '\'' +
                      ", age='" + age + '\'' +
                      '}';
          }
        } 
      ```
    3. 在controller下，新建UserController8.java，使用jackson
      ```
        package com.itheima.controller;
        import com.fasterxml.jackson.databind.ObjectMapper;
        import com.itheima.domain.User;
        import org.springframework.stereotype.Controller;
        import org.springframework.web.bind.annotation.RequestMapping;
        import org.springframework.web.bind.annotation.RequestMethod;
        import org.springframework.web.bind.annotation.ResponseBody;
        import java.io.IOException;
        @Controller
        @RequestMapping("/user")
        @ResponseBody
        public class UserController8 {
            @RequestMapping(value = "/test8", method = RequestMethod.GET, params = {"username"})
            public String testController() throws IOException {
                User user = new User();
                user.setName("zhangsan");
                user.setAge("18");
                System.out.println(user.toString());
                // 使用json转换工具将对象转换成json格式字符串再返回
                ObjectMapper mapper = new ObjectMapper();
                String s = mapper.writeValueAsString(user);
                System.out.println(s);
                return s;
            }
        }
      ```


### 返回对象或集合
  1. 通过SpringMVC帮助我们对对象或集合进行json字符串的转换并回写，为处理器适配器配置消息转换参数，指定使用jackson进行对象或集合的转换，因此需要在spring-mvc.xml中进行如下配置：
    * 引入mvc命名空间
    * 配置
      - ![mvc自动转换为json](/images/spring/mvc7.jpg)
      - ![mvc自动转换为json](/images/spring/mvc8.jpg)
    * 在controller下，新建UserController9.java，返回User对象
      ```
        package com.itheima.controller;
        import com.itheima.domain.User;
        import org.springframework.stereotype.Controller;
        import org.springframework.web.bind.annotation.RequestMapping;
        import org.springframework.web.bind.annotation.RequestMethod;
        import org.springframework.web.bind.annotation.ResponseBody;
        import java.io.IOException;
        @Controller
        @RequestMapping("/user")
        @ResponseBody
        // 期望SpringMVC自动将User转换为json格式的字符串
        public class UserController9 {
            @RequestMapping(value = "/test9", method = RequestMethod.GET, params = {"username"})
            public User testController() throws IOException {
                User user = new User();
                user.setName("zhangsan");
                user.setAge("18");
                return user;
            }
        }
      ```

  2. 使用`<mvc:annotation-driven />`就可以替代上面图片配置处理器映射器的操作
    * 在spring-mvc.xml中：
      ```
        <!--mvc的注解驱动-->
        <mvc:annotation-driven />
      ```
    * 在controller下，新建UserController9.java，返回User对象
      ```
        package com.itheima.controller;
        import com.itheima.domain.User;
        import org.springframework.stereotype.Controller;
        import org.springframework.web.bind.annotation.RequestMapping;
        import org.springframework.web.bind.annotation.RequestMethod;
        import org.springframework.web.bind.annotation.ResponseBody;
        import java.io.IOException;
        @Controller
        @RequestMapping("/user")
        @ResponseBody
        // 期望SpringMVC自动将User转换为json格式的字符串
        public class UserController9 {
            @RequestMapping(value = "/test9", method = RequestMethod.GET, params = {"username"})
            public User testController() throws IOException {
                User user = new User();
                user.setName("zhangsan");
                user.setAge("18");
                return user;
            }
        }
      ```

# SpringMVC获得请求数据
## 获得请求参数
  * 客户端请求参数的格式是：name=value&name=value…服务器端要获得请求的参数，有时还需要进行数据的封装，SpringMVC可以接收如下类型的参数：
    1. 基本类型参数
    2. POJO类型参数
    3. 数组类型参数
    4. 集合类型参数

## 获得基本类型参数
  * Controller中的业务方法的参数名称要与请求参数的name一致，参数值会自动映射匹配。
    ```
      http://localhost:8080/user/test10?name=zhangsan&age=12
    ```
  * 在controller下，新建UserController10.java，testController方法的参数和地址栏的参数要一致
    ```
      package com.itheima.controller;
      import org.springframework.stereotype.Controller;
      import org.springframework.web.bind.annotation.RequestMapping;
      import org.springframework.web.bind.annotation.ResponseBody;
      import java.io.IOException;
      @Controller
      @RequestMapping("/user")
      @ResponseBody
      // 期望SpringMVC自动将User转换为json格式的字符串
      public class UserController10 {
          @RequestMapping(value = "/test10")
          public void testController(String name, int age) throws IOException {
              System.out.println(name);
              System.out.println(age);
          }
      }
    ```

## 获得POJO类型参数
  * Controller中的业务方法的POJO参数的属性名与请求参数的name一致，参数值会自动映射匹配。
    ```
      http://localhost:8080/user/test11?name=zhangsan&age=12
    ```
  * 在java下，新建User的实体类,有对应的get和set方法
    ```
      package com.itheima.domain;
      public class User {
        private String name;
        private String age;
        public String getName() {
            return name;
        }
        public void setName(String name) {
            this.name = name;
        }
        public String getAge() {
            return age;
        }
        public void setAge(String age) {
            this.age = age;
        }
        @Override
        public String toString() {
            return "User{" +
                    "name='" + name + '\'' +
                    ", age='" + age + '\'' +
                    '}';
        }
      } 
    ```

  * 在controller下，新建UserController11.java，testController方法的参数和你创建的实体类名要一致
    ```
      package com.itheima.controller;
      import com.itheima.domain.User;
      import org.springframework.stereotype.Controller;
      import org.springframework.web.bind.annotation.RequestMapping;
      import org.springframework.web.bind.annotation.ResponseBody;
      import java.io.IOException;
      @Controller
      @RequestMapping("/user")
      @ResponseBody
      // 期望SpringMVC自动将User转换为json格式的字符串
      public class UserController11 {
          @RequestMapping(value = "/test11")
          public void testController(User user) throws IOException {
              user.setName("wangmazi");
              user.setAge("20");
              System.out.println(user);
          }
      }
    ```

## 获得数组类型参数
  * Controller中的业务方法数组名称与请求参数的name一致，参数值会自动映射匹配。
    ```
      http://localhost:8080/itheima_springmvc1/quick11?strs=111&strs=222&strs=333
    ```
  * 在controller下，新建UserController12.java
    ```
      package com.itheima.controller;
      import org.springframework.stereotype.Controller;
      import org.springframework.web.bind.annotation.RequestMapping;
      import org.springframework.web.bind.annotation.ResponseBody;
      import java.io.IOException;
      import java.util.Arrays;

      @Controller
      @RequestMapping("/user")
      @ResponseBody
      // 期望SpringMVC自动将User转换为json格式的字符串
      public class UserController12 {
          @RequestMapping(value = "/test12")
          public void testController(String[] strs) throws IOException {
              System.out.println(Arrays.asList(strs));
          }
      }
    ```

## 获得集合类型参数
### 创建pojo类
  * 获得集合参数时，要将集合参数包装到一个POJO中才可以。在domain下新建VO的实体类
    ```
      package com.itheima.domain;
      import java.util.List;
      public class VO {
          private List<User> userList;
          public List<User> getUserList() {
            return userList;
          }
          public void setUserList(List<User> userList) {
            this.userList = userList;
          }
      }
    ```
  * 在controller下，新建UserController13.java
    ```
      package com.itheima.controller;
      import com.itheima.domain.VO;
      import org.springframework.stereotype.Controller;
      import org.springframework.web.bind.annotation.RequestMapping;
      import org.springframework.web.bind.annotation.ResponseBody;
      import java.io.IOException;
      @Controller
      @ResponseBody
      public class UserController13 {
          @RequestMapping(value = "/test13")
          public void testController(VO vo) throws IOException {
              System.out.println(vo.getUserList());
          }
      }
    ```
  * webapp下新建form.jsp
    ```   
      <%@ page contentType="text/html;charset=UTF-8" language="java" %>
      <html>
      <head>
          <title>Title</title>
      </head>
      <body>
          <form action="${pageContext.request.contextPath}/test13" method="post">
              <%--表明是第一个User对象的username--%>
              <input type="text" name="userList[0].name" /> <br />
              <input type="text" name="userList[0].age" /> <br />
              <input type="text" name="userList[1].name" /> <br />
              <input type="text" name="userList[1].age" /> <br />
              <input type="submit" value="提交" />
          </form>
      </body>
      </html>
    ```
  * 访问localhost:8080/form.jsp，输入内容，vo.getUserList()就能打印出值

### ajax提交
  * 当使用ajax提交时，可以指定contentType为json形式，那么在方法参数位置使用@RequestBody可以直接接收集合数据而无需使用POJO进行包装
  * 在webapp下，新建js/jquery.main.js
  * 在webapp下，新建ajax.jsp
    ```
      <%@ page contentType="text/html;charset=UTF-8" language="java" %>
      <html>
      <head>
          <title>Title</title>
          <script src="${pageContext.request.contextPath}/js/jquery.min.js"></script>
          <script>
              // 模拟数据
              const arrayList = new Array();
              arrayList.push({name: "zhangsan",age: 20})
              arrayList.push({name: "lisi",age: 18})
              $.ajax({
                  type: "POST",
                  url: "${pageContext.request.contextPath}/test14",
                  data: JSON.stringify(arrayList),
                  contentType : 'application/json;charset=utf-8'
              })
          </script>
      </head>
      <body>
      </body>
      </html>
    ```
  * 在controller下，新建UserController14.java
    ```
      package com.itheima.controller;
      import com.itheima.domain.User;
      import org.springframework.stereotype.Controller;
      import org.springframework.web.bind.annotation.RequestBody;
      import org.springframework.web.bind.annotation.RequestMapping;
      import org.springframework.web.bind.annotation.ResponseBody;
      import java.io.IOException;
      import java.util.List;
      @Controller
      @ResponseBody
      public class UserController14 {
          @RequestMapping(value = "/test14")
          public void testController(@RequestBody List<User> userList) throws IOException {
              System.out.println(userList);
          }
      }
    ```
  * 在resources/spring-mvc.xml配置文件中指定放行的资源
    ```
      <mvc:resources mapping="/js/**" location="/js/"/> 
    ```
  * 在resources/spring-mvc.xml配置文件中也可以使用`<mvc:default-servlet-handler/>`代替`<mvc:resources mapping="/js/**" location="/js/"/>` 
  * 测试：输入http://localhost:8080/ajax.jsp，看控制台

{% note warning %}
注意:
  通过谷歌开发者工具抓包发现，没有加载到jquery文件，原因是SpringMVC的前端控制器DispatcherServleturl-pattern配置的是/,代表对所有的资源都进行过滤操作，我们可以通过以下两种方式指定放行静态资源：
  * 在spring-mvc.xml配置文件中指定放行的资源
     `<mvc:resources mapping="/js/**" location="/js/"/> `
  * 使用`<mvc:default-servlet-handler/>`标签
{% endnote %}

## 请求数据乱码问题
  * 当post请求时，数据会出现乱码，我们可以设置一个过滤器来进行编码的过滤。
  * 在web.xml中配置过滤器，解决乱码问题
    ```
      <!--配置全局过滤的filter-->
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
    ```

## 参数绑定注解@requestParam
* 当请求的参数名称与Controller的业务方法参数名称不一致时，就需要通过@RequestParam注解显示的绑定。
  1. 在webapp下新建form2.jsp
    ```
      <%@ page contentType="text/html;charset=UTF-8" language="java" %>
      <html>
      <head>
          <title>Title</title>
      </head>
      <body>
          <form action="${pageContext.request.contextPath}/test15" method="post">
              <%--表明是第一个User对象的username--%>
              <input type="text" name="username" /> <br />
              <input type="text" name="age" /> <br />
              <input type="submit" value="提交" />
          </form>
      </body>
      </html>
    ```
  2. 在controller下，新建UserController15.java, 使用@RequestParam把表单中的username变为业务参数中的name
    ```
      package com.itheima.controller;
      import org.springframework.stereotype.Controller;
      import org.springframework.web.bind.annotation.RequestMapping;
      import org.springframework.web.bind.annotation.RequestParam;
      import org.springframework.web.bind.annotation.ResponseBody;
      import java.io.IOException;
      @Controller
      @ResponseBody
      public class UserController15 {
          @RequestMapping(value = "/test15")
          public void testController(@RequestParam("username") String name) throws IOException {
              System.out.println(name);
          }
      }
    ```

* 注解@RequestParam还有如下参数可以使用
  1. <span style="color: red">value：</span>与请求参数名称
  2. <span style="color: red">required：</span>此在指定的请求参数是否必须包括，默认是true，提交时如果没有此参数则报错
  3. <span style="color: red">defaultValue：</span>当没有指定请求参数时，则使用指定的默认值赋值
    ```
      package com.itheima.controller;
      import org.springframework.stereotype.Controller;
      import org.springframework.web.bind.annotation.RequestMapping;
      import org.springframework.web.bind.annotation.RequestParam;
      import org.springframework.web.bind.annotation.ResponseBody;
      import java.io.IOException;
      @Controller
      @ResponseBody
      public class UserController15 {
          @RequestMapping(value = "/test15")
          public void testController(@RequestParam(value = "username",required = true,defaultValue = "王麻子") String name) throws IOException {
              System.out.println(name);
          }
      }
    ```

## 获得Restful风格的参数
  * ![mvc9](/images/spring/mvc9.jpg)
  * 上述url地址/user/1中的1就是要获得的请求参数，在SpringMVC中可以使用占位符进行参数绑定。地址/user/1可以写成/user/{id}，占位符{id}对应的就是1的值。在业务方法中我们可以使用@PathVariable注解进行占位符的匹配获取工作。
  * 在浏览器中输入http://localhost:8080/test16/zhangsan
  * 在controller下，新建UserController16.java
    ```
      package com.itheima.controller;
      import org.springframework.stereotype.Controller;
      import org.springframework.web.bind.annotation.PathVariable;
      import org.springframework.web.bind.annotation.RequestMapping;
      import org.springframework.web.bind.annotation.ResponseBody;
      import java.io.IOException;
      @Controller
      @ResponseBody
      public class UserController16 {
          @RequestMapping(value = "/test16/{username}")
          public void testController(@PathVariable(value = "username") String name) throws IOException {
              System.out.println(name);
          }
      }
    ```

## 自定义类型转换器
  1. ![mvc10](/images/spring/mvc10.jpg)
  2. 定义转换器类实现Converter接口,在com.itheima下新建converter.DateConverter的实现类，去实现实现org.springframework.core.convert的Converter接口
    ```
      package com.itheima.converter;
      import org.springframework.core.convert.converter.Converter;
      import java.text.ParseException;
      import java.text.SimpleDateFormat;
      import java.util.Date;
      // 实现org.springframework.core.convert的Converter接口, 泛型第一个String表示你要转的类型，第二个Date转后的类型
      public class DateConverter implements Converter<String, Date> {
          public Date convert(String dateStr) {
              // 将日期字符串转换为真正的日期对象返回
              SimpleDateFormat format = new SimpleDateFormat("yyyy-MM-dd");
              Date date = null;
              try {
                  date = format.parse(dateStr);
              } catch (ParseException e) {
                  e.printStackTrace();
              }
              return date;
          }
      }
    ```
  3. 在配置文件spring-mvc.xml中声明转换器
    ```
      <!--声明转换器-->
      <!--输入ConversionServiceFactoryBean有提示-->
      <bean id="ConversionService2" class="org.springframework.context.support.ConversionServiceFactoryBean">
          <property name="converters">
              <list>
                  <bean class="com.itheima.converter.DateConverter"></bean>
              </list>
          </property>
      </bean>
    ```
  4. 在配置文件spring-mvc.xml中的`<annotation-driven>`中引用转换器
    ```
      <mvc:annotation-driven conversion-service="ConversionService2" />
    ```
  5. 在controller下，新建UserController17.java
    ```
      package com.itheima.controller;
      import org.springframework.stereotype.Controller;
      import org.springframework.web.bind.annotation.RequestMapping;
      import org.springframework.web.bind.annotation.ResponseBody;
      import java.io.IOException;
      import java.util.Date;
      @Controller
      @ResponseBody
      public class UserController17 {
          @RequestMapping(value = "/test17")
          public void testController(Date date) throws IOException {
              System.out.println(date);
          }
      }
    ```
  6. 输入localhost:8080/test17，冷打印出值

## 获得Servlet相关API
  * SpringMVC支持使用原始ServletAPI对象作为控制器方法的参数进行注入，常用的对象如下：
    - HttpServletRequest
    - HttpServletResponse
    - HttpSession
  * 在controller下，新建UserController18.java
    ```
      package com.itheima.controller;
      import org.springframework.stereotype.Controller;
      import org.springframework.web.bind.annotation.RequestMapping;
      import org.springframework.web.bind.annotation.ResponseBody;
      import javax.servlet.http.HttpServletRequest;
      import javax.servlet.http.HttpServletResponse;
      import javax.servlet.http.HttpSession;
      import java.io.IOException;
      @Controller
      @ResponseBody
      public class UserController18 {
          @RequestMapping(value = "/test18")
          public void testController(HttpServletRequest request, HttpServletResponse response, HttpSession session) throws IOException {
              System.out.println(request);
              System.out.println(response);
              System.out.println(session);
          }
      }
    ```

## 获得请求头
### @RequestHeader
  * 使用@RequestHeader可以获得请求头信息，相当于web阶段学习的request.getHeader(name)
  * @RequestHeader注解的属性如下：
    - <span style="color: red">value：</span>请求头的名称
    - <span style="color: red">required：</span>是否必须携带此请求头
  * 在controller下，新建UserController19.java,获取请求头中的User-Agent
    ```
      package com.itheima.controller;
      import org.springframework.stereotype.Controller;
      import org.springframework.web.bind.annotation.RequestHeader;
      import org.springframework.web.bind.annotation.RequestMapping;
      import org.springframework.web.bind.annotation.ResponseBody;
      import java.io.IOException;
      @Controller
      @ResponseBody
      public class UserController19 {
          @RequestMapping(value = "/test19")
          public void testController(@RequestHeader(value = "User-Agent", required = false) String headerValue) throws IOException {
              System.out.println(headerValue);
          }
      }
    ```

### @CookieValue
  * 使用@CookieValue可以获得指定Cookie的值
  * @CookieValue注解的属性如下：
    - <span style="color: red">value：</span>指定cookie的名称
    - <span style="color: red">required：</span>是否必须携带此cookie
  * 在controller下，新建UserController20.java,获取Cookie
    ```
      package com.itheima.controller;
      import org.springframework.stereotype.Controller;
      import org.springframework.web.bind.annotation.CookieValue;
      import org.springframework.web.bind.annotation.RequestMapping;
      import org.springframework.web.bind.annotation.ResponseBody;
      import java.io.IOException;

      @Controller
      @ResponseBody
      public class UserController20 {
          @RequestMapping(value = "/test20")
          public void testController(@CookieValue(value = "JSESSIONID", required = false) String cookieValue) throws IOException {
              System.out.println(cookieValue);
          }
      }
    ```

## 文件上传
### 文件上传客户端三要素
  1. 表单项type=“file”
  2. 表单的提交方式是post
  3. 表单的enctype属性是多部分表单形式，及enctype=“multipart/form-data”
    * 在webapp下新建upload.jsp
      ```
        <%@ page contentType="text/html;charset=UTF-8" language="java" %>
        <html>
        <head>
            <title>Title</title>
        </head>
        <body>
            <form action="${pageContext.request.contextPath}/test21" method="post" enctype="multipart/form-data">
                名称<input type="text" name="username"><br/>
                文件<input type="file" name="upload"><br/>
                <input type="submit" value="提交">
            </form>
        </body>
        </html>
      ```

### 文件上传原理
![原理](/images/spring/mvc11.jpg)

## 单文件上传步骤
  1. 导入fileupload和io坐标
  2. 配置文件上传解析器
  3. 编写文件上传代码

### 导入fileupload和io坐标
  * 在pom.xml中导入commons-fileupload和commons-io包
    ```
      <dependency>
        <groupId>commons-fileupload</groupId>
        <artifactId>commons-fileupload</artifactId>
        <version>1.3.3</version>
      </dependency>
      <dependency>
        <groupId>commons-io</groupId>
        <artifactId>commons-io</artifactId>
        <version>2.6</version>
      </dependency>
    ```

### 配置文件上传解析器
  * 在spring-mvc.xml中配置文件上传解析器
    ```
      <!--配置文件上传解析器-->
      <!--输入CommonsMultipartResolver有代码提示-->
      <bean id="multipartResolver" class="org.springframework.web.multipart.commons.CommonsMultipartResolver">
          <!--上传文件总大小-->
          <property name="maxUploadSize" value="5242800"/>
          <!--上传单个文件的大小-->
          <property name="maxUploadSizePerFile" value="5242800"/>
          <!--上传文件的编码类型-->
          <property name="defaultEncoding" value="UTF-8"/>
      </bean>
    ```

### 编写文件上传代码
  * 在controller下，新建UserController21.java
    ```
      package com.itheima.controller;
      import org.springframework.stereotype.Controller;
      import org.springframework.web.bind.annotation.RequestMapping;
      import org.springframework.web.bind.annotation.ResponseBody;
      import org.springframework.web.multipart.MultipartFile;
      import java.io.File;
      import java.io.IOException;
      @Controller
      @ResponseBody
      public class UserController21 {
          @RequestMapping(value = "/test21")
          // 参数username和upload这两个必须和upload.jsp中表单的name一致
          public void testController(String username, MultipartFile upload) throws IOException {
              System.out.println(username);
              System.out.println(upload);
              // 获取文件名称
              String filename = upload.getOriginalFilename();
              // 保存文件
              upload.transferTo(new File("D:\\upload\\"+filename));
          }
      }
    ```
  * 通过访问http://localhost:8080/upload.jsp,文件上传到D盘的upload文件夹里面

## 多文件上传数据
  * 在webapp下新建upload2.jsp
    ```
      <%@ page contentType="text/html;charset=UTF-8" language="java" %>
      <html>
      <head>
          <title>Title</title>
      </head>
      <body>
          <form action="${pageContext.request.contextPath}/test22" method="post" enctype="multipart/form-data">
              名称<input type="text" name="username"><br/>
              文件<input type="file" name="upload"><br/>
              文件<input type="file" name="upload"><br/>
              文件<input type="file" name="upload"><br/>
              <input type="submit" value="提交">
          </form>
      </body>
      </html>
    ```
  * 在controller下，新建UserController22.java
    ```
      package com.itheima.controller;
      import org.springframework.stereotype.Controller;
      import org.springframework.web.bind.annotation.RequestMapping;
      import org.springframework.web.bind.annotation.ResponseBody;
      import org.springframework.web.multipart.MultipartFile;
      import java.io.File;
      import java.io.IOException;
      @Controller
      @ResponseBody
      public class UserController22 {
          @RequestMapping(value = "/test22")
          // 参数username和upload这两个必须和upload.jsp中表单的name一致, 多个文件MultipartFile为数组
          public void testController(String username, MultipartFile[] upload) throws IOException {
              for(int i=0; i< upload.length; i++) {
                  System.out.println(username);
                  System.out.println(upload[i]);
                  // 获取文件名称
                  String filename = upload[i].getOriginalFilename();
                  // 保存文件
                  upload[i].transferTo(new File("D:\\upload\\"+filename));
              }
          }
      }
    ```
  * 通过访问http://localhost:8080/upload2.jsp,文件上传到D盘的upload文件夹里面
  * 多文件上传和单文件上传的唯一区别就是参数MultipartFile，多文件为MultipartFile[]

# SpringMVC拦截器
## 拦截器（interceptor）的作用
  * Spring MVC 的<span style="color: red">拦截器</span>类似于 Servlet  开发中的过滤器 Filter，用于对处理器进行<span style="color: red">预处理</span>和<span style="color: red">后处理</span>。
  * 将拦截器按一定的顺序联结成一条链，这条链称为<span style="color: red">拦截器链（Interceptor Chain）</span>。在访问被拦截的方法或字段时，拦截器链中的拦截器就会按其之前定义的顺序被调用。拦截器也是AOP思想的具体实现。

## 拦截器和过滤器区别
![拦截器](/images/spring/mvc拦截器.jpg)

## 拦截器是快速入门
自定义拦截器很简单，只有如下三步：
1. 创建拦截器类实现HandlerInterceptor接口
2. 配置拦截器
3. 测试拦截器的拦截效果
### 代码准备
  1. 创建一个itheima_spring_interceptor的module，并配置webapp文件夹
  2. 在pom.xml中导入对应的坐标
    ```
      <dependency>
          <groupId>org.springframework</groupId>
          <artifactId>spring-context</artifactId>
          <version>5.3.6</version>
      </dependency>
      <dependency>
          <groupId>org.springframework</groupId>
          <artifactId>spring-web</artifactId>
          <version>5.3.6</version>
      </dependency>
      <dependency>
          <groupId>org.springframework</groupId>
          <artifactId>spring-webmvc</artifactId>
          <version>5.3.6</version>
      </dependency>
      <dependency>
          <groupId>javax.servlet</groupId>
          <artifactId>javax.servlet-api</artifactId>
          <version>4.0.1</version>
      </dependency>
      <dependency>
          <groupId>javax.servlet.jsp</groupId>
          <artifactId>javax.servlet.jsp-api</artifactId>
          <version>2.3.3</version>
      </dependency>
    ```
  3. 在webapp下新建index.jsp，随便写点内容
    ```
      <%@ page contentType="text/html;charset=UTF-8" language="java" %>
      <html>
      <head>
          <title>Title</title>
      </head>
      <body>
          <h1>hello, world</h1>
      </body>
      </html>
    ```
  4. 在resources下新建spring-mvc.xml的核心配置文件，并配置mvc注解驱动、配置视图解析器、静态资源权限开放、组件扫描
    ```
      <?xml version="1.0" encoding="UTF-8"?>
      <beans xmlns="http://www.springframework.org/schema/beans"
            xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
            xmlns:mvc="http://www.springframework.org/schema/mvc"
            xmlns:context="http://www.springframework.org/schema/context"
            xsi:schemaLocation="
            http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd
            http://www.springframework.org/schema/mvc http://www.springframework.org/schema/mvc/spring-mvc.xsd
            http://www.springframework.org/schema/context http://www.springframework.org/schema/context/spring-context.xsd
      ">
          <!--1、mvc注解驱动-->
          <mvc:annotation-driven />

          <!--2、配置视图解析器-->
          <!--输入InternalResourceViewResolver有代码提示-->
          <bean class="org.springframework.web.servlet.view.InternalResourceViewResolver">
              <property name="prefix" value="/"/>
              <property name="suffix" value=".jsp"/>
          </bean>

          <!--3、静态资源权限开放-->
          <mvc:default-servlet-handler/>

          <!--4、组件扫描  扫描Controller-->
          <context:component-scan base-package="com.itheima.controller"/>
      </beans>
    ```
  5. 在webapp/WEB-INF/web.xml中，配置SpringMVC的前端控制器
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
  6. 在java下新建com.itheima.controller.TargetController
    ```
      package com.itheima.controller;
      import org.springframework.stereotype.Controller;
      import org.springframework.web.bind.annotation.RequestMapping;
      import org.springframework.web.servlet.ModelAndView;
      @Controller
      public class TargetController {
          @RequestMapping("/target")
          public ModelAndView show() {
              System.out.println("执行...");
              ModelAndView modelAndView = new ModelAndView();
              modelAndView.addObject("name", "zhangsan");
              modelAndView.setViewName("index");
              return modelAndView;
          }
      }
    ```
  7. 配置Artifacts便于tomcat发布
  8. 点击idea右上角的Edit Configurations添加Artifacts,启动
  9. 访问localhost:8080/target

### 创建拦截器类实现HandlerInterceptor接口
  1. 上面的代码基础上，在java下创建com.itheima.interceptor.MyInterceptor.java,去实现HandlerInterceptor
    ```
      package com.itheima.interceptor;
      import org.springframework.web.servlet.HandlerInterceptor;
      import org.springframework.web.servlet.ModelAndView;
      import javax.servlet.http.HttpServletRequest;
      import javax.servlet.http.HttpServletResponse;
      public class MyInterceptor implements HandlerInterceptor {
          // 在目标方法(这里是contorller里的show方法)执行之前 执行
          public boolean preHandle(HttpServletRequest request, HttpServletResponse response, Object handler) throws Exception {
              System.out.println("preHandle...");
              return true;
          }
          // 在目标方法执行之后 视图对象返回之前执行
          public void postHandle(HttpServletRequest request, HttpServletResponse response, Object handler, ModelAndView modelAndView) throws Exception {
              System.out.println("postHandle...");
          }

          // 在流程都执行完毕后 执行
          public void afterCompletion(HttpServletRequest request, HttpServletResponse response, Object handler, Exception ex) throws Exception {
              System.out.println("afterCompletion...");
          }
      }
    ```

### 配置拦截器
  1. 在spring-mvc.xml中配置拦截器
    ```
      <!--配置拦截器-->
      <mvc:interceptors>
        <mvc:interceptor>
          <!--对哪些资源进行拦截操作-->
          <mvc:mapping path="/**"/>
          <bean class="com.itheima.interceptor.MyInterceptor"></bean>
        </mvc:interceptor>
      </mvc:interceptors>
    ```

### 测试拦截器的拦截效果
  * 输入localhost:8080/target看控制台

## 拦截器方法说明
![mvc拦截器](/images/spring/mvc拦截器2.jpg)
* 需求：如果request没有username操作，这跳转到报错页面
  1. 在com.itheima.interceptor.MyInterceptor.java中，修改preHandle方法
    ```
      package com.itheima.interceptor;
      import org.springframework.web.servlet.HandlerInterceptor;
      import org.springframework.web.servlet.ModelAndView;
      import javax.servlet.http.HttpServletRequest;
      import javax.servlet.http.HttpServletResponse;
      public class MyInterceptor implements HandlerInterceptor {
          // 在目标方法(这里是contorller里的show方法)执行之前 执行
          public boolean preHandle(HttpServletRequest request, HttpServletResponse response, Object handler) throws Exception {
              System.out.println("preHandle...");
              // 获取客户端的参数
              String param = request.getParameter("user");
              System.out.println("param:" + param);
              if(param.equals("zhangsan")) {
                  return true;
              } else {
                  request.getRequestDispatcher("/error.jsp").forward(request, response);
                  return false;
              }
          }
          // 在目标方法执行之后 视图对象返回之前执行
          public void postHandle(HttpServletRequest request, HttpServletResponse response, Object handler, ModelAndView modelAndView) throws Exception {
              System.out.println("postHandle...");
          }

          // 在流程都执行完毕后 执行
          public void afterCompletion(HttpServletRequest request, HttpServletResponse response, Object handler, Exception ex) throws Exception {
              System.out.println("afterCompletion...");
          }
      }
    ```
  2. 输入localhost:8080/target和输入localhost:8080/target?user=zhangsan看下控制台

# SpringMVC异常处理机制
## 异常处理的思路
  * 系统中异常包括两类：<span style="color: red">预期异常</span>和<span style="color: red">运行时异常RuntimeException</span>，前者通过捕获异常从而获取异常信息，后者主要通过规范代码开发、测试等手段减少运行时异常的发生。
  * 系统的<span style="color: red">Dao、Service、Controller</span>出现都通过throws Exception向上抛出，最后由SpringMVC前端控制器交由异常处理器进行异常处理，如下图：
  * ![mvc异常处理](/images/spring/mvc异常处理.jpg)

## 异常处理两种方式
  1. 使用Spring MVC提供的简单异常处理器SimpleMappingExceptionResolver
  2. 实现Spring的异常处理接口HandlerExceptionResolver 自定义自己的异常处理器

### 代码准备
  1. 新建itheima_spring_exception的module，并设置webapp文件夹
  2. 在pom.xml中导入响应的坐标
    ```
      <dependency>
          <groupId>org.springframework</groupId>
          <artifactId>spring-context</artifactId>
          <version>5.3.6</version>
      </dependency>
      <dependency>
          <groupId>org.springframework</groupId>
          <artifactId>spring-web</artifactId>
          <version>5.3.6</version>
      </dependency>
      <dependency>
          <groupId>org.springframework</groupId>
          <artifactId>spring-webmvc</artifactId>
          <version>5.3.6</version>
      </dependency>
      <dependency>
          <groupId>javax.servlet</groupId>
          <artifactId>javax.servlet-api</artifactId>
          <version>4.0.1</version>
      </dependency>
      <dependency>
          <groupId>javax.servlet.jsp</groupId>
          <artifactId>javax.servlet.jsp-api</artifactId>
          <version>2.3.3</version>
      </dependency>
    ```
  3. 在webapp下新建index.jsp，随便写点内容
    ```
      <%@ page contentType="text/html;charset=UTF-8" language="java" %>
      <html>
      <head>
          <title>Title</title>
      </head>
      <body>
          <h1>hello, world</h1>
      </body>
      </html>
    ```
  4. 在resources下新建spring-mvc.xml的核心配置文件，并配置mvc注解驱动、配置视图解析器、静态资源权限开放、组件扫描
    ```
      <?xml version="1.0" encoding="UTF-8"?>
      <beans xmlns="http://www.springframework.org/schema/beans"
            xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
            xmlns:mvc="http://www.springframework.org/schema/mvc"
            xmlns:context="http://www.springframework.org/schema/context"
            xsi:schemaLocation="
            http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd
            http://www.springframework.org/schema/mvc http://www.springframework.org/schema/mvc/spring-mvc.xsd
            http://www.springframework.org/schema/context http://www.springframework.org/schema/context/spring-context.xsd
      ">
          <!--1、mvc注解驱动-->
          <mvc:annotation-driven />

          <!--2、配置视图解析器-->
          <!--输入InternalResourceViewResolver有代码提示-->
          <bean class="org.springframework.web.servlet.view.InternalResourceViewResolver">
              <property name="prefix" value="/"/>
              <property name="suffix" value=".jsp"/>
          </bean>

          <!--3、静态资源权限开放-->
          <mvc:default-servlet-handler/>

          <!--4、组件扫描  扫描Controller-->
          <context:component-scan base-package="com.itheima.controller"/>
      </beans>
    ```
  5. 在webapp/WEB-INF/web.xml中，配置SpringMVC的前端控制器
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
  6. 在java下新建com.itheima.service.DemoService的接口
    ```
      package com.itheima.service;
      import com.itheima.exception.MyException;
      import java.io.FileNotFoundException;
      public interface DemoService {
          void show1();
          void show2();
          void show3() throws FileNotFoundException;
          void show4();
          void show5() throws MyException, MyException;
      }
    ```
  7. 在service下新建impl.DemoServiceImpl的实现类
    ```
      package com.itheima.service.impl;
      import com.itheima.exception.MyException;
      import com.itheima.service.DemoService;
      import java.io.FileInputStream;
      import java.io.FileNotFoundException;
      import java.io.InputStream;
      public class DemoServiceImpl implements DemoService {
          public void show1() {
              System.out.println("抛出类型转换异常....");
              Object str = "zhangsan";
              Integer num = (Integer)str;
          }
          public void show2() {
              System.out.println("抛出除零异常....");
              int i = 1/0;
          }
          public void show3() throws FileNotFoundException {
              System.out.println("文件找不到异常....");
              InputStream in = new FileInputStream("C:/xxx/xxx/xxx.txt");
          }
          public void show4() {
              System.out.println("空指针异常.....");
              String str = null;
              str.length();
          }
          public void show5() throws MyException {
              System.out.println("自定义异常....");
              throw new MyException();
          }
      }
    ```
  8. 在java下新建com.itheima.exception.MyException.java
    ```
      package com.itheima.exception;
      public class MyException extends Exception {

      }
    ```
  9. 新建applicationContext.xml,配置service
    ```
      <?xml version="1.0" encoding="UTF-8"?>
      <beans xmlns="http://www.springframework.org/schema/beans"
            xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
            xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd">
          <bean class="com.itheima.service.impl.DemoServiceImpl"></bean>
      </beans>
    ```
  10. 在web.xml中引入applicationContext.xml
    ```
      <context-param>
        <param-name>contextConfigLocation</param-name>
        <param-value>classpath:applicationContext.xml</param-value>
      </context-param>
      <listener>
        <listener-class>org.springframework.web.context.ContextLoaderListener</listener-class>
      </listener>
    ```
  11. 在java下新建com.itheima.controller.DemoController
    ```
      package com.itheima.controller;
      import com.itheima.exception.MyException;
      import com.itheima.service.DemoService;
      import org.springframework.beans.factory.annotation.Autowired;
      import org.springframework.stereotype.Controller;
      import org.springframework.web.bind.annotation.RequestMapping;
      import java.io.FileNotFoundException;
      @Controller
      public class DemoController {
          @Autowired
          private DemoService demoService;
          @RequestMapping("/show")
          public String show() throws MyException, FileNotFoundException {
              demoService.show1();
  //            demoService.show2();
  //            demoService.show3();
  //            demoService.show4();
  //            demoService.show5();
              return "index";
          }
      }
    ```
  12. 生成Artifacts并启动tomcat, 输入http://localhost:8080/show

## 简单异常处理器SimpleMappingExceptionResolver
SpringMVC已经定义好了该类型转换器，在使用时可以根据项目情况进行相应异常与视图的映射配置
  1. 在spring-mvc.xml中，配置异常处理器
    - ![mvc异常处理器](/images/spring/mvc异常处理器.jpg)
  2. 在webapp下新建error.jsp
    ```
      <%@ page contentType="text/html;charset=UTF-8" language="java" %>
      <html>
      <head>
          <title>Title</title>
      </head>
      <body>
          <h1>通用的异常错误</h1>
      </body>
      </html>
    ```
  3. 在浏览器输入localhost:8080/show，他会跳转到error.jsp

## 自定义异常处理步骤
1. 创建异常处理器类实现HandlerExceptionResolver
2. 配置异常处理器
3. 编写异常页面
4. 测试异常跳转

### 创建异常处理器类实现HandlerExceptionResolver
  1. 在java下新建com.itheima.resolver.MyExceptionResolver.java，并继承HandlerExceptionResolver
    ```
      package com.itheima.resolver;
      import com.itheima.exception.MyException;
      import org.springframework.web.servlet.HandlerExceptionResolver;
      import org.springframework.web.servlet.ModelAndView;
      import javax.servlet.http.HttpServletRequest;
      import javax.servlet.http.HttpServletResponse;
      public class MyExceptionResolver implements HandlerExceptionResolver {
          /*
              参数Exception: 异常对象
              返回值ModelAndView: 跳转到错误视图信息
          */
          public ModelAndView resolveException(HttpServletRequest httpServletRequest, HttpServletResponse httpServletResponse, Object o, Exception e) {
              ModelAndView modelAndView = new ModelAndView();
              // 如果异常是我自定义的异常
              if(e instanceof MyException) {
                  modelAndView.addObject("info", "自定义异常");
              } else if(e instanceof ClassCastException) {
                  modelAndView.addObject("info", "内转换异常");
              }
              modelAndView.setViewName("error3");
              return modelAndView;
          }
      }
    ```

### 配置异常处理器
  1. 在spring-mvc.xml中，配置自定义异常处理器
    ```
      <!--配置自定义异常处理器-->
      <bean class="com.itheima.resolver.MyExceptionResolver"></bean>
    ```

### 编写异常页面
  1. 在webapp下新建error3.jsp
    ```
      <%@ page contentType="text/html;charset=UTF-8" language="java" %>
      <html>
      <head>
          <title>Title</title>
      </head>
      <body>
          <h1>异常信息类型：${info}</h1>
      </body>
      </html>
    ```

### 测试异常跳转
  * 输入localhost:8080/show

## 知识要点
  * <span style="color: red">异常处理方式</span>
    - 配置简单异常处理器SimpleMappingExceptionResolver
    - 自定义异常处理器
  * <span style="color: red">自定义异常处理步骤</span>
    - 创建异常处理器类实现HandlerExceptionResolver
    - 配置异常处理器
    - 编写异常页面
    - 测试异常跳转

# Spring练习
## Spring练习环境搭建
### Spring环境搭建步骤
1. 创建工程（Project&Module）
  * 新建itheima_spring_test的module，并设置webapp文件夹
2. 导入静态页面（见资料jsp页面）
  * 将资料中的jsp页面放到webapp文件夹下
3. 导入需要坐标（见资料中的pom.xml）
  ```
    <dependency>
        <groupId>mysql</groupId>
        <artifactId>mysql-connector-java</artifactId>
        <version>5.1.46</version>
    </dependency>
    <dependency>
        <groupId>c3p0</groupId>
        <artifactId>c3p0</artifactId>
        <version>0.9.1.2</version>
    </dependency>
    <dependency>
        <groupId>com.alibaba</groupId>
        <artifactId>druid</artifactId>
        <version>1.1.10</version>
    </dependency>
    <dependency>
        <groupId>junit</groupId>
        <artifactId>junit</artifactId>
        <version>4.12</version>
        <scope>test</scope>
    </dependency>
    <dependency>
        <groupId>org.springframework</groupId>
        <artifactId>spring-context</artifactId>
        <version>5.3.6</version>
    </dependency>
    <dependency>
        <groupId>org.springframework</groupId>
        <artifactId>spring-test</artifactId>
        <version>5.3.6</version>
    </dependency>
    <dependency>
        <groupId>org.springframework</groupId>
        <artifactId>spring-web</artifactId>
        <version>5.3.6</version>
    </dependency>
    <dependency>
        <groupId>org.springframework</groupId>
        <artifactId>spring-webmvc</artifactId>
        <version>5.3.6</version>
    </dependency>
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
    <dependency>
        <groupId>com.fasterxml.jackson.core</groupId>
        <artifactId>jackson-core</artifactId>
        <version>2.9.0</version>
    </dependency>
    <dependency>
        <groupId>com.fasterxml.jackson.core</groupId>
        <artifactId>jackson-databind</artifactId>
        <version>2.9.0</version>
    </dependency>
    <dependency>
        <groupId>com.fasterxml.jackson.core</groupId>
        <artifactId>jackson-annotations</artifactId>
        <version>2.9.0</version>
    </dependency>
    <dependency>
        <groupId>commons-fileupload</groupId>
        <artifactId>commons-fileupload</artifactId>
        <version>1.3.3</version>
    </dependency>
    <dependency>
        <groupId>commons-io</groupId>
        <artifactId>commons-io</artifactId>
        <version>2.6</version>
    </dependency>
    <dependency>
        <groupId>commons-logging</groupId>
        <artifactId>commons-logging</artifactId>
        <version>1.2</version>
    </dependency>
    <dependency>
        <groupId>org.slf4j</groupId>
        <artifactId>slf4j-log4j12</artifactId>
        <version>1.7.7</version>
    </dependency>
    <dependency>
        <groupId>log4j</groupId>
        <artifactId>log4j</artifactId>
        <version>1.2.17</version>
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
        <groupId>jstl</groupId>
        <artifactId>jstl</artifactId>
        <version>1.2</version>
    </dependency>
  ```

4. 创建包结构（controller、service、dao、domain、utils）
  在java下依次创建com.itheima.controller、com.itheima.service、com.itheima.dao、com.itheima.domain、com.itheima.utils文件夹

5. 导入数据库脚本（见资料test.sql）
  ```
    /*
    SQLyog Ultimate v12.09 (64 bit)
    MySQL - 5.7.24-log : Database - test
    *********************************************************************
    */


    /*!40101 SET NAMES utf8 */;

    /*!40101 SET SQL_MODE=''*/;

    /*!40014 SET @OLD_UNIQUE_CHECKS=@@UNIQUE_CHECKS, UNIQUE_CHECKS=0 */;
    /*!40014 SET @OLD_FOREIGN_KEY_CHECKS=@@FOREIGN_KEY_CHECKS, FOREIGN_KEY_CHECKS=0 */;
    /*!40101 SET @OLD_SQL_MODE=@@SQL_MODE, SQL_MODE='NO_AUTO_VALUE_ON_ZERO' */;
    /*!40111 SET @OLD_SQL_NOTES=@@SQL_NOTES, SQL_NOTES=0 */;
    CREATE DATABASE /*!32312 IF NOT EXISTS*/`test` /*!40100 DEFAULT CHARACTER SET utf8 */;

    USE `test`;

    /*Table structure for table `sys_role` */

    DROP TABLE IF EXISTS `sys_role`;

    CREATE TABLE `sys_role` (
      `id` bigint(20) NOT NULL AUTO_INCREMENT,
      `roleName` varchar(50) DEFAULT NULL,
      `roleDesc` varchar(50) DEFAULT NULL,
      PRIMARY KEY (`id`)
    ) ENGINE=InnoDB AUTO_INCREMENT=5 DEFAULT CHARSET=utf8;

    /*Data for the table `sys_role` */

    insert  into `sys_role`(`id`,`roleName`,`roleDesc`) values (1,'院长','负责全面工作'),(2,'研究员','课程研发工作'),(3,'讲师','授课工作'),(4,'助教','协助解决学生的问题');

    /*Table structure for table `sys_user` */

    DROP TABLE IF EXISTS `sys_user`;

    CREATE TABLE `sys_user` (
      `id` bigint(20) NOT NULL AUTO_INCREMENT,
      `username` varchar(50) DEFAULT NULL,
      `email` varchar(50) DEFAULT NULL,
      `password` varchar(80) DEFAULT NULL,
      `phoneNum` varchar(20) DEFAULT NULL,
      PRIMARY KEY (`id`)
    ) ENGINE=InnoDB AUTO_INCREMENT=4 DEFAULT CHARSET=utf8;

    /*Data for the table `sys_user` */

    insert  into `sys_user`(`id`,`username`,`email`,`password`,`phoneNum`) values (1,'zhangsan','zhangsan@itcast.cn','123','13888888888'),(2,'lisi','lisi@itcast.cn','123','13999999999'),(3,'wangwu','wangwu@itcast.cn','123','18599999999');

    /*Table structure for table `sys_user_role` */

    DROP TABLE IF EXISTS `sys_user_role`;

    CREATE TABLE `sys_user_role` (
      `userId` bigint(20) NOT NULL,
      `roleId` bigint(20) NOT NULL,
      PRIMARY KEY (`userId`,`roleId`),
      KEY `roleId` (`roleId`),
      CONSTRAINT `sys_user_role_ibfk_1` FOREIGN KEY (`userId`) REFERENCES `sys_user` (`id`),
      CONSTRAINT `sys_user_role_ibfk_2` FOREIGN KEY (`roleId`) REFERENCES `sys_role` (`id`)
    ) ENGINE=InnoDB DEFAULT CHARSET=utf8;

    /*Data for the table `sys_user_role` */

    insert  into `sys_user_role`(`userId`,`roleId`) values (1,1),(1,2),(2,2),(2,3);

    /*!40101 SET SQL_MODE=@OLD_SQL_MODE */;
    /*!40014 SET FOREIGN_KEY_CHECKS=@OLD_FOREIGN_KEY_CHECKS */;
    /*!40014 SET UNIQUE_CHECKS=@OLD_UNIQUE_CHECKS */;
    /*!40111 SET SQL_NOTES=@OLD_SQL_NOTES */;
  ```

6. 创建POJO类（见资料User.java和Role.java）
  * 在domain下新建User实体类
    ```
      package com.itheima.domain;
      public class User {
          private Long id;
          private String username;
          private String email;
          private String password;
          private String phoneNum;
          public Long getId() {
              return id;
          }
          public void setId(Long id) {
              this.id = id;
          }
          public String getUsername() {
              return username;
          }
          public void setUsername(String username) {
              this.username = username;
          }
          public String getEmail() {
              return email;
          }
          public void setEmail(String email) {
              this.email = email;
          }
          public String getPassword() {
              return password;
          }
          public void setPassword(String password) {
              this.password = password;
          }
          public String getPhoneNum() {
              return phoneNum;
          }
          public void setPhoneNum(String phoneNum) {
              this.phoneNum = phoneNum;
          }
          @Override
          public String toString() {
              return "User{" +
                      "id=" + id +
                      ", username='" + username + '\'' +
                      ", email='" + email + '\'' +
                      ", password='" + password + '\'' +
                      ", phoneNum='" + phoneNum + '\'' +
                      '}';
          }
      }
    ```
  * 在domain下新建Role实体类
    ```
      package com.itheima.domain;
      public class Role {
          private Long id;
          private String roleName;
          private String roleDesc;
          public Long getId() {
              return id;
          }
          public void setId(Long id) {
              this.id = id;
          }
          public String getRoleName() {
              return roleName;
          }
          public void setRoleName(String roleName) {
              this.roleName = roleName;
          }
          public String getRoleDesc() {
              return roleDesc;
          }
          public void setRoleDesc(String roleDesc) {
              this.roleDesc = roleDesc;
          }
          @Override
          public String toString() {
              return "Role{" +
                      "id=" + id +
                      ", roleName='" + roleName + '\'' +
                      ", roleDesc='" + roleDesc + '\'' +
                      '}';
          }
      }
    ```

7. 创建配置文件（applicationContext.xml、spring-mvc.xml、jdbc.properties、log4j.properties）
  * 在resources下创建log4j.properties
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
      log4j.rootLogger=info, stdout
    ```

  * 在resources下创建jdbc.properties
    ```
      jdbc.driver=com.mysql.jdbc.Driver
      jdbc.url=jdbc:mysql://localhost:3306/test
      jdbc.username=root
      jdbc.password=root
    ```
  
  * 在resources下创建springContext.xml
    ```
      <?xml version="1.0" encoding="UTF-8"?>
      <beans xmlns="http://www.springframework.org/schema/beans"
            xmlns:context="http://www.springframework.org/schema/context"
            xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
            xsi:schemaLocation="
            http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd
            http://www.springframework.org/schema/context http://www.springframework.org/schema/context/spring-context.xsd
      ">
          <!--1、加载jdbc.properties-->
          <context:property-placeholder location="classpath:jdbc.properties"/>

          <!--2、配置数据源对象-->
          <bean id="dataSource" class="com.mchange.v2.c3p0.ComboPooledDataSource">
              <property name="driverClass" value="${jdbc.driver}"/>
              <property name="jdbcUrl" value="${jdbc.url}"/>
              <property name="user" value="${jdbc.username}"/>
              <property name="password" value="${jdbc.password}"/>
          </bean>

          <!--3、配置JdbcTemplate对象-->
          <bean id="jdbcTemplate" class="org.springframework.jdbc.core.JdbcTemplate">
              <property name="dataSource" ref="dataSource"></property>
          </bean>
      </beans>
    ```

  * 在resources下创建spring-mvc.xml
    ```
      <?xml version="1.0" encoding="UTF-8"?>
      <beans xmlns="http://www.springframework.org/schema/beans"
            xmlns:mvc="http://www.springframework.org/schema/mvc"
            xmlns:context="http://www.springframework.org/schema/context"
            xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
            xsi:schemaLocation="
            http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd
            http://www.springframework.org/schema/mvc http://www.springframework.org/schema/mvc/spring-mvc.xsd
            http://www.springframework.org/schema/context http://www.springframework.org/schema/context/spring-context.xsd
      ">
          <!--1、mvc注解驱动-->
          <mvc:annotation-driven/>

          <!--2、配置视图解析器-->
          <bean class="org.springframework.web.servlet.view.InternalResourceViewResolver">
              <property name="prefix" value="/pages/"/>
              <property name="suffix" value=".jsp"/>
          </bean>

          <!--3、静态资源权限开放-->
          <mvc:default-servlet-handler/>

          <!--4、组件扫描  扫描Controller-->
          <context:component-scan base-package="com.itheima.controller"/>
      </beans>
    ```

8. 配置web.xml
  ```
    <?xml version="1.0" encoding="UTF-8"?>
    <web-app xmlns="http://xmlns.jcp.org/xml/ns/javaee"
            xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
            xsi:schemaLocation="http://xmlns.jcp.org/xml/ns/javaee http://xmlns.jcp.org/xml/ns/javaee/web-app_4_0.xsd"
            version="4.0">
        <!--全局的初始化参数-->
        <context-param>
            <param-name>contextConfigLocation</param-name>
            <param-value>classpath:applicationContext.xml</param-value>
        </context-param>
        <!--Spring的监听器-->
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
            <load-on-startup>2</load-on-startup>
        </servlet>
        <servlet-mapping>
            <servlet-name>DispatcherServlet</servlet-name>
            <url-pattern>/</url-pattern>
        </servlet-mapping>
    </web-app>
  ```

### 用户和角色的关系
![关系](/images/spring/关系.jpg)

## 角色列表的展示和添加操作
### 角色列表的展示效果
![练习1](/images/spring/练习1.jpg)

### 角色列表的展示步骤分析
1. 点击角色管理菜单发送请求到服务器端（修改角色管理菜单的url地址）
  * 找到webapp/pages/aside.jsp，修改以下代码：
    ```
      <li>
        <a href="${pageContext.request.contextPath}/role/showList"> 
          <i class="fa fa-circle-o"></i> 角色管理
        </a>
      </li>
    ```
2. 创建RoleController和showList()方法
  * 在controller下创建RoleController.java
    ```
      package com.itheima.controller;
      import org.springframework.web.servlet.ModelAndView;
      public class RoleController {
          public ModelAndView showList() {
              ModelAndView modelAndView = new ModelAndView();
              // 设置视图，跳转到role-list.jsp
              modelAndView.setViewName("role-list");
              return modelAndView;
          }
      }
    ```
3. 创建RoleService和getRoleList()方法
  * 在service下新建RoleService的接口
    ```
      package com.itheima.service;
      import com.itheima.domain.Role;
      import java.util.List;
      public interface RoleService {
          public List<Role> getRoleList();
      }
    ```
  * 在service下新建impl.RoleServiceImpl的实现类
    ```
      package com.itheima.service.impl;
      import com.itheima.dao.RoleDao;
      import com.itheima.domain.Role;
      import com.itheima.service.RoleService;
      import org.springframework.beans.factory.annotation.Autowired;
      import org.springframework.stereotype.Service;
      import java.util.List;

      @Service("roleService")
      public class RoleServiceImpl implements RoleService {
          @Autowired
          private RoleDao roleDao;
          public List<Role> getRoleList() {
              List<Role> roleList =  roleDao.fildAllList();
              return roleList;
          }
      }
    ```

4. 创建RoleDao和fildAllList()方法
  * 在dao下创建RoleDao的接口
    ```
      package com.itheima.dao;
      import com.itheima.domain.Role;
      import java.util.List;
      public interface RoleDao {
          List<Role> fildAllList();
      }
    ```
  * 在dao下创建impl.RoleDaoImpl的实现类
    ```
      package com.itheima.dao.impl;
      import com.itheima.dao.RoleDao;
      import com.itheima.domain.Role;
      import org.springframework.stereotype.Repository;
      import java.util.List;
      @Repository("roleDao")
      public class RoleDaoImpl implements RoleDao {
          public List<Role> fildAllList() {
              return null;
          }
      }
    ```
5. 使用JdbcTemplate完成查询操作
  * 在RoleDaoImpl.java中
    ```
      package com.itheima.dao.impl;
      import com.itheima.dao.RoleDao;
      import com.itheima.domain.Role;
      import org.springframework.beans.factory.annotation.Autowired;
      import org.springframework.jdbc.core.BeanPropertyRowMapper;
      import org.springframework.jdbc.core.JdbcTemplate;
      import org.springframework.stereotype.Repository;
      import java.util.List;
      @Repository("roleDao")
      public class RoleDaoImpl implements RoleDao {
          @Autowired
          private JdbcTemplate template;
          public List<Role> fildAllList() {
              List<Role> roleList = template.query("select * from sys_role", new BeanPropertyRowMapper<Role>(Role.class));
              System.out.println("roleList:" + roleList.toString());
              return roleList;
          }
      }
    ```
6. 将查询数据存储到Model中
  * 在RoleController.java中，将Service获取的数据放到Model中
  ```
    package com.itheima.controller;
    import com.itheima.domain.Role;
    import com.itheima.service.RoleService;
    import org.springframework.beans.factory.annotation.Autowired;
    import org.springframework.stereotype.Controller;
    import org.springframework.web.bind.annotation.RequestMapping;
    import org.springframework.web.bind.annotation.ResponseBody;
    import org.springframework.web.servlet.ModelAndView;
    import java.util.List;
    @Controller
    @RequestMapping("/role")
    public class RoleController {
        @Autowired
        private RoleService roleService;
        @RequestMapping("/showList")
        @ResponseBody
        public ModelAndView showList() {
            ModelAndView modelAndView = new ModelAndView();
            List<Role> roleList = roleService.getRoleList();
            // 设置模型
            modelAndView.addObject("roleList", roleList);
            // 设置视图，跳转到role-list.jsp
            modelAndView.setViewName("role-list");
            return modelAndView;
        }
    }
  ```
7. 转发到role-list.jsp页面进行展示
  * 找到webapp/pages/role-list.jsp，引入jstl模板,在最顶部
    ```
      <%@ taglib prefix="c" uri="http://java.sun.com/jsp/jstl/core" %>
    ```
  * 使用jstl的foreach标签
    ```
      <c:forEach items="${roleList}" var="role">
        <tr>
          <td><input name="ids" type="checkbox"></td>
          <td>${role.id}</td>
          <td>${role.roleName}</td>
          <td>${role.roleDesc}</td>
          <td class="text-center">
            <a href="#" class="btn bg-olive btn-xs">删除</a>
          </td>
        </tr>
      </c:forEach>
    ```

### 角色添加的效果
  ![test](/images/spring/springtest.jpg)

### 角色添加的步骤分析
1. 点击列表页面新建按钮跳转到角色添加页面
  * 找到webapp/pages/role-add.jsp，找到form表单, 说明在RoleController,有个save方法
    ```
      <form action="${pageContext.request.contextPath}/role/save" method="post">
    ```
2. 输入角色信息，点击保存按钮，表单数据提交服务器
3. 编写RoleController的save()方法
  * 在controller/RoleController.java中添加save方法
    ```
      @RequestMapping("/save")
      @ResponseBody
      public String save(Role role) {
        roleService.saveItem(role);
        return null;
      }
    ```
4. 编写RoleService的saveItem()方法
  * 在RoleService.java中，添加saveItem方法
    ```
      void saveItem(Role role);
    ```
  * 在impl.RoleServiceImpl中实现saveItem方法
    ```
      public void saveItem(Role role) {
        roleDao.add(role);
      }
    ```
5. 编写RoleDao的add()方法
  * 在dao/RoleDao.java中添加add方法
    ```
      void add(Role role);
    ```
  * 在impl/RoleDaoImpl.java中实现add方法
    ```
      public void add(Role role) {
        
      }
    ```
6. 使用JdbcTemplate保存Role数据到sys_role
  ```
    public void add(Role role) {
      template.update("insert into sys_role values(?,?,?)", null, role.getRoleName(), role.getRoleDesc());
    }
  ```
7. 跳转回角色列表页面
  * 在RoleController.java中
    ```
      @RequestMapping("/save")
      public String save(Role role) {
          roleService.saveItem(role);
          return "redirect:/role/showList";
      }
    ```
8. 测试
  * 点击新建按钮，输入表单后，发现数据库和页面刚新增的都是乱码![乱码](/images/spring/springtest2.jpg) 
  * 解决办法：在web.xml中配置过滤器
    ```
      <!--解决乱码的过滤器-->
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
    ```

## 用户列表的展示和添加操作
### 用户列表的展示和添加操作
![展示](/images/spring/springtest3.jpg)

### 用户列表的展示步骤分析
1. 点击用户管理菜单发送请求到服务器端（修改用户管理菜单的url地址）
  * 找到webapp/pages/aside.jsp，修改以下代码
    ```
      <li>
        <a href="${pageContext.request.contextPath}/user/showList"> 
          <i class="fa fa-circle-o"></i> 用户管理
        </a>
      </li>
    ```
2. 创建UserController和showList()方法
  * 在controller下新建UserController
    ```
      package com.itheima.controller;
      import org.springframework.stereotype.Controller;
      import org.springframework.web.bind.annotation.RequestMapping;
      import org.springframework.web.servlet.ModelAndView;
      @Controller
      @RequestMapping("/user")
      public class UserController {
          @RequestMapping("/showList")
          public ModelAndView showList() {
              ModelAndView modelAndView = new ModelAndView();
              // 设置视图
              modelAndView.setViewName("user-list");
              return modelAndView;
          }
      }
    ```
3. 创建UserSerivce和getList()方法
  * 在service下新建UserSerivce.java
    ```
      package com.itheima.service;
      import com.itheima.domain.User;
      import java.util.List;
      public interface UserSerivce {
          List<User> getList();
      }
    ```
  * 在impl下创建UserServiceImpl的实现类，去实现getList方法
    ```
      package com.itheima.service.impl;
      import com.itheima.domain.User;
      import com.itheima.service.UserSerivce;
      import org.springframework.beans.factory.annotation.Autowired;
      import org.springframework.stereotype.Service;
      import java.util.List;
      @Service("userService")
      public class UserServiceImpl implements UserSerivce {
          @Autowired
          private UserDao userDao;
          public List<User> getList() {
              List<User> userList = userDao.findAllList();
              return userList;
          }
      }

    ```
4. 创建RoleDao和findAllList()方法
  * 在dao下新建UserDao的接口
    ```
      package com.itheima.dao;
      import com.itheima.domain.User;
      import java.util.List;
      public interface UserDao {
          List<User> findAllList();
      }
    ```
  * 在impl下新建UserDaoImpl的实现类，去实现findAllList方法
    ```
      package com.itheima.dao.impl;
      import com.itheima.dao.UserDao;
      import com.itheima.domain.User;
      import org.springframework.stereotype.Repository;
      import java.util.List;
      @Repository("userDao")
      public class UserDaoImpl implements UserDao {
          public List<User> findAllList() {
              return null;
          }
      }
    ```
5. 使用JdbcTemplate完成查询操作
  * 在UserDaoImpl中，引入jdbctemplate
    ```
      package com.itheima.dao.impl;
      import com.itheima.dao.UserDao;
      import com.itheima.domain.User;
      import org.springframework.beans.factory.annotation.Autowired;
      import org.springframework.jdbc.core.BeanPropertyRowMapper;
      import org.springframework.jdbc.core.JdbcTemplate;
      import org.springframework.stereotype.Repository;
      import java.util.List;
      @Repository("userDao")
      public class UserDaoImpl implements UserDao {
          @Autowired
          private JdbcTemplate template;
          public List<User> findAllList() {
              List<User> userList =  template.query("select * from sys_user", new BeanPropertyRowMapper<User>(User.class));
              System.out.println(userList.toString());
              return userList;
          }
      }
    ```
6. 将查询数据存储到Model中
  * 在UserController.java中
    ```
      List<User> userList = userSerivce.getList();
      modelAndView.addObject("userList", userList);
    ```
7. 转发到user-list.jsp页面进行展示
  * 找到webapp/pages/user-list.jsp，引入jstl模板,在最顶部
    ```
      <%@ taglib prefix="c" uri="http://java.sun.com/jsp/jstl/core" %>
    ```
  * 使用jstl的foreach标签
    ```
      <c:forEach items="${userList}" var="user">
        <tr>
          <td><input name="ids" type="checkbox"></td>
          <td>${user.id}</td>
          <td>${user.username}</td>
          <td>${user.email}</td>
          <td>${user.phoneNum}</td>
          <td class="text-center">
            课程研究员&nbsp;讲师&nbsp;
          </td>
          <td class="text-center">
            <a href="javascript:void(0);" class="btn bg-olive btn-xs">删除</a>
          </td>
        </tr>
      </c:forEach>
    ```
8. 启动tomcat，输入localhost:8080/user/showList
{% note warning %}
注意:
  由于用户关联角色，前面的代码还没有查询到用户对应的角色，所以在service层去查询角色，不是在dao层
{% endnote %}

9. 在User实体类中添加Role集合
  * 在domain/User.java实体类中，添加Role集合
    ```
      // 当前用户具备哪些角色
      private List<Role> roles;
      public List<Role> getRoles() {
        return roles;
      }
      public void setRoles(List<Role> roles) {
        this.roles = roles;
      }
    ```

10. 修改UserServiceImpl.java，添加用户对应的角色
  ```
    package com.itheima.service.impl;
    import com.itheima.dao.RoleDao;
    import com.itheima.dao.UserDao;
    import com.itheima.domain.Role;
    import com.itheima.domain.User;
    import com.itheima.service.UserSerivce;
    import org.springframework.beans.factory.annotation.Autowired;
    import org.springframework.stereotype.Service;
    import java.util.List;
    @Service("userService")
    public class UserServiceImpl implements UserSerivce {
        @Autowired
        private UserDao userDao;
        @Autowired
        private RoleDao roleDao;
        public List<User> getList() {
            List<User> userList = userDao.findAllList();
            // 封装userList中的每一个User的roles数据
            for (User user : userList) {
                // 获得user得的id
                Long id = user.getId();
                // 将id作为参数，查询当前userid对应的Role集合数据
                List<Role> roles = roleDao.findRoleListById(id);
                user.setRoles(roles);
            }
            return userList;
        }
    }
  ```
11. 在RoleDao中，添加findRoleListById方法
  * 在RoleDao中，添加findRoleListById方法
    ```
      List<Role> findRoleListById(Long id);
    ```
  * 在impl.RoleDaoImpl.java中，实现findRoleListById方法
    ```
      public List<Role> findRoleListById(Long id) {
        List<Role> roleList = template.query("select * from sys_user_role ur, sys_role r where ur.userId=r.id and ur.userId=?", new BeanPropertyRowMapper<Role>(Role.class), id);
        return roleList;
      }
    ```
12. 修改webapp/pages/user-list.jsp
  ```
    <c:forEach items="${userList}" var="user">
      <tr>
        <td><input name="ids" type="checkbox"></td>
        <td>${user.id}</td>
        <td>${user.username}</td>
        <td>${user.email}</td>
        <td>${user.phoneNum}</td>
        <td class="text-center">
          <c:forEach items="${user.roles}" var="role">${role.roleDesc}</c:forEach>
        </td>
        <td class="text-center">
          <a href="javascript:void(0);" class="btn bg-olive btn-xs">删除</a>
        </td>
      </tr>
    </c:forEach>
  ```
13. 启动tomcat,点击用户管理

### 用户添加的效果
![添加用户](/images/spring/springtest4.jpg)

### 用户添加的步骤分析
1. 点击列表页面新建按钮跳转到用户添加页面
  * 找到webapp/pages/user-list.jsp, 修改以下代码
    ```
      <button 
        type="button" 
        class="btn btn-default" 
        title="新建" 
        onclick="location.href='${pageContext.request.contextPath}/user/userAdd'"
      >
        <i class="fa fa-file-o"></i> 新建
      </button>
    ```
  * 找到webapp/pages/user-add.jsp,修改以下代码
    ```
      <form action="${pageContext.request.contextPath}/user/save" method="post">
    ```
2. 编写UserController的userAdd()方法
  * 在刚进入新增页面时，还需要查询角色列表
  * 在UserController.java中，添加userAdd方法
    ```
      @Autowired
      private RoleService roleService;

      // 添加用户信息
      @RequestMapping("/userAdd")
      public ModelAndView userAdd() {
        ModelAndView modelAndView = new ModelAndView();
        List<Role> roleList = roleService.getRoleList();
        // 设置model
        modelAndView.addObject("roleList", roleList);
        // 设置视图
        modelAndView.setViewName("user-add");
        return modelAndView;
      }
    ```
  * 在webapp/pages/user-add.jsp，渲染角色展示
    ```
      <%@ taglib prefix="c" uri="http://java.sun.com/jsp/jstl/core" %>

      <div class="col-md-10 data">
        <c:forEach items="${roleList}" var="role">
          <input type="checkbox" name="roleIds" value="${role.id}">${role.roleName}
        </c:forEach>
      </div>
    ```
3. 输入用户信息，点击保存按钮，表单数据提交服务器
4. 编写UserService的saveItem()方法
  * 在UserController.java中，添加save方法
    ```
      // 新增用户
      @RequestMapping("/save")
      public String save(User user, Long[] roleIds) {
        userSerivce.saveItem(user, roleIds);
        return "redirect:/user/showList";
      }
    ```
  * 在UserService.java中添加saveItem
    ```
      void saveItem(User user, Long[] roleIds);
    ```
  * 在UserServiceImpl.java中实现saveItem方法
    ```
      // 保存用户数据
      public void saveItem(User user, Long[] roleIds) {
        // 第一步 向sys_user表中存储数据
        userDao.addItem(user);
        // 第二步 向sys_user_role 关系表中存储多条数据
        userDao.addUserRoleRel(user.getId(), roleIds);
      }
    ```
5. 编写UserDao的addItem()和addUserRoleRel方法
  * 在UserDao.java中，添加addItem()和addUserRoleRel方法
    ```
      void addItem(User user);
      void addUserRoleRel(Long id, Long[] roleIds);
    ```
  * 在UserDaoImpl.java中，实现addItem和addUserRoleRel方法
    ```
      public void addItem(User user) {
      
      }
      public void addUserRoleRel(Long userId, Long[] roleIds) {
        
      }
    ```
6. 使用JdbcTemplate保存User数据到sys_user
  ```
    public void addItem(User user) {
      template.update("insert into sys_user values(?,?,?,?,?)", null, user.getUsername(), user.getEmail(), user.getPassword(), user.getPhoneNum());
    }
    public void addUserRoleRel(Long userId, Long[] roleIds) {
      for (Long roleId : roleIds) {
        template.update("insert into sys_user_role values(?,?)", userId, roleId);
      }
    }
  ```
7. 跳转回角色列表页面
  * 启动tomcat,访问localhost:8080/user/userAdd,填写数据后点击保存
{% note warning %}
注意:
  由于新增时没有传入userId所以报错了，解决思路，在insert之后，返回userId
{% endnote %}

### 解决报错
1. 修改UserDao.java中的addItem方法
  ```
    Long addItem(User user);
  ```
2. 修改UserDaoImpl.java中的addItem实现方法
  ```
    public Long addItem(final User user) {
        // 创建PreparedStatementCreator
        PreparedStatementCreator creator = new PreparedStatementCreator() {
            public PreparedStatement createPreparedStatement(Connection connection) throws SQLException {
                // 使用原始jdbc完成一个PreparedStatement的组件
                PreparedStatement preparedStatement = connection.prepareStatement("insert into sys_user values(?,?,?,?,?)", PreparedStatement.RETURN_GENERATED_KEYS);
                preparedStatement.setObject(1, null);
                preparedStatement.setObject(2, user.getUsername());
                preparedStatement.setObject(3, user.getEmail());
                preparedStatement.setObject(4, user.getPassword());
                preparedStatement.setObject(5, user.getPhoneNum());
                return preparedStatement;
            }
        };

        // 创建keyHolder
        GeneratedKeyHolder keyHolder = new GeneratedKeyHolder();
        template.update(creator, keyHolder);

        // 获得生成的主键
        Long userId = keyHolder.getKey().longValue();

        // 返回当前保存用户的id, 该Id是数据库自动生成的
        return userId;
    }
  ```

3. 修改UserServiceImpl.java中的saveItem方法
  ```
    public void saveItem(User user, Long[] roleIds) {
      // 第一步 向sys_user表中存储数据
      Long userId = userDao.addItem(user);
      // 第二步 向sys_user_role 关系表中存储多条数据
      userDao.addUserRoleRel(userId, roleIds);
    }
  ```
  
## 删除用户操作
### 删除用户的效果
![删除用户](/images/spring/springtest5.jpg)

### 删除用户的步骤分析
1. 点击用户列表的删除按钮，发送请求到服务器端
  * 找到webapp/pages/user-list.jsp,修改以下代码
    ```
      <td class="text-center">
        <a href="${pageContext.request.contextPath}/user/del/${user.id}" class="btn bg-olive btn-xs" onclick="delUser(user.id)">删除</a>
      </td>
    ```
2. 编写UserController的deleteById()方法
  ```
    // 删除用户
    @RequestMapping("/del/{userId}")
    public String del(@PathVariable String userId) {
        userSerivce.deleteItem(userId);
        return "redirect:/user/showList";
    }
  ```
3. 编写UserService的deleteItem()方法
  * 在UserService.java中，添加deleteItem方法
    ```
      void deleteItem(String userId);
    ```
  * 在UserServiceImpl.java中实现deleteItem方法
    ```
      // 删除用户数据
      public void deleteItem(String userId) {
        // 删除sys_user_role表
        userDao.delUserRoleRel(userId);
        // 删除sys_user表
        userDao.delUserById(userId);
      }
    ```
4. 编写UserDao的delUserRoleRel()方法和delUserById()方法
  * 在UserDao.java中，添加delUserRoleRel()方法和delUserById()方法
    ```
      void delUserRoleRel(String userId);
      void delUserById(String userId);
    ```
  * 在UserDaoImpl.java中，实现delUserRoleRel()方法和delUserById()方法
    ```
      public void delUserRoleRel(String userId) {
          template.update("delete from sys_user_role where userId=?", userId);
      }
      public void delUserById(String userId) {
          template.update("delete from sys_user where id=?", userId);
      }
    ```
6. 跳回当前用户列表页面
  * 启动tomcat,访问localhost:8080/user/showList,点击删除按钮





 

  








    

