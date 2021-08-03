# React-Tools





记录一些react学习中用到的库或者工具。





### Classnames



Link：[JedWatson/classnames: A simple javascript utility for conditionally joining classNames together (github.com)](https://github.com/JedWatson/classnames)



Install：

```bash
# via npm
npm install classnames
```



如果我们需要在ts中使用，那么需要安装 @types

有些库可能TS不支持,我们无法直接在TS中使用, 此时则需要通过npm安装类型声明文件 

[@types 官方声明文件库](https://github.com/DefinitelyTyped/DefinitelyTyped/)   [@types 搜索声明库](https://microsoft.github.io/TypeSearch/)

> 比如 :  npm install --save @types/node

🌟@types包在编译的时候都会被包含进去  (`node_modules/@types`)



Usage:



```js
classNames('foo', 'bar'); // => 'foo bar'
classNames('foo', { bar: true }); // => 'foo bar'
classNames({ 'foo-bar': true }); // => 'foo-bar'
classNames({ 'foo-bar': false }); // => ''
classNames({ foo: true }, { bar: true }); // => 'foo bar'
classNames({ foo: true, bar: true }); // => 'foo bar'

// lots of arguments of various types
classNames('foo', { bar: true, duck: false }, 'baz', { quux: true }); // => 'foo bar baz quux'

// other falsy values are just ignored
classNames(null, false, 'bar', undefined, 0, 1, { baz: null }, ''); // => 'bar 1'

```

#### 

Arrays will be recursively flattened as per the rules above:

```js
var arr = ['b', { c: true, d: false }];
classNames('a', arr); // => 'a b c'
```



Dynamic class names with ES2015

If you're in an environment that supports [computed keys](https://www.ecma-international.org/ecma-262/6.0/#sec-object-initializer) (available in ES2015 and Babel) you can use dynamic class names:

```js
let buttonType = 'primary';
classNames({ [`btn-${buttonType}`]: true });
```



Use in React

```jsx
var classNames = require('classnames');

class Button extends React.Component {
  // ...
  render () {
    var btnClass = classNames({
      btn: true,
      'btn-pressed': this.state.isPressed,
      'btn-over': !this.state.isPressed && this.state.isHovered
    });
    return <button className={btnClass}>{this.props.label}</button>;
  }
}
```



Because you can mix together object, array and string arguments, supporting optional `className` props is also simpler as only truthy arguments get included in the result:

```jsx
var btnClass = classNames('btn', this.props.className, {
  'btn-pressed': this.state.isPressed,
  'btn-over': !this.state.isPressed && this.state.isHovered
});
```



![](data:image/jpg;base64,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)







![](data:image/jpg;base64,iVBORw0KGgoAAAANSUhEUgAAAHgAAAB4CAYAAAA5ZDbSAAAAAXNSR0IArs4c6QAABIpJREFUeAHt3cFq1EAYwPFJD30HEQTvimDPggd9AmlPIuKDiYinFp9AD4LnCqJ3QRDfoYfG/bLEprvzTbLJZOab5J9DdzvpJrP/H9l2S5M6x0IBClCAAhSgAAUoQAEKUIACFKAABShAAQpQgAIUoAAFKEABClCAAhSgAAUoQAEKUIACFKAABShAAQpQoOwCVdnTX8fs6wdnb1zlXri6ftw846r65mr3sfp5/ravAMB9hTKurx+d3nXX1bu6rp/5plFV1Wd3fPyquvzw17dexo60FYznLVCfnN5z1+6rhiuza9ZdXb0PzRTgUJ1M6xrcK/elrt39vikIcvMSrnwhwEqYXMOH4P6fo3x/VhaAlTA5hkfhykTbH748kwbYEyXH0GjcnskC3BMoxerJuPK2SVkAVsKkGp6MKxPdvCfW5sv7YK1MgvEYuPJeuPpx/lybLkewVmbm8Ti47pc7ql+HpgpwqM5M66LhHrun1feLP6Fp8hIdqjPDuqi4lxe/+6YIcF+hiOtT48rUAY4IGNpUDlyAQyIR1+XCBTgiorapnLgAayqRxnPjAhwJ0rcZC7gA+2QijFnBBTgC5u4mLOECvKsz8XNruABPBO0+3CIuwF2hCfet4gI8AbV9qGVcgFulkbfWcQEeCSsPKwEX4JHApeACPAK4JFyADwQuDRfgA4BLxAV4IHCpuAAPAC4ZF+Ae4NJxAQ4ALwEXYAV4KbiDgadcI0JpaHZ4Sbi9wPXJyztuc4kA7TICzTUiNqdO9P11vVnNnYktDVeeXvjUlQCuPLiBl+tIyPUkCl+WiCskKrC8LGtHbteyuY6EXE+iYOSl4gaBm+sydSUD90tGXjJuGDhw3QefdYnIS8cNA/sUe8ZKQl4Dbhg4cN2HkHMJyGvBDQMHrvsQApZ1lpHXhCsWwdNH64dnn4b8JC0b8i1VtbnEgJyFPuBEZd/jY4+tDVf6qW+TmrjySwxBGrlYOpLXiCtsQeDmN1RyBBaOvFZcAQ6+RMsXyFJyoJLnvq0/7eMg4FKR144rboOBS0MGV8QOBJYHlBCuhDlKyxTLQUdwOyHLAS3Pre2X8nYUsEzQYkiLc0qJ6dvXaGBryOD6eEd8D97djIWwFuaw28XK55OO4PZJ5Aycc9/t87d8GwVYnmCO0Dn2aRnTN7dowKmRwfVx7o9FBU6FDO4+pDYSHXhuZHA1Sv/4LMBzIYPrRwyNzgYcG7l5EgP/3Zv2hK39AYI2z5jjswLLRKMddbKtAf/LT4uzRlxpMTuw7CQGsmxn7LJWXOmVBFh2lAt5zbhJgXMgrx03OXBKZHCldsKX6O3uth/nfrkG96Z2su/BN7ucFxnc26WzAcs0Yh/J4N7Glc+yAssEYiGDKzX3l+zAMqWpyODuw7YjJoCnIIPbUvpvzQCPQQbXj9odNQV8CDK4XUb9fvDkM/1h861pTjU9ck+aSzQpu9levmnzNUZOS1WmaWLY3BHcrbKmC7B1nzf3KUABClCAAhSgAAUoQAEKUIACFKAABShAAQpQgAIUoAAFKEABClCAAhSgAAWWXOAfb2af+4peE0AAAAAASUVORK5CYII=)







![](data:image/jpg;base64,iVBORw0KGgoAAAANSUhEUgAAACIAAAAiCAYAAAA6RwvCAAAACXBIWXMAACE4AAAhOAFFljFgAAAAAXNSR0IArs4c6QAAAARnQU1BAACxjwv8YQUAAAE+SURBVHgBzdZBbsIwEAXQmTjdcwSWXWTRI7RHqNRWPUY5QcMNeoXuAHEHjsAGJHbkCOwhfOwskIAg4pkx4m8SOZL9NGPZIXqQMCmC4usXjB8C9Zi45MVkSMKIIXj+7iOv16eTyTEZCbPN6t75GAhlqBIJomrNvvic+8fL5aTxlRFXpFlw5959GarzcUllVBVpFg17xdUzP1P/cvLulVFDrDAmEAuMGUSLMYVoMOYQKSYJRIJJBonFJIXEYJJDbmGAbOCW4z/VEd81vBpVXLu3tuuAed9cBXeBHNO2Gmhz7ZN5mtY8+dagpTWM//C8z2a9hiAM3WJaJod0RSSFxCCSQWIRSSAShDlEijCFaBBmEC3CBGKBUEOsECGqI94KoYJsi49XK4QKku/yiowQKkj4xwgL+9dwjW8ADKSIh8oBWjEB08q28xQAAAAASUVORK5CYII=)







![](data:image/jpg;base64,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)







































