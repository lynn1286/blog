---
title: JavaScript重点之原型与原型链
date: 2020-09-01 11:44:04
categories: Javascript
tags: 原型与原型链
---

### JavaScript 之 原型链



#### 对象

理解原型链 的前提是要搞清楚 对象 ，JavaScript中 **万物皆对象** ，但是 对象也是有区别的 ， 它分为 普通对象 和 函数对象。



**Object** 、**Function** 是 JavaScript 中自带的函数对象。

```javascript
// 在 浏览器 中打印 Object 以及 Function 这两个对象
console.log(typeof Object); //function 
console.log(typeof Function); //function  


// 普通对象
var o1 = {}; 
var o2 =new Object();
var o3 = new f1();

console.log(typeof o1); //object 
console.log(typeof o2); //object 
console.log(typeof o3); //object


// 函数对象
function f1(){}; 
var f2 = function(){};
var f3 = new Function('str','console.log(str)');


console.log(typeof f1); //function 
console.log(typeof f2); //function 
console.log(typeof f3); //function   


```



这个例子 可以看出 ，凡是通过 **new Function** 创建的对象都是函数对象 ， 其他的都是普通对象。

> 注意： f1 , f2 归根结底 都是通过new Function 创建出来的 ， Function 以及 Object也都是通过new Function 创建的；JavaScript 中 其他内置对象也都是 通过new Function 创建出来的。



分清楚普通对象 以及 函数对象 这两点 **很重要！！！**



#### 构造函数

记住普通对象和函数对象的区别之后，我们再来看看 构造函数。

先来看个实例：

```javascript
// 我们声明了一个 Person 类 ， 它具有名字 年龄 以及一个行为 sayName 方法
function Person(name, age) {
 this.name = name;
 this.age = age;
 this.sayName = function() { 
   alert(this.name) 
 } 
}

// 实例化
let xiaoming = new Person("小明",18)
let xiaohong = new Person("小红",18)

```

例子中xiaoming 以及 xiaohong 都是Person类的实例，这两个实例都会有一个 constructor（构造函数） 属性,这个属性指向的就是Person这个类，它其实就是一个指针，我们来证明下他们之间的关系:

```javascript
xiaoming.constructor === Person // true
xiaohong.constructor === person // true

```

在这里我们需要记住两个点: 

- 构造函数
- 实例

总结一句话其实就是: **实例的构造函数属性指向构造函数**。



#### 原型对象

记住了构造函数和对象的知识点后，我们再来看看**原型对象**。

在JavaScript中 ，每声明一个对象（函数也是对象）的时候，对象中都会包含一些预定义的属性。其中 **函数对象**都会有一个`prototype` 属性 ，而这个 `prototype`属性指向函数的原型对象



看下实例：

```javascript
function Person() {}

Person.prototype.name = 'xing'; // 在原型对象上添加了 name 属性
Person.prototype.age  = 28; // 在原型对象上添加了 age 属性

Person.prototype.sayName = function() {  // 在原型对象上添加了 sayName 方法
  alert(this.name);
}
  
var person1 = new Person();  // 实例化
person1.sayName(); // 'xing'

var person2 = new Person(); // 实例化
person2.sayName(); // 'xing'

console.log(person1.sayName == person2.sayName); //true

```



我们在Person的prototype属性上添加了各种属性 ， 最后在调用的时候它们其实都是同一个 ， 这是因为 prototype属性指向的是函数的原型对象，如果你还不理解 ，那 直接不要想那么多 ，它 就是一个**普通对象**；你只需要记住: prototype 指 函数的原型对象，就如上述例子: **Person.prototype 就是原型对象**



哦!对了，你还需要记住一个知识点:

>在默认情况下，所有的**原型对象**都会**自动获得**一个 `constructor`（构造函数）属性，这个属性（是一个指针）指向 `prototype` 属性所在的函数（Person）



好像听不太懂的样子. - -!!!

没关系 ，翻译一下就是: **Person.prototype**  有一个 `constructor` 默认属性（自动获得）, 这个属性（`constructor`）指向了 Person ， 也就是:    <font color=red>`Person.prototype.constructor == Person`</font>



到了这里 有没有突然反应过来什么?

在上面 讲`构造函数`的时候 ， 是不是讲过 **实例的构造函数属性指向构造函数**  ， 也就是 <font color=red>`xiaoming.constructor === Person`</font>；好像这样的话 它们之间是不是就会有点联系了啊???

是的!!!

控制台打印看看:

