---
title: hexo环境搭建                                                                      # 标题(必填)
date: 2020年6月22日                                                                  # 开始时间(必填)
updated: 2020年6月22日                                                               # 更新时间
type: hexo                                                                         # 标签、分类和友情链接三個页面需要配置(必填)
description: 快速、简洁且高效的博客框架                                                            # 描述
keywords: hexo的搭建                                                                       # 关键词，便于搜索
top_img: https://timgsa.baidu.com/timg?image&quality=80&size=b9999_10000&sec=1592955098372&di=3a5c3a61faf47869b3656df83ea232a2&imgtype=0&src=http%3A%2F%2Fpic2.zhimg.com%2Fv2-f9654b817205f6af3e472af284ecc2b2_1200x500.jpg             # 文章的顶部图片
# comments: false                                                                     # 评论模块(默认为false)
# mathjax: false                                                                      # 显示mathjax(默认为false)
# katex: false                                                                        # 显示katex(默认为false)
aside: true                                                                         # 展示文章侧边栏(默认为true)
# highlight_shrink: false                                                             # 配置代碼框是否展开(true/false)
categories: 
  - 教程
  - Hexo                                                                             # 分类
tags: 
  - 教程     
  - Hexo                                                                       # 文章标签
cover: https://timgsa.baidu.com/timg?image&quality=80&size=b9999_10000&sec=1592955098372&di=3a5c3a61faf47869b3656df83ea232a2&imgtype=0&src=http%3A%2F%2Fpic2.zhimg.com%2Fv2-f9654b817205f6af3e472af284ecc2b2_1200x500.jpg                 # 文章的缩略图（用在首页）
---

# 环境搭建
## 安装node.js
  * 下载地址: [https://nodejs.org/en/](http://nodejs.org/en/)
  * 选择一个10以上的版本即可
  * 安装成功后，输入node -v出现版本号就算安装成功了 ![nodejs安装](/images/hexo/nodejs安装.jpg)

## 安装git bash
  * 下载地址: [https://git-scm.com/downloads](https://git-scm.com/downloads)
  * 安装成功后，在空白处右键出现gitbash就算安装成功了 ![git安装](/images/hexo/git安装.jpg)
  * 安装完之后，注册用户名和邮箱
    {% note info %}
        在控制台输入:
        git config --global user.name dj49846917
        git config --global user.email 821084785@qq.com
    {% endnote %}

## 安装hexo
  * 输入命令: npm install -g hexo-cli进行全局安装
  * 安装成功后，输入hexo -v出现版本号就算安装成功了 ![hexo安装](/images/hexo/hexo安装.jpg)

## 创建项目
 * 输入命令：hexo init blog

## 创建github仓库
  * 新建一个github项目，命名为: dj49846917.github.io 
    ![github新建项目](/images/hexo/github新建项目.jpg)
    ![github新建项目2](/images/hexo/github新建项目2.jpg)

## 创建ssh，便于推送github时不再重复输入账号密码
  * 在控制台输入：ssh-keygen -t rsa -C 821084785@qq.com, 然后会在C:\Users\ASUS生成.ssh文件夹，里面的id_rsa.pub就是我们想要的
  * 在github的settings=>SSH and GPG keys里，新建SSHkey
  ![获取ssh-key](/images/hexo/获取ssh-key.jpg)
  ![获取ssh-key2](/images/hexo/获取ssh-key2.jpg)
  ![获取ssh-key3](/images/hexo/获取ssh-key3.jpg)
  ![github绑定ssh-key](/images/hexo/github绑定ssh-key.jpg)
  ![github绑定ssh-key2](/images/hexo/github绑定ssh-key2.jpg)
  ![github绑定ssh-key3](/images/hexo/github绑定ssh-key3.jpg)
  ![github绑定ssh-key4](/images/hexo/github绑定ssh-key4.jpg)

## 部署到github
  * 输入命令：npm install hexo-deployer-git --save 这个包可以部署

  * 找到_config.yml，添加以下内容: 
    {% note primary %}
      deploy:
      type: 'git'
      repo: 'https://github.com/dj49846917/dj49846917.github.io.git',
      branch: master
    {% endnote %}

  * 执行命令:
    {% note primary %}
      hexo clean (清除缓存)
      hexo g (生成对应的文件)
      hexo d (发布)
      hexo new page 文件名 (新建页面)
    {% endnote %}

# 记录下hexo当前博客下的版本
  {% note info %}
    {
      "name": "hexo-site",
      "version": "0.0.0",
      "private": true,
      "scripts": {
        "build": "hexo generate",
        "clean": "hexo clean",
        "deploy": "hexo deploy",
        "server": "hexo server"
      },
      "hexo": {
        "version": "4.2.1"
      },
      "dependencies": {
        "hexo": "^4.2.1",
        "hexo-deployer-git": "^2.1.0",
        "hexo-generator-archive": "^1.0.0",
        "hexo-generator-category": "^1.0.0",
        "hexo-generator-index": "^1.0.0",
        "hexo-generator-tag": "^1.0.0",
        "hexo-renderer-ejs": "^1.0.0",
        "hexo-renderer-marked": "^2.0.0",
        "hexo-renderer-pug": "^1.0.0",
        "hexo-renderer-stylus": "^1.1.0",
        "hexo-server": "^1.0.0"
      }
    }
  {% endnote %}
