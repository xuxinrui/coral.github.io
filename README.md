前端架构师

#### *scss*

#### *TypeScript*

#### *vue网络请求*

### async、await

	document.getElementById('id').onclick= async()=>{
		let a = await alert(1)
		console.log(a)
	}
	//generator yield
	function* h(){
	    yield 'hello';
	    yield 'world';
	    return 'ok'
	} 
	let hh = h();
	console.log(hh.next())
	console.log(hh.next())
	console.log(hh.next())
### HTTP

#### 输入一个网址http全过程

```
1、对网址进行DNS域名解析得到IP地址
2、根据这个IP地址找到服务器，发起tcp三次握手
3、发起http请求
4、服务器响应http请求，浏览器得到数据
5、浏览器解析
6、浏览器渲染
7、关闭TCP
```



## 闭包

### 闭包基础代码

```javascript
	function a(){
		     var i=0;
		     function b(){
		         alert(++i);
		     }
		     return b;
		}
	var c = a();
	c();//外部的变量
```
### 闭包的理解

```
当我们需要在模块中定义一些变量，并希望这些变量一直保存在内存中但又不会 “污染” 全局的变量时，就可以用闭包来定义这个模块。
```

## 原型和原型链

### 原型的概念

```
每个对象都会在其内部初始化一个属性，就是prototype(原型)。通俗的说，原型就是一个模板，更准确的说是一个对象模板
```

### 原型链的概念

```javascript
实例对象通过__proto__找原型，再通过__proto__找原型的原型    最终找不到的时候是：null



//当我们访问一个对象的属性时，如果这个对象内部不存在这个属性，那么他就会去prototype里找这个属性，这个prototype又会有自己的prototype，于是就这样一直找下去，也就是我们平时所说的原型链的概念。

function a(){
	this.aname= 'aname';
}
function b(){
	this.bname= 'bname';
}
b.prototype = new a()

function c(){
	this.cname= 'cname';
}
c.prototype = new b()

console.log(new c().aname)//从才开始找，再找b，再找a
```

### 继承

### es6继承

```javascript
class a extends b{
    constructor(){
		super()
        this.name
    }
}

简单写法：
class a extends b{
   name
}
```



#### 原型链继承

```javascript
//父构造函数
function f(){
	this.fname = '爸爸'
}
//孩子构造函数
function c1(){
	this.c1name = 'c1孩子'
}
//孩子的原型上添加父亲的实例
c1.prototype = new f();
//将孩子实例一下，看看孩子里有没有继承爸爸
let c1s = new c1();
//打印c1s.fname，最后喊出了“爸爸”
```
#### call继承

```javascript
//父构造函数
function f(xx){
	this.fname = '爸爸'+xx;
}
//孩子构造函数
function c1(){
	//相当于f收了一个孩子，叫做c1【这里的this就是c1】
	f.call(this,'我修改了爸爸')
}
```

#### 原型继承

```javascript
//父构造函数
function Person(){
	this.name = '爸爸';
}
// 先封装一个函数容器，用来承载继承的原型和输出对象
function object() {
	function F() {}
	F.prototype = new Person();
	return new F();
}

object().name
```
#### 寄生组合继承【没时间看】

```javascript
//父构造函数
function Person(){
	this.name = '爸爸';
}// 寄生
function object(obj) {
	function F(){}
	F.prototype = obj;
	return new F();
}
// object是F实例的另一种表示方法
var obj = object(Person.prototype);
// obj实例（F实例）的原型继承了父类函数的原型
// 上述更像是原型链继承，只不过只继承了原型属性

// 组合
function Sub() {
	this.age = 100;
	Person.call(this); // 这个继承了父类构造函数的属性
} // 解决了组合式两次调用构造函数属性的特点

// 重点
Sub.prototype = obj;
console.log(Sub.prototype.constructor); // Person
obj.constructor = Sub; // 一定要修复实例
console.log(Sub.prototype.constructor); // Sub
var sub1 = new Sub();
// Sub实例就继承了构造函数属性，父类实例，object的函数属性
console.log(sub1.job); // frontend
console.log(sub1 instanceof Person); // true
```


## promise

		Promise 是异步编程的一种解决方案
### Promise对象有以下三个特点
		1、对象的状态不受外界影响
		2、一旦状态改变，就不会再变
		3、Promise也有一些缺点。首先，无法取消Promise，一旦新建它就会立即执行，无法中途取消。

### promise基本用法

```javascript
	const promise = new Promise(function(resolve, reject) {
	  // ... some code
	
	  if (/* 异步操作成功 */){
	    resolve(value);
	  } else {
	    reject(error);
	  }
	});
	//resolve函数的作用是，将Promise对象的状态从“未完成”变为“成功”
	//在异步操作成功时调用，并将异步操作的结果，【作为参数传递出去】
```
### Promise then
```javascript
	Promise实例生成以后，可以用then方法分别指定resolved状态和rejected状态的回调函数。
	const p = new Promise(function(resolve, reject) {
	  if(0){
			resolve(123)
		}else{
			reject(456)
		}
	}).then(
		res=>{},
		err=>{console.log(err)}
	)
	
	//备注：catch也可以
```
 ### promise的三个状态
		pending：等待状态，比如正在进行网络请求，或者定时器没有到时间。
		fulfill：满足状态，当我们主动回调了resolve时，就处于该状态，并且会回调.then()
		reject：拒绝状态，当我们主动回调了reject时，就处于该状态，并且会回调.catch()

 ### promise链式调用
```javascript
	//基础链式调用
	const p = new Promise(function(resolve, reject) {
			resolve(1)
	}).then(res=>{
			console.log(res);
			return Promise.resolve(res+"3")
		}).then(res=>{
			console.log(res);
			return Promise.resolve(res+"4")
		}).then(res=>{
			console.log(res);
			return Promise.resolve(res+"5")
		})
	//链式调用简写
	const p = new Promise(function(resolve, reject) {
			resolve(1)
	}).then(res=>{
			console.log(res);
			return res+"2"
		}).then(res=>{
			console.log(res);
			return res+"3"
		})
```
### promise all处理并发
```javascript
const p = new Promise(function(resolve, reject) {
			resolve(123)
		})
const q = new Promise(function(resolve, reject) {
			resolve(456)
		})

Promise.all([p, q]).then((res) => {
    console.log(res);
	//输出[123, 456]
});	
```

## vue脚手架
	1、安装node
	2、安装webpack ：npm install webpack -g
	3、安装vue cli
	
	? Check the features needed for your project: (Press <space> to select, <a> to toggle all, <i> to invert selection)
	>(* ) Babel //转码器，可以将ES6代码转为ES5代码，从而在现有环境执行。 
	( ) TypeScript// TypeScript是一个JavaScript（后缀.js）的超集（后缀.ts）包含并扩展了 JavaScript 的语法，
		需要被编译输出为 JavaScript在浏览器运行
	( ) Progressive Web App (PWA) Support// 渐进式Web应用程序
	(* ) Router // vue-router（vue路由）
	(* ) Vuex // vuex（vue的状态管理模式）
	( ) CSS Pre-processors // CSS 预处理器（如：less、sass）
	(* ) Linter / Formatter // 代码风格检查和格式化（如：ESlint）
	( ) Unit Testing // 单元测试（unit tests）
	( ) E2E Testing // e2e（end to end） 测试
	
	Use history mode for router? (Requires proper server setup for index fallback in production)  
	Vue-Router 利用了浏览器自身的hash 模式和 history 模式的特性来实现前端路由（通过调用浏览器提供的接口）。
	选n。这样打包出来丢到服务器上可以直接使用了，后期要用的话，也可以自己再开起来。
	选yes的话需要服务器那边再进行设置。
	hash:location.href   锚点 
	history：history.go


### 脚手架配置之
	如果在之后的开发中，你依然使用template，就需要选择Runtime-Compiler
	如果你之后的开发中，使用的是.vue文件夹开发，那么可以选择Runtime-only

### vue程序运行过程
	【template】---解析-->【ast抽象语法树】---编译--->【render函数】------>【虚拟dom】------->UI
	
	如果安装了vue-template-conpiler
	【render函数】------>【虚拟dom】------->UI
