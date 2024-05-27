---
title: TypeScript基础
categories: TypeScript基础
tags: TypeScript基础
date: 2023/10/1
cover: https://tse1-mm.cn.bing.net/th/id/OIP-C.m4m_xmJrAndBw761wWzH1QHaE8?rs=1&pid=ImgDetMain
---

### TypeScript的基本使用

#### 安装TypeScript包

```bash
npm i -g typescript
```

> 解释：全局安装typescript包

```typescript
//hello.ts
console.log('Hello TS')
```

#### 将ts文件转化为js文件

```bash
tsc hello.ts
```

> 此时当前目录中会生成一个hello.js文件

#### 执行

```bash
node hello.js
```

> 注意：在执行的时候，执行的是`js`文件

#### 简化执行TS的步骤

> 使用ts-node包，可以直接在Node.js中执行TS代码
>
> > 原理：其实就是借助这个包将上面的步骤简化为一个步骤

> 全局安装`ts-node`

```bash
npm i -g ts-node
```

**ts-node包的使用**

```bash
ts-node hello.ts
```

> 注意这里执行的是ts文件，而且这里将不再生成新的`hello.js`,可以直接输出js执行的结果

#### 变量的基本使用

1. 声明变量并指定类型

   ```typescript
   let age: number;
   ```

2. 给变量赋值

   ````typescript
   age = 18
   //声明时就赋初值
   let age: number = 18
   ````

#### 类型注解

> 作用：是一种为变量添加类型约束的方式
>
> 也就是说在声明时指定了变量是什么类型，那么在后面使用该变量的时候就只能时指定的类型

#### TypeScript中常用的数据类型

- number：包含整数类型和浮点数类型
- string
- boolean
- undefined
- null

#### 创建数组的两个语法形式

```typescript
let name: string[] = []
let age: number[] = []
let age: number[] = [1, 2, 3, 4, 5]
```

> 推荐使用

```typescript
let names: string[] = new Array()
let names: string[] = ['aaa', 'bbb', 'ccc']
```

**向数组中添加元素**

```typescript
name[name.length] = 'ddd'
```

> 如果索引不存在，就表示：添加元素

#### 函数的使用

```typescript
function fn(name: string, age: number) {
    console.log('name = ' + name + ' age = ' + age)
}
fn('zhangsan', 18)
```

> 上面的函数没有返回值
>
> > 注意：如果没有指定函数的返回值，那么，函数返回值的默认类型为void(空)

```typescript
function getSum(nums: number[]): number {
    let sum: number = 0
    for (let i: number = 0; i < nums.length; i++) {
        sum += nums[i]
    }
    return sum
}
let arr: number[] = [1, 2, 3, 4, 5]
let result: number = getSum(arr)
```

> 此处介绍的是有返回值函数的使用，以及返回值的接收
>
> > 注意： 变量`result`的类型与函数`getSum`的返回值类型要一致

#### 调试函数

##### 修改launch.json中的相关配置

```json
{
    "version": "0.2.0",
    "configurations": [
        {
            "type": "node",
            "request": "launch",
            "name": "调试函数", //这个配置其实是将调式界面左上角的调试按钮后面的文字改为 调试函数 这四个字
            //ts-node 命令： 直接运行ts代码
            //作用： 调试时加载ts-node包（再调试时直接运行ts代码）
            "runtimeArgs": ["-r", "ts-node/register"],
            //此处的function_debug.ts表示要调试的TS文件（可以修改为其他要调试的文件）
            "args": ["${workspaceFolder}/function_debug.ts"]
        }
    ]
}
```

#### 对象的类型注解

> TS中的对象是结构化的，结构简单来说就是对象有什么属性或方法。再使用对象前，就可以根据需求，提前设计号对象的结构。比如创建一个对象，包含姓名、年龄两个属性。

```typescript
let person: {
    name: string;
    age: number;
}

person = {
    name: 'zhangsan',
    age: 18
}
```

#### 对象方法的类型注解

> 技巧：鼠标放在变量名称上，VSCode就会给出该变量的类型注解。

```typescript
let person: {
    sayHi: () => void //没有参数和返回值的函数
    sing: (name: string) => void //有参数没有返回值的函数
    sum: (num1: number, num2: number) => number //有参数有返回值的函数
}
```

#### 接口的使用

> 直接在对象名称后面写类型注解的坏处：
>
> 1. 代码结构不简洁
> 2. 无法复用类型注解

---

> 接口：为对象的类型注解命名，并为代码建立契约来约束对象的结构

语法：

```typescript
interface IUser {
    name: string
    age: number
}
let p1: IUser = {
    name: 'zhangsan',
    age: 18
}
```

> interface表示接口，接口名称约定以`I`开头。
>
> 推荐：使用接口来作为对象的类型注解

浏览器中运行TS

> 注意：浏览器中只能运行JS，无法直接运行TS，因此，需要将TS转化为JS然后再运行
>
> 浏览器中运行TS的步骤：
>
> 1. 使用命令tsc index.ts将ts文件转化为js文件
> 2. 在页面中，使用script标签引入生成的js文件（注意是js文件）

```html
<script src="./index.js"></script>
```

