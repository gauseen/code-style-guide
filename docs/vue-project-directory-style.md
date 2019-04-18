### 项目目录规范 vue-cli 3.x

```sh
|————— dist                 # 编译产出目录
|
|————— docs                 # 组件使用文档（可参照组件文档示例）
|
|————— public               # 静态资源，不经过 webpack，需要通过绝对路径来引用它们
|
|
|————— src
|      |———— api            # 抽取出 ajax 请求
|      |
|      |———— assets         # 静态资源目录，经过 webpack 打包处理
|      |
|      |———— components     # 公用组件
|      |
|      |———— directives     # 指令
|      |
|      |———— filters        # filter
|      |
|      |———— icons          # svg icons
|      |
|      |———— layout         # 应用骨架级页面(SideBar、Header、AppMain)
|      |
|      |———— locales        # 国际化
|      |
|      |———— pages/views    # 页面(与路由对应)
|      |
|      |———— plugins        # 插件
|      |
|      |———— router         # 路由
|      |
|      |———— store                   # 状态管理(vuex)
|      |     |
|      |     |———— index.js          # 组装模块并导出 store 的地方
|      |     |
|      |     |———— actions.js        # 根级别的 action
|      |     |
|      |     |———— mutations.js      # 根级别的 mutation
|      |     |
|      |     └──── modules
|      |           |
|      |           |—— user.js       # 用户模块
|      |           |
|      |           └── todo.js       # todo 模块
|      |
|      |———— styles         # 样式
|      |
|      |———— utils          # 公用方法
|      |
|      |———— App.vue        # 应用入口组件
|      |
|      └──── main.js        # 入口文件
|
|
|————— tests                # 测试
|
|
|————— .env                 # 环境变量（在所有的环境中被载入）
|
|
|————— env.[mode]           # 环境变量（只在指定的模式中被载入：development、production）
|
|
|————— .env.[mode].local    # 环境变量（只在指定的模式中被载入，但会被 git 忽略），本地开发时可自己创建该文件
|
|
|————— .eslintrc.js         # eslint 规则
|
|
|————— vue.config.js        # vue-cli 3.x 配置文件
|
|
|————— vue.config.utils.js  # vue-cli 配置文件
|
|
└───── yarn.lock            # 依赖资源版本锁定不可编辑或删除该文件
```

**注：按需创建自己的项目目录**
