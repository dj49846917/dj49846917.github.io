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

# 最新vue3项目vite使用
  1. 安装命令：
    ```
      npm create vue@3  可选择vur-router、pinia、eslint
    ```
  2. 运行时代码强校验
    * 安装命令：
      ```
        npm install --save-dev vite-plugin-eslint
      ```
    * 在.eslintrc.cjs中添加如下代码：(vite-plugin-eslint有报错，但是不影响运行，所以直接注释掉)
      ```
        // @ts-ignore
        import eslintPlugin from 'vite-plugin-eslint'

        export default defineConfig({
        plugins: [
          ...
          eslintPlugin()
        ],
        ...
      })
      ```

## pinia的使用
  1. 在src下，新建store/index.ts,并写入
    ```
      import userStore from "./modules/user";

      export default function useStore() {
          return {
              user: userStore()
          }
      }
    ```
  2. 在src/store下新建modules/user.ts，并写入
    ```
      import { defineStore } from 'pinia'

      const userStore = defineStore('user', {
        state() {
          return {
            count: 0
          }
        },
        actions: {
          increment() {
            this.count ++;
          },
          decrement() {
            this.count --;
          }
        }
      })

      export default userStore
    ```
  3. 检查在main.ts中引入pinia
    ```
      import { createApp } from 'vue'
      import { createPinia } from 'pinia'

      import App from './App.vue'
      const app = createApp(App)
      app.use(createPinia())
      app.mount('#app')
    ```
  4. 使用pinia中的count
    ```
      <script setup lang="ts">
        import useStore from './stores';

        const store = useStore();

        const add = () => {
          store.user.increment()
        }
        const deleteAction = () => {
          store.user.decrement();
        }
      </script>

      <template>
        <div>
          {{ store.user.count }}
          <button @click="add">加</button>
          <button @click="deleteAction">减</button>
        </div>
      </template>
    ```
  5. 使用pinia中的storeToRefs进行解构（不能用toRefs,因为会把store里面的所有属性方法都转换成ref）
    ```
      <script setup lang="ts">
      import useStore from './stores';
      import { storeToRefs } from 'pinia';

      const store = useStore();
      const { count } = storeToRefs(store.user);

      const add = () => {
        store.user.increment()
      }

      const deleteAction = () => {
        store.user.decrement
      }
      </script>

      <template>
        {{ count }}
        <button @click="add">加</button>
        <button @click="deleteAction">减</button>
      </template>
    ```

  6. `$subscribe`相当于watch,当值发生变化时触发
    ```
      <script setup lang="ts">
        import useStore from './stores';
        import { storeToRefs } from 'pinia';

        const store = useStore();
        const { count } = storeToRefs(store.user);

        store.user.$subscribe((mutate, state)=>{
          console.log("mutate", mutate, state);
        })

        const add = () => {
          store.user.increment()
        }

        const deleteAction = () => {
          store.user.decrement
        }
      </script>

      <template>
        {{ count }}
        <button @click="add">加</button>
        <button @click="deleteAction">减</button>
      </template>
    ```

  7. 组合式API写法
    * 修改src/store/modules/user.ts
      ```
        import { ref } from 'vue'
        import { defineStore } from 'pinia'

        const userStore = defineStore('counter', () => {
          const count = ref(0)
          function increment() {
            count.value++
          }

          function decrement() {
            count.value--
          }

          return { count, increment, decrement }
        })

        export default userStore
      ```


# vite的使用
## 安装插件
  * npm init vite@latest
    - ![vite安装](/images/vue/vite安装.jpg)
    - npm install

  * 详细配置文档请看：https://cn.vitejs.dev/

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
            name: 'Home',
            component: () => import('@/views/Home.vue'), //路由懒加载
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
    - 根据提示，需要安装@type/node，执行命令：npm install --save-dev @type/node
    - 在src下新建views文件夹，并新建Home.vue

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

  3. 按需导入
    * 首先你需要安装unplugin-vue-components 和 unplugin-auto-import这两款插件
      ```
        npm install -D unplugin-vue-components unplugin-auto-import
      ```
    * 在vite.config.ts中配置
      ```
        import { defineConfig } from 'vite'
        import AutoImport from 'unplugin-auto-import/vite'
        import Components from 'unplugin-vue-components/vite'
        import { ElementPlusResolver } from 'unplugin-vue-components/resolvers'

        export default defineConfig({
          // ...
          plugins: [
            // ...
            AutoImport({
              resolvers: [ElementPlusResolver()],
            }),
            Components({
              resolvers: [ElementPlusResolver()],
            }),
          ],
        })
      ```

