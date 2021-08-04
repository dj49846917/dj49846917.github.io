---
title: maven基础
date: 2021-08-03 08:34:52
tags:
  - maven
type: maven                                                                         
description: maven的本质是一个项目管理工具，讲项目开发和管理过程抽象成一个项目对象模型    # 描述
keywords: maven                  # 关键词，便于搜索
top_img: /images/maven/log.jpg             # 文章的顶部图片
aside: true                                                                         # 展示文章侧边栏(默认为true)
categories: 
  - 教程
  - maven
cover: /images/maven/log.jpg                 # 文章的缩略图（用在首页）
---

# Maven简介
## Maven是什么
  * Maven的本质是一个项目管理工具，将项目开发和管理过程抽象成一个项目对象模型(POM)
  * POM: 项目对象模型

## Maven的作用
  * 项目构建：提供标准的、跨平台的自动化项目构建方式
  * 依赖管理：方便快捷的管理项目依赖的资源(jar包)，避免资源间的版本冲突问题
  * 统一开发结构：提供标准的、统一的项目结构

# 下载与安装
## 下载
  * 官网：http://maven.apache.org/
  * 下载地址：http://maven.apache.org/download.cgi
  * ![下载](/images/maven/安装.jpg)

## 安装
  * 将下好的zip包解压

## Maven环境变量配置
  * 依赖Java, 需要配置JAVA_HOME
  * 设置Maven自身的运行环境，需要配置MAVEN_HOME
    - 在环境变量中添加MAVEN_HOME,并添加在path中
    - ![环境变量](/images/maven/环境变量.jpg)
    - ![环境变量](/images/maven/环境变量2.jpg)

# Maven基本概念
## 仓库
  * 仓库：用于存储资源，包含各种jar包
  * 仓库分类：
    - 本地仓库：自己电脑上存储资源的仓库，远程连接仓库获取资源
    - 远程仓库：非本机电脑上的仓库，为本地仓库提供资源
      - 中央仓库：Maven团队维护，存储所有资源的仓库
      - 私服：部门/公司范围内存储资源的仓库，从中央仓库获取资源
  * 私服的作用：
    - 保存具有版权的资源，包含购买或自主研发的jar
      - 中央仓库中的jar都是开源的，不能存储具有版权的资源
    - 一定范围内共享资源，仅对内部开放，不对外共享

## 坐标
  * 什么是坐标
    - Maven中的坐标用于描述仓库中资源的位置
  
  * Maven坐标主要注册部分
    - groupId: 定义当前maven项目隶属组织名称（通常hi域名反写，列入：org.mybatis）
    - artifactId: 定义当前maven项目名称（通常是模块名称，列入CRM, SMS）
    - version：定义当前项目的版本号
    - packaging: 定义该项目的打包方式（不重要）
  
  * Maven坐标的作用
    - 使用唯一标识，唯一性定位资源位置，通过该标记可以将资源的识别和下载工作交由机器完成 

## 本地仓库配置
* Maven启动后，会自动保存下载的资源到仓库
    - 默认位置：`C:\Users\lip\.m2\repository`
{% note primary %}
注意：可以把maven下载的内容放到其他位置
  * 比如说在D盘下新建一个maven\repository文件夹，希望把maven下载的东西放到这个文件夹中
  * 找到下载maven目录下的config文件夹下的settings.xml，修改以下的配置 ![修改存储位置](/images/maven/修改存储位置.jpg)
{% endnote %}

## 远程仓库配置
  * Maven默认连接的仓库位置：找到maven下载的路径下的lib/maven-model-builder-3.6.3.jar,使用WinRAR打开，找到org/apache/maven/model/pom.4.0.0.xml，右键点击查看文件，能看到下面代码：
    ```
      <repository>
        <id>central</id>
        <name>Central Repository</name>
        <url>https://repo.maven.apache.org/maven2</url>
        <layout>default</layout>
        <snapshots>
          <enabled>false</enabled>
        </snapshots>
      </repository>
    ```

{% note primary %}
注意：由于中央仓库是国外的服务器，下载比较忙，安装阿里的maven的镜像仓库
  * 找到下载maven目录下的config文件夹下的settings.xml，修改以下的配置 ![镜像仓库](/images/maven/镜像仓库.jpg)
{% endnote %}

