# jQuery・JavaScriptメモ
サイズ取得や実行タイミングなど微妙な差異による不具合がちらほらあったのでまとめました。

## DOM要素のサイズ取得
### JavaScript
※要素にdisplay:noneが当たっている場合は0となる
#### 高さ
* `clientHeight`　paddingを含んだ高さ
* `scrollHeight`　paddingを含んだ画面上に表示されていないコンテンツを含む高さ
* `offsetHeight`　border、padding、スクロールバーを含んだ高さ

#### 横幅
* `clientWidth`　paddingを含んだ幅
* `scrollWidth`　paddingを含んだ画面上に表示されていないコンテンツを含む幅
* `offsetWidth`　border、padding、スクロールバーを含んだ幅

### jQuery
※要素にdisplay:noneが当たっていても値を取得できる
#### 高さ
* `height()`			要素の高さのみ
* `innerHeight()`		要素のpaddingを含んだ高さ
* `outerHeight()`		要素のborder、paddingを含んだ高さ
* `outerHeight(true)`	要素のmargin、border、paddingを含んだ高さ

#### 横幅
* `width()`				要素の幅のみ
* `innerWidth()`		要素のpaddingを含んだ幅
* `outerWidth()`		要素のborder、paddingを含んだ幅
* `outerWidth(true)`	要素のmargin、border、paddingを含んだ幅

## 実行タイミング
### JavaScript
* **`document.addEventListener('DOMContentLoaded', function())`	HTMLが読み込まれた時**
* **`window.onload = function()`	画像などコンテンツ全体が読み込まれた時**

### jQuery
* **`$(document).ready`	HTMLが読み込まれた時**
```javascript
$(function(){ /*処理*/ });

jQuery(function(){ /*処理*/ });

$(document).ready(function(){ /*処理*/ });

jQuery(document).ready(function(){ /*処理*/ });
```
* **`$(window).load`		画像などコンテンツ全体が読み込まれた時**
```javascript
$(window).load

jQuery(window).load
```

#### 処理の順番
1. ページの読み込みが始まる
1. HTMLの読み込みが終わる
1. $(document).readyが実行
1. 画像など含めすべてのコンテンツの読み込みが終わる
1. $(window).loadが実行

## イベントのバブリング (伝播) について
クリックなどのイベントは子要素から親要素へと伝播される（バブリング）ため、意図しない動作をする可能性があるので注意が必要

* **`event.preventDefault()`	その要素のイベントをキャンセル**
* **`event.stopPropagation()`	親要素への伝播をキャンセル**
* **`return false;`	その要素のイベントも親要素への伝播も両方キャンセル**

aタグやbuttonタグにevent処理を独自に実装する場合は処理の終わりに`return false;`を付ける

##　不具合修正等で引っかかった点
### jQueryのonは追加式なので注意
重複を避ける場合はjavascriptのaddEventListenerを使うか、同時にoffを使う
### iOSの場合、スクロールでresizeイベントが発火
画面の横幅を比較するか、向きが変わったときのみ(orientationchangeイベント)resize処理を行う  
** ※orientationchangeイベントだとviewport変更時のresizeには反応しないので注意 **
### href取得の際はプロパティによって相対パスか絶対パスかが変わる
```html
<link rel="stylesheet" type="text/css" href="./css/style.css">
```
↑のhrefを取得するときに  
`document.getElementsByTagName('link')[0].getAttribute('href')`で取得すると結果は`./css/style.css`  
`document.styleSheets[0].href`で取得すると結果は`http://ドメイン/～/css/style.css`
## その他(雑多メモ)
### 要素に付与されているイベント確認方法
`$._data($('selector').get(0), "events")`
