---
title: react
date: 2022-01-06 15:17:14
tags: 
  - react
type: hexo                                                                         # 标签、分类和友情链接三個页面需要配置(必填)
description: 用于构建用户界面的 JavaScript 库                                        # 描述
keywords: react                                                                    # 关键词，便于搜索
top_img: /images/react/logo.jpg             # 文章的顶部图片
aside: true                                                                         # 展示文章侧边栏(默认为true)
categories: 
  - 教程
  - react
cover: /images/react/logo.jpg                 # 文章的缩略图（用在首页）
---

# 环境搭建
## 初始化项目
  1. 执行命令
    ```
      npx create-react-app react-demo-ts --template typescript
      cd react-demo-ts
      npm start
    ```

  2. 优化项目结构，优化后的项目结构如下：![初始化](/images/react/初始化ts.jpg)

## 安装react-router v6
  1. 执行命令：npm install --save react-router-dom@6
  2. 在src下新建router/index.js
    ```
      import React from 'react';
      import { Routes, Route } from 'react-router-dom';
      import Layout from '../layout';
      import Category from '../views/Category';
      import Home from '../views/Home';
      import Login from '../views/Login';

      function RouterConfig() {
        return (
          <Routes>
            <Route path="/" element={<Layout />}>
              <Route index element={<Home />} />
              <Route path="/category" element={<Category />} />
            </Route>
            <Route path="/login" element={<Login />} />
          </Routes>
        )
      }

      export default RouterConfig
    ```
  3. 在index.js中引入router
    ```
      import React from 'react';
      import ReactDOM from 'react-dom';
      import { BrowserRouter as Router } from 'react-router-dom';
      import RouterConfig from './router/index'

      ReactDOM.render(
        <Router>
          <RouterConfig />
        </Router>,
        document.getElementById('root')
      );
    ```
  4. 使用：在浏览器输入localhost:3000/index即可看到效果

{% note danger %}
> 重点：使用useRoutes管理路由
  1. 修改src/router/index.tsx
    ```
      import { createBrowserRouter, Navigate } from "react-router-dom";
      import Home from "../view/Home.tsx";
      import Main from "../view/Main.tsx";
      import Layout from "../layout/Layout.tsx";
      import { lazy, Suspense } from "react";
      const V1 = lazy(() => import("@/view/Dashboard")); // 懒加载
      const router = [
        {
          path: "/",
          element: <Layout />,
          children: [
            {
              path: "/",
              element: <Navigate to={"/dashboard"} />,
            },
            {
              path: "/dashboard",
              element: (
                <Suspense>
                  <V1 />
                </Suspense>
              ),
            },
          ],
        },
        {
          path: "/home",
          element: <Home />,
        },
        {
          path: "/*",
          element: <Main />,
        },
      ];

      export default createBrowserRouter(router);

    ```
  2. 修改app.tsx
    ```
      import RouterConfig from './router/index'
      import { useRoutes } from 'react-router-dom';
      function App() {
        return (
          <div className="App">
            {useRoutes(RouterConfig)}
          </div>
        );
      }
      export default App;
    ```
  3. 修改index.tsx
    ```
      import React from 'react';
      import ReactDOM from 'react-dom';
      import App from './App';
      import { BrowserRouter as Router } from 'react-router-dom';

      ReactDOM.render(
        <React.StrictMode>
          <Router>
            <App />
          </Router>
        </React.StrictMode>,
        document.getElementById('root')
      );
    ```
{% endnote %}

### 路由嵌套（v6）
> 规则：使用Route标签包裹，在嵌套的组件里，使用outlet标签
  1. 在src下新建router/index.tsx
    ```
      import { lazy, ReactNode, Suspense } from 'react';
      import { RouteObject } from 'react-router-dom';
      import Layout from '../layout';
      const Category = lazy(() => import('../views/Category'));
      const Home = lazy(() => import('../views/Home'));
      const Login = lazy(() => import('../views/Login'));
      const Cat = lazy(() => import('../views/Cat'));

      const lazyload = (children: ReactNode): ReactNode => {
        return (
          <Suspense fallback={<>loading</>}>
            {children}
          </Suspense>
        )
      }

      const RouterConfig: RouteObject[] = [
        {
          path: '/',
          element: <Layout />,
          children: [
            {
              index: true,
              element: lazyload(<Home />)
            },
            {
              path: '/category',
              element: lazyload(<Category />)
            },
            {
              path: '/cat',
              element: lazyload(<Cat />)
            }
          ],
        },
        {
          path: '/login',
          element: lazyload(<Login />)
        },
      ]

      export default RouterConfig
    ```
  2. 在src/Layout/index.js中使用outlet标签插入
    ```
      import React from 'react'
      import { Outlet } from 'react-router-dom'
      import './index.css'

      function Layout() {
        return (
          <div className='container'>
            <div className='left'></div>
            <div className='right'>
              <Outlet />
            </div>
          </div>
        )
      }

      export default Layout
    ```

