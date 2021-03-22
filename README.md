## React

### 一、初识React

#### 1、引入文件

````html
<!--引入react核心库-->
<script src="https://unpkg.com/react@16/umd/react.development.js"></script>
<!--引入react-dom用于支持react操作dom-->
<script src="https://unpkg.com/react-dom@16/umd/react-dom.development.js"></script>
<!-- 引入babel，用于将jsx转传成js -->
<script src="https://unpkg.com/babel-standalone@6.15.0/babel.min.js"></script>
<!-- 引入prop-types，用于对组件标签属性进行限制 -->
<script src="https://cdnjs.cloudflare.com/ajax/libs/prop-types/15.7.2/prop-types.min.js"></script>
````

#### 2、创建根节点

````html
<div id="app"></div>
````

#### 3、使用

````js
<script type="text/babel">
  	// 创建虚拟Dom
  		// 方法1 -- jsx 需要引入babel
    const vDom = <h1>你好,React</h1>
			// 方法2 -- js
		// const vDom = React.creatElement(标签， 标签属性， 标签内容)
		// const vDom = React.creatElement('h1'， {class: 'tagClass'}， '<span>你好啊</span>')
    ReactDOM.render(vDom, document.getElementById("app"));
</script>
````

#### 4、虚拟dom是什么？

* 虚拟dom是object类型的对象（一般的对象）；

* 虚拟dom比较轻，真实dom重（虚拟dom是react内部再用，无需太多其他属性）；
* 虚拟dom最终会转换成真实dom。

#### 5、jsx语法规则

* 不要用引号；
* 引入变量用{}；
* 不要给元素样式类名指定不要用class，要用className；
* 写行间样式的时候不要写style=""，得写成style={  { key: value; key: value; }  };
* 不能有多个根标签；
* 标签必须闭合；
* 标签首字母：
  * 小写字母开头，转为html同名元素，若无同名元素，报错；
  * 大写字母开头，react渲染对应的组件，若组建未定义，报错；
* jsx里面使用{}时必须使用表达式，map很好用

### 二、组件

#### 1、函数式组件

````js
function Test() { // 函数名要大写
    return <h1>我是函数定义的简单组件</h1>
}
ReactDOM.render(<Test />, document....id("app"));
````

#### 2、类式组件

##### 2.1、复习类

````js
class Person {
  // 构造器方法
  constructer(name, age) {
    // this指向实例对象
    this.name = name;
    this.age = age;
  }
  speak() {
    console.log(`我的名字是${this.name}，我的年龄是${this.age}`)
  }
}
// 继承
class Student extends Person {
  // 继承的constructer构造函数中必须调用super，而且放在最前面
  constructer(name, age, gradge) {
    super(name, age)
    this.gradge = gradge
  }
  // 重写父类方法
  speak() {
     console.log(`我的名字是${this.name}，我的年龄是${this.age}, 我上${this.gradge}年级`)
  }
  study() {
    console.log('我热爱学习')
  }
}
// 给类自身添加属性
class Car {
  static a = {}
}
// 相当于 Car.a = {}
````

* 类中的构造器不是必须写的，要对实例进行一些初始化操作，如添加指定属性时才写
* 如果A类继承了B类，且A类中写了构造器，A类构造器中super是必须调用的
* 类中所有定义的方法都是放在了类的原型对象上

##### 2.2、类式组件

````js
class MyComponent extends React.Component {
  // render放在类（MyComponent）的原型对象上，供类的实例使用
  render() {
    // render中的this就是 -- MyComponent的实例对象
    return (
    	<div>
      	<span>我是用类定义的组件，我适用于复杂组件</span>
      </div>
    )
  }
}
````

#### 3、渲染组件到页面

````js
ReactDOM.render(组件标签（必须闭合）, document.get....Id("app"));
````

发生了什么？？？

* React解析组件标签，发现了MyComponent组件对象
* 发现该组件是用类定义的，随后new出来该类（组件）的实例，并且通过该实例调用到原型上的render方法
* 将render返回的虚拟DOM转换成真实DOM呈现在页面上

#### 4、组件实例的三大核心属性

##### 4.1、state

