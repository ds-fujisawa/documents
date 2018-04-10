### ES6(2015)で追加された機能・構文
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
let arr = ["a", "b", "c"]
console.log(arr);		// Array(3)
console.log(...arr);	// a b c
```