### 路由跳转（v6）
> 使用useNavigate跳转,也可以用Link标签
  ```
    import React from 'react'
    import { Link, useNavigate } from 'react-router-dom'

    function Home() {
      const navigate = useNavigate()
      const goToPage = (path) => {
        navigate(path)
      }
      return (
        <div>
          <h1>home</h1>
          <Link to="/category">点击跳转到category</Link>
          <button onClick={() => goToPage("/login")}>点击跳转</button>
        </div>
      )
    }
    export default Home
  ```

### 路由懒加载
  * 使用lazy和Suspense组件, 在src/router/index.js中
    ```
      import React, { lazy, Suspense } from 'react';
      import Layout from '../layout';
      const Category = lazy(() => import('../views/Category'));
      const Home = lazy(() => import('../views/Home'));
      const Login = lazy(() => import('../views/Login'));
      const Cat = lazy(() => import('../views/Cat'));
      const lazyload = (children) => {
        return (
          <Suspense fallback={<>loading</>}>
            {children}
          </Suspense>
        )
      }

      const RouterConfig = [
        {
          path: '/',
          element: <Layout />,
          children: [
            {
              index: true,
              element: lazyload(<Home />)
            },
            {
              path: '/category',
              element: lazyload(<Category />)
            },
            {
              path: '/cat',
              element: lazyload(<Cat />)
            }
          ],
        },
        {
          path: '/login',
          element: lazyload(<Login />)
        },
      ]

      export default RouterConfig
    ```

### 获取地址栏的参数
  * 使用useSearchParam
    ```
      import React from 'react'
      import { useSearchParams } from 'react-router-dom'

      function Login() {
        const [state, setState] = useSearchParams()
        const param = state.get('name')
        return (
          <div>
            {param}
          </div>
        )
      }
      export default Login
    ```

## create-react-app中使用ant-design
  1. 执行命令：npm install --save antd
  2. 在app.css中引入antd的css样式`@import '~antd/dist/antd.css'`
  3. 使用

## create-react-app中的css模块化
  1. 新建xx.module.css
  2. 在组件中引入`import styles from './xx.module.css'`
  3. 使用 `<div className={styles.button}></div>`

## 修改ant-design的默认样式
  * 使用`:golbal()`
    ```
      :global(.ant-collapse-header) {
        background-color: red;
      }
    ```

## create-react-app中使用sass
  * 执行命令：npm install --save sass
  * 将xx.module.css改成xx.module.scss即可