* state必须是个对象
* 组件中render方法中的this指向组件实例对象
* 组件自定义方法中的this是undefined，解决办法：
  * 赋值语句 + 箭头函数  例如： demo = () => {}
  * 强制绑定this，通过bind进行绑定
* 状态数据不能直接更改
  * 错误写法： this.state.name = "koby"
  * 正确写法： this.setStore({ name: "koby" })
    * 内部通过Object.assign(this.state, {name: "koby"})进行合并，当然了，这是原理，但不是真正的过程

###### 4.1.1  简单/复杂组件

​		组件分为两种，简单组件和复杂组件 区别是简单组件没有状态（state），复杂组件有状态（state）

````js
class MyComponent extends React.Component {
        constructor(props) {
            super(props);
            this.state = {
                isHot: true
            }
            this.changeWeather = this.changeWeather.bind(this);
        }
        render(){
            const { isHot } = this.state;
            return <h1 onClick={ this.changeWeather }>今天天气很{ isHot ? '炎热' : '凉爽' }</h1>
        }
        changeWeather() {
            // console.log(this)
            // this.state.isHot = !this.state.isHot // 错误写法
            this.setState({ this.state.isHot: !this.state.isHot });
        }
    }
    ReactDOM.render(<MyComponent/>, document.getElementById("app"));
````

*状态不可直接更改，要借助内置API setState({})*

###### 4.1.2 简化代码

````js
class MyCpn extends React.Component {
    // 初始化状态
    state = { name: 'koby' }
    render() {
        return <h2 onClick={ this.changeName }>你好啊, { this.state.name }</h2>
    }
    // 自定义方法,要用赋值语句 + 箭头函数
    changeName = () => {
        this.setState({name: 'Jams'})
    }
}
````

注： 

* 类中添加固定参数可以不写在constructor构造器中

  * ````js
    class Person {
      constructor() {
        a: 1
      }
    }
    // 等同于
    class Person {
      a = 1
    }
    ````

* 类中定义的方法想要this指向实例对象的话，需要使用赋值语句 + 箭头函数

  * ````js
    class Person {
      demo() {} // 等同于 demo = function() {}，因为其内部有this，所以不能访问外部的this
      demo = () => {} // 由于箭头函数内部没有this，要使用this就会去找他外层的作用域
    }
    ````

##### 4.2、props

###### 4.2.1、基本使用方式：

````js
// 创建组件
class Person extends React.Component {
  // 可以不写constructor，因为react对new出来的实例对象内置props属性
  constructor(props) {
    super(props);
  }
  render() {
    const {name, age, sex} = this.props;
    return (
    	<ul>
      	<li>姓名： { name }</li>
        <li>性别： { sex }</li>
        <li>年龄： { age }</li>
      </ul>
    )
  }
}
// 渲染组件
	//	需要传入参数，属性就是参数名	
ReactDOM.render(<Person name="koby" age=18 sex="女" />, document.getElementById("app"));

// 语法糖形式传入props，模拟网络请求数据传参
const data = {name: "Jams", age: "19", sex: "mail"}
ReactDOM.render(<Person {...data} speak={speak}/>, document.getElementById("app"));
````

注意：*<span style="color: red;">构造器尽可能的不写，写了就必须传props，并且需要使用super接收，否则在构造器中就不能访问this.props</span>*

###### 4.2.2、传值限制

````html
<!--首先引入包prop-types，因为react16版本以上不支持React.PropTypes-->
<script type="text/babel">
Person.propTypes = {
	name: PropTypes.string.isRequired, // 必须是字符串类型,而且为必传选项，否则报错
	age: PropTypes.number, // 限制为number类型
	sex: PropTypes.string,
	speak: PropTypes.func // 限制speak为方法
}
Person.defaultProps = {
	sex: '不男不女' // 默认值，不传时使用
}
</script>
````

###### 4.2.3、类props简写方式

````js
class Person extends React.Component {
	static propTypes = {
    name: PropTypes.string.isRequired
  }
	static defaultProps = {
    sex: "不男不女"
  }
  render() {
    。。。
  }
} 
````

##### 4.3、ref

###### 4.3.1、字符串形式的ref

