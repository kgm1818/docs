```
├── public                              //
│   ├── favicon.ico                     // 图标
│   ├── index.html                      // 入口html文件
├── src                                 // 源码目录
│   ├── assets                          // 静态资源
│   │   ├── images                      //
│   │   ├── styles                      //
│   │   ├── js                          //
│   │   ├── components                  // 组件
│   │   │   ├── xxx.vue                 // 公共组件
│   │   │   ├── xxx                     // 单页面组件
│   │   ├── config                      // 底层静态数据配置
│   │   │   ├── server.js               // 环境配置
│   │   │   ├── xxx.js                  //
│   │   ├── entities                    // 数据模型
│   │   │   ├── xxx.js                  // 公共数据模型
│   │   │   ├── xxx                     // 单模数据块模型
│   │   ├── infrastructrue              // 基础方法
│   │   │   ├── http.js                 // 请求接口公共处理
│   │   ├── model                       // 接口数据输出
│   │   │   ├── xxx.js                  // 单模块接口数据输出
│   │   ├── pages                       // 页面
│   │   │   ├── xxx.vue                 // xxx页面
│   │   ├── router                      // 路由
│   │   │   ├── index.js                // 路由公共处理
│   │   │   ├── routes.js               // 路由
│   │   ├── App.vue                     // 页面入口文件
│   │   ├── main.js                     // 程序入口文件，加载各种公共组件
├── babel.config.js                     // babel配置
├── vue.config.js                       // 配置
├── package.json                        // 安装包信息
├── package-lock.json                   // 锁定安装包的版本号
├── README.md                           // 文档
```