## render函数
```javascript
//普通用法
const vm = new Vue({
	el:"#app",
		//这里的参数h是一个方法
	render: (h) => {
		return h('h1',{class:"h"},['wobushi'])
	}
})
//嵌套：
const vm = new Vue({
	el:"#app",
	render: (h) => {
		return h('h1',{class:"h"},['wobushi'h('h2',[xuxinrui])])
	}
})
```


## 路由
### 路由基础

```javascript
<router-view></router-view>
<router-link  to="/login?id=10" tag="span">登录</router-link>
<router-link  to="/reg/11">注册</router-link>

router:new VueRouter({
	routes:[
		{path:'/',redirect:'/login'},//redirect重定向
		{path:'/login',component:login},
		{path:'/reg/:id',component:reg}
	]
	
})
```

###  $route 和$router
	route是路由信息对象，里面主要包含路由的一些基本信息，
	包括name、meta、path、hash、query、params、fullPath、matched、redirectedFrom
	
	router是VueRouter的实例，包含了一些路由的跳转方法，钩子函数等



### 路由的默认路径【redirect】
```javascript
const routes = [
	{
		path:'/',
		redirect:'/home'
		component:
	}
]
```

### 路由模式
```javascript
const router = new VueRouter({
  routes:r,
  mode:"history",
  linkActiveClass:""
})
```

### 路由代码跳转
```javascript
//有时候, 页面的跳转可能需要执行对应的JavaScript代码, 这个时候, 就可以使用触发事件的跳转方式了
<buttton @click="tiaozhuan"></botton>

mathods:{
	tiaozhuan(){
		this.$route.push('/home')
	}
}
```
### 动态路由
	在某些情况下，一个页面的path路径可能是不确定的，比如我们进入用户界面时，希望是如下的路径：
	/user/aaaa或/user/bbbb
	除了有前面的/user之外，后面还跟上了用户的ID
	这种path和Component的匹配关系，我们称之为动态路由(也是路由传递数据的一种方式)。
	
	1、格式
	[
		path:"/home/:id"
	]
	2、获取id
		{{$route.params.id}}
	3、传递
	<router-link to:"/home/123">点击跳转</router-link>

### 路由懒加载
```javascript
//amd写法
const Home = resolve => require(['../components/Home.vue'], resolve);
//es6写法
const Home = () => import('../components/Home.vue')
{
	path:'',
	components:Home
}	
```

### 嵌套路由
	实现嵌套*路由的两个步骤:
	1、创建对应的子组件, 并且在路由映射中配置对应的子路由.
	2、在组件内部使用<router-view>标签.

### 嵌套路由基础【children[]】
```javascript
{
	path: "/About",
	component: () => import("../views/About.vue"),
	children: [
		{
			//这个是设置路径，并且不需要加【/】
			path:'aboutA',
			component:aboutA,
		},
		{
			path:'aboutB',
			component:aboutB
		}
	]
}
```


### 路由导航守卫的【beforeEach】主要用来监听监听路由的进入和离开的.
		三个参数
		to: 即将要进入的目标的路由对象.
		from: 当前导航即将要离开的路由对象.
		next: 调用该方法后, 才能进入下一个钩子.
		
	router.beforeEach((to, from, next) => {
		to.xxx
	  next()
	})


​	
###	路由传参
	一、router-link路由导航
	
	父组件: 使用<router-link to = "/跳转路径/传入的参数"></router-link>
	
	例如：<router-link to="/a/123">routerlink传参</router-link>
	
	子组件: this.$route.params.num接收父组件传递过来的参数
	
	mounted () {
	  this.num = this.$route.params.num
	}
	路由配置:：{path: '/a/:num', name: A, component: A}
	
	地址栏中的显示:：http://localhost:8080/#/a/123
	
	二、调用$router.push实现路由传参
	
	父组件: 绑定点击事件，编写跳转代码
	
	<button @click="deliverParams(123)">push传参</button>
	  methods: {
	    deliverParams (id) {
	      this.$router.push({
	        path: `/d/${id}`
	      })
	    }
	  }
	子组件: this.$route.params.id接收父组件传递过来的参数
	
	mounted () {
	  this.id = this.$route.params.id
	}
	路由配置:：{path: '/d/:id', name: D, component: D}
	
	地址栏中的显示:：http://localhost:8080/#/d/123
	
	三、通过路由属性中的name匹配路由，再根据params传递参数
	
	父组件: 匹配路由配置好的属性名
	
	<button @click="deliverByName()">params传参</button>
	    deliverByName () {
	      this.$router.push({
	        name: 'B',
	        params: {
	          sometext: '一只羊出没'
	        }
	      })
	    }
	子组件:
	
	<template>
	  <div id="b">
	    This is page B!
	    <p>传入参数：{{this.$route.params.sometext}}</p>
	  </div>
	</template>
	路由配置: 路径后面不需要再加传入的参数，但是name必须和父组件中的name一致
	{path: '/b', name: 'B', component: B}
	
	地址栏中的显示: 可以看出地址栏不会带有传入的参数，且再次刷新页面后参数会丢失
	http://localhost:8080/#/b
	
	四、通过query来传递参数
	
	父组件:
	
	<button @click="deliverQuery()">query传参</button>
	    deliverQuery () {
	      this.$router.push({
	        path: '/c',
	        query: {
	          sometext: '这是小羊同学'
	        }
	      })
	    }
	子组件:
	
	<template>
	  <div id="C">
	    This is page C!
	    <p>这是父组件传入的数据: {{this.$route.query.sometext}}</p>
	  </div>
	</template>
	路由配置: 不需要做任何修改
	{path: '/c', name: 'C', component: C}

### VueRouter之query与params两种传参区别
	query语法：
	this.$router.push({path:"地址",query:{id:"123"}}); 这是传递参数
	this.$route.query.id； 这是接受参数
	params语法：
	this.$router.push({name:"地址",params:{id:"123"}}); 这是传递参数
	this.$route.params.id; 这是接受参数
	以上就是这两种方法得语法，那大家也能从中看出一点区别：
	1.首先就是写法得不同，query 得写法是 用 path 来编写传参地址，
		而 params 得写法是用 name 来编写传参地址
	2.接收方法不同， 一个用 query 来接收， 一个用 params 接收 ，总结就是谁发得 谁去接收
	3.query 在刷新页面得时候参数不会消失，而 params 刷新页面得时候会参数消失，可以考虑本地存储解决
	4.query 传得参数都是显示在url 地址栏当中，而 params 传参不会显示在地址栏

## vuex
### 起步
	export default new Vuex.Store({
	  state: {数据},
	  mutations: {修改数据},
	  actions: {异步操作},
	  modules: {模块},
	  getters:{计算}
	})
### vuex常见存储内容
	比如用户的登录状态、用户名称、头像、地理位置信息等等。 
	比如商品的收藏、购物车中的物品等等。
### vuex获取状态state
	<h2>{{$store.state.name}}</h2>
	$store: 表示定义的一个仓库
	state:表示状态
	name：state里的一个数据


### vuex【mutations状态更新】修改state  
### !!!mutations里不能写异步代码
	1、在mutations里修改
	mutations: {
		notshin(state){
		state.name = "我不是徐欣瑞"
		}
	}
	2、触发修改事件
	methods:{
		change(){
		this.$store.commit('notshin')
		}
	}
### vuex【mutations状态更新】 修改state 【带参数】  
	1、在mutations里修改
	mutations: {
		notshin(state,str){
			state.name = "我不是徐欣瑞"+str
		}
	}
	2、触发修改事件
	methods:{
		change(str){
			this.$store.commit('notshin',str)
		}
	}
### vuex【mutations状态更新】 修改state 完整版
	methods:{
		change(str){
			this.$store.commit({
				type:'notshin',    //mutations里的方法
				str:"imnot"
			})
		}
	}
	mutations: {
		notshin(state,playload){
			state.name = "我不是徐欣瑞"+playload.str
		}
	}
### vuex计算属性getters  
	//计算属性里可以写过滤
	1、计算
	getters:{
		allname(state){
		return "imnot" +state.name 
		}
	}
	2、获取
	<h3>{{$store.getters.allname}}</h3>
