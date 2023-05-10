---
title: bpmnjs
date: 2022-05-24 15:10:41
tags:
  - 流程图
  - bpmnjs
type: bpmnjs
description: BPMN 2.0 查看器和编辑器。 
keywords: bpmnjs
top_img: /images/bpmn/logo.jpg
aside: true
categories: 
  - bpmnjs
  - 流程图
cover: /images/bpmn/logo.jpg
---

# 搭建ant-design-pro工程
  1. 创建myapp的ant-design-pro工程, 选择ant-design-pro => typescript => simple
    ```
      npx create-umi myapp

      npm install
      npm run start
    ```
    ![环境搭建](/images/bpmn/环境搭建.jpg)

# 在ant-desin-pro中集成bpmnjs
  1. 安装bpmnjs及其相关的包
    ```
      npm install --save bpmn-js@10.3.0 bpmn-js-properties-panel@1.11.2 camunda-bpmn-moddle@7.0.1
    ```

  2. 初始化配置
  [详细请看这里](https://github.com/dj49846917/antd-pro-experience/blob/v5/src/pages/template/bpmn/CustomPanel/index.tsx)
 
  
