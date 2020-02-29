# 🔥Vue"非常规"技巧, 或抛弃"指令", 让"原生Event"与"v-on"配合出花

## 一句话理解"Event"
类似vue中`$emit`, 使用`new Event`可以创建浏览器的**原生事件**,通过使用`addEventListener`监听事件. 

简单使用:
```javascript
// 监听
el.addEventListener('abc', onAbc);
// 创建事件
const event = new Event(type);
// 派发事件对象
el.dispatchEvent(event);
```

## 这和Vue有什么关系?
看, 这样给组件绑定事件很常见:
```html
<my-scroll @scroll-reach-bottom="onScrollReachBottom"/>
```
但是如果不是组件是一个**html元素**呢?
```html
<div @scroll-reach-bottom="onScrollReachBottom"/>
```
要实现上例就需要用到自定义事件(Event).

## 为什么不用"组件"?
以"拖拽组件"为例, 众所周知组件至少有一个元素(根), 那么如果我们使用"拖拽组件":
```html
<my-drag>
    <span>待拖拽</span>
</my-drag>

<!--实际dom结构-->
<div>
    <span>待拖拽</span>
</div>
```
可以看到组件破坏了dom结构, 使用时可能会直接影响样式, 所以很多vue插件都使用了"vue指令"解决这个问题.

## 更好的"vue指令"
这个是饿了么ui的InfiniteScroll(无限滚动)指令:
```html
<ul 
    v-infinite-scroll="load" 
    infinite-scroll-distance="10" 
    infinite-scroll-delay="10">
    <li v-for="i in count">{{ i }}</li>
</ul>
```

```javascript
export default{
    directives:{
        infiniteScroll
    }
}
```

对比下如果用Event实现后:
```html
<ul @infinite-scroll="load">
    <li v-for="i in count">{{ i }}</li>
</ul>
```

```javascript
export default{
    mounted(){
        const is = new InfiniteScroll(this.$el,{
            distance:10,
            delay:10
        });
        this.$on('hook:destroy', is.destroy);
    }
}
```
说实话没对使用者差别不太大, 无非就是"@"语义看起来稍微好点, 但是销毁需要自己手动触发.

但是如果你编写过"vue指令", 你应该还记得vue对指令的"钩子函数"的设计并不是特别友好, 需要自己实现钩子间的数据管理, 不优雅.

