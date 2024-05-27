---
title: HTML5+CSS3+JS基础
categories: HTML5+CSS3+JS基础
tags: HTML5+CSS3+JS基础
date: 2023/2/5
cover: https://tse4-mm.cn.bing.net/th/id/OIP-C.9LiYwfQcPg6I1lGK06X9-gHaEe?w=273&h=180&c=7&r=0&o=5&dpr=1.3&pid=1sh
---

### 鼠标样式

| 属性            | cursor             |
| :-------------- | :----------------- |
| 值：default     | 默认的小白鼠标样式 |
| 值：pointer     | 鼠标小手样式       |
| 值：move        | 鼠标移动样式       |
| 值：text        | 鼠标文本样式       |
| 值：not-allowed | 鼠标禁止样式       |

### 取消表单轮廓+防止拖拽文本域

#### 取消表单轮廓

input和textarea输入框，当点击后会出现默认的边框，

```css
outline: none;/*可以取消这个默认的边框*/
```

#### 防止拖拽文本域

textarea输入框默认是可以手动变大变小的，

```css
resize: none;/*可以禁止用户拖拽文本域*/
```

### 去掉图片底部默认的空白缝隙

img标签放入的图片，在图片的底部存在默认的空白缝隙，

```css
vertical-align: top/middle/bottom;/*或*/display: block;/*可以去掉这个默认的空白缝隙*/
```

### flex布局（自适应，常用于移动端布局）

> 注意：flex布局中，默认的子元素是不换行的，如果装不开，会缩小子元素的宽度，放到父元素里面

#### 布局原理

> flex是flexible Box的缩写，意为“弹性布局”，用来为盒装模型提供最大的灵活性，任何一个容器都可以指定为flex布局。
>
> 当我们为父盒子设为flex布局以后，子元素的float、clear和vertical-align属性将失效。
>
> 伸缩布局 = 弹性布局 = 伸缩盒布局 = 弹性盒布局 = flex布局
>
> 总结：flex布局就是通过给父盒子添加flex属性（在父级盒子中样式：display: flex; 此时默认为flex-direction: row，默认的主轴为x轴---从左到右，row-reverse---从右到左。同理：column为y轴---从上到下，column-reverse---从下到上），来控制子盒子的位置和排列方式
>
> 注意：元素是跟着主轴来排列的。

在flex布局中分为主轴和侧轴两个方向：

​	默认主轴方向就是x轴方向，水平向右

​	默认侧轴方向就是y轴方向，水平向下

#### 常见父项属性：

| flex-direction  | 设置主轴的方向                                      |
| --------------- | --------------------------------------------------- |
| justify-content | 设置主轴上的子元素排列方式                          |
| flex-wrap       | 设置子元素是否换行                                  |
| align-content   | 设置侧轴上的子元素的排列方式（多行）                |
| align-items     | 设置侧轴上的子元素排列方式（单行）                  |
| flex-flow       | 复合属性，相当于同时设置了flex-direction和flex-wrap |

**justify-content (设置主轴上的子元素排列方式)**

| 属性值        | 说明                                       |
| ------------- | ------------------------------------------ |
| flex-start    | 默认值从头部开始 如果主轴是x轴，则从左到右 |
| flex-end      | 从尾部开始排列                             |
| center        | 在主轴居中对齐（如果主轴是x轴则水平居中）  |
| space-around  | 平分剩余空间                               |
| space-between | 先两边贴边 再评分剩余空间（重要）          |

**flex-wrap (设置子元素是否换行)**

| 属性值 | 说明           |
| ------ | -------------- |
| nowrap | 默认值，不换行 |
| wrap   | 换行           |

**align-items (设置侧轴上的子元素排列方式（单行）)**

| 属性值     | 说明                                        |
| ---------- | ------------------------------------------- |
| flex-start | 从上到下                                    |
| flex-end   | 从下到上                                    |
| center     | 挤在一起居中（垂直居中）                    |
| stretch    | 拉伸（默认值）注意拉伸方向上不能有高度/宽度 |

**align-content (设置侧轴上的子元素的排列方式（多行,在单行下没有效果）)**

| 属性值        | 说明                                   |
| ------------- | -------------------------------------- |
| flex-start    | 默认值在侧轴的头部开始排列             |
| flex-end      | 在侧轴的尾部开始排列                   |
| center        | 在侧轴中间显示                         |
| space-around  | 子项在侧轴平分剩余空间                 |
| space-between | 子项在侧轴先分布在两头，再平分剩余空间 |
| stretch       | 设置子项元素高度平分父元素高度         |

**flex-flow (复合属性，相当于同时设置了flex-direction和flex-wrap)**

例如：flex-direction: row;    flex-wrap: wrap;

flex-flow: row wrap;

#### 常见子项属性：

| flex       | 子项占的份数                       |
| ---------- | ---------------------------------- |
| align-self | 控制子项自己在侧轴的排列方式       |
| order      | 属性定义子项的排列顺序（前后顺序） |

**flex(子项占的份数)**

flex: <number>; /*default  0*/

**align-self(控制子项自己在侧轴的排列方式)**

> align-self属性允许单个项目有与其他项目不一样的对齐方式，可覆盖align-items属性。默认值为auto，表示继承父元素的align-items属性，如果没有父元素，则等同于stretch。

**order（属性定义子项的排列顺序（前后顺序））**

> 数组越小，排列越靠前，默认为0。
>
> 注意：和z-index不一样。

### （图片等行内块元素）后的文字和图片的位置

| 属性       | vertical-align |
| ---------- | -------------- |
| 值：top    | 顶部           |
| 值：middle | 中间           |
| 值：bottom | 底部           |

### base.css

将一些标签中自带的、我们不想要的样式都去掉。

```css
/* 把我们所有标签的内外边距都清零（因为有一些网页元素自带内外边距） */

\* {

  margin: 0;

  padding: 0;

  /* css3盒子模型 不需要考虑内外边距的影响*/

  box-sizing: border-box;

}

/* em和i斜体的文字不倾斜 */

em,

i {

  font-style: normal;

}

/* 去掉li前面的小圆点 */

li {

  list-style: none;

}

img {

  /* border 0 照顾低版本浏览器 如果 图片外面包含了链接会有边框的问题 */

  border: 0;

  /* 取消图片底侧有空白缝隙的问题 */

  vertical-align: middle;

}

button {

  /* 当我们鼠标经过button按钮的时候鼠标变成小手 */

  cursor: pointer;

}

a {

  color: #666;

  text-decoration: none;

}

a:hover {

  color: #c81623;

}

button,

input {

  font-family: Microsoft YaHei, Heiti SC, tahoma, arial, Hiragino Sans GB, "\5B8B\4F53", sans-serif;

  /* 默认有灰色边框我们需要手动去掉 */

  border: 0;

  /* 去掉点击完出现的蓝色边框 */

  outline: none;

}

body {

  /* 抗锯齿性让文字显示的更见清晰 */

  -webkit-font-smoothing: antialiased;

  background-color: #ffffff;

  font: 12px/1.5 Microsoft YaHei, Heiti SC, tahoma, arial, Hiragino Sans GB, "\5B8B\4F53", sans-serif;

  color: #666;

}

.hide,

.none {

  display: none;

}

/* 清除浮动 */

.clearfix:after {

  visibility: hidden;

  clear: both;

  display: block;

  content: ".";

  height: 0

}

.clearfix {

  *zoom: 1;

}
```

### 文字显示不开也强制一行内显示

```css
white-space: nowrap;/* 文字显示不开也必须强制一行内显示 */
```

### 将溢出的部分隐藏起来

```css
overflow: hidden;/* 溢出的部分隐藏起来 */

overflow:auto;/*显示不完时，自动出现滚动条*/
```

### 文字溢出的时候用省略号来显示

```css
text-overflow: ellipsis;/* 文字溢出的时候用省略号来显示 */
```

### 弹性伸缩盒子模型显示

```css
display: -webkit-box;/* 弹性伸缩盒子模型显示 */
```

### 限制在一个块元素显示的文本的行数

```css
-webkit-line-clamp: 2;/* 限制在一个块元素显示的文本的行数 */
```

### 设置或检索伸缩盒子对象的子元素的排列方式

```css
-webkit-box-orient: vertical;/* 设置或检索伸缩盒子对象的子元素的排列方式（此例子为垂直居中） */
```

### 将某个组件固定在页面中的某个位置

| 属性      | position                                                     |
| --------- | ------------------------------------------------------------ |
| 值：fixed | 生成绝对定位的元素，相对于浏览器窗口进行定位。元素的位置通过"left", "top","right","bottom"属性进行规定。 |

### 锚点链接

```html
/*锚点链接（点击后可以跳转到相应内容的位置）*/
<a href="#table">
    <h1>表格标签</h1>
</a>

/*相应内容*/
<h1 id="table">表格标签</h1>
```

### 常见键盘事件

| 事件函数 | 功能                                     |
| -------- | ---------------------------------------- |
| keyup    | 当按下键盘并弹起键盘后触发该函数         |
| keydown  | 当按下键盘就触发该函数（不能区分大小写） |
| keypress | 当按下键盘就触发该函数（可以区分大小写） |

### 浏览器local存储(关闭浏览器后数据仍然存在)

| 函数API                           | 功能                       |
| --------------------------------- | -------------------------- |
| localStorage.setItem('key',value) | 设置一组键值对             |
| localStorage.getItem('key',value) | 通过key获取value值         |
| localStorage.removeItem('key')    | 通过key删除键值对          |
| localStorage.clear();             | 清空浏览器中存储的所有数据 |

### 浏览器session存储(关闭浏览器后数据就销毁了)

| 函数API                             | 功能                       |
| ----------------------------------- | -------------------------- |
| sessionStorage.setItem('key',value) | 设置一组键值对             |
| sessionStorage.getItem('key',value) | 通过key获取value值         |
| sessionStorage.removeItem('key')    | 通过key删除键值对          |
| sessionStorage.clear();             | 清空浏览器中存储的所有数据 |

### JS全局作用域的特殊情况

```js
/*如果在函数内部，没有声明(且在任何地方都没有声明)就直接赋值的变量属于——全局变量*/
var num = 10;//全局变量
function fn() {
    var num1 = 20;//局部变量
    num2 = 30;//全局变量
}

/*全局变量和局部变量的区别*/
/*
（1）全局变量只有浏览器关闭的时候才会销毁，比较占内存资源
（2）局部变量当我们程序执行完毕就会销毁，比较节约内存资源
*/

/*注意：es6之前没有块级作用域*/
/*例：*/if(true) {
         	var num3 = 40;//该变量在if大括号外也可以使用 
         }

/*作用域链*/
/*内部函数访问外部函数的变量，采取的是链式查找的方式来决定取哪个值（就近原则）*/
```

### var，let，const三者的特点和区别

#### 一、var的特点

**1、存在变量提升**

```js
console.log(a); // undefined
var a = 10;

// 编译过程
var a;
console.log(a); // undefined
a = 10;
```

**2、一个变量可多次声明，后面的声明会覆盖前面的声明**

```js
var a = 10;
var a = 20;
console.log(a); // 20
```

**3、在函数中使用var声明变量的时候，该变量是局部的**

```js
var a = 10;
function change(){
    var a = 20;
}
change();
console.log(a); // 10
```

**而如果在函数内不使用var，该变量是全局的**

```js
var a = 10;
function change(){
    a = 20
};
change();
console.log(a); // 20
```

#### 二、let的特点

**1、不存在变量提升，let声明变量前，该变量不能使用（暂时性死区）**

```js
console.log(a); // ReferenceError: a is not defined
let a = 10;
```