{% note danger %}
  注意: 
    vite在打包（也就是执行npm run build）的时候，element-plus会报错，解决办法是修改package.json中的build打包命令：
    ```
      "build": "vite build",
    ```
{% endnote %}

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

  4. 在代码中使用mock
    ```
      // 初始化数据
      onMounted(()=>{
        axios.get("/api/get").then(res=>{
          console.log("res", res)
        }).catch(err=>{
          console.log("err", err)
        })
      })
    ```

### vite生产环境中使用Mock
  1. 在src下新建mockProdServer.ts(名字无所谓), 把mock文件逐一引入
    ```
      //  mockProdServer.ts
      import { createProdMockServer } from 'vite-plugin-mock/es/createProdMockServer';

      // 逐一导入您的mock.ts文件
      import cartApi from '../mock/cart'
      import categoryApi from '../mock/cateogry'
      import goodsInfoApi from '../mock/goodsInfo'
      import goodsListApi from '../mock/goodsList'
      import homeApi from '../mock/home'
      import loginApi from '../mock/login'
      import miFamilyApi from '../mock/miFamily'

      export function setupProdMockServer() {
        createProdMockServer([...cartApi, ...categoryApi, ...goodsInfoApi, ...goodsListApi, ...homeApi, ...loginApi, ...miFamilyApi]);
      }
    ```
  2. 修改vite.config.ts，在plugins里面添加以下代码
    ```
      plugins: [
        vue(),
        viteMockServe({
          // default
          mockPath: 'mock',
          localEnabled: command === 'serve',
          prodEnabled: true,
          //  这样可以控制关闭mock的时候不让mock打包到最终代码内
          injectCode: `
            import { setupProdMockServer } from './mockProdServer';
            setupProdMockServer();
          `,
        }),
      ],
    ```

## 在vite中使用多环境（非常重要！！）
  > 默认情况下，vite只支持`.env.development`和`.env.production`这两个环境文件(创建在根路径下)
  > 执行npm run dev时，`默认加载.env.development文件中的内容`，执行npm run build时，`默认加载.env.production文件中的内容`
  > 如果想配置其他环境，例如：配置sit环境，需要创建.env.sit文件后，在package.json中指定环境`--mode sit`
  > 在.env.xxx文件中的变量要想在views页面中使用，必须加上`VITE_`, 必须是大写，例如`VITE_NODE_NAME=dev`
  > .env文件，所有环境都有效

  * 具体代码如下：
    1. 在根路径下新建.env.development文件
      ```
        # 测试变量
        VITE_PROXY_URL=https://www.aiqiyi.com
      ```
    2. 在根路径下新建.env.production文件
      ```
        # 测试变量
        VITE_PROXY_URL=https://www.baidu.com
      ```
    3. 在根路径下新建.env.sit文件
      ```
        # 测试变量
        VITE_PROXY_URL=https://www.youku.com
      ```
    4. 修改package.json中的启动命令
      ```
        "scripts": {
          "dev": "vite",                      // 加载.env.development
          "dev:sit": "vite --mode sit",       // 加载.env.sit
          "build": "vite build",              // 加载.env.production
          "serve": "vite preview"
        },
      ```
    5. 在页面中使用环境变量：`import.meta.env.VITE_PROXY_URL`
      * 由于新添的`VITE_PROXY_URL`，在输入的时候没有代码提示，如果要配置代码提示，需要在src/vite-env.d.ts中设置以下代码：
        ```
          /// <reference types="vite/client" />

          // 加上这个接口就有代码提示了
          interface ImportMetaEnv {
            VITE_APP_PORT: number,
            VITE_PROXY_URL: string,
          }
        ```

    6. 在vite.config.ts中，使用环境变量
      ```
        import { loadEnv } from 'vite';

        export default ({ command, mode }: ConfigEnv): UserConfigExport => {
          // 获取根路径
          const root = process.cwd();
          // 获取环境变量文件的内容
          const envInfo = loadEnv(mode, root);

          server: {
            host: 'localhost',
            port: Number(envInfo.VITE_APP_PORT), // 使用
            open: true,
          },
        }
      ```

