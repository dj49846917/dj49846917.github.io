---
title: lowcodeEngine
date: 2022-05-20 11:01:31
tags:
  - 低代码
  - lowcodeEngine
type: lowcodeEngine                                                                         # 标签、分类和友情链接三個页面需要配置(必填)
description: 基于 Low-Code Engine 快速打造高生产力的低代码研发平台。                   # 描述
keywords: lowcodeEngine                  # 关键词，便于搜索
top_img: /images/lowcode/logo.png             # 文章的顶部图片
aside: true                                                                         # 展示文章侧边栏(默认为true)
categories: 
  - lowcodeEngine
  - 低代码
cover: /images/lowcode/logo.png                 # 文章的缩略图（用在首页）
---

# 记录阿里低代码框架lowcode-engine的学习之路

# 框架相关文档
  * 文档地址：https://lowcode-engine.cn/docV2/intro

# 拓展框架区
## 自定义辅助操作区
> 需求如下，在拖拽的组件中，添加操作按钮，比如添加一个增加按钮。如图所示：
> ![添加按钮](/images/lowcode/添加辅助区按钮.png)

1. 在API文档中的material-物料API中找到addBuiltinComponentAction这个方法，如图所示：![辅助区api](/images/lowcode/辅助区api.png)
2. 启动lowcode-demo项目，在项目中搜索关键字"addBuiltinComponentAction",找到在src/node-extended-actions/plugin.tsx
  * ![辅助区代码](/images/lowcode/辅助区代码.png)
  * 界面效果：在左侧拖动nextTable到编辑器，就会看到一个按钮。![默认辅助区按钮](/images/lowcode/默认辅助区按钮.png)
3. 添加一个增加按钮
  * 在src/node-extended-actions/plugin.tsx中，添加如下代码
    ```
      const addHelloAction = (ctx: ILowCodePluginContext) => {
        return {
          async init() {
            console.log("ctx", ctx.material.addBuiltinComponentAction)
            const { addBuiltinComponentAction } = ctx.material;
            addBuiltinComponentAction({
              name: 'hello',
              content: {
                icon: <Icon type="atm" size="small" />,
                title: 'hello',
                action(node: Node) {
                  Message.show('Welcome to Low-Code engine');
                },
              },
              condition: (node: Node) => {
                return node.componentMeta.componentName === 'NextTable';
              }
            });

            //-------------------添加部分-----------------
            addBuiltinComponentAction({
              name: 'add',
              content: {
                icon: "add",
                title: 'add',
                action(node: Node) {
                  Message.show('1111');
                },
              }
            });
          }
        };
      }
    ```
  * 查看页面效果：![添加效果](/images/lowcode/添加按钮效果.png)


