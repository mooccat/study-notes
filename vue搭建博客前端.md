## 环境及依赖准备

Vue.js 提供一个官方命令行工具，可用于快速搭建大型单页应用。该工具提供开箱即用的构建工具配置，带来现代化的前端开发流程。只需几分钟即可创建并启动一个带热重载、保存时静态检查以及可用于生产环境的构建配置的项目:

```
# 全局安装 vue-cli
$ npm install --global vue-cli
# 创建一个基于 webpack 模板的新项目,期间会让你输入项目信息以及是否安装vue-router、单元测试、ESLint等
$ vue init webpack vue-blog
# 安装依赖，走你
$ cd vue-blog
$ npm install
$ npm run dev
```

### 安装vuex

Vuex 是一个专为 Vue.js 应用程序开发的状态管理模式。它采用集中式存储管理应用的所有组件的状态，并以相应的规则保证状态以一种可预测的方式发生变化。Vuex 也集成到 Vue 的官方调试工具 devtools extension，提供了诸如零配置的 time-travel 调试、状态快照导入导出等高级调试功能。

```
# 安装vuex并保存到package.json
npm install vuex -s
```
此时src目录下的结构为

```
│  App.vue
│  main.js
├─assets
│      logo.png
│      
├─components
│      HelloWorld.vue
│      
└─router
        index.js
```

新建store文件夹存放store

```
│  App.vue
│  main.js
│  tree.txt
│  
├─assets
│      logo.png
│      
├─components
│      HelloWorld.vue
│      
├─router
│      index.js
│      
└─store
    │  actions.js
    │  getters.js
    │  index.js
    │  mutation-types.js
    │  
    └─modules
```

index.js内容为

```
import Vue from 'vue'
import Vuex from 'vuex'
import * as actions from './actions'
import * as getters from './getters'
import createLogger from 'vuex/dist/logger'

Vue.use(Vuex)

const debug = process.env.NODE_ENV !== 'production'

export default new Vuex.Store({
  actions,
  getters,
  modules: {
    
  },
  strict: debug,
  plugins: debug ? [createLogger()] : []
})
```

在main.js中引入store
```
import Vue from 'vue'
import App from './App'
import router from './router'

+ import store from './store'

Vue.config.productionTip = false

/* eslint-disable no-new */
new Vue({
  el: '#app',
+ store，
  router,
  template: '<App/>',
  components: { App }
})

```