## create-react-app中使用redux
  1. 执行命令：npm install --save redux react-redux @types/react-redux
  2. 在src下新建store/reducers文件夹下，新建login.ts
    ```
      interface IState {
        username: string,
        password: string
      }

      const initLoginState: IState = {
        username: 'admin',
        password: '123456'
      }

      const login = (state: IState = initLoginState, action: any) => {
        return state
      }

      export default login
    ```
  3. 在src下新建store/reducers文件夹下，新建home.ts
    ```
      interface homeState {
        dataSource: any[],
        loading: boolean
      }

      const initHomeState: homeState = {
        dataSource: [],
        loading: false
      }

      const home = (state:homeState=initHomeState, action: any) => {
        return state
      }
      export default home
    ```
  4. 在src下新建store/reducers文件夹下，新建index.ts
    ```
      import { combineReducers } from 'redux'
      import login from './login'
      import home from './home'

      export default combineReducers({ login, home })
    ```
  5. 在src下新建store/index.ts
    ```
      import { createStore } from "redux";
      import reducers from "./reducers";

      const store = createStore(reducers);
      export default store;
    ```
  6. 在根路径下的index.tsx中，引入store
    ```
      import React from 'react';
      import ReactDOM from 'react-dom';
      import App from './App';
      import { BrowserRouter as Router } from 'react-router-dom';
      import { Provider } from 'react-redux';
      import store from './store';

      ReactDOM.render(
        <Provider store={store}>
          <React.StrictMode>
            <Router>
              <App />
            </Router>
          </React.StrictMode>
        </Provider>,
        document.getElementById('root')
      );
    ```

  7. 在页面中使用
    * 在src/views/Cat/index.tsx中
      ```
        import { ReactElement } from 'react'
        import { Dispatch } from 'redux'
        import { connect } from 'react-redux'
        import { rootState } from 'store'
        import { ILoginActionType } from 'store/reducers/login'

        interface Iprops {
          username?: string,
          password?: string,
          changeName?: VoidFunction
        }

        function Cat(props: Iprops): ReactElement {
          return (
            <div>
              cat
              <hr />
              {props.username}
              <hr />
              <button onClick={() => props.changeName && props.changeName()}>点击修改名称</button>
            </div>
          )
        }

        const mapStateToProps = (state: rootState) => {
          return { ...state.login }
        }

        const mapDispatchToPros = (dispatch: Dispatch) => ({
          changeName: () => {
            dispatch({
              type: ILoginActionType.CHANGE,
              payload: { username: '123' }
            })
          }
        })

        export default connect(mapStateToProps, mapDispatchToPros)(Cat)
      ```

    * 在store/reducers/login.ts中
      ```
        interface IState {
          username: string,
          password: string
        }

        const initLoginState: IState = {
          username: 'admin',
          password: '123456'
        }

        export enum ILoginActionType {
          INIT,
          CHANGE
        }

        const login = (state: IState = initLoginState, action: {type: ILoginActionType, payload: any}) => {
          switch(action.type) {
            case ILoginActionType.INIT:
              return state
            case ILoginActionType.CHANGE:
              return { ...state, ...action.payload }
            default:
              return state
          }
        }

        export default login
      ```
    
    8. 使用hooks进行改造(推荐)
      * 修改src/views/Cat/index.tsx
        ```
          import { useSelector, useDispatch } from "react-redux";
          import { rootState } from "store";
          import { ILoginActionType, ILoginState } from "store/reducers/login";

          function Cat() {
            const user: ILoginState = useSelector((state: rootState) => state.login)
            const dispatch = useDispatch()
            const changeName = () => {
              dispatch({
                type: ILoginActionType.CHANGE,
                payload: { username: '123' }
              })
            }
            return (
              <div>
                cat
                <hr />
                {user.username}
                <hr />
                <button onClick={changeName}>点击修改名称</button>
              </div>
            )
          }
          export default Cat;
        ```

# react-hooks api学习
## useState
  ```
    import { useState } from "react";
    function Test() {
      let [count, setCount] = useState<number>(0)
      return (
        <div>
          <div>数量：{count}</div>
          <button onClick={() => setCount(count += 1)}>点击</button>
        </div>
      )
    }
    export default Test;
  ```

## useEffect
  ```
    import { useEffect, useState } from "react";
    type listItemState = {
      name: string,
      age: string
    }
    type Istate = {
      list: listItemState[]
    }
    function UseEffect() {
      let [info, setInfo] = useState<Istate>({
        list: []
      })

      useEffect(() => {
        const data: listItemState[] = [{ name: '张三', age: '18' }]
        setInfo({
          list: data
        })
      }, [])
      return (
        <div>
          数据：{JSON.stringify(info.list)}
        </div>
      )
    }

    export default UseEffect;
  ```

## useLayoutEffect
> 与useEffect的区别在于，useEffect会阻塞渲染，是同步的，相当于componentDidMount

  ```
    import { useLayoutEffect, useState } from "react";
    type IState = {
      user: string,
      age: string
    }
    type listState = {
      list: IState[]
    }
    function UseLayoutEffect() {
      let [info, setInfo] = useState<listState>({
        list: []
      })
      let data: IState[] = [{ user: 'lisi', age: '18' }]
      useLayoutEffect(() => {
        setInfo({
          list: data
        })
      }, [])
      return <div>{JSON.stringify(info)}</div>;
    }
    export default UseLayoutEffect;
  ```

