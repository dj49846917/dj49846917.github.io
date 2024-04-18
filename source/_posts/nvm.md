---
title: mac安装nvm
date: 2023-12-15 14:36:14
tags: 
  - nvm
type: hexo                                                                         # 标签、分类和友情链接三個页面需要配置(必填)
description: nodejs版本管理库                                                         # 描述
keywords: nvm                                                                    # 关键词，便于搜索
aside: true                                                                         # 展示文章侧边栏(默认为true)
categories: 
  - 教程
  - nvm
---

# mac安装nvm
1. 删除已安装的node环境和全局node模块
  ```
    // 删除全局 node_modules 目录
    sudo rm -rf /usr/local/lib/node_modules 
    // 删除 node
    sudo rm /usr/local/bin/node 
    // 删除全局 node 模块注册的软链
    cd  /usr/local/bin && ls -l | grep "../lib/node_modules/" | awk '{print $9}'| xargs rm
  ```

2. 安装nvm
  * 打开终端输入：git clone https://github.com/nvm-sh/nvm.git
  * 再进入 nvm目录中执行install.sh 等待执行完成
  * 输入：cd nvm （进入nvm目录）
  * 再输入：./install.sh  （等待执行成功）

3. 配置.bash_profile
  * 进入bash_profile
    ```
      cd ~
      open ~/.bash_profile
    ```
  * 复制以下配置信息并保存
    ```
      export NVM_DIR="$HOME/.nvm"
        [ -s "$NVM_DIR/nvm.sh" ] && \. "$NVM_DIR/nvm.sh" # This loads nvm
        [ -s "$NVM_DIR/bash_completion" ] && \. "$NVM_DIR/bash_completion" # This loads nvm bash_completion
    ```
  * 重新执行刚修改的初始化文件，使之立即生效
    ```
      source ~/.bash_profile
    ```

4. 配置.zshrc
  * 进入.zshrc
    ```
      cd ~
      open ~/.zshrc

      如果没有.zshrc,则执行touch ~/.zshrc
    ```
  * 复制以下配置信息并保存
    ```
      export NVM_DIR="$HOME/.nvm"
          [ -s "$NVM_DIR/nvm.sh" ] && \. "$NVM_DIR/nvm.sh" # This loads nvm
          [ -s "$NVM_DIR/bash_completion" ] && \. "$NVM_DIR/bash_completion"


      PATH=/bin:/usr/bin:/usr/local/bin:${PATH}
      export PATH
    ```
  * 重新执行刚修改的初始化文件
    ```
      source ~/.zshrc

      source ~/.bash_profile
    ```

5. 执行nvm -v看到版本号表示操作成功。

# nvm常用命令
  ```
    nvm list 	是查找本电脑上所有的node版本

    nvm list 	查看已经安装的版本

    nvm list installed 	查看已经安装的版本

    nvm list available 	查看网络可以安装的版本

    nvm install 	安装最新版本nvm

    nvm use 		## 切换使用指定的版本node

    nvm ls 		列出所有版本

    nvm current		显示当前版本

    nvm alias 		## 给不同的版本号添加别名

    nvm unalias 	## 删除已定义的别名

    nvm reinstall-packages 	## 在当前版本node环境下，重新全局安装指定版本号的npm包

    nvm on 	打开nodejs控制

    nvm off 	关闭nodejs控制

    nvm proxy 	查看设置与代理

    nvm node_mirror [url] 
    设置或者查看setting.txt中的node_mirror，如果不设置的默认是 https://nodejs.org/dist/

    nvm npm_mirror [url] 
    设置或者查看setting.txt中的npm_mirror,如果不设置的话默认的是： https://github.com/npm/npm/archive/.

    nvm uninstall 卸载制定的版本

    nvm use [version] [arch] 切换制定的node版本和位数

    nvm root [path] 设置和查看root路径

    nvm version 查看当前的版本
  ```
    