````html
<input type="text" placeholder="请输入账号" ref="input"/>
<!--js代码-->
// this可以取到实例对象
this.refs.input // 获取节点，类似document.getElementById("input");
````

````js
class Demo extends React.Component {
    render() {
        return (
           <div>
            <input type="text" placeholder="点击按钮提示数据" ref="leftInput"/>&nbsp;&nbsp;&nbsp;&nbsp;
            <button onClick={this.messageLeft}>点击</button>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;
           </div>
        )
    }
    messageLeft = () => {
        console.log(this);
        alert(this.refs.leftInput.value)
    }
}
````

注：

* 和vue的ref用法很像
* 获取的时候用this.refs.名字
* 字符串类型的ref已经过时了，因为有效率问题，效率不高



###### 4.3.2、回调函数形式的ref（推荐）

````js
 render() {
    return (
        <div>
            <input type="text" placeholder="点击按钮提示数据" ref={ c => this.input1 = c}/>
            <button onClick={this.messageLeft}>点击</button>                        
        </div>
    )
}
````

注：

* 这里的ref是通过回调函数设置的，最终的ref会被挂载到组件的实例对象上，可以用this.input1获取到节点。

* 内联形式的回调函数当页面重新render的时候会执行两次回调，第一次返回null

* 想要避免上述问题就应该把回调函数写在外面，代码如下

  * ````js
    // 定义ref
    ref={this.callBack}
    // class内部定义函数
    callBack = (c) => {
      this.input1 = c;
    }
    ````



###### 4.3.3、createRef

*<span style="color:blue; font-weight:600;">使用 React.createRef()  调用后可以返回一个容器，该容器可以存储ref所标识的节点，该容器是专人专用的，只能存一个，后存挤前存</span>*

````js
class ..... {
  myRef = React.createRef();
  getRef = () => {
    // 通过this.myRef.current获取
    console.log(this.myRef.current); // <input />
  }
  render() {
    return (
    	<input ref={this.myRef} />
    )
  }
}
````



###### 总结 ref

* 不要过度使用ref，能省则省
  * 发生事件的元素和操作事件的元素是一个人的时候可以省，事件参数为event

#### 5、受控组件和非受控组件

一句话总结：

* 受控组件：随着每次输入框内容的改变，都会讲值保存到state中，类似v-model
* 非受控组件：只有在用到的时候才回去获取到值，需要ref
* 尽量不要写或少写ref

反正以后写表单类似的功能就用下面的方法：

<img src="/Users/pufeiyang/Library/Application Support/typora-user-images/image-20210127215850476.png" alt="image-20210128123428632" style="zoom:40%;" />



### 三、组件生命周期

​	新旧对比： 

* 旧版本废弃了三个生命周期钩子
  * componentWillMount
  * componentWillUpdate
  * componentWillReceiveProps
* 新版本新增两个生命周期钩子：
  * getDerivedStateFromProps  从props得到派生的状态
  * getSnapshotBeforeUpdate



#### 1、（旧）

* 按顺序 创建到挂载：
  * constructor 构造器最先调用；
  * componentWillMount 组件将要挂载
  * render 组件渲染
  * componentDidMount 组件挂载之后
* 调用 setState() 之后：（更新状态）
  * shouldComponentUpdate （控制组件更新的阀门）当调用setState()的时候调用，返回结果为Boolean，当为true会往下走，否则页面不发生任何更新
  * componentWillUpdate 组件将要更新的钩子，shouldComponentUpdate返回结果为true时首先调用，随后会调用render
  * componentDidUpdate 组件已经更新（组件更新后）回调
* 调用 forceUpdate() 之后 （强制更新）
  * 不会走shouldComponentUpdate，直接执行后面的
* 调用ReactDOM.unmountComponentAtNode(documnet.getElementById("app"))（卸载组件）之前
  * componentWillUnmount 组件卸载之前调用
* 父子组件传值的时候，父组件状态更新传到子组件：
  * componentWillReceiveProps 组件将要接收新的props的时候调用，<span style="color: orange">能借到props参数</span>

