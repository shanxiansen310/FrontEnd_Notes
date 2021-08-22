[toc]



首先理解一下bind

# Function.prototype.bind



**`bind()`** 方法创建一个新的函数, 当被调用时，将其 `this` 关键字设置为提供的值，在调用新函数时，在任何提供之前提供一个给定的参数序列。
▼语法：
fun.bind(thisArg[, arg1[, arg2[, ...]]])
▼参数：
`thisArg`：当绑定函数被调用时，该参数会作为原函数运行时的 this 指向。当使用 new 调用绑定函数时，该参数无效。如果 `bind` 函数的参数列表为空，或者`thisArg`是`null`或`undefined`，执行作用域的 `this` 将被视为新函数的 `thisArg`

`arg1, arg2, ...`：当绑定函数被调用时，这些参数将置于实参之前传递给被绑定的方法。
▼返回值：
返回由指定的 `this` 值和初始化参数改造的原函数拷贝

`▼bind` 函数有哪些功能：

1. **改变原函数的 `this` 指向，即绑定 `this`**

2. **返回原函数的拷贝**

3. **注意，还有一点，当 `new` 调用绑定函数的时候，`thisArg` 参数无效。也就是 `new` 操作符修改 `this` 指向的优先级更高**



举例:

```js
function foo() {
    this.b=1
    return this.a;
}
let obj = { a: 2 };
let bound=foo.bind(obj);
console.log(bound());     // 2
console.log(new bound()); //foo {b: 1}
```



## 简单bind实现

```js
/*简易版bind*/
Function.prototype.easyBind=function (otherThis) {
	if (typeof this!=="function") console.log("This is not callable!");

	let fToBind=this,   //fToBind表示要进行绑定的函数
		args=Array.prototype.slice.call(arguments,1);  //这里的args是bind中的参数
	return function () {
		//在otherThis上调用fToBind函数
		return fToBind.apply(otherThis,
			args.concat(Array.prototype.slice.call(arguments)));  //这里的参数包括了bind中的和调用bound时传入的
	}
}

function foo() {
	this.b=1;
	return this.a;
}
let obj = { a: 2 };
let bound=foo.easyBind(obj);
console.log(bound());        // 2
```

其实我们现在已经实现了bind函数中 1.`绑定函数的this指向`和 2.`返回改造的原函数拷贝` 两个功能了!!!  BUT

`this` 绑定有 4 种绑定规则：

- 默认绑定
- 隐式绑定
- 显式绑定
- new 绑定

四种绑定规则的优先级从上到下，依次递增，默认绑定优先级最低，`new` 绑定最高。但现在使用new会出现问题

```js
function foo() {
    this.b=1
    console.log(this);
    console.log(this.a);
}
let obj = { a: 2 };
let bound=foo.easyBind(obj);
console.log(bound());      //依次是 Object{a:2,b:1}  2   undefined
console.log(new bound());  //依次是 Object{a:2,b:1}  2   Object{}


//上面那个例子没看明白可以看这个加深理解
function foo(name) {
    this.name = name;
}
let obj = {};
let bar = foo.myBind(obj);
bar('Jack');
console.log(obj.name);  // Jack
var alice = new bar('Alice');
console.log(obj.name);  // Alice
console.log(alice.name);    // undefined
```

💎正常情况下, new优先级最高, new bound() 会将bound执行时的this指向bound本身, 那么this就应该不再具有属性a, 应该是Object{b:1}。这是由于我们使用的是easyBind绑定，并没有考虑new的情况。

★ new Bound() 为什么会返回Object{}？ 很简单，这和new的原理有关！

🌟注意，执行easyBind返回后函数就可以直接理解为 执行this指向为obj的foo函数！！！



## 实现new的Bind



🚩`new` 操作符在调用构造函数的时候，会进行一个什么样的过程：

1. 创建一个全新的对象
2. 这个对象被执行 `[[Prototype]]` 连接
3. 将这个对象绑定到构造函数中的 `this`
4. 如果函数没有返回其他对象，则 `new` 操作符调用的函数则会返回这个对象

这可以看出，在 `new` 执行过程中的第三步，会对函数调用的 `this` 进行修改。在我们简易版的 `bind` 函数里，原函数调用中的 `this` 永远执行指定的对象，而不能根据如果是 `new` 调用而绑定到 `new` 创建的对象。所以，我们要<span style='color:red; font-weight:bold;'> 对原函数的调用进行判断 </span>，是否是 `new` 调用。



