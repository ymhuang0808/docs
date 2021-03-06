# Laravel Elixir

- [簡介](#introduction)
- [安裝](#installation)
- [使用方式](#usage)
- [Gulp](#gulp)
- [功能擴充](#extensions)

<a name="introduction"></a>
## 簡介

Laravel Elixir 提供了簡潔流暢的 API，讓你能夠為你的 Laravel 應用程式定義基本的 [Gulp](http://gulpjs.com) 任務。Elixir 支援許多常見的 CSS 與 JavaScrtip 預處理器，甚至包含了測試工具。

如果你曾經對於使用 Gulp 及編譯資源感到困惑，那麼你絕對會愛上 Laravel Elixir！

<a name="installation"></a>
## 安裝與設定

### 安裝 Node

在開始使用 Elixir 之前，你必須先確定你的開發環境上有安裝 Node.js。

    node -v

預設情況下，Laravel Homestead 會包含你所需的一切；當然，如果你沒有使用 Vagrant，那麼你可以瀏覽 [Node  的下載頁](http://nodejs.org/download/)進行安裝。別擔心，安裝是很簡單又快速的！

### Gulp

接著你需要全域安裝 [Gulp](http://gulpjs.com) 的 NPM 安裝包，像是這樣：

    npm install --global gulp

### Laravel Elixir

最後的步驟就是安裝 Elixir！伴隨著新安裝的 Laravel，你會發現根目錄有個名為 `package.json` 的檔案。想像它就如同你的 `composer.json` 檔案，只是它定義的是 Node 的依賴，而不是 PHP。你可以使用以下的指令進行安裝依賴的動作：

	npm install

<a name="usage"></a>
## 使用方式

現在你已經安裝好 Elixir，未來任何時候你都能進行編譯及合併檔案！專案根目錄的 `gulpfile.js` 包含你所有的 Elixir 任務。

#### 編譯 Less

```javascript
elixir(function(mix) {
	mix.less("app.less");
});
```

在上述例子中，Elixir 會假設你的 Less 檔案儲存在 `resources/assets/less` 裡。

#### 編譯多個 Less 檔案

```javascript
elixir(function(mix) {
	mix.less([
		'app.less',
		'something-else.less'
	]);
});
```

#### 編譯 Sass

```javascript
elixir(function(mix) {
	mix.sass("app.sass");
});
```

在上述例子中，Elixir 會假設你的 Sass 檔案儲存在 `resources/assets/sass` 裡。 The `sass` method may only be called once. If you would like to compile multiple Sass files, pass an array to the `sass` method.

By default, Elixir, underneath the hood, uses the LibSass library for compilation. In some instances, it might prove advantageous to instead leverage the Ruby version, which, though slower, is more feature rich. Assuming that you have both Ruby and the Sass gem installed (`gem install sass`), you may enable Ruby-mode, like so:

```javascript
elixir(function(mix) {
	mix.rubySass("app.sass");
});
```

#### Compile Without Source Maps

```javascript
elixir.config.sourcemaps = false;

elixir(function(mix) {
	mix.sass("app.scss");
});
```

Source maps are enabled out of the box. As such, for each file that is compiled, you'll find a companion `*.css.map` file in the same directory. This mapping allows you to, when debugging, trace your compiled stylesheet selectors  back to your original Sass or Less partials! Should you need to disable this functionality, however, the code sample above will do the trick.

#### 編譯 CoffeeScript

```javascript
elixir(function(mix) {
	mix.coffee();
});
```

在上述例子中，Elixir 會假設你的 CoffeeScript 檔案儲存在 `resources/assets/coffee` 裡。

#### 編譯所有的 Less 及 CoffeeScript

```javascript
elixir(function(mix) {
    mix.less()
       .coffee();
});
```

#### 觸發 PHPUnit 測試

```javascript
elixir(function(mix) {
	mix.phpUnit();
});
```

#### 觸發 PHPSpec 測試

```javascript
elixir(function(mix) {
	mix.phpSpec();
});
```

#### 合併樣式檔案

```javascript
elixir(function(mix) {
	mix.styles([
		"normalize.css",
		"main.css"
	]);
});
```

傳遞給此方法的檔案路徑均相對於 `resources/css` 目錄。

#### 合併樣式檔案且儲存在自訂的路徑

```javascript
elixir(function(mix) {
	mix.styles([
		"normalize.css",
		"main.css"
	], 'public/build/css/everything.css');
});
```

#### 從特定相對目錄合併樣式檔案

```javascript
elixir(function(mix) {
	mix.styles([
		"normalize.css",
		"main.css"
	], 'public/build/css/everything.css', 'public/css');
});
```

`styles` 與 `scrtips` 方法可以透過傳入第三個參數來決定來源檔案的相對目錄。

#### 合併指定目錄裡所有的樣式檔案

```javascript
elixir(function(mix) {
	mix.stylesIn("public/css");
});
```

#### 合併腳本檔案

```javascript
elixir(function(mix) {
	mix.scripts([
		"jquery.js",
		"app.js"
	]);
});
```

同樣的，傳遞給此方法的檔案路徑均相對於 `resources/js` 目錄

#### 合併指定目錄裡所有的腳本檔案

```javascript
elixir(function(mix) {
	mix.scriptsIn("public/js/some/directory");
});
```

#### 合併多組腳本檔案

```javascript
elixir(function(mix) {
    mix.scripts(['jquery.js', 'main.js'], 'public/js/main.js')
       .scripts(['forum.js', 'threads.js'], 'public/js/forum.js');
});
```

#### 壓縮檔案並加上雜湊的版本號

```javascript
elixir(function(mix) {
	mix.version("css/all.css");
});
```

這個動作會為你的檔案名稱加上獨特的雜湊值，以防止檔案被快取。舉例來說，產生出來的檔案名稱可能像這樣：`all-16d570a7.css`。

接著在你的視圖中，你能夠使用 `elixir()` 函式來正確載入名稱被雜湊後的檔案。舉例如下：

```html
<link rel="stylesheet" href="{{ elixir("css/all.css") }}">
```

程式的作用下，`elixir()` 函式會將參數內的原始檔名轉換成被雜湊後的檔名並載入。是否有如釋重擔的感覺呢？

你也可以傳遞一個陣列給 `version` 方法來把多個檔案加上版本：

```javascript
elixir(function(mix) {
	mix.version(["css/all.css", "js/app.js"]);
});
```

```html
<link rel="stylesheet" href="{{ elixir("css/all.css") }}">
<script src="{{ elixir("js/app.js") }}"></script>
```

#### 複製檔案到新的位置

```javascript
elixir(function(mix) {
	mix.copy('vendor/foo/bar.css', 'public/css/bar.css');
});
```

#### 將整個目錄都複製到新的位置

```javascript
elixir(function(mix) {
	mix.copy('vendor/package/views', 'resources/views');
});
```

#### 方法連接

當然，你能夠串連 Elixir 大部份的方法來建立一連串的任務：

```javascript
elixir(function(mix) {
    mix.less("app.less")
       .coffee()
       .phpUnit()
       .version("css/bootstrap.css");
});
```

<a name="gulp"></a>
## Gulp

現在你已經告訴 Elixir 要執行的任務，接著只需要在命令列執行 Gulp。

#### 執行一次所有註冊的任務

	gulp

#### 監控檔案變更

	gulp watch

#### 只編譯腳本

	gulp scripts

#### 只編譯樣式

	gulp styles

#### 監控測試以及 PHP 類別的變更

	gulp tdd

> **提示：** 所有的任務都會使用開發環境進行，所以壓縮功能不會被執行。如果要使用上線環境，可以使用 `gulp --production`。

<a name="extensions"></a>
## Custom Tasks and Extensions

Sometimes, you'll want to hook your own Gulp tasks into Elixir. Perhaps you have a special bit of functionality that you'd like Elixir to mix and watch for you. No problem!

As an example, imagine that you have a general task that simply speaks a bit of text when called.

```javascript
gulp.task("speak", function() {
	var message = "Tea...Earl Grey...Hot";

	gulp.src("").pipe(shell("say " + message));
});
```

Easy enough. From the command line, you may, of course, call `gulp speak` to trigger the task. To add it to Elixir, however, use the `mix.task()` method:

```javascript
elixir(function(mix) {
    mix.task('speak');
});
```

That's it! Now, each time you run Gulp, your custom "speak" task will be executed alongside any other Elixir tasks that you've mixed in. To additionally register a watcher, so that your custom tasks will be re-triggered each time one or more files are modified, you may pass a regular expression as the second argument.

```javascript
elixir(function(mix) {
    mix.task('speak', 'app/**/*.php');
});
```

By adding this second argument, we've instructed Elixir to re-trigger the "speak" task each time a PHP file in the "app/" directory is saved.


For even more flexibility, you can create full Elixir extensions. Using the previous "speak" example, you may write an extension, like so:

```javascript
var gulp = require("gulp");
var shell = require("gulp-shell");
var elixir = require("laravel-elixir");

elixir.extend("speak", function(message) {

	gulp.task("speak", function() {
		gulp.src("").pipe(shell("say " + message));
	});

	return this.queueTask("speak");

 });
```

請注意我們 `擴增（extend）` Elixir 的 API 時所使用的第一個參數名稱，稍後我們需要在 Gulpfile 中參考到它，以及建立 Gulp 任務所使用的回呼函式。

跟前面一樣，如果你想要讓你的自訂任務能被監控，只要在註冊一個監控器就行了。

```javascript
this.registerWatcher("speak", "app/**/*.php");
```

這行程式的意思是指，當符合正規表示式 `app/**/*.php` 的檔案一經修改，就會觸發 `speak` 任務。

很好！接著你可以將這行程式寫在 Gulpfile 的頂端，或者將它放到自訂任務的檔案裡。如果你選擇後者，那麼你必須將它載入至你的 Gulpfile，例如：

```javascript
require("./custom-tasks")
```

大功告成！最後你只需要將他們結合。

```javascript
elixir(function(mix) {
	mix.speak("Tea, Earl Grey, Hot");
});
```

加入之後，每當你觸發 Gulp，Picard 就會要求一些茶。