```javascript
function Person(name,age){
    this.name = name
    this.age = age
    this.sayName = function(){
        console.log(this.name)
    }
}

let xiaoming = new Person("小红", 18)
let xiaohong = new Person("小明", 18)

xiaohong.constructor == Person
// true
xiaoming.constructor == Person
// true
Person.prototype.constructor == xiaohong.constructor
// true
Person.prototype.constructor == xiaoming.constructor
// true
```



但是为什么xiaoming 跟 xiaohong 会有 constructor 属性呢? 因为 他们是Person 的实例啊!!!

那Person.prototype 为什么也会有constructor属性呢? 因为他也是Person的实例啊!!!

其实就是 Person 在创建的时候 ，创建了一个它的实例并赋值给了它的prototype属性。



**结论：原型对象（Person.prototype）是 构造函数（Person）的一个实例。**



原型对象其实就是普通对象，但 **Function.prototype** 除外，它是函数对象，但它很**特殊**，他没有prototype属性。

看下实例: 

```javascript
function Person(){};
console.log(Person.prototype) //Person{}
console.log(typeof Person.prototype) //Object
console.log(typeof Function.prototype) // Function，这个特殊
console.log(typeof Object.prototype) // Object
console.log(typeof Function.prototype.prototype) //undefined
```



`Function.prototype` 为什么是函数对象呢？

  上面提到过 <font color=red>凡是通过 **new Function** 创建的对象都是函数对象 ， 其他的都是普通对象。</font>

```javascript
function Function(){};  
var temp = new Function();  
Function.prototype = temp; //由new Function()产生的对象都是函数对象  
```



那原型对象是用来做什么的呢？

主要作用是用于继承。

```javascript
 var Person = function(name){
 	this.name = name; // tip: 当函数执行时这个 this 指的是谁？
 };
 Person.prototype.getName = function(){
 	return this.name;  // tip: 当函数执行时这个 this 指的是谁？
 }
 var person1 = new Person('xing');
 person1.getName(); //xing
```

从这个例子可以看出，通过给 `Person.prototype` 设置了一个函数对象的属性，那有 Person 的实例（person1）出来的普通对象就继承了这个属性，而这个 this 两次都是指向了 person1



#### \_\_proto\_\_

JavaScript 在创建对象（不论是普通对象还是函数对象）的时候，都有一个叫做`__proto__` 的内置属性，用于指向创建它的构造函数的原型对象。 



>记住:
>
>​		最普通的对象: 有\_\_proto\_\_属性（指向其原型链），没有prototype属性。 
>
>​		原型对象： 有\_\_proto\_\_属性 , 还有constructor属性（指向构造函数对象）)
>
>​		函数对象： 拥有\_\_proto\_\_、prototype属性（指向原型对象 , 凡是通过new Function()创建的都是函数对象）。

对象 person1 有一个 `__proto__`属性，创建它的构造函数是 Person，构造函数的原型对象是 Person.prototype ，所以： `person1.__proto__ == Person.prototype`



贴一张《JavaScript 高级程序设计》的图:

![](/img/WX20200320-193040@2x.png)



根据上面这个连接图，我们能得到：

```javascript
Person.prototype.constructor == Person;
person1.__proto__ == Person.prototype;
person1.constructor == Person;
```

> 不过要明确一点重要信息就是，这个链接存在于实例与构造函数的原型对象之间，而不是存在于实例与构造函数之间。



#### 构造器

熟悉 JavaScript的同学都知道，创建一个对象我们可以使用字面量的方式来创建一个对象，例如: 

```javascript
let obj = {}
```

它等同于：

```javascript
let obj = new Object()
```

那么 obj 就是 Object的一个实例 ，所以:

```javascript
obj.constructor === Object

obj.__proto__ === Object.prototype
```



同理，可以创建对象的构造器不仅仅有 Object，也可以是 Array，Date，Function等。
所以我们也可以构造函数来创建 Array、 Date、Function

```javascript
var b = new Array();
b.constructor === Array;
b.__proto__ === Array.prototype;

var c = new Date(); 
c.constructor === Date;
c.__proto__ === Date.prototype;

var d = new Function();
d.constructor === Function;
d.__proto__ === Function.prototype;
```

**这些构造器都是函数对象：**

![函数对象](/img/WX20200322-153422@2x.png)



#### 原型链

- 什么是原型链呢?

```javascript
原型链的核心就是依赖对象的_proto_的指向，当自身不存在的属性时，就一层层的扒出创建对象的构造函数，直至到Object时，就没有_proto_指向了。

```

- 分析原型链（属性查找）

```javascript
因为_proto_实质找的是prototype，所以我们只要找这个链条上的构造函数的prototype。其中Object.prototype是没有_proto_属性的，它==null。

```



练习题：

