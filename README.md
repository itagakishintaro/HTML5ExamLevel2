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

### call/apply

call/applyによって、thisが指すオブジェクトを変更できる

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

### プロトタイプ

各インスタンスで共通のメソッドはprototypeプロパティに登録する

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

## WebブラウザにおけるJavaScript API
## イベント ★★★★★★★★ 8
## ドキュメントオブジェクト／DOM ★★★★★★ 6
## ウィンドウオブジェクト ★★★★★★★★ 8
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






