#### 双向绑定的几种方式
1. 发布者-订阅者模式

```bash
一般通过sub, pub的方式实现数据和视图的绑定监听，更新数据方式通常做法是 vm.set('property', value)
```
2. 脏值检查

```bash
angular.js 是通过脏值检测的方式比对数据是否有变更，来决定是否更新视图，最简单的方式就是通过 setInterval() 定时轮询检测数据变动，当然Google不会这么low，angular只有在指定的事件触发时进入脏值检测
```
3. 数据劫持

```bash
通过 Object.defineProperty() 来劫持各个属性的setter，getter，在数据变动时发布消息给订阅者，触发相应的监听回调。
```

#### Vue 原理理解
1. Vue 采用数据劫持结合发布者-订阅者模式，通过```Object.defineProperty()```来劫持各个属性的setter,getter,在数据变动时发布消息给订阅者，触发相应的监听回调

2. 渐进式原理

3. 虚拟Dom

```js
// virtual Dom 是将真实的Dom的数据抽取出来，以对象的形式模拟树形结构
// js对象,dom信息的对象
// 例
const Vnode = (
  { type: "ul", props: { className: "list" }, children: [
    { type: "li", props: {}, children: ["item 1"] },
    { type: "li", props: {}, children: ["item 2"] }
  ] }
);
```
4. diff算法

```bash
本质：找两个对象之间的差异,尽可能复用节点
```
