# axios

导入 import axios from 'axios'

全局注册	Vue.prototype.$axios = axios

通过this.$axios调用



#### axios的异步请求的两种写法

```
getNewsData().then((resp) => {

	console.log(resp);

});


async function test(){
	resp = await getNewsData();
	cnosole.log(resp);
}

test();
```



#### 组件之间通讯

//我知道我的数据发生了重新渲染,但是我不知道变了以后要给干什么

//触发一个事件,让组件的使用者去搞定这件事

```
this.$emit("change", this.chooseId);
					监听事件名		参数 会返回给handleChange	

//然后在组件使用者身上监听事件

@change="handleChange"

methods: {
	handle(cId){
		console.log(cId);
	}
}

```



#### 插槽

使组件居中等,确定组件位置的特殊组件,相当于组件占位符,可以写默认值 

```
<slot></slot>
```



尽量不要刷新,刷新的消耗比较大,我们一般只通过history模式,只修改路径,router --> view(通过路由配置路径,)



# 指令

#### V-if v-else

V-if 和v-else之间不允许出现其他元素

新增了一个v-else-if

+ v-if是通过是否渲染DOM元素来实现显示是否的,只渲染一次的用v-if
+ 可以搭配v-for使用,进行筛选渲染,但是会消耗性能,不推荐使用
+ 如果需要可以用计算属性处理一下也很简单

#### v-show

+ v-show是通过改变元素的显示模式来实现的,反复需要切花内容的用v-show



#### v-for

+ 可以迭代数组和对象,迭代数组的时候可以写两个迭代器(item,index)
+ 迭代对象的时候可以写三个迭代器(value,name,index)
+ 可以接受整数 v-for="n in 10" //1~10
+ 可以在组件上使用v-for但是key时必须的,且item不要直接渲染,明确数据来源以复用组件

建议尽可能在使用 `v-for` 时提供 `key` attribute，除非遍历输出的 DOM 内容非常简单，或者是刻意依赖默认行为以获取性能上的提升。

vue总是会尽量用已经存在DOM元素进行渲染以获得性能上的提升,我么也应遵循这个渲染原则

*不要使用对象或数组之类的非基本类型值作为 `v-for` 的 `key`。请用字符串或数值类型的值。*

#### v-band

+ 值是变量的情况下记得用 **:**

#### v-html

识别html内容,但是不建议使用,防止脚本攻击,因为html里面可以嵌套script片段或者引入攻击脚本

#### 生命周期

要以函数形式写在vue实例里面,不允许用箭头函数,因为要频繁用到this

+ beforeCreate
+ created
+ beforeMount
+ mounted
+ beforeUpdate(当数据发生改变之前响应的)
+ updated
+ beforedestoryed  在组件发生撤销渲染的情况触发,之后再触发destroyed
+ Destroyed



#### 模版语法

在模版语法中完全支持javascritp表达式,要注意区分是不是表达式,不是表达式的不支持

主要是写些三元运算符和一些简单的加减计算,复杂一些的计算要用到计算属性

复杂运算写在模版里面会影响性能,计算属性也是用的表达式,但是会减少渲染,性能要优化一些



#### v-on

事件绑定 简写 `@`

有时也需要在内联语句处理器中访问原始的 DOM 事件。可以用特殊变量 `$event` 把它传入方法

且必须使用`$event`命名,且必须作为最后一个参数传入

#### 计算属性computed

可以减少内容消耗,先计算出来,就相当于减少点·运算符的消耗,先用变量接收然后后期就不需要运算了

只要计算一次就会进行缓存

可以设置一个时间对象计算属性进行比较 `new Date()`

cuomputed带有两个属性 set get

#### 事件修饰符

常用的一些修饰符

+ stop阻止事件冒泡
+ prevent阻止默认事件
+ enter只响应回车键
+ self
+ once
+ passive

#### 动态绑定class和style

可以与原生class共存

+ :class可以添加多个样式类,可以以对象、数组方式设置数据,或者数组和对象混合使用