### vuex计算属性getters  【携带参数】
	//计算属性里可以写过滤
	1、计算
	getters:{
		allname(state){
			return function(str){
				return str +state.name 
			}
		}
	}
	2、获取
	<h3>{{$store.getters.allname(str)}}</h3>

### vuex中的异步操作actions 【such as change state】
	1、vuex的actions
	actions:{
		change(context,str){
			settimeout(function(){
				context.$store.commit({type:'notshin', str})
			},1000)
		}
	}
	2、调用
	this.$store.dispatch('change',str)

### vuex的模块modules






### 父子访问refs  roots
	*父访问子：refs
	this.$refs.r1.func();
	r1代表：<c ref="r1"></c>   选中ref="r1"的子组件
	func()代表：访问到的子元素的其中的一个方法
	
	*子访问父
	this.$root.$data.b
	$parent

#### 父组件模板的所有东西都会在父级作用域内编译；
#### 子组件模板的所有东西都会在子级作用域内编译
#### 如果内容在子组件，希望父组件告诉我们如何展示
#### 利用slot作用域插槽就可以了


### 插槽slot
	【组件一般在实例化后，实例的标签内默认不能再添加元素。如有需要，那么需要加入slot】
	<body>
		<div id="app">
			<hl>
				<span slot="bb">44</span>
			</hl>
			
			<hl>
				<template slot-scope="ss">
					<li v-for="j in ss.data">{{j}}我不是</li>
				</template>
			</hl>
		</div>
		<template id="t1">
			<div>
				<p>我不是</p>
				<slot name="aa"><span>11</span></slot>
				<slot name="bb"><span>22</span></slot>
				<slot name="cc"><span>33</span></slot>


​				
​				<slot :data="names">
​					<ul>
​						<li v-for="i in names">{{i}}</li>
​					</ul>
​				</slot>
​			</div>
​		</template>
​	</body>
​	
​	<script type="text/javascript">
​		let vm  = new Vue({
​			el:"#app",
​			components:{
​				hl:{
​					template:"#t1",
​					data(){
​						return{
​							names:['武器','佐伊','妖姬']
​						}
​					}
​				}
​			}
​		})
​	</script>

### Object.defineProperty 
	Object.defineProperty 方法会直接在一个对象上定义一个新属性，或者修改一个对象的现有属性，并返回此对象。
	let obj = {}
	Object.defineProperty(obj,'name',{
		get(){
			//当访问该属性时，会调用此函数。
			//return obj;
		},
		set(oldv){
			//当属性值被修改时，会调用此函数
			console.log(newv);
		}
	})

### 动态组件
	<div id="app">
		<div>
		    <component :is="'kkkkk'"></component> 
		</div>
	</div>
	<script>
		let jjjjj = { template:'<p>组件jjjjj</p>'}
		let kkkkk = { template:'<p>组件kkkkk</p>'}
	    let vm = new Vue({
	        el:'#app',
	        components:{jjjjj,kkkkk}
		})
	</script>

### 谈谈你对MVVM开发模式的理解
	View层：
	视图层
	在我们前端开发中，通常就是DOM层。
	主要的作用是给用户展示各种信息。
	Model层：
	数据层
	数据可能是我们固定的死数据，更多的是来自我们服务器，从网络上请求下来的数据。
	在我们计数器的案例中，就是后面抽取出来的obj，当然，里面的数据可能没有这么简单。
	VueModel层：
	视图模型层
	视图模型层是View和Model沟通的桥梁。
	一方面它实现了Data Binding，也就是数据绑定，将Model的改变实时的反应到View中
	一方面它实现了DOM Listener，也就是DOM监听，当DOM发生一些事件(点击、滚动、touch等)时，可以监听到，并在需要的情况下改变对应的Data。

### 数组方法filter【过滤】
	const nums=[33,22,77,123,667,2,273]
	let nnums = nums.filter(function(n){
		return n < 100
	})
	console.log(nnums)  //[33, 22, 77, 2]

### 数组方法map【遍历】
	const nums=[33,22,77,123,667,2,273]
	let a = nums.map(function(n){
		return n * 10
	})
	console.log() 
### 数组方法reduce【汇总】
	const nums=[33,22,77,123,667,2,273]
	let a = nums.reduce(function(prev,v){
		return prev + v
	},0)




### 异步组件
	const Foo = () => Promise.resolve({
	  template: '<div>hello vue !</div>'
	})

### 什么是渐进式框架
	渐进式意味着可以将VUE作为应用的一部分嵌入其中


​	
### ES6导入导出	
	import r from('/r')
	export default r

### v-once 
	只渲染一次，不会随着数据的改变而改变
	<h1 v-once>{{}}</h1>
### v-html
	相比v-text可以解析标签
	<h1 v-html=""></h1>
### v-pre
	原封不动得显示【不解析】
	<h1 v-pre>{{}}</h1>
### v-cloak解决闪烁
	在vue解析之前，div中有一个属性v-cloak
	在vue解析之后，div中没有v-cloak
	[v-cloak]{display:none;}
	<h1 v-cloak>{{}}</h1>


​	
### v-bind:绑定属性

### 类绑定
	<div v-bind:class="{ 类名: 布尔值 }"></div>
	<div v-bind:class="[ 类名 ]"></div>
### 属性绑定
	<div v-bind:style="{ css属性名: css属性值 }"></div>
	<div v-bind:style="[ {,} ]"></div>

### 什么场景使用计算属性
	计算属性一般没有set方法，只读属性
	computed: 每次调用的时候，如果里面的数据没有发生变化，将不会再次执行这个函数

### 计算属性methods、computed
	相较于methods，不管依赖的数据变不变，methods都会重新计算，但是依赖数据不变的时候computed从缓存中获取，不会重新计算。
	每次使用时，methods里的函数都会调用

### computed和watch有什么区别

	computed
	computed是计算属性，也就是计算值，它更多用于计算值的场景
	computed具有缓存性，computed的值在getter执行后是会缓存的，只有在它依赖的属性值改变之后，
		下一次获取computed的值时重新调用对应的getter来计算
	computed适用于计算比较消耗性能的计算场景
	
	watch
	watch更多的是[观察]的作用，类似于某些数据的监听回调，用于观察props $emit或者本组件的值，当数据变化时来执行回调进行后续操作
	无缓存性，页面重新渲染时值不变化也会执行
	
	使用场景
	当我们要进行数值计算，而且依赖于其他数据，那么把这个数据设计为computed
	如果你需要在某个数据变化时做一些事情，使用watch来观察这个数据变化。

### . computed 和 watch 有什么区别及运用场景?

	区别
	computed 计算属性 : 依赖其它属性值,并且 computed 的值有缓存,只有它依赖的属性值发生改变,
		下一次获取 computed 的值时才会重新计算 computed 的值。
	watch 侦听器 : 更多的是「观察」的作用,无缓存性,类似于某些数据的监听回调,每当监听的数据变化时都会执行回调进行后续操作。
	运用场景
	当我们需要进行数值计算,并且依赖于其它数据时,
	应该使用 computed,因为可以利用 computed 的缓存特性,避免每次获取值时,都要重新计算。
	当我们需要在数据变化时执行异步或开销较大的操作时,
	应该使用 watch,使用 watch 选项允许我们执行异步操作 ( 访问一个 API ),限制我们执行该操作的频率,
	并在我们得到最终结果前,设置中间状态。这些都是计算属性无法做到的。


​	
​	
​	
​	
​	
### 对象增强写法
	let obj = {
		name,
		age,
		run(){}
	}
	等于
	let obj = {
		name:name,
		age:age,
		run:function(){}
	}

### v-on:传参
	<div v-on:click="run(''.$event)"></div>
### 事件修饰
	.stop阻止冒泡：       <div v-on:click.stop="run()"></div>
	.prevent阻止默认事件：<div v-on:click.prevent="run()"></div>
	.once只出发一次：     <div v-on:click.once="run()"></div>
### 事件冒泡
	点击子元素的时候，如果父元素也有事件，那么两个地方的事件都会触发。
	当子元素添加了.stop的时候，就不会同时触发父级的事件

###  v-if-else中的key

	 dom出于性能的考虑，会尽可能复用已经存在的元素，而不是创建新的元素，这个时候就需要KEY
	 这个KEY给每个节点做了唯一的标识，这样vue会重新渲染