## 在vite中使用接口代理
  * 以`http://api.vikingship.xyz/api/columns?currentPage=1&pageSize=5`这个接口为例
    1. 修改vite.config.ts，添加proxy
      ```
        server: {
          host: 'localhost',
          port: Number(envInfo.VITE_APP_PORT),
          open: true,
          proxy: {
            '/api': {
              target: 'http://api.vikingship.xyz/api',
              changeOrigin: true,
              rewrite: path => path.replace(/^\/api/, '')
            }
          }
        },
      ```
    2. 页面中使用：
      ```
        import axios from 'axios';

        onMounted(()=>{
          axios.get("/api/columns?currentPage=1&pageSize=5").then(res=>{
            console.log("res", res)
          }).catch(err=>{
            console.log("err", err)
          })
        })
      ```

## 在vite中使用vant ui
  * 安装
    - 执行命令：npm i vant@next -S
  * 使用
    - 在main.ts中引入
      ```
        import { createApp } from 'vue'
        import App from './App.vue'
        import router from '@/router'
        import store from '@/store'
        import Vant from 'vant';
        import 'vant/lib/index.css';

        const app = createApp(App)
        app.use(router).use(store).use(Vant).mount('#app')
      ```

{% note danger %}
  注意: 
    出现以下报错：![报错](/images/vue/报错.png)
  解决办法：在文件根目录下面的，vite-env.d.ts文件中添加如下代码，即可成功解决该问题。
    ```
      declare module "*.vue" {
        import type { DefineComponent } from "vue";
        const vueComponent: DefineComponent<{}, {}, any>;
        export default vueComponent;
      }

    ```
{% endnote %}
  
# vant ui的rem适配
  1. 安装
    * 执行命令：
      ```
        npm install --save-dev postcss postcss-pxtorem amfe-flexible
      ```
  2. 在main.ts中引入
    ```
      import { createApp } from 'vue'
      import App from './App.vue'
      import router from '@/router'
      import store from '@/store'
      import Vant from 'vant';
      import 'vant/lib/index.css';
      import '@/assets/css/global.scss';
      import "amfe-flexible/index.js"

      const app = createApp(App)
      app.use(router).use(store).use(Vant).mount('#app')
    ```
  3. 在根路径下新建postcss.config.js
    ```
      module.exports = {
        plugins: {
          'postcss-pxtorem': {
            rootValue: 37.5,
            propList: ['*'],
          },
        },
      };
    ```

# rem适配pc端方案
1. 安装 postcss-pxtorem
  ```
    npm install postcss-pxtorem amfe-flexible -D
  ```
2. 配置 vite.config.ts
  ```
    import { defineConfig } from 'vite'
    import postcssPxtoRem from 'postcss-pxtorem'

    export default defineConfig({
      css: {
        postcss: {
          plugins: [
            postcssPxtoRem({
              rootValue: 144, // 按照自己的设计稿修改 1440/10
              unitPrecision: 5, // 保留到5位小数
              selectorBlackList: ['ignore', 'tab-bar', 'tab-bar-item'],  // 忽略转换正则匹配项
              propList: ['*'],
              replace: true,
              mediaQuery: false,
              minPixelValue: 0
            })  
          ]
        }
      }
    })
  ```
3. 再main.ts文件中引入amfe-flexible
  ```
    import 'amfe-flexible'
  ```
4. 在样式中直接使用 px 作为单位即可
5. 如果是行内样式或者js赋值的px这个插件不会转行rem，这个时候需要在赋值的时候/144
  ```
    <div :style="{width: 265 / 144 + 'rem', height: 180 / 144 + 'rem'}"></div>
  ```

