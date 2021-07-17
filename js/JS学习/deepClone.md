## 假如易立竞要你手写深拷贝？

[toc]





深拷贝相信大家十分熟悉，一般图省事会有人直接写 JSON.parse(JSON.stringify())。但大家肯定知道这样是十分简陋的，适用于大部分情况的数据，但是缺陷也很明显。也有很多人直接调用lodash的cloneDeep方法，不过这也有点小问题🚨🚨🚨

接下来我会通过提问的方式一步步地引出一个比较完美的深拷贝，如果想直接看最终结果可以直接滑下去！



 <img src="C:\Users\shanxiansen310\Pictures\emoji\20210617125535.png" alt="易立竞 的图像结果" style="zoom:127%;" />



### 乞丐版

▼易老师：你能写一个深拷贝吗？

当然，这不是有手就行？ 一行代码足以解决！

```js
const cloned = JSON.parse(JSON.stringify(objectToClone));
```



▼易老师：你能够不用JSON这种方式吗？



内心os： 啊！ 我只记住了这种方法啊！ 怎么办！ 

虽然你内心十分着急，但还是在易老师的死亡凝视下快速写出了下面的这段代码：

```js
const deepClone=target=> {
	//非对象类型直接返回
	if (typeof target!=='object') return target;
	let cloneTarget=Array.isArray(target)? [] : {};
	for (let key in target){
		//这里每次通过递归得到属性,省去判断是否为对象的代码
		cloneTarget[key]=deepClone(target[key]);
	}
	return cloneTarget;
}
```

再进行测试一下:

```js
//测试代码
const target = {
	field1: 1,
	field2: undefined,
	field3: 'ConardLi',
	field4: {
		child: 'child',
		child2: {
			child2: 'child2'
		}
	}
};

let copy1=deepClone(target);
console.log(copy);
console.log(copy1.field4.child2===target1.field4.child2);
```

测试结果:

```js
//output
{
  field1: 1,
  field2: undefined,
  field3: 'ConardLi',
  field4: { child: 'child', child2: { child2: 'child2' } }
}
false
```

完全正确, 狂喜ing😆😆😆

内心os：这下我通过

1. 递归的形式解决了对象中的对象的拷贝问题

2. Arrya.isArray() 并且区分了array和object类型3.
3. 而且基本类型的话我不用复制直接返回

易老师你没话讲了吧！😁😁😁



### 基础版

▼易老师: 哦, 你是不是会觉得你这么快写出个递归的方法很自豪? 你考虑到了数组和对象的区别, 但你有考虑到for in方法是会把对象原型链上所有的键值都考虑吗? 你有考虑到for in并不能遍历Symbol为键的属性吗? 



内心os:  天哪, 易老师压迫感也太强了! 我以为写的很棒了, 居然还有那么多没有考虑到! 不慌不慌, 我慢慢改!

在偷偷翻阅MDN后你发现了一种神奇的api   

**Reflect.ownKeys` 方法返回一个由目标对象自身的属性键组成的数组。它的返回值等同于`Object.getOwnPropertyNames(target).concat(Object.getOwnPropertySymbols(target))。**

天哪, 这个方法不仅可以解决将原型链上的属性多拷贝了的问题, 还可以解决Symbol的问题, 真的是一举两得! 于是你赶紧修改了之后给易老师看! 这次, 你易立竞总算把持不住了把!!!😆😆😆

 <img src="https://image-list-1258374833.cos.ap-chengdu.myqcloud.com/rush.gif" alt="rush" style="zoom:67%;" />

```js
const deepClone=target=> {
	//非对象类型直接返回
	if (typeof target!=='object') return target;
	let cloneTarget=Array.isArray(target)? [] : {};
	//for in target会包括原型链上继承的属性,一般不考虑
	for (let key of Reflect.ownKeys(target)){
		//这里每次通过递归得到属性,省去判断是否为对象的代码
		cloneTarget[key]=deepClone(target[key]);
	}
	return cloneTarget;
}
```

测试symbol:

```js
let sym=Symbol('deepClone')
const target = {
	field1: 1,
	field2: undefined,
	field3: 'ConardLi',
	field4: {
		child: 'child',
		child2: {
			child2: 'child2'
		}
	},
	sym:'symbol'
};
const copy=deepClone(target);
console.log(copy);
console.log(copy.field4.child2===target.field4.child2);
```