### v-for中key的作用主要是为了高效的更新虚拟DOM。  每个数据拥有唯一的标识

### 数组splice()

	arrayObject.splice(index,howmany,item1,.....,itemX)
	index	必需。整数，规定添加/删除项目的位置，使用负数可从数组结尾处规定位置。
	howmany	必需。要删除的项目数量。如果设置为 0，则不会删除项目。
	item1, ..., itemX	可选。向数组添加的新项目。

### set()修改数据
	//set(要修改的对象，索引值，修改后的值)
	Vue.set(this.arr,1,'')

### filters过滤器
	<div>{{name | notnumber}}</div>
	-------------------------------
	filters:{
		notnumber(n){
			return n.xxxxxxxx;
		}
	}


​	
### 数组多方法串联【过滤+遍历+汇总】
	let newnums = nums.filter(function(n){
		return n < 100
	}).map(function(n){
		return n * 10
	}).reduce(function(prev,v){
		return prev + v
	},0)


​	

​		

#### 样式绑定的区别   [值] {blooean} 

	<div v-bind:class="{ active: isActive }"></div>
	<div v-bind:class="[activeClass, errorClass]"></div>
	<div v-bind:style="{ color: activeColor, fontSize: fontSize + 'px' }">菜鸟教程</div>
	<div v-bind:style="[baseStyles, overridingStyles]">菜鸟教程</div>
#### 混入 (mixins)(created) 定义了一部分可复用的方法或者计算属性

	var mixin = {
		created: function () {
			document.write('混入调用' + '<br>')
		}
	}
	new Vue({
		mixins: [mixin],
			created: function () {
			document.write('组件调用' + '<br>')
		}
	});


​	
#### this.$emit('')可以触发一个自定义的事件  【!!!不是函数】

	1、子组件内创建一个方法：sendmsg();
	2、子组件的sendmsg()方法内创建自定义方法this.$emit('函数名',参数)
		创建出来的自定义事件func在实例中调用
	3、子组件实例化中调用自定义方法，并在这个方法中调用父组件的方法
	4、父组件的方法中getMsgFromSon(data)，data就是子组件通过emit传过来的参数
	
	<div id="app"><com  @func="getMsgFromSon"></com></div>
	<template id="tmp1">
		<div id="">
			<button type="button" @click="sendmsg">put</button>
		</div>
	</template>
	var com = {
		template:"#tmp1",
		data(){
			return{
				msg:"我还是个孩子"
			}
		},
		methods:{
			sendmsg(){
				this.$emit('func',this.msg)
			}
		}
	}

### 组件props

	<div id="app">
		<child v-bind:props='fmsg'></child>
	</div>
	
	<template id="t1"></template>？？
	
	<script type="text/javascript">
		var vm = new Vue({
			el:'#app',
			data:{
				fmsg:'我是你爸爸'
			},
			components:{
				'child':{
					props:['props'],
					template: '<h1>{{props}}</h1>'
				}
			}
	
		})
	</script>

### diff算法

	diff算法的本质是找出两个对象之间的差异，目的是尽可能复用节点。
	diff 算法的本质是找出两个对象之间的差异
	diff 算法的核心是子节点数组对比,思路是通过 首尾两端对比
	key 的作用 主要是
	决定节点是否可以复用
	建立key-index的索引,主要是替代遍历，提升性能

### Object.defineProperty(对象名，属性名，属性)  创建对象

	Vue 则采用的是数据劫持与发布订阅相结合的方式实现双向绑定，数据劫持主要通过 Object.defineProperty 来实现。
	
	<div id="app">
	    <input type="text" id="txt">
	    <p id="show"></p>
	</div>
	
	<script type="text/javascript">
		
	    var obj = {}
	    Object.defineProperty(obj, 'txt', {
		get: function () {
		    return obj
		},
		set: function (newValue) {
		    //document.getElementById('txt').value = newValue
		    document.getElementById('show').innerHTML = newValue
		}
	    })
	    document.addEventListener('keyup', function (e) {
		obj.txt = e.target.value
	    })
	</script>	


### 虚拟 DOM 的实现原理主要包括以下 3 部分：

	用 JavaScript 对象模拟真实 DOM 树，对真实 DOM 进行抽象；
	diff 算法 — 比较两棵虚拟 DOM 树的差异；
	pach 算法 — 将两个虚拟 DOM 对象的差异应用到真正的 DOM 树	

### 现在 vue3.0 也全面改用 TypeScript 来重写

	3.0 将带来基于代理 Proxy 的 observer 实现，提供全语言覆盖的反应性跟踪。
	这消除了 Vue 2 当中基于 Object.defineProperty 的实现所存在的很多限制：
		1.只能监测属性，不能监测对象
		2.检测属性的添加和删除；
		3.检测数组索引和长度的变更；
		4.支持 Map、Set、WeakMap 和 WeakSet。

### v-once

	<span v-once>这个将不会改变: {{ msg }}</span>

### 怎样理解 Vue 的单向数据流？

	所有的 prop 都使得其父子 prop 之间形成了一个单向下行绑定：父级 prop 的更新会向下流动到子组件中，
	但是反过来则不行。这样会防止从子组件意外改变父级组件的状态，从而导致你的应用的数据流向难以理解。

### 生命周期
	beforeCreate（创建前）
		在数据观测和初始化事件还未开始,data、watcher、methods都还不存在，
		但是$route已存在，可以根据路由信息进行重定向等操作。
	created（创建后） 
		在实例创建之后被调用。
		该阶段可以访问data，使用watcher、events、methods，也就是说 
		数据观测(data observer) 和event/watcher 事件配置 已完成。
		但是此时dom还没有被挂载。该阶段允许执行http请求操作。
	beforeMount（载入前） 
		将HTML【类似组件也是组件】解析生成AST节点，再根据AST节点动态生成渲染函数。
		相关render函数首次被调用(划重点)。
			
	mounted（载入后）
		执行render函数生成虚拟dom，创建真实dom替换虚拟dom，并挂载到实例。可以操作dom，比如事件监听
	
	beforeUpdate（更新前） 在数据更新之前调用，发生在虚拟DOM重新渲染和打补丁之前。可以在该钩子中进一步地更改状态，
			不会触发附加的重渲染过程。
	updated（更新后） 在由于数据更改导致的虚拟DOM重新渲染和打补丁之后调用。
			调用时，组件DOM已经更新，所以可以执行依赖于DOM的操作。然而在大多数情况下，应该避免在此期间更改状态，
			因为这可能会导致更新无限循环。该钩子在服务器端渲染期间不被调用。
	beforeDestroy（销毁前） 在实例销毁之前调用。实例仍然完全可用。
	destroyed（销毁后） 在实例销毁之后调用。


​	
​		
### 在哪个生命周期内调用异步请求？

	可以在钩子函数 created、beforeMount、mounted 中进行调用，
	因为在这三个钩子函数中，data 已经创建，可以将服务端端返回的数据进行赋值。
	但是本人推荐在 created 钩子函数中调用异步请求

### vue等单页面应用及其优缺点

	优点：Vue 的目标是通过尽可能简单的 API 实现响应的数据绑定和组合的视图组件，核心是一个响应的数据绑定系统。
	MVVM、数据驱动、组件化、轻量、简洁、高效、快速、模块友好。
	
	缺点：不支持低版本的浏览器，最低只支持到IE9；
	不利于SEO的优化（如果要支持SEO，建议通过服务端来进行渲染组件）；
	第一次加载首页耗时相对长一些；
	不可以使用浏览器的导航按钮需要自行实现前进、后退。


​	
### v-on 绑定多个方法

	<p v-on="{click:dbClick,mousemove:MouseClick}"></p>

### v-on事件修饰符

	stop阻止单击事件冒泡
	prevent提交事件不再重载页面
	<a v-on:click.stop.prevent="alert_">修饰符可以串联</a>
	只有修饰符
	capture添加事件侦听器时使用事件捕获模式
	self只当事件在该元素本身（而不是子元素）触发时触发回调

### v-model的常用修饰符

	在添加了lazy之后，会把 oninput 事件改成 onchange 事件。
	trim的作用是过滤用户输入时首尾的空格字符。