# 使用vite构建vue项目打包发布gitee pages或者github pages
  1. 由于我使用了vite的多环境，在.env.development和.env.production中分别添加变量VITE_APP_BASE，设置项目基础路径，也就是你要部署github/gitee的仓库名称，我这里的仓库名称是vue3-shopping
    * .env.development
      ```
        # 项目基础路径
        VITE_APP_BASE=/
      ```
    * .env.production
      ```
        # 项目基础路径
        VITE_APP_BASE=/vue3-shopping/
      ```
  2. 修改vite.config.ts，设置base属性
    ```
      import { ConfigEnv, defineConfig, loadEnv, UserConfigExport } from 'vite'
      import vue from '@vitejs/plugin-vue'
      import { resolve } from 'path'

      // https://vitejs.dev/config/
      export default ({ command, mode }: ConfigEnv): UserConfigExport => {
        // 获取根路径
        const root = process.cwd();
        // 获取环境变量文件的内容
        const envInfo = loadEnv(mode, root);
        return {
          plugins: [vue()],
          base: envInfo.VITE_APP_BASE, // 核心：读取VITE_APP_BASE
          resolve: {
            alias: {
              '@': resolve(__dirname, "src") //设置别名
            }
          },
          server: {
            host: '0.0.0.0',
            port: Number(envInfo.VITE_APP_PORT), // 使用
            open: false,
          },
        };
      };
    ```
  3. 修改src/router/index.ts，生产环境中，使用hash路由，这样服务端就不用配置任何东西
    ```
      import { routeItemType } from '@/types/global';
      import { createRouter, createWebHashHistory, createWebHistory } from 'vue-router';
      export const routes = [
        {
          path: '/',
          name: 'Layout',
          redirect: '/index',
          component: () => import('@/layout/index.vue'),
          children: [
            {
              path: '/index',
              name: 'Home',
              component: () => import('@/views/Home/index.vue'),
              meta: { title: '首页' }
            },
          ]
        }
      ]
      const router = createRouter({
        history: createWebHashHistory(import.meta.env.VITE_APP_BASE), // 部署gitee page最好设置hash路由，传入项目仓库基础路径
        routes,
      });
      export default router;
    ```
  4. 打开gitee pages，进行部署
    * ![部署](/images/vue/部署2.jpg)
    * ![部署](/images/vue/部署.jpg)

{% note danger %}
  注意: 
    如果要使用history路由模式：
      ```
        const router = createRouter({
          history: createWebHistory(import.meta.env.VITE_APP_BASE), // 部署gitee page最好设置hash路由，传入项目仓库基础路径
          routes,
        });
      ```
    在nginx上，要添加：
      ```
        try_files $uri $uri.html /index.html $uri/ =404;
      ```
{% endnote %}

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

# vue3修改框架的默认样式
  * 使用:deep()
    ```
      .van-cell--clickable {
        border-bottom: 1px solid rgba(0, 0, 0, 0.15);
        height: 40px;
        box-sizing: border-box;
        :deep(.van-cell__value span) {
          font-size: 12px;
          color: #000;
        }
      }
    ```

# vue3给computed传值，动态修改图片路径
  ```
    <li v-for="(item, index) in dataSource.product_info && dataSource.product_info.sell_point_desc" :key="index">
      <van-image lazy-load :src="decImg(index + 1)" />
      <span>{{ item }}</span>
    </li>

    // 描述里面的图片
    const decImg = computed(()=>{
      return (str: number)=>{
        return new URL(`../../assets/images/img${str}.png`, import.meta.url).href
      }
    })
  ```

# vue3中使用js-cookie
  - 安装： npm install --save js-cookie
  - 使用:
    - Cookies.set('name', 'value');
    - Cookies.set('name', 'value', { expires: 7 });
    - Cookies.get('name');
    - Cookies.get();
    - Cookies.remove('name');

# vue3中使用computed
  ```
    // 注册资本
    let RegisteredCapital: ComputedRef<string> = computed(() => {
      return `${Number(userInfo.DealerInfo?.RegisteredCapital) / 10000}`;
    });

    return {
      RegisteredCapital,
    };
  ```

# vue3中监听路由值的变化
  - 使用组件内的onBeforeRouteUpdate
    ```
      setup() {
        const route = useRoute();
        const isLogin = ref(false)
        if(route.path === "/login" || route.path === "/register") {
          isLogin.value = true
        } else {
          isLogin.value = false
        }

        // 监听路由值得变化
        onBeforeRouteUpdate((to): void => {
          console.log("to.path", to.path)
          if(to.path === "/login" || to.path === "/register") {
            isLogin.value = true
          } else {
            isLogin.value = false
          }
        });
        return {
          isLogin,
        };
      },
    ```

# 在vue3中使用404页面
  ```
    { // 设置404页面
      path: '/:catchAll(.*)',
      name: '404',
      component: () => import('@/views/ErrorPage.vue')
    },
  ```

