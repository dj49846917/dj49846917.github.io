---
title: spring2-aop
date: 2021-06-18 09:50:02
tags:
  - spring
  - aop
  - java
type: java                                                                         # 标签、分类和友情链接三個页面需要配置(必填)
description: Spring是分层的JavaSE/EE应用full-stack轻量级开源框架                   # 描述
keywords: spring                                                                       # 关键词，便于搜索
top_img: /images/spring/logo.jpg             # 文章的顶部图片
aside: true                                                                         # 展示文章侧边栏(默认为true)
categories: 
  - 教程
  - spring 
  - aop                                                               # 文章标签
  - java
cover: /images/spring/logo.jpg                 # 文章的缩略图（用在首页）
---

# Spring的AOP简介
## 什么是 AOP
  * AOP 为 Aspect Oriented Programming 的缩写，意思为面向切面编程，是通过预编译方式和运行期动态代理实现程序功能的统一维护的一种技术。
  * AOP 是 OOP 的延续，是软件开发中的一个热点，也是Spring框架中的一个重要内容，是函数式编程的一种衍生范型。利用AOP可以对业务逻辑的各个部分进行隔离，从而使得业务逻辑各部分之间的耦合度降低，提高程序的可重用性，同时提高了开发的效率。

## AOP 的作用及其优势
  * 作用：在程序运行期间，在不修改源码的情况下对方法进行功能增强
  * 优势：减少重复代码，提高开发效率，并且便于维护

## AOP 的底层实现
  * 实际上，AOP 的底层是通过 Spring 提供的的动态代理技术实现的。在运行期间，Spring通过动态代理技术动态的生成代理对象，代理对象方法执行时进行增强功能的介入，在去调用目标对象的方法，从而完成功能的增强。

## AOP 的动态代理技术
  * JDK 代理 : 基于接口的动态代理技术
  * cglib 代理：基于父类的动态代理技术

## AOP 相关概念
  * Target（目标对象）：代理的目标对象
  * Proxy （代理）：一个类被 AOP 织入增强后，就产生一个结果代理类
  * Joinpoint（连接点）：所谓连接点是指那些被拦截到的点。在spring中,这些点指的是方法，因为spring只支持方法类型的连接点（可以被增强的方法）
  * Pointcut（切入点）：所谓切入点是指我们要对哪些 Joinpoint 进行拦截的定义
  * Advice（通知/ 增强）：所谓通知是指拦截到 Joinpoint 之后所要做的事情就是通知
  * Aspect（切面）：是切入点和通知（引介）的结合
  * Weaving（织入）：是指把增强应用到目标对象来创建新的代理对象的过程。spring采用动态代理织入，而AspectJ采用编译期织入和类装载期织入

## AOP 开发明确的事项
  1. 需要编写的内容
    * 编写核心业务代码（目标类的目标方法）
    * 编写切面类，切面类中有通知(增强功能方法)
    * 在配置文件中，配置织入关系，即将哪些通知与哪些连接点进行结合

  2. AOP 技术实现的内容
    * Spring 框架监控切入点方法的执行。一旦监控到切入点方法被运行，使用代理机制，动态创建目标对象的代理对象，根据通知类别，在代理对象的对应位置，将通知对应的功能织入，完成完整的代码逻辑运行。
  
  3. AOP 底层使用哪种代理方式
    * 在 spring 中，框架会根据目标类是否实现了接口来决定采用哪种动态代理的方式。

## 知识要点
  * aop：面向切面编程
  * aop底层实现：基于JDK的动态代理 和 基于Cglib的动态代理
  * aop的重点概念：
    - Pointcut（切入点）：被增强的方法
    - Advice（通知/ 增强）：封装增强业务逻辑的方法
    - Aspect（切面）：切点+通知
    - Weaving（织入）：将切点与通知结合的过程
  * 开发明确事项：
    - 谁是切点（切点表达式配置）
    - 谁是通知（切面类中的增强方法）
    - 将切点和通知进行织入配置

