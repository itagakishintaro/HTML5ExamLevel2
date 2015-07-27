# JavaScript

参考資料:http://html5exam.jp/images/news/event_20141115_01.pdf

## JavaScript文法 ★★★★★★★★★★ 10

### プリミティブ型とオブジェクト型

* プリミティブ型は値そのものを比較する
* オブジェクト型は参照しているオブジェクトが同じものかどうかを比較する

```
console.log( "hoge" === "hoge" ); // true
console.log( new String("hoge") == new String("hoge") ); // false
console.log( "hoge" == new String("hoge") ); // true
console.log( "hoge" === new String("hoge") ); // false
```

### NaN

* Not a Number
* NaNもNumber型
* NaNの判定はisNaN()

```
console.log(isNaN(0)); // false 0
console.log(isNaN('0')); // false 0
console.log(isNaN('')); // false 0
console.log(isNaN('hoge')); // true
console.log(isNaN(undefined)); // true
console.log(isNaN(null)); // false 0
```

## ラジアン

360度 = 2 * Math.PI 

## with

```
with(Math) {
	var r = 90;
  var x = round( sin(r / 180 * PI) * 10 ) / 10;
}
```

### call/apply

call/applyによって、thisが指すオブジェクトを変更できる

#### call

```
var obj1 = {
	name: "hoge",
	func: function() {
		console.log(this.name); // fuga
	}
};
var obj2 = {
	name: "fuga"
};
obj1.func.call(obj2); //obj1.func()を、obj2をthisとして実行
```

#### callとapplyの違い

call()はカンマ区切り、apply()は配列でメソッドの引数を渡す

```
var obj1 = {
  name: "わたし",
  showName: function(msg1, msg2) {
    console.log(this.name + msg1 + msg2);
  }
};
var obj2 = {
  name: "ぼく"
};
obj1.showName.call(obj2, "こんにちは", "よろしくね");
obj1.showName.apply(obj2, ["こんにちは", "よろしくね"]);
```

#### call/applyの利用シーン

配列のような構造だがArrayオブジェクトではないオブジェクトから、Arrayオブジェクトが持つjoinメソッドを利用する例

```
var obj= {
  "0": "aaa",
  "1": "bbb",
  "2": "ccc",
  length: 3
};
// Arrayオブジェクトのjoinメソッドを借りる
var str= Array.prototype.join.call(obj, "+");
```

### ホイスティング（変数の巻き上げ）

* 関数の途中で宣⾔された変数は、関数の先頭に巻き上げて宣言される
* ただし、初期値として代⼊されている値は巻き上げない

```
var a = "global";
function func() {
	console.log(a);
	var a = "func";
}
func();
```

```
var a = "global";
function func() {
	var a;
	console.log(a);
	a = "func";
}
func();
```

### クロージャ

クロージャは、変数を参照し続けることで、状態を保持する仕組み

```
var func = (function() {
	var a = 0;
	return function() {
		console.log(++a);
	};
})();
func(); // 1
func(); // 2
```

### クラスとコンストラクタ

インスタンス＝Objectオブジェクト
プロトタイプオブジェクトはFunction型だが、インスタンスはObject型となる

```
var Dog = function(_name) {
// 省略
};
var dog1 = new Dog("ポチ");
console.log(typeofDog); // function
console.log(typeofdog1);// object
```

### プロトタイプ

各インスタンスで共通のメソッドはprototypeプロパティに登録する
(各インスタンスが内部的に持つ__proto__プロパティから元のオブジェクトのprototypeプロパティを参照する)

```
var Dog = function(_name) {
	this.name = _name;
//	this.show = function() {
//		alert(this.name);
//	};
};
Dog.prototype.show = function() {
	alert(this.name);
};
```

継承

```
var MyClass3 = function(){};
MyClass3.prototype = new MyClass();
```

# WebブラウザにおけるJavaScript API

## イベント ★★★★★★★★ 8

### イベントハンドラ

`<button onclick='func();'>`のように使う。

#### フォーム

* onfocus
* onblur
* onselect
* onchange
* onformchange
* onforminput
* oninput
* onsubmit
* oncontextmenu
* oninvalic

#### キーボード

* onkeydown
* onkeyup
* onkeypress

#### マウス

* onclick
* ondblclick
* onmousedown
* onmouseup
* onmousemove
* onmouseover
* onmouseout
* onmousewheel
* onscroll

#### ドラッグ&ドロップ

* ondrag
* ondragstart
* ondragend
* ondragenter
* ondragleave
* ondragover
* ondragdrop

### イベントハンドラ

`document.getElementById('hoge').addEventListener('event', func());`のように使う。

#### キーボード

* input
* keydown
* keyup
* keypress