# 在vue3中绑定样式
  ```
    <div class="nav" :style="personalNav">

    props: {
      navHeight: { type: Number, default: 42 }
    },
    setup(props) {
      const personalNav = computed(()=>{
        if(props.navHeight) {
          return {
            'height': props.navHeight,
            'background-color': 'red'
          }
        }
      })
      return {
        personalNav
      };
    },
  ```

# element-plus递归渲染动态菜单
  1. 准备路由数据(hidden表示不渲染出来)
    ```
      export const constantRoutes = [
        {
          path: '/',
          name: 'home',
          component: Layout,
          redirect: '/index',
          hidden: true,
          children: [{
            path: 'index',
            name: 'Home',
            component: () => import('@/views/home.vue'),
            meta: { title: '首页', icon: 'el-icon-s-home' }
          }]
        },
        {
          path: '/system',
          component: Layout,
          redirect: '/system/user',
          meta: { title: '系统管理', icon: 'el-icon-reading' },
          children: [
            {
              path: '/system/user',
              name: 'UserManage',
              component: () => import('@/views/UserManage/index.vue'),
              meta: { title: '用户管理', icon: 'el-icon-goods' }
            },
            {
              path: '/system/role',
              name: 'RoleManage',
              component: () => import('@/views/RoleManage/index.vue'),
              meta: { title: '角色管理', icon: 'el-icon-goods' }
            },
            {
              path: '/system/menu',
              name: 'MenuManage',
              component: () => import('@/views/MenuManage/index.vue'),
              meta: { title: '菜单管理', icon: 'el-icon-help' }
            },
            {
              path: '/system/dept',
              name: 'DepartmentManage',
              component: () => import('@/views/DepartmentManage/index.vue'),
              meta: { title: '部门管理', icon: 'el-icon-picture-outline-round' }
            },
            {
              path: '/system/post',
              name: 'PostManage',
              component: () => import('@/views/PostManage/index.vue'),
              meta: { title: '岗位管理', icon: 'el-icon-video-camera' }
            },
            {
              path: '/system/dict',
              name: 'DictionaryManage',
              component: () => import('@/views/DictionaryManage/index.vue'),
              meta: { title: '字典管理', icon: 'el-icon-message-solid' }
            },
            {
              path: '/system/config',
              name: 'ParamSet',
              component: () => import('@/views/ParamSet/index.vue'),
              meta: { title: '参数设置', icon: 'el-icon-s-platform' }
            },
            {
              path: '/system/notice',
              name: 'NoticeMessage',
              component: () => import('@/views/NoticeMessage/index.vue'),
              meta: { title: '通知公告', icon: 'el-icon-s-promotion' }
            },
            {
              path: '/system/log',
              name: 'LogManage',
              meta: { title: '日志管理', icon: 'el-icon-scissors' },
              component: () => import('@/views/LogManage/ActionLog.vue'),
              children: [
                {
                  path: '/system/log/operlog',
                  name: 'ActionLog',
                  component: () => import('@/views/LogManage/ActionLog.vue'),
                  meta: { title: '操作日志', icon: 'el-icon-s-comment' }
                },
                {
                  path: '/system/log/logininfor',
                  name: 'LoginLog',
                  component: () => import('@/views/LogManage/LoginLog.vue'),
                  meta: { title: '登录日志', icon: 'el-icon-s-data' }
                }
              ]
            },
          ]
        },
        {
          path: '/:catchAll(.*)',
          component: () => import('@/views/404.vue'),
          hidden: true
        },
      ]
    ```
  
  2. 新建menu.vue
    ```
      <template>
        <el-menu
          :uniqueOpened="true"
          class="menu"
          background-color="#545c64"
          text-color="#fff"
          active-text-color="#ffd04b"
        >
          <menu-item
            v-for="route in menuData.menuArr"
            :key="route.path"
            :item="route"
            :base-path="route.path"
          />
        </el-menu>
      </template>

      <script lang="ts">
      import { defineComponent, onMounted, reactive } from "vue";
      import { constantRoutes } from "@/router/index";
      import menuItem from '@/layout/components/menuItem.vue';

      export default defineComponent({
        components: {
          menuItem
        },
        setup() {
          let menuData = reactive<any>({
            menuArr: [],
          });

          onMounted(() => {
            menuData.menuArr = constantRoutes;
          });

          return {
            menuData,
          };
        },
      });
      </script>

      <style scoped lang="scss">
      .menu {
        height: 100vh;
        background-color: rgb(48, 65, 86);
      }
      </style>
    ```
  
  3. 子组件menuItem.vue
    ```
      <template>
        <div v-if="!item.hidden">
          <el-submenu v-if="item.children && item.children.length > 0" :index="item.path">
            <template #title>
              <i :class="item.meta.icon"></i>
              <span>{{ item.meta.title }}</span>
            </template>
            <menu-item 
              v-for="it in item.children"
              :key="it.path"
              :item="it"
              :basePath="it.path"
            />
          </el-submenu>
          <el-menu-item v-else :index="item.path" @click="getPath(item.path)">
            <i :class="item.meta.icon"></i>
            <span>{{ item.meta.title }}</span>
          </el-menu-item>
        </div>
      </template>

      <script lang="ts">
      import { defineComponent } from "vue";
      import { useRouter } from "vue-router";

      export default defineComponent({
        props: ["item", "basePath"],
        setup() {
          const router = useRouter();

          function getPath(path: string) {
            console.log("path", path)
            router.push(path)
          }
          return {
            getPath
          };
        },
      });
      </script>

      <style scoped>
      </style>
    ```

