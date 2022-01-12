---
title: 如何优雅的使用ES6
tags: 优雅
categories: ES6
abbrlink: e38dcee3
date: 2022-01-11 08:58:45
---
## 解构赋值
### 解构的默认值
undefined不支持，null相当于有值，但值为null
```js
let [foo = true] = [];
console.log('log',foo)
let [a,b="i100"]=['百里']
console.log(a+b); //控制台显示“百里i100”
```
> 注意：对象的解构与数组有一个重要的不同。数组的元素是按次序排列的，变量的取值由它的位置决定；而对象的属性没有次序，变量必须与属性同名，才能取到正确的值。
<!--more-->
### 圆括号的使用
在解构之前定义了变量在解构会报错，解决方法是在整体的外边加一个括号
```js
let foo;
({foo} = {foo: 'i100'});
console.log('log',foo);
```
### 字符串解构
```js
const [a,b,c,d] = 'test'
console.log('log',a,b,c,d) // t,e,s,t
```
## 扩展与rest运算符
### 对象扩展运算符
编写一个方法，允许入参是不确定的
```js
function i100 (...arg) {
  console.log('log',arg[0],arg[1])
}
i100(1,2,3)
```
### 扩展运算符用处
声明2个数组arr1，arr2，把arr1赋值给arr2，然后改变arr2的值发现arr1的值也变了，这是对内存堆栈的引用，不是真正的赋值
```js
let arr1 = ['www','i100','xyz'];
let arr2 = arr1;
console.log('log',arr2)
arr2.push('add')
console.log('i100',arr1)
```
> 我们可以利用扩展运算符
```js
let arr2 = [...arr1]
```
### rest运算符
与对象扩展运算符有相似之处
```js
function i100(first,...rest) {
  console.log('i100',resr.length)
}
i100(0,1,2,3)
```
## 模板字符串
### 简单使用
```js
let i100 = '百里';
let blog = `这是${i100}的博客`
```
### 对运算的支持
```js
let a=1;
let b=2;
let sum = `${a+b}`
```
### 字符串查询
- 查询全部
```js
let res = blog.includes(i100)
```
- 查询开头
```js
let res = blog.startsWith(i100)
```
- 查询结尾
```js
let res = blog.endsWith(i100)
```

## 新增数组知识
### JSON格式转换
特殊的json格式都可以轻松使用ES6的语法转变成数组
```js
let json = {
  '0': 'i100',
  '1': '百里',
  '2': '博客',
  length: 3
}
let arr = Array.from(json)
console.log('i100',arr)
```
### Array.of()方法
它负责把一堆文本或者变量转换成数组。
```js
let arr = Array.of(1,2,3,4)
console.log('i100',arr)
```
### find()实例方法
find方法是从数组中查找。在find方法中我们需要传入一个匿名函数，函数需要传入三个参数：
- value：表示当前查找的值。
- index：表示当前查找的数组索引。
- arr：表示当前数组。
在函数中如果找到符合条件的数组元素就进行return，并停止查找。
```js
let arr = [1,2,3,4,5];
let res = arr.find(function(value, index, arr) { 
  return value > 2;
})
```

### fill()实例方法
fill()也是一个实例方法，它的作用是把数组进行填充，它接收三个参数，第一个参数是填充的变量，第二个是开始填充的位置，第三个是填充到的位置。
```js
let arr = [1,2,3,4,5];
arr.fill('i100',2,3)
// 上边的代码是把数组从第二位到第三位用i100进行填充。
```
### 数组的遍历
- for...of循环
这种形式比ES5的for循环要简单而且高效。先来看一个最简单的for…of循环
```js
let arr = ['i100','百里','博客'];
for (let item of arr) {
  console.log('i100',item);
}
```