### 全局过滤器filter

	<p>过滤器res：<span>{{string | res}}</span></p>
	
	Vue.filter('res',function(val){
		return val.split('').reverse().join('')
	})
### 局部过滤器filter

	filters:{
	    FormatDate:function(v1,v2,v3){
	        return v1+","+v2+","+v3
	    }
	}
### 监听$watch

	vm.$watch('change', function(nval, oval) {
	    alert( oval + ' 变为 ' + nval);
	});
### 监听watch

	watch : {
	        kilometers:function(val) {
	            this.kilometers = val;
	            this.meters = this.kilometers * 1000
	        }
	}
### 全局基础组件Vue.component

	<child></child>
	Vue.component('child', {
	  data: function () {
	    return {
	      count: 0
	    }
	  },
	  template: '<button v-on:click="count++">You clicked me {{ count }} times.</button>'
	})




### :todo循环组件数据

	<child v-for= "items in arr" :todo="items"></child>


​	
### 全局自定义指令Vue.directive

	<input type="text"  name="" id="" value="" v-focus />
	
	Vue.directive('focus',{
		//inserted: 一旦元素被添加到父元素时触发
		inserted:function(this_){
			this_.focus();
		}
	})
	
	bind: 一旦指令附加到元素时触发
	inserted: 一旦元素被添加到父元素时触发
	update: 每当元素本身更新(但是子元素还未更新)时触发
	componentUpdate: 每当组件和子组件被更新时触发
	unbind: 一旦指令被移除时触发。


​	
### 数组删除

	<tr v-for="(item,index) in arr_obj" :key="item.a">
		<td>{{item.a}}</td>
		<td>{{item.b}}</td>
		<td><button @click="dele_arr(item.a)" type="button">删除</button></td>
		<td><button @click="dele_arr_index(index)" type="button">index删除</button></td>
	</tr>
	
	----------------------------
	var index = this.arr_obj.findIndex(item=>{
		if(item.a ===id ){
			return true;
		}
	})
	
	this.arr_obj.splice(index,1)
	-----------------------------
	dele_arr_index(id){
		this.arr_obj.splice(id,1)
	}

### 获取dom   get($event)

	<div @click="get($event)"></div>
	
	------------------------------
	get(data){
		data.currentTarget.className='';
	}


​	
### 在 beforeDestroy 中销毁定时器




### 双向绑定原理

	当你把一个普通的 JavaScript 对象传入 Vue 实例作为 data 选项，Vue 将遍历此对象所有的 property，
	并使用 Object.defineProperty 把这些 property 全部转为 getter/setter。
	watcher
	默认Vue在初始化数据时，会给data中的属性使用Object.defineProperty重新定义所有属性，
	当页面到对应属性时，会进行依赖收集(收集当前组件中的watcher)如果属性发生变化会通知相关依赖进行更新操作

### vue和react的区别

	组件写法不一样, React推荐的做法是 JSX + inline style, 
	Vue推荐的做法是webpack+vue-loader的单文件组件格式,即html,css,jd写在同一个文件;

### 延迟回调$nextTick
	vue中的nextTick主要用于处理数据动态变化后，DOM还未及时更新的问题，用nextTick就可以获取数据更新后最新DOM的变化
	有些时候在改变数据后立即要对dom进行操作，此时获取到的dom仍是获取到的是数据刷新前的dom
	
	vue实现响应式并不是数据发生变化后dom立即变化，而是按照一定的策略来进行dom更新。
	$nextTick 是在下次 DOM 更新循环结束之后执行延迟回调，在修改数据之后使用 $nextTick，则可以在回调中获取更新后的 DOM
		适用场景：
		第一种：有时需要根据数据动态的为页面某些dom元素添加事件，这就要求在dom元素渲染完毕时去设置，
		但是created与mounted函数执行时一般dom并没有渲染完毕，所以就会出现获取不到，添加不了事件的问题，
		这回就要用到nextTick处理
		第二种：在使用某个第三方插件时 ，希望在vue生成的某些dom动态发生变化时重新应用该插件，
		也会用到该方法，这时候就需要在 $nextTick 的回调函数中执行重新应用插件的方法，例如:应用滚动插件better-scroll时
		
		mounted(){
			this.$nextTick(() => 
				//这里才可以
			})
		}


​	

#### 怎么解决vue动态设置img的src不生效的问题

	 logo:require("./../assets/images/logo.png")

#### Vue3.0你知道有哪些改进

	Vue3 中响应式数据原理改成proxy

#### 对 Vue 项目进行哪些优化？

	v-if 和 v-show 区分使用场景
	computed 和watch 区分使用场景
	v-for 遍历必须为 item 添加 key，且避免同时使用 v-if
	事件的销毁
	图片资源懒加载
	路由懒加载
	第三方插件的按需引入

#### Vue中v-html会导致哪些问题

	可能会导致xss攻击
	v-html会替换掉标签内部的子元素

#### Vue的渲染过程

	1、把模板编译为render函数
	2、实例进行挂载, 根据根节点render函数的调用，递归的生成虚拟dom
	3、对比虚拟dom，渲染到真实dom
	4、组件内部data发生变化，组件和子组件引用data作为props重新调用render函数，生成虚拟dom, 返回到步骤3


#### 组件data是函数的原因

	因为组件是用来复用的，JS里对象是引用关系，这样作用域没有隔离，而new Vue的实例，是不会被复用的，因此不存在引用对象问题


#### 如何比较React和Vue

	监听数据变化的实现原理不同：
	
	Vue通过getter/setter以及一些函数，能精确知道数据变化
	React默认是通过比较引用的方式(diff)进行的，React不精确监听数据变化
	数据流不同：
	
	Vue2.0可以通过props实现双向绑定，用vuex单向数据流的状态管理框架
	React不支持双向绑定，提倡单项数据流，Redux单向数据流的状态管理框架
	组件通信的区别：
	
	Vue三种组件通信方法：
	父组件通过props向子组件传递数据或回调
	子组件通过事件event向父组件发送数据或回调
	通过provide/inject实现父组件向子组件传入数据，可跨层级
	React三种组件通信方法：
	父组件通过props向子组件传递数据
	React不支持子组件像父组件发送数据，而使用的是回调函数
	通过 context实现父组件向子组件传入数据， 可跨层级
	模板渲染方式不同：
	
	表面上来看：
	React通过JSX渲染模板
	Vue通过HTML进行渲染
	深层上来看：
	React是通过原生JS实现模板中常见语法,如：插件，条件，循环
	Vue是与组件JS分离的单独模板，通过指令实现，如：v-if
	模板中使用的数据：
	
	React里模板中使用的数据可以直接import的组件在render中调用
	Vue里模板中使用的数据必须要在this上进行中转，还要import一个组件，还要在components中声明
	渲染过程不同：
	
	Vue不需要渲染整个组件树
	React状态改变时，全部子组件重新渲染
	框架本质不同：
	
	Vue本质是MVVM框架，由MVC发展而来
	React是前端组件化框架，由后端组件化发展而来
	Vuex和Redux的区别：
	
	Vuex可以使用dispatch、commit提交更新
	Redux只能用dispatch提交更新
	组合不同功能方式不同：
	
	Vue组合不同功能方式是通过mixin，可以帮我定义的模板进行编译、声明的props接收到数据….
	React组合不同功能方式是通过HoC(高阶组件)，本质是高阶函数	


​	

#### Vue中相同逻辑如何抽离

	使用混入
	Vue.mixin用法给组件每个生命周期，函数等都混入一些公共逻辑	

#### 为什么v-for和v-if不能连用

	当 v-for 和 v-if 处于同一个节点时，v-for 的优先级比 v-if 更高，这意味着 v-if 将分别重复运行于每个 v-for 循环中。
	如果要遍历的数组很大，而真正要展示的数据很少时，这将造成很大的性能浪费。 这种场景建议使用 computed，先对数据进行过滤


​	

​	


#### prop 验证，和默认值

	我们在父组件给子组件传值得时候，为了避免不必要的错误，可以给prop的值进行类型设定，
	让父组件给子组件传值得时候，更加准确，prop可以传一个数字，一个布尔值，一个数组，一个对象，
	以及一个对象的所有属性。组件可以为 props 指定验证要求。如果未指定验证要求，
	Vue 会发出警告比如传一个number类型的数据，用defalt设置它的默认值，如果验证失败的话就会发出警告。
	props: {
		visible: {
			default: true,
			type: Boolean,
			required: true
		}
	}

