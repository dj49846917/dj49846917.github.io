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
      
# vite的使用
## 安装插件
  * npm init @vitejs/app
    - ![vite安装](/images/vue/vite安装.jpg)
    - npm install

## 在vite中使用vue-router
  1. 安装：
    - npm install --save vue-router@4
  2. 使用：
    - 在src下新建文件夹 router，在router下建立文件index.ts，并写入以下代码
      ```
        import { createRouter, createWebHistory } from 'vue-router';

        const routes = [
          {
            path: '/',
            name: 'home',
            component: () => import('@/views/home.vue'), //路由懒加载
          }
        ]

        const router = createRouter({
          history: createWebHistory("/"), //history模式使用HTML5模式
          routes,
        });

        export default router;
      ```
    
    - 在main.ts中引入
      ```
        import router from '@/router'

        import { createApp } from 'vue'
        import App from './App.vue'
        import router from '@/router'

        const app = createApp(App)
        app.use(router).mount('#app')
      ```

    - 在tsconfig.json中添加@对应的映射,目的是取消代码的校验错误
      ```
        "baseUrl": ".",
        "paths": {
          "@/*": ["src/*"]
        },
      ```

    - 在vite.config.ts中添加@对应的映射
      ```
        import { defineConfig } from 'vite'
        import vue from '@vitejs/plugin-vue'
        import { resolve } from 'path'

        // https://vitejs.dev/config/
        export default defineConfig({
          plugins: [vue()],
          resolve: {
            alias: {
              '@': resolve(__dirname, "src") //设置别名
            }
          },
          server: {
            host: 'localhost',
            port: 8080,
            open: true,
          },
        })
      ```

## 在vite中使用vue-router
  1. 安装
    - npm install --save vuex@next

  2. 使用
    - 在store文件夹下，创建index.ts，并写入
      ```
        import { createStore } from "vuex";
        import user from '@/store/modules/user'
        export default createStore({
          modules: {
            user
          }
        });
      ```

    - 在store文件夹下新建modules文件夹，并新建user.ts,写入：
      ```
        const state = {
          num: 1
        }

        const mutations = {}

        const actions = {}

        export default {
          namespaced: true,
          state,
          mutations,
          actions,
        }
      ```

    - 在main.ts中引入stroe
      ```
        import { createApp } from 'vue'
        import App from './App.vue'
        import router from '@/router'
        import store from '@/store'

        const app = createApp(App)
        app.use(router).use(store).mount('#app')
      ```

    - 在页面中使用：
      ```
        <template>
          <div>
            {{ data.count }}
          </div>
        </template>

        <script>
          import { reactive } from 'vue';
          import { useStore } from "vuex";
          export default {
            setup () {
              const store = useStore()
              console.log('store',store.state.user.num)
              let data = reactive({
                count: store.state.user.num
              })
              return {
                data
              }
            }
          }
        </script>

        <style scoped></style>
      ```

## 在vite中使用sass、less、stylus
  1. 安装
    - npm install -D sass
  2. 使用
    ```
      <style scoped lang="ts"></style>
    ```

## 在vite中，使用element-plus
  1. 安装
    - npm install element-plus --save

  2. 引入
    ```
      import { createApp } from 'vue'
      import App from './App.vue'
      import router from '@/router'
      import store from '@/store'
      import ElementPlus from 'element-plus'
      import 'element-plus/lib/theme-chalk/index.css';

      const app = createApp(App)
      app.use(router).use(store).use(ElementPlus).mount('#app')
    ```