for…of数组索引:有时候开发中是需要数组的索引的，那我们可以使用下面的代码输出数组索引
```js
let arr = ['i100','百里','博客'];
for (let index of arr.keys()) {
  console.log('i100',index);
}
```
同时输出数组的内容和索引：我们用entries()这个实例方法，配合我们的for…of循环就可以同时输出内容和索引了。
```js
let arr = ['i100','百里','博客'];
for (let [index,val] of arr.entries()) {
  console.log('i100',index+':'+val)
}
```
### entries实例方法
entries()实例方式生成的是Iterator形式的数组，那这种形式的好处就是可以让我们在需要时用next()手动跳转到下一个值。
```js
let arr = ['i100','百里','博客'];
let list = arr.entries();
console.log('i100-1',list.next().value);
console.log('i100-2',list.next().value);
console.log('i100-3',list.next().value);
```
## 箭头函数和扩展
### 默认值
```js
function add(a,b=1) {
  return a+b;
}

console.log('i100',add(1));
```
### 主动抛出错误
```js
function add(a,b=1) {
  if(a==0) {
    throw new Error('This is error')
  }
  return a+b;
}
console.log('i100',add(0))
```
### 箭头函数
箭头函数中不可加new，也就是说箭头函数不能当构造函数进行使用
```js
let add = (a,b=1) => {
  return a+b;
}
  console.log('i100',add(1))
```
## 函数与数组的补漏
### 对象的函数解构
```js
let json = {
  a: 'i100',
  b: '百里',
  c: '博客'
}
function fun({a,b='test'}) {
  console.log('i100',a,b)
}
fun(json)
```
## 数组函数解构
声明一个数组，然后写一个方法，最后用…进行解构赋值。
```js
let arr = ['i100','百里','博客'];
function fun(a,b,c) {
  console.log('i100',a,b,c)
}
fun(...arr)
```
### `in`的用法
> in是用来判断对象或者数组中是否存在某个值的
- 对象判断
```js
let obj = {
  a: 'i100',
  b: '百里',
  c: '博客'
}
console.log('i100' in obj); // true
```
- 数组判断
先来看一下ES5判断的弊端，以前会使用length属性进行判断，为0表示没有数组元素。但是这并不准确，或者说真实开发中有弊端。
```js
let arr = [,,,,,];
console.log('i100',arr.length); // 5
```
上边的代码输出了5，但是数组中其实全是空值，这就是一个坑啊。那用ES6的in就可以解决这个问题。
```js
let arr = [,,,,,];
console.log('i100',0 in arr); // false
let arr1 = ['i100','百里','博客'];
console.log('i100',0 in arr1) // true
```
> 这里的0指的是数组下标位置是否为空。

### 数组遍历用法
- `forEach`
```js
let arr = ['i100','百里',,'博客'];
arr.forEach((val,index)=>console.log('i100',index,val))
```
> forEach循环的特点是会自动省略为空的数组元素，相当于直接给我们筛空了。当是有时候也会给我们帮倒忙。
- `filter`
```js
let arr = ['i100','百里','博客'];
arr.filter(x=>console.log(x));
```
- `some`
```js
let arr = ['i100','百里','博客'];
arr.some(x=>console.log(x));
```
- `map`
```js
let arr = ['i100','百里','博客'];
console.log('i100',arr.map(x=>'wb'));
```
> map在这里起到一个替换的作用

数组转换字符串 在开发中我们经常会碰到把数组输出成字符串的形式，我们今天学两种方法，你要注意两种方法的区别
- `join`
```js
let arr = ['i100','百里','博客'];
console.log('i100',arr.join('|'))
```
> join()方法就是在数组元素中间，加了一些间隔，开发中很有用处
- `toString`
```js
let arr = ['i100','百里','博客'];
console.log('i100',arr.toString());
```
> 转换时只是是用逗号隔开了。

