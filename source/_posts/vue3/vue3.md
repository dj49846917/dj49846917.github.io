---
title: vue3开发总结
date: 2021-01-04 13:41:56
tags: 
  - vue
type: vue                                                                 # 标签、分类
description:  Vue是一套用于构建用户界面的渐进式框架。
top_img: /images/vue/vue.jpg             # 文章的顶部图片
aside: true                                                                         # 展示文章侧边栏(默认为true)
categories: 
  - vue                                                                 # 文章标签
cover: /images/vue/vue.jpg                 # 文章的缩略图（用在首页）
---

# 开发环境搭建
## 安装插件
  1. 对于Vue 3，您应该使用Vue CLI v4.5（可从上npm获得）@vue/cli。要升级，您需要在@vue/cli全球范围内重新安装最新版本：
    ```
      yarn global add @vue/cli
      # OR
      npm install -g @vue/cli
    ```
  
  2. 创建项目
    ```
      vue create vue3-typescript
    ```

## 安装设置
  ```
    1. Please pick a preset: Manually select features

    2.  (*) Choose Vue version
        (*) Babel
        (*) TypeScript
        ( ) Progressive Web App (PWA) Support
        (*) Router
        (*) Vuex
        (*) CSS Pre-processors
        (*) Linter / Formatter
        ( ) Unit Testing
        ( ) E2E Testing

    3. ? Choose a version of Vue.js that you want to start the project with 
          2.x
        > 3.x (Preview)

    4. Use class-style component syntax? No

    5. Use Babel alongside TypeScript (required for modern mode, auto-detected polyfills, transpiling JSX)? No

    6. Use history mode for router? (Requires proper server setup for index fallback in production) Yes

    7. Pick a CSS pre-processor (PostCSS, Autoprefixer and CSS Modules are supported by default): (Use arrow keys)
      > Sass/SCSS (with dart-sass)
        Sass/SCSS (with node-sass)
        Less
        Stylus

    8. Pick a linter / formatter config: (Use arrow keys)
      > ESLint with error prevention only
        ESLint + Airbnb config
        ESLint + Standard config
        ESLint + Prettier
        TSLint (deprecated)

    9. Pick additional lint features: (Press <space> to select, <a> to toggle all, <i> to invert selection)
      >(*) Lint on save
      ( ) Lint and fix on commit

    10. Where do you prefer placing config for Babel, ESLint, etc.? (Use arrow keys)
        > In dedicated config files
          In package.json

    11. Save this as a preset for future projects? (y/N) n
  ```

## 安装vantui
  ```
    npm i vant@next -S
  ```

  * 注意：在安装了vant后会报以下错误：
    - ![安装时的报错](/images/vue/installError.jpg)
    - 解决办法：
      