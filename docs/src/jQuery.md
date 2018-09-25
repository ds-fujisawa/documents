# jQuery・JavaScriptメモ
サイズ取得や実行タイミングなど微妙な差異による不具合がちらほらあったのでまとめました。
<script src="https://code.jquery.com/jquery-3.2.1.min.js" integrity="sha256-hwg4gsxgFZhOsEEamdOYGBf13FyQuiTwlAQgxVSNgt4=" crossorigin="anonymous"></script>

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

**デモ**

<div>
	<div id="parent">
		<div id="child"></div>
	</div>
</div>

|id:parent(ピンク)|id:child(水色)|
|:--|:--|
|display: inline-block;|width: 200px;|
|width: 400px;|height: 600px;|
|height: 400px;|padding: 10px;|
|padding: 10px;|margin: 10px;|
|margin: 10px;|border: 1px solid gray;|
|border: 1px solid gray;|background-color: skyblue;|
|background-color: pink;||
|overflow: hidden;||

<div class="js">
	<h1>JavaScript</h1>
	<div class="js-parent">
		<h2>parent(ピンク)</h2>
	</div>
	<div class="js-child">
		<h2>child(水色)</h2>
	</div>
</div>

<div class="jquery">
	<h1>jQuery</h1>
	<div class="jquery-parent">
		<h2>parent(ピンク)</h2>
	</div>
	<div class="jquery-child">
		<h2>child(水色)</h2>
	</div>
</div>

<script type="text/javascript">
	$(function () {
		$('.js-parent')
		.append('<p>clientHeight：'+document.getElementById('parent').clientHeight+'px</p>')
		.append('<p>scrooHeight：'+document.getElementById('parent').scrollHeight+'px</p>')
		.append('<p>offsetHeight：'+document.getElementById('parent').offsetHeight+'px</p>')
		$('.js-child')
		.append('<p>clientHeight：'+document.getElementById('child').clientHeight+'px</p>')
		.append('<p>scrooHeight：'+document.getElementById('child').scrollHeight+'px</p>')
		.append('<p>offsetHeight：'+document.getElementById('child').offsetHeight+'px</p>')
		$('.jquery-parent')
		.append('<p>height()：'+$('#parent').height()+'px</p>')
		.append('<p>innerHeight()：'+$('#parent').innerHeight()+'px</p>')
		.append('<p>outerHeight()：'+$('#parent').outerHeight()+'px</p>')
		.append('<p>outerHeight(true)'+$('#parent').outerHeight(true)+'px</p>');
		$('.jquery-child')
		.append('<p>height()：'+$('#child').height()+'px</p>')
		.append('<p>innerHeight()：'+$('#child').innerHeight()+'px</p>')
		.append('<p>outerHeight()：'+$('#child').outerHeight()+'px</p>')
		.append('<p>outerHeight(true)'+$('#child').outerHeight(true)+'px</p>');
	});
</script>

```js
$(function () {
	$('.js-parent')
	.append('<p>clientHeight：'+document.getElementById('parent').clientHeight+'px</p>')
	.append('<p>scrooHeight：'+document.getElementById('parent').scrollHeight+'px</p>')
	.append('<p>offsetHeight：'+document.getElementById('parent').offsetHeight+'px</p>')
	$('.js-child')
	.append('<p>clientHeight：'+document.getElementById('child').clientHeight+'px</p>')
	.append('<p>scrooHeight：'+document.getElementById('child').scrollHeight+'px</p>')
	.append('<p>offsetHeight：'+document.getElementById('child').offsetHeight+'px</p>')
	$('.jquery-parent')
	.append('<p>height()：'+$('#parent').height()+'px</p>')
	.append('<p>innerHeight()：'+$('#parent').innerHeight()+'px</p>')
	.append('<p>outerHeight()：'+$('#parent').outerHeight()+'px</p>')
	.append('<p>outerHeight(true)'+$('#parent').outerHeight(true)+'px</p>');
	$('.jquery-child')
	.append('<p>height()：'+$('#child').height()+'px</p>')
	.append('<p>innerHeight()：'+$('#child').innerHeight()+'px</p>')
	.append('<p>outerHeight()：'+$('#child').outerHeight()+'px</p>')
	.append('<p>outerHeight(true)'+$('#child').outerHeight(true)+'px</p>');
});
```

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
$(window).load(function(){ /*処理*/ });

jQuery(window).load(function(){ /*処理*/ });

$(window).on('load', function(){ /*処理*/ });

jQuery(window).on('load', function(){ /*処理*/ });
```
※jQuery3からはdocument-readyが常に非同期になっているので↓は×みたい  
(まだ試してはいない。。。)
```
$(function(){
  $(window).on('load', function(){ /*処理*/ });
   /*処理*/
});
```
↓のように分けないといけないみたい
```
$(function(){ /* 処理 */ });
$(window).on('load', function(){ /*処理*/ });
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

**[デモ]**

