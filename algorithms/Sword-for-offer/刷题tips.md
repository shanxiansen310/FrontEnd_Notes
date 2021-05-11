#### 1.mid问题

   i              0     1      2     3     4      5
arr[i]         5    12    34    4    17    87 

**Math.floor((low+high)/2)** : 长度为奇数时刚好是中位数 , 长度为偶数时是中间较小的那个数

**Math.floor((arr.length-1)/2)同理**  ❗注意 -1 的位置

🌟直接用位运算代替更好!!!   <span style="color:red; font-weight:bold">(low+high)>>1</span>



★更严谨的考虑:(left + right) / 2 可以用left + ((rigth - left) >> 1)), 因为直接加可能会溢出



#### 2.浮点数要考虑溢出的问题



#### 3.ACM模式输入输出



```js
const readline=require('readline');
const rl = readline.createInterface({
    input:process.stdin,
    output:process.stdout
})
rl.on('line',function(line){

}
```

单行数据处理

```js
const data=[]
rl.on('line',function(line){
	data=line.trim().split(' '); //可以不用trim	
}
```



多行数据处理

```js
const data=[]
rl.on('line',function(line){
	data.push(line.trim().split(' ')); //可以不用trim	
    //通过判断data长度来决定何时进入逻辑部分
    if(data.length===n){}
}
```



▼如何保证连续输入输出? 

ACM模式下每次输入的数据不止一组, 我们不能只考虑一次的情况

★对于单行数据, 我们不需要额外的处理, rl.on本身就会一直监听, 不会关闭

★对于多行数据, 我们在每次输出结果后需要重置一下接收数据的数组

```js
const data=[]
rl.on('line',function(line){
	data.push(line.trim().split(' ')); //可以不用trim	
    //通过判断data长度来决定何时进入逻辑部分
    if(data.length===n){
        //...
        console,log();
        //before next circulation
        data.length=0;
    }
}
```





#### 4.关于成双成对

有时会遇到一些题，表示什么对， 比如逆序对， 此时我们可以联想到归并排序， 在归并排序递归的尽头就是一对！！！





#### 5.Math.max()

如果里面参数存在有Number后为NaN的,那么会直接返回NaN













