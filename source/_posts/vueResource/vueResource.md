---
title: vueResource
date: 2021-10-14 10:27:42
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

# vue源码学习

## vue-router部分(3.x部分对应vue2)
### 思考
1. vue-router是一个插件？
2. 内部做了什么？
    * 实现并声明了两个组件router-view、router-link
    * 实现install方法，及this.$router.push()
3. 为什么要将router添加到main.js的配置项中
    * 因为要在vue插件vue-router的install方法中去使用，而VueRouter.install方法是谁在调用呢？是Vue.use在调用，也就是说这个router插件执行的时刻是非常早的，在执行Vue.use(VueRouter)的时候，会调用install,所以他执行的时刻会比创建实例newVueRouter要早，所以在Vue.install方法里面，没有办法拿到vue实例this,所以我们才要把这个router写到main.js的new Vue里面
4. router-view是如何实现的
    * dom实现：因为在路由文件里定义了path和component，可以通过js的onHashChange事件，监听当前页的hash地址值，或者history的变化popstate事件，当事件发生变化的时候，我就可以拿到当前的地址，当我拿到这个地址之后，就可以根据路由文件path与这个地址匹配，拿到对应的component,然后就可以把这个component变成真实的dom,再追加到router-view里面去，可以把之前的router-view清空掉，然后把这些内容创建完之后再追加进去
    * 利用vue响应式：及创建一个响应式的数据，这个值可能是当前的地址，可能是current，如果将来这个地址发生变化，router-view里面的相关组件可以重新渲染（核心思路）

### 编写router-view和router-link
1. 安装命令：
    ```
        npm install -g @vue/cli, 选择vue2版本
    ```
2. 在src下新建vue-router文件夹，并新建index.js
3. 修改router/index.js的引入
    ```
        import Vue from 'vue'
        import VueRouter from '@/vue-router' // 修改部分
        import Home from '../views/Home.vue'

        // 1. VueRouter是一个插件？
        // 2. 内部做了什么？
        //  1) 实现并声明两个组件router-view、router-link
        //  2) install: this.$router.push()
        Vue.use(VueRouter)

        const routes = [
            {
                path: '/',
                name: 'Home',
                component: Home
            },
            {
                path: '/about',
                name: 'About',
                component: () => import('../views/About.vue')
            }
        ]


        const router = new VueRouter({
            routes
        })
        export default router
    ```

4. 编写vue-router/index.js
    ```
        // 1. 插件
        // 2. 两个组件

        // vue组件：
        // function或者object
        // 要求必须有一个install, 将来会被Vue.use调用

        let Vue; // 保存Vue构造函数，插件中要使用

        class VueRouter {
            constructor(options) {
                this.$options = options
                const initial = window.location.hash.split("#")[1] || '/'
                // 把current作为响应式数据
                // 将来发生变化，router-view的render函数能够再次执行
                Vue.util.defineReactive(this, 'current', initial)
                // 监听hash变化
                window.addEventListener("hashchange", ()=>{
                    this.current = window.location.hash.slice(1)
                })
            }
        }

        // 默认会调用install方法
        // 蚕食是Vue.use调用时传入的
        VueRouter.install = (_Vue) => {
            Vue = _Vue

            // 1. 挂载$router属性
            // this.$router.push
            // 全局混入目的：延迟下面的逻辑到router创建完毕并附加到选项上时才执行
            Vue.mixin({
                beforeCreate() {
                    // 此钩子在每个组件创建实例时都会调用
                    // 根实例才有该属性
                    if(this.$options.router) {
                        Vue.prototype.$router = this.$options.router
                    }
                }
            })
            // 注册实现两个组件router-view、router-link
            Vue.component('router-link', {
                props: {
                    to: {
                        type: String,
                        required: true,
                    }
                },
                render(h) {
                    return h('a', { attrs: { href: '#' + this.to } } ,this.$slots.default)
                }
            })

            Vue.component('router-view', {
                render(h) {
                    console.log("this.$router.current", this.$router.current)
                    // 获取当前路由对应的组件
                    let component;
                    const route = this.$router.$options.routes.find((route)=>{
                        return route.path === this.$router.current
                    })
                    if(route) {
                        component = route.component
                    }
                    return h(component)
                }
            })
        }

        export default VueRouter
    ```

## vuex源码部分(对应vue2.x)
### 思考
1. mutations里面的函数参数state是哪里来的
    ```
        mutations: {
            add(state) {
            state.count ++
            }
        },
    ```
2. actions里面的函数中的第一个参数是哪里来的
    ```
        actions: {
            addCount({ commit }) {
            // 参数是什么，哪儿来的？
            setTimeout(()=>{
                commit('add')
            }, 1000)
            }
        },
    ```

### 编写代码
1. 安装vuex
2. 在src下新建store/index.js
    ```
        import Vue from 'vue'
        import Vuex from '@/vue-store'

        // this.$store
        Vue.use(Vuex)

        export default new Vuex.Store({
            state: {
                count: 0
            },
            mutations: {
                add(state) {
                console.log("111")
                // state从哪来
                state.count ++
                }
            },
            actions: {
                addCount({ commit }) {
                // 参数是什么，哪儿来的？
                setTimeout(()=>{
                    commit('add')
                }, 1000)
                }
            },
            modules: {
            }
        })
    ```
