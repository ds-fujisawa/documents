# JavaScript配列操作メモ
個人的によく使っている配列操作をまとめてみました。

### Array.prototype.concat
[詳しくはこちら](https://developer.mozilla.org/ja/docs/Web/JavaScript/Reference/Global_Objects/Array/map)
```js
/* 概要 */
// 配列の合成
// 配列に他の配列や値をつないでできた新しい配列を返す
// ※引数は複数可

/* 使い方 */
const arr = [1,2,3,4].concat([5,6,7,8]);
console.log('arr', arr);

/* 結果 */
// (arr, [1,2,3,4,5,6,7,8])
```

### Array.prototype.filter
[詳しくはこちら](https://developer.mozilla.org/ja/docs/Web/JavaScript/Reference/Global_Objects/Array/filter)
```js
/* 概要 */
// 配列から条件に一致した値を抽出して配列を返す
// ※１．index, array, thisArgは省略可能
// ※２．thisArgはコールバック関数内でthisとして扱う値

/* 使い方 */
const arr = [1,2,3,4,5,6].filter(function(element, index, array) {
  console.log(element, index, array);
  return 3 < element;
}, thisArg);
console.log('arr', arr);

/* 結果 */
// (1, 0, [1,2,3,4,5,6])
// (2, 1, [1,2,3,4,5,6])
// (3, 2, [1,2,3,4,5,6])
// (4, 3, [1,2,3,4,5,6])
// (5, 4, [1,2,3,4,5,6])
// (6, 5, [1,2,3,4,5,6])
// (arr, [4,5,6])
```

### Array.prototype.find
[詳しくはこちら](https://developer.mozilla.org/ja/docs/Web/JavaScript/Reference/Global_Objects/Array/find)
```js
/* 概要 */
// filterとの違いは配列か値か
// 配列から条件に一致した最初の値を返す
// 見つからない場合はundefinedを返す
// ※１．index, array, thisArgは省略可能
// ※２．thisArgはコールバック関数内でthisとして扱う値

/* 使い方 */
const val = [1,2,3,4,5,6].find(function(element, index, array) {
  console.log(element, index, array);
  return 3 < element;
}, thisArg);
console.log('val', val);

/* 結果 */
// (1, 0, [1,2,3,4,5,6])
// (2, 1, [1,2,3,4,5,6])
// (3, 2, [1,2,3,4,5,6])
// (4, 3, [1,2,3,4,5,6])
// (5, 4, [1,2,3,4,5,6])
// (6, 5, [1,2,3,4,5,6])
// (val, 4)
```

### Array.prototype.forEach
[詳しくはこちら](https://developer.mozilla.org/ja/docs/Web/JavaScript/Reference/Global_Objects/Array/forEach)
```js
/* 概要 */
// お馴染みのやつ
// 配列の各要素で処理したい時とか使用
// ※１．index, array, thisArgは省略可能
// ※２．thisArgはコールバック関数内でthisとして扱う値

/* 使い方 */
[1,2,3,4].forEach(function(currentValue, index, array) {
  console.log(currentValue, index, array);
}, thisArg);

/* 結果 */
// (1, 0 [1,2,3,4])
// (2, 1 [1,2,3,4])
// (3, 2 [1,2,3,4])
// (4, 3 [1,2,3,4])
```

### Array.prototype.includes
[詳しくはこちら](https://developer.mozilla.org/ja/docs/Web/JavaScript/Reference/Global_Objects/Array/includes)
```js
/* 概要 */
// 配列の要素を検索する時とかに使用
// ※．第2引数(fromIndex)は省略可能

/* 使い方 */
console.log([1, 2, 3].includes(2));
console.log([1, 2, 3].includes(4));
console.log([1, 2, 3].includes(3, 3));
console.log([1, 2, 3].includes(3, -1));

/* 結果 */
// (true)
// (false)
// (false)
// (true)
```

### Array.prototype.join
[詳しくはこちら](https://developer.mozilla.org/ja/docs/Web/JavaScript/Reference/Global_Objects/Array/join)
```js
/* 概要 */
// 配列のすべての要素を繋いで文字列
// ※separatorは省略可

/* 使い方 */
console.log([1,2,3,4].join());
console.log([1,2,3,4].join(''));
console.log([1,2,3,4].join('+'));

/* 結果 */
// 1,2,3,4
// 1234
// 1+2+3+4
```

### Array.prototype.map
[詳しくはこちら](https://developer.mozilla.org/ja/docs/Web/JavaScript/Reference/Global_Objects/Array/map)
```js
/* 概要 */
// forEachとの違いは戻り値があるかどうか
// 配列の各要素で処理して新しい配列を生成する
// ※１．index, array, thisArgは省略可能
// ※２．thisArgはコールバック関数内でthisとして扱う値

/* 使い方 */
const arr = [1,2,3,4].map(function(currentValue, index, array) {
  console.log(currentValue, index, array);
  return currentValue * 2;
}, thisArg);
console.log('arr', arr);

/* 結果 */
// (1, 0, [1,2,3,4])
// (2, 1, [1,2,3,4])
// (3, 2, [1,2,3,4])
// (4, 3, [1,2,3,4])
// (arr, [2,4,6,8])
```

### Array.prototype.reduce
[詳しくはこちら](https://developer.mozilla.org/ja/docs/Web/JavaScript/Reference/Global_Objects/Array/reduce)
```js
/* 概要 */
// アキュムレータと配列の各要素に対して（左から右へ）関数を適用し、単一の値を返す
// ※１．アキュムレータ：コールバックの戻り値を累積
// ※２．currentIndex, array, initialValueは省略可

/* 使い方 */
const initialValue = 10;
const res = [1,2,3,4,5].reduce(function(accumulator, currentValue, currentIndex, array) {
  console.log(accumulator, currentValue, currentIndex, array);
  return accumulator + currentValue;
}, initialValue);
console.log(res);

/* 結果 */
// (10,1,0,[1,2,3,4,5])
// (11,2,1,[1,2,3,4,5])
// (13,3,2,[1,2,3,4,5])
// (16,4,3,[1,2,3,4,5])
// (20,5,4,[1,2,3,4,5])
// 25
```

### Array.prototype.reverse
[詳しくはこちら](https://developer.mozilla.org/ja/docs/Web/JavaScript/Reference/Global_Objects/Array/reverse)
```js
/* 概要 */
// 配列の要素を反転させる

/* 使い方 */
console.log(['one', 'two', 'three'].reverse());

/* 結果 */
// (['three','two','one'])
```

### Array.prototype.slice
[詳しくはこちら](https://developer.mozilla.org/ja/docs/Web/JavaScript/Reference/Global_Objects/Array/slice)
```js
/* 概要 */
// 配列の一部を取り出して新しい配列を返す
// ※１．第2引数は省略可、その場合は最後まで
// ※２．第2引数は直前まで対象

/* 使い方 */
console.log([1,2,3,4,5,6].slice(2));
console.log([1,2,3,4,5,6].slice(0, 4));

/* 結果 */
// ([3,4,5,6])
// ([1,2,3,4])
```

### Array.prototype.sort
[詳しくはこちら](https://developer.mozilla.org/ja/docs/Web/JavaScript/Reference/Global_Objects/Array/sort)
```js
/* 概要 */
// 配列の要素を並び替える
// ※１．引数(コールバック関数)の戻り値によって並び順が決まる
// 　- コールバック関数(a, b)が0未満の場合、aをbより小さい添字にソート
// 　- コールバック関数(a, b)が0を返す場合、aとbは互いに変更せず、他のすべての要素に対してソート
// 　- コールバック関数(a, b)が0より大きい場合、bをaより小さい添字にソート
// ※２．引数(コールバック関数)は省略可

/* 使い方 */
const res = [1, 30, 4, 21].sort(function(a, b) {
  console.log(a, b)
  return a - b;
});
console.log(res);

/* 結果 */
// 1 30
// 30 4
// 1 4
// 30 21
// 4 21
// ([1,4,21,30])
```