```javascript
function Person(name){
  this.name = name
}

let person1 = new Person("xing")

题目一： person1.__proto__ 是什么？
题目二： Person.__proto__ 是什么？
题目三： Person.prototype.__proto__ 是什么？
题目四： Object.__proto__ 是什么？
题目五： Object.prototype.__proto__ 是什么？


```



题目一：

```javascript
person1.__proto__ === Person.prototype // 实例的__proto__ 指向 实例的构造函数的prototype ，结果为true
```



题目二：

```javascript
Person.__proto__ === Function.prototype // Person 的 __proto__ 指向 创建它的构造函数的prototype ，结果为true
```



题目三：

```javascript
Person.prototype.__proto__ === Object.prototype // 记住： 原型对象其实就是普通对象，那么它的构造函数肯定就是 Object.prototype
```



题目四：

```javascript
Object.__proto__ === Function.prototype // 跟题目二是一样的， Object 其实也是构造函数
```



题目五：

```javascript
Object.prototype.__proto__ === null // Object.prototype 对象也有proto属性，但它比较特殊，为 null 。因为 null 处于原型链的顶端，这个只能记住。

```



#### 函数对象

##### 所有*函数对象*的**proto**都指向Function.prototype，它是一个空函数（Empty function）

```javascript
Number.__proto__ === Function.prototype  // true
Number.constructor == Function //true

Boolean.__proto__ === Function.prototype // true
Boolean.constructor == Function //true

String.__proto__ === Function.prototype  // true
String.constructor == Function //true

// 所有的构造器都来自于Function.prototype，甚至包括根构造器Object及Function自身
Object.__proto__ === Function.prototype  // true
Object.constructor == Function // true

// 所有的构造器都来自于Function.prototype，甚至包括根构造器Object及Function自身
Function.__proto__ === Function.prototype // true
Function.constructor == Function //true

Array.__proto__ === Function.prototype   // true
Array.constructor == Function //true

RegExp.__proto__ === Function.prototype  // true
RegExp.constructor == Function //true

Error.__proto__ === Function.prototype   // true
Error.constructor == Function //true

Date.__proto__ === Function.prototype    // true
Date.constructor == Function //true
```



JavaScript中有内置(build-in)构造器/对象共计12个（ES5中新加了JSON），这里列举了可访问的8个构造器。剩下如Global不能直接访问，Arguments仅在函数调用时由JS引擎创建，Math，JSON是以对象形式存在的，无需new。它们的**proto**是Object.prototype。如下：

```javascript
Math.__proto__ === Object.prototype  // true
Math.construrctor == Object // true

JSON.__proto__ === Object.prototype  // true
JSON.construrctor == Object //true
```

上面说的**函数对象**当然包括自定义的。如下:

```
// 函数声明
function Person() {}
// 函数表达式
var Perosn = function() {}
console.log(Person.__proto__ === Function.prototype) // true
```



>所有的构造器都来自于 `Function.prototype`，甚至包括根构造器`Object`及`Function`自身。所有构造器都继承了Function.prototype 的属性及方法。如length、call、apply、bind



除了 `Function.prototype` 既是对象又是函数对象之外，其它的构造器的`prototype`都是一个对象:

```javascript
console.log(typeof Function.prototype) // function
console.log(typeof Object.prototype)   // object
console.log(typeof Number.prototype)   // object
console.log(typeof Boolean.prototype)  // object
console.log(typeof String.prototype)   // object
console.log(typeof Array.prototype)    // object
console.log(typeof RegExp.prototype)   // object
console.log(typeof Error.prototype)    // object
console.log(typeof Date.prototype)     // object
console.log(typeof Object.prototype)   // object
```

上面提到 `Function.prototype	`是一个空的函数 ， 你可以试着打印看看这个空函数:

`console.log(Function.prototype)`

知道了所有构造器（含内置及自定义）的`__proto__`都是`Function.prototype`，那`Function.prototype`的`__proto__`是谁呢？



相信都听说过JavaScript中函数也是一等公民，那从哪能体现呢？如下:

`console.log(Function.prototype.__proto__ === Object.prototype) // true`



这说明所有的构造器也都是一个普通 JS 对象，可以给构造器添加/删除属性等。同时它也继承了Object.prototype上的所有方法：toString、valueOf、hasOwnProperty等。



最后Object.prototype的**proto**是谁？
`Object.prototype.__proto__ === null // true`



`Object.prototype`的原型是`null`，`null`指空对象。世界上最难的事情是从0生出1，无中生出有，`Object.prototype`就是照着`null`的样子创造了1。



#### prototype

