# lodash





### lodash使用



#### In a browser:

```html
<script src="lodash.js"></script>
```



cdn引入：

<script src="https://www.jsdelivr.com/package/npm/lodash"></script>



#### Node

这里以单个文件index.js举例

Using npm:

```shell
npm i -g npm
npm i --save lodash
```



##### CommonJS

这是默认的环境，按照官网教程即可！

```js
// Load the full build.
var _ = require('lodash');
// Load the core build.
var _ = require('lodash/core');
// Load the FP build for immutable auto-curried iteratee-first data-last methods.
var fp = require('lodash/fp');

// Load method categories.
var array = require('lodash/array');
var object = require('lodash/fp/object');

// Cherry-pick methods for smaller browserify/rollup/webpack bundles.
var at = require('lodash/at');
var curryN = require('lodash/fp/curryN');
```



##### Module



❗️注意需要在 package.json中设置  <span style='color:blue;'>"type": "module"</span>



```js
import _ from 'lodash'

console.log(_.chunk(['a', 'b', 'c', 'd'])) //直接使用lodash里的方法
```



按需引入：

```js
import chunk from 'lodash/chunk.js'

const res = chunk(['a', 'b', 'c', 'd'], 2);
console.log(res)
```



⚠️lodash不支持具名引入！

不过可以这样：

```js
import _ from 'lodash'

const {chunk}=_;
const res = chunk(['a', 'b', 'c', 'd'], 2);
console.log(res)
```



如果在module模式下遇到解决不了的问题可以用npm安装 lodash-es 这个库，支持lodash使用es module





### lodash方法



#### throttle

`_.throttle(func, [wait=0], [options=])`[#](https://www.lodashjs.com/docs/lodash.throttle#_throttlefunc-wait0-options)

创建一个节流函数，在 wait 秒内最多执行 `func` 一次的函数。 该函数提供一个 `cancel` 方法取消延迟的函数调用以及 `flush` 方法立即调用。 可以提供一个 options 对象决定如何调用 `func` 方法， options.leading 与|或 options.trailing 决定 wait 前后如何触发。 `func` 会传入最后一次传入的参数给这个函数。 随后调用的函数返回是最后一次 `func` 调用的结果。



```html
<body>
<div id="content" style="height:150px;line-height:150px;text-align:center; color: #fff;background-color:#ccc;font-size:80px;"></div>
<script type="module">
  import throttle from '../../node_modules/lodash-es/throttle.js'
  let num = 1;
  let content = document.getElementById('content');

  function count() {
    content.innerHTML = `${num++}`;
  }
  content.onmousemove = throttle(count,1000,
    {
      'leading':true,
      'trailing':false
    });
</script>
</body>
```

主要说一下leading和trailing的区别：

* leading：立即触发
* trailing：非立即触发（等待wait时间后触发）









