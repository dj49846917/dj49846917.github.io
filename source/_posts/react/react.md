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