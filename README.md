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

### 円

```
<svg width="500" height="200">
  <circle cx="50" cy="50" r="50" fill="skyblue" stroke="black" stroke-width="1" />
</svg>
```

### 楕円

```
<svg width="500" height="200">
  <ellipse cx="50" cy="50" rx="50" ry="30" fill="skyblue" stroke="black" stroke-width="1" />
</svg>
```

### 直線

```
<svg width="500" height="200">
  <line x1="10" y1="30" x2="100" y2="300" stroke="black" stroke-width="1" />
</svg>
```

### 矩形

```
<svg width="500" height="200">
  <rect x="10" y="100" width="200" height="50" rx="5" ry="5" fill="none" stroke="blue" stroke-width="3" />
</svg>
```

### 文字

```
<svg width="500" height="200">
  <text x="10 30 50" y="40 30 50" font-size="30" fill="red" stroke="none">SVG</text>
</svg>
```

### 連続直線

```
<svg width="500" height="200">
  <polyline points="200,150,250,180,300,150,350,100" fill="none" stroke="red" stroke-width="2" />
</svg>
```

### 多角形

```
<svg width="500" height="200">
  <polygon points="200,150,250,180,300,150,350,100" fill="pink" stroke="red" stroke-width="2" />
</svg>
```

### 変形

* (100, 50)を中心に30度回転
* (100, 30)移動

```
<svg width="500" height="200">
  <rect x="10" y="100" width="200" height="50" rx="5" ry="5" fill="none" stroke="blue" stroke-width="3" transform="rotate(30, 100,50)" />
  <rect x="10" y="100" width="200" height="50" rx="5" ry="5" fill="none" stroke="blue" stroke-width="3" transform="translate(100,30)" />
</svg>
```

### グラデーション

```
<svg width="500" height="200">
  <defs>
    <linearGradient id="gradient1">
      <stop offset="0%" stop-color="white" />
      <stop offset="100%" stop-color="gray" />
    </linearGradient>
    <radialGradient id="gradient2">
      <stop offset="0%" stop-color="white" />
      <stop offset="100%" stop-color="gray" />
    </radialGradient>
  </defs>
  
  <circle fill="url(#gradient1)" cx="100" cy="100" r="100" stroke="black" stroke-width="0" />
    <circle fill="url(#gradient2)" cx="250" cy="100" r="100" stroke="black" stroke-width="0" />
</svg>
```

### グループ化

```
<svg width="500" height="200">
  <defs>
    <linearGradient id="gradient1">
      <stop offset="0%" stop-color="white" />
      <stop offset="100%" stop-color="gray" />
    </linearGradient>
    <radialGradient id="gradient2">
      <stop offset="0%" stop-color="white" />
      <stop offset="100%" stop-color="gray" />
    </radialGradient>
  </defs>
  
  <g transform="rotate(20, 200, 100)">
    <circle fill="url(#gradient1)" cx="100" cy="100" r="100" stroke="black" stroke-width="0" />
    <circle fill="url(#gradient2)" cx="250" cy="100" r="100" stroke="black" stroke-width="0" />
  </g>
</svg>
```

### アニメーション

```
<svg width="500" height="200">
  <circle cx="100" cy="100" r="100" fill="blue" stroke="black" stroke-width="0">
    <animate
      attributeType="xml"
      attributeName="cx"
      begin="0s"
      dur="5s"
      values="0;400;0"
      repeatCount="infinite"
    />
  </circle>
</svg>
```

# マルチメディア
## video要素,audio要素 ★★ 2

### Audio API

```
var audio = new Audio;
audio.src = "audio.mp3";
```

### Video API

```
var video = document.getElementById("video");
video.src = "video.m4v";
```

### メソッド

* play()
* pause()
* load()

### プロパティ