+ :style也可以绑定多个样式属性

这里也可以绑定返回对象的计算属性进行进行计算然后绑定

#### 监听器watch

`msg(newValue, oldValue){}`

方法速写,msg是监控的数据

侦听器不仅包裹了数据的变化,还包裹了数组的一些方法

- `push()`
- `pop()`
- `shift()`
- `unshift()`
- `splice()`
- `sort()`
- `reverse()`

### 

#### 函数事件

当需要传入事件对象的时候,必须以`$event`命名并作为最后一个参数传递

#### v-model表单输入绑定

复选数据应设为数组,以选择多个

当我提交表单的时候,我们可用一个对象收集表单数据进行提交

+ `v-model` 指令在表单 ``、`` 及 `` 元素上创建双向数据绑定

+ `v-model` 会忽略所有表单元素的 `value`、`checked`、`selected` attribute 的初始值而总是将 Vue 实例的数据作为数据来源。你应该通过 JavaScript 在组件的 `data` 选项中声明初始值。
+ 在文本区域插值 (`{{text}}`) 并不会生效，应用 `v-model` 来代替。

**绑定的数据也可以通过修饰符来修饰**

`v-model.lazy`	使表单数据输入后在change事件后才发生改变

`v-model.number`	将输入的数据类型转化为整型

`v-model.trim`	过滤用户输入的空白字符



# 组件

#### 全局注册

```
// 定义一个名为 button-counter 的新组件
Vue.component('button-counter', {
  data: function () {
    return {
      count: 0
    }
  },
  template: '<button v-on:click="count++">You clicked me {{ count }} times.</button>'
})
```

+ 因为组件是可复用的 Vue 实例，所以它们与 `new Vue` 接收相同的选项，例如 `data`、`computed`、`watch`、`methods` 以及生命周期钩子等。仅有的例外是像 `el` 这样根实例特有的选项。

+ data必须是一个函数,以消除数据间的影响,各组件维护自己的独立的数据对象



#### 通过props向子组件传递数据

+ Prop 是你可以在组件上注册的一些自定义 attribute。当一个值传递给一个 prop attribute 的时候，它就变成了那个组件实例的一个 property
+ 理论上一个组件可以有任意个prop,数据类型也是任意的

```
命名:
props: ['postTitle']	如果以驼峰命名,下面就要以短横线的方式命名
<blog-post post-title="hello!"></blog-post>
```



#### $emit内建方法实现组件间通讯

`$emit(handle-fun, data)`

+ data作为第二个参数可以用来抛出一个特定的值
+ **推荐始终使用 kebab-case 的事件名**

因为HTML对大小写不敏感,如果用大驼峰小驼峰最后都会变成小写,导致监听不到事件

```
父组件传值给子组件: props
子组件传值给父组件:	$emit(fun, prams)
```



#### @ is an alias to /src

+ @使用在路径里,作为语法糖指向/src



#### slot插槽

```
<slot></slot>
插槽元素就是留个槽,你在这个组件里面写注入的内容会自动嵌套在插槽位置
```

+ 如果一个组件 template 中没有包含一个  `slot` 元素，则该组件起始标签和结束标签之间的任何内容都会被抛弃。
+ 可以为插槽设置一个后备默认值，它只会在没有提供内容的时候被渲染
+ slot可以通过name属性定义多个具名插槽,然后在模版中通过v-slot指令调用对应name插入到相应插槽
+ 没有定义name属性的插槽都隐式带有default属性,模版中没有放在指定name插槽中的内容默认放在默认插槽中
+ 注意 **`v-slot` 只能添加在 `template` 上**,只有在独占默认插槽的情况下在可以写在插件上
+ 只要出现多个插槽,就尽量使用完整的template语法,不要使用独占默认插槽的语法
+ 把参数之前的所有内容 (`v-slot:`) 替换为字符 `#`。例如 `v-slot:header` 可以被重写为 `#header`,但是必须要有参数才行,直接写#是不行的,最好使用明确的插槽取而代之



#### 动态组件