## useCallback+useMemo
  * 父组件
    ```
      import { Input } from "antd";
      import { ChangeEvent, useCallback, useState, useMemo } from "react";
      import Child from "./Child/Child";
      import Child2 from "./Child/Child2";

      function UseCallback() {
        let [username, setUsername] = useState<string>('')
        let [password, setPassword] = useState<string>('')
        let [age, setAge] = useState<string>('')
        let newUser = useMemo(() => ({ username }), [username]);
        let newPassword = useMemo(() => ({ password }), [password]);
        const getUser = useCallback((v: ChangeEvent<HTMLInputElement>) => {
          setUsername((username) => username = v.target.value)
        }, []);

        const getPassword = useCallback((v: ChangeEvent<HTMLInputElement>) => {
          setPassword((password) => password = v.target.value)
        }, []);

        return (
          <div>
            测试useCallBack
            <Input 
              placeholder="输入框" 
              defaultValue={age} 
              onChange={(v)=>{
                setAge((age)=>age = v.target.value)
              }} 
            />
            <br />
            <Child
              username={newUser.username}
              changeValue={getUser}
            />
            <hr />
            <Child2 password={newPassword.password} changeValue={getPassword} />
          </div>
        )
      }

      export default UseCallback;
    ```
  * 子组件child1
    ```
      import { Input } from "antd";
      import { memo } from "react";

      type Iprops = {
        username: string,
        changeValue: Function
      }

      function Child(props: Iprops) {
        console.log("child")
        return (
          <div>
            用户名：<Input placeholder="请输入用户名" defaultValue={props.username} onChange={(v)=>props.changeValue(v)} />
          </div>
        )
      }

      export default memo(Child);
    ```
  * 子组件2
    ```
      import { Input } from "antd";
      import { memo } from "react";

      type Iprops = {
        password: string,
        changeValue: Function
      }

      function Child2(props: Iprops) {
        console.log("child2")
        return (
          <div>
            密码：<Input placeholder="请输入密码" defaultValue={props.password}  onChange={(v)=>props.changeValue(v)} />
          </div>
        )
      }

      export default memo(Child2);
    ```

# 最新vite5搭建React18环境
## 初始化项目
  1. 安装命令：
    ```
      yarn create vite
      pnpm create vite
    ```
  2. 安装依赖
    ```
      pnpm install
    ```
## 配置editorConfig(不同ide相同展示)
> 在webstorm中会自动读取.editorcondig，vscode需要下载插件EditorConfig for VS Code
  1. 在根路径下创建.editorconfig文件
     ```
      # https://editorconfig.org
      root = true

      # *表示所有的文件都生效
      [*]
      charset = utf-8
      # 空格缩进、每次2格
      indent_style = tab
      indent_size = 2
      # 换行
      end_of_line = lf
      insert_final_newline = true
      trim_trailing_whitespace = true

      [*.md]
      insert_final_newline = false
      trim_trailing_whitespace = false
     ```

## 配置npm/yarn/pnpm镜像
  1. 必须要有稳定版的nodejs
  2. 安装cnpm、yarn或者pnpm
    ```
      # 安装yarn
      npm install -g yarn
      # 安装pnpm
      npm install -g pnpm
    ```
  3. 查看当前镜像源
    ```
      npm config get registry
    ```
  4. 修改npm配置
    * 在项目根目录下(package.json同一目录)中新建.npmrc文件，编辑文件内容如下：
      ```
        registry=https://registry.npmmirror.com
      ```
  5. 修改yarn配置
    * 在项目根目录下(package.json同一目录)中新建.yarnrc文件，编辑文件内容如下：
      ```
        registry "https://registry.npmmirror.com"
      ```
  6. 命令行修改配置
    ```
      npm config set registry https://registry.npmmirror.com
      yarn config set registry https://registry.npmmirror.com
    ```
  7. pnpm使用命令：
    ```
      pnpm install 包名

      pnpm i 包名

      pnpm add 包名 -S   // 默认写入dependencies

      pnpm add 包名 -D   // devDependencies

      pnpm add 包名 -g   // 全局安装

      pnpm remove 包名   // 移除
      
      pnpm up           // 更新所有依赖项

      pnpm upgrade 包名  // 更新包

      pnpm upgrade 包名 --global  // 全局更新包
    ```

## pretter集成(代码格式化)
> 官网：https://www.prettier.cn/
  1. 安装：
    ```
      pnpm add prettier -D 或
      yarn add prettier -D
    ```
  2. 在项目根目录下(package.json同一目录)中新建prettierrc.cjs文件，编辑文件内容如下：
    ```
      module.exports = {
        // 每行最大列，超过换行
        printWidth: 120,
        // 使用制表符而不是空格缩进
        useTabs: false,
        // 缩进
        tabWidth: 2,
        // 结尾不用分号
        semi: false,
        // 使用单引号
        singleQuote: true,
        // 在jsx中使用单引号而不是双引号
        jsxSingleQuote: true,
        // 箭头函数里面，如果是一个参数的时候，去掉括号
        arrowParens: 'avoid',
        // 对象、数组括号与文字间添加空格
        bracketSpacing: true,
        // 尾随逗号
        trailingComma: 'none'
      }
    ```
  3. 自动格式化
    * 在vscode搜索安装prettier插件`Prettier - Code formatter`
    * 在项目根目录下(package.json同一目录)中新建.vscode文件夹，再新建settings.json,编辑文件内容如下：(代码保存是会自动格式化代码)
      ```
        {
          // 保存自动格式化代码
          "editor.formatOnSave": true,
          "editor.defaultFormatter": "esbenp.prettier-vscode",
          // 开启stylelint自动修复
          "editor.codeActionsOnSave": {
            "source.fixAll": true
          }
        }
      ```

