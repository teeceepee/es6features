# ECMAScript 6

## 简介
ECMAScript 6, 也叫做 ECMAScript 2015，是 ECMAScript 标准的最新版本。ES6 是语言的一次重大更新，也是 2009 年 ES5 标准化后的第一次更新。主流 JavaScript 引擎里的这些特性的实现正在[进行中](http://kangax.github.io/es5-compat-table/es6/)。

前往 [ES6 标准](http://www.ecma-international.org/ecma-262/6.0/) 查看 ECMAScript 6 语言的完整规范。

ES6 包括以下新特性：
- [arrows](#arrows) done
- [classes](#classes) done
- [enhanced object literals](#enhanced-object-literals) done
- [template strings](#template-strings) done
- [destructuring](#destructuring) done
- [default + rest + spread](#default--rest--spread) done
- [let + const](#let--const) done
- [iterators + for..of](#iterators--forof) done
- [generators](#generators) done
- [unicode](#unicode) done
- [modules](#modules)
- [module loaders](#module-loaders)
- [map + set + weakmap + weakset](#map--set--weakmap--weakset)
- [proxies](#proxies)
- [symbols](#symbols)
- [subclassable built-ins](#subclassable-built-ins)
- [promises](#promises)
- [math + number + string + array + object APIs](#math--number--string--array--object-apis)
- [binary and octal literals](#binary-and-octal-literals)
- [reflect api](#reflect-api)
- [tail calls](#tail-calls)

## ECMAScript 6 特性

### 箭头 Arrows
箭头是用 `=>` 表示的函数简写法。从语法上看，它很像 C#、Java 8 以及 CoffeeScript 中相似的特性。箭头函数的函数体可以是语句块，也可以是表达式。不像普通函数，箭头函数与包裹它的代码共享相同的 `this` 引用。

```JavaScript
// Expression bodies
var odds = evens.map(v => v + 1);
var nums = evens.map((v, i) => v + i);
var pairs = evens.map(v => ({even: v, odd: v + 1}));

// Statement bodies
nums.forEach(v => {
  if (v % 5 === 0)
    fives.push(v);
});

// Lexical this
var bob = {
  _name: "Bob",
  _friends: [],
  printFriends() {
    this._friends.forEach(f =>
      console.log(this._name + " knows " + f));
  }
}
```

### 类 Classes
ES6 的类是基于原型的面向对象模式的一层简单的语法糖。使用单一的声明方式让类模式用起来更简单，同时也利于与其他语言互通。类支持基于原型的继承、super 调用、实例和静态方法以及构造函数。

```JavaScript
class SkinnedMesh extends THREE.Mesh {
  constructor(geometry, materials) {
    super(geometry, materials);

    this.idMatrix = SkinnedMesh.defaultMatrix();
    this.bones = [];
    this.boneMatrices = [];
    //...
  }
  update(camera) {
    //...
    super.update();
  }
  get boneCount() {
    return this.bones.length;
  }
  set matrixType(matrixType) {
    this.idMatrix = SkinnedMesh[matrixType]();
  }
  static defaultMatrix() {
    return new THREE.Matrix4();
  }
}
```

### 增强的对象字面量 Enhanced Object Literals
对象字面量被扩展来支持构造时指定原型，简化 `foo: foo` 赋值、方法定义、父类方法调用，通过表达式计算属性名。这也让对象字面量和类声明更相近，从而令基于对象的设计从中受益。

```JavaScript
var obj = {
    // __proto__
    __proto__: theProtoObj,
    // Shorthand for ‘handler: handler’
    handler,
    // Methods
    toString() {
     // Super calls
     return "d " + super.toString();
    },
    // Computed (dynamic) property names
    [ 'prop_' + (() => 42)() ]: 42
};
```

### 模板字符串 Template Strings
模板字符串提供了构造字符串的语法糖。它类似 Perl、Python 等语言的字符串插值。作为可选项，可以在前面加一个标签函数来定制字符串的构造，用来防止注入攻击或根据字符串内容构造高级的数据结构。

```JavaScript
// Basic literal string creation
`In JavaScript '\n' is a line-feed.`

// Multiline strings
`In JavaScript this is
 not legal.`

// String interpolation
var name = "Bob", time = "today";
`Hello ${name}, how are you ${time}?`

// Construct an HTTP request prefix is used to interpret the replacements and construction
POST`http://foo.org/bar?a=${a}&b=${b}
     Content-Type: application/json
     X-Credentials: ${credentials}
     { "foo": ${foo},
       "bar": ${bar}}`(myOnReadyStateChangeHandler);
```

### 解构 Destructuring
解构允许模式匹配的变量绑定，还支持数组和对象的匹配。解构是 fail-soft 的，类似标准的对象查找 `foo["bar"]`，查找失败会返回 `undefined` 值。

```JavaScript
// list matching
var [a, , b] = [1,2,3];

// object matching
var { op: a, lhs: { op: b }, rhs: c }
       = getASTNode()

// object matching shorthand
// binds `op`, `lhs` and `rhs` in scope
var {op, lhs, rhs} = getASTNode()

// Can be used in parameter position
function g({name: x}) {
  console.log(x);
}
g({name: 5})

// Fail-soft destructuring
var [a] = [];
a === undefined;

// Fail-soft destructuring with defaults
var [a = 1] = [];
a === 1;
```

### 函数默认参数、rest 参数、spread 参数 Default + Rest + Spread
调用时求值的默认参数值。把一个数组变成函数调用时的连续几个参数。把结尾的几个参数绑定到一个数组。rest 参数取代了对 `arguments` 变量的需求并且让一些常用的参数处理方式变得更直接。


```JavaScript
function f(x, y=12) {
  // y is 12 if not passed (or passed as undefined)
  return x + y;
}
f(3) == 15
```
```JavaScript
function f(x, ...y) {
  // y is an Array
  return x * y.length;
}
f(3, "hello", true) == 6
```
```JavaScript
function f(x, y, z) {
  return x + y + z;
}
// Pass each elem of array as argument
f(...[1,2,3]) == 6
```

### let 和 const Let + Const
块作用域的绑定语法构造。`let` 就是新的 `var`，`const` 是单赋值。静态的限制来预防使用前赋值。

```JavaScript
function f() {
  {
    let x;
    {
      // okay, block scoped name
      const x = "sneaky";
      // error, const
      x = "foo";
    }
    // error, already declared in block
    let x = "inner";
  }
}
```

### 迭代器和 `for..of` Iterators + For..Of
迭代器对象允许像 CLR IEnumerable 或 Java Iterable 那样定制迭代过程。从 `for..in` 推广到使用 `for..of` 的基于迭代器的定制化迭代过程。不需要实现一个数组，使得像 LINQ 那样的惰性设计模式成为可能。

```JavaScript
let fibonacci = {
  [Symbol.iterator]() {
    let pre = 0, cur = 1;
    return {
      next() {
        [pre, cur] = [cur, pre + cur];
        return { done: false, value: cur }
      }
    }
  }
}

for (var n of fibonacci) {
  // truncate the sequence at 1000
  if (n > 1000)
    break;
  console.log(n);
}
```

### 生成器
生成器通过 `function*` 和 `yield` 简化迭代器相关操作(TODO)。用 function* 声明的函数返回一个生成器实例。生成器是迭代器的子类型，包括额外的 `next` 和 `throw` 方法。这使值流回生成器成为可能，因此 `yield` 是一个可以返回值的表达式。

```JavaScript
var fibonacci = {
  [Symbol.iterator]: function*() {
    var pre = 0, cur = 1;
    for (;;) {
      var temp = pre;
      pre = cur;
      cur += temp;
      yield cur;
    }
  }
}

for (var n of fibonacci) {
  // truncate the sequence at 1000
  if (n > 1000)
    break;
  console.log(n);
}
```

生成器的对外接口是（使用 TypeScript 的类型语法描述）：

```TypeScript
interface Generator extends Iterator {
    next(value?: any): IteratorResult;
    throw(exception: any);
}
```

### Unicode
增加向前兼容的新特性来完整支持 Unicode，包括新的 Unicode 字符串字面量，正则表达式中新的处理代码点的 `u` 模式，以及新的处理包含 21 位代码点字符串的 API。这些新特性支持使用 JavaScript 构建全球化的应用。

```JavaScript
// same as ES5.1
"𠮷".length == 2

// new RegExp behaviour, opt-in ‘u’
"𠮷".match(/./u)[0].length == 2

// new form
"\u{20BB7}"=="𠮷"=="\uD842\uDFB7"

// new String ops
"𠮷".codePointAt(0) == 0x20BB7

// for-of iterates code points
for(var c of "𠮷") {
  console.log(c);
}
```