![](https://user-gold-cdn.xitu.io/2020/3/1/17091ce34303e5cf?w=654&h=114&f=png&s=20487)

但是用了`Event`方式, 你可以把自己的组件做成构造函数, 上面的"状态管理"问题就好解决了, **而且你这样写出的代码就不局限于是vue插件了, 他就是一个普通的js插件, 可以用在任何框架下.**

**当然**指令的"修饰符"等概念还是很吸引人的, 如果需要可以结合`Event`和"vue指令"一起用, 如虎添翼.

## 如何使用Event?
定义事件'abc', 并给事件对象赋值:
```javascript
// 注册监听
el.addEventListener('abc', event=>{
    'abc' === event.type // true
    1 === event.a // true
});
// 创建事件
const event = new Event(type, {
     // 事件是否可以冒泡, 默认false
    bubbles: false,
     // 事件是否可以取消(使用preventDefault时), 默认false
    cancelable: false,
    // 事件是否会在影子DOM根节点之外触发侦听器, 默认false, 
    // 此属性涉及原生web component, 欲深入了解web component请查阅mdn
    composed: false 
});

// 修改事件对象的值
Object.assign(event, {a:1,b:2});
// 派发事件对象
el.dispatchEvent(event);
```


### cancelable实例
```javascript
el.addEventListener('abc', event=>{
    // 受到cancelable控制
    // cancelable === false时,
    // preventDefault()无效
    event.preventDefault();
});
```

### bubbles实例
```javascript
parent.appendChild(el);

el.addEventListener('abc', event=>{
    // event.stopPropagation();
});

parent.addEventListener('abc', event=>{
    // 受到bubble控制
    // bubble === false时,
    // 子元素绑定的事件回调中, 
    // 有没有event.stopPropagation()都不触发
});
```

## 兼容
![](https://user-gold-cdn.xitu.io/2020/3/1/17091ea1f6f53a76?w=1253&h=235&f=png&s=45032)
`Event`对低版本不兼容, 但是mdn上已经明确写出, 未来的浏览器都会以`Event`作为标准, 所以我们还要结合老语法做下兼容.

### createEvent
```javascript
const event = document.createEvent('HTMLEvents');
// 参数意义和new Event一样
event.initEvent(type, bubbles, cancelable);
```

![](https://user-gold-cdn.xitu.io/2020/3/1/17091ece8976456b?w=1255&h=296&f=png&s=52985)
"灰色"代表"未知", 但是可以放心使用, 因为vue2.6的源码中关于"v-model"部分多处使用了`createEvent`说明应该可以兼容到ie9.

![](https://user-gold-cdn.xitu.io/2020/3/1/17091eec08081b52?w=580&h=121&f=png&s=17667)



### 兼容代码
```javascript
function dispatchDOMEvent(el, payload, eventInit){
    let event;
    if (void 0 !== Event) {
        event = new Event(type, eventInit);
    } else {
        event = document.createEvent('HTMLEvents');
        event.initEvent(type, eventInit?.bubbles, eventInit?.cancelable);
    }
    return el.dispatchEvent(event);
}
```
源码: https://github.com/any86/any-touch/blob/master/packages/core/src/dispatchDomEvent.ts

## 我的应用
其实这种方式和vue配合也是偶然发现, 不是什么复杂的东西, 只是一般不会往这上面想.

我做了一个手势库用这种方式实现在vue下和"v-on"配合:
```html
<div 
    @tap="onTap" 
    @swipe="onSwipe" 
    @press="onPress" 
    @pan="onPan" 
    @pinch="onPinch" 
    @rotate="onRotate">
    <p>Hello any-touch</p>
</div>
```

```javascript
import AnyTouch from 'any-touch';
export default {
    mounted() {
        const {destroy} = new AnyTouch(this.$el);
        this.on('hook:destroy', destroy);
    }
};
```
✋手势库: https://github.com/any86/any-touch

[![](https://user-gold-cdn.xitu.io/2020/3/1/17091fb2cc7fb8b7?w=853&h=275&f=png&s=69214)](https://github.com/any86/any-touch)

## 🔥typescript系列课程
如果你对ts感兴趣了, 欢迎看看我的ts基础教程.

[第一课, 体验typescript](https://juejin.im/post/5d19ad6de51d451063431864)

[第二课, 基础类型和入门高级类型](https://juejin.im/post/5d1af3426fb9a07ed4411a9b)

[第三课, 泛型](https://juejin.im/post/5d27f160e51d45108223fcf9)

[第四课, 解读高级类型](https://juejin.im/post/5d3fe80fe51d456206115987)

[第五课, 命名空间(namespace)是什么](https://juejin.im/post/5d5d04dfe51d4561af16dd24)

[特别篇, 在vue3🔥源码中学会typescript🦕 - "is"](https://juejin.im/post/5da6d1aae51d4524ad10d1d8)

[第六课, 什么是声明文件(declare)? 🦕 - 全局声明篇](https://juejin.im/post/5dcbc9e2e51d451bcb39f123)

[新手前端学🔥typescript - 实战篇, 实现浏览器全屏(59行)](https://juejin.im/post/5dd33ce3e51d453fbf29e0e5)

## 微信群
感谢大家的阅读, 如有疑问可以加我微信, 我拉你进入**微信群**(由于腾讯对微信群的100人限制, 超过100人后必须由群成员拉入)

![](https://user-gold-cdn.xitu.io/2019/9/19/16d474d245b69492?w=512&h=512&f=jpeg&s=27137)