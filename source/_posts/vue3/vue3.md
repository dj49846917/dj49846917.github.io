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

## 在vite中使用vuex
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

## 在vite中，使用mockjs
  1. 安装：
    - npm i  mockjs -S
    - npm i vite-plugin-mock -D

  2. 在vite.config.ts中写入：
    ```
      import { UserConfigExport, ConfigEnv } from 'vite';
      import { viteMockServe } from 'vite-plugin-mock';
      import vue from '@vitejs/plugin-vue';

      export default ({ command }: ConfigEnv): UserConfigExport => {
        return {
          plugins: [
            vue(),
            viteMockServe({
              // default
              mockPath: 'mock',
              localEnabled: command === 'serve',
            }),
          ],
        };
      };
    ```

  3. 在跟路径下新建mock文件夹，新建auction.ts并写入：
    ```
      import { MockMethod } from 'vite-plugin-mock';

      // 获取列表数据
      const getList = {
        url: '/api/get',
        method: 'get',
        response: ({ query }) => {
          return {
            code: 0,
            data: {
              name: 'vben',
            },
          };
        },
      };

      export default [
        getList,
      ] as MockMethod[];
    ```

  4. 新建mockProdServer.ts，并引入auction.ts
    ```
      import { createProdMockServer } from 'vite-plugin-mock/es/createProdMockServer';
      import auction from './auction';

      export function setupProdMockServer() {
        createProdMockServer([...auction]);
      }
    ```

# vsCode没有代码提示的解决办法
  - ![vscode没有代码提示](/images/hexo/vscode没有代码提示.jpg)

# 在vue3中使用全局变量
  - 在main.ts中定义全局变量$axios
    ```
      import axios from 'axios'

      const app = createApp(App)
      app.config.globalProperties.$axios = axios
    ```

  - 在页面中使用
    ```
      import { onMounted, getCurrentInstance } from 'vue';

      onMounted(()=>{
        ctx.$axios.get("/Auction/GetListJson", {index: 1}).then(res=>{
          console.log("res", res)
        }).catch(err=>{
          console.log('err', err)
        })
      })
    ```

# 封装axios
  1. 在src下，新建utils/request.ts，并写入:
    ```
      // 默认利用axios的cancelToken进行防重复提交。
      // 如需允许多个提交同时发出。则需要在请求配置config中增加 neverCancel 属性，并设置为true

      import axios from 'axios';
      // import store from '../store/index';
      // import { getSessionId } from '@/utils/auth';

      /* 防止重复提交，利用axios的cancelToken */
      let pending: any[] = []; // 声明一个数组用于存储每个ajax请求的取消函数和ajax标识
      const CancelToken: any = axios.CancelToken;

      const removePending: any = (config: any, f: any) => {
        // 获取请求的url
        const flagUrl = config.url;
        // 判断该请求是否在请求队列中
        if (pending.indexOf(flagUrl) !== -1) {
          // 如果在请求中，并存在f,f即axios提供的取消函数
          if (f) {
            f('取消重复请求'); // 执行取消操作
          } else {
            pending.splice(pending.indexOf(flagUrl), 1); // 把这条记录从数组中移除
          }
        } else {
          // 如果不存在在请求队列中，加入队列
          if (f) {
            pending.push(flagUrl);
          }
        }
      };

      /* 创建axios实例 */
      const service = axios.create({
        timeout: 5000, // 请求超时时间
      });

      /* request拦截器 */
      service.interceptors.request.use((config: any) => {
        // neverCancel 配置项，允许多个请求
        if (!config.neverCancel) {
          // 生成cancelToken
          config.cancelToken = new CancelToken((c: any) => {
            removePending(config, c);
          });
        }
        // 在这里可以统一修改请求头，例如 加入 用户 token 等操作
        //   if (store.getters.sessionId) {
        //     config.headers['X-SessionId'] = getSessionId(); // 让每个请求携带token--['X-Token']为自定义key
        //   }
        return config;
      }, (error: any) => {
        Promise.reject(error);
      });

      /* respone拦截器 */
      service.interceptors.response.use(
        (response: any) => {
          // 移除队列中的该请求，注意这时候没有传第二个参数f
          removePending(response.config);
          // 获取返回数据，并处理。按自己业务需求修改。下面只是个demo
          const res = response.data;
          console.log("res", res)
          if (res.code !== 200) {
            if (res.code === 401) {
              // 返回对应的页面
              // if (location.hash === '#/') {
              //   return res;
              // } else {
              //   location.href = '/#/';
              // }
            }
            return Promise.reject('error');
          } else {
            return response;
          }
        },
        (error: any) => {
          // 异常处理
          console.log(error)
          pending = [];
          if (error.message === '取消重复请求') {
            return Promise.reject(error);
          }
          return Promise.reject(error);
        },
      );

      export default service;
    ```

  2. 在src下，新建service文件夹，新建acution.ts，并写入:
    ```
      import request from '@/utils/request'

      // get
      export function getList(params?: any) {
        return request({
          url: '/Auction/GetListJson',
        });
      }

      // post
      export function loginService(params?: any) {
        return request({
          url: "/Login/DoLogin",
          method: "post",
          data: params
        });
      }
    ```

# 在vue3中使用element-plus的form组件并校验
  - 详细代码如下：
    ```
      <template>
      <div class="box">
        <el-form :model="ruleForm" :rules="rules" ref="ruleFormsss" label-width="100px">
          <el-form-item label="用户" prop="username">
            <el-input v-model="ruleForm.username"></el-input>
          </el-form-item>
          <el-form-item label="密码" prop="password">
            <el-input v-model="ruleForm.password"></el-input>
          </el-form-item>
          <el-form-item label="活动区域" prop="region">
          <el-select v-model="ruleForm.region" placeholder="请选择活动区域">
            <el-option label="区域一" value="shanghai"></el-option>
            <el-option label="区域二" value="beijing"></el-option>
          </el-select>
        </el-form-item>
          <el-form-item>
            <el-button type="primary" size="medium" @click="submitForm">登 录</el-button>
          </el-form-item>
        </el-form>
      </div>
      </template>
      
      <script>
      import {
        reactive,
        ref,
        unref
      } from 'vue'
      export default {
        setup(props) {
          const ruleFormsss = ref(null);
          // 定义变量
          const ruleForm = reactive({
            username: '',
            password: '',
            region: ''
          })
      
          const rules = {
            username: [
              { required: true, message: '请输入用户名', trigger: 'blur' },
            ],
            password: [
              { required: true, message: '请输入密码', trigger: 'blur' },
            ],
            region: [
              { required: true, message: '请选择活动区域', trigger: 'change' }
            ]
          }
      
          const submitForm = async () => {
            const form = unref(ruleFormsss);
            if (!form) return
            try {
              await form.validate()
              const { username, password, region } = ruleForm
              console.log(username, password, region)
            } catch (error) {
            } 
          }
          return {
            ruleForm,
            rules,
            submitForm,
            ruleFormsss
          }
        }
      }
      </script>
      
      <style>
      </style>
    ```