# 基于XML的AOP开发
## 快速入门
  1. 导入 AOP 相关坐标
  2. 创建目标接口和目标类（内部有切点）
  3. 创建切面类（内部有增强方法）
  4. 将目标类和切面类的对象创建权交给 spring
  5. 在 applicationContext.xml 中配置织入关系
  6. 测试代码

### 导入AOP相关坐标
  1. 创建一个itheima_spring_aop的module,并配置webapp文件夹
  2. 在pom.xml中导入spring-context、aspectjweaver、junit包
    ```
      <dependency>
          <groupId>org.springframework</groupId>
          <artifactId>spring-context</artifactId>
          <version>5.3.7</version>
      </dependency>
      <!--aspectj:aop配置-->
      <dependency>
          <groupId>org.aspectj</groupId>
          <artifactId>aspectjweaver</artifactId>
          <version>1.9.4</version>
      </dependency>
      <dependency>
          <groupId>junit</groupId>
          <artifactId>junit</artifactId>
          <version>4.12</version>
      </dependency>
    ```

### 创建目标接口和目标类（内部有切点）
  1. 在java下新建com.itheima.aop.TargetInterface的接口
    ```
      package com.itheima.aop;
      public interface TargetInterface {
          public void buy();
      }
    ```

  2. 在aop下创建Target的实现类，去实现TargetInterface的buy方法(这个target就是目标类，buy方法就是一个连接点，也可以是一个切点)
    ```
      package com.itheima.aop;
      public class Target implements TargetInterface {
          public void buy() {
              System.out.println("男孩买了一个游戏机");
          }
      }
    ```

### 创建切面类（内部有增强方法）
  1. 在aop下，新建BuyAspect的切面类，并添加wantToBuy这个增强方法
    ```
      package com.itheima.aop;
      public class BuyAspect {
          public void wantToBuy() {
              System.out.println("男孩想买的东西是游戏机");
          }
      }
    ```

### 将目标类和切面类的对象创建权交给 spring
  1. 在resources下创建applicationContext.xml，并配置目标类和切面类
    ```
      <?xml version="1.0" encoding="UTF-8"?>
      <beans xmlns="http://www.springframework.org/schema/beans"
            xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
            xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd">

      <!--    配置目标类-->
          <bean id="buy" class="com.itheima.aop.Target"></bean>
      <!--    配置切面类-->
          <bean id="wangToBuy" class="com.itheima.aop.BuyAspect"></bean>
      </beans>
    ```

### 在 applicationContext.xml 中配置织入关系(也就是告诉spring框架，哪些方法(切点)需要被哪些增强(前置，后置...)，比如说例子里的，buy方法需要被wangToBuy增强)
  1. 导入aop命名空间
    ![aop命名空间](/images/spring/aop.jpg)

  2. 配置切点表达式和前置增强的织入关系
    ```
      <!--配置织入：告诉spring框架，哪些方法(切点)需要进行哪些增强(前置、后置...)-->
      <aop:config>
          <!--声明切面-->
          <aop:aspect ref="aspect">
              <!--切面：切点+通知-->
              <aop:before method="wantToBuy" pointcut="execution(public void com.itheima.aop.Target.buy())"></aop:before>
          </aop:aspect>
      </aop:config>
    ```
    
  * 注意：execution(public void com.itheima.aop.Target.buy())可以写成execution(* com.itheima.aop.*.*.(..))

### 测试代码
  1. 在test/java下，新建com.itheima.aop.AopTest的测试类，要使用spring的测试方法，需要在pom.xml中，导入spring-test包
    ```
      # 在pom.xml中：

        <dependency>
            <groupId>org.springframework</groupId>
            <artifactId>spring-test</artifactId>
            <version>5.3.7</version>
        </dependency>
    ```

    ```
      # 在com.itheima.aop.AopTest中：
      
        package com.itheima.aop;
        import org.junit.Test;
        import org.junit.runner.RunWith;
        import org.springframework.beans.factory.annotation.Autowired;
        import org.springframework.test.context.ContextConfiguration;
        import org.springframework.test.context.junit4.SpringJUnit4ClassRunner;

        @RunWith(SpringJUnit4ClassRunner.class)
        @ContextConfiguration("classpath:applicationContext.xml") // 读取appliationContext代码
        public class AopTest {
            @Autowired // 注入applicationContext的目标类
            private TargetInterface target;

            @Test
            public void test1() {
                target.buy();
            }
        }
    ```

  2. 测试结果
    ```
      男孩想买的东西是游戏机
      男孩买了一个游戏机
    ```

