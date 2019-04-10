函数声明语句
```javascript
	{
		let a = 'secret';
		function f() {
			return a;
		}
	}

```
函数表达式
```javascript
	{
		let a = 'secret';
		let f = function () {
			return a;
		}
	}
```

---

ES6 6种声明变量的方式
- var
- function
- let
- const
- import
- class

**import export**
编译时加载/静态加载
> 通过export 命令显示指定输出的代码，再通过import 命令输入
> import 命令输入的变量都只是 只读

```javascript
{
	function v1() { ... }

	export var firstName = 'Michael';
	export {
		v1 as funcV1;
	};

	import {stat, exists, readFile} from './profile.js';
}
```

#### webpack
- Entry		入口
- Output 	出口
- Loaders 	加载器
- Plugins	插件
```javascript
{
	var config = {
	modules: {
		rules: {
			<!-- 凡是在require()或者 import时遇到后缀名是.css文件时，先以css-loader转换，再以style-loader转换 -->
			test: /\.css$/,
			use: [
				'style-loader',
				'css-loader',
			]
}
}
}
}
```

初始化目录
npm init

本地局部安装 webpack
npm install webpack --save-dev

生产打包
webpack --progress --hide-modules

---

ES6	语法提示：
=> 是箭头函数 
```javascript
render: h => h(App)等同于:
	render: function(h) {
		return h(App);
}
也等同于：
	render: h => {
		return h(App);
}
```

**Vuex**
涉及改变数据的用**mutations**
存在业务逻辑的用**actions**
modules 将main.js里的 store分割到不同的模块