<img src="/Users/pufeiyang/Library/Application Support/typora-user-images/image-20210128123428632.png" alt="image-20210128123428632" style="zoom:50%;" />

<img src="/Users/pufeiyang/Library/Application Support/typora-user-images/image-20210128125539525.png" alt="image-20210128125539525" style="zoom:50%;" />



````js
// 初始化渲染  状态更新之后调用
render() {
    return (
        <h1 style={{ 'opacity': this.state.opacity }} onClick={ this.dropDom }>学习react之路很漫长,点击我卸载</h1>
    )
}

// 组件挂载完毕调用
componentDidMount() {
    this.timer = setInterval(() => {
        let { opacity } = this.state
        opacity -= 0.1
        if (opacity <=0) opacity = 1
        this.setState({ opacity })
    }, 200)
    console.log('爸爸,我被触发了');
}
// 组件将要被卸载调用
componentWillUnmount() {
  清除定时器
  clearInterval(this.timer);
}
dropDom = () => {
    // 卸载dom
    ReactDOM.unmountComponentAtNode(document.getElementById("app"))
}
````



#### 2、（新）

<img src="/Users/pufeiyang/Library/Application Support/typora-user-images/image-20210128173420360.png" alt="image-20210128173420360" style="zoom:50%;" />



##### 2.1、 getDerivedStateFromProps 生命周期函数的使用（几乎不用）

````js
static getDerivedStateFromProps(props, state) {
  // 必须return一个对象或null
  // 想让他不影响就返回null
  return props
}
````

注：

* 使用这个函数会使得state改变，切一切状态更新都会以props为主，改变state不会发生更新，几乎不使用！！！



##### 2.2、getSnapshotBeforeUpdate 在更新之前获取快照（几乎不用）

````js
getSnapshotBrforeUpdate() {
  console.log('getSnapshotBrforeUpdate');
  // 必须返回一个值，可以为任意类型的值，除了undefined
  return "爸爸"
}
// 他会将返回值传递给componentDidUpdate，当第三个参数
componentDidUpdate(prevProps, prevState, snapshotValue) {
  // prevProps 更新之前的props
  // prevState 更新之前的state
  // snapshotValue 当前快照
}
````



<img src="/Users/pufeiyang/Library/Application Support/typora-user-images/image-20210128200233107.png" alt="image-20210128200233107" style="zoom:50%;" />

#### 3、生命周期总结

注： 

* 重要的钩子

  * render： 组件初始化渲染或更新渲染调用
  * componentDidMount： 开启监听，发送ajax请求
  * componentWillUnmount： 做一些收尾的工作，例如：清除定时器等等

* 即将废弃的钩子：

  * componentWillMount
  * componentWillReceiveProps
  * componentWillUpdate

  现在使用会有警告，下一个大版本需要加上UNSAFE_前缀，以后可能会彻底删除



### 四、聊一聊diff

#### 1、react/vue遍历时key值的作用（内部原理）

* 简单的说：key时虚拟DOM对象的标识，在更新显示时key骑着极其重要的作用；
* 详细的说：
  * 当状态中的数据发生变化时，react会根据【新数据】生成【新的虚拟DOM】，随后react进行【新虚拟DOM】与【旧虚拟DOM】的diff比较，比较规则如下：
    * 旧虚拟DOM中好到了与新虚拟DOM相同的key：
      * 若虚拟DOM中内容没变，直接使用之前的真实DOM
      * 拖虚拟DOM中内容变了，则生成新的真实DOM，随后替换掉页面中之前的真实DOM
    * 旧虚拟DOM中未找到与新虚拟DOM相同的key：
      * 根据数据创建新的真实DOM，随后渲染到页面

#### 2、用index作为key可能会引发的问题

* 若对数据进行：逆序添加、逆序删除等破坏顺序的操作：
  * 会产生没有必要的真实DOM更新 ，界面效果没问题，但效率低
* 如果结构中还包含输入类的DOM：
  * 会产生错误DOM更新 ，界面有问题
* 如果不存在对数据的逆序添加、你需删除等破坏顺序操作，仅用于渲染列表用于展示，使用index作为key是完全没有问题的。

#### 3、如何选择key

