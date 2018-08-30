## Vue2.X 项目结构
> 技术栈：vue, vue-router, vuex, axios, iview, less

<pre>
├──src
    ├── assets
        ├── images
            ├── icons
            ...
        └── styles (样式)
            ├── modules
                ├── Panination.less
                ├── Navigation.less
                ...
            ├── pages
                ├── demoPage1
                    ├── index.less
                    ├── detail.less
                    ...
                ├── demoPage2
                ...
            ├── color.less
            ├── common.less
            ├── form.less
            └── my─iview.less
    ├── components (组件)
        ├── layout
            ├── Header.vue
            ├── Sider.vue
            ├── Footer.vue
            └── Index.vue
        ├── modules (通用组件)
            ├── Panination.vue
            ├── Navigation.vue
            ├── CustomTable.vue
            ├── DlgConfirmReason.vue
            ├── index.js
            ...
        └── pages (按页面功能分组定义组件)
            ├── login
                ├── Login.vue
                ...
            ├── demoPage1
                ├── Index.vue
                ├── Detail.vue
                ...
            ├── demoPage2
            ├── demoPage3
            ...
    ├── const (静态常量，数据字典)
        ├── enumData.js
            ...
    ├── router (路由配置)
        ├── routes
            ├── login.js
            ├── demoPage1.js
            ├── demoPage2.js
            ...
        └── index.js
    ├── store (状态管理)
        ├── stores
            ├── login.js
            ├── common.js
            ├── file.js
            ├── demoPage1.js
            ├── demoPage2.js
            ...
        └── index.js
    ├── utils (工具类)
        ├── ajax.js
        ├── config.js
        ├── mixinsXXX.js
        ├── tool.js
        ...
    ├── App.vue
    └── main.js
</pre>