<div class="bubbling">
	<div class="none">
		<a href="?a=a">none</a>
	</div>
	<div class="preventDefault">
		<a href="?a=a">preventDefault</a>
	</div>
	<div class="stopPropagation">
		<a href="?a=a">stopPropagation</a>
	</div>
	<div class="return_false">
		<a href="?a=a">return_false</a>
	</div>
</div>

<script type="text/javascript">
	$(function() {
		$('.bubbling div').click(()=>{alert('hoge')});
		$('.none > a').click(()=>{alert('foo');});
		$('.preventDefault > a').click((e)=>{e.preventDefault();alert('foo');});
		$('.stopPropagation > a').click((e)=>{e.stopPropagation();alert('foo');});
		$('.return_false > a').click(()=>{alert('foo');return false;});
	});
</script>

|||
|:--|:--|
|none|alertでfoo、hogeが表示されurlに?a=aが付与|
|preventDefault|alertでfoo、hogeが表示|
|stopPropagation|alertでfooが表示されurlに?a=aが付与|
|return_false|alertでfooが表示|

```js
$(function() {
	$('.bubbling div').click(()=>{alert('hoge')});
	$('.none > a').click(()=>{alert('foo');});
	$('.preventDefault > a').click((e)=>{e.preventDefault();alert('foo');});
	$('.stopPropagation > a').click((e)=>{e.stopPropagation();alert('foo');});
	$('.return_false > a').click(()=>{alert('foo');return false;});
});
```

## 不具合修正等で引っかかった点
### jQueryのonは追加式なので注意
重複を避ける場合はjavascriptのaddEventListenerを使うか、同時にoffを使う
### iOSの場合、スクロールでresizeイベントが発火
画面の横幅を比較するか、向きが変わったときのみ(orientationchangeイベント)resize処理を行う  
**※orientationchangeイベントだとviewport変更時のresizeには反応しないので注意**
### href取得の際はプロパティによって相対パスか絶対パスかが変わる
```html
<link rel="stylesheet" type="text/css" href="./css/style.css">
```
↑のhrefを取得するときに  
`document.getElementsByTagName('link')[0].getAttribute('href')`で取得すると結果は`./css/style.css`  
`document.styleSheets[0].href`で取得すると結果は`http://ドメイン/～/css/style.css`

**デモ**

<p class="attr-txt"></p>
<p class="href-txt"></p>
<script>
	$(function(){
		$('.attr-txt').text(document.getElementsByTagName('link')[0].getAttribute('href'));
		$('.href-txt').text(document.styleSheets[0].href);
	});
</script>

```js
$(function(){
	$('.attr-txt').text(document.getElementsByTagName('link')[0].getAttribute('href'));
	$('.href-txt').text(document.styleSheets[0].href);
});
```

## その他(雑多メモ)
### 要素に付与されているイベント確認方法
`$._data($('selector').get(0), "events")`

### フリック操作

<div id="flickscroll">
	<ul>
		<li style="background-color: aqua;"></li>
		<li style="background-color: blue;"></li>
		<li style="background-color: skyblue;"></li>
		<li style="background-color: aquamarine;"></li>
	</ul>
</div>
<script type="text/javascript">
	$(function() {
		var $setMainId = $('#flickscroll');
		var scrollSpeed = 300;

		var $setMainUl = $setMainId.children('ul'),
		listWidth = parseInt($setMainUl.children('li').css('width')),
		listCount = $setMainUl.children('li').length,
		leftMax = -((listWidth)*((listCount)-1));

		$setMainUl.each(function() {
			$(this).css({width: listWidth * listCount + 100});
		});

		var isTouch = ('ontouchstart' in window);
		$setMainUl.bind({
			'touchstart mousedown': function(e) {
				var $setMainUlNot = $setMainId.children('ul:not(:animated)');
				$setMainUlNot.each(function() {
					e.preventDefault();
					this.pageX = (isTouch ? event.changedTouches[0].pageX : e.pageX);
					this.leftBegin = parseInt($(this).css('left'));
					this.left = parseInt($(this).css('left'));
					this.touched = true;
				});
			},
			'touchmove mousemove': function(e) {
				if(!this.touched) return;
				e.preventDefault();
				this.left = this.left - (this.pageX - (isTouch ? event.changedTouches[0].pageX : e.pageX));
				this.pageX = (isTouch ? event.changedTouches[0].pageX : e.pageX);

				if (this.left < 0 && this.left > leftMax) {
					$(this).css({left:this.left});
				}
				else if (this.left >= 0) {
					$(this).css({left:'0'});
				}
				else if (this.left <= leftMax) {
					$(this).css({left:(leftMax)});
				}
			},
			'touchend mouseup mouseout': function(e) {
				if (!this.touched) return;
				this.touched = false;

				if (this.leftBegin > this.left && (!((this.leftBegin) === (leftMax)))) {
					$(this).stop().animate({left:((this.leftBegin)-(listWidth))},scrollSpeed);
				}
				else if (this.leftBegin < this.left && (!((this.leftBegin) === 0))) {
					$(this).stop().animate({left:((this.leftBegin)+(listWidth))},scrollSpeed);
				}
				else if (this.leftBegin === 0) {
					$(this).css({left:'0'});
				}
				else if (this.leftBegin <= leftMax) {
					$(this).css({left:(leftMax)});
				}
			}
		});
	});
