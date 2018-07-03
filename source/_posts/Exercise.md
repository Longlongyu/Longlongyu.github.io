---
title: 学习使用Express的框架来开发项目(1)
date: 2018-06-28 14:39:37
abstract: 记录学习足迹，学习使用Express框架的使用以及webpack等工具。
tags:
- learn
- express
- webpack
- babel
---

# 写在前面的话
在最近的学习中，我深刻认识到前端学习就是一块大坑，也了解了自身能力的不足，以及想要在这个行业发展必须不断提升自己。 
所以，我写下这篇博客是意在监督自己，同时也能更好的整理在学习过程中的知识并进行回顾，和记录自己对遇到问题解决办法的处理来和大家分享。

# 关于Express
***Express*** 是一个基于 ***Node.js*** 平台的极简、灵活的 ***web*** 应用开发框架，它提供一系列强大的特性，可以帮助开发者创建各种 ***web*** 和移动设备应用。 
用 ***Express*** 框架开发 ***web*** 项目可以直接构建整个项目框架并且将前端页面跟后台贯穿起来，可以说是非常的灵活。

---

# 开发环境准备
## 安装node.js
***Express*** 是基于 ***Node.js*** 的，所以 ***Node.js*** 的安装必不可少，不过因为之前通过 ***hexo*** 来架构博客的原因，我的电脑已经安装了。

![nodejs](/assets/nodejs.png)

## 用npm安装express
***npm*** 是随同 ***Node.js*** 一起安装的包管理工具，可以用来安装卸载一些 ***api*** 包。
使用命令行工具`$ npm install (要安装的包)`进行本地安装，可以加上`-g`或者`--global`进行全局安装。  
这里执行命令`$ npm install express`就行。

---

# 构建项目
## 使用express来新建项目框架
`cd`进入想要建项目的文件夹，执行`$ express (项目名称)`，会在该目录下创建一个新的你所命名的项目工程。
这里我执行的是`$ express yiyun --pug --css sass`，因为我想同时学习 ***pug*** 和 ***sass*** 的语法，配合整个项目来更好的理解模块化。   

**关于项目框架的说明**
> /bin: www  文件用于应用启动
> /public: 静态资源目录：用来放置项目资源文件的
> /routes: 路由，是项目的控制器，不过我对其了解不太深，是学习的重点之一
> /views: view(视图)目录，用来放置前端页面的样式
> app.js：程序的主文件夹，目前项目中有用到的就是添加新的页面以及页面相应的路由需要在这里配置
> package.json：项目中用到的一些包的版本信息

## 试运行项目
项目创建好了之后，用命令行进入项目根目录，然后用`npm i`命令会安装 ***package.json*** 中的依赖项目。
通过执行`$ npm start`启动项目，到浏览器输入：***localhost:3000***，看到 ***Express*** 说明成功运行。

---

# 项目编写(遇到的问题
之后便开始了编写的工作，这里我打算重写我期末作品的网页。 ~~因为代码实在有够乱的，想到什么写什么。~~   

不过这并不是本篇的主题。   

在编写项目的过程中，我写完一段代码进行调试时，每次调试时都需要我关闭服务器才能进行重新刷新页面。
并且在之后我又有了新的需求，以便我有更好的开发体验。

## Express的debug模块
其实在创建项目时，***Express*** 通过命令行输出告知了，不过我并没有注意到，多方查找后才发现输入`$ DEBUG=(项目名称):* npm start`就行。
看来关于对于一个新工具的学习，应该要多加注意才行，以后自己可不能这么马虎了。**(汗颜- -**

## 使用babel来写ES6
在编写项目的 ***javascript*** 时，我使用了 ***ES6*** 标准来撰写代码，写是写爽了，不过在调试的过程中却遇到了问题，浏览器不支持 ***ES6*** 标准，
怎么办？幸好我在之前搭建博客时了解到了 ***babel*** 可以将 ***ES6*** 的代码转换为 ***ES5*** 标准，在当下 ***ES6*** 还没有在浏览器普及的今天，这可是大利器。
   
执行`$ npm install -g --save-dev babel-cli babel-core babel-loader babel-plugin-transform-runtime babel-preset-es2015`
***babel*** 已经在我们的项目中创建好了，这里的参数`--save-dev`会将所下载的工具保存到 ***package.json*** 的依赖项目中。
   
新建文件 ***.babelrc*** 并且写入

```
	{
	  "presets": ["es2015"]
	}
```

根据需求在命令行输入指令

```
	# 转码结果输出到标准输出
	$ babel example.js

	# 转码结果写入一个文件
	# --out-file 或 -o 参数指定输出文件
	$ babel example.js --out-file compiled.js
	# 或者
	$ babel example.js -o compiled.js

	# 整个目录转码
	# --out-dir 或 -d 参数指定输出目录
	$ babel src --out-dir lib
	# 或者
	$ babel src -d lib

	# -s 参数生成source map文件
	$ babel src -d lib -s
```

这样就可以简单地运行 ***babel*** 了。

## 搭配webpack来开发
虽然可以编写 ***ES6*** 的代码了，但开发友好度依然不好，而且在调试的时候依然遇到了问题，浏览器又又又又报错了。   

`Uncaught ReferenceError: require is not defined`

再查看一下错误位置，发现出现错误的原因是因为我们使用了 ***import*** 和 ***export*** ，***babel*** 对其只是进行了翻译，并不会合并代码的内容。所以我们需要另一项工具———— ***webpack***。
   
***webpack*** 是一个模块打包器。***webpack*** 的主要目标是将 ***JavaScript*** 文件打包在一起，将打包后的文件用于在浏览器中使用。
   
了解用途后，来命令行执行`$ npm install -g --save-dev webpack webpack-cli`
   
新建`webpack.config.js`文件，并对其进行配置   

```
	var path = require('path')
	module.exports = {
	  entry : {
		main : './src/js/main.js'
	  },
	  output : {
		//__dirname，就是当前webpack.config.js文件所在的绝对路径
		filename : '[name].js',
		path : path.join(__dirname, './public/javascripts'),
	  },
	  mode:"development",
	  module: {
	    rules: [
	      {
		  test: /\.js$/,
		  loader: 'babel-loader',
		  exclude: /node_modules/,
		  query: {
			'presets': ['es2015'],
			plugins : ['transform-runtime']
		  }
		}
		]
	   }
	};
```

最后在 ***package.json*** 里编辑命令

```
	"scripts": {
	  "dev": "webpack -w",
	  "start": "node ./bin/www"
	}
```

大功告成，另开一个命令行，分别执行`$ npm run dev`和`$ DEBUG=yiyun:* npm start`，现在 ***webpack*** 会监视 ***js*** 的改动，并重新发布成 ***main.js***，代码修改后只需要刷新浏览器就可以进行调试了。
   
---

# 最后
文章的最后也没有关于更多关于 ***express*** 的框架内容，关于 ***routes*** 部分的内容，我会继续进行学习和理解，就这样。 