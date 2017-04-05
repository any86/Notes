# spa商城前端架构

### 目录结构
```
[cdn]
docs/
static/
store/
src/
    assets/
    componets/
        layout/
            shoppingCart/
        common/
            checkbox.vue
            dropdown.vue
            ...      
    views/
        index/
        detail/
        ...
    mock/
    router/
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