```js
/*改进后的bind*/
Function.prototype.goodBind = function (otherThis) {
	if (typeof this !== "function") console.log("This is not callable!");

	let fToBind = this,
		args = Array.prototype.slice.call(arguments, 1),  //
		fBound = function () {
			/*检测是否由new创建*/
			return fToBind.apply(fToBind.prototype.isPrototypeOf(this)? this:otherThis
				, args.concat(Array.prototype.slice.call(arguments)));
			//在MDN上采用的是记录args长度push的方法, 但我觉得concat更简洁而且不会改变原数组
			//fToBind.prototype.isPrototypeOf(this) <==> this instanceof fToBind
		};

	if (this.prototype){ /*判断this/foo有没有原型, 比如说内置函数Math.push没有原型*/
		fBound.prototype=this.prototype;
	}
	return fBound;
}
function foo(name) {
	this.name = name;
}
let obj = {};
let bar = foo.goodBind(obj);
bar('Jack');
console.log(obj.name);        // Jack
let alice = new bar('Alice');
console.log(obj.name);        // Jack
console.log(alice.name);      // Alice
```



😍这时我们已经实现了原生bind的三个功能

- 首先，变量 `bar` 是绑定之后的函数，也就是 `fBound`。`self` 是原函数 `foo` 的引用。
- 对于 `fBound` 函数中的 `this` 的指向，**如果是 `bar('Jack')` 这样直接调用，`this` 指向全局变量或者 `undefined` （视是否在严格模式下）**，不过这里绑定了的话就是this就是指向obj！。但是如果是 `new bar('Alice')` ，根据上面给出的 `new` 执行过程，我们知道，`fBound` 函数中的 `this` 会指向 `new` 表达式返回的对象，即 `alice`。

💎分析下  <span style="color:blue;">fToBind.prototype.isPrototypeOf(this) ? this:otherThis</span>

如果是 new 的话会存在一个环节 let obj=Object.create(fn.prototype) 这样this的prototype就是fToBind了, 但是如果没有 new 的话 这里的this 就是 global 对象( 不懂的话单独看foo函数就知道为啥是global了)。因此有new的话this不会是global 而是foo因此 apply中的thisArg就是this， 如果不是new的话就采用bind绑定时的otherThis！



💎分析下   <span style="color:blue;">fBound.prototype=this.prototype; </span>      **(this: foo)**

​		▼如果没有这行代码

- 如果是直接调用 `bar('Jack')`，`this instanceof self ? this : oThis` 这句判断，根据上述变量分析，所以此判断为 `false`，绑定函数的 `this` 指向 `oThis`，也即是指定的 `this` 对象。
- 如果是 `new` 调用绑定函数，此时绑定函数中的 `this` 是由 `new` 调用绑定函数返回的实例对象，这个对象的构造函数是 `fBound`，**当我们忽略掉原型连接那行代码时，其原型对象并不等于原函数 `self` 的原型**，所以 `this instanceof self ? this : oThis` 得到的值还是指定的对象，而不是 `new` 返回的对象。(我们返回的是自己新建的fBound函数，但并没有给这个新函数添加prototype，其prototype直接指向最高层的Object)
- 所以，知道为什么要在绑定的时候，绑定函数要与原函数进行原型连接了吧？**每次绑定的时候，将绑定函数 `fBound` 的原型指向原函数的原型，如果 `new` 调用绑定函数，得到的实例的原型，也是原函数的原型。**这样在 `new` 执行过程中，执行绑定函数的时候对 `this` 的判断就可以判断出是否是 `new` 操作符调用



```js
foo.prototype.sex="female";
foo.prototype.name="Jane";
console.log(alice.name);  // Alice
console.log(alice.sex);   // female
//这样修改了foo, 由于是同一个原型链肯定也会相应的使其新实例受影响。 
//如果采用的是继承的方法，也会顺着原型链向上寻找属性

bar.prototype.key="ni";
console.log(foo.prototype.key);  //ni
//但是这样的话由于fBound.prototype=this.prototype, 子类反而会影响到父类,这是不合常理的!!!
```

💣但这样还有一个问题, 当绑定函数直接连接原函数的原型的时候，如果 `fBound` 的原型有修改时，是不是原函数的原型也会受到影响了？

比如：

```js
function SuperType() {
	this.property = true;
}
SuperType.prototype.getSuperValue = function() {
	return this.property;
};
function SubType() {
	this.subproperty = false;
}
```

💎继承也是如此，如果我们直接SubType.prototype=Supertype.prototype, 这两个指针会指向同一个对象。当我们在SubType.prototype做修改时SuperType.prototype也会改变

💎但是如果SubType.prototype=new SuperType()，这样SubType.prototype只是指向了Supertype的一个实例，对**SubType.prototype**进行修改影响的是实例而不是**SuperType.prototype**

