# 🚀揭秘vue/react组件库中🤚5个"作者不造的轮子"
:rocket: 这五个轮子其实是5个纯js实现的插件, 都非常优秀, 下面一一给大家揭秘.

## async-validator(数据验证工具)
默认集成了**url**和**email**验证, 支持**异步验证**. **element-ui和iview**的表单组件都是用他实现的验证功能.

![](https://ws1.sinaimg.cn/large/005IQkzXly1g7anm9gh8jj30k107a74s.jpg)

```javascript
import schema from 'async-validator';
// 监视对象'name'字段的值是否等于muji, 且必须存在
var descriptor = {
  name: {
    type: "string",
    required: true,
    validator: (rule, value) => value === 'muji',
  }
};
var validator = new schema(descriptor);
validator.validate({name: "muji"}, (errors, fields) => {
  if(errors) {
    return handleErrors(errors, fields);
  }
});
```
https://github.com/yiminghe/async-validator

**补充**: 看了作者的github, 作者应该是阿里的员工, 而且也是ant design的代码维护者.

## moment | day.js(操作时间)

ant design在[DatePicker](https://github.com/ant-design/ant-design/blob/master/components/date-picker/createPicker.tsx#L2)组件中用了[moment](https://github.com/moment/moment).

![](https://ws1.sinaimg.cn/large/005IQkzXly1g7annipwzzj308a09pwew.jpg)

[moment](https://github.com/moment/moment)由于历史兼容原因体积比较大, 现在建议大家用[day.js](https://github.com/iamkun/dayjs)代替他, 两者语法相似.
```javascript
dayjs('2018-08-08') // 解析字符串

dayjs().format('{YYYY} MM-DDTHH:mm:ss SSS [Z] A') // 格式化日期

dayjs().add(1, 'year') // 当前年份增加一年

dayjs().isBefore(dayjs()) // 比较
```

## popover(气泡对话框)
**element-ui和iview**的[tooltip和popover](https://github.com/ElemeFE/element/blob/dev/packages/popover/src/main.vue#L25)组件都是基于vue-popover实现的, 而vue-popover只是对[popper](https://github.com/FezVrasta/popper.js)做了一层vue的封装, 所以气泡对话框的核心是[popper](https://github.com/FezVrasta/popper.js).

![](https://ws1.sinaimg.cn/large/005IQkzXly1g7anoo8bemj30ar05zglr.jpg)

```html
<div class="my-button">按钮</div>
<div class="my-popper">气泡菜单</div>
```

```javascript
var reference = document.querySelector('.my-button');
var popper = document.querySelector('.my-popper');
var popperInstance = new Popper(reference, popper, {
  // 更多设置
});
```

## autosize(让textarea随着文字换行而变化高度)
[vux的textarea](https://github.com/airyland/vux/blob/v2/src/components/x-textarea/index.vue#L41)用autosize让textarea组件随着输入文字换行而变化高度.

![](https://user-gold-cdn.xitu.io/2019/9/24/16d62490fb12c67e?w=270&h=175&f=gif&s=1777983)

```javascript
// 就一行, 就实现了"textarea随着输入文字换行而变化高度"
autosize(document.querySelector('textarea'));
```


## resize-observer-polyfill(Resize Observer API的兼容补丁)
基本所有的ui组件库都在用, 让低版本浏览器也支持Resize Observer API, 这样我们可以放心的监视元素的尺寸变化.

![](https://ws1.sinaimg.cn/large/005IQkzXly1g7ampq4uj7j30xw0bdq4p.jpg)
```javascript
import ResizeObserver from 'resize-observer-polyfill';
const ro = new ResizeObserver((entries, observer) => {
    for (const entry of entries) {
        const {left, top, width, height} = entry.contentRect;
        console.log('Element:', entry.target);
        console.log(`Element's size: ${ width }px x ${ height }px`);
        console.log(`Element's paddings: ${ top }px ; ${ left }px`);
    }
});
ro.observe(document.body);
```


## 最后
学习了很多组件库的源码, 基于对写代码的热情, 我用ts写了2个小插件, 抽象了一些组件中重复的代码, 大家看下是否需要.

### any-touch 
[👋一个手势库](https://github.com/any86/any-touch), 支持**tap**(点击) / **press**(按) / **pan**(拖拽) / **swipe**(划) / **pinch**(捏合) / **rotate**(旋转) 6大类手势, 同时支持鼠标和触屏.

```javascript
import AnyTouch from "any-touch";
const el = doucument.getElementById("box");
const at = new AnyTouch(el);

at.on("pan", ev => {
  // 拖拽触发.
});
```

#### tap(点击)
用来解决移动端"click的300ms延迟问题", 同时通过设置支持"双击"事件.

### press(按)
用来触发自定义菜单.

#### pan(拖拽)
这应该是组件库中最常用的手势, carousel(轮播) / drawer(抽屉) / scroll(滑动) / tabs(标签页)等都需要"拖拽识别"

#### swipe(滑)
carousel/tabs的切换下一幅, scroll的快速滑动等.

#### pinch(捏合)  / rotate(旋转)
pinch用来缩放商品图片, rotate一般用在高级定制化功能呢, 比如对图片(商品)刻字后旋转文字.

[:rocket: 更多说明: https://github.com/any86/any-touch](https://github.com/any86/any-touch)




### vue-create-root
🍭 不到1kb的小工具, 把vue组件变成this.$xxx这样的命令.
```javascript
// main.js
Vue.use(createRoot);

// xxx.vue
import UCom from '../UCom.vue';
{
    mounted(){
        // 默认组件被插入到<body>尾部
        this.$createRoot(UCom, {props: {value:'hello vue!'}});
        // 或者简写为:
        this.$createRoot(UCom, {value:'hello vue!'});
    }
}
```
[:rocket: 更多说明: https://github.com/any86/vue-create-root](https://github.com/any86/vue-create-root)


## 微信群
感谢大家的阅读, 如有疑问可以加**qq**群🚀

![](https://user-gold-cdn.xitu.io/2019/9/19/16d473b3f57615d9?w=540&h=740&f=jpeg&s=54564)

也可加我微信, 我拉你进入**微信群**(由于腾讯对微信群的100人限制, 超过100人后必须由我拉进去)

![](https://user-gold-cdn.xitu.io/2019/9/19/16d474d245b69492?w=512&h=512&f=jpeg&s=27137)
