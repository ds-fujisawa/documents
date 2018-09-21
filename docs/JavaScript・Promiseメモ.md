# Promiseメモ
自分の中でもあやふやだったPromiseについてまとめてみました。

## Promiseとは？
[詳しくはこちら](https://developer.mozilla.org/ja/docs/Web/JavaScript/Guide/Using_promises)  

```js
/* 概要 */
// 非同期処理の最終的な完了もしくは失敗を表すオブジェクト

/* 使い方 */
const promise = new Promise((resolve, reject) => {
  setTimeout(() => {
    console.log('hoge');
    resolve();
  }, 300);
});
promise.then(() => {
  console.log('foo');
});

/* 結果 */
// hoge
// foo

/* 使わないと… */
setTimeout(() => {
  console.log('hoge');
}, 300);
console.log('foo');

/* 結果 */
// foo
// hoge
// ↑結果が逆になってしまう
```

## 基本的な形

```js
const promise = new Promise((resolve, reject) => {
  setTimeout(() => {
    if (success) {
      resolve('success');  // ← resolveを実行するとthenになる
    } else {
      reject('error');  // ← rejectを実行するとcatchになる
    }
  }, 300);
});
promise.then(res => {
  console.log(res);  // success
}).catch(res => {
  console.log(res);  // error
}).finaly(() => { // ← どちらでも実行される
  console.log('default');
});
```

## 並列処理
Promise.all() or Promise.race()を使う

```js
const promise1 = Promise.resolve(3);
const promise2 = 42;  // ← Promise以外も設定可能
const promise3 = new Promise((resolve, reject) => {
  setTimeout(() => {
    resolve('foo');
  }, 100);
});

// 全て終わってから処理したい時
Promise.all([promise1, promise2, promise3]).then(values => {
  console.log(values); // 結果：Array [3, 42, "foo"]
});

// どれか１つが終わったら処理したい時
Promise.race([promise1, promise2, promise3]).then(value => {
  console.log(value); // 結果：3
});
```

## 直列処理(逐次処理)

**処理が少なければthenを繋げていく**

```js
const promise = new Promise((resolve, reject) => {
  setTimeout(() => {
    console.log('hoge');
    resolve();
  }, 300);
}).then(() => new Promise((resolve, reject) => {
  setTimeout(() => {
    console.log('foo');
    resolve();
  }, 500);
}));
promise.then(() => {
  console.log('bar');
});
```

**for等で繰り返す場合が少し厄介**

```js
// 以下のように書くと結果が思った通りにならない
const add = n => new Promise(resolve => {
  setTimeout(() => {
    resolve(n + 10)
  }, 2000);
});

for(let i = 1; i < 5; i++) {
  console.log(i);
  add(i).then(v => {
    console.log(v);
  });
};

/* 結果 */
// 1 2 3 4   11 12 13 14
// 本当は↓こうなってほしい
// 1 11 2 12 3 13 4 14
```

**Array.prototype.reduceを使う**

```js
const add = n => new Promise(resolve => {
  setTimeout(() => {
    resolve(n + 10)
  }, 2000);
});
(()=>{
  const addArr = [];
  for(let i = 1; i < 5; i++) {
    addArr.push(() => {
      console.log(i);
      return add(i).then(v => {
        console.log(v);
      })
    });
  }
  addArr.reduce((a, c) => a.then(c), Promise.resolve());
})();

/* 結果 */
// 1 11 2 12 3 13 4 14
```

## async + await

**async関数**  
[詳しくはこちら](https://developer.mozilla.org/ja/docs/Web/JavaScript/Reference/Statements/async_function)

* 非同期処理を制御するために使用
* async関数の中でのみ、await式が使用可能
* await式は、async関数の実行を一時停止しPromiseの解決を待つ
* Promiseが解決したら、async関数の実行が再開される

**await**  
[詳しくはこちら](https://developer.mozilla.org/ja/docs/Web/JavaScript/Reference/Operators/await)

```js
/* 概要 */
// await演算子は、async関数によってPromiseが返されるのを待機するために使用
// これによって直列処理(逐次処理)が楽に書ける

/* 使い方 */
// 上記Array.prototype.reduceで書いていた処理をasync + await使うとこうなる
const add = n => new Promise(resolve => {
  setTimeout(() => {
    resolve(n + 10)
  }, 2000);
});

(async () => {  // 関数の最初にasyncを付ける
  for(let i = 1; i < 5; i++) {
    console.log(i);
    await add(i).then(v => {  // async関数内でPromiseを返す関数の最初にawaitを付ける
      console.log(v);
    });
  }
})();

/* 結果 */
// 1 11 2 12 3 13 4 14
```

**繰り返しが多重になった場合**  
Array.prototype.reduce
```js
const mul = n => new Promise(resolve => {
  setTimeout(() => {
    resolve(n * 2)
  }, 1000);
});
const add = n => new Promise(resolve => {
  setTimeout(() => {
    resolve(n + 10)
  }, 2000);
});
(() => {
const mulArr = [];
for(let i = 1; i < 5; i++) {
  mulArr.push(() =>
    mul(i).then(r => {
      const addArr = [];
      console.log(r);
      for(let j = 1; j < r; j++) {
        addArr.push(() => add(j).then(a => {
            console.log(a);
          })
        );
      }
      return addArr.reduce((a, c) => a.then(c), Promise.resolve());
    })
  );
}
mulArr.reduce((a, c) => a.then(c), Promise.resolve());
})();
/* 結果（見やすくするため改行あり） */
// 2 11
// 4 11 12 13
// 6 11 12 13 14 15
// 8 11 12 13 14 15 16 17
```
async + await  
```js
const mul = n => new Promise(resolve => {
  setTimeout(() => {
    resolve(n * 2)
  }, 1000);
});
const add = n => new Promise(resolve => {
  setTimeout(() => {
    resolve(n + 10)
  }, 2000);
});
(async ()=>{
for(let i = 1; i < 10; i++) {
  await mul(i).then(async r => {
    console.log(r);
    for(let j = 1; j < r; j++) {
      await add(j).then(a => {
        console.log(a);
      })
    }
  })
}})();

/* 結果（見やすくするため改行あり） */
// 2 11
// 4 11 12 13
// 6 11 12 13 14 15
// 8 11 12 13 14 15 16 17
```

## Promiseメソッド一覧

* [Promise.all()](https://developer.mozilla.org/ja/docs/Web/JavaScript/Reference/Global_Objects/Promise/all)
* [Promise.prototype.catch()](https://developer.mozilla.org/ja/docs/Web/JavaScript/Reference/Global_Objects/Promise/catch)
* [Promise.prototype.finally()](https://developer.mozilla.org/ja/docs/Web/JavaScript/Reference/Global_Objects/Promise/finally)
* [Promise.prototype.then()](https://developer.mozilla.org/ja/docs/Web/JavaScript/Reference/Global_Objects/Promise/then)
* [Promise.race()](https://developer.mozilla.org/ja/docs/Web/JavaScript/Reference/Global_Objects/Promise/race)
* [Promise.reject()](https://developer.mozilla.org/ja/docs/Web/JavaScript/Reference/Global_Objects/Promise/reject)
* [Promise.resolve()](https://developer.mozilla.org/ja/docs/Web/JavaScript/Reference/Global_Objects/Promise/resolve)

## もっと詳細に知りたい方
**[Promiseの本](http://azu.github.io/promises-book/)**  
こちらがかなり細かく記載されていて分かり易いです