####  如何让CSS只在当前组件中起作用？
​	将当前组件的style修改为style scoped
​	

#### <keep-alive></keep-alive>的作用是什么？

	<keep-alive></keep-alive> 包裹动态组件时，会缓存不活动的组件实例,主要用于保留组件状态或避免重新渲染。

#### vue.js的两个核心是什么？

	数据驱动、组件系统


​	

### 网页从输入网址到渲染完成经历了哪些过程？

	输入网址；
	发送到DNS服务器，并获取域名对应的web服务器对应的ip地址；
	与web服务器建立TCP连接；
	浏览器向web服务器发送http请求；
	web服务器响应请求，并返回指定url的数据（或错误信息，或重定向的新的url地址）；
	浏览器下载web服务器返回的数据及解析html源文件；
	生成DOM树，解析css和js，渲染页面，直至显示完成；







### Vue子组件调用父组件的方法

	第一种方法是直接在子组件中通过this.$parent.event来调用父组件的方法
	第二种方法是在子组件里用$emit向父组件触发一个事件，父组件监听这个事件就行了。



### split('').reverse().join('')

### this 指向对象，作为普通函数调用的时候，就指向全局了

### console.log(array1.push.apply(array1, array2)); 数组合并

### 判断一个字符串中出现次数最多的字符，统计这个次数

```javascript
//将字符串转化数组
//创建一个对象
//遍历数组，判断对象中是否存在数组中的值，如果存在值 +1，不存在赋值为 1
//定义两个变量存储字符值，字符出现的字数
var str = 'abaasdffggghhjjkkgfddsssss3444343';
// 1.将字符串转换成数组
var newArr = str.split("");
// 2.创建一个对象
var json = {};
// 3. 所有字母出现的次数，判断对象中是否存在数组中的值，如果存在值 +1，不存在赋值为 1
for(var i = 0; i < newArr.length; i++){
// 类似：json : { ‘a’: 3, ’b’: 1 }
		  if(json[newArr[i]]){
			 json[newArr[i]] +=1;
		  } else {
			   json[newArr[i]] = 1;
		  }
	}
	// 4 定义两个变量存储字符值，字符出现的字数
	var num = 0 ; //次数
	var element = ""; //最多的项
	for(var k in json){
	   if(json[k] > num){
		 num = json[k];
		 element = k ;
	   }
	}
	console.log("出现次数："+num +"最多的字符："+ element);
```



### 常见的浏览器内核有哪些 ？

	Trident 内核：IE, 360，搜狗浏览器 MaxThon、TT、The World,等。[又称 MSHTML]
	Gecko 内核：火狐，FF，MozillaSuite / SeaMonkey 等
	Presto 内核：Opera7 及以上。[Opera 内核原为：Presto，现为：Blink]
	Webkit 内核：Safari，Chrome 等。 [ Chrome 的：Blink（WebKit 的分支）]

### 事件委托是什么

	答案: 利用事件冒泡的原理，让自己的所触发的事件，让他的父元素代替执行
### 如何阻止默认事件

	(1)return false；
	(2) ev.preventDefault();
### https

	https是HTTP运行在SSL/TLS之上，SSL/TLS运行在TCP之上

### 事件绑定

	btn4.addEventListener("click",hello1);

### 清除循环

	clearInterval();
### xss 跨站脚本攻击

### window.onload方法是在网页中所有的元素(包括元素的所有关联文件)

### 0.1+0.2 ===0.30000000000000004

### apply、call、bind

	apply 、 call 、bind 三者第一个参数都是 this 要指向的对象，也就是想指定的上下文；
	apply 、 call 、bind 三者都可以利用后续参数传参；
	bind 是返回对应 函数，便于稍后调用；apply 、call 则是立即调用 。
	三者都可以改变函数的this对象指向。
	三者第一个参数都是this要指向的对象，如果如果没有这个参数或参数为undefined或null，则默认指向全局window。
	三者都可以传参，但是apply是数组，而call是参数列表，且apply和call是一次性传入参数，而bind可以分为多次传入。
	bind 是返回绑定this之后的函数，便于稍后调用；apply 、call 则是立即执行 。
	apply(thisArg, [argsArray])
	call(thisArg, arg1, arg2, …)

### 盒子模型

	在IE盒子模型中，width表示content+padding+border这三个部分的宽度
	在标准的盒子模型中，width指content部分的宽度

### 水平居中

	行内元素: text-align: center
	块级元素: margin: 0 auto
	position:absolute +left:50%+ transform:translateX(-50%)
	display:flex + justify-content: center
### 垂直居中

	设置line-height 等于height
	position：absolute +top:50%+ transform:translateY(-50%)
	display:flex + align-items: center /*垂直对齐方式*/
	display:table+display:table-cell + vertical-align: middle;

### 

### 多行文本溢出

	p {
	  position: relative;
	  line-height: 1.5em;
	  /*高度为需要显示的行数*行高，比如这里我们显示两行，则为3*/
	  height: 3em;
	  overflow: hidden;
	  white-space:nowrap; 
		  text-overflow:ellipsis; 
	}

### display: grid

```css
.f {
    display: grid;
    grid-template-columns: 100px 200px auto;/*行布局*/
    grid-template-rows: repeat(3, 33.33%);/*列布局*/
    grid-row-gap: 20px;/*行间距*/
	grid-column-gap: 20px;/*列间距*/
    /*grid-gap*/
}

.f {
  display: grid;
  grid-template-columns: repeat(auto-fill, 100px);
    /*单元格的大小是固定的，但是容器的大小不确定。如果希望每一行（或每一列）容纳尽可能多的单元格，这时可以使用auto-fill关键字表示自动填充。*/
}

fr 关键字
/**/为了方便表示比例关系，网格布局提供了fr关键字（fraction 的缩写，意为"片段"）。如果两列的宽度分别为1fr和2fr，就表示后者是前者的两倍。
.f{
    display: grid;
    grid-template-columns: 1fr 2fr 1fr;
    grid-template-rows: repeat(3, 33.33%);
}

更多详情见：http://www.ruanyifeng.com/blog/2019/03/grid-layout-tutorial.html
```



### flex

```css
Webkit 内核的浏览器，必须加上 -webkit 前缀

.box{
  display: -webkit-flex; /* Safari */
  display: flex;
    
  /*主轴的方向*/
  flex-direction:flex-direction: row | row-reverse | column | column-reverse; 
  /*换行方式*/
  flex-wrap: nowrap | wrap | wrap-reverse;
  
  flex-flow 
  /*水平对齐方式*/
  justify-content: flex-start | flex-end | center | space-between | space-around;
  /*垂直对齐方式*/
  align-items: flex-start | flex-end | center | baseline | stretch;
  /*出现多行时的垂直对齐方式*/
  align-content: flex-start | flex-end | center | space-between | space-around | stretch;
}
	

order：子容器的排列顺序
flex-grow：子容器剩余空间的拉伸比例
flex-shrink：子容器超出空间的压缩比例
flex-basis：子容器在不伸缩情况下的原始尺寸
flex：子元素的 flex 属性是 flex-grow,flex-shrink 和 flex-basis 的简写
align-self
```

	.item {
	  
	  flex-grow: <number>; /* default 0 */  /*子元素放大比例*/
	}

### ajax

	function loadXMLDoc()
	{
		var xmlhttp='';
		if (window.XMLHttpRequest)
		{
			//  IE7+, Firefox, Chrome, Opera, Safari 浏览器执行代码
			xmlhttp=new XMLHttpRequest();
		}
		else
		{
			// IE6, IE5 浏览器执行代码
			xmlhttp=new ActiveXObject("Microsoft.XMLHTTP");
		}
		xmlhttp.onreadystatechange=function()
		{
			if (xmlhttp.readyState==4 && xmlhttp.status==200)
			{
				document.getElementById("myDiv").innerHTML=xmlhttp.responseText;
			}
		}
		xmlhttp.open("GET","/try/ajax/ajax_info.txt",true);
		xmlhttp.send();
	}



