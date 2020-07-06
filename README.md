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
	

	
#### this.$emit('')可以触发一个自定义的事件
	
	1、子组件内创建一个方法：sendmsg();
	2、子组件的sendmsg()方法内创建自定义方法this.$emit('函数名',参数)
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
	
### 组件components	

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

	创建created、载入mounted、更新updated、销毁destroyed
	beforeCreate（创建前） 在数据观测和初始化事件还未开始。
	created（创建后） 完成数据观测，属性和方法的运算，初始化事件，$el属性还没有显示出来。
	beforeMount（载入前） 在挂载开始之前被调用，相关的render函数首次被调用。
			实例已完成以下的配置：编译模板，把data里面的数据和模板生成html。
			注意此时还没有挂载html到页面上。
	mounted（载入后） 在el 被新创建的 vm.$el 替换，并挂载到实例上去之后调用。
			实例已完成以下的配置：用上面编译好的html内容替换el属性指向的DOM对象。完成模板中的html渲染到html页面中。
			此过程中进行ajax交互。
	beforeUpdate（更新前） 在数据更新之前调用，发生在虚拟DOM重新渲染和打补丁之前。可以在该钩子中进一步地更改状态，
			不会触发附加的重渲染过程。
	updated（更新后） 在由于数据更改导致的虚拟DOM重新渲染和打补丁之后调用。
			调用时，组件DOM已经更新，所以可以执行依赖于DOM的操作。然而在大多数情况下，应该避免在此期间更改状态，
			因为这可能会导致更新无限循环。该钩子在服务器端渲染期间不被调用。
	beforeDestroy（销毁前） 在实例销毁之前调用。实例仍然完全可用。
	destroyed（销毁后） 在实例销毁之后调用。

### Vue每个生命周期什么时候被调用

	beforeCreate在实例初始化之后，数据观测(data observer)之前被调用
	created实例已经创建完成之后被调用，在这一步，完成已完成以下的配置：数据观测(data observer)，
		属性和方法的运算，watch/event事件回调，这里没有$el
	beforeMount在挂载开始之前被调用：相关的render函数首次被调用
	mounted el被创建的vm.$el替换，并挂载到实例上去之后调用该钩子
	beforeUpdate数据更新时调用，发生虚拟DOM重新渲染和补丁之前
	updated由于数据更改导致的虚拟DOM重新渲染和打补丁，在这之后调用该钩子
	beforeDestory实例销毁之前调用，在这一步，实例仍然完全可用
	destroyedVue实例销毁后调用，调用后，Vu实例指示的所有东西都会解绑定，
		所有的事件监听器会被移除，所有的子实例也会被销毁，该钩子在服务器端渲染期间不被调用
		
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

### 计算属性methods、computed

	相较于methods，不管依赖的数据变不变，methods都会重新计算，但是依赖数据不变的时候computed从缓存中获取，不会重新计算。

### key的作用

	保证项和值对应
	
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
	【如果data不是一个函数，那么当复用组件的时候，会导致数据改变同步】
	
### 局部基础组件

	components: {
	    'runoob':{
			  template: '<h1>自定义组件!</h1>'
			}
	  }
	
	
### 组件通过 Props 向子组件传递数据

	 <child v-bind:title="data"></child> 
	
	Vue.component('child', {
		  props: ['title'],
		  template: '<h3>{{title}}</h3>'
	})
	
	
		
### :todo循环组件数据

	<child v-for= "items in arr" :todo="items"></child>
	

	
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
	
	
##数组删除

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
	
##获取dom   get($event)

	<div @click="get($event)"></div>
	
	------------------------------
	get(data){
		data.currentTarget.className='';
	}
	
##路由

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
	
#在 beforeDestroy 中销毁定时器

##访问子组件数据$refs

	<HelloWorld ref="hello" :message="message"></HelloWorld>
	this.$refs.hello.属性  this.$refs.hello.方法
	
	
#子组件如何主动获取父组件中的数据

	this.$parent.属性
	

