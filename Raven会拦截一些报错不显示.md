# Raven会拦截一些报错不显示


### 情况
```javascript
var obj = {};
console.log(obj.a.c);  // 不报错, 错误信息只发送给Raven后台

```
