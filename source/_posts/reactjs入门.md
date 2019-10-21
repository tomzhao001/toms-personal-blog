---
title: Reactjs入门
categories: ["React"]
tags: ["react"]
---

## 搭建环境

1. 可以通过直接安装的方式，有下面三个包是必须要安装的

   ```bash
   npm install react react-dom babel-standalone
   ```

   因为react推荐使用的JSX（JavaScript XML）脚本，必须要用babel进行翻译才能运行，

2. 也可以通过脚手架的方式搭建

   ```bash
   npm install -g create-react-app
   
   # To the workspace path
   create-react-app yourprojectname
   ```

   这样会自动建一个yourprojectname的路径，然后里面直接包含了一个可运行的react项目。

当然，在中国如果安装有问题的话，可以使用阿里的npm服务器就行了。

```bash
npm install -g cnpm --registry=https://registry.npm.taobao.org
```



## 使用ReactDOM

### Script的引用

在HTML中，需要引入相应的script，才能在下面用react相关的包。下面是三个基础包的引用：

```html
<script src="../node_modules/react/umd/react.development.js"></script>
<script src="../node_modules/react-dom/umd/react-dom.development.js"></script>
<script src="../node_modules/babel-standalone/babel.js"></script>
```

这里需要注意的是，`react.development.js`这个包一定要最先引用，因为`react-dom`的包对他是有依赖的。反过来的话会报错。



### ReactDOM.render(htmlTag, element)

这里的htmlTag，要确保只能有一个。比如下面这样就会报错

```javascript
ReactDOM.render(
	<label for="username">Name: </label>
  <input id="username" type="text">,
  document.getElementById('oDiv')
);
```

需要把这些都包含在一个parentTag中，就可以了！

```javascript
ReactDOM.render(
  <div>
    <label for="username">Name: </label>
    <input id="username" type="text">
  </div>,
  document.getElementById('oDiv')
);
```



所有的双标签要闭合，所有的单标签要用“/”结尾。

因为这里不是普通的html，是JSX！ 在XML中，要比html更规范一点，没有结尾的标签会报错。



JavaScript中的关键字，不能出现在标记中。比如上面那段代码，其实还是会报错的。

```
Warning: Invalid DOM property `for`. Did you mean `htmlFor`?
Warning: Invalid DOM property `class`. Did you mean `className`?
```

所以上面的`for`要变成`htmlFor`，`class`要变成`htmlClass`。



### 大括号可以使用inline代码

例子很简单：

```javascript
let userNameTag = 'username';
let userNameLabel = 'Name: ';
ReactDOM.render(
  <div>
  	<label htmlFor={userNameTag}>{userNameLabel}</label>
		<input htmlClass="content" type="text" id={userNameTag} />
    <p>10 * 100 = {10 * 100}</p>
		<p>(100 &gt; 5) &amp;&amp; (5 &lt; 7) = {((100 > 5) && (5 < 7)).toString()}</p>
  </div>,
	document.getElementById('oDiv')
);
```



## React Component

### 使用Class构造Component

1. 首字母必须大写，因为React是通过首字母的大小写来判断这个是自定义组件还是html标签。
2. 需要继承React.Component
3. render函数必须有。
4. Constructor函数建议有。`constructor(props){ super(props); }`
5. 可以使用get，set属性。

```javascript
class MyComponent extends React.Component {
  contructor(props) {
    super(props);
  }
  get name() {
    return this._name;
  }
  set name(value) {
    this._name = value;
  }
  render() {
    return (
    	<h4>Hello world! I am {this.name}</h4>
    );
  }
}
```

### 参数传递

#### props的参数传递

```javascript
class MyComponent extends React.Component {
  render() {
  	return (
    	<div>
      	<h4 style={this.props.style}>
      		My name is {this.props.name}, age is {this.props.age}
				</h4>
      </div>
    );
  }
}

// 字符串传递
<MyComponent name="Tom" age="25" />
// 值或对象传递
let userName = 'Tom';
let age = 34;
let style = {color: "#f00", backgroundColor: "#ccc"};
<MyComponent name={userName} age={age} style={style} />
```

### Key的Warning

#### 现象

当使用数组去渲染一系列子标签的时候，会报一个没有unique key的警告。

```
Warning: Each child in a list should have a unique "key" prop.
```

这个其实是由于VDOM的关系，当数据的集合变化的时候，这个key应该是不变的。

数组在这里就是一个变化的东西，因为数组内容的变化，会导致key中的内容也一起变化。

