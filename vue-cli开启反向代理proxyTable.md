# vue-cli开启反向代理(proxyTable)

### 有啥用
设置后, npn run dev阶段, 本地如果访问'/get/apple, 本地服务器会帮你访问http://api.com:6688/get/apple拿到远程的数据, 变相的实现了**跨域**功能

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