## vite配置
> 在vite.config.ts中配置如下：
  ```
    import { defineConfig } from "vite";
    import react from "@vitejs/plugin-react";
    import path from "path";

    // https://vitejs.dev/config/
    export default defineConfig({
      plugins: [react()],
      resolve: {
        alias: {
          "@": path.resolve(__dirname, "./src"),
        },
      },
      server: {
        host: "localhost",
        port: 8000,
        proxy: {
          "/api": "http://api-driver.marsview.cc",
        },
      },
    });
  ```

## 集成react-router6.x
  1. 安装：
    ```
      yarn add react-router-dom 或
      pnpm add react-router-dom
    ```
  2. 在src下心间router/router.tsx,并写入：
    ```
      import { createBrowserRouter, Navigate } from "react-router-dom";
      import Home from "../view/Home.tsx";
      import Main from "../view/Main.tsx";
      import Layout from "../layout/Layout.tsx";
      import Dashboard from "@/view/Dashboard";

      const router = createBrowserRouter([
        {
          path: "/",
          element: <Layout />,
          children: [
            {
              path: "/",
              element: <Navigate to={"/dashboard"} />,
            },
            {
              path: "/dashboard",
              element: <Dashboard />,
            },
          ],
        },
        {
          path: "/home",
          element: <Home />,
        },
        {
          path: "/*",
          element: <Main />,
        },
      ]);

      export default router;
    ```
  3. 在main.tsx中引入router
    ```
      import React from "react";
      import ReactDOM from "react-dom/client";
      import "./index.css";
      import { RouterProvider } from "react-router-dom";
      import router from "./router";

      ReactDOM.createRoot(document.getElementById("root")!).render(
        <React.StrictMode>
          <RouterProvider router={router} />
        </React.StrictMode>,
      );
    ```

# 封装loading组件(使用antd的spin)
  1. 安装antd
    ```
      yarn add antd
    ```
  2. 在components/loading文件夹中新建如下文件
    * index.css文件
      ```
        #i-loading {
          position: fixed;
          top: 0;
          left: 0;
          right: 0;
          bottom: 0;
          display: flex;
          align-items: center;
          justify-content: center;
          font-size: 20px;
        }
      ```
    * index.tsx文件
      ```
        import { createRoot } from "react-dom/client";
        import Loading from "./loading";

        let count = 0;

        export const showLoading = () => {
          if (count === 0) {
            const loading = document.createElement("div");
            loading.setAttribute("id", "i-loading");
            document.getElementById("root")?.appendChild(loading);
            createRoot(loading).render(<Loading />);
          }
          count++;
        };

        export const hideLoading = () => {
          if (count < 0) {
            return;
          }
          count--;
          if (count === 0) {
            document
              .getElementById("root")
              ?.removeChild(document.getElementById("i-loading") as HTMLDivElement);
          }
        };
      ```
    * Loading.tsx文件
      ```
        import { Spin } from "antd";
        import "./index.css";

        export default function Loading({ tip = "loading" }: { tip?: string }) {
          return <Spin tip={tip} size="large" className="i-loading" />;
        }
      ```
  3. 在封装的axios中使用
    ```
      import { hideLoading, showLoading } from "@/components/loading";
      import { message } from "antd";
      import axios, { AxiosError } from "axios";

      const instance = axios.create({
        // baseURL: "/test",
        timeout: 8000,
        timeoutErrorMessage: "请求超时，请稍后再试",
        withCredentials: true,
      });

      // 请求拦截器
      instance.interceptors.request.use(
        (config) => {
          showLoading();
          const token = localStorage.getItem("token");
          if (token) {
            config.headers.Authorization = "Token:" + token;
          }
          return {
            ...config,
          };
        },
        (error: AxiosError) => {
          return Promise.reject(error);
        },
      );

      // 响应拦截器
      instance.interceptors.response.use(
        (response) => {
          const data = response.data;
          hideLoading();
          if (data.code === 500001) {
            message.error(data.msg);
          } else if (data.code != 0) {
            return Promise.reject(data);
          }
          return data.data;
        },
        (error) => {
          hideLoading();
          message.error(error.message);
          return Promise.reject(error.message);
        },
      );

      export default {
        get<T>(url: string, params?: object): Promise<T> {
          return instance.get(url, { params });
        },
        post<T>(url: string, params?: object): Promise<T> {
          return instance.post(url, params);
        },
      };
    ```

