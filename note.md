##### vue项目搭建步骤
```html
$ vue init webpack my-project
$ cd my-project
$ npm install
$ npm run dev
// 使用scss
    npm install  sass-loader -D
    npm install  node-sass -D
// 如果需要全局引入.scss文件的配置
    npm install  sass-resources-loader -D
    在项目里找到build/utils文件 如下修改
```
##### vue-cli 3.0 搭建Vue项目

```
1. 安装vue-cli 3.0
   npm i @vue/cli -g
2. vue-cli搭建脚本文件
   vue create vue-test
```

##### git基本用法

```
1. git stash save
2. git pull
3. git add ./
4. git commit -m'xxx'
5. git push
```

##### iconfont 用法
```
- 引入
 <link rel="stylesheet" href="https://at.alicdn.com/t/font_1352771_ktvjvzhb1v.css">
- 使用
 <i class="iconfont ju-hk-count-down"></i>
```

##### 图片阴影实现案例
```
// html
<div class="shade__wrap">
    <img src="https://image.juooo.com//group1/M00/02/BA/rAoKmV0HKROANbIoAAC-qZa523k987.jpg" alt="" class="shade__wrap__img"/>
</div>
// css
.shade{ 
    &__wrap{
        position: absolute;
        left: 0;
        top: 0;
        width: 100%;
        height: 100%;
        overflow: hidden;
        &__img{
            width: 100%;
            height: 100%;
            filter: blur(40px);
        }
    }
}
```
- 效果：

![阴影](./assets/img/企业微信截图_15668930477104.png "图片")

##### 对象的深拷贝方法
- Object
```js
  const a1 = {name: "", id: ""}
  const a2 = Object.assign({}, a1)
```
- Array 
```js
   es5:     const a1 = [1,2,3]
            const a2 = a1.concat(al)
   es6:     const a1 = [1,3,5,6]
            const a2 = [...a1] // [...a2] = a1
```

##### SPA页面缓存(包含接口)
- 在```App.vue``` 中加 ```keep-alive,keep-alive```的自由属性:

```js
  include: // 包含需要缓存的页面
  exclude: // 包含不需要缓存的页面
```
![keep-alive](./assets/img/企业微信截图_15697511529553.png "图片")

- 获取 需要缓存的页面（```includeComponents```）

![keep-alive](./assets/img/企业微信截图_15697511799732.png "图片")

- 在需要做缓存的页面加路由钩子函数

![keep-alive](./assets/img/企业微信截图_15697512335870.png "图片")
```js
   beforeRouterEnter(){ 
        // 添加需要缓存的页面
        this.$store.dispatch("ADD_INCLUDE_COMPONENT_ACTION", "Home")
   }
   beforeRouteLeave(){
       // 移除需要缓存的页面
       this.$store.dispatch("REMOVE_INCLUDE_COMPONENT_ACTION", "Home")
   }
```
- ```store```里面的逻辑

![keep-alive](./assets/img/企业微信截图_15697513351089.png "图片")

![keep-alive](./assets/img/企业微信截图_15697512757104.png "图片")

![keep-alive](./assets/img/企业微信截图_15697513058054.png "图片")