## XML 配置 AOP 详解
### 切点表达式的写法
  * 表达式语法：execution([修饰符] 返回值类型 包名.类名.方法名(参数))
    - 访问修饰符可以省略
    - 返回值类型、包名、类名、方法名可以使用星号*  代表任意
    - 包名与类名之间一个点 . 代表当前包下的类，两个点 .. 表示当前包及其子包下的类
    - 参数列表可以使用两个点 .. 表示任意个数，任意类型的参数列表

  * 举例：
    ```
      execution(public void com.itheima.aop.Target.method())	
      execution(void com.itheima.aop.Target.*(..))  // 表示Target下面的任意方法，任意参数
      execution(* com.itheima.aop.*.*(..))  // 表示aop包下的任意类，任意方法和参数
      execution(* com.itheima.aop..*.*(..)) // 表示aop包下的及其子包任意类，任意方法和参数
      execution(* *..*.*(..))
    ```

### 通知的类型
  * 通知的配置语法：`<aop:通知类型 method=“切面类中方法名” pointcut=“切点表达式"></aop:通知类型>`
  * ![通知方法](/images/spring/aop2.jpg)

### 切点表达式的抽取
  * 当多个增强的切点表达式相同时，可以将切点表达式进行抽取，在增强中使用 pointcut-ref 属性代替 pointcut 属性来引用抽取后的切点表达式。
  * 举例：
    1. 在XML的aop快速入门中，在切面类BuyAspect中，再添加一个payMoney的方法
      ```
        package com.itheima.aop;
        public class BuyAspect {
            public void wantToBuy() {
                System.out.println("男孩想买的东西是游戏机");
            }

            public void payMoney() {
                System.out.println("花了2000元");
            }
        }
      ```

    2. 在applicationContext.xml中，配置
      * 抽取前
        ```
          <!--配置织入：告诉spring框架，哪些方法(切点)需要进行哪些增强(前置、后置...)-->
          <aop:config>
              <!--声明切面-->
              <aop:aspect ref="aspect">
                  <!--切面：切点+通知-->
                  <aop:before method="wantToBuy" pointcut="execution(* com.itheima.aop.*.*(..))"></aop:before>
                  <aop:after method="payMoney" pointcut="execution(* com.itheima.aop.*.*(..))"></aop:after>
              </aop:aspect>
          </aop:config>
        ```

      * 抽取后：
        ```
          <!--配置织入：告诉spring框架，哪些方法(切点)需要进行哪些增强(前置、后置...)-->
          <aop:config>
              <!--声明切面-->
              <aop:aspect ref="aspect">
                  <!--抽取切点表达式-->
                  <aop:pointcut id="pointCut" expression="execution(* com.itheima.aop.*.*(..))"/>
                  <aop:before method="wantToBuy" pointcut-ref="pointCut"></aop:before>
                  <aop:after method="payMoney" pointcut-ref="pointCut"></aop:after>
              </aop:aspect>
          </aop:config>
        ```

## 知识要点
  * aop织入的配置
    ```
      <aop:config>
        <aop:aspect ref=“切面类”>
          <aop:before method=“通知方法名称” pointcut=“切点表达式"></aop:before>
        </aop:aspect>
      </aop:config>
    ```
  * 通知的类型：前置通知、后置通知、环绕通知、异常抛出通知、最终通知
  * 切点表达式的写法：`execution([修饰符] 返回值类型 包名.类名.方法名(参数))`

