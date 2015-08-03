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