>在 ECMAScript 核心所定义的全部属性中，最耐人寻味的就要数 `prototype` 属性了。对于 ECMAScript 中的引用类型而言，`prototype` 是保存着它们所有实例方法的真正所在。换句话所说，诸如 `toString()`和 `valuseOf()` 等方法实际上都保存在 `prototype` 名下，只不过是通过各自对象的实例访问罢了
>
>--- 《JavaScript 高级程序设计》第三版 P116



我们知道 JS 内置了一些方法供我们使用，比如：

```
对象可以用 constructor/toString()/valueOf() 等方法;
数组可以用 map()/filter()/reducer() 等方法；
数字可以用 parseInt()/parseFloat()等方法;
```



当我们创建一个对象时:

```javascript
let Person = new Object()


// Person 是 Object 的实例，所以 Person 继承了Object 的原型对象Object.prototype上所有的方法：

console.dir(Object.prototype) // Object.prototype 其实就是一个普通对象
/**
  Object
  constructor: ƒ Object()
  __defineGetter__: ƒ __defineGetter__()
  __defineSetter__: ƒ __defineSetter__()
  hasOwnProperty: ƒ hasOwnProperty()
  __lookupGetter__: ƒ __lookupGetter__()
  __lookupSetter__: ƒ __lookupSetter__()
  isPrototypeOf: ƒ isPrototypeOf()
  propertyIsEnumerable: ƒ propertyIsEnumerable()
  toString: ƒ toString()
  valueOf: ƒ valueOf()
  toLocaleString: ƒ toLocaleString()
  get __proto__: ƒ __proto__()
  set __proto__: ƒ __proto__()
*/
```



**Object 的每个实例都具有以上的属性和方法。**
所以我可以用 `Person.constructor` 也可以用 `Person.hasOwnProperty`。



当我们创建一个数组时：

```javascript
let num = new Array()
// num 是 Array 的实例，所以 num 继承了Array 的原型对象Array.prototype上所有的方法：

console.log(Array.prototype)
/**
	[constructor: ƒ, concat: ƒ, copyWithin: ƒ, fill: ƒ, find: ƒ, …]
  length: 0
  constructor: ƒ Array()
  concat: ƒ concat()
  copyWithin: ƒ copyWithin()
  fill: ƒ fill()
  find: ƒ find()
  findIndex: ƒ findIndex()
  lastIndexOf: ƒ lastIndexOf()
  pop: ƒ pop()
  push: ƒ push()
  reverse: ƒ reverse()
  shift: ƒ shift()
  unshift: ƒ unshift()
  slice: ƒ slice()
  sort: ƒ sort()
  splice: ƒ splice()
  includes: ƒ includes()
  indexOf: ƒ indexOf()
  join: ƒ join()
  keys: ƒ keys()
  entries: ƒ entries()
  values: ƒ values()
  forEach: ƒ forEach()
  filter: ƒ filter()
  flat: ƒ flat()
  flatMap: ƒ flatMap()
  map: ƒ map()
  every: ƒ every()
  some: ƒ some()
  reduce: ƒ reduce()
  reduceRight: ƒ reduceRight()
  toLocaleString: ƒ toLocaleString()
  toString: ƒ toString()
  Symbol(Symbol.iterator): ƒ values()
  Symbol(Symbol.unscopables): {copyWithin: true, entries: true, fill: true, find: true, findIndex: true, …}
  __proto__: Object
	
*/
```

这样你就明白了随便声明一个数组，它为啥能用那么多方法了。

并且它的原型链（\_\_proto\_\_）中还有对象的方法。



当我们创建一个函数时：

```javascript
var f = new Function("xing","return xing;"); //当然你也可以这么创建 f = function(x){ return x }
console.log(f.arguments) // arguments 方法从哪里来的？
console.log(f.call(window)) // call 方法从哪里来的？
console.log(Function.prototype) // function() {} （一个空的函数）
console.dir(Function.prototype); 
```

疑问来了，我们之前说过:

>所有**函数对象**proto都指向 `Function.prototype`，它是一个空函数（Empty function）



嗯！ 我们确实证明了它就是一个空函数，但是你也别忘了我们之前说过的:

```javascript
所有对象的 __proto__ 都指向其构造器的 prototype
```



我们下面再复习下这句话。

```javascript
// 先看看 JS 内置构造器：

var obj = {name: 'xing'}
var arr = [1,2,3]
var reg = /hello/g
var date = new Date
var err = new Error('err')
 
console.log(obj.__proto__ === Object.prototype) // true
console.log(arr.__proto__ === Array.prototype)  // true
console.log(reg.__proto__ === RegExp.prototype) // true
console.log(date.__proto__ === Date.prototype)  // true
console.log(err.__proto__ === Error.prototype)  // true
```