# 基于注解的AOP开发
## 快速入门
  1. 导入AOP相关坐标
  2. 创建目标接口和目标类（内部有切点）
  3. 创建切面类（内部有增强方法）
  4. 将目标类和切面类的对象创建权交给 spring
  5. 在切面类中使用注解配置织入关系
  6. 在配置文件中开启组件扫描和 AOP 的自动代理
  7. 测试

### 导入AOP相关坐标
  1. 创建一个itheima_spring_aop2的module,并配置webapp文件夹
  2. 在pom.xml中导入spring-context、aspectjweaver、junit包
    ```
      <dependency>
          <groupId>org.springframework</groupId>
          <artifactId>spring-context</artifactId>
          <version>5.3.7</version>
      </dependency>
      <!--aspectj:aop配置-->
      <dependency>
          <groupId>org.aspectj</groupId>
          <artifactId>aspectjweaver</artifactId>
          <version>1.9.4</version>
      </dependency>
      <dependency>
          <groupId>junit</groupId>
          <artifactId>junit</artifactId>
          <version>4.12</version>
      </dependency>
    ```

### 创建目标接口和目标类（内部有切点）
  1. 在java下新建com.itheima.aop.TargetInterface的接口
    ```
      package com.itheima.aop;
      public interface TargetInterface {
          public void buy();
      }
    ```

  2. 在aop下创建Target的实现类，去实现TargetInterface的buy方法(这个target就是目标类，buy方法就是一个连接点，也可以是一个切点)
    ```
      package com.itheima.aop;
      public class Target implements TargetInterface {
          public void buy() {
              System.out.println("男孩买了一个游戏机");
          }
      }
    ```

### 创建切面类（内部有增强方法）
  1. 在aop下，新建BuyAspect的切面类，并添加wantToBuy这个增强方法
    ```
      package com.itheima.aop;
      public class BuyAspect {
          public void wantToBuy() {
              System.out.println("男孩想买的东西是游戏机");
          }
      }
    ```

### 将目标类和切面类的对象创建权交给 spring
  1. 在Target目标类中，添加@Component注解
    ```
      package com.itheima.aop;
      import org.springframework.stereotype.Component;

      @Component("target")
      public class Target implements TargetInterface {
          public void buy() {
              System.out.println("男孩买了一个游戏机");
          }
      }
    ```

  2. 在BuyAspect切面类中，添加@Conponent注解
    ```
      package com.itheima.aop;
      import org.springframework.stereotype.Component;

      @Component("aspect")
      public class BuyAspect {
          public void wantToBuy() {
              System.out.println("男孩想买的东西是游戏机");
          }
          public void payMoney() {
              System.out.println("花了2000元");
          }
      }
    ```

### 在切面类中使用注解配置织入关系
  1. 在BuyAspect切面类中，添加@Aspect和@Before注解
    * @Aspect
      - 标注当前BuyAspect是一个切面类
    * @Before(value = "execution(* com.itheima.aop.*.*(..))") 
      - 配置前置通知
    ```
      package com.itheima.aop;
      import org.aspectj.lang.annotation.After;
      import org.aspectj.lang.annotation.Aspect;
      import org.aspectj.lang.annotation.Before;
      import org.springframework.stereotype.Component;

      @Component("aspect")
      @Aspect // 标注当前BuyAspect是一个切面类
      public class BuyAspect {
          // 配置前置通知
          @Before(value = "execution(* com.itheima.aop.*.*(..))")
          public void wantToBuy() {
              System.out.println("男孩想买的东西是游戏机");
          }
          @After(value = "execution(* com.itheima.aop.*.*(..))")
          public void payMoney() {
              System.out.println("花了2000元");
          }
      }
    ```