所以应该写成：

```javascript
let array = [
  {id: "s01", str: "aaa"},
  {id: "s01", str: "bbb"},
  {id: "s01", str: "ccc"}
];
let renderContents = array.map((ele, index) => (
	<h4 key={ele.id}>{ele.str}</h4>
));
```

标签上的key属性，要与内容相对应，就可以了！

#### 原因

参照下面VDOM的原理，因为VDOM中Diff的算法，使用key会降低不少渲染时的时间复杂度。

而且，key属性不应该改动，不然的话，就和没有key的时候一样了，会降低效率。



### 什么是Virtual DOM

在jQuery中，是通过JS Engine来操作DOM，然后通过Layout Engine（IE）来重新渲染页面。为了高效率也会使用局部渲染之类的。

这样的话，效率还是比较低的，因为每一次操作DOM都会触发页面的渲染。特别是在存在动画的页面中，每秒钟都要渲染很多很多次。

React第一次提出了Virtual DOM的概念，也就是说，不是每次都会实际操作DOM，而是到了时间点之后，一次性操作DOM，这样的话就会大大减少页面render的数量，从而提高了效率。特别是动画中，因为人类觉得60FPS已经很流畅了，其实每秒60次以上的很多的渲染次数都是浪费的。

1. 当数据被修改之后，React根据新的数据生成一个新的VDOM。
2. VDOM是一个JS对象，类似JSON。
3. 新生成的VDOM会和之前的VDOM进行比较（Diff算法），从根本实现了局部渲染。
   1. 类型不同 / 根type不同 ，认为是不同的树，清理重建。（所以React render返回的一定是一个根节点，返回的是一个树形的结构）
   2. 类型相同，直接比较。
   3. 类型相同，有key的话，会按照key进行比较。没有key的话，会根据顺序比较。如果顺序变换的时候，如果没有key，就会有很大的diff。但是如果有key的话，只是单纯的顺序调换的话，就判断没有diff。使用key的时间复杂度会降低很多。

所以，在循环的元素上，需要key的属性。

key属性不应该改动，错误的改动key属性意味着效率的降低和可能发生错误。

VDOM是根据Diff算法生成patch（与git有点类似），然后基于这个patch来更新DOM。



### setState

#### React的手动触发渲染？

比如有个需求是点击一个按钮，每点一次，一个标签就自动加一。

常规的使用render，就会这样写：

```javascript
class MyIncrButton extends React.Component {
  constructor(props) {
    super(props);
    this.number = 1;
  }
  incrFunc() {
    this.number += 1;
  }
  render() {
    return (
      <div>
        <label>{this.state.number}</label>
        <br />
        <input type="button" value="+1" onClick={this.incrFunc.bind(this)}></input>
			</div>
		);
	}
}
```

注：这里`bind(this)`是为了把外部的`this`绑定进函数里面，不然函数里面的this有自己的上下文context。

但是，这里会发现，其实并不会自增。

其实也是因为上面VDOM的原因。在React中，改变DOM中变量的内容，与会不会触发渲染，**一点关系都没有**！！

那么怎么处理这种事情呢，React中有个叫state的东西，可以出发render。

所以正确的代码就会变成：

```javascript
class MyIncrButton extends React.Component {
  constructor(props) {
    super(props);
    this.state = {
      number: 1
    };
  }
  incrFunc() {
    this.setState({
      number: this.state.number + 1
    });
  }
  render() {
    return (
      <div>
        <label>{this.state.number}</label>
        <br />
        <input type="button" value="+1" onClick={this.incrFunc.bind(this)}></input>
    	</div>
    );
  }
}
```

#### 手动触发重新render的条件

1. state发生变化
2. props发生变化

#### 真的是手动的吗？

其实如果到React源代码里面去看的话，会发现注释是很详细的，这里的`setState`操作，并不会直接触发update事件。而是有点像Java的垃圾回收请求一样，发出请求之后，并不知道什么时候会运行，而是交给框架后台去决定。

当然肯定也有强制触发的函数，叫`forceUpdate`，里面没有入参，只有一个callback。



### 变量props，state，children

表面上看的话，`this.state`是可以修改的，但是`this.props`是不可修改的。

所以一般情况下，props是给传递参数并且使用的。但是，`this.props.obj`中的内容是可以改的。

props和children的话，props是每个组件的属性，children是每个组件双标签中间的内容。

```javascript
<Test myName="aaa">This is aaa</Test>
// this.props.myName -> aaa
// this.props.children -> This is aaa
```







