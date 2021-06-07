## TypeScript学习



![image-20210607213054817](TS基础.assets/image-20210607213054817.png)





### 什么是TypeScript?

 ![image-20210607230321764](TS基础.assets/image-20210607230321764.png)



❗TypeScript无法在浏览器中运行

















### Quick Start

#### 安装

>npm i -g typescript



#### 第一个文件

建立typescript文件, 以 .ts 结尾

```typescript
function greeter(person: string) {
    return "Hello, " + person;
}

let user = "Jane User";
greeter(user);
```





#### 编译代码

在命令行上，运行TypeScript编译器：

> tsc greeter.ts



❗这仅仅是编译，不会运行哦！  **这个命令的结果是将ts文件编译成js文件并且会生成一个js文件**

 <img src="TS基础.assets/image-20210607221502149.png" alt="image-20210607221502149" style="zoom:73%;" />

greeter.js

```js
function greeter(person) {
    return "Hello, " + person;
}
var user = "Jane User";
greeter(user);
```



#### **类型注解**

**type annotation**

TypeScript里的类型注解是一种轻量级的为函数或变量添加约束的方式。 在这个例子里，我们希望 `greeter`函数接收一个字符串参数。 如果我们给greeter传入其它类型的参数就会报错!

🌟**不过尽管有错误，`greeter.js`文件还是会被创建**。 就算你的代码里有错误，你仍然可以使用TypeScript。但在这种情况下，TypeScript会警告你代码可能不会按预期执行。





#### Interfaces



这里我们使用接口来描述一个拥有`firstName`和`lastName`字段的对象。 **在TypeScript里，只要两个类型内部的结构兼容那么这两个类型就是兼容的。** 这就允许我们在实现接口时候只要保证==包含==了接口要求的结构就可以，而不必明确地使用 `implements`语句。

greeter.ts

```ts
interface Person {
  firstName: string;
  lastName: string;
}

function greeter(person: Person) {
  return "Hello, " + person.firstName + " " + person.lastName;
}

let user = {firstName: "Jane", lastName: "User",age:12};

console.log(greeter(user));
```



greeter.js

```js
function greeter(person) {
  return "Hello, " + person.firstName + " " + person.lastName;
}
var user = { firstName: "Jane", lastName: "User", age: 12 };
console.log(greeter(user));
```



#### Classes

TypeScript支持JavaScript的新特性，比如支持基于类的面向对象编程。

让我们创建一个`Student`类，它带有一个构造函数和一些公共字段。 注意类和接口可以一起共作，程序员可以自行决定抽象的级别。

🌟还要注意的是，**在构造函数的参数上使用`public`等同于创建了同名的成员变量**。

greeter.ts

```js
class Student {
  fullName: string;
  constructor(public firstName, public middleInitial, public lastName) {
    this.fullName = firstName + " " + middleInitial + " " + lastName;
  }
}

interface Person {
  firstName: string;
  lastName: string;
}

function greeter(person : Person) {
  return "Hello, " + person.firstName + " " + person.lastName;
}

let user = new Student("Jane", "M.", "User");
console.log(greeter(user));
```



greeter.js

```js
var Student = /** @class */ (function () {
    function Student(firstName, middleInitial, lastName) {
        this.firstName = firstName;
        this.middleInitial = middleInitial;
        this.lastName = lastName;
        this.fullName = firstName + " " + middleInitial + " " + lastName;
    }
    return Student;
}());
function greeter(person) {
    return "Hello, " + person.firstName + " " + person.lastName;
}
var user = new Student("Jane", "M.", "User");
console.log(greeter(user));
console.log(user.fullName);
```





#### Running

其实就是结合TS编译生成的js文件

```html
<!DOCTYPE html>
<html>
    <head><title>TypeScript Greeter</title></head>
    <body>
        <script src="greeter.js"></script>
    </body>
</html>
```