## ES6对象
对象对于Javascript是非常重要的。在ES6中对象有了很多新特性。
###对象赋值
> ES6允许把声明的变量直接赋值给对象
```js
let name = 'i100';
let skill = 'web';
let obj = {name,skill}
console.log('i100',obj);
```
## 对象Key值构建
有时候我们会在后台取出key值，而不是我们前台定义好的，这时候我们如何构建我们的key值那。比如我们在后台取了一个key值，然后可以用[ ] 的形式，进行对象的构建。
```js
let key = 'skill';
let obj = {
  [key]: 'web'
}
console.log('i100',obj.skill);
```
### 自定义对象方法
- 对象方法就是把兑现中的属性，用匿名函数的形式编程方法
```js
let obj = {
  add: function(a,b) {
    return a + b;
  }
}
console.log('i100',obj.add(1,2));
```
- `Object.is()`对象比较
对象的比较方法,以前进行对象值的比较，经常使用===来判断，比如下面的代码：
```js
let obj1 = {name:'lisi'};
let obj2 = {name:'xiaoming'};
let res = Object.is(obj1,obj2);
console.log('i100',res);
```
- `Object.assign()`合并对象
```js
let a={a:'a'}
let b={b:'b'}
let c={c:'c'}
let d=Object.assign(a,b,c);
console.log('i100',d);
```

## Symbol在对象的作用
### 声明Symbol
```js
let g = Symbol('i100');
console.log('i100',g);
console.log('i100',g.toString());
```
> 这时候我们仔细看控制台是有区别的，没有toString的是红字，toString的是黑字。
### Symbol在对象中的应用
```js
let i100 = Symbol();
let obj = {
  [i100]: '百里'
}
console.log('i100',obj[i100]);
obj[i100]='web';
console.log('i100',obj[i100]);
```
### Symbol对象元素的保护作用
>在对象中有很多值，但是循环输出时，并不希望全部输出，那我们就可以使用Symbol进行保护。

- 没有进行保护的写法：
```js
let obj = {
  a: 'i100',
  b: '百里',
  c: '博客'
};
for(let item in obj) {
  console.log('i100',obj[item]);
}
```
现在我不想别人知道我的年龄，这时候我就可以使用Symbol来进行循环保护。
```js
let obj = {
  a: 'i100',
  b: '百里',
};
let age = Symbol();
obj[age] = 18;
for(let item in obj) {
  console.log('i100',obj[item]);
}
console.log('i100',obj);
```

## Set数据结构
> Set数据结构，注意这里不是数据类型，而是数据结构。它是ES6中新的东西，并且很有用处。Set的数据结构是以数组的形式构建的。
### Set声明
```js
let setArr = new Set(['i100','百里','博客']);
console.log('i100',setArr); // Set(3) {'i100', '百里', '博客'}
```
> Set和Array 的区别是Set不允许内部有重复的值，如果有只显示一个，相当于去重。虽然Set很像数组，但是他不是数组。
### Set值的增删查
- 追加add
在使用Array的时候，可以用push进行追加值，那Set稍有不同，它用更语义化的add进行追加
```js
let setArr = new Set(['i100','百里','博客']);
setArr.add('前端');
console.log('i100',setArr);
```
- 删除delete
```js
let setArr = new Set(['i100','百里','博客']);
setArr.delete('前端');
console.log('i100',setArr);
```
- 查找has
用has进行值的查找，返回的是true或者false。
```js
let setArr = new Set(['i100','百里','博客']);
setArr.has('i100');
```
- 删除clear
```js
let setArr = new Set(['i100','百里','博客']);
setArr.clear();
console.log('i100',setArr);
```
- set的for...of循环
```js
let setArr = new Set(['i100','百里','博客']);
for(let item of setArr) {
  console.log('i100',item);
}
```
- size属性
size属性可以获取Set值的数量
```js
let setArr = new Set(['i100','百里','博客']);
console.log('i100',setArr.size);
```
- forEach循环
```js
let setArr = new Set(['i100','百里','博客']);
setArr.forEach((value)=>console.log(value));
```

