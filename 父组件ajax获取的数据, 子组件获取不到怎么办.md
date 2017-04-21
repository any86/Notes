# 父组件ajax获取的数据, 子组件怎么获取不到怎么办


### 为啥获取不到?
先看个例子
```javascript
// parent.vue
<template>
	<son :data="data"></son>
</template>

<script>
export default {
  mounted: {
  	xhr.get('/api').then(res=>{
		this.data = res;
  	});
  },

  data(){
  	return {data: []}
  }
}
初看没毛病吧, 就是简单的父给子传数据, 但是其实如果ajax请求慢的话, 子组件会渲染失败, 因为渲染子组件的时候data还没获取到呢.

</script>
```

### 解决
只需要用下`v-if`问题就解决了
```javascript
// parent.vue
<template>
	<son v-if="0 < data.length" :data="data"></son>
	<loading v-else></loading>
</template>
```
这样只有数据真的后去到的时候, 子组件才会去渲染, 也就不会报错了, 建一个价格loading, 体验更好.