可以通过 Vue 的 `<component> `元素加一个特殊的 ` is` attribute 来实现诸如选项卡的组件切换

```
<!-- 组件会在 `currentTabComponent` 改变时改变 -->
<component v-bind:is="currentTabComponent"></component>
```

在上述示例中，`currentTabComponent` 可以包括

- 已注册组件的名字，或
- 一个组件的选项对象

`is`属性还可以实现不可嵌套的元素的嵌套

```
<table>
  <tr is="blog-post-row"></tr>
</table>

//自定义组件blog-post-row 作为属性传入
```



#### <keep-alive>组件

我们可以用<keep-alive>元素包裹那些我们第一次渲染想要缓存下来的组件

+ 注意这个 <keep-alive>要求被切换到的组件都有自己的名字，不论是通过组件的 `name` 选项还是局部/全局注册。



#### 访问元素或组件的边界

+ 在每个 `new Vue` 实例的子组件中，其根实例可以通过 `$root` property 进行访问
+ 和 `$root` 类似，`$parent` property 可以用来从一个子组件访问父组件的实例



#### Vue 提供了 `transition` 的封装组件，在下列情形中，可以给任何元素和组件添加进入/离开过渡

- 条件渲染 (使用 `v-if`)
- 条件展示 (使用 `v-show`)
- 动态组件
- 组件根节点

**利用赋值name属性,定义固定的样式类,vue在渲染的时候会自动添加或删除**



Vue 的过渡系统提供了非常多简单的方法设置进入、离开和列表的动效。那么对于数据元素本身的动效呢，比如：

- 数字和运算
- 颜色的显示
- SVG 节点的位置
- 元素的大小和其他的 property



#### JavaScript 钩子

可以在 attribute 中声明 JavaScript 钩子

```
<transition
  v-on:before-enter="beforeEnter"
  v-on:enter="enter"
  v-on:after-enter="afterEnter"
  v-on:enter-cancelled="enterCancelled"

  v-on:before-leave="beforeLeave"
  v-on:leave="leave"
  v-on:after-leave="afterLeave"
  v-on:leave-cancelled="leaveCancelled"
>
  <!-- ... -->
</transition>
```

当只用 JavaScript 过渡的时候，**在 `enter` 和 `leave` 中必须使用 `done` 进行回调**。否则，它们将被同步调用，过渡会立即完成。

推荐对于仅使用 JavaScript 过渡的元素添加 `v-bind:css="false"`，Vue 会跳过 CSS 的检测。这也可以避免过渡过程中 CSS 的影响。



#### 混入对象

`minins`

+ 如果混入对象的钩子和自身的钩子同名的时候,混入对象的钩子先执行

+ 值为对象的选项,methods.components和内部的键如果冲突,都合并为组件

#### Prop

#### 静态或动态值

+ 无论值是动态还是静态,都要告诉Vue实例这是一个表达式而不是一个原始值
+ 如果prop没有值都意味着`true`

```
<!-- 包含该 prop 没有值的情况在内，都意味着 `true`。-->
<blog-post is-published></blog-post>
```

**porp单向数据流**

所有的 prop 都使得其父子 prop 之间形成了一个**单向下行绑定**：父级 prop 的更新会向下流动到子组件中，但是反过来则不行



+ prop除了可以是常见类型外还可以是*构造函数*,用instanceof验证

+ 如果你**不**希望组件的根元素继承 attribute，你可以在组件的选项中设置 `inheritAttrs: false`



#### directives

如果想注册局部指令，组件中也接受一个 `directives` 的选项：

```
directives: {
  focus: {
    // 指令的定义
    inserted: function (el) {
      el.focus()
    }
  }
}
```

然后你可以在模板中任何元素上使用新的 `v-focus` property，如下：

```
<input v-focus>
```

**指令有一些可选的钩子函数和钩子函数的参数**



#### $root $parent $children

子组件是可以通过`$root`访问根组件的,可以通过`$parent`访问父组件

父组件可以通过`$children[0]`格式访问子组件



