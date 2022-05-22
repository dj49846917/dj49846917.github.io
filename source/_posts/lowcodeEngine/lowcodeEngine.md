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

# 开发环境搭建
## 在windows中安装WSL
1. 打开WSL官网：https://docs.microsoft.com/zh-cn/windows/wsl/install
2. 使用手动安装WSL
  1. 启用适用于 Linux 的 Windows 子系统
      * **以管理员身份打开 PowerShell（开始菜单右键=>Windows PowerShell<管理员>），并执行命令:**
        ```
          dism.exe /online /enable-feature /featurename:Microsoft-Windows-Subsystem-Linux /all /norestart
        ```
      - ![打开powershell](/images/lowcode/安装wsl.jpg)
      - ![安装wsl2](/images/lowcode/安装wsl2.jpg)
  2. 检查运行WSL2的要求
      * 若要更新到 WSL 2，需要运行 Windows 10。
        - 对于 x64 系统：版本 1903 或更高版本，采用内部版本 18362 或更高版本。
        - 对于 ARM64 系统：版本 2004 或更高版本，采用内部版本 19041 或更高版本。
        - 低于 18362 的版本不支持 WSL 2。 使用 Windows Update 助手更新 Windows 版本。
      * 若要检查 Windows 版本及内部版本号，选择 Windows 徽标键 + R，然后键入“winver”，选择“确定”。![安装wsl3.jpg](/images/lowcode/安装wsl3.jpg)
  3. 启用虚拟机功能
      * **在管理员power shell中执行以下命令，成功后重启电脑** 
        ```
          dism.exe /online /enable-feature /featurename:VirtualMachinePlatform /all /norestart
        ```
        - ![安装wsl4.jpg](/images/lowcode/安装wsl4.jpg)
  4. 下载Linux内核更新包
      * 点击链接下载  ![安装wsl5.jpg](/images/lowcode/安装wsl5.jpg)
      * 一路next安装即可
  5. 将WSL2设置为默认版本
      * 普通powershell中执行如下命令：
        ```
          wsl --set-default-version 2
        ```
        - ![安装wsl6.jpg](/images/lowcode/安装wsl6.jpg)
  6. 安装所选的Linux分发
      * 选择Ubuntu 18.04LTS或者20.04LTS ![安装wsl7.jpg](/images/lowcode/安装wsl7.jpg)
      * 点开，这里需要翻墙才能显示内容，再点击"在Microsoft Store获取"
        - ![安装wsl8.jpg](/images/lowcode/安装wsl8.jpg)
        - ![安装wsl9.jpg](/images/lowcode/安装wsl9.jpg)
        - ![安装wsl10.jpg](/images/lowcode/安装wsl10.jpg)
      * 如果安装时输入用户名提示“参考的对象类型不支持尝试的操作”，可执行如下命令并重启电脑即可
        ```
          netsh winsock reset
        ```

## 在ubuntu中安装nvm
  1. 检查是否安装git，如果没有安装就请安装上
  2. 克隆代码到文件夹 .nvm
    ```
      git clone https://github.com/creationix/nvm.git .nvm
    ```
  3. 进入nvm代码目录，切换到v0.33.11版本，执行命令
    ```
      cd ~/.nvm 
      git checkout v0.33.11 
      . nvm.sh
    ```
  4. 验证，出来版本号，说明安装成功
    ```
      nvm --version
    ```
  5. 设置环境, cd到安装nvm的路径，执行
    ```
      vim .bashrc
    ```
  6. 按下"i"键进入vim编辑模式，按esc退出修改，输入:wq回车完成修改。在空白处输入下列命令：
    ```
      source ~/.nvm/nvm.sh
    ```
## 在ubuntu中安装nodejs
   * 执行nvm install 14.19.3,等待安装成功即可
   * 如果前面没有安装nvm，可用以下步骤直接安装nodejs指定版本
      * 更新ubuntu最小版本：
       ```
         sudo apt-get update
       ```
      * 指定node版本（14.x）
       ```
         curl -sL https://deb.nodesource.com/setup_14.x | sudo -E bash -
       ```
      * 安装node
       ```
         sudo apt-get install -y nodejs
       ```
## 在ubuntu中配置vscode
> 只需要在vscode中安装"Remote-WSL"插件即可 ![配置vscode](/images/lowcode/配置vscode.jpg)

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


