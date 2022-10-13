---
title: UniApp引入Tailwind CSS
date: 2022-10-13 11:22:54
tags: [UniApp,CSS,前端]
categories: 
- 菜鸡修炼手册
permalink: /articles/2022/10/13/1665631374512.html
top_img: https://tmx.fishpi.cn/img/rocks-g837bc8c3c_1920.jpg
cover: https://tmx.fishpi.cn/img/rocks-g837bc8c3c_1920.jpg
---
![1628345033618.jpg](https://tmx.fishpi.cn/img/rocks-g837bc8c3c_1920.jpg)

## 踩坑
可能是我的姿势不对，引入一直失败
用HBuilderX创建的VUE 3项目似乎有问题，模板还是VUE 2的写法
按照Tailwind CSS 文档导入一直不生效
最后发现需要手动创建`vite.config.js`并在其中引入postcss
虽然能引到Tailwind CSS，但是一直报`[vite] [plugin:vite:css] Cannot read property 'config' of undefined`
研究了一晚上，最终放弃

## 引入
1. 使用官方的vue3模板
    `npx degit dcloudio/uni-preset-vue#vite my-vue3-project`
    可能会下载失败，可以去[gitee](https://gitee.com/dcloud/uni-preset-vue)下载
    完事之后`yarn install`
2. 移除vuex
    直接运行会报如下warn
    `DeprecationWarning: Use of deprecated folder mapping "./" in the "exports" field module resolution of the package at \node_modules\vuex\package.json.`
    运行`yarn remove vuex`移除
3. 引入Tailwind CSS
    直接按照Tailwind CSS官网文档操作
    先安装依赖
    `npm install -D tailwindcss@latest postcss@latest autoprefixer@latest`
    创建配置文件
    `npx tailwindcss init -p`
    然后在`main.js`中引入css
    `import "tailwindcss/tailwind.css"`
    官网文档到此结束
    然而，并没有生效
4. 在`vite.config.js`中引入
    修改后如下
    ```js
    import { defineConfig } from 'vite'
    import uni from '@dcloudio/vite-plugin-uni'
    import postcssImport from "postcss-import"
    import autoprefixer from 'autoprefixer'
    import tailwindcss from 'tailwindcss'
    // https://vitejs.dev/config/
    export default defineConfig({
        plugins: [
            uni(),
        ],
        css:{
            postcss:{
                plugins:[
                    postcssImport,
                    autoprefixer,
                    tailwindcss
                ]
            }
        }
    })

    ```
至此，已能正常使用

## 配合插件食用
安装`Tailwind CSS语言服务`增加自动补全
地址https://ext.dcloud.net.cn/plugin?id=8560