## Map数据结构
在一些构建工具中是非常喜欢使用map这种数据结构来进行配置的，因为map是一种灵活，简单的适合一对一查找的数据结构。
```js
let obj = {
  a: 'i100',
  b: '百里',
  c: '博客'
};
console.log('i100',obj.c);
let map = new Map();
// 设置value
map.set(obj,'test');
// 设置key
map.set('test',obj);
console.log('i100',map);
```
### 取值get
```js
let res = map.get(obj);
console.log('i100',res);
```
### 删除delete
删除delete指定值：
```js
map.delete(obj)
```
### size属性
```js
map.size
```
### 查找has
```js
map.has('i100');
```
### 清除clear
```js
map.clear()
```
总结：map在现在开发中已经经常使用，它的灵活性和高效性是我们喜欢的。开发中试着去使用map吧，你一定会喜欢上它的。
## 用Proxy进行预处理
>钩子函数：当我们在操作一个对象或者方法时会有几种动作，比如：在运行函数前初始化一些数据，在改变对象值后做一些善后处理。这些都算钩子函数，Proxy的存在就可以让我们给函数加上这样的钩子函数，你也可以理解为在执行方法前预处理一些代码。你可以简单的理解为他是函数或者对象的生命周期。 Proxy的应用可以使函数更加强大，业务逻辑更加清楚，而且在编写自己的框架或者通用组件时非常好用

回顾定义对象方法
```js
var obj={
    add:function(val){
        return val+10;
    },
    name:'I am Jspang'

};
console.log(obj.add(100));
console.log(obj.name);
```
### 声明Proxy
用new的方法对Proxy进行声明。可以看一下声明Proxy的基本形式。
```js
new Proxy({},{});
```
> 需要注意的是这里是两个花括号，第一个花括号就相当于我们方法的主体，后边的花括号就是Proxy代理处理区域，相当于我们写钩子函数的地方。

现在把上边的obj对象改成我们的Proxy形式。
```js
let pro = new Proxy({
  add: function(val) {
    return val + 10;
  },
  name: 'I am i100'
},{
  get:function(target, key, property) {
    console.log('come in Get',target, key, property);
    return target[key];
  }
})
console.log('i100',pro.name);
```
>可以在控制台看到结果，先输出了come in Get。相当于在方法调用前的钩子函数
### get属性
get属性是在你得到某对象属性值时预处理的方法，他接受三个参数
- target：得到的目标值
- key：目标的key值，相当于对象的属性
- property：这个不太常用，用法还在研究中。

### set属性
set属性是值你要改变Proxy属性值时，进行的预先处理。它接收四个参数。
- target:目标值。
- key：目标的Key值。
- value：要改变的值。
- receiver：改变前的原始值。
```js
let pro = new Proxy({
  add: function(val) {
    return val + 10;
  },
  name: 'I am i100'
},{
  get:function(target, key, property) {
    console.log('come in Get',target, key, property);
    return target[key];
  },
  set:function(target, key, value, receiver) {
    console.log('i100-set',key,value);
    return target[key] = value;
  }
})
console.log('i100',pro.name);
pro.name = '李四';
console.log('i100',pro.name);
```

### apple的使用
apply的作用是调用内部的方法，它使用在方法体是一个匿名函数时。
```js
var twice = {
  //目标对象，目标对象的上下文对象，目标对象的参数数组
  apply:function (target,ctx,args) {
    console.log("ctx",ctx,"args",args);
    return Reflect.apply(...arguments)*2;
  }
};
function sum (left,right) {
  return left*right;
};
var proxy = new Proxy(sum,twice);
console.log("proxy1",proxy(1,2));
console.log("proxy.call",proxy.call(null,5,6));
console.log("proxy,apply",proxy.apply(null,[7,8]));
console.log("proxy,apply",Reflect.apply(proxy,null,[7,8]));
```
## promise对象的使用
>ES6中的promise的出现给我们很好的解决了回调地狱的问题，在使用ES5的时候，在多层嵌套回调时，写完的代码层次过多，很难进行维护和二次开发，ES6认识到了这点问题，现在promise的使用，完美解决了这个问题。