##双向绑定原理

	当你把一个普通的 JavaScript 对象传入 Vue 实例作为 data 选项，Vue 将遍历此对象所有的 property，
	并使用 Object.defineProperty 把这些 property 全部转为 getter/setter。
	watcher
	默认Vue在初始化数据时，会给data中的属性使用Object.defineProperty重新定义所有属性，
	当页面到对应属性时，会进行依赖收集(收集当前组件中的watcher)如果属性发生变化会通知相关依赖进行更新操作

##vue和react的区别

	组件写法不一样, React推荐的做法是 JSX + inline style, 
	Vue推荐的做法是webpack+vue-loader的单文件组件格式,即html,css,jd写在同一个文件;

##延迟回调$nextTick

	有些时候在改变数据后立即要对dom进行操作，此时获取到的dom仍是获取到的是数据刷新前的dom
	vue实现响应式并不是数据发生变化后dom立即变化，而是按照一定的策略来进行dom更新。
	$nextTick 是在下次 DOM 更新循环结束之后执行延迟回调，在修改数据之后使用 $nextTick，则可以在回调中获取更新后的 DOM
	this.$nextTick(() => //这里才可以 
	})


#怎么解决vue动态设置img的src不生效的问题

	 logo:require("./../assets/images/logo.png")
	
#Vue3.0你知道有哪些改进

	Vue3 中响应式数据原理改成proxy
	
#对 Vue 项目进行哪些优化？

	v-if 和 v-show 区分使用场景
	computed 和watch 区分使用场景
	v-for 遍历必须为 item 添加 key，且避免同时使用 v-if
	事件的销毁
	图片资源懒加载
	路由懒加载
	第三方插件的按需引入

#Vue中v-html会导致哪些问题

	可能会导致xss攻击
	v-html会替换掉标签内部的子元素
	
#Vue的渲染过程

	1、把模板编译为render函数
	2、实例进行挂载, 根据根节点render函数的调用，递归的生成虚拟dom
	3、对比虚拟dom，渲染到真实dom
	4、组件内部data发生变化，组件和子组件引用data作为props重新调用render函数，生成虚拟dom, 返回到步骤3
	
	
#组件data是函数的原因

	因为组件是用来复用的，JS里对象是引用关系，这样作用域没有隔离，而new Vue的实例，是不会被复用的，因此不存在引用对象问题

	
#如何比较React和Vue

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
	
	
	
#Vue中相同逻辑如何抽离

	使用混入
	Vue.mixin用法给组件每个生命周期，函数等都混入一些公共逻辑	
	
#为什么v-for和v-if不能连用

	当 v-for 和 v-if 处于同一个节点时，v-for 的优先级比 v-if 更高，这意味着 v-if 将分别重复运行于每个 v-for 循环中。
	如果要遍历的数组很大，而真正要展示的数据很少时，这将造成很大的性能浪费。 这种场景建议使用 computed，先对数据进行过滤
	
	

	
#computed和watch有什么区别

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
	
#3. computed 和 watch 有什么区别及运用场景?

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
	
	
#prop 验证，和默认值

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
	
#如何让CSS只在当前组件中起作用？
	将当前组件的style修改为style scoped
	
#<keep-alive></keep-alive>的作用是什么？

	<keep-alive></keep-alive> 包裹动态组件时，会缓存不活动的组件实例,主要用于保留组件状态或避免重新渲染。
	
##vue.js的两个核心是什么？

	数据驱动、组件系统
	
##谈谈你对MVVM开发模式的理解

	MVVM分为Model、View、ViewModel三者。
	Model 代表数据模型，数据和业务逻辑都在Model层中定义；
	View 代表UI视图，负责数据的展示；
	、、ViewModel 负责监听 Model 中数据的改变并且控制视图的更新，处理用户交互操作；
	、、Model 和 View 并无直接关联，而是通过 ViewModel 来进行联系的，
		Model 和 ViewModel 之间有着双向数据绑定的联系。
		因此当 Model 中的数据改变时会触发 View 层的刷新，View 中由于用户交互操作而改变的数据也会在 Model 中同步。
	这种模式实现了 Model 和 View 的数据自动同步，因此开发者只需要专注对数据的维护操作即可，而不需要自己操作 dom。
	
	
