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



#### 6.while(i--)循环问题

```js
let i=4;
while (i--){
 console.log(i);
}
console.log(i);
//3 2 1 0 
//-1
```

while先对条件语句进行判断, 这里是i, 也就是先对i进行判断, i=4为truthy, 那么才会继续执行i--，再继续执行下面的语句，也就是说真正的执行顺序是：

1. 判断i是否为真值
2. 执行条件语句i--后i=3 
3. 执行while循环体内的语句

💣<span style="font-weight:bold; color:red;">注意:不论i是否为真值都会执行i--</span> ,  也就是说在最后i为0时, while循环体内的语句不再执行, 但条件语句i--还是会执行。因此 i 最后的值为 -1 ！



同理 while(i-->0) 也是一个道理, 先判断i是否大于0, 再进行 i-=1 





#### 7.模数问题





最近遇到了一道题，我觉得   $a\%b+c = (a+c)\%b $  , 实际上这在不涉及刚好 $a\%b=0$ 的情况可行，但一旦涉及就会出错！  

比如  $17 \% 9 + 1=9$ ,  可是 $(17+1) \% 9=0$

再比如  $18\%9-2=-2$  ,  可是 $(18-2)\%9=7$











































