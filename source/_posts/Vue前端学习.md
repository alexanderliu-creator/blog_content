---
title: Vue前端学习
date: 2021-03-23 19:29:54
tags:
	前端
---



# 由于参加计算机设计大赛，最近把Vue捡起来，这是第三次看Vue了，整理一份对应的笔记出来。对应的Vue是2.x版本的。



<!--more-->



# Vue与前端的恩恩怨怨

- 前端发展路线：

![image-20210323193416642](https://gitee.com/alexs-rabbit//picture/raw/master/image-20210323193416642.png)

- 框架是一套完整的解决方案，库只是小功能的集合

## 核心模块：

- Vue核心库
- 第三方库
  - AXIOS模块（Ajax实现）
  - Router模块（路由管理）
  - Vuex模块（状态管理）
- UI模块：
  - element-ui
  - iview
- Webpack:
  - 模块打包器，打包，压缩，合并以及顺序加载





# Vue基础语法

## Nginx部署使用：

- 配置文件：

![image-20210323194858209](https://gitee.com/alexs-rabbit//picture/raw/master/image-20210323194858209.png)

- 配置文件：

![Nginx配置文件内容](https://gitee.com/alexs-rabbit//picture/raw/master/image-20210323194745344.png)

- 注意，location这里的根目录是Nginx.exe所在的那个目录。
- 双击一闪而过说明打开正常
- `localhost:80`即可访问。
- Nginx服务的关闭：

![image-20210323195404680](https://gitee.com/alexs-rabbit//picture/raw/master/image-20210323195404680.png)



## 基本语法（不使用脚手架的写法）：

### 数据单向绑定：

- `v-bind:value="message"`,从下往上绑定，message可以改变value，但是value却不能改变message
- `{{ message }}`，数据绑定，实现信息传递。
- 简写：`:title="message"`

### 数据双向绑定：

- `v-model="message"`

- v-model指令的本质是：  它负责监听用户的输入事件，从而更新数据，并对一些极端场景进行一些特殊处理。同时，v-model会忽略所有表单元素的value、checked、selected特性的初始值，它总是将vue实例中的数据作为数据来源。  然后当输入事件发生时，实时更新vue实例中的数据。

- `v-model.lazy='a_0'`，失去焦点的时候才更新，和`onblur`有点像

- 和JS中有点像，比如：

  ```html
  <input type="checkbox" v-model="a_1" value="1号">
  
  
  <script>
  	var vm = new Vue{
          el: '#app',
          data:
          {
              a_1: false
          }
      }
  </script>
  ```

- input触发的对应的值，都会和下面相绑定，比如说value对应的值，会直接和我下方的data中的数据结构绑定，并更新到数据结构中来。

- 重要概念：Vue框架调用对应的Javascript底层代码来对于DOM进行操作，来避免我们获取DOM结点，直接对于DOM进行操作。

### v-html：

- `v-html='url'`，与data中的url相绑定，渲染到页面中

### v-show：

- 这个实际上是对应了:`display: none`这个属性

### v-if:

- 这个本质上是结点的删除与重新创建，不符合条件的对象直接就从DOM中删除掉了！！！

### v-for:

```vue
<li v-for="(item,index) in items">
	值:{{ item }}, 索引:{{ index }}
</li>
```

- 第一个是元素，第二个是索引。这个和原生JS中，`for(var i = 0;i < items.length ; i++){target = target + items[i]}`是一样的嗷！！！
- 此外，vue里面的data和原生的JS感觉是一样的嗷，对象的定义，而且对象都有相同的方法。方法感觉也是JS中的方法，感觉上都是一样的！！！









### Vue.set:

- `Vue.set(vm.items,0,'XXXXXL')`
- 可以查查Vue里面提供了哪些对应的方法来执行类似的操作。







### Ajax:

1. `mounted()`钩子函数（可以下去查一查生命周期嗷！！！）
2. response.data才能拿到对应的返回的数据
3. 钩子函数和方法之间的区别：
   - 钩子函数自动调用，感觉有点像Java中的静态代码块会自动执行一次？？？可以为程序的运行打下一些基础之类的东西。
   - 钩子函数不可以写成属性，钩子函数单独是一个属性







### Computed属性：

- 类似于缓存的作用。

- 调用computed下的函数，只需要像调用属性一样调用就可以了，后面是不能加上括号的。

- 刷新页面的话，computed和正常的method一样，会调用。但是在这之后，computed计算出来的值，就不会改变了，相当于页面加载的时候是调用了这个方法，这个方法在后续的过程中成为了一个属性！！！所以整个computed的调用整体也像是属性的调用。

- computed方法体中的某些信息被改变之后，方法体本身才会被又一次执行刷新嗷！！！不过只调用方法，不改方法体中的值，调用的仍然是其中的缓存

- 实例：

  ```html
  <script>
  data:
  {
  	inputing:"",
  	list: ["Hi","Ha","Hab","Habc","Habcd"],
  }
  
  computed:
  {
  	myList()// myList = function(){}
  	{
  		return this.list.filter(
  			item =>
              item.indexOf(this.inputing)>-1
  		)
  	}
  }
  </script>
  ```

- 这个地方的意思就是说，当inputing这个属性改变时，computed属性内部的值改变了，这个时候就要重新计算。有点像它的名字！！！computed是计算属性，当内部的元素发生了改变时，该方法自动进行计算。



### Method属性：

- 写执行函数的，我个人感觉，和JS中写的那些函数没啥区别，绑定事件然后调用就可以了。







### 组件基础：

#### 基本语法格式：

- 与new Vue是一个档次的，以Vue.component开头嗷！

- 实例:

  ```html
  <script>
  Vue.component
  (
  	'abar',
       {
          template:`
  			<div style="backgroud:yellow">
  			<button @click='ccc>确定</button>
  			</div>'`,
          data(){  //注意这里data的写法嗷！！！
      		return{abarname:"abarname"}
  		},
          methods:
          (
          	ccc(){
              	alert("bbar")
              }
          )
      }
  )
  </script>
  ```

- 子组件里面可以嵌套子组件，如果在内部就是私有，如果在外部就是公有的组件。

- 私有的组件，自己内部定义，只能自己使用嗷！！！

- 公有的组件大家都可以调用嗷！！！

- 同样的可以进行事件绑定嗷！！！`@click='ccc'`

- 缺陷，这里的css不能提取出来，必须放在属性里面。如果是单个Vue文件的，可以专门抽出来一块儿地方写CSS文件的。





#### 父传子：

- ```vue
  props:
  {
  	thename: String,
  	show: Boolean,
  }
  ```

- 这个属性用来定义父传子的属性。父组件可以在使用子组件的时候传递props里面的属性给子组件。

- 这里加上属性的类型比较好，可以及逆行类型检查。





#### 子传父：

- ```vue
  methods:
  {
  	send(){
  		this.$emit('b_event','雷猴')
  	}
  } //子组件
  
  <bbar @b_event='receive($event)'></bbar>
  //bbar是在父组件中对于子组件的调用，可以在父组件中写对应的方法来对于子组件的信息进行处理
  
  data:
  {
  	show: [],
  }
  
  methods:
  {
  	receive(e){this.show.push('父组件收到消息！！！${e}')}
  } //父组件处理方法
  ```

- 子组件事件发生，发出事件给父组件，父组件通过监听子组件的反馈来完成信息传递。



#### ref

- 对于组件进行重命名，可以直接获取子组件，**甚至子组件中的原生结点**。
- `this.$refs.input_ref.value`，这种方式可以引用到对象的组件对象，甚至可以调用其中的方法！！！`this.$refs.bbar_ref.f('');`



#### 事件总线

- 事件总线

```html
<script>
	template:`
		<div>
			<input type='text' ref='b_text'/>
			<button @click='send()'>点击发送</button>`,
     methods:{
         send(){
             bus.$emit('c_message',this.$refs.b_text.value)
         }
     } //一个组件
    
    
    Vue.component(
    	mounted(){
        	bus.$on('c_message',(data)=>{console.log("已收到bbar的消息",data)})
        }
    )//另一个组件
    
    
    var bus = new Vue();//这个就是事件总线，啥都不用写哈！！！
</script>
```

- 用于事件混杂的时候，有的时候组件层数太多着实不好搞。



#### 动态组件：

- `components:{"home":{...},"news":{...}}`

- 这个东西可以根据输入来动态切换组件

```html
<component :is="who"></component>
<!--上面这个是引用的组件-->

<ul>
    <li @click='who'='home'>首页</li>
    <li @click='who'='news'>新闻</li>
</ul>
```

- `<keep-alive>`标签，组件切换的时候能够保留组件内残留的值，切换回来仍然可以康到嗷。注意哈，这个保留的结点也是新建和删除，数据被Vue框架保存了







#### 插槽：

`<slot></slot>`，把component之间的内容插入插槽中。

- 为了插入多个插槽，`<slot name="no.1">`，使用的过程中，`<div slot="no.1"></div>`

##### 嵌套：

```html
<todo>
	<todo-title slot="todo-title" :title_e="title"></todo-title>
    <todo-items slot="todo-items" v-for="item in items" :item_m="item"></todo-items>
</todo>
```



- 相当于先将数据传入一些子组件中，构成我们要的html模块儿
- 然后再将这些模块儿插入到其他子组件插槽里面去。
- 相当于上面`<div slot="no.1">`中的div改成我们所定义的子组件而已嘛。

##### 实际应用：

- 可以用另外一种方式来实现子传父。
- 要插入插槽的内容实际上可以访问父组件的元素，而插槽则是子组件的元素。插入子组件中的html模块儿可以访问父组件元素，并且可以更改。因此可以实现“子传父”，本质还是父传父啊orz......





### 钩子函数：

- mounted啊，beforeCreate啊，created啊之类的。
- 例如beforeCreate()是在Vue实例创建之前被嗲用的，这个时候就访问不到这个实例的属性和方法。
- beforeMount()和mounted():
  - 把编译好的模板放到页面上
  - 将模板中的值替换掉
  - 所以前面访问的时候，只能访问到`{{ message }}`，访问不到具体的值的嗷！！！
- 生命周期里面有很多环境都只执行一次的
- updated和beforeUpdate就是会循环多次的那种。更新是异步的，点击按钮的时候立马就进行了更新，但是在这里生命周期中讲的更新是页面上的更新嗷！！！
- 销毁前后各有一个`destroyed()`，`beforeDestroy()`方法，销毁之前，所有的方法和属性都可以使用。销毁之后还可以打印属性和方法？？？`v-show`注意哈，没有销毁，只是改了属性，是调用不了这两个方法的，`v-if`才可以嗷。
- 销毁本质上，是缓存了之前的数据。但是注意一下，这个时候已经无法对于缓存的数据进行修改了嗷！！！
- 例如setInterval()之类的，JS中学的定时器，销毁之后仍然可以读，但是它已经不运作了，起这个作用的。
- 外部销毁能够销毁该组件的DOM结构，但是内部销毁不可以嗷！！！
- 注意注意，这些生命周期是对于**某个组件**来说的，对于某个组件的设置有十分重要的作用！！！
- 注意，非常重要嗷！！！！！





## ajax的一丢丢内容

```vue
<script>
	methods:
    {
        ajaxx(){
            axios.get(this.ipp+this.dataa).then(
            res => {
                	console.log(res.data);
                	this.getdata = res.data;
            	}
            )
        }
    }
</script>
```