* 最好使用每条数据的唯一标识作为key，比如id、手机号、身份证号、学号等唯一值
* 如果确定只是简单的展示数据，用index也是可以的 



### 五、react脚手架

#### 1、安装脚手架

​	全局安装

````js
npm install -g create-react-app
````

​	安装项目

````js
create-react-app <Project Name>
````

#### 2、文件说明

（1）public/index.html

<img src="/Users/pufeiyang/Library/Application Support/typora-user-images/image-20210128221533726.png" alt="image-20210128221533726" style="zoom:50%;" />

#### 3、配置代理

##### 3.1 简单配置方式

在package.json文件写入proxy

````js
"proxy": "http://localhost:5000"
````

注：

* 不配置代理就会显示跨域问题，能发请求，但接收不到
* react脚手架默认开启3000端口
* 3000没有的资源回去5000端口发请求
* 5000端口没有的资源就会返回404



##### 3.2 配置多个代理地址

首先在src文件夹下新建文件 `setupProxy.js`

````js
// setupProxy.js
// 需要使用commonjs模块化
	//引入内置模块
const proxy = require("http-proxy-middleware");

// 导出
module.exports = function(app) {
  // 可以传入多个参数
  app.use(
  	proxy("/api1", { // 遇见“/api1”前缀的请求就会触发代理
      target: "http://localhost:5000", // 请求换发给谁
      changeOrigin: true, // 控制服务器收到的请求头中 Host 字段的值
      pathRewrite: { "/api1": ""} // 重写请求路径（必须写上）
    }),
    proxy("/api1", {
      target: "http://localhost:5001",
      changeOrigin: true,
      pathRewrite: { "/api2": ""}
    }),
    ...
  )
}
````



#### 4、连续结构赋值的用法

````js
const obj = {
  a: {
    b: {
      c: 1
    }
  }
}
// 连续解构赋值
/*
const { a: {b : {c : d}}} = obj
// 结构到c的时候把c重命名，相当于结构之后 const d = c
console.log(d) // 1
*/
/*
const { a: { b: { c } } } = obj
console.log(c) // 1
*/
````

#### 5、兄弟组件通信

##### 5.1 消息订阅与发布

 订阅机制

````js
// 首先安装pubsub-js
npm i --save pubsub-js

// 引入
import PubSub from "pubsub-js";
// 在生命周期钩子中订阅消息
componentDidMount() {
  PubSub.subscribe("coderYang", (mag, data) => {
    console.log(data);
  })
}
// 取消订阅
componentWillUnmount() {
  PubSub.unsubscribe("coderYang");
}
````

发布机制

````js
// 引入包
import PubSub from "pubsub-js";

// 在事件方法中（点击）发布
handleSearch = () => {
  PubSub.publish("coderYang", /*参数*/);
}
````

##### 5.2、fetch发送请求

注：

* fetch 是原生函数，根本没有用到 xmlHttpRequest 对象提交 ajax 请求
* 老版本浏览器根本不支持

一句话，太关注分离了

````js
fetch("http://localhost:3000/user").then(
  res => {
    // 当前返回值只是告诉你访问服务器成功了，还拿不到返回值
    return res.json(); // res.json()返回的是一个Promise实例
  },
  /*err => {
    consle.log("链接服务器失败", err);
    return new Promise(() => {}); // 中断Promise
  }*/
 ).then(res => {
  console.log(res); // 这里的res才是真正返回的值
}).catch(err => {
  console.log(err); // Promise 错误穿透，统一处理失败回调
})
````

优化一下吧：

````js
async handleSearch = () => {
  try{
    const res = await fetch("http://localhost:1000");
    const data = res.json();
    console.log(data); // 返回的数据
  } catch (err) {
    console.log(err); // 出错打印信息
  }
}
````

### 六、路由 react-router-dom

##### 1、 基本使用

安装依赖

````js
// npm i --save react-router-dom
````

引入API

````js
import { Link, BrowserRouter} from "react-router-dom";
````

使用方式

````js
import React, { Component } from 'react'
import { Link, BrowserRouter} from 'react-router-dom'

