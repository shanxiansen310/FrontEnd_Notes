[toc]



**promise用法其实我搞懂了, 但一直不明白为什么要产生promise, 而且不太懂为什么promise比嵌套回调函数更好。所以这里主要想探讨一下promise和回调函数的区别, 也想帮助自己加深对回调函数的理解**



首先抛一个结果

## ▼ 为什么要使用Promise?

🌟

1. 指定回调函数的方式更加灵活：

旧的：必须在启动异步任务前指定

promise：启动异步任务 => 返回promie对象 => 给promise对象绑定回调函数(甚至可以在异步任务结束后指定)

 

2. 支持链式调用，**可以解决回调地狱问题**

什么是回调地狱？回调函数嵌套调用，外部回调函数异步执行的结果是嵌套的回调函数执行的条件

回调地狱的缺点？ 不便于阅读 ／ 不便于异常处理

参考: [关于js的callback回调函数以及嵌套回调函数的执行过程理解](https://blog.csdn.net/samt007/article/details/54647361)





## ▼为什么需要嵌套回调函数？



**回调函数：简单理解就是我提供一个信息，回调函数会根据我提供的信息进行对应的操作**

但是嵌套回调函数的使用场景是什么呢？ 我查了很多资料都有写， 但都写的是伪代码，所以我参考了一下写了可以运行的代码来加深理解  

e.g.

我们需要返回一个data数据，而这个data数据由一个回调函数提供

```js
function receive(){
    let myData;
    setTimeout(function (data){
        myData=data;
    },0,"data");
    return myData;
}

console.log(receive());  //undefined
```

由于异步机制, 我们始终无法得到返回的myData。因为return是本轮事件中执行，而setTimeout则会等到下一轮



但是如果改用嵌套回调函数

```js
function receive(callback){
    let myData;
    setTimeout(function (data){
        callback(data);
        console.log(data);
        myData=data;
    },0,"data");
    return myData;
}
receive(function (data) {
    //对返回的data进行相关操作
    console.log(data);  //data
})

```

此时我们便能正确的的到data并对data进行相应的操作!

❗注意此时receive函数返回的myData依旧是undefined



## ▼为什么有了嵌套回调函数还需要Promise?

参考的例子：[图解 Promise 实现原理（一）](https://zhuanlan.zhihu.com/p/58428287) （我加上了一些代码使其可以运行加深理解）



e.g.

这里我们通过url获取id, 然后又要通过id获取name, 最后再通过name获得course

```js
function getId(url,callback){
    //doSomething
    let id=url+"-->id";
    callback(id);
}
function getNameById(id,callback){
    //doSomething
    let name=id+"-->name";
    callback(name);
}
function getCourseByName(name,callback){
    //doSomething
    let course=name+"-->course";
    callback(name);
}

getId("url",function (id) {
    //getId在调用回调函数前已经得到了id
    //现在是回调函数对id进行相关操作
    console.log(id);
    getNameById(id,function (name) {
        //回调函数对name进行相关操作
        console.log(name);
        getCourseByName(name,function (course) {
            //回调函数对course进行相关操作
            console.log(course);
        })
    })
})

/*
▼打印结果:
url-->id
url-->id-->name
url-->id-->name
*/
```

可以看出来, 嵌套回调函数十分繁琐 ! 

但promise能让我们写出干净的代码且避免 call-back hell 回调地狱

用promise改写后的代码

```js
function getId(url) {
    return new Promise(function (resolve) {
        let id=url+"-->id";
        console.log(id)
        resolve(id);
    })
}
function getNameById(id){
    //doSomething
    let name=id+"-->name";
    console.log(name);
    return  name;

}
function getCourseByName(name){
    //doSomething
    let course=name+"-->course";
    console.log(course);
    return  course;
}

//then返回的promise会把返回的name作为resolve参数传递下去
getId("url").then(function (id) {
    return getNameById(id);
}).then(function (name) {
    let course=getCourseByName(name);
    //对course进行相关操作
})
```

★可以发现改写后在调用时比嵌套回调函数简洁了许多!!!



ps:

听说有async/await方法更厉害! 等以后学了更新



















