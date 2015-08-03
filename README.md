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

* querySelector()
* querySelectorAll()

## テスト・デバッグ ★★ 2

* console.assert(assertion, errorMesage)
* console.log()
* console.debug()
* console.dirxm()
* console.error()
* console.info()
* console.profile()
* console.profileEnd()
* console.trace()
* console.warn()

# グラフィックス
## Canvas(2D) ★★★★★★ 6

### コンテキスト

```
// <canvas id="canvas" width="500" height="300"></canvas>
var canvas = document.getElementById('canvas');
var ctx = canvas.getContext('2d');
```

### 矩形

```
// 塗りつぶし
ctx.fillStyle = 'red';
ctx.fillRect(10, 10, 50, 50); // x, y, 幅, 高さ
// 消去
ctx.clearRect(40, 40, 50, 50);
// 枠線
ctx.strokeStyle = 'skyblue';
ctx.strokeRect(10, 10, 90, 90);
```

### 多角形

```
ctx.beginPath();
ctx.moveTo(110, 10); // 開始点
ctx.lineTo(150, 90);
ctx.lineTo(190, 50);
ctx.closePath();
ctx.fillStyle = 'green';
ctx.fill();
```

### 直線

```
ctx.beginPath();
ctx.moveTo(110, 10); // 開始点
ctx.lineTo(150, 90);
ctx.lineTo(190, 50);
ctx.lineWidth = 10;
ctx.strokeStyle = 'green';
ctx.stroke();
```

### 円弧

```
var beginAngle = 40 / 180 * Math.PI;
var endAngle = 300 / 180 * Math.PI;
ctx.beginPath();
// 中心x座標、中心y座標、半径、開始角度(時計回り)、終了角度(時計回り)、回転方向(true: 反時計回り, false: 時計回り)
ctx.arc(100, 50, 40, beginAngle, endAngle, false);
ctx.strokeStyle = 'yellow';
ctx.lineWidth = 10;
ctx.stroke();
```

### 円

```
ctx.beginPath();
ctx.arc(100, 50, 40, 0, 2 * Math.PI, false);
ctx.closePath();
ctx.fillStyle="pink";
ctx.fill();
```

### 2次曲線

```
ctx.beginPath();
ctx.moveTo(10, 150);　// 開始点
// 制御点のx座標、y座標、終点のx座標、y座標
ctx.quadraticCurveTo(100, 100, 190, 150);
ctx.strokeStyle = 'lightgreen';
ctx.lineCap = 'round'; // 線の開始点、終了点のフチを丸くする
ctx.lineWidth = 10;
ctx.stroke();
```

### 3次曲線

```
ctx.beginPath();
ctx.moveTo(10, 150);　// 開始点
// 制御点1のx座標、y座標、制御点2のx座標、y座標、終点のx座標、y座標
ctx.bezierCurveTo(100, 100, 200, 200, 190, 150);
ctx.strokeStyle = 'lightgreen';
ctx.lineCap = 'round'; // 線の開始点、終了点のフチを丸くする
ctx.lineWidth = 10;
ctx.stroke();
```

### グラデーション

```
// グラデーション開始x, y, グラデーション終了x, y
var g = ctx.createLinearGradient(0, 0, 100, 100);
// 0: 開始, 1: 終了
g.addColorStop(0, 'red');
g.addColorStop(0.5, 'white');
g.addColorStop(1, 'blue');
ctx.fillStyle = g;
ctx.fillRect(0, 0, 100, 100);
```

### 文字列

```
ctx.fillStyle = 'red';
ctx.font = '16px, serif';
ctx.fillText('html5', 100, 50);

ctx.font = '20px, serif';
ctx.textAlign = 'right';
var measure = ctx.measureText('html5');
ctx.fillText(measure.width, 200, 100);
```

### 画像描画