# vue3移动端开发
## vue3中使用vant ui
  1. 安装vant ui
    * 官网：https://vant-contrib.gitee.io/vant/v3/#/zh-CN
    * 执行命令：npm i vant@next -S
  2. 在main.ts中引用
    ```
      import Vant from 'vant';
      import 'vant/lib/index.css';

      const app = createApp(App)
      app.use(Vant)
    ```

## vue3rem转换
  1. 安装postcss-pxtorem
    * 执行命令：npm install postcss postcss-pxtorem --save-dev
  2. 在根路径下新建postcss.config.js
    ```
      module.exports = {
        plugins: {
          'postcss-pxtorem': {
            rootValue: 16,
            propList: ['*'],
          },
        },
      };
    ```
  3. 注意：如果设计稿的尺寸不是 375，而是 750 或其他大小，可以将 rootValue 配置调整为:
    ```
      module.exports = {
        plugins: {
          // postcss-pxtorem 插件的版本需要 >= 5.0.0
          'postcss-pxtorem': {
            rootValue({ file }) {
              return file.indexOf('vant') !== -1 ? 16 : 8;
            },
            propList: ['*'],
          },
        },
      };
    ```

## 底部安全区适配
  * iPhone X 等机型底部存在底部指示条，指示条的操作区域与页面底部存在重合，容易导致用户误操作，因此我们需要针对这些机型进行安全区适配。Vant 中部分组件提供了 safe-area-inset-top 或 safe-area-inset-bottom 属性，设置该属性后，即可在对应的机型上开启适配，如下示例：
    ```
      <!-- 在 head 标签中添加 meta 标签，并设置 viewport-fit=cover 值 -->
      <meta
        name="viewport"
        content="width=device-width, initial-scale=1.0, maximum-scale=1.0, minimum-scale=1.0, viewport-fit=cover"
      />

      <!-- 开启顶部安全区适配 -->
      <van-nav-bar safe-area-inset-top />

      <!-- 开启底部安全区适配 -->
      <van-number-keyboard safe-area-inset-bottom />
# typescript表示未知对象的任意类型
  ```
    interface tabInfo {
      title: string,
      [key: string]: string;
    }
  ```

# typescript解决以下问题：
  ```
    <input
      type="text"
      placeholder="邮箱/手机号码/小米ID"
      v-model="userInfo.username"
      @input="changeValue('username', userInfo.username)"
    />

    const userInfo = reactive({
      username: "",
      password: "",
      showPassword: false, // 是否展示password
      showUserNameError: false, // 是否开启错误校验
    });

    const changeValue = (key: string, val: string): void => {
      console.log(key, userInfo[key]); 这里报错
      userInfo[key] = val;
    };
  ```
  * ![问题](/images/vue/问题.jpg)
  * 解决办法：在tsconfig.json中配置suppressImplicitAnyIndexErrors: true
    ```
      {
        "compilerOptions": {
          "suppressImplicitAnyIndexErrors": true,
          ...
        },
        ...
      }
    ```