* autoplay
* controls
* currenttime: 再生時間
* duradion: メディアの長さ
* ended
* error
* loop
* networkState(0:NETWORK_EMPTY, 1:NETWORK_IDLE, 2:NETWORK_LOADING, 3:NETWORK_NO_SOURCE)
* paused
* playbackRate
* played
* preload
* readyState(0:HAVE_NOTHING, 1:HAVE_METADATA, 2:HAVE_CURRENT_DATA, 3:HAVE_FUTURE_DATA, 4:HAVE_ENOUGH_DATA)
* volume

### イベントハンドラ

* onplay
* onplaying
* ontime
* onupdate
* onpause
* onwaiting
* onstalled
* onended
* onerror
* onabort

### Canvas上での動画表示

```
<video controls="" id="video" width="0" height="0" src="http://sites.lafayette.edu/newquisk/files/2011/08/ken-video.m4v"></video>
<canvas id="canvas" width="200" height="200"></canvas>
```


```
var video = document.getElementById('video');
var canvas = document.getElementById('canvas');

video.play();

setInterval(function(){
  canvas.getContext('2d').drawImage(video, 0, 0, 200, 200, 0, 0, 200, 200);
}, 500);
```

# オフラインアプリケーションAPI
## アプリケーションキャッシュの制御 ★★ 2

```
console.log(window.applicationCache);
```

### プロパティ(status)

* 0: UNCACHED
* 1: IDLE
* 2: CHECKING
* 3: DOWNLOADING
* 4: UPDATEREADY
* 5: OBSOLETE

### メソッド

* update()
* swapCache()

### イベント

* checking
* error
* noupdate
* downloading
* progress
* updateready
* cached
* obsolete

### Navigatorオブジェクト

* window.navigator.onLine

# Session History and Navigation
## History API ★★★ 3

### Historyオブジェクト

* history.length()
* history.back()
* history.forward()
* history.go(N)
* history.pushState(state, title, URL)
* history.replaceState(state, title, URL)

### Locationオブジェクト

#### プロパティ

https://developer.mozilla.org/ja/docs/Web/API/Window/location

例) http://www.google.com:80/search?q=devmo#test

プロパティ|説明|例
---|---|---
hash|# 記号に続くURL の部分。|#test
host|ホスト名とポート番号。|www.google.com:80
hostname|ホスト名（ポート番号を含まない）。|www.google.com
href|完全な URL。|http://www.google.com:80/search?q=devmo#test
pathname|パス（ホストからの相対）。|/search
port|URL のポート番号。|80
protocol|URL のプロトコル|http:
search|? 記号に続く URL の部分。? 記号も含みます。|?q=devmo

#### メソッド

* assign()
* replace()
* reload()
* toString()

# 表示制御
## Page Visibility ★★ 2

### プロパティ

* document.hidden
* visibilityState
  - visible
  - hidden
  - prerender

### イベント

* visibilitychange

## Timing control for script-based animations ★★ 2

ブラウザの負荷状況によって最適なタイミングで開始。
ページがバックグラウンドにあると速度が低くなる。

* Window.requestAnimationFrame()
* window.cancelAnimationFrame()

# ストレージ
## Web Storage ★★★★ 4

### localStorageオブジェクト,sessionStorageオブジェクト

localStorageはブラウザを閉じても残る。sessionStorageはブラウザを閉じると消える。

### メソッド

* key(n)
* setItem(key, value)
* getItem(key)
* removeItem(key)
* clear()

### StorageEventオブジェクト

Storageを更新した場合、Storageを更新したドキュメント以外の、同じStorage領域を持ってるドキュメントでStorageEventが発火する。

```
window.addEventListener('storage', function(e) {
  console.log(e);
}, false);
```

#### イベントのプロパティ

* key
* oldValue
* newValue
* url
* storageArea

## Indexed Database API ★★ 2

### IDBFactoryオブジェクト

```
var indexedDB = window.indexedDB;
var req = indexedDB.open('mydb', 1.0);
indexedDB.deleteDatabase('mydb');
```

### IDBRequestオブジェクト

