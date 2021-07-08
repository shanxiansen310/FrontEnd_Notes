# React基础



### 背景

#### ▼什么是Babel？ [Babel](https://babeljs.io/)

##### Babel 是一个 JavaScript 编译器。

Babel 是一个工具链，主要用于在旧的浏览器或环境中将 ECMAScript 2015+ 代码转换为向后兼容版本的 JavaScript 代码：

- 转换语法
- Polyfill 实现目标环境中缺少的功能 (通过 [@babel/polyfill](https://babel.docschina.org/docs/en/babel-polyfill))
- 源代码转换 (codemods)



Babel 通过语法变换器支持最新版本的 JavaScript。这些 [plugins](https://babel.docschina.org/docs/en/plugins) 允许你现在就使用新语法，而无需等待浏览器的支持。查看我们的[使用指南](https://babel.docschina.org/docs/en/usage)以开始使用。



##### JSX 和 React

Babel 可以转换 JSX 语法！查看 [React preset](https://babel.docschina.org/docs/en/babel-preset-react) 以开始使用

你可以通过以下这个命令安装该 preset

```shell
npm install --save-dev @babel/preset-react
Copy
```

并将 `@babel/preset-react` 添加到你的 Babel 配置中。

```jsx
export default React.createClass({
  getInitialState() {
    return { num: this.getRandomNumber() };
  },

  getRandomNumber() {
    return Math.ceil(Math.random() * 6);
  },

  render() {
    return <div>
      Your dice roll:
      {this.state.num}
    </div>;
  }
});
```





### ▼什么是JSX？

```jsx
const element = <h1>Hello, world!</h1>;
```

这个有趣的标签语法既不是字符串也不是 HTML。它被称为 JSX，是一个 JavaScript 的语法扩展。我们建议在 React 中配合使用 JSX，JSX 可以很好地描述 UI 应该呈现出它应有交互的本质形式。JSX 可能会使人联想到模版语言，但它**具有 JavaScript 的全部功能。**



#### **Why use？**

1. JSX 执行更快，因为它在编译为 JavaScript 代码后进行了优化。
2. 它是类型安全的，在编译过程中就能发现错误。
3. 使用 JSX 编写模板更加简单快速。



#### How to use JSX?

##### 在 JSX 中嵌入表达式

在下面的例子中，我们声明了一个名为 `name` 的变量，然后在 JSX 中使用它，并将它包裹在大括号中：

```jsx
const name = 'Josh Perez';
const element = <h1>Hello, {name}</h1>;
ReactDOM.render(
  element,
  document.getElementById('root')
);
```

仅仅这样, 就可以通过babel编译该jsx文件, 使得页面显示内容! 

💎在 JSX 语法中，你可以在大括号内放置任何有效的 [JavaScript 表达式](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Guide/Expressions_and_Operators#Expressions)。例如，`2 + 2`，`user.firstName` 或 `formatName(user)` 都是有效的 JavaScript 表达式。



```jsx
function formatName(user) {
  return user.firstName + ' ' + user.lastName;
}

const user = {
  firstName: 'Harper',
  lastName: 'Perez'
};

const element = (
  <h1>
    Hello, {formatName(user)}!  </h1>
);

ReactDOM.render(
  element,
  document.getElementById('root')
);
```

<iframe height="265" style="width: 100%;" scrolling="no" title="MWpLydV" src="https://codepen.io/shanxiansen310/embed/MWpLydV?height=265&theme-id=light&default-tab=js,result" frameborder="no" loading="lazy" allowtransparency="true" allowfullscreen="true">
  See the Pen <a href='https://codepen.io/shanxiansen310/pen/MWpLydV'>MWpLydV</a> by shanxiansen310
  (<a href='https://codepen.io/shanxiansen310'>@shanxiansen310</a>) on <a href='https://codepen.io'>CodePen</a>.
</iframe>





##### JSX 也是一个表达式

在编译之后，JSX 表达式会被转为普通 JavaScript 函数调用，并且对其取值后得到 JavaScript 对象。

也就是说，你可以在 `if` 语句和 `for` 循环的代码块中使用 JSX，将 JSX 赋值给变量，把 JSX 当作参数传入，以及从函数中返回 JSX：

```jsx
function getGreeting(user) {
  if (user) {
    return <h1>Hello, {formatName(user)}!</h1>;  }
  return <h1>Hello, Stranger.</h1>;
}
```





##### JSX 特定属性

你可以通过使用引号，来将属性值指定为字符串字面量：

```jsx
const element = <div tabIndex="0"></div>;
```

也可以使用大括号，来在属性值中插入一个 JavaScript 表达式：

```jsx
const element = <img src={user.avatarUrl}></img>;
```

在属性中嵌入 JavaScript 表达式时，不要在大括号外面加上引号。你应该仅使用引号（对于字符串值）或大括号（对于表达式）中的一个，对于同一属性不能同时使用这两种符号。

> **警告：**
>
> 因为 JSX 语法上更接近 JavaScript 而不是 HTML，所以 React DOM 使用 `camelCase`（小驼峰命名）来定义属性的名称，而不使用 HTML 属性名称的命名约定。
>
> 例如，JSX 里的 `class` 变成了 [`className`](https://developer.mozilla.org/en-US/docs/Web/API/Element/className)，而 `tabindex` 则变为 [`tabIndex`](https://developer.mozilla.org/en-US/docs/Web/API/HTMLElement/tabIndex)。





##### JSX 防止注入攻击

你可以安全地在 JSX 当中插入用户输入内容：

```js
const title = response.potentiallyMaliciousInput;
// 直接使用是安全的：
const element = <h1>{title}</h1>;
```

React DOM 在渲染所有输入内容之前，默认会进行[转义](https://stackoverflow.com/questions/7381974/which-characters-need-to-be-escaped-on-html)。它可以确保在你的应用中，永远不会注入那些并非自己明确编写的内容。所有的内容在渲染之前都被转换成了字符串。这样可以有效地防止 [XSS（cross-site-scripting, 跨站脚本）](https://en.wikipedia.org/wiki/Cross-site_scripting)攻击。



##### JSX 表示对象

Babel 会把 JSX 转译成一个名为 `React.createElement()` 函数调用。

以下两种示例代码完全等效：

```jsx
const element = (
  <h1 className="greeting">
    Hello, world!
  </h1>
);
const element = React.createElement(
  'h1',
  {className: 'greeting'},
  'Hello, world!'
);
```

`React.createElement()` 会预先执行一些检查，以帮助你编写无错代码，但实际上它创建了一个这样的对象：

```jsx
// 注意：这是简化过的结构
const element = {
  type: 'h1',
  props: {
    className: 'greeting',
    children: 'Hello, world!'
  }
};
```

这些对象被称为 “React 元素”。它们描述了你希望在屏幕上看到的内容。React 通过读取这些对象，然后使用它们来构建 DOM 以及保持随时更新



推荐使用的高亮插件:

**Visual Studio Code**

Install the [vscode-language-babel](https://marketplace.visualstudio.com/items?itemName=mgmcdermott.vscode-language-babel) extension and follow the instructions.

There seems to be one other way to get the syntax highlighting working and you can learn more about it in the [Visual Studio Code docs](https://code.visualstudio.com/Docs/languages/javascript#_javascript-projects-jsconfigjson).

**WebStorm**

WebStorm now ships with support for ES2015+ without requiring the installation of any additional extensions. You may, however, need to [enable it](https://blog.jetbrains.com/webstorm/2015/05/ecmascript-6-in-webstorm-transpiling/).