#### $event 就是vue里面的事件对象



利用内置组件`<component></component>`配合`is`属性可以实现动态组件渲染



**项目里面的地址如果是项目自身的路径,是需要用require(URL)的,因为在打包后,项目里面是没有那种文件路径的**



## vue实现原理 

```
https://www.bilibili.com/video/BV1m741137Q5/?p=32
```



## router

**通过注入路由器，我们可以在任何组件内通过 `this.$router` 访问路由器，也可以通过 `this.$route` 访问当前路由：**

**利用async await做一个异步请求,熟悉这些钩子,beforeMount(),数据请求放在这里也比较合理**

#### 动态路由

![image-20200722154535325](/Users/volcanoboy/Library/Application Support/typora-user-images/image-20200722154535325.png)

#### 路由嵌套

路由嵌套里面的子组件路径是不需要"/"的,

```
path: "news",
```



### 编程式的导航

除了使用 `` 创建 a 标签来定义导航链接，我们还可以借助 router 的实例方法，通过编写代码来实现。

#### `router.push(location, onComplete?, onAbort?)`

#### `router.replace(location, onComplete?, onAbort?)`

#### `router.go(n)`



#### 重定向和别名

`redirect`	直接配置,或者命名路由,或者返回一个函数

`alias`	命名别名



#### 组件传参

可以使用布尔模式|对象模式|函数模式(把route传进去就行)

如果 `props` 被设置为 `true`，`route.params` 将会被设置为组件属性。



#### *设置警告路由

通配所有为定义组件,404页面



### 导航守卫

就是路由在跳转的过程中的处理事件,起拦截作用,方便理解也可以理解为,路由钩子

例如跳转要查询是不是vip,显示对应页面

全局前置钩子

在所有页面跳转都会触发,这样对特定操作不友好,所以需要路由独享守卫

```
beforeEach(to, from, nextfun)
```

全局后置钩子

```
afterEach(to, from)
```

路由独享守卫

只在特定路由触发,这样可以个性化操作,比如只在特定路由触发登录组件

```
beforeEnter(to, from, nextfun)
```

组件内守卫

```
beforeRouteEnter(to, from, nextFun)
beforeRouteUpdate(to, from, nextFun)
beforeRouteLeave(to, from, nextFun)
```



#### 完整的导航解析流程

1. 导航被触发。
2. 在失活的组件里调用 `beforeRouteLeave` 守卫。
3. 调用全局的 `beforeEach` 守卫。
4. 在重用的组件里调用 `beforeRouteUpdate` 守卫 (2.2+)。
5. 在路由配置里调用 `beforeEnter`。
6. 解析异步路由组件。
7. 在被激活的组件里调用 `beforeRouteEnter`。
8. 调用全局的 `beforeResolve` 守卫 (2.5+)。
9. 导航被确认。
10. 调用全局的 `afterEach` 钩子。
11. 触发 DOM 更新。
12. 用创建好的实例调用 `beforeRouteEnter` 守卫中传给 `next` 的回调函数。



#### 路由元信息

就是放在`meta`里面的信息



#### 数据获取

有时候，进入某个路由后，需要从服务器获取数据。例如，在渲染用户信息时，你需要从服务器获取用户的数据。我们可以通过两种方式来实现：

- **导航完成之后获取**：先完成导航，然后在接下来的组件生命周期钩子中获取数据。在数据获取期间显示“加载中”之类的指示。
- **导航完成之前获取**：导航完成前，在路由进入的守卫中获取数据，在数据获取成功后执行导航。

从技术角度讲，两种方式都不错 —— 就看你想要的用户体验是哪种。



**在为后面的视图获取数据时，用户会停留在当前的界面，因此建议在数据获取期间，显示一些进度条或者别的指示。如果数据获取失败，同样有必要展示一些全局的错误提醒。**



#### 滚动行为

使用前端路由，当切换到新路由时，想要页面滚到顶部，或者是保持原先的滚动位置，就像重新加载页面那样。 `vue-router` 能做到，而且更好，它让你可以自定义路由切换时页面如何滚动。