#### マウス

* mousedown
* mouseup
* mousemove
* mouseover
* mouseout
* mousewheel
* scroll
* click

#### ドラッグ&ドロップ

* drag
* dragstart
* dragend
* dragenter
* dragleave
* dragover
* dragdrop

#### タッチ

`e.touches[i].clientX`のように処理。
iが指の何本目か、clientX, clientYでxy座標をとる。

* touchmove
* touchstart
* touchend

```
x.addEventListener('touchmove', function(e){
	e.preventDefault(); // 画面上をスライドしてもブラウザが移動しないように
	var rect = e.target.getBoundingClientRect(); // 現在位置
	var xMove = e.touches[0].clientX - rect.left;
  var yMove = e.touches[0].clientY - rect.top;
});
```

#### 加速度

```
window.addEventListener('devicemotion', function(e){
	var x = e.accelerationIncludingGravity.x;
	var y = e.accelerationIncludingGravity.y;
	var z = e.accelerationIncludingGravity.z;
});
```

#### 回転

```
window.addEventListener('orientationchange', function(){
	console.log( window.orientation );
});
```

### バリデーション

* required：<input type="text" required>
* pattern：<input type="text" pattern="^[0-9]{3}-[0-9]{4}$">
* min/max<input type="number" min="18" max="99">
* maxlength：<input type="text" maxlength="6">

```
x.addEventListener('invalid', func);
```

## ドキュメントオブジェクト／DOM ★★★★★★ 6

### セレクタ

* document.getElementById()
* document.getElementsByTagName()
* document.getElementsByClassname()

### スタイル

ハイフンをCamelCaseにかえるのがポイント

```
elm.style.fontSize = '100px';
```

### 要素の親および子要素の取得

* parentNode
* previousSibling
* nextSibling
* firstChild
* lastChild

### 要素の表示、非表示制御

* visibility = 'visible' / 'hidden'
* display = 'block' / 'none'

### 要素の上書き

* elm.innerHTML = '<tag>hoge</tag>;
* elm.innerText = 'hoge';

### 要素の挿入、削除

* createElement()
* insertBefore()
* appendChild()

### 属性の追加、取得、削除

* createAttribute()
* hasAttribute()
* removeAttribute()
* setAttributeNode()

### フォームのデータにアクセスおよび、入力値の検証

```
<form action="..." onsubmit="return check();" method="post"></form>
```

## ウィンドウオブジェクト ★★★★★★★★ 8

### プロパティ

#### ウィンドウ

* .name
* .closed
* .innerHeight
* .innerWidth
* .outerHeight
* .outerWidth
* .pageXOffset
* .pageYOffset

#### 親子関係

* .parent
* .top
* .self
* .opener

#### 画面

* .screen
* .screenLeft
* .screenTop
* .screenX
* .screenY

#### ブラウザ

* .navigator
* .history
* .location
* .document
* .status
* .defaultStatus

#### フレーム

* .frames
* .length

#### ストレージ

* .localStorage
* .sessionStorage

### メソッド

#### ウィンドウ操作

* open()
* close()
* resizeBy()
* resizeTo()

#### 表示関係

* alert()
* confirm()
* createPopup()
* prompt()
* print()

#### 移動関係

* moveBy()
* moveTo()
* scrollBy()
* scrollTo()

#### フォーカス

* focus()
* blur()

#### タイマー

* setInterval()
* clearInterval()
* setTimeout()
* clearTimeout()

### postMessageを利用したメッセージ送信とイベント処理

window間で、安全にクロスドメイン通信できる。

```送信側
var otherWindow = window.open(url, 'post', 'target=_blank');
otherWindow.postMessage(message, url);
```

```受信側
window.addEventListener('message', function(e){
	console.log(e.data);
});

```

## Selectors API ★★★★ 4
## テスト・デバッグ ★★ 2

# グラフィックス
## Canvas(2D) ★★★★★★ 6
## SVG ★ 1

# マルチメディア
## video要素,audio要素 ★★ 2

# オフラインアプリケーションAPI
## アプリケーションキャッシュの制御 ★★ 2

# Session History and Navigation
## History API ★★★ 3

# 表示制御
## Page Visibility ★★ 2
## Timing control for script-based animations ★★ 2

# ストレージ
## Web Storage ★★★★ 4
## Indexed Database API ★★ 2
## File API ★★ 2

# 通信
## WebSocket ★★ 2
## XMLHttpRequest ★★★★ 4

# Geolocation API
## Geolocation APIの基本と位置情報の取得 ★★ 2

# Web Workers
## 並列処理の記述 ★★★★ 4

# パフォーマンス
## Navigation Timing ★★★★ 4
## High Resolution Time ★ 1