测试结果:  symbol完全拷贝了!

```js
{
  field1: 1,
  field2: undefined,
  field3: 'ConardLi',
  field4: { child: 'child', child2: { child2: 'child2' } },
  sym: 'symbol'
}
false
```

🚩

1. 解决了for in拷贝了原型链上的属性的问题
2. 解决了symbol无法复制的问题



▼易老师: 哦,  你平时也是这么自信的吗? 我只指出了两个问题, 你就真的觉得只有两个问题吗? 既然你会使用ES6基本类型中的新属性, 为什么你没有考虑到引用类型中的Date和RegExp呢? 你的方法有解决循环引用的问题吗,比如target.copy=target不会报错吗? 

这里我们尝试添加 target.target=target;  , 会发现报错❗  RangeError: Maximum call stack size exceeded, 这就是因为没有解决循环引用!

内心os: OMG, 蚌埠住了! 谁来救救我! 

 ![蚌埠](https://gitee.com/shanxiansen310/picrepo/raw/master/img/sad1.gif)

不过不慌, 这两个问题解决起来也不难! 我们可以利用Date和RegExp中的构造函数来拷贝, 至于循环引用可以建立一张hash表来解决!





### 进阶版

```js
const isComplexDataType = obj => (obj instanceof Object && typeof obj !== 'function')
const deepClone = (target, map = new Map()) => {
	if (!isComplexDataType(target)) {  //基础类型
		return target;
	} else {
		//map记录将对象作为key,记录该对象是否在之前出现过
		if (map.has(target)) {
			return map.get(target);
		}
		//对于Date,RegExp的处理很棒!!!
		let constructor = target.constructor;
		if (/^(RegExp|Date)$/i.test(constructor.name)) {
			return new constructor(target);
		}
		//这里就是object或数组了
		const cloneTarget = Array.isArray(target) ? [] : {};
		map.set(target, cloneTarget);
		for (let key of Reflect.ownKeys(target)) {
			cloneTarget[key] = deepClone(target[key],map);
		}
		return cloneTarget;
	}
}
```

* 考虑了内置对象比如 Date、RegExp 等对象
* 解决了循环引用的问题。(这里的map中key是target, value是对应的cloneTarget, 如果当前的target以前就创建过则直接返回cloneTarget)
* isComplexDataType 判断是否为对象类型, 我们这里不拷贝函数, 因为函数一般很少有需要拷贝的场景, 因此函数的话也相当于基本类型直接返回







### 终极版

▼易老师: 这么久写出来一个稍微能用的了, 你平时有深入了解过js语法吗?  这里能想到用map, 既然键都是对象为什么不适用weakMap呢?  你知道对象也有descriptor属性吗? 你这样拷贝下来的对象并没有保留descriptor属性, 如果我原来设置的configurable: false, 可是到你这却变成了configurable:true , 你不担心会发生严重的安全隐患吗?  还有考虑了RegExp和Date, 还有set和map呢? 



 <img src="https://image-list-1258374833.cos.ap-chengdu.myqcloud.com/akjsbc.gif" alt="bully" style="zoom:50%;" />

内心os: 易老师我错了, 我终于知道自己有多菜了!!!  我慢慢改吧😭😭😭

```js
const isComplexDataType = obj => (obj instanceof Object && typeof obj !== 'function')
const deepClone = function(obj, hash = new WeakMap()) {
	if (!isComplexDataType(obj)){  //基础类型,包括null和undefined
		return obj;
	}else {
    //防止循环引用
		if (hash.has(obj)) return hash.get(obj)
    
		let constructor=obj.constructor;
		//这里是为了得到所有的数据描述符的值,[Configurable],[Enumerable],[Writable],[Value]等
		const allDesc = Object.getOwnPropertyDescriptors(obj);
		let cloneObj;

		//Date和RegExp
		if (/^(Date|RegExp)$/i.test(constructor.name)){
			cloneObj=new constructor(obj);
			Object.defineProperties(cloneObj,allDesc);
			return cloneObj;
		}
		//WeakMap和WeakSet不支持遍历, 没法copy
		//Map
		if (constructor.name==='Map'){
			cloneObj=new constructor();
			Object.defineProperties(cloneObj,allDesc);
			obj.forEach((value,key)=>{
				cloneObj.set(key,deepClone(value,hash));
			})
			return cloneObj;
		}

		//Set
		if (constructor.name==='Set'){
			cloneObj=new constructor();
			Object.defineProperties(cloneObj,allDesc);
			obj.forEach((value)=>{
				cloneObj.add(deepClone(value,hash));
			})
			return cloneObj;
		}
		
    //这里就是简单的对象Object or Array
    
		//Object.create原本是创建一个新对象,并将第一个参数作为新对象的__proto__
		//结合Descriptors方法后可以用第二个参数指定数据描述符
		//这样一来也考虑了数组
		cloneObj = Object.create(Object.getPrototypeOf(obj), allDesc)
		hash.set(obj, cloneObj)

		for (let key of Reflect.ownKeys(obj)) {
			cloneObj[key] = deepClone(obj[key], hash);
		}
		return cloneObj
	}
}
```

🚩

1. 采用了WeakMap来存储已经遍历过的对象, 防止循环引用和内存泄漏
2. Object.getOwnPropertyDescriptors(obj)得到 obj的[Configurable],[Enumerable],[Writable],[Value] 等属性。然后结合Object.create()将这些属性赋值给cloneObj
3. 之前还需要进行数组和对象的区分, 这里直接采用 Object.getPrototypeOf(obj) 就不用区分!!!  (这种方式建立的数组在node显示为 Array{key:value} , 但实际上就是数组, 在v8中显示正常!)
4. 增加了map和set的考虑, WeakMap和WeakSet不支持遍历, 没法copy,



测试代码:

```js
const items = new Map([
	["key1", "val1"],
	["key2", "val2"],
	["key3", "val3"]
]);

let setTest = new Set([1, 2, 3,items]);
let s=Symbol(123);
const target = {
	field1: new Date(),
	field2: function (n) {
		console.log(n);
	},
	field3: 'ConardLi',
	field4: {
		child: 'child',
		child2: {
			child2: [1,'child2']
		}
	},
	[s]:'symbol',
	map:items,
	set:setTest
};
target.target=target;
let copy=deepClone(target);
console.log(copy);  
console.log(target.set.has(items))   //true
console.log(copy.set.has(items))     //false
console.log(copy.field4.child2 === target.field4.child2);           //false
console.log(copy.field2 === target.field2);                         //true
console.log(Reflect.ownKeys(target)[4]===Reflect.ownKeys(copy)[4])  //true
console.log(copy.target===copy);                                    //true
```

可以看见满足了大多数场景!



▼易老师: 你现在的方法看起来也许挺好，可是你一直在自己思考，你有考虑过去借鉴开源社区的代码吗？你为什么没有想到站在巨人的肩膀上看待事物呢？



内心os：没想到写的这么好了易老师还能从其他角度拷问，太恐怖了！😭😭😭 不过我发现了一个js常用的且有几万star的库--lodash，里面也有深拷贝，让我们来康康！



### 对比lodash

▼当然这并不是最完美的, 详情可以看很多人用的lodash库

[lodash/baseClone.js at master · lodash/lodash (github.com)](https://github.com/lodash/lodash/blob/master/.internal/baseClone.js)

❗不过loadash虽然比较全面(), 但是并没有考虑到 descriptor 属性哦! (这个库几年没更新了...)

我们继续采用上面的target来测试!

```js
Object.getOwnPropertyDescriptor(target, 'field3').configurable;  //true
Object.defineProperty(target,'field3',{
  configurable:false
})
Object.getOwnPropertyDescriptor(target, 'field3').configurable;  //false

target.target=target;
let copy=deepClone(target);
Object.getOwnPropertyDescriptor(copy, 'field3').configurable;    //false

const _=require('lodash');
const res = _.cloneDeep(target);
Object.getOwnPropertyDescriptor(res, 'field3').configurable;     //true
```

可以看见lodash并不会clone原来对象上的descriptors! 但是lodash库比较全面吧, 对于很多类型(Bigint, int8等)都有考虑到! 



总的来说一般场景直接调用lodash的方法基本可以解决了, 但是我们如果了解一些深拷贝的原理, 我们便可以自定义的写出很多深拷贝的方法从而应对不同的场景! 😁😁😁



▼易老师：今天的采访到此结束，希望你能对深拷贝有新的认识！

我：谢谢易老师！😅😅😅  

内心os：这恐怖的压迫感再也不想有了！😭😭😭

![sad2](deepClone.assets\sad2.png)

