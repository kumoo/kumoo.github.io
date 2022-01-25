---
title: 搭建一个Vue2+Typescript+CompositionApi+Vite项目基本结构
date: 2022-01-25 14:52:02
tags:
cover: /assets/20220125155602.jpg
---
通过使用`vue2`和`@vue/composition-api`，在保证兼容性的前提下使用vue3的语法或是对旧项目进行一个升级改造（便于后期转换到vue3）。

这里直接使用`vite`来代替`webpack`构建项目。

### 创建项目
``` bash
// init一个vite项目
npm init @vitejs/app
// 项目名
? Project name: demo
// 选择框架 这里选 vanilla， 选 vue 的话会默认 vue3
? Select a framework: vanilla
// 是否使用ts
? Select a variant: vanilla-ts
// 创建完成
cd demo     
npm install 
npm run dev
```
选择的框架有以下几种：
- `vanilla`：原生js，没有任何框架集成
- `vue`：vue3框架，只支持vue3
- `react`：react框架
- `preact`：轻量化react框架
- `lit-element`：轻量级web组件
- `svelte`：svelte框架

### 项目结构
![](/assets/20220125151157.png)

### 安装项目依赖
使用官方提供的vue2支持插件[vite-plugin-vue2](https://vitejs.bootcss.com/guide/features.html#vue)

``` bash
npm install vite-plugin-vue2 -D
```
创建配置文件`vite.config.js`

``` js
import { createVuePlugin } from 'vite-plugin-vue2'

export const plugins = [createVuePlugin( /*options*/)]
```

安装`vue`以及`@vue/composition-api`
``` bash
npm install vue @vue/composition-api
```

创建 App.vue
``` vue
<template>
	<div>Hello Vue 2 + Vite</div>
</template>
```

修改main.ts
``` js
import Vue from 'vue'
import VueCompositionAPI from '@vue/composition-api'
import App from './App.vue'

Vue.use(VueCompositionAPI)

new Vue({
  render: h => h(App)
}).$mount('#app')
```

此时会提示`找不到模块“./App.vue”或其相应的类型声明`，因为前面选了`vanilla-ts`，所以这是个ts项目。

src目录下创建`shims-vue.d.ts`

``` ts
declare module '*.vue' {
  import Vue from 'vue'
  export default Vue
}
```

运行命令启动项目
``` bash
npm run dev
```

这样基本的项目搭建完成，我们可以在项目中使用vue3语法。
``` js
<script lang="ts">
import { defineComponent } from '@vue/composition-api'

export default defineComponent({
  name: 'ComponentMenu',
  setup() {
    
  }
})
</script>
```

### 添加eslint

``` bash
npm install eslint

// 初始化eslint配置
npx eslint --init

? How would you like to use ESLint? // 你想怎样使用eslint
  To check syntax only //只检查语法
> To check syntax and find problems//检查语法、发现问题
  To check syntax, find problems, and enforce code style//检查语法、发现问题并执行代码样式


? What type of modules does your project use?  // 您的项目使用什么类型的模块?
> JavaScript modules (import/export)
  CommonJS (require/exports)
  None of these



? Which framework does your project use? // 项目中使用的什么框架？
  React
> Vue.js
  None of these

Does your project use TypeScript? » No / Yes // 项目中是否使用了ts？


? Where does your code run?  // 你的代码运行在什么地方？(多选)
> Browser // 浏览器
  Node // node

? What format do you want your config file to be in? // 您希望配置文件的格式是什么?
> JavaScript
  YAML
  JSON

// ts项目需要下载的额外eslint插件 默认安装
eslint-plugin-vue@latest @typescript-eslint/eslint-plugin@latest @typescript-eslint/parser@latest
? Would you like to install them now with npm? » No / Yes
```
具体的规则可自行配置。