### ES6(2015)以降で追加された機能・構文
**変数**
```javascript
let a = 'test';		// 再宣言が不可能
const b = 'temp';	// 再宣言と再代入が不可能
```
**アロー関数**
```javascript
/* 従来(ES5) */
var fn = function (a, b) {
  return a + b;
};

/* ES6(2015) */
const fn = (a, b) => {
	return a + b;
}
// ↓引数は１つの場合()省略可能、戻り値のみの場合{}省略可能
const fn = a => a * 2;  // (a) => { return a * 2 } と同じ

/* ※thisの参照先が異なるので注意 */
// (例)
param = 'global param';

function printParam() { console.log(this.param); }

let allowParam = () => { console.log(this.param); }

let object = {
  param: 'object param',
  func: printParam
}
let objectAllow = {
  param: 'object param',
  func: allowParam
}

object.func();		// object param
objectAllow.func();	// global param

```
**Class構文**
```javascript
/* 従来(ES5) */
var Person = function(name) {
  this.name = name;
};
Person.prototype = {
	sayHello: function() {
		console.log("Hello, I'm " + this.getName());
	},
	getName: function() {
		return this.name;
	}
}

/* ES6(2015) */
class Person {
  constructor(name) {
    this.name = name;
  }
  sayHello() {
    console.log("Hello, I'm " + this.getName());
  }
  getName() {
    return this.name;
  }
}
// extends(継承)も可能
class Teacher extends Person {
  sayHello() {
    console.log("Hi, I'm " + this.getName());
  }
}
const teacher = new Teacher('soarflat');
teacher.sayHello();	// Hi, I'm soarflat
```
**関数のデフォルト引数**
```javascript
function multiply (a = 5) {
  return a * a;
}
console.log(multiply());	// 25
console.log(multiply(10));	// 100
```
**展開演算子**
```javascript
const arr = ["a", "b", "c"]
console.log(arr);		// Array(3)
console.log(...arr);	// a b c

/* ※Objectのコピーにも使える */
const obj = {
  a: 1,
  b: 2,
  c: 3
};
const hoge = obj;  // ディープコピー
const foo = {...obj};  // シャロウコピー
// const foo = Object.assign({}, obj) と同じ

hoge.a = 3;  // 参照も渡してしまっているため、元のobjも変わってしまう
/**
 * console.log(hoge, obj);
 * [Result]
 * hoge: { a: 3, b: 2, c: 3 }, obj: { a: 3, b: 2, c: 3 }
 */
foo.a = 3;  // 値だけ渡しているのでfooしか変わらない
/**
 * console.log(foo, obj);
 * [Result]
 * foo: { a: 3, b: 2, c: 3 }, obj: { a: 1, b: 2, c: 3 }
 */
```
**テンプレート文字列**
```javascript
const foo = 'foo'
const bar = 'bar'
const baz = () => {
    return 'baz'
}

// 変数を展開する
const foobar = `${foo}, ${bar}` // Result → "foo, bar"

// 式の評価もできる
const some = `${baz()}` // Result → "baz"
```