##网页从输入网址到渲染完成经历了哪些过程？

	输入网址；
	发送到DNS服务器，并获取域名对应的web服务器对应的ip地址；
	与web服务器建立TCP连接；
	浏览器向web服务器发送http请求；
	web服务器响应请求，并返回指定url的数据（或错误信息，或重定向的新的url地址）；
	浏览器下载web服务器返回的数据及解析html源文件；
	生成DOM树，解析css和js，渲染页面，直至显示完成；
	
	
#懒加载（按需加载路由）（常考）

	webpack 中提供了 require.ensure()来实现按需加载。以前引入路由是通过 import 这样的方式引入，改为 const 定义的方式进行引入。
	不进行页面按需加载引入方式：
	import  home   from '../../common/home.vue'
	进行页面按需加载的引入方式：
	const  home = r => require.ensure( [], () => r (require('../../common/home.vue')))


#active-class是哪个组件的属性？

  vue-router模块的router-link组件。


#Vue子组件调用父组件的方法

	第一种方法是直接在子组件中通过this.$parent.event来调用父组件的方法
	第二种方法是在子组件里用$emit向父组件触发一个事件，父组件监听这个事件就行了。



#split('').reverse().join('')


##this 指向对象，作为普通函数调用的时候，就指向全局了

#console.log(array1.push.apply(array1, array2)); 数组合并
	
#判断一个字符串中出现次数最多的字符，统计这个次数

	将字符串转化数组
	创建一个对象
	遍历数组，判断对象中是否存在数组中的值，如果存在值 +1，不存在赋值为 1
	定义两个变量存储字符值，字符出现的字数
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



	
#常见的浏览器内核有哪些 ？

	Trident 内核：IE, 360，搜狗浏览器 MaxThon、TT、The World,等。[又称 MSHTML]
	Gecko 内核：火狐，FF，MozillaSuite / SeaMonkey 等
	Presto 内核：Opera7 及以上。[Opera 内核原为：Presto，现为：Blink]
	Webkit 内核：Safari，Chrome 等。 [ Chrome 的：Blink（WebKit 的分支）]
	  

#事件委托是什么

	答案: 利用事件冒泡的原理，让自己的所触发的事件，让他的父元素代替执行
#如何阻止默认事件

	(1)return false；
	(2) ev.preventDefault();
#https

	https是HTTP运行在SSL/TLS之上，SSL/TLS运行在TCP之上


#事件绑定

	btn4.addEventListener("click",hello1);

#清除循环

	clearInterval();
#xss 跨站脚本攻击

#window.onload方法是在网页中所有的元素(包括元素的所有关联文件)

#0.1+0.2 ===0.30000000000000004



#apply、call、bind
	

	apply 、 call 、bind 三者第一个参数都是 this 要指向的对象，也就是想指定的上下文；
	apply 、 call 、bind 三者都可以利用后续参数传参；
	bind 是返回对应 函数，便于稍后调用；apply 、call 则是立即调用 。
	三者都可以改变函数的this对象指向。
	三者第一个参数都是this要指向的对象，如果如果没有这个参数或参数为undefined或null，则默认指向全局window。
	三者都可以传参，但是apply是数组，而call是参数列表，且apply和call是一次性传入参数，而bind可以分为多次传入。
	bind 是返回绑定this之后的函数，便于稍后调用；apply 、call 则是立即执行 。
	apply(thisArg, [argsArray])
	call(thisArg, arg1, arg2, …)


#盒子模型

	在IE盒子模型中，width表示content+padding+border这三个部分的宽度
	在标准的盒子模型中，width指content部分的宽度

#水平居中

	行内元素: text-align: center
	块级元素: margin: 0 auto
	position:absolute +left:50%+ transform:translateX(-50%)
	display:flex + justify-content: center
#垂直居中

	设置line-height 等于height
	position：absolute +top:50%+ transform:translateY(-50%)
	display:flex + align-items: center /*垂直对齐方式*/
	display:table+display:table-cell + vertical-align: middle;


