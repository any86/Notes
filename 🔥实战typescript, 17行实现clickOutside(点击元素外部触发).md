# 🔥17行代码实战, 深入理解UI组件库都在用的"el.contains(node)"

## 干什么用的?
`el.contains(node)`用来判断一个元素是否在另一个元素内.
一般组价库中的"下拉"和"气泡对话框"用这个特性来实现"**点击组件外部关闭组件**"功能.
![demo](https://user-gold-cdn.xitu.io/2020/3/23/171052b7e0893372?w=366&h=206&f=gif&s=1715417)

## 17行实现clickOutside(点击元素外部触发)
本文并不是要讲如何实现一个"气泡"组件, 而是实现一个组件中的通用功能:**点击元素外部触发**, 希望帮助大家能举一反三.

**源码** : 
https://github.com/any86/6h/blob/master/packages/click-outside/src/index.ts
## 最终目标
```typescript
// 开始监听
const cancel = clickOutside(el, e=>{
    // 点击el外部触发
});
// 取消监听
cancel();
```

## 原理
1. 监听`document`的`touchend`和`click`事件.
2. 在事件回调中判断`event.target`(当前触发事件的元素)是否在"目标元素"内
**代码详解请看代码注释.**

## 实现代码
代码基于ts实现, 🤖不过只要会js就能看懂.
```javascript
const eventNames = ['click', 'touchend'];
export default function (el: Node, callback: (ev:Event) => void) {
    let isTouch = false;
    function handler(ev: Event): void {
        // touchend
        if (eventNames[1] === ev.type) isTouch = true;
        // 禁止移动端touchend触发后还触发click
        if (eventNames[0] === ev.type && isTouch) return;
        // 判断点击元素是否在el外
        // 由于ev.target的类型是EventTarget,
        // 而contains方法标注的参数类型是Node, 
        // 实际上EventTarget也是dom元素,
        // 所以此处使用需要类型断言, 标注为Node类型
        if (!el.contains(ev.target as Node)) callback(ev);
    }

    eventNames.forEach(name => {
        document.addEventListener(name, handler);
    });

    return () => {
        eventNames.forEach(name => {
            document.removeEventListener(name, handler);
        });
    }
}
```

## 扩展
说个"锦上添花"的功能, 其实我们可以借助`new Event()`模拟实现原生事件`click-outside`, 这样我们可以在🤖**vue**中这么用:
```html
<div @click-outside="onCall"></div>
```
此处的逻辑也很简单, 不展开讲解, 如有需要请看我之前发的文章: https://juejin.im/post/5e5f326af265da576c24d2cc#heading-8

## 计划
工作中只要发现可复用的"**短代码**", 尽量保证代体积在1kb以内, 我都会用**typescript**封装成函数方便大家学习, 现已增加了2例:

[@6h/be-full](https://github.com/any86/6h/tree/master/packages/be-full)
任意元素**全屏**显示, 支持PC/移动端, 不到1kb.

[@6h/click-outside](https://github.com/any86/6h/tree/master/packages/click-outside) 
点击指点元素外部触发回调, 支持手机/桌面端.


## 🔥typescript系列课程
基础教程从这里开始

[第一课, 体验typescript](https://juejin.im/post/5d19ad6de51d451063431864)

[第二课, 基础类型和入门高级类型](https://juejin.im/post/5d1af3426fb9a07ed4411a9b)

[第三课, 泛型](https://juejin.im/post/5d27f160e51d45108223fcf9)

[第四课, 解读高级类型](https://juejin.im/post/5d3fe80fe51d456206115987)

[第五课, 命名空间(namespace)是什么](https://juejin.im/post/5d5d04dfe51d4561af16dd24)

[特别篇, 在vue3🔥源码中学会typescript🦕 - "is"](https://juejin.im/post/5da6d1aae51d4524ad10d1d8)

[第六课, 什么是声明文件(declare)? 🦕 - 全局声明篇](https://juejin.im/post/5dcbc9e2e51d451bcb39f123)

[新手前端学🔥typescript - 实战篇, 实现浏览器全屏(59行)](https://juejin.im/post/5dd33ce3e51d453fbf29e0e5)

## 🔥往期热门文章
[🔥常用正则大全2020](https://juejin.im/post/5d245d4151882555300feb77)

[🚆新手前端不要慌! 给你✊10根救命稻草🍃](https://juejin.im/post/5d904712e51d45781e0f5dd0)

[真.1px边框, 🚀 支持任意数量边和圆角, 1 个万金油的方法](https://juejin.im/post/5d70a030f265da03a715f3fd)

[🚀揭秘vue/react组件库中🤚5个"作者不造的轮子"](https://juejin.im/post/5d89cd156fb9a06acb3ee19e)

[vue / react的UI库都在用的几个DOM API🚀](https://juejin.im/post/5d808601f265da03ef7a469b)

## 微博
刚玩微博, 咱们可以互相关注, 嘿嘿
![weibo.png](https://user-gold-cdn.xitu.io/2019/12/30/16f54bffe31ce14b?w=810&h=1020&f=jpeg&s=84481)

## 微信群
感谢大家的阅读, 如有疑问可以加我微信, 我拉你进入**微信群**(由于腾讯对微信群的100人限制, 超过100人后必须由群成员拉入)

![](https://user-gold-cdn.xitu.io/2019/9/19/16d474d245b69492?w=512&h=512&f=jpeg&s=27137)