**2、let命令所在的代码块内有效，在块级[作用域](https://so.csdn.net/so/search?q=作用域&spm=1001.2101.3001.7020)内有效**

```js
{
	let a = 10;
}
console.log(a);  // ReferenceError: a is not defined
```

**3、let不允许在相同作用域中重复声明，注意是相同作用域，不同作用域有重复声明不会报错**

```js
let a = 10;
let a = 20;
// Uncaught SyntaxError: Identifier 'a' has already been declared

let a = 10;
{
	let a = 20;
}
// ok
```

#### 三、const

**1、const声明一个只读的变量，声明后，值就不能改变**

```js
const a = 10;
a = 20;  // TypeError: Assignment to constant variable.
```

**2、const必须初始化**

```js
const obj = {
	age: 17
}
obj.age = 18;  // ok

obj = {
	age: 18
}
//  SyntaxError: Identifier 'obj' has already been declared
```

**4、let该有的特点const都有**

**四、区别**
**变量提升**
>var声明的变量存在变量提升，即变量可以在声明之前调用，值为undefined
>let和const不存在变量提升，即它们所声明的变量一定要在声明后使用，否则报错

**块级作用域**
>var不存在块级作用域
>let和const存在块级作用域

**重复声明**
>var允许重复声明变量
>let和const在同一作用域不允许重复声明变量

**修改声明的变量**
>var和let可以
>const声明一个只读的常量。一旦声明，常量的值就不能改变，但对于对象和数据这种引用类型，内存地址不能修改，可以修改里面的值。

**五、使用**

>能用const的情况下尽量使用const，大多数情况使用let，避免使用var。
>const > let > var
>const声明的好处，一让阅读代码的人知道该变量不可修改，二是防止在修改代码的过程中无意中修改了该变量导致报错，减少bug的产生。let声明没有产生预编译和变量提升的问题，先声明再使用可以让代码本身更加规范，let是个块级作用域，也不会污染到全局的变量声明。
>**使用的场景说明：**let一般应用于基本数据类型；const 一般应用于引用数据类型，也就是函数对象等。

### JS预解析

1.JS引擎运行JS分为两步： 1—预解析  2—代码执行

（1）预解析   js引擎会把js里面所有的var还有function提升到当前作用域的最前面

（2）代码执行  按照代码书写的顺序从上往下执行

2.预解析分为   变量预解析（变量提升）和  函数预解析（函数提升）

（1）变量提升就是把所有的变量声明提升到当前的作用域最前面    不提升赋值操作

（2）函数提升就是把所有的函数声明提升到当前作用域的最前面    不调用函数

```js
console.log(num);
var num = 10;
//结果：undefined
/*变量提升 相当于执行如下代码*/
var num;
console.log(num)
num = 10;
```

```js
fn();
function fn() {
    console.log(11);
}
//结果：11
/*函数提升 相当于执行如下代码*/
function fn() {
    console.log(11);
}
fn();
```

```js
fun();
var fun = function() {
    console.log(22);
}
//结果：报错
/*变量提升 相当于执行如下代码*/
var fun;
fun();//在此处，执行fun函数，但是此时fun函数还没有定义
fun = function() {
    console.log(22);
}
```

### 创建对象的三种方式

```js
/*1*/
var obj = {
    uname: '张三',
    age: 18,
    sayHi: function() {
        console.log('Hi')
    }
}
console.log(obj.age)
console.log(obj['age'])
obj.sayHi()
/*2*/
var obj = new Object();//创建一个空的对象
obj.uname = '张三';//追加属性
obj.sayHi = function() {
    console.log('Hi');
}
/*3*/
//利用构造函数创建对象（构造函数不需要return 就可以返回结果）
function Star(uname, age, sex) {
    this.uname = uname;
    this.age = age;
    this.sex = sex;
    this.sayHi = function(content) {
        console.log(content);
    }
}
var zs = new Star('张三', 18, '男')//创建了一个zs对象
```

### 对象的遍历

```js
var obj = {
    uname: '张三',
    age: 18
}
for (var k in obj) {
    console.log(k)//得到的是对象的key
    console.log(obj[k])//得到的是value
}
```

### 判断是否为数组

```js
var arr = []
console.log(arr instanceof Array)//true
console.log(Array.isArray(arr))//true
```

### 重要补充：

```js
var str = "123456";
str.indexOf(''); //输出 0
str.indexOf('1'); //输出 0
```

### 数组相关操作

push()返回值为新数组的长度

var arr = [1,2,3];

arr.push(4,'a')//arr = [1,2,3,4,a]

arr.unshift(4,'a')//arr = [4,a,1,2,3]

arr.pop()返回值为删除掉的那个元素

arr.shift()删除第一个元素，返回值为删除掉的那个元素

```js
/*翻转数组*/
arr.reverse();
/*冒泡排序*/
arr.sort()//会一位一位的去比较，如[13,4,77,1,7]=>[1,13,4,7,77]
/*解决方法*/
arr.sort(function(a,b) {
    return a - b;//升序
    //return b - a;//降序
})

/*获取元素索引号*/
var arr = [1,2,3,2];
arr.indexOf(2)   ——— 1（只返回第一个2的索引号，如果没有2则返回-1）
arr.lastIndexOf(2) ——— 3（从后往前查找，如果没有2则返回-1）
arr.indexOf(2,2) ———3（第二个2为开始查找的起始位置，即从3开始查找）


/*从第三个位置开始删除数组后的两个元素：*/
var fruits = ["Banana", "Orange", "Apple", "Mango"];
fruits.splice(2,1,"Lemon","Kiwi");
//输出结果：Banana,Orange,Lemon,Kiwi,Mango

/*从第三个位置开始删除数组后的两个元素：*/
var fruits = ["Banana", "Orange", "Apple", "Mango"];
fruits.splice(2,2);
//输出结果：Banana,Orange
```

### 数组去重

```js
function unique(arr) {
    var newArr = [];
    for (var i = 0; i < arr.length; i++) {
        if (newArr.indexOf(arr[i]) === -1) {
            newArr.push(arr[i]);
        }
    }
    return newArr;
}
```

### 数组与字符串间的相互转化

```js
var arr = [1, 2, 3];
arr.toString();//输出结果：1,2,3
arr.join('-');//输出结果：1-2-3

var str2 = '1,2,3,4';
var arr2 = str2.split(',');//arr2 = ['1', '2', '3', '4'];
```

### 基本包装类型

将简单的数据类型包装为复杂的数据类型

```js
var str = 'andy';
//js遇到如上代码时会将简单数据类型包装为复杂数据类型，简单数据类型没有属性和方法，js处理方法如下

//1、将简单的数据类型包装为复杂的数据类型
var temp = new String('andy');
//2、把临时变量的值给str
str = temp;
//3、销毁这个临时变量
temp = null;

注意：js中会被包装的三个特殊的引用类型: String、Number、Boolean
```

### 字符串的相关操作

1、如数组一样也有indexOf()、lastIndexOf()方法

2、根据位置返回字符

var str  =  '123a';

str.charAt(1); —— 2

str.charCodeAt(3);—— 97(a的ASCII码值)

str[3];—— a

### 字符串的拼接和截取

```js
var str1 = '123456123'
str1.substr(2,3)//从索引号2开始截取3个字符—— 345
str1.replace('1', 'a');——'a23456123'
/*将一个字符串中的某个字符替换为另一个字符*/
var str2 = '123123123';
while(str2.indexOf('1') !== -1) {
    str2 = str2.replace('1', 'a');
}
//结果：a23a23a23
```

### 字符串大小写转换

```js
var str = 'Hello';
str.toUpperCase();//HELLO

var str1 = 'HEllo';
str1.to LowerCase();//hello
```

### JS中常见的6种数据类型

| 数据类型  | 说明                                                         |
| --------- | ------------------------------------------------------------ |
| Number    | 数值，JavaScript 数值类型不再细分整型、浮点型等，`js 的所有数值都属于浮点型，64位浮点数。` |
| String    | 字符串，最抽象的数据类型，信息传播的载体，`字符串必须包含在单引号、双引号或反引号之中，一个字符两个字节。` |
| Boolean   | 布尔值，最机械的数据类型，逻辑运算的载体，仅有两个值，true / false。 |
| null      | 空值，表示不存在，当为对象的属性赋值为 null，表示删除该属性，使用 typeof 运算符检测 null 值，返回 Object。 |
| undefined | 未定义，当声明变量而没有赋值时会显示该值，可以为变量赋值为 undefined。 |
| Object    | 对象，是一种无序的数据集合，内容是键值对的形式，键名（key）是字符串，可以包含任意字符（空格），字符串引号可省略。可以通过 Object.keys(obj) 打印出 obj 对象中的所有 key 值。读对象的属性时，如果使用 [ ] 语法，那么 JS 会先求 [ ] 中表达式的值。如果使用点语法，那么点后面一定是 string 常量。 |

### JS数组（Array）的相关操作

#### 一、改变原数组的方法

##### **1.push（） 末尾添加数据**

**语法:** `数组名.push(数据)`

**作用:** 就是往数组末尾添加数据

**返回值:**  就是这个数组的长度

```//push
//push复制代码var arr = [10, 20, 30, 40]
res = arr.push(20)
console.log(arr);//[10,20,30,40,20]
console.log(res);//5
```

##### 2. pop（） 末尾出删除数据

**语法:** `数组名.pop()`

**作用:** 就是从数组的末尾删除一个数据

**返回值:** 就是你删除的那个数据

```//pop
//pop复制代码var arr = [10, 20, 30, 40] 
res =arr.pop()
console.log(arr);//[10,20,30]
console.log(res);//40
```

##### 3.unshift（） 头部添加数据

**语法:** `数组名.unshift(数据)`

**作用:**  就是在数组的头部添加数据

**返回值:** 就是数组的长度

```//pop
//pop复制代码 var arr = [10, 20, 30, 40]
 res=arr.unshift(99)
 console.log(arr);//[99,10,20,30,40]
 console.log(res);//5
```

##### 4.shift（） 头部删除数据

**语法:** `数组名.shift()`

**作用:**  头部删除一个数据

**返回值:**  就是删除掉的那个数据

```//shift
//shift复制代码 var arr = [10, 20, 30, 40]
 res=arr.shift()
 console.log(arr);[20,30,40]
 console.log(res);10
```

##### 5.reverse（） 翻转数组

**语法:** `数组名.reverse()`

**作用:** 就是用来翻转数组的

**返回值:** 就是翻转好的数组

```//reverse
//reverse复制代码var arr = [10, 20, 30, 40]
res=arr.reverse()
console.log(arr);//[40,30,20,10]
console.log(res);//[40,30,20,10]
```

##### 6.sort（） 排序

语法一: `数组名.sort()`                       会排序 会按照位排序

语法二: 数组名.sort(function (a,b) {return a-b})  会正序排列

语法三: 数组名.sort(function (a,b) {return b-a})  会倒序排列

```//sort()
//sort()复制代码var arr = [2, 63, 48, 5, 4, 75, 69, 11, 23]
arr.sort()
console.log(arr);
arr.sort(function(a,b){return(a-b)})
console.log(arr);
arr.sort(function(a,b){return(b-a)})
console.log(arr);
```

##### 7.splice（）  截取数组

语法一: `数组名.splice(开始索引,多少个)`

作用: 就是用来截取数组的

返回值: 是一个新数组 里面就是你截取出来的数据

语法二: 数组名.splice(开始索引,多少个,你要插入的数据)

作用: 删除并插入数据

注意: 从你的开始索引起

返回值: 是一个新数组 里面就是你截取出来的数据

```js
//splice()复制代码var arr = [2, 63, 48, 5, 4, 75]
res = arr.splice(1,2)
console.log(arr);
console.log(res);
//******************************
//splice() 语法二
var arr = [2, 63, 48, 5, 4, 75]
res = arr.splice(1,1,99999,88888)
console.log(arr);
console.log(res);
```

#### 二、不改变原数组的方法

##### 1.concat（）合并数组

**语法:** `数组名.concat(数据)`

**作用:**  合并数组的

**返回值:**  一个新的数组

```js
//concat复制代码var arr = [10, 20, 10, 30, 40, 50, 60]
res = arr.concat(20,"小敏",50)
console.log(arr) 
console.log(res);
```

##### 2.join（） 数组转字符串

**语法:** `数组名.join('连接符')`

**作用:** 就是把一个数组转成字符串

**返回值:**  就是转好的一个字符串

```js
//join复制代码var arr = [10, 20, 10, 30, 40, 50, 60]
res = arr.join("+")
console.log(arr)
console.log(res);
```

##### 3.slice（）截取数组的一部分数据

**语法:** `数组名.slice(开始索引,结束索引)`

**作用:** 就是截取数组中的一部分数据

**返回值:** 就是截取出来的数据 放到一个新的数组中

**注意:** 包前不好后 包含开始索引不包含结束索引

```js
//slice复制代码var arr = [10, 20, 10, 30, 40, 50, 60]
res = arr.slice(1,4)
console.log(arr)
console.log(res);
```

##### 4.indexOf 从左检查数组中有没有这个数值

**语法一:** `数组名.indexOf(要查询的数据)`

**作用:** 就是检查这个数组中有没有该数据

如果有就返回该数据**第一次**出现的索引

如果没有返回 -1

**语法二:** **数组名.indexOf(** **要查询的数据,** **开始索引)**

```js
//indexOf复制代码var arr = [10, 20, 10, 30, 40, 50, 60]
res = arr.indexOf(10)
console.log(arr)
console.log(res);
//*************************************
//indexOf  语法二
var arr = [10, 20, 10, 30, 40, 50, 60]
res = arr.indexOf(10,1)
console.log(arr)
console.log(res);
```

##### 5.lastIndexOf 从右检查数组中有没有这个数值

**语法一:** `数组名.indexOf(要查询的数据)`

**作用:** 就是检查这个数组中有没有该数据

如果有就返回该数据**第一次**出现的索引

如果没有返回 -1

**语法二:** **数组名.lastIndexOf(** **要查询的数据,** **开始索引)**

```js
//lastIndexOf复制代码var arr = [10, 20, 10, 30, 40, 50, 60]
res = arr.lastIndexOf(50)
console.log(arr) 
console.log(res);
//*************************************
//lastIndexOf 语法二
var arr = [10, 20, 10, 30, 40, 50, 60]
res = arr.lastIndexOf(50,4)
console.log(arr)
console.log(res);
```

#### 三、ES6新增的数组方法

##### 1. forEach()  用来循环遍历的 for

语法: `数组名.forEach(function (item,index,arr) {})`

- item : 这个表示的是数组中的每一项
- index : 这个表示的是每一项对应的索引
- arr : 这个表示的是原数组

作用: 就是用来循环遍历数组的 代替了我们的for

```js
//forEach复制代码var arr = [1, 2, 3, 4, 5]
console.log('原始数组 : ', arr);
var res = arr.forEach(function (item, index, arr) {
    console.log(item, "------", index, "-------", arr);
})
```

##### 2.map  映射数组的

语法: `数组名.map(function (item,index,arr) {})`

- item : 这个表示的是数组中的每一项
- index : 这个表示的是每一项对应的索引
- arr : 这个表示的是原数组

作用: 就是用来映射

返回值: 必然是一个数组 一个映射完毕的数组；这个数组合原数组长度一样

```js
//map复制代码var arr = [1, 2, 3, 4, 5]
console.log('原始数组 : ', arr);
var res = arr.map(function (item) {
    return item*1000
})
console.log(res);
```

##### 3.filter  过滤数组

语法: `数组名.filter(function (item,index,arr) {})`

- item : 这个表示的是数组中的每一项
- index : 这个表示的是每一项对应的索引
- arr : 这个表示的是原数组

作用: 用来过滤数组的

返回值: 如果有就是过滤(筛选)出来的数据 保存在一个数组中；如果没有返回一个空数组

```js
//filter复制代码var arr = [1, 2, 3, 4, 5]
console.log('原始数组 : ', arr);
var res = arr.filter(function (item) {
    return item > 2
})
console.log(res);
```

##### 4.every  判断数组是不是满足所有条件

语法: `数组名.every(function (item,index,arr) {})`

- item : 这个表示的是数组中的每一项
- index : 这个表示的是每一项对应的索引
- arr : 这个表示的是原数组

作用: 主要是用来判断数组中是不是 每一个 都满足条件

```text
 只有所有的都满足条件返回的是true

 只要有一个不满足返回的就是false
```

返回值: 是一个布尔值 注意: 要以return的形式执行返回条件

```js
//every复制代码var arr = [1, 2, 3, 4, 5]
console.log('原始数组 : ', arr);
var res = arr.every(function (item) {
    return item > 0
})
console.log(res);//打印结果  true
```

##### 5.some（） 数组中有没有满足条件的

语法: `数组名.some(function (item,index,arr) {})`

- item : 这个表示的是数组中的每一项
- index : 这个表示的是每一项对应的索引
- arr : 这个表示的是原数组

作用: 主要是用来判断数组中是不是 每一个 都满足条件

```text
 只有有一个满足条件返回的是true

 只要都不满足返回的就是false
```

返回值: 是一个布尔值

注意: 要以return的形式执行返回条件

```js
//some复制代码var arr = [1, 2, 3, 4, 5]
console.log('原始数组 : ', arr);
var res = arr.some(function (item) {
    return item > 3
})
console.log(res);//true
```

##### 6.find（）用来获取数组中满足条件的第一个数据

语法: `数组名.find(function (item,index,arr) {})`

- item : 这个表示的是数组中的每一项
- index : 这个表示的是每一项对应的索引
- arr : 这个表示的是原数组

作用: 用来获取数组中满足条件的数据

返回值: 如果有 就是满足条件的第一个数据；如果没有就是undefined

注意: 要以return的形式执行返回条件

```js
//find复制代码var arr = [1, 2, 3, 4, 5]
console.log('原始数组 : ', arr);
var res = arr.find(function (item) {
    return item > 3
})
console.log(res)//4
```

##### 7.reduce（）叠加后的效果

语法:` 数组名.reduce(function (prev,cur,index,arr) {},初始值)`

- prev :一开始就是初始值 当第一次有了结果以后；这个值就是第一次的结果
- cur: 当前正在处理的数据元素
- index : 这个表示的是每一项对应的索引
- arr : 这个表示的是原数组

作用: 就是用来叠加的

返回值: 就是叠加后的结果

注意: 以return的形式书写返回条件

```js
//reduce复制代码var arr = [1, 2, 3, 4, 5]
var res = arr.reduce(function (prev, cur) {
    return prev *= cur
}, 1)//如果这里不写任何数据，那么reduce在遍历该数组的时候，第一次循环时prev的值为1(arr[0])，cur的值为2(arr[1])
	//此处写1之后，prev的值就为1(此处写的值1)，cur的值为1(arr[0])
console.log(res);//120
```

### JS中的常用容器的使用

**map**:[JavaScript Map (w3school.com.cn)](https://www.w3school.com.cn/js/js_object_maps.asp)

**set**:[JavaScript Set (w3school.com.cn)](https://www.w3school.com.cn/js/js_object_sets.asp)

### 特殊元素body和html的获取

```js
/*获取body*/
var bd = document.body;
/*获取html*/
var htmlElement = document.documentElement;
```

### 内置属性值及自定义属性值的操作

```js
/*获取*/
element.属性（可以得到属性值）
element.getAttribute('属性');(自定义属性值的获取方法)

/*设置属性值*/
element.属性 = '值' //设置内置属性
element.setAttribute('属性', '值'); //主要设置自定义属性

/*移除某个元素的某个属性*/
元素.removeAttribute('属性')

注意：H5规定自定义属性要以data-开头
H5新增的获取自定义属性的方法：
element.dataset.属性  或者  element.dataset['属性']  ie11才开始支持
```

### 节点的复制

```js
结点.cloneNode()//参数默认为false，浅拷贝，只克隆复制节点本身，不克隆里面的子节点
结点.cloneNode(true);//深拷贝，将标签本身及里面的子节点全部克隆复制
```

### 事件解绑

```js
//1、传统方式
div.onclick = null;
//2、removeEventListener
div.addEventListener('click', fn);
function fn() {
    ...
}
div.removeEventListener('click', fn);
```

### DOM事件流

1、捕获阶段

2、当前目标阶段

3、冒泡阶段

注意：捕获阶段和冒泡阶段只能发生其中一个

addEventListener('click', function(){}, **false**);

如果上式第三个参数  **省略或者是false**  则发生**冒泡**  即由内到外

如果上式第三个参数  **true**  则发生**捕获**  即由外到内

### e.target和this的区别

e.target   返回的是触发事件的对象（元素）

this   返回的是绑定事件的对象（元素）

### 阻止事件冒泡和默认行为

在事件function(e)中添加e.stopPropagation();此事件后面的事件将不再执行

低版本浏览器：e.cancelBubble=true;



```js
/*阻止默认行为（事件）例如:让链接不跳转 或者让提交按钮不提交*/
var a = document.querySelector('a');
a.addEventListener('click', function(e) {
	e.preventDefault();//dom 标准写法
})
//传统的注册方式
a.onclick = function(e) {
	e.preventDefault();//普通浏览器
	e.returnValue; //低版本浏览器
	return false;//没有兼容性问题 特点：return后面的代码不执行了，而且只限于传统的注册方式
}
```

### 事件委托

```html
<ul>
	<li>、、、</li>
	<li>、、、</li>
	<li>、、、</li>
</ul>
```

给ul添加一个点击事件，那么当点击每一个li时，因为li并未绑定事件向上冒泡发现ul有点击事件，于是就触发事件。

如果在点击事件中添加e.target.style.backgroundColor='pink'；则被点击的li会变为pink色。

### 禁止选中文字和禁止右键菜单

```js
document.addEventListener('selectstart',function(e){
	e.preventDefault();
})
```

```js
document.addEventListener('contextmenu',function(e){
	e.preventDefault();
})
```

### 鼠标事件

| 事件        | 功能                                    |
| ----------- | --------------------------------------- |
| e.clientX/Y | 返回鼠标相对于浏览器窗口可视化的X/Y坐标 |
| e.pageX/Y   | 返回鼠标相对于文档页面的X/Y坐标         |
| e.screenX/Y | 返回鼠标相对于电脑屏幕的X/Y坐标         |

### 键盘事件

| 事件       | 功能                                                    |
| ---------- | ------------------------------------------------------- |
| onkeyup    | 某个键盘按键被松开时触发                                |
| onkeydown  | 按下时（不区分字母大小写）                              |
| onkeypress | 按下时（不识别功能键eg:ctrl,shift等）（区分字母大小写） |
| e.key      | （用户按下的是那个键）但很多浏览器不兼容                |
| e.keyCode  | 按键的ASCII码                                           |

### 页面加载完毕后再执行函数

| window.onload=function(){}                               | 页面加载完毕后再执行函数（会导致页面重绘） |
| -------------------------------------------------------- | ------------------------------------------ |
| window.addEventListener('load',function(){})             | 页面全部加载完毕后                         |
| window.addEventListener('DOMContentLoaded',function(){}) | DOM加载完毕即可                            |
| 节点1.nextElementSibling;                                | 节点1的下一个同级节点                      |
| 节点1.previousElementSibling;                            | 节点1的上一个同级节点                      |

### 调整窗口大小事件

| window.addEventListener('resize',function(){}) | 窗口变化时调用function |
| ---------------------------------------------- | ---------------------- |
| window.innerWidth                              | 当前屏幕的宽度         |

### 定时器

| setTimeout()  | var timer=window.setTimeout(调用函数，[延迟毫秒数]); |
| ------------- | ---------------------------------------------------- |
| 停止定时器    | window.clearTimeout(timer)                           |
| setInterval() | setInterval(调用函数，[延迟毫秒数]);循环调用函数     |

### this的指向问题

1、全局作用域或者普通函数中this指向全局对象window (注意定时器里面的this指向window)

2、方法调用中谁调用this指向谁

3、构造函数中this指向构造函数的实例

### JS执行机制

1、先执行主线程执行栈中的同步任务

2、异步任务（回调函数）放入任务队列中

3、一旦执行栈中的所有同步任务执行完毕，系统就会按次序读取任务队列中异步任务，于是被读取的异步任务结束等待状态，进入执行栈，开始执行

注意：对于定时器中的回调函数来讲，当定时器的时间到了之后，才把回调函数放入任务队列中

### http协议各部分解读

protocol://host[:port]/path/[?query]#fragment

http://www.itcast.cn/index.html?name=andy&age=18#link

| 组成     | 说明                                                         |
| -------- | ------------------------------------------------------------ |
| protocol | 通信协议 常用的http,ftp,maito等                              |
| host     | 主机（域名）www.baidu.com                                    |
| port     | 端口号 可选，省略时使用方案的默认端口 如http的默认端口为80   |
| path     | 路径由零或多个'/'符号隔开的字符串，一般用来表示主机上的一个目录或文件地址 |
| query    | 参数 以键值对的形式，通过&符号分隔开                         |
| fragment | 片段 #后面内容 常见于链接 锚点                               |

### 	location对象属性

| location对象属性  | 返回值                            |
| ----------------- | --------------------------------- |
| location.href     | 获取或者设置整个URL               |
| location.host     | 返回主机（域名）www.baidu.com     |
| location.port     | 返回端口号 如果未写返回空字符串   |
| location.pathname | 返回路径                          |
| location.search   | 返回参数                          |
| location.hash     | 返回片段 #后面内容常见于链接 锚点 |

### 用不同的设备打开显示不同的页面：

```js
if((navigator.userAgent.match(/(phone|pad|pod|iPhone|iPod|ios|iPad|Android|Mobile|BlackBerry|IEMobile|MQQBrowser|JUC|Fennec|wOSBrowser|BrowserNG|WebOS|Symbian|Windows Phone)/i))) {
	window.location.href=""; 		//手机
}	else {
		window.location.href="";	//电脑
}
```

### 前进和后退

| history.forward(); | 前进                                 |
| ------------------ | ------------------------------------ |
| history.go(1)      | 前进一步（括号中的数字为前进的步数） |
| history.back();    | 后退                                 |
| history.go(-1)     | 后退一步（括号中的数字为后退的步数） |

### offset系列常用属性

| offset系列属性       | 作用                                                         |
| -------------------- | ------------------------------------------------------------ |
| element.offsetParent | 返回作用该元素带有定位的父级元素如果父级都没有定位则返回body |
| element.offsetTop    | 返回元素相对带有定位父元素上方的偏移                         |
| element.offsetLeft   | 返回元素相对带有定位父元素左边框的偏移                       |
| element.offsetWidth  | 返回自身包括padding、边框、内容区的宽度，返回数值不带单位    |
| element.offsetHeight | 返回自身包括padding、边框、内容区的高度，返回数值不带单位    |

### offset与style的区别

| offset                                        | style                                       |
| --------------------------------------------- | ------------------------------------------- |
| offset可以得到任意样式表中的样式值            | style只能得到行内样式表中的样式值           |
| offset系列获得的数值是没有单位的              | style.width获得的是带有单位i的字符串        |
| offsetWidth包含padding+border+width           | style.width获得不包含padding和border的值    |
| offsetWidth等属性是只读属性，只能获取不能赋值 | style.width是可读写属性，可以获取也可以赋值 |
| 所以我们想要获取元素大小位置，用offset更合适  | 所以我们想要给元素更改值，则需要用style改变 |

### client系列属性

| client系列属性       | 作用                                                        |
| -------------------- | ----------------------------------------------------------- |
| element.clientTop    | 返回元素上边框的大小                                        |
| element.clientLeft   | 返回元素左边框的大小                                        |
| element.clientWidth  | 返回自身包括padding、内容区的宽度，不含边框，返回值不带单位 |
| element.clientHeight | 返回自身包括padding、内容区的高度，不含边框，返回值不带单位 |

### 立即执行函数

（不需要调用立马可以执行的函数）

（function() {})();	第二个小括号可以看做是调用函数，括号中也可以传递参数

eg:（function(a,b) {console.log(a+b);})(1,2)

或者用	(function(){}());		两种方法也都可以加函数名如：(function	sum(){}());

### scroll系列属性

| scroll系列属性       | 作用                                           |
| -------------------- | ---------------------------------------------- |
| element.scrollTop    | 返回被卷去的上侧距离，返回数值不带单位         |
| element.scrollLeft   | 返回被卷去的左侧距离，返回数值不带单位         |
| element.scrollWidth  | 返回自身实际的宽度，不含边框，返回数值不带单位 |
| element.scrollHeight | 返回自身实际的高度，不含边框，返回数值不带单位 |

div.addEventListener('scroll',function(){});当div中的滚动条滚动的时候会触发

overflow:auto;当文字超出设定范围的时候会自动显示滚动条

position='fixed';把定位改为固定定位

**对于页面：**

window.pageYOffset	获取页面被卷去的头部（用于做滚动特效）（上下滚动）

window.pageXOffset	获取页面被卷去的头部（用于做滚动特效）（左右滚动）

window.scroll(x,y);	直接将页面滚动到指定的位置（x:为页面距离屏幕左侧的距离；y:为页面距离屏幕右侧的距离）

eg:window.scroll(0,0);	直接将页面滚动到页面的顶端

document.addEventListener('scroll',function(){});//给整个页面添加滚动监听事件

**对于一个div中的滚动事件：**

div.scrollTop	获取div被卷去的头部（用于做滚动特效）（上下滚动）

div.scrollLeft	获取div被卷去的头部（用于做滚动特效）（左右滚动）

div.addEventListener('scroll',function(){});//给div添加滚动监听事件

### mouseove和mouseenter区别

mouseover鼠标经过自身盒子会触发，经过子盒子还会触发。mouseenter只会经过自身盒子触发，因为mouseenter不会冒泡，跟mouseenter搭配鼠标离开mouseleave同样不会冒泡。

### 动画原理（元素必须加定位）

1、获得盒子当前位置

2、让盒子在当前位置加上1个移动距离

3、利用定时器不断重复这个操作

4、加一个结束定时器的条件

5、注意此元素需要添加定位，才能使用element.style.left

#### 常见动画函数的封装：

```js
function animate(obj,target) {

	clearInterval(obj.timer);//先清除以前的定时器，只保留当前的一个定时器执行（防止同时执行多次同一个定时器产生动画叠加效果）
		obj.timer=setInterval(function(){		//此句代码可以给obj对象添加一个属性timer；
		if(动画停止条件){
					clearInterval(obj.timer);		//停止动画（即停止计时器）	
			}
			具体动画代码
	},间隔时间);
}
```

#### 缓动动画公式：

1、让盒子每次移动的距离慢慢变小，速度就会慢慢停下来

2、缓动动画公式：（目标值-现在的位置）/10；

3、停止的条件是：让当前盒子位置等于目标位置就停止定时器

**常见缓动动画函数的封装：**

```js
function animate(obj,target,callback) {
		clearInterval(obj.timer);//先清除以前的定时器，只保留当前的一个定时器执行（防止同时执行多次同一个定时器产生动画叠加效果）
		obj.timer=setInterval(function(){		//此句代码可以给obj对象添加一个属性timer；
		var step=(target-obj.offsetLeft)/10;//在此处设置步长值
		//因为除法会产生小数，会出现问题
		step=step>0?Math.ceil(step):Math.floor(step);//来对不同的step值进行取整（ceil往上取整）（floor往下取整）
		if(obj.offsetLeft==target){//动画停止条件
					clearInterval(obj.timer);		//停止动画（即停止计时器）
					//回调函数写到定时器结束里面
					if(callback) {
							//调用函数
							callback();
							}
			}
			具体动画代码
	},间隔时间ms);
}
```

### 移动端布局

#### 1、meta视口标签:

```html
<meta name="viewport" content="width=device-width, initial-scale=1.0初始缩放比, maximum-scale=1.0最大缩放比, minimun-scale=1.0最小缩放比, user-scalable=no/yes/0/1用户是否可以缩放">
```

#### 2、点击高亮我们需要清除  设置为transparent 完成透明

```html
-webkit-tap-highlight-color: transparent;
```

#### 3、在移动端浏览器默认的外观在ios上加上这个属性才能给按钮和输入框自定义样式

`-webkit-appearance: none;`

#### 4、禁用长按页面时的弹出菜单

`img,a {-webkit-touch-callout: none;}`

#### 5、flex布局原理：（display:flex;给父级设置）

就是通过给父盒子添加flex属性，来控制子盒子的位置和排列方式。

##### **flex-direction:设置主轴的方向**

| row            | 默认值从左到右 |
| -------------- | -------------- |
| row-reverse    | 默认值从右到左 |
| column         | 从上到下       |
| column-reverse | 从下到上       |
|                |                |

##### **justify-content:设置主轴上的子元素的排列方式**

| flex-start    | 默认值从头部开始如果主轴是x轴，则从左到右 |
| ------------- | ----------------------------------------- |
| flex-end      | 从尾部开始排列                            |
| center        | 在主轴居中对齐（如果主轴是x轴则水平居中） |
| space-around  | 平分剩余空间                              |
| space-between | 先两边贴边再平分剩余空间（重要）          |

##### **flex-wrap:设置子元素是否换行**

| nowrap | 默认为子元素不自动换行 |
| ------ | ---------------------- |
| wrap   | 设置为自动换行         |

##### **align-content:设置侧轴上的子元素的排列方式（多行**）

| flex-start    | 默认值再侧轴的头部开始排列             |
| ------------- | -------------------------------------- |
| flex-end      | 在侧轴的尾部开始排列                   |
| center        | 在侧轴中间显示                         |
| space-around  | 子项在侧轴平分剩余的空间               |
| space-between | 子项在侧轴先分布在两头，再平分剩余空间 |
| stretch       | 设置子项元素高度平分父元素高度         |

##### **align-items:设置侧轴上的子元素的排列方式（单行）**

| flex-start | 从上到下                 |
| ---------- | ------------------------ |
| flex-end   | 从下到上                 |
| center     | 挤在一起居中（垂直居中） |
| stretch    | 拉伸（默认值）           |

##### **flex-flow:符合属性，相当于同时设置了flex-direction和flex-wrap**

eg:`flex-flow:row wrap;`

##### **flex：数值；剩余空间的分配，未分配空间的每个子盒子所占剩余空间的份数**

##### **align-self:可以控制单个盒子的变动**

##### **order:数值;默认是0，数值越小排列越靠前**

#### 6、overflow

| overflow-x：hidden; | 隐藏水平滚动条 |
| ------------------- | -------------- |
| overflow-y：hidden; | 隐藏垂直滚动条 |

#### 7、背景线性渐变：

```css
background: -webkit-linear-gradient(起始方向，颜色1，颜色2，...);

background: -webkit-linear-gradient(left,red,blue);

background: -webkit-linear-gradient(left top,red,blue);
```

#### 8、媒体查询：可以根据不同的屏幕尺寸在改变不同的样式

语法:

```css
@media mediatype and/not/only (media feature) {
		CSS代码;
}
```

| mediatype | 查询类型                           |
| --------- | ---------------------------------- |
| all       | 用于所有的设备                     |
| print     | 用于打印机和打印浏览               |
| screen    | 用与电脑屏目，平板电脑，智能手机等 |

**关键字：**

| and  | 可以将多个媒体特性连接到一起，相当于且的意思 |
| ---- | -------------------------------------------- |
| not  | 排除某个媒体类型，相当于非的意思，可以省略   |
| only | 指定某个特定的媒体类型，可以省略             |

```css
@media screen and (max-width: 800px) {
		body {
			background-color: pink;
	}
}
/*在屏幕上 并且最大的宽度是 800px 设置我们想要的样式。*/
```

**8、媒体查询+rem实现元素动态变化：**

```css
@media screen and (min-width:320px) {
		html {
			font-size:50px;
		}
}
```

```css
@media screen and (min-width:640px) {
			html {
				font-size:100px;
		}
}
```

```css
.top {
		height:1rem;/*.top的高度=html的高度*/
		font-size:.5rem;/*.top的文字大小=html的文字大小*0.5*/
}
```

**9、媒体查询中的引入资源**

eg: `<link rel="stylesheet" href="style320.css" media="screen and (min-width: 320px)">`当屏幕宽度大于320px时调用style320.css的文件。

10、用不同的设备打开显示不同的页面：

```css
if((navigator.userAgent.match(/(phone|pad|pod|iPhone|iPod|ios|iPad|Android|Mobile|BlackBerry|IEMobile|MQQBrowser|JUC|Fennec|wOSBrowser|BrowserNG|WebOS|Symbian|Windows Phone)/i))) {
	window.location.href=""; 		//手机
}	else {
		window.location.href="";	//电脑
}
```

### less:（便于后期的维护）

(1)less的使用：首先新建一个后缀为less的文件，在这个less文件里面书写less语句

(2)变量的定义：@变量名：属性;

(3)变量命名规范：必须有@为前缀。不能包含特殊字符。不能以数字开头。大小写敏感。

(4)less编译：编译生成相应的css文件，然后通过link引入即可（easy less插件）

(5)less嵌套：eg:    .header{属性  a{属性}}   

​								伪类选择器的less嵌套：a{属性  &:hover{属性}}

(6)less运算：less文件夹中支持算数运算。（运算符的左右两侧必须敲一个空格隔开）

​					（如果两个运算数都有的单位且不同，那么结果以第一个单位为准）

### 引入js文件

```js
<script src="js/flexible.js"></script>
```

### 移动端触屏事件

| touchstart     | 手指触摸到一个DOM元素时触发                       |
| -------------- | ------------------------------------------------- |
| touchmove      | 手指在一个DOM元素上滑动时触发                     |
| touchend       | 手指从一个DOM元素上移开时触发                     |
| touches        | 正在触摸屏幕的所有手指的一个列表                  |
| targetTouches  | 正在触摸当前DOM元素上的手指的一个列表（使用最多） |
| changedTouches | 手指状态发生了改变的列表，从无到有，从有到无变化  |

注意：手指移动也会触发滚动屏幕所以这里要阻止默认的屏幕滚动e.preventDefault();

### classList属性

添加类：(在后面追加类名，不会覆盖原类名)

element.classList.add('类名');	eg:focus.classList.add('current');

移除类：

element.classList.remove('类名');

document.body.classList.toggle('类名');//可以自动判断是否有这个类名，如果有就删除此类，如果没有就添加此类

### click延时解决方案（移动端click会有300ms延时的问题）

1.禁止缩放。浏览器禁用默认的双击+缩放行为并且去掉300ms的点击延迟。

```html
<meta name="viewport" content="user-scalable=no">
```

2.利用touch事件自己封装这个事件解决300ms延迟。

原理就是：

1、当我们手指触摸屏幕，记录当前的触摸时间

2、当我们手指离开屏幕，用离开的时间减去触摸的时间

3、如果时间小于150ms，并且没有滑动过屏幕，那么我们就定义为点击

```js
//分装tap,解决click 300ms延时
function tap (obj, callback) {
    var isMove = false;
    var startTime = 0; //记录触摸时候的时间变量
    obj.addEventListener('touchstart', function(e) {
        startTime = Date.now(); //记录触摸时间
    });
    obj.addEventListener('touchmove', function(e) {
        isMove = true; //看看是否有滑动，有滑动算拖拽，不算点击
    });
    obj.addEventListener('touchhend', function(e) {
        if (!isMove && (Date.now() - startTime) < 150) {
            //如果手指触摸和离开时间小于150ms算点击
            callback && callback();//执行回调函数
        }
        isMove = false; //取反 重置
        startTime = 0;
    });
}
//调用
tap(div, function() {
    //执行代码
});
```

4.fastclick插件解决300ms延迟。

GitHub官网地址：https://github.com/ftlabs/fastclick

### 防抖和节流

#### 防抖：

函数防抖，就是指触发事件后，在n秒后只能执行一次，如果在n秒内又触发了事件，则会重新计算函数的执行时间。

>简单来说，当一个动作连续触发，只执行最后一次

#### 常见应用场景：

1、搜索框搜索输入。只需要用户最后一次输入完再发送请求

2、手机号、邮箱格式的输入验证检测

3、窗口大小的resize。只需窗口调整完成后，计算窗口的大小，防止重复渲染。

#### 简单实现：(debounce)

```js
const debounce = (func, wait) => {
    let timer;
    return () => {
        clearTimeout(timer);
        timer = setTimeout(func, wait);
    }
}
```

> 函数防抖在执行目标方法时，若前一个定时任务未执行完，则清楚掉定时任务，重新定时。

#### 封装成函数：

```js
function debounce(fn, delay = 500) {
    let timer = null;
    return function() {
        if (timer) {
            clearTimeout(timer);
        }
        timer = setTimeout(() => {
            fn.apply(this, arguments);
            timer = null;
        }, delay);
    }
}
//简单使用
var myInput = document.getElementById('myInput');
myInput.addEventListener('keyup', debounce(function() {
    console.log(myInput.value);
}, 600));
```

> 当按下某个键的时候触发keydown事件，并执行回调。timer默认为null,在return 的函数中定时器timer被赋值，如果在delay延迟之内再次触发了keydown事件，那么timer就会被重置为null...,当用户输入完成之后（delay事件已过），那么就会触发debounce中的回调函数，也就是keydown最终要执行的事件。

例子：

```js
let btn = document.querySelector('button');
        btn.addEventListener('click', login(fn, 1000, true));

        function fn(...arguments) {
            console.log('send request!!!');
            console.log(arguments);
        }
        let obj = {
            a: 1,
            b: 2,
            c: 3
        }

        function login(fn, delay = 500, immediate) {
            let timer = null;
            let flag = true;
            return function() {
                if (immediate && flag) {
                    fn.apply(this, [...Object.values(obj)]);
                    flag = false;
                } else {
                    if (timer) {
                        clearTimeout(timer);
                    }
                    timer = setTimeout(() => {
                        fn.apply(this, [777]);
                        timer = null;
                    }, delay)
                }
            }
        }
```



#### 节流：

> 限制一个函数在一定时间内只能执行一次

#### 常见应用场景：

1、滚动加载，加载更多或滚动到底部监听

2、谷歌搜索框，搜索联想功能

3、高频点击提交，表单重复提交

4、省市信息对应字母快速选择

#### 简单实现：(throttle)

```js
const throttle = (func, wait) => {
    let timer;
    return () => {
        if (timer) {
            return;
        }
        timer = setTimeout(() => {
            func();
            timer = null;
        }, wait);
    }
}
```

> 函数节流的目的，是为了限制函数一段时间内只能执行一次。因此，通过使用定时任务，延时方法执行。在延时的时间内，方法若被触发，则直接退出方法。从而实现一段时间内只执行一次。

#### 封装成函数：

```js
function throttle(func, delay) {
    let timer = null;
    return function() {
        if (timer) {
            return;
        }
        timer = setTimeout(() => {
            func.apply(this, arguments);
            timer = null;
        })
    }
}
//简单使用
let myDiv = document.getElementById('myDiv');
myDiv.addEventListener('drag', throttle(function(e) {
    console.log(e.offsetX, e.offsetY);
}, 100))
```

> 如果timer存在，那就直接返回，不再往下执行了。这样就实现了一段时间内执行一次的目的。

**总结：**

> 函数防抖关注一段时间内连续触发，只在最后一次执行；而函数节流侧重于在一段时间内只执行一次

### JS闭包

闭包就是将一个变量和函数封装成一个函数，实现了数据的私有，外部可以使用，但外部不能修改。

闭包**可能**引起内存泄漏：例：

```js
function fn() {
    let count = 1;
    function func() {
        count ++;
        console.log(`函数被调用${count}次`)
    }
    return func;
}
const result = fn();
result();//2
result();//3
```

解释：

1、result是一个全局变量，代码执行完毕不会立即销毁

2、result使用fn函数

3、fn用到func函数

4、func函数里面用到count

5、count被引用就不会被回收，所以一直存在

**此时：闭包引起了内存泄漏**

> 注意：不是所有的内存泄漏都要手动回收的，比如react里面很多闭包是不能回收的。

### JS事件循环(eventloop)

1、JS是单线程，为了防止代码阻塞，我们把代码（任务）分为：同步和异步

2、同步代码给js引擎执行，异步代码交给宿主环境

3、同步代码放入执行栈中，异步代码等待时机成熟送入任务队列排队

4、执行栈执行完毕，会去任务队列中看是否有异步任务，有就送到执行栈执行，反复循环查看执行，这个过程是事件循环（eventloop）

### 宏任务和微任务

任务可以分为同步任务和异步任务，异步任务又分为宏任务和微任务。

宏任务是由宿主（浏览器、Node）发起的，微任务是由JS引擎发起的任务。

主要的微任务有Promise、process.nextTick(node)、Async/Await、Object.observe等，Promise本身是同步的，promise里面的then/catch的回调函数是异步的。

主要的宏任务有script（代码块）、setTimeout / setInterval定时器、setImmediate 定时器。

执行顺序：

1、同步任务

2、异步微任务

3、异步宏任务

例题：

```js
async function async1() {
    console.log('async1 start');//同步任务
    await async2();
    console.log('async1 end');//异步微任务
}
async function async2() {
    console.log('async2')//同步任务
}
console.log('script start');//同步任务
setTimeout(function() {
    console.log('setTimeout');
},0);//异步宏任务
async1();
/*
执行结果:
script start
async1 start
async2
async1 end
setTimeout
*/
```

### 原型和原型链

#### 原型：

每个函数都有prototype属性称之为原型，因为这个属性的值是个对象，也称为原型对象

**作用：**

1、存放一些属性和方法，共享给实例对象使用

2、在JavaScript中实现继承

问题：为什么实例化一个实例对象后，这个对象就可以使用原型上的方法呢？

解答：因为我们实例化的实例对象上有一个属性`__proto__`,它指向原型对象，即实例对象.`__proto__` === 原型对象.`prototype`

#### 原型链：

对象都有`__proto__`属性，这个属性指向它的原型对象，原型对象也是对象，也有`__proto__`属性，指向原型对象的原型对象，这样一层一层形成的链式结构称为原型链，最顶层找不到则返回null

### 练习事件循环网站：

[JS Visualizer 9000 (jsv9000.app)](https://www.jsv9000.app/)

### 解决谷歌浏览器无法默认播放音乐的问题

```html
<audio src="01.mp3" autoplay="autoplay" loop="loop"></audio>
<script>
	function toggleSound() {
        var music = document.querySelector('audio');
        //console.log(music)
        //console.log(music.paused)
        if(music.paused) {//判断是否播放
            music.paused = false;
            music.play();//没有就播放
        }
    }
    setInterval(toggleSound, 1000);
</script>
```

### jQuery

在线版jQuery（地址：staticfile.org）

#### jQuery API文档

[***\*jQuery-API文档\****](https://jquery.cuishifeng.cn/)

#### 入口函数

**等着页面DOM加载完毕再去执行js代码**

$(document).ready(function(){

​	eg:$('div').hide();

})       (原始写法)

$(function(){

​	$('div').hide();

})      (现在推荐的简单写法)

#### DOM对象转换为jQuery对象：$(DOM对象)

eg:$('div')

#### jQuery对象转换为DOM对象：(两种方式)

$('div')会获取所有的div；

$('div')[index]//index是索引号

$('div').get(index)//index是索引号

#### JQuery基础选择器

| 名称       | 用法            | 描述                     |
| ---------- | --------------- | ------------------------ |
| ID选择器   | $("#id")        | 获取指定ID的元素         |
| 全选选择器 | $("*")          | 匹配所有元素             |
| 类选择器   | $(".class")     | 获取同一类class的元素    |
| 标签选择器 | $("div")        | 获取同一类标签的所有元素 |
| 并集选择器 | $("div,p,li")   | 选取多个元素             |
| 交集选择器 | $("li.current") | 交集元素                 |

#### JQuery基础选择器

| 名称       | 用法       | 描述                                                         |
| ---------- | ---------- | ------------------------------------------------------------ |
| 子代选择器 | $("ul>li") | 使用>号，获取亲儿子层级的元素；注意，并不会获取孙子层级的元素 |
| 后代选择器 | $("ul li") | 使用空格，代表后代选择器，获取ul下的所有li元素，包括孙子     |

#### jQuery设置样式

$(''div'').css("属性","值")

eg:$(”div“).css("background","pink");//将页面中所有的div的背景颜色变为pink色

#### jQuery中的筛选选择器

| 语法       | 用法          | 描述                                                      |
| ---------- | ------------- | --------------------------------------------------------- |
| :first     | $("li:first") | 获取第一个li元素                                          |
| :last      | $("li:last")  | 获取最后一个li元素                                        |
| :eq(index) | $("li:eq(2)") | 获取到的li元素中，选择索引号为2的元素，索引号index从0开始 |
| :odd       | $("li:odd")   | 获取到的li元素中，选择索引号为奇数的元素                  |
| :even      | $("li:even")  | 获取到的li元素中，选择索引号为偶数的元素                  |

#### jQuery中的筛选方法

| 语法               | 用法                           | 说明                                                    |
| ------------------ | ------------------------------ | ------------------------------------------------------- |
| parent()           | $("li").parent();              | 查找父级                                                |
| children(selector) | $("ul").children("li")         | 相当于$("ul>li"),最近一级（亲儿子）                     |
| find(selector)     | $("ul").find("li")             | 相当于$("ul li"),后代选择器                             |
| siblings(selector) | $(".first").siblings("li")     | 查找兄弟结点，不包括自己本身                            |
| nextAll([expr])    | $(".first").nextAll()          | 查找当前元素之后所有的同辈元素                          |
| prevAll([expr])    | $(".last").prevAll()           | 查找当前元素之后的同辈元素                              |
| hasClass(class)    | $("div").hasClass("protected") | J检查当前的元素是否含有某个特定的类，如果有，则返回true |
| eq(index)          | $("li").eq(2)                  | 相当$("li:eq(2)")，index从0开始                         |

$(this)	jQuery当前元素	this不要加引号

show()	显示元素	hide()	隐藏元素

#### 链式编程

eg:$(this).css(''color'',''red'').sibling().css("color","");//让自己的color变成red，且其他同级元素去掉颜色

$(this).css({color:"red",font-size:20px})(如果属性值不是数字则需要加引号)

#### 类操作

| $("div").addClass("类名")    | （类名不能加点）追加类                                 |
| ---------------------------- | ------------------------------------------------------ |
| $("div").removeClass("类名") | 移除类名                                               |
| $("div").toggleClass("类名") | 切换类（如果有此类就删除此类，如果没有此类就加上此类） |

#### 显示隐藏效果（动态下拉和动态上拉）

show("slow"或"normal"或“fast”或1000,"swing"或“linear”,回调函数，在动画完成时执行的函数，每个元素执行一次)

hide(),hide(),toggle(),slideDown(),slideUp(),slideToggle(),fadeln(),fadeOut(),fadeToggle(),fadeTo(),animate()等（查jQuery文档）

```js
eg1:$(".nav>li").hover(function(){

$(this).children("ul").slideDown(200);},function(){

	$(this).children("ul").stop().slideUp(200);

});		//(stop必须写在动画的前面，停止动画排队)
//注意：动画或者效果一旦触发就会执行，如果短时间内多次触发，就会造成多个动画或者效果排队执行。所以需要在下一次动画开始前，停止上一次动画。
```

```js
eg2:$(".nav>li").hover(function(){

	$(this.children("ul").stop().slideToggle();)

})		//(stop必须写在动画的前面，停止动画排队)
//注意：动画或者效果一旦触发就会执行，如果短时间内多次触发，就会造成多个动画或者效果排队执行。所以需要在下一次动画开始前，停止上一次动画。
```

eg1和eg2效果相同（动态下拉和动态上拉）

#### jQuery动画函数

```js
$("div").animate({
	left:	500,
    top:	300,
	opacity:	.4,
	width:	500
},500)
```

#### jQuery API文档

[jQuery API 中文文档 | jQuery API 中文在线手册 | jquery api 下载 | jquery api chm (cuishifeng.cn)](https://jquery.cuishifeng.cn/)

#### 属性操作

| prop("属性")                                                 | 获取属性语法                                         |
| ------------------------------------------------------------ | ---------------------------------------------------- |
| prop("属性","属性值")                                        | 设置属性语法                                         |
| attr("属性")	//类似原生getAttribute()                     | 获取自定义属性                                       |
| attr("属性"，”属性值“)	//类似原生setAttribute()           | 设置属性语法                                         |
| $("span").data("uname","andy");  console.log($("span").data("uname")); | 数据缓存data()这个里面的数据是存放在元素的内存里面的 |
| console.log($("div").html());      console.log($("div").text()); | 获取元素内容（含标签）html()                         |
| $("div").html("内容")；    $("div").text("内容");            | 设置元素内容                                         |
| console.log($("input").val());       $("input").val("内容"); | 获取表单值	val()                                  |

#### 遍历dom对象each()方法

eg:$("div").each(function(index,domEle){

$(domEle).css("color","red");此时循环将每一个div中的文字颜色变为了红色

})此时的index将对所有的div进行编号domEle为dom对象不能直接用jQuery方法

注意：1、each()方法遍历匹配的每一个元素。主要用DOM处理。each每一个

2、里面的回调函数有2个参数：index是每一个元素的索引号；demEle是每个DOM元素对象，不是jquery对象

3、所有想要使用jquery方法，需要给这个dom元素转换为jquery对象$(domEle)

#### 遍历数据对象(数组或对象)

```js
var arr = [1,2,3];
$.each(arr, function(i,ele) {
    console.log(i);//索引号
    console.log(ele);//值
})

var obj = {
    name: "andy",
    age: 18
};
$.each(obj, function(i,ele) {
    console.log(i);//属性名 如name、age
    console.log(ele);//属性值 如andy、18
})
```

#### 创建、添加、删除元素

```js
//1、创建元素
var li = $("<li>123</li>");

//2、添加元素
/*（1）内部添加*/
$("ul").append(li);//内部添加并且放到内容的最后面
$("ul").prepend(li);//内部添加并且放到内容的最前面
/*（2）外部添加*/
var div = $("<div>123</div>");
$("div").after(div);//外部添加并且放到该结点的后面
$("div").before(div);//外部添加并且放到该结点的前面

//3、删除元素
$("ul").remove();//可以删除匹配的元素 "自杀"
$("ul").empty();//可以删除匹配的元素里面的子节点 "孩子"
$("ul").html("");//可以删除匹配的元素里面的子节点 "孩子"
```

#### jQuery尺寸

| 语法                                 | 用法                                                         |
| ------------------------------------ | ------------------------------------------------------------ |
| width() / height()                   | 取得匹配元素的宽度和高等值 只算 width / height;如果括号里面带参数就是设置宽度 / 高度 |
| innerWidth() / innerHeight()         | 取得匹配元素的宽度和高等值 包含 padding 、                   |
| outerWidth() / outerHeight()         | 取得匹配元素的宽度和高等值 包含 padding 、border             |
| outerWidth(true) / outerHeight(true) | 取得匹配元素的宽度和高等值 包含 padding 、border、margin     |

#### jQuery位置

位置主要有三个：offset()、position()、scrollTop()/scrollLeft()

##### 1、offset()设置或获取元素偏移

（1）offset()方法实则hi在或返回被选元素相对于文档的偏移坐标，跟父级没有关系

（2）该方法有2个属性left、top、offset().top 用于获取距离文档顶部的距离，offset().left用于获取距离文档左侧的距离

（3）可以设置元素的偏移：offset({top: 10,left: 30});

##### 2、position()设置或获取元素偏移

(1)position() 方法用于返回被选元素相对于带定位的父级偏移坐标，如果父级都没有定位，则以文档为准

**注意：position()只能获取不能设置属性值**

##### 3、scrollTop()/scrollLeft()设置或获取元素被卷去的头部和左侧

```js
/*页面滚动事件*/
$(window).scroll(function() {
    $(document).scrollTop()//页面被卷去的头部
})

/*带有动画的返回顶部*/
$.(".back").click(function() {
    $("body, html").stop().animate({
        scrollTop: 0
    });
})
```

#### 事件的绑定与解绑

##### on()方法优势：可以绑定多个事件，多个处理事件处理程序

```js
$("div").on({
	mouseover: function() {},
    mouseout: function() {},
    click: function() {}
});

//鼠标经过加上current类鼠标离开去掉current类
$("div").on("mouseover mouseout", function() {
    $(this).toggleClass("current");
});
```

##### on实现事件委托（委派）

```js
$("ul").on("click", "li", function() {
    
})//click事件绑定在ul身上，但是触发对象是li
```

##### on可以给未来动态创建的元素绑定事件

```js
$("ol").on("click", "li", function() {
	alert(11);
})
var li = $("<li>123</li>");
$("ol").append(li);
```

#### off()解绑事件

```js
//off()方法可以移除通过on()方法添加的事件处理程序
$("p").off()//解绑p元素所有处理程序
$("p").off("click")//解绑p元素上的点击事件
$("ul").off("click", "li")//解绑事件委托
```

#### one() 只触发一次事件

```js
$("p").one("click",function(){
	alert(11);
})
```

#### 自动触发事件trigger()

1、元素.事件（）自动触发事件

eg: element.click();

2、通过trigger自动触发事件

element.trigger("click");

3、通过triggerHandler自动触发事件() (不会触发元素的默认行为)

element.triggerHandler("click");

#### extend()对象拷贝

$.extend([deep], target, object1, [objectN])

1、deep：如果设为true为深拷贝，默认为false浅拷贝

2、target：要拷贝的目标对象

3、object1：待拷贝到第一个对象的对象

4、objectN：带拷贝到第N个对象的对象

5、浅拷贝是把被拷贝的对象复杂数据类型中的地址拷贝给目标对象，修改目标对象会影响被拷贝对象

注意：浅拷贝把原来对象里面的复杂数据类型地址拷贝给目标对象；深拷贝把里面的数据完全复制一份给目标对象，如果里面有不冲突的属性，会合并到一起

#### 多库共存

如果$符号冲突

1、可以用jquery代替$

2、用户自定义

eg:var	新字符=jQuery.noConflict();

以后就可以用”新字符“代替jQuery

#### jQuery插件

[***\*jQuery插件库\****](https://www.jq22.com/)***\*-和-\****[***\*（常用)jQuery之家\****](http://www.htmleaf.com/)

#### 懒加载

相关js文件必须写在页面中所有图片的下面；

EasyLazyload.js(查上方插件库)

#### 全屏滚动（fullpage.js）

gitHub:	http://github.com/alvarotrigo/fullPage.js

中文翻译网站：http://www.dowebok.com/demo/2014/77/

#### ECharts的使用

步骤1：下载并引入echarts.js文件————图标依赖这个js库

步骤2：准备一个具备大小的DOM容器————生成图标会放入这个容器内

步骤3：初始化echarts实例对象————实例化echarts对象

步骤4：指定配置项和数据（option）————根据具体需求修改配置选项

步骤5：将配置项设置给echarts实例对象————让echarts对象根据修改好滴配置生效

### boostrap地址

****[***\*Bootstrap\****](https://www.bootcss.com/)

### Ajax基础

#### Ajax的实现步骤

##### get请求

1、创建Ajax对象

```js
var xhr = new XMLHttpRequest();
```

2、告诉Ajax请求地址以及请求方式

```js
//拼接请求参数
var params = "username=" + nameValue + '&age=' + ageValue;
//配置ajax对象
xhr.open('get', 'http://localhost:3000/get?' + params);
```

3、发生请求

```js
xhr.send();
```

4、获取服务器端给与客户端的响应数据

```js
xhr.onload = function() {
	console.log(xhr.responseText);
    var responseText = JSON.parse(xhr.responseText);//将json字符串转变为json对象
}
```

##### post请求

1、创建Ajax对象

```js
var xhr = new XMLHttpRequest();
```

2、告诉Ajax请求地址以及请求方式

```js
//拼接请求参数
var params = "username=" + nameValue + '&age=' + ageValue;
//配置ajax对象
xhr.open('get', 'http://localhost:3000/post');
//设置请求参数格式的类型（post请求必须要设置）
xhr.setRequestHeader('Content-Type', 'application/x-www-form-urlencoded');
```

3、发生请求

```js
xhr.send(params);
//如果params是对象则需将其转变成字符串
//xhr.send(JSON.stringify({name: 'lisi', age: 50}))
```

4、获取服务器端给与客户端的响应数据

```js
xhr.onload = function() {
	console.log(xhr.responseText);
    var responseText = JSON.parse(xhr.responseText);//将json字符串转变为json对象
}
```

#### 获取http状态码

xhr.status

当网络中断时会触发onerror事件

xhr.onerror = function() {

​	alert('网络中断，无法发送Ajax请求')

}

#### Ajax函数封装

```js
// ajax函数封装
function ajax(options) {
    // 存储默认值
    var defaults = {
        type: 'get',
        url: '',
        data: {},
        header: {
            'Content-Type': 'application/x-www-form-urlencoded'
        },
        success: function() {},
        error: function() {}
    };
    // 使用options对象中的属性覆盖defaults对象中的属性
    // assign方法对覆盖原对象所以不用重新给原对象赋值
    Object.assign(defaults, options);
    // 创建ajax对象
    var xhr = new XMLHttpRequest();
    // 拼接请求参数的变量
    var params = '';
    // 循环用户传递进来的对象格式参数
    for (var attr in defaults.data) {
        // 将参数转换为字符串格式
        params += attr + '=' + defaults.data[attr] + '&';
    }
    // 将参数最后面的&截取掉
    // 将截取的结果重新赋值给params变量
    params = params.substr(0, params.length - 1);
    // 判断请求方式
    if (defaults.type == 'get') {
        defaults.url = defaults.url + '?' + params;
    }
    // 配置ajax对象
    xhr.open(defaults.type, defaults.url);
    if (defaults.type == 'post') {
        // 用户希望的向服务器端传递的请求参数的类型
        var contentType = defaults.header['Content-Type'];
        xhr.setRequestHeader('Content-Type', contentType);
        //    判断用户希望的请求参数格式的类型
        // 如果类型为json
        if (contentType == 'application/json') {
            //    向服务器端传递json数据格式的参数
            xhr.send(JSON.stringify(defaults.data))
        } else {
            //    向服务器端传递json数据格式的参数
            xhr.send(params);
        }
    } else {
        // 发送请求
        xhr.send();
    }
    // 监听xhr对象下面的onload事件
    // 当xhr对象接收完响应数据后触发
    xhr.onload = function() {
        // 获取响应头中的数据
        var contentType = xhr.getResponseHeader('Content-Type');
        var responseText = xhr.responseText;
        // 如果响应类型中包含application/json
        if (contentType.includes('application/json')) {
            // 将json字符串转换为json对象
            responseText = JSON.parse(responseText)
        }
        // 当http状态码等于200的时候
        if (xhr.status == 200) {
            // 请求成功，调用处理成功情况的函数
            defaults.success(responseText, xhr);
        } else {
            // 请求失败，调用处理失败情况的函数
            defaults.error(responseText, xhr);
        }
    }
}
```

#### 客户端模板引擎的使用

```html
<!DOCTYPE html>
<html lang="en">

<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Document</title>
    <!-- 1、将模板引擎的库文件引入到当前页面 -->
    <script src="../js/template-web.js"></script>
</head>

<body>
    <div id="container"></div>
    <!-- 2、准备art-template模板 -->
    <script type="text/html" id="tpl">
        <h1>{{username}} {{age}}</h1>
    </script>
    <script type="text/javascript">
        // 3、告诉模板引擎将哪个数据和哪个模板进行拼接
        // 1）模板id 2）数据 对象类型
        // 方法的返回值就是拼接好的html字符串
        var html = template('tpl', {
            username: 'zhangsan',
            age: 30
        })
        document.getElementById('container').innerHTML = html;
    </script>
</body>

</html>
```

#### 搜索框自动提示

```html
<!DOCTYPE html>
<html lang="en">

<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>搜索框自动提示</title>
    <style type="text/css">
        .container {
            padding-top: 150px;
        }
        
        .list-group {
            display: none;
        }
    </style>
</head>

<body>
    <div class="container">
        <div class="from-group">
            <input type="text" class="form-control" placeholder="请输入搜索关键字" id="search">
            <ul class="list-group" id="list-box">

            </ul>
        </div>
    </div>
    <script src="../js/ajax.js"></script>
    <script src="../js/template-web.js"></script>
    <script type="text/html" id="tpl">
        {{each result}}
        <li class="list-group-item">
            {{$value}}
        </li>
        {{/each}}
    </script>
    <script>
        // 获取搜索框
        var searchInp = document.getElementById('search');
        // 获取提示文字的存放容器
        var listBox = document.getElementById('list-box');
        // 存储定时器的变量
        var timer = null;
        // 当用户在文本框中输入的时候触发
        searchInp.oninput = function() {
            // 清除上一次开启的定时器
            clearTimeout(timer);
            // 获取用户输入的内容
            var key = this.value;
            // 如果用户没有在搜索框中输入内容
            // trim清除内容前后的空格
            if (key.trim().length == 0) {
                listBox.style.display = 'none';
                // 阻止程序向下执行
                return;
            }
            timer = setTimeout(function() {
                // 向服务器端发送请求
                // 向服务器端索取和用户输入关键字相关的内容
                ajax({
                    type: 'get',
                    url: 'http://localhost:8080/api/searchAutoPrompt',
                    data: {
                        key: key
                    },
                    success: function(result) {
                        // 使用模板引擎拼接字符串
                        var html = template('tpl', {
                            result: result.data
                        })

                        // 将拼接好的字符串显示到页面中
                        listBox.innerHTML = html;
                        // 显示ul容器
                        listBox.style.display = 'block'
                    }
                })
            }, 800)
        }
    </script>
</body>

</html>
```

#### 腾讯天气

jsonp可以解决跨域问题

```html
<!DOCTYPE html>
<html lang="en">

<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Document</title>
    <script src="https://cdn.staticfile.org/jquery/3.6.0/jquery.min.js"></script>
    <style>
        .container {
            margin-left: 300px;
        }
        
        table {
            width: 800px;
            text-align: center;
            border-collapse: collapse;
        }
        
        th,
        td {
            border: 1px solid grey;
            background-color: skyblue;
        }
    </style>
</head>

<body>
    <div class="container">
        <table class="table table-striped table-hover" id="box">

        </table>
    </div>
    <script src="../js/ajax.js"></script>
    <script src="../js/template-web.js"></script>
    <script type="text/html" id="tpl">
        <tr>
            <th>时间</th>
            <th>温度</th>
            <th>天气</th>
            <th>风向</th>
            <th>风力</th>
        </tr>
        {{each info}}
        <tr>
            <td>{{dateFormat($value.update_time)}}</td>
            <td>{{$value.degree}}</td>
            <td>{{$value.weather}}</td>
            <td>{{$value.wind_direction}}</td>
            <td>{{$value.wind_power}}</td>
        </tr>
        {{/each}}
    </script>
    <script>
        // 获取table标签
        var box = document.getElementById('box');

        function dateFormat(date) {
            var year = date.substr(0, 4);
            var month = date.substr(4, 2);
            var day = date.substr(6, 2);
            var hour = date.substr(8, 2);
            var minute = date.substr(10, 2);
            var seconds = date.substr(12, 2);
            return year + '年' + month + '月' + day + '日' + hour + '时' + minute + '分' + seconds + '秒';
        }
        // 向模板中开放外部变量
        template.defaults.imports.dateFormat = dateFormat;
        $.ajax({
            type: 'GET',
            url: 'https://wis.qq.com/weather/common',
            dataType: 'jsonp',//jsonp可以解决跨域问题
            data: {
                source: 'pc',
                weather_type: 'forecast_1h',
                // weather_type: 'forecast_1h|forecast_24h',
                province: '湖北省',
                city: '武汉市'
            },
            success: function(data) {
                var html = template('tpl', {
                    info: data.data.forecast_1h
                });
                box.innerHTML = html;
            }
        })
    </script>
</body>

</html>
```

#### 验证邮箱的唯一性

```html
<!DOCTYPE html>
<html lang="en">

<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>验证邮箱地址唯一性</title>
    <style type="text/css">
        input {
            outline: none;
        }
        
        p:not(:empty) {
            padding: 15px;
        }
        
        .container {
            padding-top: 100px;
        }
    </style>
</head>

<body>
    <div class="container">
        <div class="form-group">
            <label for="">邮箱地址:</label>
            <input type="email" class="form-control" placeholder="请输入邮箱地址" id="email">
        </div>
        <!-- 错误bg-danger 正确bg-success -->
        <p id="info"></p>
    </div>
    <script src="../js/ajax.js"></script>
    <script>
        var emailInp = document.getElementById('email');
        var info = document.getElementById('info');
        emailInp.onblur = function() {
            // 获取用户输入的邮箱地址
            var email = this.value;
            // 验证邮箱地址的正则表达式
            var reg = /^[A-Za-z\d]+([-_.][A-Za-z\d]+)*@([A-Za-z\d]+[-.])+[A-Za-z\d]{2,4}$/;
            if (!reg.test(email)) {
                info.innerHTML = '请输入符合规则的邮箱地址';
                info.className = 'bg_danger';
                return;
            }
            // 向服务器端发送请求
            ajax({
                type: 'get',
                url: 'http://localhost:8080/api/verify_email',
                data: {
                    email: email
                },
                success: function(result) {
                    console.log(result);
                    info.innerHTML = result.message;
                    info.className = 'bg-success';
                },
                error: function(result) {
                    console.log(result);
                    info.innerHTML = result.message;
                    info.className = 'bg-danger';
                }
            });
        }
    </script>
</body>

</html>
```

#### FormData对象的使用

```html
<!DOCTYPE html>
<html lang="en">

<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Document</title>
</head>

<body>
    <!-- 创建普通的html表单 -->
    <form id="form">
        <input type="text" name="username">
        <input type="password" name="password">
        <input type="button" id="btn" value="提交">
    </form>
    <p></p>
    <script>
        /*
                get('key')获取表单对象属性的值
                set('set','value')设置表单对象属性的值（如果设置的表单属性存在，将会覆盖属性原有的值；如果设置的表单属性不存在，将会创建这个表单属性）
                delete('key') 删除表对象属性中的值
                formData.append('key', 'value') 向表单对象中追加属性值
                */

        // 获取按钮
        var btn = document.getElementById('btn');
        // 获取表单
        var form = document.getElementById('form');
        var info = document.querySelector('p');
        // 为按钮添加点击事件
        btn.onclick = function() {
            // 将普通的html表单转换为表单对象
            var formData = new FormData(form);
            // 创建ajax对象
            var xhr = new XMLHttpRequest();
            // 对ajax对象进行配置
            xhr.open('post', 'http://localhost:8080/api/formData');
            // 发送ajax请求
            xhr.send(formData);
            // 监听xhr对象下面的onload事件
            xhr.onload = function() {
                // 对象http状态码进行判断
                if (xhr.status == 200) {
                    console.log(xhr.responseText);
                }
            }
        }
    </script>
</body>

</html>
```

#### FormData二进制文件上传

```html
<!DOCTYPE html>
<html lang="en">

<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>FormData二进制文件上传</title>
    <style type='text/css'>
        .container {
            padding-top: 60px;
        }
        
        .padding {
            padding: 5px 0 20px 0;
        }
        
        .progress {
            width: 1400px;
            height: 10px;
            border: 1px solid grey;
            border-radius: 15px;
        }
        
        .progress-bar {
            background-color: pink;
            height: 10px;
            border-radius: 15px;
            text-align: center;
        }
    </style>
</head>

<body>
    <div class="container">
        <div class="form_group">
            <label for="">请选择文件</label>
            <input type="file" id="file">
            <div class="progress">
                <div class="progress-bar" style="width: 0%;" id="bar">0%</div>
            </div>
            <div class="padding" id="box">
            </div>
        </div>
    </div>
    <script type="text/javascript">
        // 获取文件选择控件
        var file = document.getElementById('file');
        // 获取进度条元素
        var bar = document.getElementById('bar');
        // 获取图片容器
        var box = document.getElementById('box');
        // 为文件选择控件添加onchanges事件
        // 在用户选择文件时触发
        file.onchange = function() {
            // 创建空的FromData表单对象
            var formData = new FormData();
            // 将用户选择的文件追加到formData表单对象中
            formData.append('attrName', this.files[0]);
            // 创建ajax对象
            var xhr = new XMLHttpRequest();
            // 对ajax对象进行配置
            xhr.open('post', 'http://localhost:8080/api/upload');
            // 在文件上传的过程中持续触发
            xhr.upload.onprogress = function(ev) {
                    // ev.loaded文件已经上传了多少
                    // er.total上传文件的总大小
                    var result = (ev.loaded / ev.total) * 100 + '%';
                    // 设置进度条的宽度
                    bar.style.width = result;
                    // 将百分比显示在进度条中
                    bar.innerHTML = result;
                }
                // 发送ajax请求
            xhr.send(formData);
            // 监听服务器端响应给客户端的数据
            xhr.onload = function() {
                // 如果服务器端返回的http状态码为200
                // 说明请求是成功的
                if (xhr.status == 200) {
                    // 将服务器端返回的数据显示在控制台中
                    var result = JSON.parse(xhr.responseText);
                    // 动态创建img标签
                    var img = document.createElement('img');
                    // 给图片标签设置src属性
                    img.src = 'http://localhost:8080' + result.path;
                    // 当图片加载完成以后
                    img.onload = function() {
                        // 将图片显示在页面中
                        box.appendChild(img);
                    }
                }
            }
        }
    </script>
</body>

</html>
```

#### serialize的使用

```html
<!DOCTYPE html>
<html lang="en">

<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>serialize方法说明</title>
</head>

<body>
    <form action="" id="form">
        <input type="text" name="username">
        <input type="password" name="password">
        <input type="submit" value="提交">
    </form>
    <script src="https://cdn.staticfile.org/jquery/3.6.0/jquery.min.js"></script>
    <script type="text/javascript">
        $('#form').on('submit', function() {
                // 将表单内容拼接成字符串类型的参数，如name=zhangsan&age=30
                // var params = $('#form').serialize();
                console.log(serializeObject($(this)));
                return false;
            })
            // 将表单中用户输入的内容转换为对象类型
        function serializeObject(obj) {
            // 处理结果对象
            var result = {};
            var params = obj.serializeArray();
            // 循环数组 将数组转换为对象类型 params:[{name: 'username', value: '输入的内容'},{name: 'password',value: '123456'}]
            $.each(params, function(index, value) {
                    result[value.name] = value.value;
                })
            //处理后结果：{username: '用户输入的内容', password: '123456'}
                // 将处理的结果返回到函数外部
            return result;
        }
    </script>
</body>

</html>
```

#### $.ajax()方法

```js
$.ajax({
	type: 'get',
	url: 'http://www.example.com',
	data: {name: 'zhangsan', age: '20'},
	contentType: 'application/x-www-form-urlencoded',
	//请求发送之前被调用
    beforeSend: function() {
        ...
        //请求不会被发送
        return false;
    },
    success: function(response) {},
    error: function(xhr) {}
});


var params = {name: 'zhangsan', age: '20'};
$.ajax({
	type: 'get',
	url: 'http://www.example.com',
	data: JSON.stringify(params),
	contentType: 'application/json',
	//请求发送之前被调用
    beforeSend: function() {
        ...
        //请求不会被发送
        return false;
    },
    success: function(response) {},
    error: function(xhr) {}
});
```

#### $.get()的使用

```js
$.get('http://www.example.com',{name: 'zhangsan', age: 30}, function(response) {})
//其中第二个参数可以是对象和字符串，也可以省略，第三个参数就是请求成功后的回调函数
```

#### $.post()的使用

```js
$.post('http://www.example.com',{name: 'zhangsan', age: 30}, function(response) {})
//其中第二个参数可以是对象和字符串，也可以省略，第三个参数就是请求成功后的回调函数
```

#### ajax全局事件

NProgress链接地址https://rstacruz.github.io/nprogress/

```js
//NProgress的使用
//官宣：纳米级进度条，使用逼真的滑动动画来告诉用户正在发生的事情！
/*引入相关css和js*/
<link rel='stylesheet' href='nprogress.css'/>
<script src='nprogress.js'></script>

//当页面中有ajax请求开始发送时触发
$(document).on('ajaxStart', function() {
    NProgress.start()
})
//当页面中有ajax请求完成时触发
$(document).on('ajaxComplete', function() {
    NProgress.done()
})
```

#### RESTful API的实现

```
GET: 获取数据
POST: 添加数据
PUT: 更新数据
DELETE: 删除数据

GET: http://www.example.com/users			获取用户列表数据
POST: http://www.example.com/users			创建（添加）用户数据
GET: http://www.example.com/users/1			获取用户ID为1的用户信息
PUT: http://www.example.com/users/1			修改用户ID为1的用户信息
DELETE: http://www.example.com/users/1		删除用户ID为1的用户信息
```

### WebSocket的基本使用

> 介绍：websocket是一种网络协议，允许客户端和服务端全双工的进行网络通讯，服务器可以给客户端发送消息，客户端也可以给服务器发送消息；且二者之间的连接时长连接，可以进行实时通信。

[WebSocket - Web API 接口参考 | MDN (mozilla.org)](https://developer.mozilla.org/zh-CN/docs/Web/API/WebSocket)

#### 创建websocket对象

```js
//参数1： url: 连接的websocket属性
//参数2：protocol: 可选的，指定连接的协议
// var socket = new WebSocket('ws://echo.websocket.org')
var Socket = new WebSocket(url, [protocol]);
```

#### websocket事件

| 事件    | 事件处理程序     | 描述                         |
| ------- | ---------------- | ---------------------------- |
| open    | Socket.onopen    | 连接建立时触发               |
| message | Socket.onmessage | 客户端接收服务器端数据时触发 |
| error   | Socket.onerror   | 通信发生错误时触发           |
| close   | Socket.onclose   | 连接关闭时触发               |

**事件使用（以opne事件为例）**

```js
socket.addEventListener('open', function() {
	console.log('连接服务器成功！')
})
```

#### 客户端给服务器端发送数据

`socket.send(数据)`

#### 客户端接收服务器端返回的数据

```js
socket.addEventListener('message', function(e) {
    console.log(e.data);//输出服务器端返回到客户端的数据
})
```

#### 断开连接时触发的事件

```js
socket.addEventListener('close', funtion() {
	console.log('断开连接')                        
})
```

#### 使用nodejs开发websocket服务

> 接下来我们自己通过nodejs实现一个简单的websocket服务
>
> > 使用nodejs开发websocket需要依赖一个第三方包[GitHub - sitegui/nodejs-websocket: A node.js module for websocket server and client](https://github.com/sitegui/nodejs-websocket#readme)

##### 项目搭建

> 安装nodejs-websocket：`yarn add nodejs-websocket`

**app.js**

```js
//1、引入nodejs-websocket包
const ws = require('nodejs-websocket')
const PORT = 5000

//2、创建一个server
//2.1如何处理用户的请求
//每次只要有用户连接，函数就会被执行，会给当前连接的用户创建一个connect对象
const server = ws.createServer(connect => {
    console.log('有用户连接上来了')
    //每当接收到用户传递过来的数据，这个text事件就会被触发
    connect.on('text', data => {
        console.log('接收到了用户传递过来的数据：' + data)
        //给用户一个响应的数据
        connect.send(data)
    })
    //只要websocket连接断开，close事件就会触发
    connect.on('close', () => {
        console.log('连接断开')
    })
    //注册一个error,处理用户的错误信息
    connect.on('error', () => {
        console.log('用户连接异常')
    })
})

server.listen(PORT, () => {
    console.log('websocket服务启动成功了，监听了端口' + PORT)
})
```

#### socketio的使用

> Socket.IO 是一个库，可以在客户端和服务器之间实现 **低延迟**, **双向** 和 **基于事件的** 通信。

socketio开发文档地址：[Get started | Socket.IO](https://socket.io/get-started/chat)

**基本使用：**

安装：`npm install --save socket.io`

> 后续内容看上面所给出的卡法文档吧！