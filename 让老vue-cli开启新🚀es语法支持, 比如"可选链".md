## 本文能学到什么?
1. 让老项目(基于vue-cli)支持ES新语法(处于试验阶段), 比如"可选链".
2. 了解其他目前在实验阶段的ES新语法.

## 可选链
**近期看到多个群中都在聊"可选链"**, 所以就把单位的老项目也开启了"可选链"功能, 使用了1个月后的感受就是: **再也不用写那么长的"undefined"判断了, 可选链"真香".**

![](https://user-gold-cdn.xitu.io/2020/4/28/171bf4422ab6da37?w=264&h=247&f=png&s=175888)


```javascript
const obj = {
  foo: {
    bar: {
      baz: 42,
    },
  },
};

const a = obj?.a; // undefined, 如果没有"?"可就报错喽
// 等价于
// const a = (null === obj || undefined === obj) ? undefined : obj.a;
const baz = obj?.foo?.bar?.baz; // 42
const baz = obj?.['foo']?.bar?.baz // 42
```
**注意**: 
1. 近期发布的vue-cli3已经默认支持"可选链", 大家可以先试下是否支持再安装.
2. 使用ts的小伙伴, 如果使用的是3.7以后的版本, 那么默认也支持"可选链".

## 安装"ES新特性", 需要vue-cli3

### 第一步
```
yarn add -D @babel/plugin-proposal-optional-chaining
```

### 第二步
项目根目录下找到"babel.config.js"文件, 修改"presets"字段:
```javascript
module.exports = {
    presets: [
        '@vue/cli-plugin-babel/preset'
    ],
    plugins:[
        // 可选链插件, 其他babel插件也是一样的安装方式
        "@babel/plugin-proposal-optional-chaining"
    ]
}
```

## 其他可玩的ES新特性(实验阶段)
通过babel的官网, 我们可以看到babel支持的"ES新特性"
参考: https://babeljs.io/docs/en/plugins#experimental

![](https://user-gold-cdn.xitu.io/2020/4/28/171bf5616b509017?w=358&h=496&f=png&s=37764)

挑几个有意思的说明下, 其他的大家可以自行看下[官网说明](https://babeljs.io/docs/en/plugins#experimental):

### @babel/plugin-proposal-nullish-coalescing-operator
"非undefined且非null"判断

```javascript
var object1 = {}
var foo = object1.foo ?? "default"; // "default"

var nl = null;
var res = nl ?? 1 // 2
```

### @babel/plugin-proposal-logical-assignment-operators
短路符判断后赋值的简写.
```javascript
let a = false;
a ||= 1; // 1
```
编译后的代码是这样的:
```javascript
var a = false;
a || (a = 1);
```



### @babel/plugin-proposal-function-bind
用"::"符号来代替"bind", "call"语法.

```javascript
obj::func
// 等价 func.bind(obj)

::obj.func
// 等价 obj.func.bind(obj)

obj::func(val)
// 等价 func.call(obj, val)

::obj.func(val)
// 等价 obj.func.call(obj, val)

$('.some-link').on('click', ::view.reset);
// 等价  $('.some-link').on('click', view.reset.bind(view));
```
复杂点的例子:
```javascript
const { map, filter } = Array.prototype;

let sslUrls = document.querySelectorAll('a')
                ::map(node => node.href)
                ::filter(href => href.substring(0, 5) === 'https');

```


### @babel/plugin-proposal-partial-application
函数科里化
```javascript
function add(x, y) { return x + y; }
const addOne = add(1, ?); // 返回函数addOne
addOne(2); // 3
```


### @babel/plugin-proposal-private-methods
私有属性关键词"#"

``` javascript
class Counter extends HTMLElement {
  #xValue = 0;

  get #x() { return this.#xValue; }
  set #x(value) {
    this.#xValue = value;
    window.requestAnimationFrame(
      this.#render.bind(this));
  }

  #clicked() {
    this.#x++;
  }
}
```

### 其他特性
其他特性可能在业务代码中不常用(大神们可能常用, 但是大神也不用看我写文章学这些😁)就不在此介绍了, 有兴趣大家可以看下[bebal实验特性](https://babeljs.io/docs/en/plugins#experimental).

## 总结
其实就这里最实用的就是"可选链"功能, 大家快快开始用起来吧, 让老项目的代码更优雅, 加油💪.



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