```
var db;
var indexedDB = window.indexedDB;
var req = indexedDB.open('mydb', 1.0);

// DB変更時
req.onupgradeneeded = function(e){
  db = e.target.result;
  var store = db.createObjectStore('mystore', {keyPath: 'mykey'});
  store.createIndex('myvalueIndex', 'myvalue'); // インデックス生成
}

// DBオープン成功時
req.onsuccess = function(e){
  db = e.target.result;
}

// トランザクション
var transaction = db.transaction(['mystore'], 'readwrite');
var store = transaction.objetStore('mystore');

// 登録・更新
var request = store.put({ mykey: key, myvalue: value });
request.onsuccess = function(){ console.log('success'); }

// 参照
var request = store.get(key);
request.onsuccess = function(e){ console.log(e.target.result); }

// 削除
var request = store.delete(key);
request.onsuccess = function(){ console.log('deleted'); }
```

#### プロパティ

* result
* error
* source
* transaction
* readyState

#### イベント

* onsuccess
* onerror

### IDBDatabaseオブジェクト

objectStoreはテーブルのようなもの

```
// https://developer.mozilla.org/ja/docs/Web/API/IDBDatabase
  DBOpenRequest.onupgradeneeded = function(event) {
    var db = event.target.result;
    
    db.onerror = function(event) {
      note.innerHTML += '<li>Error loading database.</li>';
    };

    // Create an objectStore for this database using IDBDatabase.createObjectStore
    
    var objectStore = db.createObjectStore("toDoList", { keyPath: "taskTitle" });
    
    // define what data items the objectStore will contain
    
    objectStore.createIndex("hours", "hours", { unique: false });
    objectStore.createIndex("minutes", "minutes", { unique: false });
    objectStore.createIndex("day", "day", { unique: false });
    objectStore.createIndex("month", "month", { unique: false });
    objectStore.createIndex("year", "year", { unique: false });

    objectStore.createIndex("notified", "notified", { unique: false });
    
    note.innerHTML += '<li>Object store created.</li>';
```

#### プロパティ

* name
* version
* objectStoreNames

#### メソッド

* createObjectStore()
* deleteObjectStore()
* transaction()
* close()

## File API ★★ 2

参考:http://www.html5rocks.com/ja/tutorials/file/dndfiles/

```
<input type="file" id="files" name="files[]" multiple />
<output id="list"></output>

<script>
  function handleFileSelect(evt) {
    var files = evt.target.files; // FileList object

    // files is a FileList of File objects. List some properties.
    var output = [];
    for (var i = 0, f; f = files[i]; i++) {
      output.push('<li><strong>', escape(f.name), '</strong> (', f.type || 'n/a', ') - ',
                  f.size, ' bytes, last modified: ',
                  f.lastModifiedDate.toLocaleDateString(), '</li>');
    }
    document.getElementById('list').innerHTML = '<ul>' + output.join('') + '</ul>';
  }

  document.getElementById('files').addEventListener('change', handleFileSelect, false);
</script>
```

### FileListオブジェクト

* プロパティ: length
* メソッド: item()

### Blobオブジェクト

* プロパティ: size, type
* メソッド: slice(), close()

### Fileオブジェクト

* プロパティ: name, lastModifiedDate

### FileReaderオブジェクト

```
var reader = new FileReader();
reader.readAsText(file, 'utf-8');
reader.addEventListener('load', function(){
  console.log(reader.result);
});
```

* プロパティ: readyState, result, error
* メソッド: readAsArrayBuffer(), readAsText(), readAsDataURL(), abort()

# 通信
## WebSocket ★★ 2

```
var ws = new WebSocket('ws://hoge');
ws.onopen = function(e){ ws.send('message'); }
ws.onmessage = function(e){ console.log(e.data); }
//ws.close();
```

### メソッド

* send()
* close()

### プロパティ

* URL
* readyState
  - 0: CONNECTING
  - 1: OPEN
  - 2: CLOSING
  - 3: CLOSED
* bufferedAmount

### イベント

* onopen
* onmessage
* onclose
* onerror

## XMLHttpRequest ★★★★ 4

