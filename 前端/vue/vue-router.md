# vue-router

## 路由传参

```javascript
this.$router.push("home"); // 页面跳转

// 1 -- 
this.$router.push({ name: 'news', params: { userId: 123 }}); // 传参
this.$router.params.userId; // 获取参数
// 地址栏不会带 ?参数 这种，并且页面刷新后参数不存在
/*
	name 对应的是路由配置中的 name
	{
		path:'/path',
		name:'name',
		component:'cName'
	}
*/


// 2 -- 
this.$router.push({ path: '/news', query: { userId: 123 }}); // 传参
this.$router.query.userId; // 获取参数
// 地址栏带有 ?参数 这种，并且页面刷新后参数依旧存在
/*
	path 对应的是路由配置中的 path
	{
		path:'/path',
		name:'name',
		component:'cName'
	}
*/
```

```javascript
// 页面跳转
<router-link to="news">编译后就是个 a 标签 </router-link>


// params 传参  规则同上
<router-link :to="{ name: 'news', params: { userId: 1111}}">click to news page</router-link>

// query 传参  规则同上
<router-link :to="{ path: '/news', query: { userId: 1111}}">click to news page</router-link>
```

