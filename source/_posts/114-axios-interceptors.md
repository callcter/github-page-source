---
title: vue+axios 实现登录状态判定以及请求拦截
tags:
  - vue
url: 541.html
id: 541
categories:
  - 前端开发
date: 2018-01-15 18:18:21
---

##### 策略

1.  通过 axios 在请求头添加 Authorization
2.  后端验证，通过验证：返回数据，未通过：返回错误状态 401
3.  通过 axios 拦截器，判断返回状态，如果是401，跳转到登陆

##### 组件

*   vue
*   vuex
*   vue-router
*   axios

##### 关键点

axios 的拦截器

    // 请求 拦截器
    axios.interceptors.request.use(
        config => {
            // 在请求之前做某事发送
            if (store.state.login.token) {// 判断是否存在token,如果存在,则每个http header都加上 token
                config.headers.Authorization = `Bearer ${store.state.login.token}`;
            }
            return config;
        }, err => {// 发生错误
            return promise.reject(err);
        });
    
    // 响应 拦截器
    axios.interceptors.response.use(
        response => { // 对响应数据做某事
            return response;
        }, error => {
            if (error.response) {// 对响应错误做出反应
                switch (error.response.status) {
                    case 401:
                        // 401 清除token信息  并跳转到login
                        })
                }
                // 返回接口返回的错误信息
                return Promise.reject(error.response.data);
            }
        });