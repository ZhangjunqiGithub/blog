---
title: flutter
categories: flutter
tags: flutter
date: 2024/4/1
cover: https://pic4.zhimg.com/v2-6ed60bca7d0b1bcd936533374f26aa9c_720w.jpg?source=172ae18b
---

### flutter在Windows平台下的安装配置

>[Flutter在Windows平台下的安装配置 - zxsh - 博客园 (cnblogs.com)](https://www.cnblogs.com/zxsh/p/8859048.html)

### 闭包（和jS中的闭包一样）

```dart
fn() {
	var a = 0;
    return () {
        a++;
        print(a);
    }
}
```

### 创建固定长度和类型的List

```dart
List list = new List<int>.filled(2,0);
list[0] = 1;
list[1] = 2;
print(list);//[1,2]
/**
	只固定类型
	List list = <int>[];
*/
```

### 命名构造函数

```dart
class Person{
    String name;
    int age;
    //默认构造函数的简写
    Person(this.name, this.age);
    Person.now() {
        print('我是命名构造函数1');
    }
    Person.setIngo() {
        print('我是命名构造函数2');
    }
    void printInfo() {
        print("${this.name}---${this.age}");
    }
}
void main() {
    //调用自定义构造函数
    Person p = new Person.now();
}
```

> Dart和其他面向对象语言不一样，Dart中没有 public private protected这些访问修饰符号
>
> 但是我们可以使用`_`把一个属性或者方法定义成私有的如下：
>
> ```dart
> class Person {
>     String name;
>     int _age;//私有变量
>     //构造函数的简写方式
>     Person(this.name,this._age);
>     void printInfo() {
>         print("${this.name}---${this._age}");
>     }
>     void _run() {
>         print('这是一个私有方法');
>     }
>     void execRun() {
>         this._run();//通过该公有的方法去调用Person类中的私有方法
>     }
> }
> ```

> 注意：只有将这个类抽离为一个文件后，在其他的文件中引入并使用的时候才能体现出这个私有的特性
>
> ```dart
> void main() {
>     Person p = new Person('zhangsan','123');
>     print(p.name);//输出：zhangsan
>     print(p._age);//报错，无法访问
>     p._run();//报错，无法调用
>     p.execRun();//输出：这是一个私有方法
> }
> ```

### 类中的getter和setter

> 这个类似与vue中的计算属性

```dart
class Rect{
    num height;
    num width;
    Rect(this.height,this.width);
    get area{
        return this.height*this.width;
    }
    set areaHeight(value) {
        this.height=value;
    }
}
void main() {
    Rect r = new Rect(10,4);
    r.areaHeight=6;//setter
    print(r.area);//getter
}
```

### 在构造函数体运行之前初始化实例变量

```dart
class Rect{
    int height;
    int width;
    Rect():height=2,width=10{
        print('${this.height}---${this.width}');
    }
    getArea() {
        return this.height*this.width;
    }
}
void main() {
    Rect r = new Rect();
    print(r.getArea());
}
```

### 继承

```dart
class Father {
    String name;
    num age;
    Father(this.name,this.age);
    Print() {
        print('${this.name}---${this.age}');
    }
}

class Son extends Father {
    String job;
    Son(String name, num age, this.job): super(name, age) {}//实例化子类的时候给父类传参
}
```

> 在Dart中无法实现`多继承`，如果想要实现多继承可以使用`mixins`实现类似多继承的功能。其实就是使用`with`
>
> ```dart
> class A {
>     void printA() {
>         print("A");
>     }
> }
> class B {
>     void printB() {
>         print("B");
>     }
> }
> class C with A,B {
>     
> }
> void main() {
>     C c = new C();
>     c.printA();//A
>     c.printB();//B
>     print(c is C);//true
>     print(c is A);//true
>     print(c is B);//true
> }
> ```
>
> 注意：mixins会随着Dart版本改变而改变
>
> mixins注意事项：
>
> 1. 作为mixins的类只能继承自Object，不能继承其他类。也就是说上面的A类和B类不能继承其他类。
> 2. 作为mixins的类不能有构造函数。也就是说上面的A类和B类不能有构造函数。
> 3. 如果非要继承继承了其他类或有构造函数的类D，那么可以这样写`class C extends D with A,B`
> 4. 一个类可以mixins多个mixins类
> 5. mixins不是继承，也不是接口，而是一种全新的特性,A类和B类其实是C的一个超类

### 接口

```dart
abstract class A {
    String name;
    void my_print1(String data);
}
abstract class B {
    String age;
    void my_print2(String data);
}
class C implements A,B {
    @override
    String name;
    
    @override
    String age;
    
    C(this.name,this.age);
    
    @override
    void my_print1(String data) {
        print(this.name);
    }
    
    @override
    void my_print2(String data) {
        print(this.age);
    }
}
void main() {
    C c = newe C('zhangsan',18);
    c.my_print1();//zhangan
    c.my_print2();//18
}
```

> Dart可以继承多个接口

### 多态

```dart
abstract class Animal {
    run();
}
class Dog extends Animal {
    @override
    run() {
        print('Dog running');
    }
}
class Cat extends Animal {
    @override
    run() {
        print('Cat running');
    }
}

void main() {
    Animal dog = new Dog();
    Animal cat = new Cat();
    dog.run();//Dog running
    cat.run();//Cat running
}
```

### 泛型

> 通俗理解：泛型就是解决 `类` `接口` `方法`的复用性、以及对不特定数据类型的支持（类型校验）

#### 泛型方法

````dart
T getData<T>(T value) {
    return value;
}
void main() {
    print(getData(21));//可以实现功能但是没有对传入的数据进行类型校验
    print(getData<String>("123"));//对传入的数据进行类型校验，必须为String类型
}
````

#### 泛型类

```dart
class MyList<T> {
    List list = <T>[];
    void add(T value) {
        this.list.add(value);
    }
    List getList() {
        return list;
    }
}
void main() {
    MyList l1 = new MyList();
    l1.add("张三");
    l1.add(123);
    l1.add(true);
    print(l1.getList());//["张三",12,true]
    MyList l2 = new MyList<String>();
    l2.add("李四");
    l2.add(123);//报错
    print(l2.getList());//["李四"]
}
```

```dart
abstract class Cache<T> {
    getByKey(String key);
    void setByKey(String key, T value);
}
class MemoryCache<T> implements Cache<T> {
    @override
    getByKey(String key) {
        return null;
    }
    @override
    void setByKey(String key, T value) {
        
    }
}
void main() {
    MemoryCache m = new MemoryCache<Map>();
    m.setByKey('index', {"name":"张三","age":20});
}
```

### Dart中的库、自定义库、系统库、第三方库

> 在Dart中库在使用时通过import关键字引入
>
> library指令可以创建一个库，每个Dart文件都是一个库，即使没有使用library指令来指定
>
> Dart中的库主要有三种：
>
> 1. 我们自定义的库
>
>    - import 'lib/xxx/dart';
>
> 2. 系统内置库
>
>    - import 'dart:math';
>    - import 'dart:io';
>    - import 'dart:convert';
>
> 3. Pub包管理系统中的库
>
>    - https://pub.dev/packages
>    - https://pub.flutter-io.cn/packages
>    - https://pub.dartlang.org/flutter
>
>    1. 需要在自己的项目根目录中新建一个`pubspec.yaml`
>    2. 在`pubspec.yaml`文件，然后配置名称、描述、依赖等信息
>    3. 然后运行 `pub get`获取包下载到本地
>    4. 项目中引入库`import 'package:http/http.dart' as http;`看文档使用

### async、await

```dart
//异步方法
testAsync() async{
    return 'Hello async';
}
void main() async{
    var result = await testAsync();
    print(result);
}
```

### 解决引入的两个库中有同名的方法或类的情况

```dart
import 'lib/Person1.dart';
import 'lib/Person2.dart' as lib;//利用as进行重命名
main(List<String> args) {
    Person p1 = new Person('张三', 20);
    
    lib.Person p2=new lib.Person('李四', 20);
}
```

### 部分引入

```dart
//lib/myfunc.dart
int getAge();
String getName();
String getSex();
```

```dart
import 'lib/myfunc.dart' show getAge;//只引入getAge方法
import 'lib/myfunc.dart' hide getAge;//引入除了getAge外的所有方法
```

### 空安全中可空类型的定义

```dart
String? username="张三";//String? 表示username是一个可空类型
username = null;
print(username);//null
```