### promise基本用法
模拟一个多步骤多过程，如在家吃饭需要三个步骤。
- 开始做饭
- 坐下来吃饭
- 🤕️洗碗
这个过程是有执行顺序多，确保上一步完成，才能继续下一步操作
```js
let state = 1;
function step1(resolve,reject) {
    console.log('1.开始做饭');
    if(state) {
        resolve('做饭完成');
    }else{
        reject('做饭出错了');
    }
}
function step2(resolve,reject) {
    console.log('2.开始吃饭');
    if(state) {
        resolve('吃饭完成')
    }else {
        reject('吃饭出错')
    }
}
function step3(resolve,reject) {
    console.log('3.开始洗碗');
    if(state) {
        resolve('洗碗完成')
    }else {
        reject('洗碗出错')
    }
}
new Promise(step1).then(function (val) { 
    console.log(val);
    return new Promise(step2)
}).then(function (val) { 
    console.log(val);
    return new Promise(step3)
}).then(function (val) {
    console.log(val);
    return val;
})
```

## class类的使用
>在ES5中经常使用方法或者对象去模拟类的使用，虽然可以实现功能，但是代码并不优雅，ES6为我们提供了类的使用。需要注意的是我们在写类的时候和ES5中的对象和构造函数要区分开来，不要学混了。
### 类的声明
```js
class coder {
    name(val) {
        console.log('i100',val);
    }
}
```
### 类的使用
```js
class Coder{
    name(val) {
        console.log('i100',val);
    }
    skill(val) {
        let res = this.name + val;
        console.log('skill',res);
    }
}
let i100 = new Coder();
i100.name('百里');
i100.skill('web');
```
这里需要注意的是两个方法中间不要写逗号了，还有这里的this指类本身，还有要注意return 的用法。
### 类的传参
在类的参数传递中我们用constructor( )进行传参。传递参数后可以直接使用this.xxx进行调用。
```js
class Coder{
    name(val) {
        console.log('i100',val);
    }
    skill(val) {
        let res = this.name + val;
        console.log('skill',res);
    }
    constructor(a,b) {
        this.a = b;
        this.b = b;
    }
    add() {
        return this.a + this.b;
    }
}
let i100 = new Coder(1,2);
console.log('i100',i100);
```
用constructor来约定了传递参数，然后用作了一个add方法，把参数相加。
### class的继承
```js
class htmler extends Coder {
    
}
let i100 = new htmler();
i100.name('百里')
```
声明一个htmler的新类并继承Coder类，htmler新类里边为空，这时候我们实例化新类，并调用里边的name方法。结果也是可以调用到的。
## 模块化操作
在ES5中我们要进行模块华操作需要引入第三方类库，随着前后端分离，前端的业务日渐复杂，ES6为我们增加了模块话操作。模块化操作主要包括两个方面。
- export :负责进行模块化，也是模块的输出。
- import : 负责把模块引，也是模块的引入操作。

### export的用法
export可以让我们把变量，函数，对象进行模块话，提供外部调用接口，让外部进行引用。先来看个最简单的例子，把一个变量模块化。
新建一个temp.js文件，然后在文件中输出一个模块变量
```js
export let i00 = '百里';
```
然后可以在index.js中以import的形式引入。
```js
import {i100} from './temp.js';
console.log('i100',i100);
```
这就是一个最简单的模块的输出和引入。
### as用法
有些时候我们并不想暴露模块里边的变量名称，而给模块起一个更语义话的名称，这时候我们就可以使用as来操作。

```js
let a = 'i100', b = '百里', c = 'web';
export {
    x as a,
    y as b,
    z as c
}
```
### export default的使用
加上default相当是一个默认的入口。在一个文件里export default只能有一个。我们来对比一下export和export default的区别
#### export
```js
export let i100 = '百里';
export function add(a,b) {
    return a+b;
}
```
对应的导入方式
```js
import {i100,add} from './temp';
```
#### export default
```js
export default let i100 = '百里';
```
对应的导入方式
```js
import str from './temp';
```