### 在配置文件中开启组件扫描和 AOP 的自动代理
  1. 在applicationContext.xml中添加aop和context的命名空间
    * ![命名空间](/images/spring/aop3.jpg)
  2. 开启组件扫描和AOP自动代理
    ```
      <?xml version="1.0" encoding="UTF-8"?>
      <beans xmlns="http://www.springframework.org/schema/beans"
            xmlns:context="http://www.springframework.org/schema/context"
            xmlns:aop="http://www.springframework.org/schema/aop"
            xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
            xsi:schemaLocation="
            http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd
            http://www.springframework.org/schema/context http://www.springframework.org/schema/context/spring-context.xsd
            http://www.springframework.org/schema/aop http://www.springframework.org/schema/aop/spring-aop.xsd
      ">
      <!--    扫描组件-->
          <context:component-scan base-package="com.itheima.aop"></context:component-scan>
      <!--    aop自动代理-->
          <aop:aspectj-autoproxy></aop:aspectj-autoproxy>
      </beans>
    ```

### 测试
  1. 在test/java下，新建com.itheima.aop.AopTest的测试类
    ```
      package com.itheima.aop;
      import org.junit.Test;
      import org.junit.runner.RunWith;
      import org.springframework.beans.factory.annotation.Autowired;
      import org.springframework.test.context.ContextConfiguration;
      import org.springframework.test.context.junit4.SpringJUnit4ClassRunner;

      @RunWith(SpringJUnit4ClassRunner.class)
      @ContextConfiguration("classpath:applicationContext.xml")
      public class AopTest {
          @Autowired
          private TargetInterface target;
          @Test
          public void test1() {
              target.buy();
          }
      }
    ```

  2. 测试结果：
    ```
      男孩想买的东西是游戏机
      男孩买了一个游戏机
      花了2000元
    ```

## 注解配置AOP详解
### 注解通知的类型
  * 通知的配置语法：@通知注解(“切点表达式")
  * ![通知注解](/images/spring/aop4.jpg)

### 切点表达式的抽取
  * 同 xml 配置 aop 一样，我们可以将切点表达式抽取。抽取方式是在切面内定义方法，在该方法上使用@Pointcut注解定义切点表达式，然后在在增强注解中进行引用。具体如下：
    - BuyAspect切面类抽取前
      ```
        package com.itheima.aop;
        import org.aspectj.lang.annotation.After;
        import org.aspectj.lang.annotation.Aspect;
        import org.aspectj.lang.annotation.Before;
        import org.springframework.stereotype.Component;

        @Component("aspect")
        @Aspect // 标注当前BuyAspect是一个切面类
        public class BuyAspect {
            // 配置前置通知
            @Before(value = "execution(* com.itheima.aop.*.*(..))")
            public void wantToBuy() {
                System.out.println("男孩想买的东西是游戏机");
            }
            @After(value = "execution(* com.itheima.aop.*.*(..))")
            public void payMoney() {
                System.out.println("花了2000元");
            }
        }
      ``` 

    - BuyAspect切面类抽取后
      ```
        package com.itheima.aop;
        import org.aspectj.lang.annotation.After;
        import org.aspectj.lang.annotation.Aspect;
        import org.aspectj.lang.annotation.Before;
        import org.aspectj.lang.annotation.Pointcut;
        import org.springframework.stereotype.Component;

        @Component("aspect")
        @Aspect // 标注当前BuyAspect是一个切面类
        public class BuyAspect {
            // 配置前置通知
            @Before("BuyAspect.pointcut()")
            public void wantToBuy() {
                System.out.println("男孩想买的东西是游戏机");
            }
            @After("BuyAspect.pointcut()")
            public void payMoney() {
                System.out.println("花了2000元");
            }
            // 定义切点表达式
            @Pointcut("execution(* com.itheima.aop.*.*(..))")
            // 方法名随便定义
            public void pointcut(){}
        }
      ```

## 知识要点
  * 注解aop开发步骤
    1. 使用@Aspect标注切面类
    2. 使用@通知注解标注通知方法
    3. 在配置文件中配置aop自动代理`<aop:aspectj-autoproxy/>`
  * 通知注解类型
    - ![通知类型](/images/spring/aop4.jpg)