> 问题：每次修改ts文件后，都要重新运行tsc命令将ts文件转化为js文件
>
> 解决方法：使用tsc命令的监视模式

```bash
tsc --watch index.ts
```

> 解释：--watch表示启用监视模式，只要重新保存了ts文件，就会自动调用tsc将ts文件转化为js文件

#### TS的类型推论

> 在TS中，某些没有明确指出类型的地方，类型推论会帮助提供类型。
>
> 换句话说：由于类型推论的存在，一些地方，类型注解可以省略不写！
>
> 发生类型推论的2中常见场景：
>
> 1. 声明变量并初始化时
> 2. 决定函数返回值时

```typescript
let num
//如果在声明变量的时候既没有指明类型也没有赋初值，那么这是变量的类型就是any
//如下的赋值都是可以的
num = 12
num = 'aaa'
num = false
```

---

```typescript
let num = 12
//如果在声明的时候就赋初值，那么在后面使用该变量的时候，类型就要以等号（=）后面的数值类型为准
//相当于：let num: number = 12
//这种情况就可以省略掉变量类型，简化编码
```

#### 类型断言

> 问题：调用`querySelector()`通过id选择器获取DOM元素时，拿到的元素类型都是`Element`
>
> 因为无法根据id来确定元素的类型，所以，该方法就返回了一个宽泛的类型：元素（Element）类型
>
> 不管是h1还是img都是元素
>
> 导致新问题：无法访问img元素的src属性了
>
> 因为：Element类型只包含所有元素共有的属性和方法（比如：id属性）

> 解决方法：使用类型断言，来手动指定更加具体的类型（比如，此处应该比Element类型更加具体）
>
> 语法:
>
> ```text
> 值 as 更具体的类型
> ```
>
> 比如：
>
> ```typescript
> let img = document.querySelector('#image') as HTMLImageElement
> ```
>
> > 解释：我们确定id="image"的元素是图片元素，所以，我们将类型指定为HTMLImageElement

---

> 总结：
>
> 类型断言：手动指定更加具体（精确）的类型
>
> 使用场景：当你比TS更了解某个值的类型，并且需要指定更具体的类型时
>
> ```typescript
> //document.querySelector() 方法的返回值类型为：Element
> //如果是 h1 标签
> let title = document.querySelector('#title') as HTMLHeadingElement
> //如果是 img 标签
> let image = document.querySelector('#image') as HTMLImageElement
> ```
>
> > 技巧：通过console.dir()打印DOM对象，来查看该元素的类型

#### 枚举

> 使用变量时存在的问题：例如：
>
> 变量的类型是string,它的值可以是任意字符串
>
> 如果不小心写错了，代码不会报错，但功能就无法实现了，并且很难找错。
>
> 也就是：string类型的变量，取值太宽泛，无法很好的限制值
>
> 枚举是组织有关联数据的一种方式
>
> 使用场景：当变量的值，只能是几个固定值中的一个，应该使用枚举来实现
>
> > 注意：JS中没有枚举，这是TS为了弥补JS自身不足而新增的

创建枚举的语法：

```typescript
enum 枚举名称 { 成员1， 成员2， ... }
```

示例：

```typescript
enum Gender { Female, Male }
```

> 约定枚举名称、成员名称以大写字母开头
>
> 多个成员之间使用逗号（,）分隔
>
> > 注意：枚举中的成员，根据功能自己指定！
> >
> > 注意：枚举中的成员不是键值对！

**使用枚举：**

>枚举是一种类型，因此，可以作为变量的类型注解

```typescript
enum Gender { Female, Male }
let userGender: Gender
```

> 访问枚举（Gender）中的成员，作为变量(userGender)的值：

```typescript
userGender = Gender.Female
userGender = Gender.Male
```

**问题：将枚举成员赋值给变量，变量的值是什么？**

```typescript
enum Gender { Female, Male }
let userGender: Gender = Gender.Female
console.log(userGender) // 0
```

> 枚举成员是有值的，默认为：从0喀什自增的数值
>
> 我们把枚举成员的值为数字的枚举，称为：数字枚举
>
> 当然，也可以给枚举中的成员初始化值

```typescript
enum Gender { Female = 1, Male } // Female => 1		Male => 2
enum Gender { Female = 1, Male = 100 } // Female = 1		Male => 100
```

**字符串枚举：枚举成员的值是字符串**

```typescript
enum Gender { Female = '女', Male = '男' }
```

> 注意：字符串枚举没有自增长的行为，因此，每个成员必须有初始值

```typescript
console.log(Gender.Female) //女
console.log(Gender.Male) //男
```

**两种常用的枚举总结：**

1. 数字枚举：枚举成员的值为数字，默认情况下就是数字枚举

   ```typescript
   enum Gender { Female, Male }
   enum Gender { Female = 100, Male } //初始化成员的值
   ```

   > 特点：成员的值是从0开始自增的数值

2. 字符串枚举：枚举成员的值为字符串

   ```typescript
   enum Gender { Female = '女', Male = '男' }
   ```

   > 特点：没有自增行为，需要为每一个成员赋值！

> **枚举是一组有名字的常量（只读）的集合。**