再看看自定义的构造器，这里定义了一个 `Person`：

```javascript
function Person(name) {
  this.name = name;
}
var p = new Person('xing')
console.log(p.__proto__ === Person.prototype) // true
```

`p` 是 `Person` 的实例对象，`p` 的内部原型总是指向其构造器 `Person` 的原型对象 `prototype`。

每个对象都有一个 `constructor` 属性，可以获取它的构造器，因此以下打印结果也是恒等的：

```javascript
function Person(name) {
    this.name = name
}
var p = new Person('xing')
console.log(p.__proto__ === p.constructor.prototype) // true
```

上面的`Person`没有给其原型添加属性或方法，这里给其原型添加一个`getName`方法：

```javascript
function Person(name) {
    this.name = name
}
// 修改原型
Person.prototype.getName = function() {}
var p = new Person('xing')
console.log(p.__proto__ === Person.prototype) // true
console.log(p.__proto__ === p.constructor.prototype) // true
```

可以看到`p.__proto__`与`Person.prototype`，`p.constructor.prototype`都是恒等的，即都指向同一个对象。



注意：如果换一种方式设置原型，结果就有些不同了。

>```javascript
>function Person(name) {
>    this.name = name
>}
>// 重写原型
>Person.prototype = {
>    getName: function() {}
>}
>var p = new Person('xing')
>console.log(p.__proto__ === Person.prototype) // true
>console.log(p.__proto__ === p.constructor.prototype) // false
>```



这里直接重写了 `Person.prototype`（注意：上一个示例是修改原型）。输出结果可以看出`p.__proto__`仍然指向的是`Person.prototype`，而不是`p.constructor.prototype`。

这也很好理解，给`Person.prototype`赋值的是一个对象直接量`{getName: function(){}}`，使用对象直接量方式定义的对象其构造器（`constructor`）指向的是根构造器`Object`，`Object.prototype`是一个空对象`{}`，`{}`自然与`{getName: function(){}}`不等。如下：

```javascript
var p = {}
console.log(Object.prototype) // 为一个空的对象{}
console.log(p.constructor === Object) // 对象直接量方式定义的对象其constructor为Object
console.log(p.constructor.prototype === Object.prototype) // 为true
```



#### 复习原型链

```javascript
function Person(){}
var person1 = new Person();
console.log(person1.__proto__ === Person.prototype); // true
console.log(Person.prototype.__proto__ === Object.prototype) //true
console.log(Object.prototype.__proto__) //null

Person.__proto__ == Function.prototype; //true
console.log(Function.prototype)// function(){} (空函数)

var num = new Array()
console.log(num.__proto__ == Array.prototype) // true
console.log( Array.prototype.__proto__ == Object.prototype) // true
console.log(Array.prototype) 
console.log(Object.prototype.__proto__) //null

console.log(Array.__proto__ == Function.prototype)// true
```



疑问一：

```
Object.__proto__ === Function.prototype // true

// Object 是函数对象，是通过new Function()创建的，所以Object.__proto__指向Function.prototype。(所有函数对象的__proto__都指向Function.prototype)
```



疑问点二：

```javascript
Function.__proto__ === Function.prototype // true

// Function 也是对象函数，也是通过new Function()创建，所以Function.__proto__指向Function.prototype。
```

>自己是由自己创建的，好像不符合逻辑，但仔细想想，现实世界也有些类似，你是怎么来的，你妈生的，你妈怎么来的，你姥姥生的，……类人猿进化来的，那类人猿从哪来，一直追溯下去……，就是无，（NULL生万物）
> 正如《道德经》里所说“无，名天地之始”。



疑问点三：

```javascript
Function.prototype.__proto__ === Object.prototype //true

// Function.prototype是个函数对象，理论上他的__proto__应该指向 Function.prototype，就是他自己，自己指向自己，没有意义。这里 鸡生蛋还是蛋生鸡 ? Object.prototype 可以看作是对象的祖先 ， Object 是有 Function （包括它自己）创建出来的，但是 Function 作为一个对象 又可以看作是由Object创建出来的。所以 Object 就当成了祖宗 ， 原型链也就有了终点。
```



# 总结：

- 原型和原型链是JS实现继承的一种模型。
- 原型链的形成靠的是`__proto__` 而非`prototype`，正如我们之前说的那句话，实例和 原型对象 存在一个连接；不过要明确一点重要信息就是，这个链接存在于实例与构造函数的原型对象之间，而不是存在于实例与构造函数之间。

![](/img/js.png)

最后献上一张原型链图，相信阅读完后的你看这张图会有一定的清晰度了.