## 全局setting与用户setting区别
  * 全局setting定义了当前计算机中的Maven的公共配置
  * 用户setting定义了当前用户的配置

# 第一个Maven项目(idea生成)
  1. 在idea中新建一个空的project
  2. maven配置
    * 点击左上角的File => Project Structure => Project ![idea配置maven](/images/maven/idea配置.jpg)
    * 点击左上角的File => settings，在搜索框搜索maven ![idea配置maven](/images/maven/idea配置2.jpg)
  3. 插件
    * 在pom.xml中添加
      ```
        <build>
          <plugins>
            <plugin>
              <groupId>org.apache.tomcat.maven</groupId>
              <artifactId>tomcat7-maven-plugin</artifactId>
              <version>2.1</version>
              <configuration>
                <port>80</port>
                <path>/</path>
              </configuration>
            </plugin>
          </plugins>
        </build>
      ```

# 依赖管理
## 依赖配置
  * 依赖指当前项目运行所需的jar,一个项目可以设置多个依赖
  * 格式：
    ```
      <!-- 设置当前项目所有的依赖 -->
      <dependencies>
          <!-- 设置具体的依赖 -->
          <dependency>
              <!-- 依赖所属群组id -->
              <groupId>junit</groupId>
              <!-- 依赖所属项目id -->
              <artifactId>junit</artifactId>
              <!-- 依赖版本号 -->
              <version>4.12</version>
          </dependency>
      </dependencies>
    ```

## 依赖传递
  * 依赖具有传递性
    - 直接传递：在当前项目中通过依赖配置建立的依赖关系
    - 简介传递：被资源的资源如果依赖其他资源，当前项目间接依赖其他资源

## 依赖传递冲突问题
  * 路径优先：当依赖中出现相同的资源时，成绩越深，优先级越低，层级越浅，优先级越高
  * 声明优先：当资源在相同层级被依赖时，配置顺序靠前的覆盖配置顺序靠后的
  * 特殊优先：当同时配置了相同的资源的不同版本，后配置的覆盖先配置的

## 可选依赖
  * 可选依赖指对外隐藏当前所依赖的资源--不透明（通过optional标签）
    ```
      <dependency>
        <groupId>junit</groupId>
        <artifactId>junit</artifactId>
        <version>4.12</version>
        <optional>true</optional>
      </dependency>
    ```

## 排除依赖
  * 排除依赖指主动断开依赖的资源，被排除的资源无需指定版本--不需要（通过exclusions标签）
    ```
      <dependency>
        <groupId>junit</groupId>
        <artifactId>junit</artifactId>
        <version>4.12</version>
          <exclusions>
            <exclusion>
            <groupId>org.hamcrest</groupId>
            <artifactId>hamcrest-core</artifactId>
          </exclusion>
        </exclusions>
      </dependency>
    ```

## 依赖范围
  * 依赖的jar默认的情况可以在任何地方使用，可以通过scope标签设置其作用范围
  * 作用范围：
    - 主程序范围有效（main文件夹范围内）
    - 测试程序范围有效（test文件夹范围内）
    - 是否参与打包（package指令范围内） ![标签](/images/maven/标签.jpg)

## 依赖范围传递性
  * 带有依赖范围的资源在进行传递时，作用范围将受到影响

# 生命周期与插件
## 构建生命周期
  * maven构建生命周期描述的是一次构建过程经历经历了多少个事件
    - compile => test-compile => test => package => install

  * Maven对项目构建的生命周期划分为3套
    - clean：清理工作
    - default：核心工作，例如：编译，测试，打包，部署等
    - site：产生报告，发布站点等

### clean生命周期
  * ![生命周期](/images/maven/生命周期3.jpg)

### default构建生命周期 
  * ![生命周期](/images/maven/生命周期.jpg)

### site构建生命周期
  * ![生命周期](/images/maven/生命周期2.jpg)

## 插件
  * 插件与生命周期内的阶段绑定，在执行到对应的生命周期时执行对应的插件功能
  * 默认maven在各个生命周期上绑定具有预设的功能
  * 通过插件可以自定义其他功能