export default class App extends Component {
    render() {
        return (
            <div>
                <BrowserRouter>
                    <Link className="test" to="/about" >About</Link>
          					<Link className="test" to="/home" >Home</Link>
                </BrowserRouter>
            </div>
        )
    }
}
````

注：

* 要想路由正常跳转必须使用一个 <BrowserRouter></BrowserRouter>
* 在index.js中用<BrowserRouter><App/></BrowserRouter>包裹即可
* <App/>最外层可疑包裹 BrowserRouter 或者 hashRouter

##### 2、路由组件与一般组件

区别：

* 写法不同
  * 一般组件：<Demo />
  * 路由组件：<Route path="/demo" component={Demo}>路由组件</Route>
* 存放位置不同（标准开发）
  * 一般组件：存放components文件夹下
  * 路由组件：存放pages文件夹下
* 接收到的props不同
  * 一般组件：<Demo /> 传递啥就是啥
  * 路由组件：接收三个固定内容
    * <img src="/Users/pufeiyang/Library/Application Support/typora-user-images/image-20210131124226327.png" alt="image-20210131124226327" style="zoom:50%;" />

##### 3、NavLink的使用

引入

````js
import { NavLink } from "react-router-dom"
````



使用 <NavLink></NavLink> 替换 <Link></Link>，当当前连接处于活跃时会在标签上默认加 active 类

````js
<NavLink className="test" to="/home" ativeName="abc">Home</NavLink>
// 注： ativeName 会设置当路由链接活跃时添加的 class 类名
````

注：标签体内容是一种特殊的标签属性

````js
<NavLink className="test" to="/home" ativeName="abc">Home</NavLink>
// 这个 Home 是标签体内容，会当做 children 属性传给组件，组件可以通过 this.props.children拿到它，当然了，适当时机还是 {...this.props}来的舒服
````

##### 4、Switch 提高路由匹配效率

引入

````js
import { Switch } from "react-router-dom";

// 使用
<Switch>
  <NavLink className="test" to="/home" ativeName="abc">Home</NavLink>
  <NavLink className="test" to="/about" ativeName="abc">About</NavLink>
  <NavLink className="test" to="/test" ativeName="abc">Test</NavLink>
</Switch>
````

注： 页面路由链接较多的情况下（比如100条），第一条就是匹配上的路由，但还会继续向下去匹配，效率极低，而且还会展示二次匹配的路由

​		使用 Switch 标签包裹，可以只匹配一次



##### 5、多级路由样式丢失问题

* 引入css的时候使用 "/css"， 不要用 "./css"

* 引入css的时候使用"%PUBLIC_URL%/css"

* 使用 <HashRouter><HashRouter>包裹<App/>， 不要用 <BrowserRouter></BrowserRouter>

  注：使用hash的时候会有 #



##### 6、模糊匹配、严格匹配

默认模糊匹配：

````js
<NavLink to="/home/a/b">Home</NavLink>
<Route path="/home" component={Home}></Route>
// 可以匹配
````

开启严格匹配 exact

````js
<NavLink to="/home/a/b">Home</NavLink>
<Route exact={true} path="/home" component={Home}></Route>
// 不可以匹配，可以简写 exact
````

注 ： 模糊匹配出问题了才开启严格匹配，不要轻易开启严格匹配



##### 7、Redirect 重定向内置API

想要在网页刚开始 localhost：3000的时候默认展示东西的时候

````js
<Route path="/home" component={Home}></Route>
<Route path="/about" component={About}></Route>
<Redirect to="/home" />
````

这是刚开始就会默认展示/home

##### 8、路由传参

###### 8.1、params传参

* 路由链接（携带参数）： <Link to="/demo/test/tom/18">详情</Link>
* 注册路由（声明参数）：<Route path="/demo/test/:name/:age" component={Test}/>
* 接收参数：const { name, age } = this.props.match.params

![image-20210131182551349](/Users/pufeiyang/Library/Application Support/typora-user-images/image-20210131182551349.png)

在Detail里通过 this.props.metch.params.参数 获取参数

###### 8.2 search传参

* 路由链接（携带参数）: <Link to={ "/demo/test/?name=zhangsan&age=18" }>详情</Link>

