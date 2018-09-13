# JavaScriptオブジェクト操作メモ
個人的によく使っている配列操作をまとめてみました。  
※良い感じにしてくれる操作が少ないので下記のライブラリ等を使用することをお勧めします。

* [Lodash.js](https://lodash.com/)
* [Ramda.js](https://ramdajs.com/)

### Object.assign
[詳しくはこちら](https://developer.mozilla.org/ja/docs/Web/JavaScript/Reference/Global_Objects/Object/assign)
```javascript
/* 概要 */
// オブジェクトのコピーや合成
// 合成によく使っている
// ※コピーであればスプレッド演算子の方が楽

/* 使い方 */
const target = {
  a: 1,
  b: 2,
  c: 3
};

console.log(Object.assign({c: 4, d: 5}, target));
console.log(Object.assign(target, {c: 4, d: 5}));
console.log(target);
console.log(Object.assign({}, target));
console.log({...target});

/* 結果 */
// Object { c: 3, d: 5, a: 1, b: 2 }
// Object { a: 1, b: 2, c: 4, d: 5 }
// Object { a: 1, b: 2, c: 4, d: 5 }
// Object { a: 1, b: 2, c: 4, d: 5 }
// Object { a: 1, b: 2, c: 4, d: 5 }
```

### Object.keys
[詳しくはこちら](https://developer.mozilla.org/ja/docs/Web/JavaScript/Reference/Global_Objects/Object/keys)
```javascript
/* 概要 */
// key値を配列にして戻す
// 戻り値から配列操作するためによく使う

/* 使い方 */
const target = {
  a: 1,
  b: 2,
  c: 3
};
console.log(Object.keys(target));

/* 結果 */
// Array(3) [ "a", "b", "c" ]
```

### Object.prototype.hasOwnProperty
[詳しくはこちら](https://developer.mozilla.org/ja/docs/Web/JavaScript/Reference/Global_Objects/Object/hasOwnProperty)
```javascript
/* 概要 */
// プロパティの検索に使用
// ※メソッドとして呼び出すと検索するオブジェクトがオブジェクトじゃない場合、エラーになるためcallしてあげた方が良い

/* 使い方 */
const target = {
  a: 1,
  b: 2,
  c: 3
};
console.log(target.hasOwnProperty("a"));
console.log(target.hasOwnProperty("d"));
Object.hasOwnProperty.call(target, "a");
Object.hasOwnProperty.call(target, "d");

/* 結果 */
// true
// false
// true
// false
```