### 构造函数

	用于创建对象的函数，叫做构造函数。
	和普通函数不同的是，构造函数调用后会得到一个对象
	构造函数中的返回值一直都是对象
	相比于对象来说，构造函数可以携带参数


### 原型链

	实例对象通过 __proto__ 得到实例对象的原型对象
	构造函数通过 prototype 扩展属性
		//原型链关系  
		xiaohuang.__proto__ === Dog.prototype  
		Dog.prototype.__proto__ === Animal.prototype  
		Animal.prototype.__proto__ === Object.prototype  
		Object.prototype.__proto__ === null 

### 继承属性

	function Person (name, age) {
	    this.name = name
	    this.age = age
	}
	
	// 方法定义在构造函数的原型上
	Person.prototype.getName = function () { console.log(this.name)}
	
	function Teacher (name, age, subject) {
	    Person.call(this, name, age)
	    this.subject = subject
	}
### 继承方法 Object.create

	Teacher.prototype = Object.create(Person.prototype)
	Teacher.prototype.constructor = Teacher

### hasOwnProperty 查询属性

### 字符串方法

	indexOf() 方法返回字符串中指定文本首次出现的索引（位置）
	：提取字符串	
	slice(start, end) 提取字符串的某个部分并在新字符串中返回被提取的部分。
	substring(start, end) 同上，数字不能为负数
	substr(start, length)
	replace()
	charAt() 方法返回字符串中指定下标（位置）的字符串：
	split() 将字符串转换为数组：

### 数组去重

	if(新数组.indexOf(旧数组[i]) == -1){
	    新数组.push(旧数组[i]);
	}			

### 沉睡排序法

	var arr = [1,55,4,7,88,75,4];
	arr.forEach(function(num){
		setTimeout(function(){
			console.log(num)
		},num);
	})
### 冒泡排序

	let arr = ['445', 1223, 4590, 1, 3];
	function paixu(arrr) {
		var length = (arrr.length) - 1;
		for (var i = 0; i < length; i++) {
			for (var j = i + 1; j < length; j++) {
				if (arr[i] > arr[j]) {
					var temp = arr[i];
					arr[i] = arr[j];
					arr[j] = temp;
				}
			}
		}
		return arr;
	}
	console.log(paixu(arr));


​		
### 数组方法

	join() 方法也可将所有数组元素结合为一个字符串。
	pop() 方法从数组中删除最后一个元素：
	push() 方法（在数组结尾处）向数组添加一个新的元素：
	shift() 方法会删除首个数组元素，并把所有其他元素“位移”到更低的索引。
	unshift() 方法（在开头）向数组添加新元素，并“反向位移”旧元素：
	.数组length=0时清空
	splice(a, b, c);		
		第一个参数（a）定义了应添加新元素的位置（拼接）。
		第二个参数（b）定义应删除多少元素。
		c定义要添加的新元素。
	concat();

### 数组排序方法	sort（）

	通常按照字母顺序排序，如果想让数字按照数字大小排序，需要添加以下方法
	var arr = ['10','5','40','25','10000','1']
	function sortNumber(a,b){
		return a - b
	}
	arr.sort(sortNumber)


### 典型闭包

	函数内部可以访问函数外部的变量，反之不能访问。如果需要访问，则需要用到ruturn.
	ie无法回收闭包内变量的内存空间
	
	var arr = [1,2,3,4,5];
	for(var i = 0; i < arr.length; i++){
	  arr[i] = function(){
		alert(i)
	  }
	}
	arr[3]();
	打印出5  应为i在for里面不是私有变量 

#outline: none;

### let / const
		var是不受限于块级的，而let是受限于块级
			var只受函数封闭，let被所有块级封闭
	
		var声明变量可以重复声明，而let不可以重复声明
		var会与window相映射（会挂一个属性），而let不与window相映射
		var可以在声明的上面访问变量，而let有暂存死区，在声明的上面访问变量会报错
		const声明之后必须赋值，否则会报错const定义不可变的量，改变了就会报错
		const和let一样不会与window相映射、支持块级作用域、在声明的上面访问变量会报错

### 箭头函数
	基础
	let f=(v)=>v;
	嵌套：
	let f=(v)=>({c:(vv)=>v+vv});

### 原型链基础
	//构造函数用prototype指向原型
	//实例用__proto__指向原型
	//原型用constructor指向构造函数
	
	const c = function(a,b){
		this.a = a+22
		this.b = b
	}
	console.log(c.prototype.dd = 'dddd');
	
	const cc = new c(22,33);
	console.log(cc.a);
	console.log(cc.__proto__.ee = 'eee')
	console.log(cc.__proto__.constructor.prototype.ff = 'fff')

## 移动端兼容
	3.h5底部输入框被键盘遮挡问题h5页面有个很蛋疼的问题就是，当输入框在最底部，点击软键盘后输入框会被遮挡。可采用如下方式解决var oHeight = $(document).height(); //浏览器当前的高度
	
	   $(window).resize(function(){
	
	        if($(document).height() < oHeight){
	
	        $("#footer").css("position","static");
	    }else{
	
	        $("#footer").css("position","absolute");
	    }
	
	   });
###   css初始化

```css
html, body, div, span, applet, object, iframe,
 h1, h2, h3, h4, h5, h6, p, blockquote, pre,
 a, abbr, acronym, address, big, cite, code,
 del, dfn, em, img, ins, kbd, q, s, samp,
 small, strike, strong, sub, sup, tt, var,
 b, u, i, center,
 dl, dt, dd, ol, ul, li,
 fieldset, form, label, legend,
 table, caption, tbody, tfoot, thead, tr, th, td,
 article, aside, canvas, details, embed,
 figure, figcaption, footer, header,
 menu, nav, output, ruby, section, summary,
 time, mark, audio, video, input {
     margin: 0;
     padding: 0;
     border: 0;
     font-size: 100%;
     font-weight: normal;
     vertical-align: baseline;
 }


/* 禁用iPhone中Safari的字号自动调整 */
html {
-webkit-text-size-adjust: 100%;
-ms-text-size-adjust: 100%;
/* 解决IOS默认滑动很卡的情况 */
-webkit-overflow-scrolling : touch;
}

/* 禁止缩放表单 */
input[type=“submit”], input[type=“reset”], input[type=“button”], input {
resize: none;
border: none;
}

/* 取消链接高亮 */
body, div, ul, li, ol, h1, h2, h3, h4, h5, h6, input, textarea, select, p, dl, dt, dd, a, img, button, form, table, th, tr, td, tbody, article, aside, details, figcaption, figure, footer, header, hgroup, menu, nav, section {
-webkit-tap-highlight-color: rgba(0, 0, 0, 0);
margin: 0;
padding: 0;
}

/* 设置HTML5元素为块 */
article, aside, details, figcaption, figure, footer, header, hgroup, menu, nav, section {
display: block;
}

/* 图片自适应 */
img {
width: 100%;
height: auto;
width: auto\9; /* ie8 */
display: block;
-ms-interpolation-mode: bicubic;/*为了照顾ie图片缩放失真*/
}



body {
font: 12px/1.5 ‘Microsoft YaHei’,‘宋体’, Tahoma, Arial, sans-serif;
color: #555;
background-color: #F7F7F7;
}
em, i {
font-style: normal;
}
ul,li{
list-style-type: none;
}
strong {
font-weight: normal;
}
.clearfix:after {
content: “”;
display: block;
visibility: hidden;
height: 0;
clear: both;
}
.clearfix {
zoom: 1;
}
a {
text-decoration: none;
color: #969696;
font-family: ‘Microsoft YaHei’, Tahoma, Arial, sans-serif;
}
a:hover {
text-decoration: none;
}
ul, ol {
list-style: none;
}
h1, h2, h3, h4, h5, h6 {
font-size: 100%;
font-family: ‘Microsoft YaHei’;
}
img {
border: none;
}
input{
font-family: ‘Microsoft YaHei’;
}
/*单行溢出*/
.one-txt-cut{
overflow: hidden;
white-space: nowrap;
text-overflow: ellipsis;
}
/*多行溢出 手机端使用*/
.txt-cut{
overflow : hidden;
text-overflow: ellipsis;
display: -webkit-box;
/ -webkit-line-clamp: 2; /
-webkit-box-orient: vertical;
}
/*移动端点击a链接出现蓝色背景问题解决 */
a:link,a:active,a:visited,a:hover {
background: none;
-webkit-tap-highlight-color: rgba(0,0,0,0);
-webkit-tap-highlight-color: transparent;
}
```