* 注册路由（无需声明参数，正常注册即可）: <Route path="/demo/test" component={Test}/>

* 获取到的 search 是 urlencoded 编码字符串，需要借助 querystring 进行解析

* 接收参数：

  * 首先需要引入框架自带的 querystring包 

  * ````js
    import qs from "querystring";
    
    const { search } = this.props.location // search 是 name=zhangsan&age=18 字符串
    const { name, age } = qs.parse(search) // 将search 解析成对象
    ````

    

###### 8.3 state参数

* 路由链接（携带参数）：  <Link to={ { path: "/demo/test", state: { name: "zhangsan", age: 18 }} }>详情</Link>
* 注册路由（无需声明参数，正常祖册即可）: <Route path="/demo/test" component={Test}/>
* 接收参数：this.state.location.state
* 刷新也可以保留住参数



##### 9、编程式路由

````js
class Cpn extends Component {
  showReplace = (id, name) => {
    this.props.history.replace(`/home/detail/${id}/${name}`)
  }
  showPush = (id, name) => {
    this.props.history.push(`/home/detail/${id}/${name}`)
  }
  render() {
    return (
      const { id, name, age } = this.props
    	<div>
      	<button onClick={() => {this.showReplace(id, name)}}>点击replace跳转<button>
        <button onClick={() => {this.showPush(id, name)}}>点击push跳转<button>
      </div>
    )
  }
}
````

注：

* this.props.history有很对API，比如前进（goForward）回退（goBack）等，可以适当时机使用
* 编程式路由可以携带 params参数（"/home/detail/${id}/${name}"）,search参数（"/home/detail?id=${id}&name=${name}"）,state参数（`/home/detail`, { id , name }）
* 接收参数时看准传参方式，不同传参方式使用不同接收参数方式接收，看 [六、8]()
* 使用this.props.history的时候一定要在路由组件中使用，其他的不好使（没history这个API）

##### 10、withRouter 的使用

* 只有路由组件的props里面才有history这个API
* 要想给一般组件加上history这个API就需要使用withRouter
* withRouter可以加工一般组件让一般组件具备路由组件特有的API

使用方式：

````js
import { Component } from "react";
import { withRouter } from "react-router-dom";

class Cpn extends Component {
  ...
}
export default withRouter(Cpn)
````

#### 七、redux

下载插件

npm  install --save redux



1、创建文件 ./redux/store.js

* 该文件专门报录一个store对象，整个应用只有一个store

````js
// 引入方法
import { createStore } from "redux";
// 引入reducer
import countReducer from "./count_reducer.js"
//导出
export default createStore(countReducer);
````

2、创建文件 ./redux/count_reducer.js

* 创建一个专门为 Count 组件服务的reducer，本质就是一个函数
* reducer函数会接到两个参数，之前的状态 （preState），动作对象（action）

````js
const initPre = 0 // 初始化状态
export default function countReducer(preState = initPre, action) {
  const { type, data } = action;
  switch (type) {
    case 'increment':
      return preState + data;
    case 'decrement':
      return preState - data;
    default: preState
  }
}
````

3、在组件中使用

* 通过store.getState() 可以获取值

* redux 里面的状态更新不会直接调用 组件中的 render 方法，所以得用下面代码中的 4-8行代码、

* store.subscribe方法每次 redux 状态更新的时候都会调用

* 一劳永逸的方法是将 store.subscribe 包裹 index 中的ReactDOM.render, 如：

  * ````js
    store.subscribe(() => {
      ReactDOM.render(<App/ >, document.getElementById("root"));
    })
    ````

````jsx
import { Component } from "react-dom";
import store from "./redux/store";
export default class App extends Component {
  componentDidMount() {
    store.subscribe(() => {
      this.setStore({}) // 不加这个页面不会重新render
    })
  }
  increment = () => {
    store.dispatch({type: 'increment', data: 1})
  }
  decrement = () => {
    store.dispatch({ type: 'decrement', data: 1})
  }
  render() {
    return (
      <div>
        <h2>{ store.getState() }</h2>
				<button onClick={this.increment}>加</button>
				<button onClick={this.decrement}>减</button>
			</div>
    )
  }
}
````

