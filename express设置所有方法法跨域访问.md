# express设置所有方法法跨域访问

### 先上个小例子,大家运行体验下效果
```Javascript
var fs = require('fs')
var express = require('express')
var app = express()

app.all('*', function(req, res, next) {
    res.header("Access-Control-Allow-Origin", "*");
    res.header("Access-Control-Allow-Methods", "PUT,POST,GET,DELETE,OPTIONS");
    next();
});

var method = 'put'
app[method]('/mock/success', function(req, res) {
    var data = {
        "status": 1,
        "message": method + '成功!'
    };
    var json = JSON.stringify(data, null, 4);
    res.send(json);
});

app.listen(9000, function(err) {
    if (err) {
        console.log(err)
        return
    }
})
```

## 核心代码在这里
```javascript
app.all('*', function(req, res, next) {
    res.header("Access-Control-Allow-Origin", "*");
    res.header("Access-Control-Allow-Methods", "PUT,POST,GET,DELETE,OPTIONS");
    next();
});
```