🚩这常常用来做原型维护！

所以，为了解决这个问题，我们需要一个空函数，作为中间人。





## 最优的方法



```js
/*最优的bind*/
Function.prototype.bestBind = function (otherThis) {
	if (typeof this !== "function") console.log("This is not callable!");

	let fToBind = this,
		args = [...arguments].slice(1),
		fProto = function () {},
		fBound = function () {
			/*检测是否由new创建*/
			// console.log(this.__proto__);
			return fToBind.apply(fProto.prototype.isPrototypeOf(this) ? this : otherThis
				, args.concat([...arguments]));
			//在MDN上采用的是记录args长度push的方法, 但我觉得concat更简洁而且不会改变原数组
			//fProto.prototype.isPrototypeOf(this) <==> this instanceof fProto
		};

	//维护原型关系
	if (this.prototype) {
		fProto.prototype = this.prototype;
	}
	fBound.prototype = new fProto();   //new this可能会遇到某些函数没有prototype的情况
	return fBound;
}
```

<span style="font-size:20px;font-weight:bold">Step: </span>

1. 判断调用bind的对象是否是一个函数
2. 把bind除了thisArg的参数用一个数组表示, 并创建一个空的函数使其原型指向this
3. 判断是否是new创建的函数, 然后在对应的对象上执行fToBind函数并得到新函数fBound
4. 新函数继承fProto后返回

```js
function foo(name) {
    this.name = name;
    console.log(this)
}
var obj = {};
var bar = foo.bestBind(obj);
bar('Jack');
console.log(obj.name);        // Jack
var alice = new bar('Alice');
console.log(obj.name);        // Jack
console.log(alice.name);      // Alice
```



★解释一下Array.prototype.slice.call(arguments, 1)
	👉因为arguments是一个类数组对象, 不能直接使用silce方法, 但Array.prototype.slice可以对类数组对象进行转换为数组在调用。（当然这里也可以以通过扩展运算符或Array.from来转换）①[...arguments] ②Array.from(arguments)

 🚩fToBind.apply(fProto.prototype.isPrototypeOf(**this**)) 这里的this不要写成fBound, fBound是执行bind操作后返回的绑定了otherThis的函数, 而这里是在判断fBound调用时的执行环境。**如果普通的调用 bar() ,那么执行环境中的this是顶层对象；但如果是通过new来调用，那么执行环境中的this会指向一个空对象，这个空对象的原型（\_\_proto\_\_）指向构造函数的prototype，也就是在这里的bar/fBound的prototype属性, ** 那么fProto.prototype.isPrototypeOf(**this**) 就为true!!   （也就是说在new的时候 obj=Object.create(fBound.prototype)，可以把空对象理解为是fBound❗ ） 

可以联系new操作符的步骤： 在第二步 obj.prototype=func.prototype, 继而在第三步 func.apply(obj), 在这里就是 obj.prototype=fBound.prototype, 然后调用fBound.apply(obj)   

总结就是： 不能写成fBound， 因为普通调用时this为global，new时this为一个\_\_proto\_\_为foo{}的fBound

❗但是也要注意 fProto.prototype.isPrototypeOf(**fBound**)  这个是肯定为假的!! 因为fBound只是构造函数, 而这个方法判断的是**实例与原型之间的关系**!!!  判断在实例的原型链上是否存在fPoto.prototype, 构造函数与其无关, fProto.prototype.isPrototypeOf(**fBound.prototype**) 倒是结果为真!!!因为fBound.prototype是fProto的实例()

🌟 fBound.prototype.__proto\_\_  = fProto.prototype，根据继承fBound.prototype = new fProto();可知！ 而在new的时候相当于new fBound()， 返回的对象也就是这里的 this  this.\_\_proto\_\_ = fBound.prototype，因此这里的原型链就是  <span style="font-weight:bold; color:red;">this => fBound.prototype => fProto</span>  因此判断this和fBound.prototype才能在fProto.prototype.isPrototypeOf被判断为true，fBound并不在原型链中为false！

🌟因此在这里判断new 的时候其实是一条原型链  

​    <span style="font-weight:bold; color:red;">实例alice ==> fBound.portotype ==>fProto.prototye</span>

所以这里写 **fBound**.prototype.isPrototypeOf(this) 也是一样的!!!



参考: 

[手写bind()函数，理解MDN上的标准Polyfill_u010552788的专栏-CSDN博客](https://blog.csdn.net/u010552788/article/details/50850453)

[bind 函数的实现原理 - 知乎 (zhihu.com)](https://zhuanlan.zhihu.com/p/38968174)