</script>

```js
$(function() {
	var $setMainId = $('#flickscroll');
	var scrollSpeed = 300;

	var $setMainUl = $setMainId.children('ul'),
	listWidth = parseInt($setMainUl.children('li').css('width')),
	listCount = $setMainUl.children('li').length,
	leftMax = -((listWidth)*((listCount)-1));

	$setMainUl.each(function() {
		$(this).css({width:(listWidth)*(listCount)});
	});

	var isTouch = ('ontouchstart' in window);
	$setMainUl.bind({
		'touchstart mousedown': function(e) {
			var $setMainUlNot = $setMainId.children('ul:not(:animated)');
			$setMainUlNot.each(function() {
				e.preventDefault();
				this.pageX = (isTouch ? event.changedTouches[0].pageX : e.pageX);
				this.leftBegin = parseInt($(this).css('left'));
				this.left = parseInt($(this).css('left'));
				this.touched = true;
			});
		},
		'touchmove mousemove': function(e) {
			if(!this.touched) return;
			e.preventDefault();
			this.left = this.left - (this.pageX - (isTouch ? event.changedTouches[0].pageX : e.pageX));
			this.pageX = (isTouch ? event.changedTouches[0].pageX : e.pageX);
				if (this.left < 0 && this.left > leftMax) {
				$(this).css({left:this.left});
			}
			else if (this.left >= 0) {
				$(this).css({left:'0'});
			}
			else if (this.left <= leftMax) {
				$(this).css({left:(leftMax)});
			}
		},
		'touchend mouseup mouseout': function(e) {
			if (!this.touched) return;
			this.touched = false;
				if (this.leftBegin > this.left && (!((this.leftBegin) === (leftMax)))) {
				$(this).stop().animate({left:((this.leftBegin)-(listWidth))},scrollSpeed);
			}
			else if (this.leftBegin < this.left && (!((this.leftBegin) === 0))) {
				$(this).stop().animate({left:((this.leftBegin)+(listWidth))},scrollSpeed);
			}
			else if (this.leftBegin === 0) {
				$(this).css({left:'0'});
			}
			else if (this.leftBegin <= leftMax) {
				$(this).css({left:(leftMax)});
			}
		}
	});
});
```

### 角度計算

<div class="rect">
	<span id="target"></span>
</div>
<p>x：<span id="x"></span></p>
<p>y：<span id="y"></span></p>
<p>deg：<span id="deg"></span></p>
<script>
	const setPosition = angle => {
		const x1 = 250;
		const y1 = 150;
		const r = Math.floor(Math.sqrt(Math.pow(x1, 2) + Math.pow(y1, 2)));
		const a = angle - 90 ;

		let x2 = x1 + r * Math.cos( a * (Math.PI / 180) ) ;
		let y2 = y1 + r * Math.sin( a * (Math.PI / 180) ) ;

		x2 = (x2 < 0) ? 0 : (x2 > x1*2) ? x1*2 : x2;
		y2 = (y2 < 0) ? 0 : (y2 > y1*2) ? y1*2 : y2;

		const element = document.getElementById("target") ;
		element.style.top = y2 + "px" ;
		element.style.left = x2 + "px" ;
		document.getElementById("x").innerHTML=Math.round(x2);
		document.getElementById("y").innerHTML=Math.round(y2);
		document.getElementById("deg").innerHTML=a;
	}

	let degree = 0 ;
	// setPosition( degree )

	setInterval(() => {
		setPosition( ++degree ) ;
	}, 100);
</script>

```js
const setPosition = angle => {
	const x1 = 250;
	const y1 = 150;
	const r = Math.floor(Math.sqrt(Math.pow(x1, 2) + Math.pow(y1, 2)));
	const a = angle - 90 ;

	let x2 = x1 + r * Math.cos( a * (Math.PI / 180) ) ;
	let y2 = y1 + r * Math.sin( a * (Math.PI / 180) ) ;

	x2 = (x2 < 0) ? 0 : (x2 > x1*2) ? x1*2 : x2;
	y2 = (y2 < 0) ? 0 : (y2 > y1*2) ? y1*2 : y2;

	const element = document.getElementById("target") ;
	element.style.top = y2 + "px" ;
	element.style.left = x2 + "px" ;
	document.getElementById("x").innerHTML=Math.round(x2);
	document.getElementById("y").innerHTML=Math.round(y2);
	document.getElementById("deg").innerHTML=a;
}

let degree = 0 ;
// setPosition( degree )

setInterval(() => {
	setPosition( ++degree ) ;
}, 100);
```
