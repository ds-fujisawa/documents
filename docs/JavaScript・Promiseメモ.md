# Promiseメモ
自分の中でもあやふやだったPromiseについてまとめてみました。

## Promiseとは？
[詳しくはこちら](https://developer.mozilla.org/ja/docs/Web/JavaScript/Guide/Using_promises)  

```js
/* 概要 */
// 非同期処理の最終的な完了もしくは失敗を表すオブジェクト

/* 使い方 */
const promise = new Promise(function(resolve, reject) {
  setTimeout(function() {
    console.log('hoge');
    resolve();
  }, 300);
});
promise.then(function() {
  console.log('foo');
});

/* 結果 */
// hoge
// foo

/* 使わないと… */
setTimeout(function() {
  console.log('hoge');
}, 300);
console.log('foo');

/* 結果 */
// foo
// hoge
// ↑結果が逆になってしまう
```

## Promiseメソッド一覧
**Promise.all()**  
[詳しくはこちら](https://developer.mozilla.org/ja/docs/Web/JavaScript/Reference/Global_Objects/Promise/all)

```js

```

**Promise.prototype.catch()**  
[詳しくはこちら](https://developer.mozilla.org/ja/docs/Web/JavaScript/Reference/Global_Objects/Promise/catch)

```js

```

**Promise.prototype.finally()**  
[詳しくはこちら](https://developer.mozilla.org/ja/docs/Web/JavaScript/Reference/Global_Objects/Promise/finally)

```js

```

**Promise.prototype.then()**  
[詳しくはこちら](https://developer.mozilla.org/ja/docs/Web/JavaScript/Reference/Global_Objects/Promise/then)

```js

```

**Promise.race()**  
[詳しくはこちら](https://developer.mozilla.org/ja/docs/Web/JavaScript/Reference/Global_Objects/Promise/race)

```js

```

**Promise.reject()**  
[詳しくはこちら](https://developer.mozilla.org/ja/docs/Web/JavaScript/Reference/Global_Objects/Promise/reject)

```js

```

**Promise.resolve()**  
[詳しくはこちら](https://developer.mozilla.org/ja/docs/Web/JavaScript/Reference/Global_Objects/Promise/resolve)

```js

```

## 逐次処理
## 並列処理
## async + await

**async関数**  
[詳しくはこちら](https://developer.mozilla.org/ja/docs/Web/JavaScript/Reference/Statements/async_function)

```js
/* 概要 */
// 非同期処理を制御するために使用
// async関数の中でのみ、await式が使用可能
// await式は、async関数の実行を一時停止しPromiseの解決を待つ
// Promiseが解決したら、async関数の実行が再開される
```

**await**  
[詳しくはこちら](https://developer.mozilla.org/ja/docs/Web/JavaScript/Reference/Operators/await)  

```js
/* 概要 */
// await演算子は、async関数によってPromiseが返されるのを待機するために使用

```