### fastclick解决在手机上点击事件的300ms延迟

```jsx
if ('addEventListener' in document) {
    document.addEventListener('DOMContentLoaded', function() {
        FastClick.attach(document.body);
    }, false);
}
```


	rem单位
	媒体查询

### cookies,sessionstorage和localstorage

```
cookie是网站用来标记用户身份的一段数据，通常是一段加密的字符串，并且默认情况下只会在同源http请求中携带
seesionstorage：浏览器本地存储的一种方式，以键值对的形式存储，存储的数据会在浏览器关闭后自动删除
localstorage：数据一直都在

cookie的大小不能超过4k
sessionstorage 和 localstorage 的大小一般在5M以上，比cookie大的多
有效时间不同
```

### CSS布局大全https://juejin.im/post/6844903574929932301



比如1px 的边框。在`高清屏`下，移动端的1px 会很粗。 比如，这个是假的1像素

目前主流的屏幕DPR=2 （iPhone 8）,或者3 （iPhone 8 Plus）。拿2倍屏来说，设备的物理像素要实现1像素，而DPR=2，所以css 像素只能是 0.5。一般设计稿是按照750来设计的，它上面的1px是以750来参照的，而我们写css样式是以设备375为参照的，所以我们应该写的0.5px就好了啊！ 试过了就知道，iOS 8+系统支持，安卓系统不支持。


```javascript
      var viewport = document.querySelector("meta[name=viewport]");
      //下面是根据设备像素设置viewport
      if (window.devicePixelRatio == 1) {
          viewport.setAttribute('content', 'width=device-width,initial-scale=1, maximum-scale=1, minimum-scale=1, user-scalable=no');
      }
      if (window.devicePixelRatio == 2) {
          viewport.setAttribute('content', 'width=device-width,initial-scale=0.5, maximum-scale=0.5, minimum-scale=0.5, user-scalable=no');
      }
      if (window.devicePixelRatio == 3) {
          viewport.setAttribute('content', 'width=device-width,initial-scale=0.3333333333333333, maximum-scale=0.3333333333333333, minimum-scale=0.3333333333333333, user-scalable=no');
      }
      var docEl = document.documentElement;
      var fontsize = 32* (docEl.clientWidth / 750) + 'px';
      docEl.style.fontSize = fontsize;
```

## BFC块级格式化上下文

### BFC的布局规则

- 内部的Box会在垂直方向，一个接一个地放置。
- Box垂直方向的距离由margin决定。属于**同一个**BFC的两个相邻Box的margin会发生重叠。
- 每个盒子（块盒与行盒）的margin box的左边，与包含块border box的左边相接触(对于从左往右的格式化，否则相反)。即使存在浮动也是如此。
- BFC的区域不会与float box重叠。
- BFC就是页面上的一个隔离的独立容器，容器里面的子元素不会影响到外面的元素。反之也如此。
- 计算BFC的高度时，浮动元素也参与计算。

### 如何创建BFC

- 1、float的值不是none。
- 2、position的值不是static或者relative。
- 3、display的值是inline-block、table-cell、flex、table-caption或者inline-flex
- 4、overflow的值不是visible

父元素添加overflow: hidden;清除浮动



### 淘宝rem处理方式

```html
<!DOCTYPE html>
<html>
	<head>
		<meta charset="utf-8" />
		<meta name="viewport" content="width=device-width, initial-scale=1, maximum-scale=1, minimum-scale=1, user-scalable=no">
		<script type="text/javascript">
			(function flexible (window, document) {
			  var docEl = document.documentElement
			  var dpr = window.devicePixelRatio || 1
			
			  // adjust body font size
			  function setBodyFontSize () {
			    if (document.body) {
			      document.body.style.fontSize = (12 * dpr) + 'px'
			    }
			    else {
			      document.addEventListener('DOMContentLoaded', setBodyFontSize)
			    }
			  }
			  setBodyFontSize();
			
			  // set 1rem = viewWidth / 10
			  function setRemUnit () {
			    var rem = docEl.clientWidth / 10
			    docEl.style.fontSize = rem + 'px'
			  }
			
			  setRemUnit()
			
			  // reset rem unit on page resize
			  window.addEventListener('resize', setRemUnit)
			  window.addEventListener('pageshow', function (e) {
			    if (e.persisted) {
			      setRemUnit()
			    }
			  })
			
			  // detect 0.5px supports
			  if (dpr >= 2) {
			    var fakeBody = document.createElement('body')
			    var testElement = document.createElement('div')
			    testElement.style.border = '.5px solid transparent'
			    fakeBody.appendChild(testElement)
			    docEl.appendChild(fakeBody)
			    if (testElement.offsetHeight === 1) {
			      docEl.classList.add('hairlines')
			    }
			    docEl.removeChild(fakeBody)
			  }
			}(window, document))
		</script>
		<title></title>
	</head>
	<style type="text/css">
		p{font-size: 2rem;}
	</style>
	<body>
		<p>我不是</p>
	</body>
</html>
```

### 深浅拷贝

```
浅拷贝的意思就是只复制引用，而未复制真正的值。
深拷贝就是对目标的完全拷贝，不像浅拷贝那样只是复制了一层引用，就连值也都复制了，
只要进行了深拷贝，它们老死不相往来，谁也不会影响谁。
```

## 四、new和this

**new 操作符具体干了什么？**

> 当我们new一个数据的时候，new操作符到底做了什么？

- 首先是创建实例对象{}
- this 变量引用该对象，同时还继承了构造函数的原型
- 其次属性和方法被加入到 this 引用的对象中
- 并且新创建的对象由 this 所引用，最后隐式的返回 this

**new的模拟实现**

```js
function objectFactory() {

    var obj = new Object(),//从Object.prototype上克隆一个对象

    Constructor = [].shift.call(arguments);//取得外部传入的构造器

    var F=function(){};
    F.prototype= Constructor.prototype;
    obj=new F();//指向正确的原型

    var ret = Constructor.apply(obj, arguments);//借用外部传入的构造器给obj设置属性

    return typeof ret === 'object' ? ret : obj;//确保构造器总是返回一个对象

};
```

**this 对象的理解**

普通函数

- this 总是指向函数的直接调用者
- 如果有 new 关键字，this 指向 new 出来的实例对象
- 在事件中，this 指向触发这个事件的对象
- IE 下 attachEvent 中的 this 总是指向全局对象 Window
- 箭头函数中，函数体内的this对象，就是定义时所在作用域的对象，而不是使用时所在的作用域的对象。

```js
function foo() {
  console.log(this.a)
}
var a = 1
foo()           //1       

const obj = {
  a: 2,
  foo: foo
}
obj.foo()      //2

const c = new foo()   //undefined
```

- 对于直接调用 foo 来说，不管 foo 函数被放在了什么地方，this 一定是window
- 对于 obj.foo() 来说，我们只需要记住，谁调用了函数，谁就是 this，所以在这个场景下 foo 函数中的 this 就是 obj 对象
- 对于 new 的方式来说，this 被永远绑定在了 new出来的对象上，不会被任何方式改变 this

说完了以上几种情况，其实很多代码中的 this 应该就没什么问题了，下面让我们看看箭头函数中的 this

```js
function a() {
  return () => {
    return () => {
      console.log(this)
    }
  }
}
a()()()        //Window
```

- 首先箭头函数其实是没有 this 的，箭头函数中的 this 只取决包裹箭头函数的第一个普通函数的 this。在这个例子中，因为包裹箭头函数的第一个普通函数是 a，所以此时的 this 是 window。另外对箭头函数使用 bind这类函数是无效的。

面试题

https://zhuanlan.zhihu.com/p/82124513

https://zhuanlan.zhihu.com/p/57338228

https://zhuanlan.zhihu.com/p/158797820

https://zhuanlan.zhihu.com/p/159439787

https://zhuanlan.zhihu.com/p/63962882

https://zhuanlan.zhihu.com/p/84212558