3. 在main.js中引入store
    ```
        import Vue from 'vue'
        import App from './App.vue'
        import router from './router'
        import store from './store'

        Vue.config.productionTip = false

        new Vue({
            // 添加到配置项中，为什么？
            router,
            store,
            render: h => h(App)
        }).$mount('#app')
    ```
4. 在src下，新建vue-store/index.js
    ```
        // 1. 插件：挂载$store
        // 2. 实现Store

        let Vue

        class Store {
            constructor(options) {
                // data响应式处理
                // this.$store.state.xxx
                console.log("options", options)
                // 这里可以把这个state隐藏起来，防止被用户修改
                this.state = new Vue({
                    data: options.state
                })
                // this._vm = new Vue({
                //     data: {
                //         $$state: options.state
                //     }
                // })
                this._mutations = options.mutations
                this._actions = options.actions
                // this.commit = this.commit.bind(this)
            }

            // 隐藏后的获取方式
            // get state() {
            //     return this._vm._data.$$state
            // }

            commit = (type, payload) => {
                const entry = this._mutations[type]
                if(!entry) {
                    console.log('unknown mutation type')
                }
                entry(this.state, payload)
            }
            // 为什么要使用箭头函数，因为this的指向可能不同
            dispatch = (type, payload) => {
                const entry = this._actions[type]
                if(!entry) {
                    console.log('unknown action type')
                }
                entry(this, payload)
            }
        }

        function install(_Vue) {
            Vue = _Vue
            Vue.mixin({
                beforeCreate() {
                    if(this.$options.store) {
                        Vue.prototype.$store = this.$options.store
                    }
                }
            })
        }

        export default { Store, install }
    ```

## vuex(4.x部分)
1. 在src下新建vuex/index.js
    ```
        import { inject, reactive } from 'vue';

        let storeKey = "store";  // store的作用域，可以是多个

        // 封装一个遍历的公共函数
        function getKeyAndValueByObject(obj, cb) {
        Object.keys(obj).forEach(item=>{
            cb(item, obj[item])
        })
        }

        class Store {
        constructor(options) {
            // state响应式
            this.vm = reactive(options.state)
            // getters { 属性：值 }
            let getters = options.getters
            this.getters = {}

            getKeyAndValueByObject(getters, (key, val)=>{
            Object.defineProperty(this.getters, key, {
                get: () => {
                return val(this.state)
                }
            })
            })

            // actions mutitions
            let mutations = options.mutations
            console.log("mutations", mutations)
            this.mutations = {}
            getKeyAndValueByObject(mutations, (key, val)=>{
            this.mutations[key] = (data) => { // 这步是为了解决this指向问题
                val(this.state, data)
            }
            })

            let actions = options.actions
            this.actions = {}
            getKeyAndValueByObject(actions, (key, val)=>{
            this.actions[key] = (data) => { // 这步是为了解决this指向问题
                val(this, data)
            }
            })
        }

        commit = (key, data) => {
            console.log("key", key, data)
            this.mutations[key](data)
        }

        dispatch = (key, data) => {
            this.actions[key](data)
        }

        get state() {
            return this.vm
        }

        install(app, key) { // 相当于vue2的实例
            console.log(app)
            // vue2让每个组件实例都有一个$store
            app.config.globalProperties.$store = this
            // 全局提供数据 provide(名称：值) inject(名称)
            app.provide(key || storeKey, this)
        }
        }

        export function createStore(options) { // 创建一个store
        return new Store(options)
        }

        export function useStore(key) { // 在你使用的组件中得到一个store
        return inject(key || storeKey)
        }

        /**
        * 思考问题：
        * 1、createStore是怎么实现的，做了哪些事情
        *    答：其实就是new一个store实例，实现传入的state,getters,mutations,actions等方法
        * 2、useStore是怎么实现的，做了什么事情
        *    答：provide加inject实现的，通过根组件的install方法，把store实例用provide注册到根实例中，在子组件通过定义useStore函数中用inject接收这个store实例。作用就是在每个子组件中拿到store实例
        * 3、如何在composition api中使用vuex
        *    答：主要是获取Store实例，通过vuex提供的useStore方法
        * 4、install方法的参数有2个，第一个是app实例，也就是vue实例，第二个是这个插件的名称
        * 
        */
    ```

2. 在store/index.js中，引入
    ```
        import { createStore } from '@/vuex'

        export default createStore({
            state: {
                age: 10
            },
            getters: { // 相当于计算属性
                changeAge(state) {
                return state.age + 5
                }
            },
            mutations: {
                addCount(state, data) {
                state.age += data
                }
            },
            actions: {
                asyncAdd({ commit }, payload) {
                commit("addCount", payload)
                }
            },
            modules: {
            }
        })
    ```

3. 使用
    ```
        <template>
            <div class="home">
                <img alt="Vue logo" src="../assets/logo.png">
                <button @click="add">点击</button>
                <button @click="addAsync">异步点击</button>
                <div>年龄：{{ $store.state.age }}</div>
                <div>哥哥年龄：{{ $store.getters.changeAge }}</div>
            </div>
        </template>

        <script>
        import { useStore } from '@/vuex'
        export default {
            name: 'Home',
            setup() {
                const store = useStore()

                function add() {
                    store.commit("addCount", 30)
                }

                function addAsync() {
                    store.dispatch('asyncAdd', 10)
                }
                return {
                    store,
                    add,
                    addAsync
                }
            }
        }
        </script>
    ```

4. 源码请看：[源码](/images/vueResource/vue-router简易版源码.zip)