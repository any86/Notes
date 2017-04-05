# spa商城前端架构

### 目录结构
```
[cdn]
docs/ -- 文档
static/
store/
src/
    assets/
    componets/
        layout/
            header/
            shoppingCart/
            footer/
            list/
        common/
            checkbox.vue
            dropdown.vue
            ...      
    views/
        index/
        list/
        detail/
        ...
    mock/
    router/
        module/
            shoppingCart/
                index.js
                ...
        action.js
        index.js
        ...
    scss/
    store/
test/
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
