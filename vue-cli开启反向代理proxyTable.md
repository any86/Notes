# vue-cli开启反向代理(proxyTable)

### 这玩意有啥用
添加proxyTable后, 本地如果访问'/get/apple, 那么代理服务器会自动访问http://api.com:6688/get/apple, 帮你拿到远程的数据, 变相的实现了**跨域**功能

### 打开config/index.js, 添加proxyTable属性
```Javascript
module.exports = {
    build: {...}
    dev: {
       	...
        proxyTable: {
	        '/': {
	                 target: 'http://api.com:6688',
	                 changeOrigin: true
	            }
        },
       	...
    }
}

```
行了, 再见.