#初始化样式

	body,h1,h2,h3,h4,h5,h6,hr,p,blockquote,dl,dt,dd,ul,ol,li,pre,form,fieldset,legend
	,button,input,textarea,th,td{margin:0;padding:0;}
	body,button,input,select,textarea{font:12px/1.5tahoma,arial,\5b8b\4f53;}
	h1,h2,h3,h4,h5,h6{font-size:100%;}
	address,cite,dfn,em,var{font-style:normal;}
	code,kbd,pre,samp{font-family:couriernew,courier,monospace;}
	small{font-size:12px;}
	ul,ol{list-style:none;}
	a{text-decoration:none;}
	a:hover{text-decoration:underline;}
	sup{vertical-align:text-top;}
	sub{vertical-align:text-bottom;}
	legend{color:#000;}
	fieldset,img{border:0;}
	button,input,select,textarea{font-size:100%;}
	table{border-collapse:collapse;border-spacing:0;}
#多行文本溢出

	p {
	  position: relative;
	  line-height: 1.5em;
	  /*高度为需要显示的行数*行高，比如这里我们显示两行，则为3*/
	  height: 3em;
	  overflow: hidden;
	  white-space:nowrap; 
		  text-overflow:ellipsis; 
	}

#flex

	.box{
	  display: -webkit-flex; /* Safari */
	  display: flex;
	  flex-direction:flex-direction: row | row-reverse | column | column-reverse; /*主轴的方向*/
	  flex-wrap: nowrap | wrap | wrap-reverse;/*换行方式*/
	  flex-flow /*以上的结合	*/
	  
	  
	  justify-content: flex-start | flex-end | center | space-between | space-around;/*水平对齐方式*/
	  align-items: flex-start | flex-end | center | baseline | stretch;/*垂直对齐方式*/
	  align-content: flex-start | flex-end | center | space-between | space-around | stretch;/*出现多行时的垂直对齐方式*/
	}

	.item {
	  
	  flex-grow: <number>; /* default 0 */  /*子元素放大比例*/
	}


#ajax
	
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



#构造函数

	用于创建对象的函数，叫做构造函数。
	和普通函数不同的是，构造函数调用后会得到一个对象
	构造函数中的返回值一直都是对象
	相比于对象来说，构造函数可以携带参数
	
	
#原型链

	实例对象通过 __proto__ 得到实例对象的原型对象
	构造函数通过 prototype 扩展属性
		//原型链关系  
		xiaohuang.__proto__ === Dog.prototype  
		Dog.prototype.__proto__ === Animal.prototype  
		Animal.prototype.__proto__ === Object.prototype  
		Object.prototype.__proto__ === null 
	

#继承属性

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
#继承方法 Object.create

	Teacher.prototype = Object.create(Person.prototype)
	Teacher.prototype.constructor = Teacher
	
#hasOwnProperty 查询属性

#字符串方法

	indexOf() 方法返回字符串中指定文本首次出现的索引（位置）
	：提取字符串	
	slice(start, end) 提取字符串的某个部分并在新字符串中返回被提取的部分。
	substring(start, end) 同上，数字不能为负数
	substr(start, length)
	replace()
	charAt() 方法返回字符串中指定下标（位置）的字符串：
	split() 将字符串转换为数组：
			
#数组去重

	if(新数组.indexOf(旧数组[i]) == -1){
	    新数组.push(旧数组[i]);
	}			
	
## 添加排序函数的sort()
	<script type="text/javascript">
		function sortNumber(a,b)
		{
		    return a - b
		}
		var arr = new Array(6)
		arr[0] = "10"
		arr[1] = "5"
		arr[2] = "40"
		arr[3] = "25"
		arr[4] = "1000"
		arr[5] = "1"
		document.write(arr + "<br />")
		document.write(arr.sort(sortNumber))
	</script>

#沉睡排序法

	var arr = [1,55,4,7,88,75,4];
	arr.forEach(function(num){
		setTimeout(function(){
			console.log(num)
		},num);
	})
#冒泡排序

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
	
		
#数组方法

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
		


闭包面试题

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
	
#outline: none;
#let / const

#箭头函数
	基础
	let f=(v)=>v;
	嵌套：
	let f=(v)=>({c:(vv)=>v+vv});


### Vue 怎么用 vm.$set() 解决对象新增属性不能响应的问题
### 组件切换

	<component v-bind:is="currentTabComponent"></component>

#es6
#vue
#less
#TypeScript
#html
#jquery
#sql
#ps
#webpack
#java
#mysql
#HTTP
#week
#css
#js