`scrollBehavior` 方法接收 `to` 和 `from` 路由对象。第三个参数 `savedPosition` 当且仅当 `popstate` 导航 (通过浏览器的 前进/后退 按钮触发) 时才可用。

如果有动画,**要注意在动画结束后设置才会生效**



#### 路由懒加载

## 把组件按组分块

有时候我们想把某个路由下的所有组件都打包在同个异步块 (chunk) 中。只需要使用 [命名 chunk](https://webpack.js.org/guides/code-splitting-require/#chunkname)，一个特殊的注释语法来提供 chunk name (需要 Webpack > 2.4)。

```js
const Foo = () => import(/* webpackChunkName: "group-foo" */ './Foo.vue')
const Bar = () => import(/* webpackChunkName: "group-foo" */ './Bar.vue')
const Baz = () => import(/* webpackChunkName: "group-foo" */ './Baz.vue')
```



### Vuex

全局注册,组件可以通过`this.$store`查看获取仓库属性

```
export default new Vuex.Store({
  //data
  state,
  getters,
  //methods,在mutation里处理状态
  mutations,
  //异步方法
  actions,
  //模块
  modules: {
    buyCar
  }
})
```

将要用到的全局state数据放在computed里面使用,使用计算属性解耦



在组件里面导入mapState

```
import {mapState} from 'vuex'
```

**找箭头函数this指向的时候要记得,函数才算执行环境,单纯的大括号不是执行环境,所以对象这些是不管里面箭头函数的this的指向**







#### TypeScript支持

TypeScript 可能在推断某个方法的类型的时候存在困难。因此，你可能需要在 `render` 或 `computed` 里的方法上标注返回值

```
import Vue, { VNode } from 'vue'

const Component = Vue.extend({
  data () {
    return {
      msg: 'Hello'
    }
  },
  methods: {
    // 需要标注有 `this` 参与运算的返回值类型
    greet (): string {
      return this.msg + ' world'
    }
  },
  computed: {
    // 需要标注
    greeting(): string {
      return this.greet() + '!'
    }
  },
  // `createElement` 是可推导的，但是 `render` 需要返回值类型
  render (createElement): VNode {
    return createElement('div', this.greeting)
  }
})
```



#### 路由

如果只想实现一个简单的页面级应用,可以以下面这种最简单的方式实现,没有必要导入完整的roter库

```
const NotFound = { template: '<p>Page not found</p>' }
const Home = { template: '<p>home page</p>' }
const About = { template: '<p>about page</p>' }

const routes = {
  '/': Home,
  '/about': About
}

new Vue({
  el: '#app',
  data: {
    currentRoute: window.location.pathname
  },
  computed: {
    ViewComponent () {
      return routes[this.currentRoute] || NotFound
    }
  },
  render (h) { return h(this.ViewComponent) }
})
```



# Vuex

```js
Vue.use(Vuex)

//创建整个项目的数据仓库对象，将多组件公用的数据放置到此对象里
export default new Vuex.Store({
  //data
  state,
  //全局计算属性
  getters,
  //methods,在mutation里处理状态,这里所有的方法都是同步方法,不能做ajax异步操作
  mutations,
  //异步方法
  actions,
  //模块
  modules: {
    buyCar
  }
})
```

<img src="/Users/volcanoboy/Library/Application Support/typora-user-images/image-20200725204638732.png" alt="image-20200725204638732" style="zoom:50%;" />



通过fetch请求后返回的就是一个promise

### 辅助函数

**mapGetters|mapState|mapMutations|mapActions**

学会使用这几个函数,数据和计算属性就比较简单的了 



## Action

Action 通过 `store.dispatch` 方法触发

Actions 支持同样的载荷方式和对象方式进行分发：

```
// 以载荷形式分发
store.dispatch('incrementAsync', {
  amount: 10
})

// 以对象形式分发
store.dispatch({
  type: 'incrementAsync',
  amount: 10
})
```