# 封装localStorage、sessionStorage、cookie
> 封装cookie需要安装js-cookie和@types/js-cookie
  1. 安装js-cookie和@types/js-cookie
    ```
      yarn add js-cookie
      yarn add @types/js-cookie -D

      pnpm add js-cookie
      pnpm add @types/js-cookie -D
    ```
  2. 在utils/storage.ts中写入：
    ```
      import Cookies from "js-cookie";

      // localstorage模块封装
      const getValue = (type: "local" | "session" | "cookie", key: string) => {
        let value;
        if (type === "local") {
          value = localStorage.getItem(key);
        } else if (type === "session") {
          value = sessionStorage.getItem(key);
        }

        if (!value) return "";
        try {
          return JSON.parse(value);
        } catch (error) {
          return value;
        }
      };

      export default {
        local: {
          set: (key: string, value: any) => {
            localStorage.setItem(key, JSON.stringify(value));
          },
          get: (key: string) => {
            return getValue("local", key);
          },
          remove: (key: string) => {
            localStorage.removeItem(key);
          },
          clear: () => {
            localStorage.clear();
          },
        },
        session: {
          set: (key: string, value: any) => {
            sessionStorage.setItem(key, JSON.stringify(value));
          },
          get: (key: string) => {
            return getValue("session", key);
          },
          remove: (key: string) => {
            sessionStorage.removeItem(key);
          },
          clear: () => {
            sessionStorage.clear();
          },
        },
        cookie: Cookies,
      };
    ```

# vite的多环境配置(编译时环境配置)
  1. 在根路径下创建对应的环境文件，例如：.env.local, .env.sit, .env.production
    ```
      # 环境设置
      NODE_ENV=development

      VITE_API_URL=https://www.fastmock.site/mock/development/f5d8d99de1a8ce59a932ad17a28ed974/temp

    ```
  2. 打印import.meta.env能够看到所有的值（注意，环境变量必须要是VITE_开头，不然不会生效）
  3. 在package.json里，添加`--mode`关键词，启动对应的程序，并且要保证在跟路径下有对应的.env文件
    ```
      ...
      "scripts": {
        ...
        "dev:development": "vite --mode development",
        "dev:sit": "vite --mode sit",
        ...
      },
    ```

# vite多环境配置（运行时环境配置-更推荐）
  1. 在根路径下新建config/index.ts,写入一下代码：
    ```
      type ENV = "development" | "sit" | "production";

      const env = (document.documentElement.dataset.env as ENV) || "development";

      const config = {
        development: {
          node_env: "development",
          api_url:
            "https://www.fastmock.site/mock/development/f5d8d99de1a8ce59a932ad17a28ed974/temp",
        },
        sit: {
          node_env: "sit",
          api_url:
            "https://www.fastmock.site/mock/sit/f5d8d99de1a8ce59a932ad17a28ed974/temp",
        },
        production: {
          node_env: "production",
          api_url:
            "https://www.fastmock.site/mock/production/f5d8d99de1a8ce59a932ad17a28ed974/temp",
        },
      };

      export default {
        env,
        ...config[env],
      };
    ```

  2. 在index.html的html标签添加`data-env="sit"`
    ```
        <!doctype html>
        <html lang="en" data-env="sit">
          ...
        </html>
    ```

# 配置@
  1. 在vite.config.ts中添加如下：
    ```
      import { defineConfig } from "vite";
      import react from "@vitejs/plugin-react";
      import path from "path";

      export default defineConfig({
        ...
        resolve: {
          alias: {
            "@": path.resolve(__dirname, "./src"),
          },
        },
        ...
      });
    ```
  2. 在tsconfig.json中添加如下：
    ```
      {
        "compilerOptions": {
          ...
            "paths": {
              "@/*": ["./src/*"]
            },
        }
      }
    ```

# 通过vscode提交git
  1. 输入：git init
  2. 点击vs code放大镜下面的按钮
    * ![vscode添加git文件](/images/react/vscode的vite配置.png)