```
var img = new Image();
img.src = "http://saqibsomal.com/wp-content/uploads/2015/06/Google-Logo.jpg";
img.onload = function(){
  // x, y
  ctx.drawImage(img, 0, 0);
  // x, y, width, height
  ctx.drawImage(img, 100, 100, 50, 50);
  // 元画像のx, yからwidth, heightを切り抜き、x, y, width, height
  ctx.drawImage(img, 0, 0, 100, 100, 200, 200, 50, 50);
};
```

### クリッピング

```
ctx.beginPath();
ctx.arc(50, 50, 50, 0, 2 * Math.PI, false);
ctx.clip();

var img = new Image();
img.src = "http://saqibsomal.com/wp-content/uploads/2015/06/Google-Logo.jpg";
img.onload = function(){
  ctx.drawImage(img, 0, 0, 100, 100);
};
```

### 変形

```
// 変形する前の状態を保存
ctx.save();

ctx.translate(100, 100);
ctx.rotate(30 / 180 * Math.PI);
ctx.scale(1, 0.5);
ctx.fillStyle = 'blue';
ctx.fillRect(-50, -50, 100, 100);

// 変形する前の状態を復元
ctx.restore();

var angle = 30 / 180 * Math.PI;
// 伸縮x, 傾斜y, 傾斜x, 伸縮y, 移動x, 移動y
ctx.setTransform(Math.cos(angle), Math.sin(angle), -Math.sin(angle), Math.cos(angle), 100, 200)
ctx.fillStyle = 'red';
ctx.fillRect(-50, -50, 100, 100);
```

### 透明度

```
ctx.beginPath();
ctx.arc(50, 50, 50, 0, 2 * Math.PI, false);
ctx.closePath();
ctx.globalAlpha = 0.5;
ctx.fillStyle="pink";
ctx.fill();

ctx.beginPath();
ctx.arc(100, 50, 50, 0, 2 * Math.PI, false);
ctx.closePath();
ctx.globalAlpha = 0.5;
ctx.fillStyle="green";
ctx.fill();
```

### 合成

souce: 前に書いた図
destination: 後に書いた図

* source-over
* destination-over
* source-in
* destination-in
* source-atop
* destination-atop
* source-out
* destination-out
* xor
* lighter
* copy

```
ctx.beginPath();
ctx.arc(50, 50, 50, 0, 2 * Math.PI, false);
ctx.closePath();
ctx.fillStyle="pink";
ctx.fill();

ctx.globalCompositeOperation = 'xor';

ctx.beginPath();
ctx.arc(100, 50, 50, 0, 2 * Math.PI, false);
ctx.closePath();
ctx.fillStyle="green";
ctx.fill();
```

### ピクセルデータ

```
var canvas1 = document.getElementById('canvas1');
var ctx1 = canvas1.getContext('2d');

ctx1.beginPath();
ctx1.arc(50, 50, 50, 0, 2 * Math.PI, false);
ctx1.closePath();
ctx1.fillStyle="pink";
ctx1.fill();
// 画像データのピクセルの値を配列で取得
var img1 = ctx1.getImageData(0, 0, 100, 100);

var canvas2 = document.getElementById('canvas2');
var ctx2 = canvas2.getContext('2d');
// 画像のオブジェクトを生成
var img2 = ctx2.createImageData(100, 100);
for(var i = 0; i < img2.width * img2.height * 4; i++) {
  img2.data[i] = img1.data[i];
}
// 描画
ctx2.putImageData(img2, 0, 0);
```

context.save(),restore()
context.beginPath()
context.rect(),clip()
context.moveTo(),context.lineTo(),context.stroke()
context.fillRect(),context.strokeRect(),context.clearRect()
context.arc(),context.arcTo(),context.bezierCurveTo(),context.quadraticCurveTo()
context.measureText()
context.fillText(),context.strokeText()
context.font
context.setTransform()
context.rotate()
context.scale()
context.translate()
context.globalAlpha
context.globalCompositeOperation
context.drawImage()
context.createImageData()

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






