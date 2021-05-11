---
title: springboot2
date: 2021-05-11 15:39:39
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

# springboot集成redis
## 下载redis的windows安装包并安装
  * 下载地址：https://github.com/MicrosoftArchive/redis/releases
  * 安装就一路next，然后找到安装redis的目录，新建start.bat，并写入：
    ```
      redis-server.exe redis.windows.conf
    ```
  * 点击start.bat启动(退出redis命令：Ctrl+c)

## 安装redis客户端Redis Desktop Manager
  * 下载地址: https://pan.baidu.com/s/1Jvr9MbgFn4UJh4M1AMo3gA 提取码：3i9b
  * 安装就一路next，然后先启动redis,然后，打开客户端
  * ![集成redis](/images/springboot/创建redis.jpg)

## 代码编写
  1. 新建StudentController类，并写入：
    ```
      package com.dj.app.springboot.web;

      import com.dj.app.springboot.service.StudentService;
      import org.springframework.beans.factory.annotation.Autowired;
      import org.springframework.stereotype.Controller;
      import org.springframework.web.bind.annotation.RequestMapping;
      import org.springframework.web.bind.annotation.ResponseBody;

      @Controller
      public class StudentController {
          @Autowired
          private StudentService studentService;
          // 存
          @RequestMapping(value = "/put")
          public @ResponseBody Object put(String key, String value) {
              studentService.put(key, value);
              return "值已存进redis";
          }

          // 取
          @RequestMapping(value = "/get")
          public @ResponseBody String get() {
              String name = studentService.get("name");
              return "数据为：" + name;
          }
      }
    ```

  2. 新建StudentService接口类，并写入:
    ```
      package com.dj.app.springboot.service;

      public interface StudentService {
          /**
          * 将值存进redis
          * @param key
          * @param value
          */
          void put(String key, String value);
          String get(String name);
      }
    ```
  3. 新建StuentServiceImpl实现类，并写入:
    ```
      package com.dj.app.springboot.service.impl;

      import com.dj.app.springboot.service.StudentService;
      import org.springframework.beans.factory.annotation.Autowired;
      import org.springframework.data.redis.core.RedisTemplate;
      import org.springframework.stereotype.Service;

      @Service
      public class StudentServiceImpl implements StudentService {
          @Autowired
          private RedisTemplate<Object, Object> redisTemplate;

          @Override
          public void put(String key, String value) {
              redisTemplate.opsForValue().set(key, value);
          }

          @Override
          public String get(String name) {
              String res = (String) redisTemplate.opsForValue().get(name);
              return res;
          }
      }
    ```

  4. 在application.yml中，配置redis:
    ```
      spring:
      # 设置redis
      redis:
        port: 6379
        host: localhost
    ```
  5. 启动，并输入：localhost:8080/put?key=name&value=zhangsan，localhost:8080/get能看到结果