参考:https://developer.mozilla.org/ja/docs/Web/API/XMLHttpRequest

レベル1と2があり、レベル2ではクロスドメイン通信、バイナリデータ送受信、MIMEタイプ設定、進捗状況、タイムアウト指定ができる。

### XMLHttpRequest　オブジェクト

```
var xhr = new XMLHttpRequest();
xhr.open('GET', 'ajax.txt');
xhr.onreadystatechange = function(){
  if(xhr.readyState === 4){
    console.log(xhr.responseText);
  }
}
xhr.ontimeout = function () {
  console.error('timed out.');
};
```

* プロパティ: readyState
  - 0: 初期化されていない
  - 1: sendされていない
  - 2: send実行中
  - 3: 応答受信中
  - 4: 受信済み
* イベント: onreadystatechange

### リクエスト関連

#### メソッド

* open(method, url, [optional] boolean async, [optional] user, [optional] password)
* setRequestHeader(header, value)
* void send([optional] body)
* abort()

#### プロパティ

* timeout
* withCredentials: boolean
* upload: イベントリスナー

### レスポンス処理

#### メソッド

* getResponseHeader(header)
* getAllResponseHeaders()
* overrideMimeType(mimetype)

#### プロパティ

* status
* statusText
* responseType
* response
* responseText
* responseXML

### XMLHttpRequestEventTarget　Interface

#### イベント

* onloadstart
* onprogress
* onabort
* onerror
* onload
* ontimeout
* onloadend

# Geolocation API
## Geolocation APIの基本と位置情報の取得 ★★ 2

```
var geolocation = navigator.geolocation;
```

### Positionオブジェクト

```
var options = {
  enableHighAccuracy: true,
  timeout: 5000,
  maximumAge: 0 // キャッシュを保有するミリ秒
};
navigator.geolocation.getCurrentPosition(success, error, options);
```

### プロパティ（cords）

#### Coordinates オブジェクト

```
function success(position) {
  console.log(position.coords.latitude); // 緯度
  console.log(position.coords.longitude); // 経度
  console.log(position.coords.accuracy); // 緯度経度の精度
  console.log(position.coords.altitude); // 高度
  console.log(position.coords.altitudeAccuracy); // 高度の精度
  console.log(position.coords.heading); // 方向
  console.log(position.coords.speed); // 速度
}
```

### Geolocationオブジェクト

#### メソッド

* getCurrentPosition()
* watchPosition()
* clearWatch()

#### errorプロパティ

* error.code
* error.message

# Web Workers

## 並列処理の記述 ★★★★ 4

### Workerオブジェクト

参考:http://www.html5rocks.com/ja/tutorials/workers/basics/

#### メソッド

```
var worker = new Worker('task.js');
worker.postMessage('message');
worker.terminate();
```

#### イベント

```
worker.onerror = function(e) {
  console.log(e.message);
  console.log(e.filename);
  console.log(e.lineno);
}

worker.onmessage = function(e){
  console.log(e.data);
}
```

# パフォーマンス

## Navigation Timing ★★★★ 4

### PerformanceTimingオブジェクト

```
var timing = window.performance.timing;
```

### プロパティ

* navigationStart
* unloadEventStart
* unloadEventEnd
* redirectStart
* redirectEnd
* fetchStart
* domainLookupStart
* domainLookupEnd
* connectStart
* connectEnd
* secureConnectionStart
* requestStart
* responseStart
* responseEnd
* domLoading
* domInteractive
* domContentLoadedEventStart
* domContentLoadedEventEnd
* domComplete
* loadEventStart
* loadEventEnd

### PerformanceNavigationオブジェクト

```
var type = window.performance.navigation.type;
```

* 0: TYPE_NAVIGATE
* 1: TYPE_RELOAD
* 2: TYPE_BACK_FORWARD
* 255: TYPE_RESERVED

## High Resolution Time ★ 1

ページを開いた時刻からnow()を起動したときまでの時刻をナノ秒の高精度で取得できる。

```
window.performance.now();
```
