# spa商城前端架构

### 目录结构

```
[cdn]
docs/ -- 文档
static/ -- 一些不支持npm的sdk
store/ -- vuex
    module/
        shoppingCart/
            index.js
            ...
    action.js -- 所有的异步请求都封装在这个文件
    index.js
    ...
src/
    assets/ -- 静态资源, 比如icon/font
    componets/ -- 抽象出的公共组件
        layout/ -- 耦合业务的组件(布局)
            header/
                index.vue
                noNav.vue
                ...
            shoppingCart/
                index.vue
                ...
            footer/
                ...
            list/
                ...
        common/ -- 不耦合业务的组件
            checkbox.vue
            dropdown.vue
            ...      
    views/ -- 整合组件到页面的父组件, 文件夹下面都是只于当前页面耦合的组件
        index/
            favour.vue
            banner.vue
            slider.vue
        list/
            ...
        detail/
            ...
        ...
    mock/ -- 假数据的json文件
        list.json
        ...
    router/ -- 路由配置文件
        index.js
        module/
            list.js
            shoppingCart.js
    scss/ -- 全局样式和sass变量
        theme.scss
        reset.scss
test/ -- 单元测试(看最终工期安排, 时间充足就进行)
    spc/
...
```


```
### 开发阶段后端接口支持跨域
```php
<?php
header("Access-Control-Allow-Origin: *");
header("Access-Control-Allow-Methods: PUT,POST,GET,DELETE,OPTIONS");
print_r($_REQUEST);
?>
```
