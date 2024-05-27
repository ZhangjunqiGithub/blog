---
title: node.js基础
categories: node.js基础
tags: node.js基础
date: 2023/3/14
cover: https://icon-library.com/images/node-js-icon/node-js-icon-15.jpg
---

### Node.js简介

Node.js是一个基于Chrome V8引擎 的JavaScript运行环境;

其中V8引擎负责解析和执行JS代码

### Node.js的安装

官网地址：[Node.js (nodejs.org)](https://nodejs.org/en)

LST为长期稳定版，对于追求稳定性的企业级项目来说，推荐安装LTS版本的Node.js

Current为新特性尝鲜版，存在隐藏的Bug或安全漏洞。

### 查看已安装的Node.js的版本号

打开终端，在终端输入命令 node -v

### 在Node.js环境中执行JavaScript代码

1、切换到JS文件说在目录中

2、打开终端

3、输入 node 文件名，即可执行相应JS文件

### 解决文件修改后无法保存的问题

1、如果文件修改后无法保存，需通过管理员身份打开powershell通过nodepad+文件名打开

2、右键文件、打开属性、修改权限

### API文档的编写

[ShowDoc](https://www.showdoc.com.cn/)

实例：[导航--ShowDoc](https://www.showdoc.com.cn/128719739414963/2513278140891548)

### fs文件系统模块

#### fs.readFile()读取指定文件中的内容+fs.writeFile()向指定文件中写入内容

```js
//导入fs模块,来操作文件
const fs=require('fs');
//2、调用fs.readFile()方法读取文件
//参数1：读取文件存放路径
//参数2：读取文件时采用的编码格式，一般默认指定 utf8
//参数3：回调函数，拿到读取失败和成功的结果 err dataStr
fs.readFile('url','utf8',function(err,dataStr){
    //如果读取成功，则err的值为null
    //如果读取失败，则err的值为错误对象，dataStr的值为undefined
	if(err)
	{
		return console.log('读取文件失败！');
	}
    //如果读取成功则dataStr的值为成功的结果
	const arrOld=dataStr.split(' ');//如果文件中的数据通过空格隔开
	const arrNew=[];
	arrOld.forEach(item=>{
		arrNew.push(item.replace('=',':'));
	})

	const newStr=arrNew.join('/r/n');

//参数1：必选参数，需要指定一个文件路径的字符串，表示文件的存放路径
//参数2：必选参数，表示要写入的内容
//参数3：可选参数，表示以什么格式写入文件内容，默认值为utf8
//参数4：必选参数，文件写入完成后的回调函数
fs.write('url',newStr,'utf8',function(err){
		if(err)
		{	
			return console.log('文件写入失败！');
		}
		console.log('文件写入成功！');
	})
})
```

### getbasename  or  getextname

```js
const path=require('path');

const fpath='/a/b/c/index.html';

pathbasename1=path.basename(fpath);//结果为：index.html
pathbasename2=path.basename(fpath, '.html');//结果为：index

pathextname=path.extname(fpath);//结果为：.html
```

### 通过正则表达式来分离出一个文件中的html、css和js

```js
//导入fs 文件系统模块
const fs = require('fs');
//导入path路径处理模块
const path = require('path');

//匹配<style></style>标签的正则
//其中 \s 表示空白字符； \S 表示非空白字符； * 表示匹配任意次
const regStyle=/<style>[\s\S]*<\/style>/
//匹配<script></script>标签的正则
const regScript=/<script>[\s\S]*<\/script>/

fs.readFile(path.join(__dirname,'相对路径'),'utf8',function(err,dataStr){
	if(err)
	{
		return  console.log('读取HTML文件失败！'+err.message);
	}

	resolveCSS(dataStr);

	})

function resolveCSS(htmlStr)  {

//使用正则提取页面中的<style></style>标签
	const r1=regStyle.exec(htmlStr);

//将提取出来的样式字符串，进行字符串的replace替换操作
	const newCSS=r1[0].replace('<style>','').replace(</style>,'');

//调用fs.writeFile()方法，将提取的样式，写入到clock目录中 index.css的文件里面
	fs.wirteFile(path.join(__dirname,'相对路径'),newCSS,function(err){
		if(err)
		{
			return console.log('写入CSS样式失败！+err.message);
		}
		console.log('写入样式文件成功！');
	})
}
    
function resolveJS(htmlStr)  {

//使用正则提取页面中的<script></script>标签
	const r2=regStyle.exec(htmlStr);

//将提取出来的脚本字符串，进行字符串的replace替换操作
	const newJS=r2[0].replace('<script>','').replace(</script>,'');

//调用fs.writeFile()方法，将提取的js脚本，写入到clock目录中 index.js的文件里面
	fs.wirteFile(path.join(__dirname,'相对路径'),newJS,function(err){
		if(err)
		{
			return console.log('写入JS脚本失败！+err.message);
		}
		console.log('写入JS文件成功！');
	})
}

//将html部分分离出来并且引入style和script相关文件
function resolveHTML(htmlStr) {
	const newHTML=htmlStr
    .replace(regStyle,'<link rel="stylesheet" href="./index.css"/>')
    .replace(regScript,'<script src="./index.js"></script>');//引入相应的style和script文件

    //将替换完成之后的html代码，写入到index.html文件中
	fs.writeFile(path.join(__dirname,'./clock/index.html),newHTML,function(err){
		if(err)	return console.log('写入HTML文件失败！'+err.message);
		console.log('写入HTML页面成功！');
	})
}
```

### http模块

#### 创建基本的web服务器

```js
const http = require('http')
const server = http.createServer()
server.on('request', (req, res) => {
    //req.url是客户端请求的URL地址
    const url = req.url
        //req.method 是客户端请求的method类型
    const method = req.method
    const str = `你的Your request url is ${url},and request method is ${method}`
    console.log(str)
        //调用res.serHeader()方法，设置Content-Type响应头，解决中文乱码的问题
    res.setHeader('Content-Type', 'text/html;charset=utf-8')
        //调用res.end()方法，向客户端响应一些内容,并结束这次请求的处理过程
    res.end(str)
})
server.listen(80, () => {
    console.log('server running ar http://127.0.0.1:80')
})

//测试输入：http://127.0.0.1:8080/index.html 
//页面展示结果：Your request url is /index.html, and request method is GET
```

#### 根据不同的url响应不同的html内容

```js
const fs=require('fs')
//导入http模块
const http=require('http')
//导入path模块
const path=require('path')
//为服务器实例绑定requset事件，监听客户端的请求
const server=http.createServer()

server.on('request',(req,res)=>{
	const url=req.url
	let fpath=''
    //假设请求完整路径为__dirname/server/index.html
    //如果请求的路径是否为 / ，则手动指定文件的存放路径
	if(url=='/') {
		fpath=path.join(__dirname,'index.html')	
	} else {//如果请求的路径不为 /。（__dirname/index.html），则动态拼接文件的存放路径
				fpath=path.join(__dirname,'/server', url)
		}

	fs.readFile(fpath,'utf8',(err,dataStr)=>{
			if(err)	return res.end('404 Not Found.')
			res.end(dataStr)
		})
})
//启动服务器
server.listen(80,()=>{
	console.log('server runing at http://127.0.0.1')
})

//开启服务器:node xxx.js
```

### 模块化

#### 把代码进行模块化拆分的好处

1、提高了代码的复用性

2、提高了代码的可维护性

3、可以实现按需加载

#### Node.js中模块的分类

Node.js中根据模块来源的不同，将模块分为了3大类，分别是：

| 内置模块   | 由Node.js官方提供的，例如：fs、path、http等                  |
| ---------- | ------------------------------------------------------------ |
| 自定义模块 | 用户创建的每个.js文件，都是自定义模块                        |
| 第三方模块 | 由第三方开发出来的模块，并非官方提供的内置模块，也不是用户创建的自定义模块，使用前需要下载 |

#### 加载模块

使用强大的require()方法，可以加载需要的内置模块、用户自定义模块、第三方模块

```js
//加载内置的fs模块
const fs = require('fs')

//加载用户的自定义模块
const custom = require('./custom.js')

//加载第三方模块
/*先下载moment.js*/
const moment = require('moment')

注意：在使用require加载其它模块时，会执行被加载模块中的代码；
在加载用户自定义模块时，可以省略.js的后缀名
```

#### module.exports的使用(也可简化为exports)

**注意：require()得到的永远是module.exprots所指向的对象**

```js
/*1.js*/
module.exports.username = '张三';
const age = 20;
module.exports.age = age;
module.exports.sayHi = function() {
    console.log(username + 'Hi');
}

/*2.js*/
const m1 = require('xxx/1.js')//其中.js后缀名可以省略
/*
此时的m1是一个对象
m1 = {
	username: '张三',
	age: 20,
	sayHi: function() {
				console.log(username + 'Hi');
			}
}
*/
```

**注意：使用require()方法导入模块时，导入的结果，永远以module.exports指向的对象为准**

```js
/*1.js*/
module.exports.username = '张三';
const age = 20;
module.exports.age = age;
module.exports.sayHi = function() {
    console.log(username + 'Hi');
}
module.exports = {
    username: '李四',
    sex: '男',
    sayHello: function() {
        		console.log("hello");
    		}
}

/*2.js*/
const m1 = require('xxx/1.js')//其中.js后缀名可以省略
/*
此时的m1是一个对象
m1 = {
	username: '李四',
    sex: '男',
    sayHello: function() {
        		console.log("hello");
    		}
}
*/
```

### npm与包

#### 查看npm版本和下载包

在终端中输入  **npm -v**  即可查看npm的版本

从https://www.npmjs.com/网站上搜索自己所需要的包

从https://registry.npmjs.org/服务器上下载自己需要的包

安装执行版本的包：eg: npm i moment@2.22.2

>npm版本更新  `npm install -g npm`
>
>更新到指定版本  `npm -g install npm@6.8.0`
>
>清理npm缓存数据  `npm cache clean --force`

#### yarn的安装与卸载

> `npm install -g yarn`（安装）
>
> `npm uninstall yarn -g` （卸载）

yarn的常用命令

| yarn -v                                                  | 查看yarn版本                                                 |
| -------------------------------------------------------- | ------------------------------------------------------------ |
| yarn config list                                         | 查看当前yarn配置                                             |
| yarn config get registry                                 | 查看当前yarn源                                               |
| yarn config set registry https://registry.npm.taobao.org | 修改yarn源（此处为淘宝镜像）                                 |
| yarn add 包名                                            | 局部安装                                                     |
| yarn global add 包名                                     | 全局安装                                                     |
| yarn remove 包名                                         | 局部卸载                                                     |
| yarn global remove 包名                                  | 全局卸载（如果安装时安到了全局，那么卸载就要对应卸载全局的） |
| yarn global list                                         | 查看全局安装过的包                                           |

#### 自定义格式化当前时间和通过导入moment包实现格式化当前时间

```js
/*dateFormat.js*/
function dateFormat(dtStr) {
    const dt = new Date(dtStr)
    const y = dt.getFullYear()
    const m = padZero(dt.getMonth() + 1)
    const d = padZero(dt.getDate())
    const hh = padZero(dt.getHours())
    const mm = padZero(dt.getMinutes())
    const ss = padZero(dt.getSeconds())
    return `${y}-${m}-${d}-${hh}-${mm}-${ss}`//模板字符串
}

function padZero(n) {//补零函数
    return n < 10 ? '0' + n : n
}
module.exports = {
    dateFormat
}

/*test.js*/
const TIME = require('./dateFormat')
const dt = new Date()
const newDT = TIME.dateFormat(dt)
console.log(newDT);

//1、使用npm包管理工具，在项目中安装格式化时间的包moment  （npm i moment）
//2、使用require()导入格式化时间的包
//3、参考moment的官方API文档对时间进行格式化
const moment = require('moment')
const dt = moment().format('YYYY-MM-DD HH:mm:ss')
console.log(dt);
```

#### node_modules、package-lock.json和package.json的简介

node_modules:存放着我们下载下来的第三方包

package-lock.json: 保存着我们所下载包的各种详细信息

package.json: 保存着我们下载的包名和对应的版本号

**注意：今后在项目开发中，一定要把node_modules文件夹，添加到.gitignore忽略文件中**

#### 创建package.json

如果项目中没有package.json文件，可以通过**npm init -y**来快熟创建package.json文件，注意路径中不能含有中文或空格

#### 一次性安装所有的包

可以运行npm install或npm i 一次性安装所有的依赖包；

执行该命令时，npm包管理工具会先读取package.json中的dependencies结点，读取到记录的所有依赖包名称和版本号之后，npm包管理工具会把这些包一次性下载到项目中

#### 卸载包

eg: npm uninstall moment

注意：npm uninstall 命令执行成功后，会把卸载的包，自动从package.json的dependencies中移除掉

#### devDependencies结点

如果某些包只在项目开发阶段会用到，在项目上线之后不会用到，则建议把这些包记录到devDependencies结点中。与之对应的，如果某些包在开发和项目上线之后都需要用到，则建议把这些包记录到dependencies结点中

```js
#安装指定的包，并记录到devDependencies结点中
npm i 包名 -D
#注意：上述命令是简写形式，等价于下面完整的写法
npm install 包名 --save-dev
```

#### 切换npm的下包镜像源

```js
#查看当前的下包镜像源
npm config get registry
#将下包的镜像源切换为淘宝镜像源
npm config set registry=https://registry.npm.taobao.org/
#检查镜像源是否下载成功
npm config get registry
```

##### nrm

为了更方便的切换下包的镜像源，我们可以安装nrm这个小工具，利用nrm提供的终端命令，可以快速查看和切换下包的镜像源

```js
#通过npm包管理器，将nrm安装为全局可用的工具
npm i nrm -g
#查看所有可用的镜像源
nrm ls
#将下包的镜像源切换为 taobao 镜像
nrm use taobao
```

#### 全局包

在执行npm install 命令时，如果提供了-g参数，则会把包安装为全局包。

全局包会被安装到C:\Users\用户目录\AppData\Roaming\npm\node_modules目录下

```js
npm i 包名 -g #全局安装指定的包
npm uninstall 包名 -g #卸载全局安装的包
```

注意：只有工具性质的包，才有全局安装的必要性。因为它们提供了好用的终端命令

判断某个包是否需要全局安装后才能使用，可以参考官方提供的使用说明即可

#### i5ting_toc

i5ting_toc是一个可以把md文档转为html页面的小工具，使用步骤如下：

```js
#将i5ting_toc安装为全局包
npm install -g i5ting_toc
#调用i5ting_toc,轻松实现 md 转 html 的功能
i5ting_toc -f 要转换的md文件路径 -o
```

#### 规范的包结构

一个规范的包，它的组成结构，必须符合以下3点要求：

1、包必须以单独的目录而存在

2、包的顶级目录下要必须包含package.json这个包管理配置文件

3、package.json中必须包含name,version,main这三个属性，分别代表包的名字、版本号、包的入口

注意：以上3点要求是一个规范的包结构必须遵守的格式，关于更多的约束，可以参考如下网址:https://yarnpkg.com/zh-Hans/docs/package-json

#### 开发属于自己的包

初始化包的基本结构：

1.新建my-tools文件夹，作为包的根目录

2.在my-tools文件加中，新建如下三个文件：

package.json	(包管理配置文件)

index.js	(报道入口文件)

README.md(包的说明文档)

##### package.json文件的相关配置：eg:

{

​	"name": "my-tools",

​	"version": "1.0.0",

​	"main": "index.js",

​	"description": "提供了格式化时间，HTMLEscape相关的功能"，

​	"keywords": [

​		"itheima",

​		"dateFormat",

​		"escape"

​	],

​	"license": "ISC"

}

##### 在index.js中定义格式化时间等方法

```js
//格式化时间的方法
function() dateFormat(dateStr) {     ......    }
function padZero(n) {
	return n>9 ? n: '0' + n
}
//定义转移HTML字符的函数
function htmlEscape(htmlStr) {
    return htmlStr.replace(/<|>"|&/g", (match) => {
       		switch(match) {
        case '<':
        	return '&lt;'
        case '>':
        	return '&gt;'
        case '""':
        	return '&quot;'
        case '&':
        	return '&amp;'
    	}                    
    })
}
//定义还原HTML字符串的函数
function htmlUnEscape(str) {
    return str.replace(/&lt;|&gt;|&quot;|&amp;/g, (match) => {
        switch(match) {
            case '&lt;':
                return '<'
            case '&gt;':
                return '>'
            case '&quot;':
                return '""'
            case '&amp;':
                return '&'
        }
    })
}

module.exports = {
		dateFormat,
    	htmlEscape,
    	htmlUnEscape
}
```

##### 将上方不同的功能进行模块化的拆分

src/dateFormat.js

```js
//格式化时间的方法
function() dateFormat(dateStr) {     ......    }
function padZero(n) {
	return n>9 ? n: '0' + n
}
```

src/htmlEscape.js

```js
//定义转移HTML字符的函数
function htmlEscape(htmlStr) {
    return htmlStr.replace(/<|>"|&/g", (match) => {
       		switch(match) {
        case '<':
        	return '&lt;'
        case '>':
        	return '&gt;'
        case '""':
        	return '&quot;'
        case '&':
        	return '&amp;'
    	}                    
    })
}
//定义还原HTML字符串的函数
function htmlUnEscape(str) {
    return str.replace(/&lt;|&gt;|&quot;|&amp;/g, (match) => {
        switch(match) {
            case '&lt;':
                return '<'
            case '&gt;':
                return '>'
            case '&quot;':
                return '""'
            case '&amp;':
                return '&'
        }
    })
}
```

index.js

```js
//这是包的入口文件
const date = require('./src/dateFormat')
const escape = require('./src/htmlEscape')

//向外暴露需要的成员
module.exports = {
    ...date,
    ...escape
}
```

##### 自定义my-tools包的使用

```js
//先导入自定义包
const mytools = require('./my-tools')
//然后通过mytools.方法或属性即可
```

##### 编写包的说明文档

内容：安装方式、导入方式、格式化时间、转义HTML中的特殊字符、还原HTML中的特殊字符、开源协议

例：

```markdown
## 安装
​```
npm install my-tools
​```

## 导入
​```js
const mytools = require('my-tools')
​```

## 格式化时间
​```js
//调用dateFormat对事件进行格式化
const dtStr = mytools.dateFormat(new Date())
//结果 2022-03-20 20:41:55
console.log(dtStr)
​```

## 转义 HTML 中的特殊字符
​```js
//待转换的 HTML 字符串
const htmlStr = '<h1 title="abc">这是h1标签<span>123&nbsp;</span></h1>'
//调用 htmlEscape 方法进行转换
const str = mytools.htmlEscape(htmlStr)
//转换的结果 &lt;h1 title=&quot;abc&quot;&gt;这是h1标签&lt;span&gt;123&amp;nbsp;&lt;/span&gt;&lt;/h1&gt;
console.log(str)
​```

## 还原 HTMl 中的特殊字符
​```js
//带还原的 HTML 字符串
const str2 = mytools.htmlUnEscape(str)
//输出的结果<h1 title="abc">这是h1标签<span>123&nbsp;</span></h1>
console.log(str2)
​```

## 开源协议
ISC
```

##### 发布包到npm

https://www.npmjs.com

```bash
#注意：在运行npm login命令之前，必须先把下包的服务器地址切换为npm的官方服务器。否则会导致发布包失败！
#1、查看当前所在服务器
nrm ls

#2、切换到官方服务器
nrm use npm

#3、注册 npm 账号
#(1)访问https://www.npmjs.com/网站，点击sign up按钮，进入注册用户界面
#(2)填写账号相关的信息: Full Name、Public Email、Username、Password
#(3)点击Create an Account按钮，注册账号
#(4)登录邮箱，点击验证链接，进行账号的验证

#4、在终端中输入npm login,然后输入Username、Password和Email进行登录

#5、在npm官网上查重（包名不能雷同）

#6、将终端切换到包的的根目录之后，运行npm publish 命令，即可将包发布到npm上
```

##### 删除已发布的包

运行npm unpublish 包名 --force命令，即可从npm删除已发布的包

注意：

1、npm unpublish 命令只能删除72小时以内发布的包

2、npm unpublish 删除的包，在24小时内不允许重复发布

3、发布包的时候要慎重，尽量不要往npm上发布没有意义的包！

### express

#### 简介

express是基于Node.js平台，快速、开放、极简的Web开发框架

#### 安装

在项目所处的目录中，运行如下的终端命令，即可将express安装到项目中使用

```js
npm i express @4.17.1
```

#### 创建基本的Web服务器

```js
//1、导入 express
const express = require('express')
//2、创建 web 服务器
const app = express()
//3、调用app.listen(端口号，启动成功后的回调函数),启动服务器
app.listen(80, () => {
    console.lgo('express server running at http://127.0.0.1')
})
```

#### 监听GET请求

通过app.get()方法，可以监听客户端的GET请求，具体的语法格式如下：

```js
//参数1：客户端请求的 URL 地址
//参数2：请求对应的处理函数
//req: 请求对象（包含了与请求相关的属性与方法）
//res: 响应对象（包含了与响应相关的属性与方法）
app.get('请求URL'， function(req, res) {/*处理函数*/})
```

#### 监听POST请求

通过app.post()方法，可以监听客户端的POST请求，具体的语法格式如下：

```js
//参数1：客户端请求的 URL 地址
//参数2：请求对应的处理函数
//req: 请求对象（包含了与请求相关的属性与方法）
//res: 响应对象（包含了与响应相关的属性与方法）
app.post('请求URL'， function(req, res) {/*处理函数*/})
```

#### 把内容响应给客户端

通过res.send()方法，可以把处理好的内容，发送给客户端：

```js
app.get('/user', (req, res) => {
    //向客户端发送 JSON 对象
    res.send({name: '张三', age: 20, gender: '男'})
})

app.post('/user', (req, res) => {
    //向客户端发送文本内容
    res.send('请求成功')
})
```

#### 获取URL中携带的查询参数

通过req.query对象，可以访问到客户端通过查询字符串的形式，发生到服务器的参数：

```js
app.get('/', (req, res) => {
    //req.query默认是一个空对象
    //客户端使用?name=张三&age=20这种查询字符串形式，发送到服务器的参数
    //可以通过req.query对象访问到，例如：
    //req.query.name req.query.age
    console.log(req.query)
})
```

#### 获取URL中的动态参数

通过req.params对象，可以访问到UTL中，通过：匹配到的动态参数：

```js
//URL地址中，可以通过：参数名 的形式，匹配动态参数值
app.get('/usr/:id', (req, res) => {
    // req.params 默认是一个空对象
    //里面存放着通过： 动态匹配得到的参数值
    console.log(req.params)
})
```

注意：动态参数可以有多个；例如：app.get('/usr/:id/:username', (req, res) => {})

#### 托管静态资源

##### express.static()

通过这个函数，我们可以非常方便地创建一个静态资源服务器，例如通过如下代码就可以将public目录下的图片、CSS文件、javaScript文件对外开放访问了：

```js
app.use(express.static('public'))
```

现在，你就可以访问public目录中的所有文件了：

http://localhost:3000/images/bg.ipg

http://localhost:3000/css/style.css

http://localhost:3000/js/login.js

**注意: Express在指定的静态目录中查找文件，并对外提供资源的访问路径。因此，存放静态文件的目录名不会出现在URL中。**

在访问public文件夹中的文件时只需要写IP/文件名即可，不需要再写public文件夹名了；即（IP/public/文件名）

##### 托管多个静态资源目录

如果要托管多个静态资源目录，请多次调用express.static()函数

```js
app.use(express.static('public'))
app.use(express.static('files'))
```

访问静态资源文件时，express.static()函数会根据目录的添加顺序查找所需要的文件。

##### 挂载路径前缀

如果希望在托管的静态资源访问路径之前，挂载路径前缀，则可以使用如下的方式：

```js
app.use('/public',express.static('public'))
```

现在，你就可以通过带有/public前缀地址来访问public目录中的文件里

http://localhost:3000/public/images/kitten.jpg

http://localhost:3000/public/css/style.css

http://localhost:3000/public/js/app.js

#### express安装并使用nodemon

在终端中，运行如下命令，即可将nodemon安装为全局可用的工具：

```js
npm install -g nodemon
```

通过nodemon app.js来启动项目时，如果app.js中的代码被修改了，就会被nodemon监听到，如果保存app.js会自动重启项目

#### express模块化路由

为了方便对路由进行模块化的管理，Express不建议将路由直接挂载到app上，而是推荐将路由抽离为单独的模块

将路由抽离为模块的步骤如下：

1、创建路由模块对应的.js文件

2、调用express.Router()函数创建路由对象

3、向路由对象上挂载具体的路由

4、使用module.exports向外共享路由对象

5、使用app.use()函数注册路由模块

##### 创建路由模块

```js
var express = require('express') //1、导入express
var router = express.Router()//2、创建路由对象
router.get('/user/list', function(req, res) {//3、挂载获取用户列表的路由
    res.send('Get user list.')
})
router.post('/user/add', function(req, res) {//4、挂载添加用户的路由
    res.send('Add new user.')
})
module.exports = router//5、向外导出路由对象
```

````js
//1、导入路由模块
const router = require('./03.router')
//2、注册路由模块
app.use(router)
//注意：app.use()函数的作用，就是来注册全局中间件
````

为路由模块添加前缀：

```js
//1、导入路由模块
const userRouter = require('./router/user.js')
//2、使用app.use()注册路由模块，并添加统一的访问前缀 /api
app.use('/api',userRouter)
```

### 中间件函数

#### 定义简单的中间件函数

```js
//常量 mw 所指向的，就是一个中间件函数
const mw = function(req, res, next) {
    console.log('这是一个最简单的中间件函数')
    //注意：在当前中间件的业务处理函数完毕后，必须调用next()函数
    //表示把流转关系转交给下一个中间件或路由
    next();
}
//将mw注册为全局生效的中间件
app.use(mw)

//全局中间件的简化形式
app.use((req, res, next) => {
    console.log('这是一个最简单的中间件函数')
    next()
})
```

#### 中间件的作用

多个中间件，共享同一份req和res，基于这样的特性，我么可以在上游的中间件中，统一为req或res对象添加自定义的属性或方法，供下游的中间件或路由进行使用。

#### 局部生效的中间件

不使用app.use()定义的中间件，叫做局部生效的中间件

```js
//定义中间件函数 mw1
const mw1 = function(req, res, next) {
    console.log('这是中间件函数')
    next()
}
//mw1 这个中间件只在当前路由中生效，这种用法属于局部生效的中间件
app.get('/', mw1, function(req, res) {
    res.send('Home page.')
})
//mw1 这个中间件不会影响下面这个路由
app.get('/user', function(req, res) {res.send('User page.')})
```

#### 定义多个局部的中间件

```js
//以下两种写法是完全等价的
app.get('/', mw1, mw2, (req, res) => {res.send('Home page.')})
app.get('/', [mw1, mw2], (req, res) => {res.send('Home page.')})
```

#### 中间件的5个使用注意事项

1、**一定要在路由之前注册中间件**

2、客户端发送过来的请求，可以连续调用多个中间件进行处理

3、执行完中间件的业务代码之后，不要忘记调用next()函数

4、为了防止代码逻辑混乱，调用next()函数之后不要再写额外的代码

5、连续调用多个中间件时，多个中间件之间，共享req 和 res 对象

#### 错误级别的中间件

```js
app.use(function(err, req, res, next) {//错误级别的中间件
    console.log('发送来错误' + err.message)//在服务器打印错误消息
    res.send('Error!' + err.message)//向客户端响应错误相关的内容
})
```

**注意：错误级别的中间件必须注册在所有路由之后**

#### express内置的中间件

默认情况下，如果不配置解析表单数据的中间件，则req.body默认等于undefined

##### express.json

解析JSON格式的请求体数据（有兼容性，仅在4.16.0+版本中可用）

```js
//配置解析 application/json格式数据的内置中间件
app.use(express,json())
```

##### express.urlencoded

解析URL-encoded格式的请求体数据（有兼容性，仅在4.16.0+版本中可用）

```js
//配置解析application/x-www-form-urlencoded格式数据的内置中间件
app.use(express.urlencoded({ extended: false }))
```

#### 第三方的中间件

例：body-parser第三方中间件的使用（解析表单数据的中间件）

1、运行 npm install body-parser 安装中间件

2、使用 require 导入中间件

3、调用 app.use() 注册并使用中间件

### 使用express写接口

#### 编写GET接口

```js
const router = express.Router()
router.get('/get', (req.res) => {
    //通过req.query获取客户端通过查询字符串，发送到服务器的数据
    const query = req.query
    //调用res.send()方法，向客户端响应处理的结果
    res.send({
        status: 0,//0 表示处理成功，1 表示处理失败
        msg: 'GET 请求成功！',//状态的描述
        data: query//需要响应给客户端的数据
    })
})
```

#### 编写POST接口

```js
const router = express.Router()
router.get('/post', (req.res) => {
    //获取客户端通过请求体，发送到服务器的 URL-encoded 数据
    const body = req.body
    //调用res.send()方法，向客户端响应处理的结果
    res.send({
        status: 0,//0 表示处理成功，1 表示处理失败
        msg: 'GET 请求成功！',//状态的描述
        data: query//需要响应给客户端的数据
    })
})
```

**注意：如果要获取 URL-encoded 格式的请求体数据，必须配置中间件 app.uer(express.urlencoded({ extended: false }))**

### CORS跨域资源共享（常用）

**注意：JSONP只支持**

#### 简介

CORS（Cross-Origin Resource Sharing，跨域资源共享）由一系列 HTTP 响应头组成，这些 HTTP 响应头决定浏览器是否阻止前端JS代码跨域获取资源

浏览器的同源安全策略默认会阻止网页“跨域”获取资源。但如果接口服务器配置了CORS相关的HTTP响应头，就可以解除浏览器端的跨域访问限制。

cors是express的一个第三方中间件。通过安装和配置cors中间件，可以很方便的解决跨域问题。

#### CORS的注意事项

1、CORS主要在服务器端进行配置。客户端浏览器无需做任何额外的配置，即可请求开启了CORS的接口

2、CORS在浏览器中有兼容性。只有支持XMLHttpRequest Level2的浏览器，才能正常访问开启了CORS的服务器端接口（例如：IE10+、Chrome4+、FireFox3.5+）

#### 使用步骤

1、运行 npm install cors 安装中间件

2、使用 const cors = require('cors') 导入中间件

3、在路由之前调用 app.use(cors()) 配置中间件

#### CORS相关的三个响应头

##### Access-Control-Allow-Origin

用法：例如：

下面的字段值将只允许来自http://test.cn的请求

```js
res.setHeader('Acess-Control-Allow-Origin', 'http://test.cn')
```

如果指定了Acess-Control-Allow-Origin字段的值为通配符 * ，表示允许来自任何域的请求，例如：

```js
res.setHeader('Acess-Control-Allow-Origin', '*')
```

##### Acess-Control-Allow-Headers

默认情况下，CORS仅支持客户端向服务器端发送如下的9个请求头：

Accept、Accept-Language、Content-Language、DPR、Downlink、Save-Data、Viewport-Width、Width、Content-Type（值仅限于text/plain、multipart/form-data、application/x-www-form-urlencoded三者之一）

如果客户端向服务器端发送来额外的请求头信息，则需要在服务器端，通过Acess-Control-Allow-Headers对额外的请求头进行声明，否则这次请求会失败！

```js
//允许客户端额外向服务器发送 Content-Type 请求头和 X-Custom-Header 请求头
//注意：多个请求头之间使用英文的都好进行分隔
res.setHeader('Acess-Control-Allow-Headers', 'Content-Type, X-Custom-Header')
```

##### Access-Control-Allow-Methods

默认情况下，CORS仅支持客户端发起GET、POST、HEAD请求。

如果客户端希望通过PUT、DELETE等方式请求服务器的资源，则需要在服务器端，通过Access-Control-Allow-Methods来指明实际请求所允许使用的HTTP方法。

```js
//只允许POST、GET、DELETE、HEAD请求方法
res.setHeader('Access-Control-Allow-Methods', 'POST, GET, DELETE, HEAD')
//允许所有的HTTP请求方法
res.setHeader('Access-Control-Allow-Methods', '*')
```

在网页中使用jQuery发起JSONP请求

```js
$.ajax({
	method: 'GET',//JSONPq请求只能用GET请求
    url 'http://127.0.0.1/api/jsonp',//表示要发起JSONP的请求
    dataType: 'jsonp',
    success: function(res) {
    console.log(res)
}
})
```

### 前后端身份认证

#### 服务器端渲染的Web开发模式(不常用)

**使用express-session中间件**

##### 安装：(在项目目录中打开终端)

```js
npm install express-session
```

##### 配置express-session中间件

```js
//导入session中间件
var session = require('express-session')
//配置session中间件
app.use(session({
    secret: '任意字符串',//加密字符串
    resave: false, //固定写法
    saveUninitialized: true //固定写法
}))
```

##### 使用

当express-session中间件配置成功后，即可通过req.session来访问和使用session对象，从而存储用户的关键消息：例：

```js
app.post('./api/login', (req, res) => {
    //判断用户提交的登录信息是否正确
    if (req.body.username !== 'admin' || req.body.password !== '123456') {
        return res.send({
            status: 1,
            msg: '登录失败'
        })
    }
    req.session.user = req.body //将用户的信息，存储到session中
    req.session.isLogin = true //将用户的登录状态，存储到session中
    res.send({
        status: 0,
        msg: '登录成功'
    })
})
```

从session中获取数据

```js
//获取用户姓名的接口
app.get('/api/username', (req, res) => {
    //判断用户是否登录
    if (!req.session.isLogin) {
        return res.send({
            status: 1,
            msg: 'fail'
        })
    }
    res.send({
        status: 0,
        msg: 'sucess',
        username: req.session.user.username
    })
})
```

清空session信息（例如：当某个用户调用退出登录的接口时，会清空服务器中保存的**该用户的session信息**）

```js
app.post('/api/logout', (req, res) => {
    //清空当前客户端对应的session信息
    req.session.destroy()
    res.send({
        status: 0,
        msg: '退出登录成功',
    })
})
```

#### 前后端分离的Web开发模式（常用）

**使用JWT进行身份认证**

##### JWT的组成部分

JWT通常由三部分组成，分别是Header(头部)、Payload(有效荷载)、Signature(签名)

**Header.Payload.Signature**

**Payload**部分才是真正的用户信息，它是用户信息经过加密之后生成的字符串

**Header和Signature**是安全性相关的部分，只是为了保证Token的安全性

##### JWT的使用方式

###### 安装如下两个JWT相关的包：

```js
npm install jsonwebtoken express-jwt
```

**jsonwebtoken**用于生成JWT字符串

**express-jwt**用于将JWT字符串解析还原成JSON对象

###### 导入JWT相关包

使用require()函数，分别导入JWT相关的两个包

```js
//导入用于生成 JWT 字符串的包
const jwt = require('jsonwebtoken')
//导入用于将客户端发送过来的 JWT 字符串，解析还原成 JSON 对象的包
const expressJWT = require('express-jwt')
```

###### 定义secret密钥

为了保证 JWT 字符串的安全性，防止 JWT 字符串在网络传输过程中被别人破解，我们需要专门定义一个用于加密和解密的secret密钥：

1、当生成JWT字符串的时候，需要使用secret密钥对用户的信息进行加密，最终得到加密号的JWT字符串

2、当把JWT字符串解析还原成JSON对象的时候，需要使用secret密钥进行解密

```js
//secret 密钥的本质就是一个字符串
const secretKey = 'abvdefg'
```

###### 在登陆成功后生成JWT字符串

调用jsonwebtoken包提供的sign()方法，将用户的信息加密成JWT字符串，响应给客户端

```js
//登录接口
app.post('/api/login', function(req, res) {
    //...省略登录失败情况下的代码
    //用户登录成功之后，生成 JWT 字符串，通过 token 属性响应给客户端
    res.send({
        status: 200,
        message: '登录成功!',
        //调用 jwt.sign()生成 JWT 字符串，三个参数分别是：用户信息对象、加密密钥、配置对象（expiresIn: token有效期）
        token: jwt.sign({ username: userinfo.username }, secretKey, { expiresIn: '30s'})
    })
})
```

###### 将JWT字符串还原为JSON对象

客户端每次在访问那些有权限接口的时候，都需要主动通过请求头中的 Authorization 字段，将Token字符串发送到服务器进行身份认证

此时，服务器可以通过 express-jwt 这个中间件，自动将客户端发送过来的Token解析还原成JSON对象：

```js
// 使用 app.use() 来注册中间件
// expressJWT({ secret: secretKey }) 就是用来解析 Token 的中间件
// .unless({ path: [/^\/api\//] }) 用来指定哪些接口不需要访问权限
app.use(expressJWT({ secret: secretKey }).unless({ path: [/^\/api\//] }))
```

**注意：只要配置成功了 express-jwt 这个中间件，就可以把解析出来的用户信息，挂载到 req.user 属性上，即可在那些有权限的接口中，使用req.user对象，来访问从JWT字符串中解析出来的用户信息**

```js
//这是一个有权限的API接口
app.get('/admin/getinfo', function(req, res) {
    console.log(req.user)
    res.send({
        status: 200,
        data: req.user
    })
})
```

**注意：一定不要把密码加密到 token 字符串中**

在apipost中发请求时，设置header

key：Authorization,

**注意：value值的设置**

value: 

Bearer

token字符串

###### 捕获解析JWT失败后产生的错误

当使用express-jwt解析Token字符串时，如果客户端发送过来的Token字符串**过期**或**不合法**，会产生一个**解析失败**的错误，影响项目的正常运行。我们可以通过express的错误中间件，捕获这个错误并进行相关的处理，实例代码如下

```js
app.use((err, req, res, next) => {
    //token解析失败导致的错误
    if(err.name === 'UnauthorizedError') {
        return res.send({
            status: 401,
            message: '无效的token'
        })
    }
    //其它原因导致的错误
    res.send({
        status: 500,
        message: '未知错误'
    })
})
```

### 数据库的基本使用

#### 在项目中安装

```js
npm install mysql
```

#### 通过mysql模块连接到MySQL数据库

```js
//导入 mysql 模块
const mysql = require('mysql')
//建立与 MySQL 数据库的连接
const db = mysql.createPool({
    host: '127.0.0.1',//数据库的IP地址
    user: 'root',//登录数据库的账号
    password: '1234',//登录数据库的密码
    database: 'my_db_01'//指定要操作那个数据库
})
```

#### 通过mysql模块执行SQL语句

#### mysql.js:

```js
//导入mysql模块
const mysql = require('mysql')
//建立与MySQL数据库的连接
const db = mysql.createPool({
    host: '127.0.0.1',//数据库的IP地址
    user: 'root',//登录数据库的账号
    password: 'root',//登录数据库的密码
    database: 'selectcourse'//指定要操作那个数据库
})
db.query('SELECT * FROM student', (err, results) => {
    if (err) return console.log(err.message);
    console.log(results);
})

const user = { sno: '555', sname: 'zhaoqi', ssex: 'female', spassword: '123456' }
    // const sqlStr = 'INSERT INTO student (sno, sname, ssex, spassword) VALUES (?,?,?,?)'
const sqlStr = 'INSERT INTO student SET ?'
db.query(sqlStr, user, (err, results) => {
    if (err) return console.log(err.message);
    if (results.affectedRows === 1) {
        console.log('插入数据成功');
    }
})

const student = { sno: '555', sname: 'wangqi' }
    // const sqlStr1 = 'UPDATE student SET sname = ? WHERE sno = ?'
const sqlStr1 = 'UPDATE student SET ? WHERE sno = ?'
db.query(sqlStr1, [student.sname, student.sno], (err, results) => {
        if (err) return console.log(err.message);
        if (results.affectedRows === 1) {
            console.log('更新数据成功！');
        }
    })
    // 一般不真正去删除，一般采用标记删除，当用户点击删除的时候将对应的标志位改变了就可以了
const sqlStr2 = 'DELETE FROM users WHERE sno = ?'
db.query(sqlStr2, ['555'], (err, results) => {
    if (err) return console.log(err.message);
    if (results.affectedRows === 1) {
        console.log('删除数据成功!');
    }
})
```

### express项目：API-SERVER

#### **1.1创建项目**

1.创建`api-server`文件夹作为项目根目录，并在项目根目录中运行如下的命令，初始化包管理配置文件：

```bash
npm init -y
```

2.运行如下的命令，安装特定版本的`express`

```bash
npm i express@4.17.1
```

3.在项目根目录中创建`app.js`作为整个项目的入口文件，并初始化如下的代码：

```js
//导入express模块
const expresss = require('express')
//创建express的服务器实例
const app = express()

//write your code here...

//调用app.listen方法，指定端口号并启动web服务器
app.listen(监听的端口号, function() {
	console.log('api server running at http://127.0.0.1:端口号')
})
```

#### **1.2配置cors跨域**

1.运行如下的命令，安装cors中间件：

```bash
npm i cors@2.8.5
```

2.在app.js中导入并配置cors中间件：

```js
//导入cors中间件
const cors = require('cors')
//将cors注册为全局中间件
app.use(cors())
```

#### **1.3配置解析标段数据的中间件**

1.通过如下的代码，配置解析`application/x-www-form-urlencoded`格式的表单数据的中间件:

```js
app.use(express.urlencoded({ extended: false }))
```

#### **1.4初始化路由相关的文件夹**

1.在项目根目录中，创建`router`文件夹，用来存放所有的路由模块

> 路由模块中，只存放客户端的请求与处理函数之间的映射关系

2.在项目根目录中，新建`router_handler`文件夹，用来存放所有的路由处理函数模块

> 路由处理函数模块中，专门负责存放每个路由对应的处理函数

#### **1.5初始化用户路由模块**

1.在`router`文件夹中，新建`user.js`文件，作为用户的路由模块，并初始化代码如下：

```js
const express = require('express')
//创建路由对象
const router = express.Router()

//初测新用户
router.post('/reguser', (req, res) => {
    res.send('reguser OK')
})

//登录
router.post('/login', (req, res) => {
    res.send('login OK')
})

//将路由对象共享出去
module.exports = router
```

2. 在`app.js`中，导入并使用用户路由模块：

```js
//导入并注册用户路由模块
const userRouter = require('./router/user')
app.use('/api', userRouter)
```

#### **1.6抽离用户路由模块中的处理函数**

> 目的：为了保证路由模块的纯粹性，所有的路由处理函数，必须抽离到对应的路由处理函数模块中

1.在`/router_handler/usr.js`中，使用`exports`对象，分别向外共享如下两个路由处理函数：

```js
//在这里定义和用户相关的路由处理函数，供/router/user.js模块进行调用

//注册用户的处理函数
exports.regUser = (req, res) => {
    rs.send('reguser OK')
}

//登录的处理函数
exports.login = (req, res) => {
    res.send('login OK')
}
```

2.将`/router/user.js`中的代码修改为如下结构：

```js
const express = require('express')
const router = express.Router()
// 导入用户路由处理函数模块
const userHandler = require('../router_handler/user')

//注册新用户
router.post('/reguser', userHandler.regUser)
//登录
router.post('/login', userHandler.login)

module.exports = router
```

#### **2.登录注册**

##### 2.1新建ev_users表

1.在`my_db_01`数据库中，新建`ev_users`表

##### 2.2安装并配置mysql模块

> 在API接口项目中，需要安装并配置`mysql`这个第三方模块，来连接和操作MySQL数据库

1.运行如下命令，安装`mysql`模块：

```bash
npm i mysql@2.18.1
```

2.在项目根目录中新建`/db/index.js`文件，在此自定义模块中创建数据库的连接对象：

```js
//导入mysql模块
const mysql = require('mysql')
//创建数据库连接对象
const db = mysql.createPool({
    host: '127.0.0.1',
    user: 'root',
    password: 'xxx',
    database: 'my_db_01'
})
//向外共享db数据库连接对象
module.exports = db
```

##### 2.3注册

###### 2.3.0实现步骤

1. 检测表单数据是否合法
2. 检测用户名是否被占用
3. 对密码进行加密处理
4. 插入新用户

###### 2.3.1检测表单数据是否合法

1.判断用户名和密码是否为空

```js
//接收表单数据
const userinfo = req.body
//判断数据是否合法
if (!userinfo.username || !userinfo.password) {
    return res.send({ status: 1, message: '用户名或密码不能为空！'})
}
```

###### 2.3.2检测用户名是否被占用

1.导入数据库操作模块：

```js
const db = require('../db/index')
```

2.定义SQL语句：

```js
const sql = 'select * from ev_users where username=?'
```

3.执行SQL语句并根据结果判断用户名是否被占用：

```js
db.query(dql, [userinfo.username], function (err, results) {
    //执行SQL语句失败
    if (err) {
        return res.send({status: 1, message: err.message})
    }
    //用户名被占用
    if (results.length > 0) {
        return res.send({status: 1, message: '用户名被占用，请更换其他用户名！'})
    }
    //TODO：用户名可用，继续后续流程...
})
```

###### 2.3.3对密码进行加密处理

> 为了保证密码的安全性，不建议在数据库以`明文`的形式保存用户密码，推荐密码进行`加密存储`

---

在当前项目中，使用`bcryptjs`对用户密码进行加密，优点：

- 加密之后的密码，无法被逆向破解
- 同一明文密码多次加密，得到的加密结果各不相同，保证了安全性

---

1.运行如下命令，安装指定版本的`bcryptjs`：

```bash
npm i bcryptjs@2.4.3
```

2.在`/router_handler/user.js`中，导入`brcyptjs`：

```js
const bcrypt = require('bcryptjs')
```

3.在注册用户的处理函数中，确认用户名可用之后，调用`bcrypt.hashSync(明文密码， 随机盐的长度)`方法，对用户的密码进行加密处理：

```js
//对用户的密码，进行bcrypt加密，返回值是加密之后的密码字符串
userinfo.password = bcrypt.hashSync(userinfo.password, 10)
```

###### 2.3.4插入新用户

1.定义插入用户的SQL语句：

```js
const sql = 'insert into ev_users set ?'
```

2.调用`db.query()`执行SQL语句，插入新用户：

```js
db.query(sql, { username: userinfo.username, password: userinfo.password }, function(err, results) {
    //执行SQL语句失败
    if (err) return res.send({ status: 1, message: err.message })
    //SQL语句执行成功，但影响行数不为1
    if (results.affectedRows !== 1) {
        return res.send({ status: 1, message: '注册用户失败，请稍后再试！' })
    }
    //注册成功
    res.send({ status: 0, message: '注册成功！' })
})
```

2.4优化res.send()代码

> 在处理函数中，需要多次调用`res.send()` 向客户端响应`处理失败`的结果，为了简化代码，可以手动封装一个res.cc()函数

1.在`app.js`中，所有路由之前，声明一个全局中间件，为res对象挂载一个`res.cc()`函数：

```js
//响应数据的中间件
app.use(function (req, res, next) {
    //status = 0为成功，status = 1为失败，默认将status的值设置为1，方便处理失败的情况
    res.cc = function (err, status = 1) {
        res.send({
            //状态
            status,
            //状态描述，判断err是错误对象，还是字符串
            message: err instanceof Error ? err.message : err
        })
    }
    next()
})
```

2.5优化表单数据验证

> 表单验证的原则：前端验证为辅，后端验证为主
>
> > 在实际开发中，前后端都需要对表单的数据进行合法性的验证，而且，后端作为数据合法性验证的最后一个关口，在拦截非法数据方面，起到了至关重要的作用。
> >
> > 单纯的使用if...else...的形式对数据合法性进行验证，效率低下、出错率高、维护性差。因此，推荐使用第三方数据验证模块，来降低出错率、提高验证的效率与可维护性，让后端程序员把更多的精力放在核心业务逻辑的处理上。

1.安装`@hapi/joi`包，为表单中携带得到每个数据项，定义验证规则：

```bash
npm install @hapi/joi@17.1.0
```

2.安装`@escook/express-joi`中间件，来实现自动对表单数据进行验证的功能：

```bash
npm i @escook/express-joi
```

3.新建`/schema/user.js`用户信息验证规则模块，并初始化代码如下：

```js
const joi = require('@hapi/joi')
/***
string()值必须是字符串
alphanum() 值只能是包含 a-zA-Z0-9的字符串
min(length)最小长度
max(length)最大长度
required()值是必填项，不能为undefined
pattern(正则表达式)值必须符合正则表达式的规则
***/
//用户名的验证规则
const username = joi.string().alphanum().min(1).max(10).required()
//密码的验证规则
const password = joi.string().pattern(/^[\S]{6,12}$/).required()
//注册和登录表单的验证规则对象
exports.reg_login_schema = {
    //表示需要对req.body中的数据进行验证
    body: {
        username,
        password
    }
}
```

4.修改`/router/user.js`中的代码如下：

```js
const express = require('express')
const router = express.Router()
//导入用户路由处理函数模块
const userHandler = require('../router_handler/user')

//1.导入验证表单数据的中间件
const expressJoi = require('@escook/express-joi')
//2.导入需要的验证规则对象
const { reg_login_schema } = require('../schema/user')

//注册新用户
//3.在注册新用户的路由中，声明局部中间件，对当前请求中携带的数据进行验证
//3.1数据验证通过后，会把这次请求流转给后面的路由处理函数
//3.2数据验证失败后，终止后续代码的执行，并抛出一个全局的Error错误，进入全局错误级别中间件中进行处理
router.post('/reguser', expressJoi(reg_login_schema), userHandler.regUser)
//登录
router.post('/login', userHandler.login)
module.exports = router
```

5.在`app.js`的全局错误级别中间件中，捕获验证失败错误，并把验证失败的结果响应给客户端：

```js
const joi = require('@hapi/joi')
//错误中间件
app.use(function (err, req, res, next) {
    //数据验证失败
    if (err instanceof joi.ValidationError) return res.cc(err)
    //未知错误
    res.cc(err)
})
```

##### 2.6登录

2.6.0实现步骤

1. 检测表单数据是否合法
2. 根据用户名查询用户的数据
3. 判断用户输入的密码是否正确
4. 生成JWT的Token字符串

###### 2.6.1检测登录表单的数据是否合法

1.将`/router/user.js`中`登录`的路由代码修改如下：

```js
//登录的路由
router.post('/login', expressJoi(reg_login_schema), userHandler.login)
```

###### 2.6.2根据用户名查询用户的数据

1.接收表单数据：

```js
const userinfo = req.body
```

2.定义SQL语句：

```js
const sql = 'select * from ev_users where username=?'
```

3.执行SQL语句，查询用户的数据：

```js
db.query(sql, userinfo.username, function (err, results) {
    //执行SQL语句失败
    if (err) return res.cc(err)
    //执行SQL语句成功，但是查询到数据条数不等于1
    if (results.length !== 1) return res.cc('登录失败！')
    //TODO：判断用户输入的登录密码是否和数据库中的密码一致
})
```

###### 2.6.3判断用户输入的密码是否正确

> 核心实现思路：调用`bcrypt.compareSync(用户提交的密码， 数据库中的密码)`	方法比较密码是否一致
>
> 返回值是布尔值（true一致、false不一致）

具体的实现代码如下：

```js
//拿着用户输入的密码和数据库中存储的密码进行对比
const compareResult = bcrypt.compareSync(userinfo.password, results[0].password)
//如果对比的结果等于false,则证明用户输入的密码错误
if (!compareResult) {
    return res.cc('登录失败！')
}
//TODO: 登录成功，生成Token字符串
```

###### 2.6.4生成JWT的Token字符串

> 核心注意点：在生成Token字符串的时候，一定要剔除密码和头像的值

1.通过ES6的高级语法，快速剔除`密码`和`头像`的值：

```js
//剔除完毕之后，user中只保留了用户的id, username, nickname, email 这四个属性的值
const user = { ...results[0], password: '',user_pic: '' }
```

2.运行如下的命令，安装生成Token字符串的包：

```bash
npm i jsonwebtoken@8.5.1
```

3.在`/router_handler/user.js`模块的头部区域，导入`jsonwebtoken`包：

```js
//用这个 包生成Token字符串
const jwt = require('jsonwebtoken')
```

4.创建`config.js`文件，并向外共享加密和还原Token的`jwtSecretKey`字符串：

```js
module.exports = {
    jwtSecretKey: 'xxx'
}
```

5.将用户信息对象加密成Token字符串：

```js
//导入配置文件
const config = require('../config')
//生成Token字符串
const tokenStr = jwt.sign(user, config.jwtSecretKey,{expiresIn: '10h'})//token有效期为10个小时
```

6.将生成的Token字符串响应给客户端

```js
res.send({
    status: 0,
    message: '登录成功！',
    //为了方便客户端使用Token，再服务器端直接拼接上Bearer的前缀
    token: 'Bearer ' + tokenStr
})
```

##### 2.7配置解析Token的中间件

1.运行如下的命令，安装解析Token的中间件：

```bash
npm i express-jwt@5.3.3
```

2.在`app.js`中注册路由之前，配置解析Token的中间件：

```js
//导入配置文件
const config = require('./config')
//解析token的中间件
const expressJWT = require('express-jwt')
//使用.unless({path: [/^\/api\//]})指定哪些接口不需要进行Token的身份认证
app.use(expressJWT({ secret: config.jwtSecretKey }).unless({path: [/^\/api\//]}))
```

3.在`app.js`中的`错误级别中间件`里面，捕获并处理Toekn认证失败后的错误：

```js
//错误中间件
app.use(funcion (err, req, res, next) {
        //省略其他代码...
        //捕获身份认证失败的错误
        if (err.name === 'UnauthorizedError') return res.cc('身份认证失败！')
		//未知错误...
        })
```

#### **3.个人中心**

##### 3.1获取用户的基本信息

###### 3.1.0实现步骤

1. 初始化路由模块
2. 初始化路由处理函数模块
3. 获取用户的基本信息

###### 3.1.1初始化路由模块

1.创建`/router/userinfo.js`路由哦模块，并初始化如下的代码结构:

```js
//导入express
const express = require('express')
//创建路由对象
const router = express.Router()
//获取用户的基本信息
router.get('/userinfo', (req, res) => {
	res.send('ok')
})
//向外共享路由对象
module.exports = router
```

2.在`app.js`中导入并使用个人中心的路由模块：

```js
//导入并使用用户信息路由模块
const userinfoRouter = require('./router/userinfo')
//注意： 以 /my 开头的接口，都是有权限的接口，需要进行Token身份认证
app.use('/my', userinfoRouter)
```

###### 3.1.2初始化路由处理函数模块

1.创建 `/router_handler/userinfo.js`路由处理函数模块，并初始化如下的代码结构：

```js
//获取用户基本信息的处理函数
exports.getUserInfo = (req, res) => {
    res.send('ok')
}
```

2.修改`/router/userinfo.js`中的代码如下：

```js
const express = require('express')
const router = express.Router()
//导入用户信息的处理函数模块
const userinfo_handler = require('../router_handler.userinfo')
//获取用户的基本信息
router.get('/userinfo', userinfo_handler.getUserInfo)
module.exports = router
```

###### 3.1.3获取用户的基本信息

1.在`/router_handler/userinfo.js`头部导入数据库操作模块：

```js
// 导入数据库操作模块
const db = require('../db/index')
```

2.定义SQL语句：

```js
//根据用户的id,查询用户的基本信息
//注意：为了防止用户的密码泄露，需要排除password字段
const sql = 'select id, username, nickname, email, user_pic from ev_users where id=?'
```

3.调用`db.query	()`执行SQL语句：

```js
//注意：req对象上的user属性，是Token解析成功，express-jwt中间件帮我们挂载上去的
db.query(sql, req.user.id, (err, results) => {
    //1.执行SQL语句失败
    if (err) return res.cc(err)
    //2.执行SQL语句成功，但是查询到的数据条数不等于1
    if (results.length !== 1) return res.cc('获取用户信息失败！')
    //3.将用户信息响应给客户端
    res.send({
        status: 0,
        message: '获取用户基本信息成功！'，
        data: results[0]
    })
})
```

##### 3.2实现步骤

1. 定义路由和处理函数
2. 验证表单数据
3. 实现跟新用户基本信息的功能

###### 3.2.1定义路由和处理函数

1.在`/router/userinfo.js`模块中，新增`更新用户基本信息`的路由：

```js
//更新用户的基本信息
router.post('/userinfo', userinfo_handler.updaateUserInfo)
```

2.在`/router_handler/userinfo.js`模块中，定义并向外共享`更新用户基本信息`的路由处理函数：

```js
//更新用户基本信息的处理函数
exports.updateUserInfo = (req, res) => {
    res.send('ok')
}
```

###### 3.2.2验证表单数据

1.在`/schema/user.js`验证规则模块中，定义`id`, `nickname`, `email`的验证规则如下：

```js
//定义id,nickname, email的验证规则
const id = joi.number().integer().min(1).required()
const nickname = joi.string().required()
const email = joi.string().email().required()
```

2.并使用`exports`向外共享如下的`验证规则对象：`

```js
//验证规则对象 - 更新用户基本信息
exports.update_userinfo_schema = {
    body: {
        id,
        nickname,
        email
    }
}
```

3.在`/router/userinfo.js`模块中，导入验证数据合法性的中间件：

```js
//导入验证数据合法性的中间件
const = expressJoi = require('@escook/express-joi')
```

4.在`/router/userinfo.js`模块中，导入需要的验证规则对象：

```js
//导入需要的验证规则对象
const { update_userinfo_schema } = require('../schema.user')
```

5.在`/router/userinfo.js`模块中，修改`更新用户的基本信息`的路由如下：

```js
//更新用户的基本信息
router.post('/userinfo', expressJoi(update_userinfo_schema), userinfo_handler.updateUserInfo)
```

###### 3.2.3实现更新用户基本信息的功能

1.定义待执行的SQL语句：

```js
const sql = 'update ev_users set ? where id=?'
```

2.调用`db.query()`执行SQL语句并传参：

```js
db.query(sql, [req.body, req.body.id], (err, results) => {
    //执行SQL语句失败
    if (err) return res.cc(err)
    //执行SQL语句成功，但是影响行数不为1
    if (results.affectedRows !== 1) return res.cc('修改用户基本信息失败！')
    //修改用户基本信息成功
    return res.cc('修改用户基本信息成功！', 0)
})
```

##### **3.3重置密码**

###### 3.3.0实现步骤

1. 定义路由和处理函数
2. 验证表单数据
3. 实现重置密码的功能

###### 3.3.1定义路由和处理函数

1.在`/router/userinfo.js`模块中，新增`重置密码`的路由：

```js
//重置密码的路由
router.post('/updatepwd', userinfo_handler.updatePassword)
```

2.在`/router_handler/userinfo.js`模块中，定义并向外共享`重置密码`的路由处理函数：

```js
//重置密码的处理函数
exports.updatePassword = (req, res) => {
    res.send('ok')
}
```

###### 3.3.2验证表单数据

> 核心验证思路：旧密码与新密码，必须符合密码的验证规则，并且新密码不能与旧密码一致！

1.在`/schema/user.js`模块中，使用`exports`向外共享如下的`验证规则对象`:

```js
//验证规则对象 - 重置密码
exports.update_password_schema = {
    body: {
        //使用password这个规则，验证req.body.oldPwd的值
        oldPwd: password,
       //使用joi.not(joi.ref('oldPwd')).concat(password)规则，验证req.body.newPwd的值
       //解读：
       //1.joi.ref('oldPwd')表示newPwd的值必须和oldPwd的值保持一致
       //2.joi.not(joi.ref('oldPwd'))表示newPwd的值不能等于oldPwd的值
       //3. .concat()用于合并joi.not(joi.ref('oldPwd'))和password这两条验证规则
        newPwd: joi.not(joi.ref('oldPwd')).concat(password)
    }
}
```

2.在`/router/userinfo.js`模块中，导入需要的验证规则对象：

```js
//导入需要的验证规则对象
const { update_userinfo_schema, update_password_schema } = require('../schema/user')
```

3.并在`重置密码的路由`中，使用`update_password_schema`规则验证表单的数据，实例代码如下：

```js
router.post('/updatepwd', expressJoi(update_password_schema), userinfo_handler.updatePassword)
```

###### 3.3.3实现重置密码的功能

1.根据`id`查询用户是否存在：

```js
//定义根据id查询用户数据的SQL语句
const sql = 'select * from ev_users where id=?'
//执行SQL语句查询用户是否存在
db.query(sql, req.user.id, (err, results) => {
    //执行SQL语句失败
    if (err) return res.cc(err)
    //检查指定id的用户是否存在
    if (results.length !== 1) return res.cc('用户不存在！')
    // TODO:判断提交的旧密码是否正确
})
```

2.判断提交的旧密码是否正确：

```js
//在头部区域导入bcryptjs后
//即可使用bcrypt.compareSync(提交的密码，数据库中的密码) 方法验证密码是否正确
//compareSync()函数的返回值为布尔值，true表示密码正确，false表示密码错误
const bcrypt = require('bcryptjs')
//判断提交的旧密码是否正确
const compareResult = bcrypt.compareSync(req.body.oldPwd, results[0].password)
if (!compareResult) return res.cc('原密码错误!')
```

3.对新密码进行`bcrpt`加密之后，更新到数据库中：

```js
//定义更新用户密码的SQL语句
const sql = 'update ev_users set password=? where id=?'
//对新密码进行bcrypt加密处理
const newPwd = bcrypt.hashSync(req.body.newPwd, 10)
//执行SQL语句，根据id更新用户的密码
db.query(sql, [newPwd, req.user.id], (err, results) => {
    //SQL 语句执行失败
    if (err) return res.cc(err)
    //SQL语句执行成功，但是影响行数不等于1
    if (results.affectedRows !== 1) return res.cc('更新密码失败！')
    //更新密码成功
    res.cc('更新密码成功！', 0)
})
```

##### **3.4更新用户头像**

###### 3.4.0实现步骤

1. 定义路由和处理函数
2. 验证表单数据
3. 实现更新用户头像的功能

###### 3.4.1定义路由和处理函数

1.在`/router/userinfo.js`模块中，新增`更新用户头像`的路由：

```js
//更新用户头像的路由
router.post('/update/avatar', userinfo_handler.updateAvatar)
```

2.在`/router_handler/userinfo.js`模块中，定义并向外共享`共享用户头像`的路由处理函数：

```js
//更新用户头像的处理函数
exports.updateAvatar = (req, res) => {
    res.send('ok')
}
```

###### 3.4.2验证表单数据

1.在`/schema/user.js`验证规则模块中，定义`avatar`的验证规则如下：

```js
//dataUri()指的是如下格式的字符串数据：
//data:image/png;base64,VE9PTUFOWVNFQ1JFVFM=
const avatar = joi.string().dataUri().required()
```

2.并使用`exports`向外共享如下的`验证规则对象`：

```js
//验证规则对象 - 更新头像
exports.update_avatar_schema = {
    body: {
        avatar
    }
}
```

3.在`/router/userinfo.js`模块中，导入需要的验证规则对象：

```js
const { update_avatar_schema } = require('../schema/user')
```

4.在`/router/userinfo.js`模块中，修改`更新用户头像`的路由如下：

```js
router.post('/update/avatar', expressJoi(update_avatar_schema), userinfo_handler.updateAvatat)
```

###### 3.4.3实现更新用户头像的功能

1.定义更新用户头像的SQL语句：

```js
const sql = 'update ev_users set user_pic=? where id=?'
```

2.调用`db.query()`执行SQL语句，更新对应用户的头像：

```js
db.query(sql, [req.body.avatar, req.user.id], (err, results) => {
    //执行SQL语句失败
    if (err) return res.cc(err)
    //执行SQL语句成功，但是影响行数不等于1
    if (results.affectedRows !== 1) return res.cc('更新头像失败！')
    //更新用户头像成功
    return res.cc('更新头像成功！', 0)
})
```

#### 接口文档

https://www.showdoc.com.cn/2224230275005665/9988970324953669

#### 项目结构：![项目结构](https://raw.githubusercontent.com/ZhangjunqiGithub/picture/master/project_structure.jpg)

**db/index.js：里面写着数据库的相关配置**

**node_modules: 里面存放着所有的包**

**router: 每一个js文件中都定义着各种路由接口API（但是没有具体实现代码，具体实现代码在router_handler下对应的js文件中）**

**router_handler: 里面写的是router下每个路由的具体实现代码**

**schema/app.js: 里面定义了各种验证规则**

**config: 这是一个全局的配置文件，里面写着加密和解密token的密钥和token的有效期**

#### db/index.js:

**数据库的引入**

```js
//导入 mysql 模块
const mysql = require('mysql')
//建立与 MySQL 数据库的连接
const db = mysql.createPool({
    host: '127.0.0.1',//数据库的IP地址
    user: 'xxxx',//登录数据库的账号
    password: 'xxxx',//登录数据库的密码
    database: 'xxxx',//指定要操作那个数据库
    multipleStatements: true // 支持执行多条 sql 语句
})

module.exports = db//向外暴露出db模块
```

##### router/user.js:

```js
//导入express模块
const express = require('express')
//创建路由对象
const router = express.Router()

// 导入用户路由处理函数对应的模块
const user_handler = require('../router_handler/user')

// 导入验证数据的中间件
const expressjoi = require('@escook/express-joi')

// 导入需要的验证规则对象
const { reg_login_schema } = require('../schema/user')

// 注册新用户
router.post('/reguser', expressjoi(reg_login_schema), user_handler.regUser)

// 登录
router.post('/login', expressjoi(reg_login_schema), user_handler.login)
//向外导出路由对象
module.exports = router
```

##### router/userinfo.js:

```js
//导入express模块
const express = require('express')
//创建路由对象
const router = express.Router()

// 挂载路由

// 导入路由处理函数模块
const userinfo_handler = require('../router_handler/userinfo')

// 导入验证数据的中间件
const expressjoi = require('@escook/express-joi')

// 导入需要的验证数据规则对象
const { update_userinfo_schema, update_password_schema, update_avatar_schema } = require('../schema/user')

// 获取用户基本信息的路由
router.get('/userinfo', userinfo_handler.getUserInfo)

// 更新用户信息的路由
router.post('/userinfo', expressjoi(update_userinfo_schema), userinfo_handler.updateUserInfo)

// 更新密码的路由
router.post('/updatepwd', expressjoi(update_password_schema), userinfo_handler.updatePassword)

// 更换头像的路由
router.post('/update/avatar', expressjoi(update_avatar_schema), userinfo_handler.updateAvatar)

module.exports = router
```

##### router_handler/user.js:

```js
// 导入数据库操作模块
const db = require('../db/index')

// 导入密码加密的包
const bcrypt = require('bcryptjs')
    // 导入生成Token的包
const jwt = require('jsonwebtoken')
const config = require('../config')
exports.regUser = (req, res) => {
    // 获取客户端提交到服务器的用户信息
    const userinfo = req.body

    // 对表单中的数据，进行合法性的校验
    if (!userinfo.username || !userinfo.password) {
        return res.send({
            status: 1,
            message: '用户名或密码不合法'
        })
    }
    // 定义 SQL 语句，查询用户明是否被占用
    const sqlStr = 'select * from ev_users where username = ?'
    db.query(sqlStr, userinfo.username, (err, results) => {
        // 执行SQL语句失败
        if (err) {
            // return res.send({
            //     status: 1,
            //     message: err.message
            // })
            return res.cc(err)
        }
        // 判断用户名是否被占用
        if (results.length > 0) {
            // return res.send({
            //     status: 1,
            //     message: '用户名被占用，请更换其他用户名！'
            // })
            return res.cc('用户名被占用，请更换其他用户名！')
        }
        // 调用 bcrypt.hashSync()对密码进行加密
        userinfo.password = bcrypt.hashSync(userinfo.password, 10)
            // 定义插入新用户的SQL语句
        const sqlstr = 'alter table ev_users drop id;alter table ev_users add id int(11) primary key auto_increment first'//解决因删除导致id断层的问题
        const sql = 'insert into ev_users set ?'
            // 调用db.query()执行SQL语句
        db.query(sqlstr, (err, results) => {
            if (err) {
                return res.cc(err)
            }
        })
        db.query(sql, { username: userinfo.username, password: userinfo.password }, (err, results) => {
            // 判断执行SQL语句是否成功
            if (err) {
                // return res.send({
                //     status: 1,
                //     message: err.message
                // })
                return res.cc(err)
            }
            // 判断影响行数是否为1
            if (results.affectedRows !== 1) {
                // return res.send({
                //     status: 1,
                //     message: '注册用户失败，请稍后再试!'
                // })
                return res.cc('注册用户失败，请稍后再试!')
            }

            // 注册用户成功
            // res.send({
            //     status: 0,
            //     message: '注册成功!'
            // })
            res.cc('注册成功!', 0)
        })
    })
}

exports.login = (req, res) => {
    // 获取客户端提交到服务器的用户信息
    const userinfo = req.body
        // 接收表单的数据
        // 定义SQL语句
    const sql = 'select * from ev_users where username = ?'
        // 执行SQL语句，根据用户名查询用户的信息
    db.query(sql, userinfo.username, (err, results) => {
        // 执行SQL语句失败
        if (err) {
            return res.cc(err)
        }
        // 执行SQL语句成功，但是获取到的数据台哦书不等于 1
        if (results.length !== 1) {
            return res.cc('登录失败!')
        }
        //判断密码是否正确
        const compareResult = bcrypt.compareSync(userinfo.password, results[0].password)
        if (!compareResult) return res.cc('登录失败!')

        //TODO: 在服务器端生成Token的字符串
        const user = {...results[0], password: '', user_pic: '' }
            // 对用户的信息进行加密，生成Token字符串
        const tokenStr = jwt.sign(user, config.jwtSecretKey, { expiresIn: config.expiresIn })
            // 调用res.send()将Token响应给客户端
        res.send({
            status: 0,
            message: '登录成功!',
            // 空格不能少
            token: 'Bearer ' + tokenStr
        })
    })
}
```

##### router_handler/userinfo.js:

```js
// 导入数据库操作模块
const db = require('../db/index')

// 导入处理密码的模块
const bcrypt = require('bcryptjs')
const { regUser } = require('./user')

// 获取用户基本信息的处理函数
exports.getUserInfo = (req, res) => {
    // 定义查询用户信息的SQL语句
    const sql = 'select id, username, nickname, email, user_pic from ev_users where id = ?'
    db.query(sql, req.user.id, (err, results) => {
        // 执行SQL语句失败
        if (err) {
            return res.cc(err)
        }
        // 执行SQL语句成功，但是查询的结果可能为空
        if (results.length !== 1) {
            return res.cc('获取用户信息失败!')
        }
        // 用户信息获取成功
        res.send({
            status: 0,
            message: '获取用户信息成功!',
            data: results[0]
        })
    })
}

// 更新用户基本信息的处理函数
exports.updateUserInfo = (req, res) => {
    // 定义待执行的SQL语句
    const sql = 'update ev_users set ? where id = ?'
    console.log(req.body);

    // 调用db.query()执行SQL语句并传递参数
    db.query(sql, [req.body, req.body.id], (err, results) => {
        // 执行SQL语句失败
        if (err) {
            return res.cc(err)
        }

        // 执行SQL语句成功，但是影响行数不等于1
        if (results.affectedRows !== 1) {
            return res.cc('更新用户的基本信息失败！')
        }
        // 成功
        res.cc('更新用户信息成功！', 0)
    })
}

// 更新用户密码的处理函数
exports.updatePassword = (req, res) => {
    // 根据 id 查询用户的信息
    const sql = 'select * from ev_users where id = ?'
        // 执行根据 id 插叙用户的信息的 SQL 语句
    db.query(sql, req.user.id, (err, results) => {
        // 执行 SQL 语句失败
        if (err) {
            return res.cc(err)
        }
        // 判断结果是否存在
        if (results.length !== 1) {
            return res.cc('用户不存在！')
        }
        // 判断用户输入的旧密码是否正确
        const compareResult = bcrypt.compareSync(req.body.oldPwd, results[0].password)
        if (!compareResult) {
            return res.cc('旧密码错误！')
        }

        // 定义更新密码的 SQL 语句
        const sql = 'update ev_users set password = ? where id = ?'
            // 对新密码进行加密处理
        const newPwd = bcrypt.hashSync(req.body.newPwd, 10)
            // 调用db.query()执行SQL语句
        db.query(sql, [newPwd, req.user.id], (err, results) => {
            // 执行SQL语句失败
            if (err) {
                return res.cc(err)
            }
            // 判断影响行数
            if (results.affectedRows !== 1) {
                return res.cc('更新密码失败！')
            }
            // 成功
            res.cc('更新密码成功', 0)
        })
    })
}

// 更新用户头像的处理函数
exports.updateAvatar = (req, res) => {
    // 定义更新头像的SQL语句
    const sql = 'update ev_users set user_pic = ? where id = ?'
        // 调用db.query()执行SQL语句
    db.query(sql, [req.body.avatar, req.user.id], (err, results) => {
        // 执行SQL语句失败
        if (err) {
            return res.cc(err)
        }

        // 执行SQL语句成功，但是影响行数不等于1
        if (results.affectedRows !== 1) {
            return res.cc('更换头像失败！')
        }
        // 成功
        res.cc('更换头像成功！', 0)
    })
}
```

##### schema/user.js:

```js
//导入数据校验模块
//joi简介：
/*
joi是一个强大的数据校验模块，可以对数据进行格式和数据类型上的校验，支持 正则表达式 ，功能非常强大，可以很方便地在后端对客户端返回的表单数据进行校验。
*/
const joi = require('joi')
    // string() 值必须是字符串
    // alphanum() 值只能是包含 a-zA-Z0-9
    // min(length) 最小长度
    // max(length) 最大长度
    // required() 值是必填项，不能为undefined
    // pattern(正则表达式) 值必须符合正则表达式的规则

// 用户名的验证规则
const username = joi.string().alphanum().min(1).max(10).required()

// 密码的验证规则
const password = joi.string().pattern(/^[\S]{6,12}$/).required()

// 定义 id,nickname,email的验证规则
const id = joi.number().integer().min(1).required()
const nickname = joi.string().required()
const user_email = joi.string().email().required()

// 定义验证 avatar 头像的验证规则
const avatar = joi.string().dataUri().required()

const userpic = joi.string().required()
// 注册和登录表单的验证规则对象
exports.reg_login_schema = {
    // 表示需要对req.body中的数据进行验证
    body: {
        username,
        password
    }
}

// 验证规则对象 - 更新用户基本信息
exports.update_userinfo_schema = {
    // 需要对 req.body里面的数据进行验证
    body: {
        userpic,
        nickname,
        email: user_email
    }
}

// 验证规则对象 - 更新密码
exports.update_password_schema = {
    body: {
        oldPwd: password,
        newPwd: joi.not(joi.ref('oldPwd')).concat(password)
    }
}

// 验证规则对象 - 更新头像
exports.update_avatar_schema = {
    body: {
        avatar
    }
}
```

##### app.js:

```js
// 导入express
const express = require('express')

// 创建服务器的实例对象
const app = express()

const joi = require('joi')

// 导入并配置cors 中间件
const cors = require('cors')
app.use(cors())

// 配置解析表单数据的中间件
app.use(express.urlencoded({ extended: false }))

// 一定要在路由之前封装res.cc函数
app.use((req, res, next) => {
        // status默认值为1，表示失败的情况
        // err的值，可能是一个错误对象，也可能是一个错误的描述字符串
        res.cc = function(err, status = 1) {
            res.send({
                status,
                message: err instanceof Error ? err.message : err
            })
        }
        next()
    })
// 一定要在路由之前配置解析Token的中间件
const expressJWT = require('express-jwt')
const config = require('./config')

app.use(expressJWT({
    secret: config.jwtSecretKey
}).unless({ path: [/^\/api/] }))

// 导入并使用用户路由模块
const userRouter = require('./router/user')
//在访问./router/user中的路由的时候，需要在根路径后面加上 /api，后面再加上具体的路由URL
app.use('/api', userRouter)

// 导入并使用用户信息的路由模块
const userinfoRouter = require('./router/userinfo')
//在访问./router/userinfo中的路由的时候，需要在根路径后面加上 /my，后面再加上具体的路由URL
app.use('/my', userinfoRouter)

// 定义错误级别的中间件
app.use((err, req, res, next) => {
    // 验证失败导致的错误
    if (err instanceof joi.ValidationError) {
        return res.cc(err)
    }
    // 身份认证失败后的错误
    if (err.name === 'UnauthorizedError') {
        return res.cc('身份认证失败!')
    }
    // 未知的错误
    res.cc(err)
})

// 启动服务器
app.listen(8080, () => {
    console.log('api server running at http:127.0.0.1:8080');
})
```

##### config.js:

```js
// 这是一个全局的配置文件

module.exports = {
    //加密和解密Token的密钥
    jwtSecretKey: 'itheima',
    // token的有效期
    expiresIn: '10h'
}
```

**package.json**

```js
{
  "dependencies": {
    "@escook/express-joi": "^1.1.1",
    "bcryptjs": "^2.4.3",
    "cors": "^2.8.5",
    "express": "^4.17.1",
    "express-jwt": "^5.3.3",
    "joi": "^17.6.0",
    "jsonwebtoken": "^8.5.1",
    "mysql": "^2.18.1"
  }
}
```



#### 项目路由详解：

##### 注册新用户（reguser）

**路由：**

```js
router.post('/reguser', expressjoi(reg_login_schema), user_handler.regUser)
```

**请求方法：**post

**请求URL：**/reguser

**joi数据验证模块的基本用法：**

1、安装 @hapi/joi 包，为表单中携带的每个数据项，定义验证规则

```js
npm install @hapi/joi@17.1.0
```

2、安装 @escook/express-joi 中间件，来实现自动对表单数据进行验证的功能

```js
npm install @escook/express-joi
```

3、导入验证数据的中间件

```js
const expressjoi = require('@escook/express-joi')
```

4、导入定义验证规则的中间件

```js
const joi = require('joi')
```

```js
// string() 值必须是字符串
// alphanum() 值只能是包含 a-zA-Z0-9
// min(length) 最小长度
// max(length) 最大长度
// required() 值是必填项，不能为undefined
// pattern(正则表达式) 值必须符合正则表达式的规则

//eg:定义用户名的验证规则
const username = joi.string().alphanum().min(1).max(10).required()
//解释： 1、必须是字符串
//		2、只能是包含 a-zA-Z0-9
//		3、1=< 长度 <=10
//		4、为必填项，不能为undefined

//eg:密码的验证规则
const password = joi.string().pattern(/^[\S]{6,12}$/).required()
//解释： 1、必须是字符串
//		2、正则表达式匹配规则，长度必须为 6 ~ 12 之间
//		3、为必填项，不能为undefined
```

**用户名的验证规则**

```js
const username = joi.string().alphanum().min(1).max(10).required()
```

**密码的验证规则**

```js
const password = joi.string().pattern(/^[\S]{6,12}$/).required()
```

**注册和登录表单的验证规则对象**

```js
exports.reg_login_schema = {
// 表示需要对req.body中的数据进行验证
  body: {
    username,
    password
  }
}
```

 **一定要在路由之前封装res.cc函数**

将该函数注册为全局中间件：

```js
app.use((req, res, next) => {
    // status默认值为1，表示失败的情况
    // err的值，可能是一个错误对象，也可能是一个错误的描述字符串
    res.cc = function(err, status = 1) {
      res.send({
        status,
        message: err instanceof Error ? err.message : err
      })
    }
    next()
  })
```

**在app.js中定义错误级别的中间件:**

用于捕获验证失败的错误，并把验证失败的结果响应给客户端

```js
// 定义错误级别的中间件
app.use((err, req, res, next) => {
    // 验证失败导致的错误
    if (err instanceof joi.ValidationError) {
        return res.cc(err)
    }
    // 身份认证失败后的错误
    if (err.name === 'UnauthorizedError') {
        return res.cc('身份认证失败!')
    }
    // 未知的错误
    res.cc(err)
})
```

**当expressjoi(reg_login_schema)数据验证失败后，会终止后续代码的执行，并抛出一个全局的 Error 错误，进入全局错误级别的中间件中进行处理。**

**具体实现函数：**

```js
exports.regUser = (req, res) => {
    // 获取客户端提交到服务器的用户信息
    const userinfo = req.body

    // 对表单中的数据，进行合法性的校验
    if (!userinfo.username || !userinfo.password) {
        return res.send({
            status: 1,
            message: '用户名或密码不合法'
        })
    }
    // 定义 SQL 语句，查询用户明是否被占用
    const sqlStr = 'select * from ev_users where username = ?'
    db.query(sqlStr, userinfo.username, (err, results) => {
        // 执行SQL语句失败
        if (err) {
            // return res.send({
            //     status: 1,
            //     message: err.message
            // })
            return res.cc(err)
        }
        // 判断用户名是否被占用
        if (results.length > 0) {
            // return res.send({
            //     status: 1,
            //     message: '用户名被占用，请更换其他用户名！'
            // })
            return res.cc('用户名被占用，请更换其他用户名！')
        }
        // 调用 bcrypt.hashSync()对密码进行加密
        userinfo.password = bcrypt.hashSync(userinfo.password, 10)
            // 定义插入新用户的SQL语句
        const sql = 'insert into ev_users set ?'
            // 调用db.query()执行SQL语句
        db.query(sql, { username: userinfo.username, password: userinfo.password }, (err, results) => {
            // 判断执行SQL语句是否成功
            if (err) {
                // return res.send({
                //     status: 1,
                //     message: err.message
                // })
                return res.cc(err)
            }
            // 判断影响行数是否为1
            if (results.affectedRows !== 1) {
                // return res.send({
                //     status: 1,
                //     message: '注册用户失败，请稍后再试!'
                // })
                return res.cc('注册用户失败，请稍后再试!')
            }

            // 注册用户成功
            // res.send({
            //     status: 0,
            //     message: '注册成功!'
            // })
            res.cc('注册成功!', 0)
        })
    })
}
```

**功能描述：**

获取客户端提交到服务器的用户信息，并存到 userinfo 中，然后对数据进行合法性验证，如果用户名（userinfo.username）或密码（userinfo.password）为空，则返回一个消息对象，状态（status）为 1 ，错误消息（message）为 ‘用户名或密码不合法’；如果用户名和密码都不为空则执行相应的SQL语句，从数据库中通过用户名（userinfo.username）进行查询，如果执行SQL语句失败，就通过全局res.cc函数返回错误对象err；如果用户名被占用，就通过res.cc返回一个 ‘用户名被占用，请更换其他用户名！’字符串；如果以上问题都没有，就调用 bcrypt.hashSync()对密码进行加密；然后执行相应的SQL语句，将从前端获取来的用户名和密码保存到数据库中，如果执行SQL语句失败，则通过res.cc返回错误对象err；然后判断数据库中的影响行数是否为 1 ，如果不为 1 ，则通过res.cc返回一个 ‘注册用户失败，请稍后再试!’ 的字符串；如果以上错误都没有出现，就通过res.cc返回一个 ‘注册成功！’ 的字符串和状态为0。

**调用 bcrypt.hashSync()对密码进行加密**

为了保证密码的安全性，不建议在数据库中以 **明文** 的形式保存用户密码，推荐对密码进行 **加密存储**

在当前项目中，使用 bcryptjs 对用户密码尽心加密，优点：

1、加密之后的密码，无法逆向破解

2、同一明文密码多次加密，得到的加密结果各不相同，保证了安全性

**使用方法：**

1、运行如下命令，安装指定版本的 bcryptjs :

```js
npm i bcryptjs@2.4.3
```

2、在router_handler/user.js中导入 bcryptjs:

```js
const bcrypt = require('bcryptjs')
```

3、在注册用户的处理函数中，确认用户名可用之后，调用 bcrypt.hashSync(明文密码, 随机盐的长度) 方法，对用户的密码进行加密处理

```js
// 调用 bcrypt.hashSync()对密码进行加密
userinfo.password = bcrypt.hashSync(userinfo.password, 10)
```

#####  登录（login）

**路由：**

```js
router.post('/login', expressjoi(reg_login_schema), user_handler.login)
```

**请求方法：**post

**请求URL：**/login

**具体实现函数：**

```js
exports.login = (req, res) => {
    // 获取客户端提交到服务器的用户信息
    const userinfo = req.body
        // 接收表单的数据
        // 定义SQL语句
    const sql = 'select * from ev_users where username = ?'
        // 执行SQL语句，根据用户名查询用户的信息
    db.query(sql, userinfo.username, (err, results) => {
        // 执行SQL语句失败
        if (err) {
            return res.cc(err)
        }
        // 执行SQL语句成功，但是获取到的数据条数不等于 1
        if (results.length !== 1) {
            return res.cc('登录失败!')
        }
        //判断密码是否正确
        const compareResult = bcrypt.compareSync(userinfo.password, results[0].password)
        if (!compareResult) return res.cc('登录失败!')

        //TODO: 在服务器端生成Token的字符串
        const user = {...results[0], password: '', user_pic: '' }
            // 对用户的信息进行加密，生成Token字符串
        const tokenStr = jwt.sign(user, config.jwtSecretKey, { expiresIn: config.expiresIn })
            // 调用res.send()将Token响应给客户端
        res.send({
            status: 0,
            message: '登录成功!',
            // 空格不能少
            token: 'Bearer ' + tokenStr
        })
    })
}
```

**核心代码解释：**

**判断密码是否正确**

```js
//此过程会将数据库中查到的已加密的密码进行解密然后再与从前端获取的密码进行比较
const compareResult = bcrypt.compareSync(userinfo.password, results[0].password)
if (!compareResult) return res.cc('登录失败!')
```

注意：通过db.query(sql, userinfo.username, (err, results) => {...})获得的results是一个数组

**在服务器端生成Token的字符串**

1、通过ES6的高级语法，快速剔除 密码 和 头像 的值

```js
//这里用到了es6的展开运算符，然后将 密码（password）和头像（user_pic）给剔除掉
const user = {...results[0], password: '', user_pic: '' }
```

2、运行如下命令，安装生成 Token 字符串的包：

```js
npm i jsonwebtoken@8.5.1
```

3、在/router_handler/user.js 模块的头部区域，导入 **jsonwebtoken** 包

```js
//用这个包来生成 Token 字符串
const jwt = require('jsonwebtoken')
```

4、创建 config.js 文件，并向外共享 加密 和还原 Token 的 jwtSecretKey 字符串：

```js
// 这是一个全局的配置文件
module.exports = {
    //加密和解密Token的密钥
    jwtSecretKey: 'itheima',
    // token的有效期
    expiresIn: '10h'
}
```

5、将用户消息对象加密成Token字符串

```js
//导入配置文件
const config = require('../config')
//生成 Token 字符串
const tokenStr = jwt.sign(user, config.jwtSecretKey, { expiresIn: config.expiresIn })
```

6、将生成的 Token 字符串响应给客户端

```js
res.send({
    status: 0,
    message: '登录成功！',
    //为了方便客户端使用 Token,在服务器端直接拼接上 Bearer 的前缀
    //注意Bearer后的空格不能少
    token: 'Bearer ' + tokenStr
})
```

完整代码：

```js
//这里用到了es6的展开运算符，然后将 密码（password）和头像（user_pic）给剔除掉
const user = {...results[0], password: '', user_pic: '' }
            // 对用户的信息进行加密，生成Token字符串
        const tokenStr = jwt.sign(user, config.jwtSecretKey, { expiresIn: config.expiresIn })
            // 调用res.send()将Token响应给客户端
        res.send({
            status: 0,
            message: '登录成功!',
            // 空格不能少
            token: 'Bearer ' + tokenStr
        })
```

**配置解析 Token的中间件**

1、运行如下的命令，安装解析 Token 的中间件：

```js
npm i express-jwt@5.3.3
```

2、在app.js中注册路由之前，配置解析Token的中间件

```js
//导入配置文件
const config = require('./config')

//解析token的中间件
const expressJWT = require('express-jwt')

//使用 .unless({ path: [/^\/api/] })指定哪些接口不需要进行 Token 的身份认证
app.use(expressJWT({
    secret: config.jwtSecretKey
}).unless({ path: [/^\/api/] }))
```

3、在 app.js 中的错误级别的中间件里面，捕获并处理Token认证失败后的错误：

```js
//错误中间件
app.use((err, req, res, next) => {
    // 验证失败导致的错误
    if (err instanceof joi.ValidationError) {
        return res.cc(err)
    }
    // 身份认证失败后的错误
    if (err.name === 'UnauthorizedError') {
        return res.cc('身份认证失败!')
    }
    // 未知的错误
    res.cc(err)
})
```

**功能描述：**

获取客户端提交到服务器的用户信息保存到userinfo中，执行相应的SQL语句根据用户名查询用户的信息，如果执行SQL语句失败，就通过全局res.cc函数返回错误对象err；然后判断数据库中的影响行数是否为 1 ，如果不为 1 ，则通过res.cc返回一个 ‘登录失败！’ 的字符串；然后判断密码是否正确，如果不正确，则通过res.cc返回一个 ‘登录失败！’ 的字符串；然后通过ES6的高级语法，快速剔除 密码 和 头像 的值，再对用户的信息进行加密，生成Token字符串；最后将Token响应给客户端。

##### 获取用户基本信息的路由

**路由：**

```js
router.get('/userinfo', userinfo_handler.getUserInfo)
```

**请求方法：**get

**请求URL：**/userinfo

**具体实现函数：**

```js
// 获取用户基本信息的处理函数
exports.getUserInfo = (req, res) => {
    // 定义查询用户信息的SQL语句
    const sql = 'select id, username, nickname, email, user_pic from ev_users where id = ?'
    db.query(sql, req.user.id, (err, results) => {
        // 执行SQL语句失败
        if (err) {
            return res.cc(err)
        }
        // 执行SQL语句成功，但是查询的结果可能为空
        if (results.length !== 1) {
            return res.cc('获取用户信息失败!')
        }
        // 用户信息获取成功
        res.send({
            status: 0,
            message: '获取用户信息成功!',
            data: results[0]
        })
    })
}
```

##### 更新用户信息的路由

**路由：**

```js
router.post('/userinfo', expressjoi(update_userinfo_schema), userinfo_handler.updateUserInfo)
```

**请求方法：**post

**请求URL：**/userinfo

**验证规则对象 - 更新用户基本信息**

```js
// 定义 id,nickname,email的验证规则
const id = joi.number().integer().min(1).required()
const nickname = joi.string().required()
const user_email = joi.string().email().required()
```

```js
exports.update_userinfo_schema = {
  // 需要对 req.body里面的数据进行验证
  body: {
    id,
    nickname,
    email: user_email
  }
}
```

**具体实现函数：**

```js
// 更新用户基本信息的处理函数
exports.updateUserInfo = (req, res) => {
    // 定义待执行的SQL语句
    const sql = 'update ev_users set ? where id = ?'
    console.log(req.body);

    // 调用db.query()执行SQL语句并传递参数
    db.query(sql, [req.body, req.body.id], (err, results) => {
        // 执行SQL语句失败
        if (err) {
            return res.cc(err)
        }

        // 执行SQL语句成功，但是影响行数不等于1
        if (results.affectedRows !== 1) {
            return res.cc('更新用户的基本信息失败！')
        }
        // 成功
        res.cc('更新用户信息成功！', 0)
    })
}

```

##### 更新密码的路由

**路由：**

```js
router.post('/updatepwd', expressjoi(update_password_schema), userinfo_handler.updatePassword)
```

**请求方法：**post

**请求URL：**/updatepwd

**验证规则对象 - 更新密码**

```js
//joi.ref('oldPwd') 表示 newPwd 的值必须 和 oldPwd 的值保持一致
//joi.not(joi.ref('oldPwd')) 表示 newPwd 的值不能等于 oldPwd 的值
//.concat() 用于合并 joi.not(joi.ref('oldPwd')) 和 password 着两条验证规则
exports.update_password_schema = {
  body: {
    oldPwd: password,
    newPwd: joi.not(joi.ref('oldPwd')).concat(password)
  }
}
```

**具体实现函数：**

```js
// 更新用户密码的处理函数
exports.updatePassword = (req, res) => {
    // 根据 id 查询用户的信息
    const sql = 'select * from ev_users where id = ?'
        // 执行根据 id 插叙用户的信息的 SQL 语句
    db.query(sql, req.user.id, (err, results) => {
        // 执行 SQL 语句失败
        if (err) {
            return res.cc(err)
        }
        // 判断结果是否存在
        if (results.length !== 1) {
            return res.cc('用户不存在！')
        }
        // 判断用户输入的旧密码是否正确
        const compareResult = bcrypt.compareSync(req.body.oldPwd, results[0].password)
        if (!compareResult) {
            return res.cc('旧密码错误！')
        }

        // 定义更新密码的 SQL 语句
        const sql = 'update ev_users set password = ? where id = ?'
            // 对新密码进行加密处理
        const newPwd = bcrypt.hashSync(req.body.newPwd, 10)
            // 调用db.query()执行SQL语句
        db.query(sql, [newPwd, req.user.id], (err, results) => {
            // 执行SQL语句失败
            if (err) {
                return res.cc(err)
            }
            // 判断影响行数
            if (results.affectedRows !== 1) {
                return res.cc('更新密码失败！')
            }
            // 成功
            res.cc('更新密码成功', 0)
        })
    })
}

```

##### 更换头像的路由

**路由：**

```js
router.post('/update/avatar', expressjoi(update_avatar_schema), userinfo_handler.updateAvatar)
```

**请求方法：**post

**请求URL：**/update/avatar

**验证规则对象 - 更新头像**

```js
// 定义验证 avatar 头像的验证规则
//joi.string().dataUri() 要求字符串值为有效的数据 URI 字符串
const avatar = joi.string().dataUri().required()
```

```js
exports.update_avatar_schema = {
  body: {
    avatar
  }
}
```

**具体实现函数：**

```js
// 更新用户头像的处理函数
exports.updateAvatar = (req, res) => {
    // 定义更新头像的SQL语句
    const sql = 'update ev_users set user_pic = ? where id = ?'
        // 调用db.query()执行SQL语句
    db.query(sql, [req.body.avatar, req.user.id], (err, results) => {
        // 执行SQL语句失败
        if (err) {
            return res.cc(err)
        }

        // 执行SQL语句成功，但是影响行数不等于1
        if (results.affectedRows !== 1) {
            return res.cc('更换头像失败！')
        }
        // 成功
        res.cc('更换头像成功！', 0)
    })
}
```

### 前端页面写完后如何将其部署到服务器上去（node.js服务器）

1、执行npm run bulid将项目打包。生成dist文件夹，其中含有html,css,js等文件。如果不打包，浏览器将不认识.vue文件。

2、将打包好的dist文件夹中的文件放到后端的static或public文件夹中。

3、在后端的server.js中写上app.use(express.static(__dirname+'/static'));或

app.use(express.static(__dirname+'/public'));

如果使用history模式（mode: 'history'），在项目部署到服务器上之后，刷新页面会将组件路由上的地址接到请求的服务器地址的后面带给服务器，此时就会出现404的问题，此时需要用一个插件来解决此问题，npm i connect-history-api-fallback。然后通过app.use(history())来使用该插件，此时就可以解决这个问题了。但是如果使用hash模式（mode: 'hash'）,就没有这个问题了。

在项目中这两种模式都有使用。

### Koa的基本使用

#### 1.安装

```bash
npm i koa
npm i @types/koa -D
```

#### 2.基本用法

```typescript
import * as Koa from "koa";
const app = new Koa();

app.use(async (ctx, next) => {
  const start = Date.now();
  await next();
  const time = Date.now() - start;
  ctx.body = time
});

app.listen(3000, () => {
  console.log(`正在监听3000端口...`);
});
```

>用Koa创建一个app实例，然后使用use方法引入中间件，最终监听一个端口。

#### 3.state

它允许开发者在请求处理过程中存储和传递一些公共的数据或状态。这个 state 对象通常用于跨多个 middleware 或者函数来共享数据。当然，你也可以使用它来传递用户身份信息、权限等相关的上下文信息。

```ts
// 添加 ctx.state 数据
app.use(async (ctx, next) => {
  ctx.state.user = { id: 123, name: 'Alice' };
  await next();
});

// 在另一个中间件中访问 ctx.state 数据
app.use(async (ctx) => {
  // 访问 ctx.state 中的 user 数据
  console.log(ctx.state.user); // 输出: { id: 123, name: 'Alice' }
  
  // 其他业务逻辑...
});
```

#### 4.获取query

```typescript
// /api/list?limit=1
app.use(async (ctx, next) => {
  const { limit } = ctx.query
});
```

#### cookie

```typescript
ctx.cookies.set(key, value)
ctx.cookies.get(key)
```

### response

```typescript
app.use(async (ctx, next) => {
  ctx.body = 'xxx'
  ctx.status = 200
});
```

### 常用的KOA中间件

#### koa-router

`koa-router` 是一个用于 Koa 应用程序的路由中间件，它可以帮助你在 Koa 应用中定义和管理路由，从而更好地组织你的应用逻辑并处理不同的 HTTP 请求。

```typescript
import * as Koa from 'koa'
import * as Router from 'koa-router'

const app = new Koa({

})
const router = new Router({
  // prefix: '/xx/xxx'
})

async function getUserInfo(ctx: Koa.Context, next: Koa.Next) {
  // todo: cookie, userinfo
  ctx.state.user = 'test'

  if (!ctx.state.user) {
    // ctx.status = 403;
    ctx.body = { code: -2, message: '没有登录' }
    return
  }
  await next()
}

async function getUserList(ctx: Koa.Context, next: Koa.Next) {
  console.log('query', ctx.query)
  ctx.body = {
    code: 0,
    total: 120,
    list: [
      { name: 'aaa', age: 40 },
      { name: 'bbb', age: 40 },
      { name: 'ccc', age: 40 },
      { name: 'ddd', age: 40 },
    ],
  }
}

async function createUser(ctx: Koa.Context) {
  //1.先接收用户提交的数据 2.处理数据。3.回包

  const newUser = { name: 'sss', age: 10 }
  //todo:write json

  //回包到浏览器
  ctx.body = { code: 0, message: '创建用户成功' }
}

async function checkUser(ctx: Koa.Context) {
  //1.先接收用户提交的数据 2.处理数据。3.回包
  // const { user, pwd } = ctx.request.body
  let isCorrect = true
  //todo:check user and pwd is correct
  if (isCorrect) {
    // 写入 koa-session
    ctx.body = { code: 0, message: '登录成功' }
  } else {
    ctx.body = { code: -1, message: '密码错误' }
  }
}

// /api/user/login
router.post('/api/user/login', checkUser)

//url后面只能有path
router.get('/api/stu/list', getUserInfo, getUserList)

router.post('/api/stu/create', getUserInfo, createUser)

app.use(router.routes()).use(router.allowedMethods())

app.listen(3000)

```

通过`router`的`post`、`get`等接口定义路由，通过use引入。当请求的path和设定的path匹配上时，就会运行相应的逻辑。

> 既然是匹配，就会存在优先级的问题。在koa-router中，先匹配的会先执行。若存在一些复杂的路径时，将具体的router放在前面，通配的router放在后面。

##### 获取params

```typescript
// /api/user/:id
app.use(async (ctx, next) => {
  const { id } = ctx.params
});
```

#### koa-body

`koa-body` 是一个用于 Koa 框架的中间件，它的主要作用是解析 HTTP 请求体，并将解析后的数据挂载到 `ctx.request.body` 中，方便后续处理请求时获取客户端提交的数据。

**安装 koa-body：**

```bash
npm install koa-body
```

**使用示例：**

```javascript
import * as Koa from 'koa'
import bodyParser from 'koa-body'

const app = new Koa();

// 使用 koa-body 中间件来解析请求体
app.use(bodyParser());

// 处理 POST 请求
app.use((ctx) => {
  // 获取客户端提交的数据
  const postData = ctx.request.body;
  
  // 其他业务逻辑...
});

app.listen(3000);
```

在这个示例中，我们通过 `koa-body` 中间件来解析请求体，并且将解析后的数据存在 `ctx.request.body` 中。这样，在接收到 POST 请求时，我们可以直接从 `ctx.request.body` 获取客户端提交的数据。

**作用：**

1. **解析请求体：** 主要作用是解析客户端发来的 HTTP 请求体（比如表单数据、JSON 数据等），方便后续处理。
2. **简化数据处理流程：** 可以让开发者更加方便地获取客户端提交的数据，避免了手动解析请求体的繁琐操作。

总之，`koa-body` 能够简化处理 POST 请求时的数据解析工作，使得开发者能够更专注于实际的业务逻辑而不是对请求体进行解析。

#### koa-static

`koa-static` 是一个用于 Koa 框架的中间件，它可以帮助你提供静态文件服务。当你需要在 Koa 应用程序中提供静态资源（如 HTML、CSS、JavaScript 文件、图片等）时，`koa-static` 中间件可以将这些静态文件关联到 URL 路径，并在客户端请求时返回相应的文件内容。

**安装 koa-static：**

```bash
npm install koa-static
```

**使用示例：**

```javascript
import * as Koa from 'koa'
import * as serve from 'koa-static'
import * as path from 'path'

const app = new Koa();

// 使用 koa-static 中间件来提供静态文件服务
app.use(static(path.join(__dirname, 'public')));

// 其他业务逻辑...
```

在上面的代码中，我们通过 `koa-static` 中间件将 `public` 目录下的静态文件与 URL 路径相关联起来，这意味着如果有一个名为 `style.css` 的文件位于 `public` 目录下，那么在浏览器中访问 `/style.css` 就能得到这个文件的内容。

**作用：**

1. **提供静态文件服务：** `koa-static` 可以帮助你向客户端提供静态资源，避免了每次请求都需要由 Koa 处理这些静态文件。
2. **性能优化：** 通过使用专门的 Web 服务器（例如 Nginx）或者 CDN 来提供静态文件服务，能够减轻 Node.js 服务器的负担，从而改善整体性能。

总之，`koa-static` 中间件简化了处理静态文件的过程，并且在实际开发中被广泛用于提供网页的 CSS、JavaScript 和图片等静态资源。

