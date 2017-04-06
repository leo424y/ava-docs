___
**備註**

這是 [readme.md](https://github.com/avajs/ava/blob/master/readme.md) 的簡體中文翻譯。這個[連結](https://github.com/avajs/ava/compare/67bf0472133041f59ef469f737b696de05ae316b...master#diff-0730bb7c2e8f9ea2438b52e419dd86c9) 用來檢視本翻譯與 AVA 的 master 分支是否有差別（如果你沒有看到 `readme.md` 發生變化，那就意味著這份翻譯文件是最新的）。
___

# ![AVA](https://github.com/avajs/ava/blob/master/media/header.png)

> 未來的測試執行器

[![Build Status: Linux](https://travis-ci.org/avajs/ava.svg?branch=master)](https://travis-ci.org/avajs/ava) [![Build status: Windows](https://ci.appveyor.com/api/projects/status/igogxrcmhhm085co/branch/master?svg=true)](https://ci.appveyor.com/project/avajs/ava/branch/master) [![Coverage Status](https://coveralls.io/repos/avajs/ava/badge.svg?branch=master&service=github)](https://coveralls.io/github/avajs/ava?branch=master) [![Gitter](https://img.shields.io/badge/Gitter-Join_the_AVA_chat_%E2%86%92-00d06f.svg)](https://gitter.im/avajs/ava)

雖然 JavaScript 是單執行緒，但在 Node.js 裡由於其非同步的特性使得 IO 可以並行。AVA 利用這個優點讓你的測試可以併發執行，這對於 IO 繁重的測試特別有用。另外，測試檔案可以在不同的程序裡並行執行，讓每一個測試檔案可以獲得更好的效能和獨立的環境。在 Pageres 項目中從 Mocha [切換](https://github.com/sindresorhus/pageres/commit/663be15acb3dd2eb0f71b1956ef28c2cd3fdeed0) 到 AVA 讓測試時間從 31 秒下降到 11 秒。測試併發執行強制你寫原子測試，意味著測試不需要依賴全局狀態或者其他測試的狀態，這是一件非常好的事情。

*如果你想貢獻（問題、PRs 等），請先閱讀我們的[貢獻嚮導](contributing.md)。*

關注 [AVA 的 Twitter 賬號](https://twitter.com/ava__js) 以獲取最新資訊。

翻譯：[Español](https://github.com/avajs/ava-docs/blob/master/es_ES/readme.md), [Français](https://github.com/avajs/ava-docs/blob/master/fr_FR/readme.md), [Italiano](https://github.com/avajs/ava-docs/blob/master/it_IT/readme.md), [日本語](https://github.com/avajs/ava-docs/blob/master/ja_JP/readme.md), [Português](https://github.com/avajs/ava-docs/blob/master/pt_BR/readme.md), [簡體中文](https://github.com/avajs/ava-docs/blob/master/zh_CN/readme.md)

## 目錄

- [用法](#用法)
- [CLI 用法](#cli)
- [配置](#配置)
- [文件](#文件)
- [API](#api)
- [斷言](#斷言)
- [小貼士](#小貼士)
- [FAQ](#faq)
- [祕方](#祕方)
- [支援](#支援)
- [相關](#相關)
- [連結](#連結)
- [團隊](#團隊)


## 為什麼要用 AVA?

- 輕量和高效
- 簡單的測試語法
- 併發執行測試
- 強制編寫原子測試
- 沒有隱藏的全局變數
- [為每個測試檔案隔離環境](#隔離程序)
- [用 ES2015 編寫測試](#支援-es2015)
- [支援 Promise](#支援-promise)
- [支援 Generator](#支援-generator)
- [支援 Async](#支援-async)
- [支援 Observable](#支援-observable)
- [強化斷言資訊](#強化斷言資訊)
- [可選的 TAP 輸出顯示](#可選的-tap-輸出)
- [簡明的堆棧跟蹤](#簡明的堆棧跟蹤)


## 測試語法

```js
import test from 'ava';

test(t => {
    t.deepEqual([1, 2], [1, 2]);
});
```


## 用法

### 在項目中新增 AVA

通過帶 `--init` 參數執行 AVA 全局安裝命令，將會新增 AVA 到 `package.json`：

```console
$ npm install --global ava
$ ava --init
```

```json
{
    "name": "awesome-package",
    "scripts": {
        "test": "ava"
    },
    "devDependencies": {
        "ava": "^0.11.0"
    }
}
```

寫在 `--init` 後面的任何參數都會被新增到 `package.json`。

#### 手動安裝

你也可以直接安裝 AVA：
```console
$ npm install --save-dev ava
```

你還需要配置 `test` 指令碼在你的 `package.json`，值為 `ava`（參照上面的配置）。

### 建立你的測試檔案

在你的工程根目錄下建立一個名字為 `test.js` 的檔案：

```js
import test from 'ava';

test('foo', t => {
    t.pass();
});

test('bar', async t => {
    const bar = Promise.resolve('bar');

    t.is(await bar, 'bar');
});
```

<img src="https://github.com/avajs/ava/blob/master/media/screenshot.png" width="150" align="right">

### 執行

```console
$ npm test
```

### 觀察

```console
$ npm test -- --watch
```

AVA 來自一個非常聰明的觀察模式。[可以在它的祕方中檢視詳情](docs/recipes/watch-mode.md)。

## CLI

![](https://github.com/avajs/ava/blob/master/media/screenshot-mini-reporter.gif)

```console
$ ava --help

  Usage
    ava [<file|directory|glob> ...]

  Options
    --init           Add AVA to your project
    --fail-fast      Stop after first test failure
    --serial, -s     Run tests serially
    --require, -r    Module to preload (Can be repeated)
    --tap, -t        Generate TAP output
    --verbose, -v    Enable verbose output
    --no-cache       Disable the transpiler cache
    --match, -m      Only run tests with matching title (Can be repeated)',
    --watch, -w      Re-run tests when tests and source files change
    --source, -S     Pattern to match source files so tests can be re-run (Can be repeated)

  Examples
    ava
    ava test.js test2.js
    ava test-*.js
    ava test
    ava --init
    ava --init foo.js

  Default patterns when no arguments:
  test.js test-*.js test/**/*.js
```

*注意，如果你本地安裝的 AVA 可用的話 CLI 將會執行它，即使你的全局也安裝了 AVA。*

資料夾會被遞迴遍歷，帶上 `*.js` 參數的話全部檔案都會被作為測試檔案。名字為 `fixtures`，`helpers` 和 `node_modules` 的資料夾*總會*被忽略。所以把 helper 名字以 `_` 開頭命名就可以一起放置在測試檔案的目錄下。

當使用 `npm test` 時你可以直接傳參數 `npm test test2.js`，但標誌需要像這樣傳遞 `npm test -- --verbose`。


## 配置

所有的 CLI 選項都可以配置在 `package.json` 的 `ava` 屬性中。你可以修改 `ava` 命令的預設行為，所以你不需要重複在命令列中輸入相同的選項。

```json
{
  "ava": {
    "files": [
      "my-test-folder/*.js",
      "!**/not-this-file.js"
    ],
    "source": [
      "**/*.{js,jsx}",
      "!dist/**/*"
    ],
    "match": [
      "*oo",
      "!foo"
    ],
    "failFast": true,
    "tap": true,
    "require": [
      "babel-register"
    ],
    "babel": "inherit"
  }
}
```

傳遞給 CLI 的參數總是比 `package.json` 中的配置優先順序高。

請看 [支援 ES2015](#支援-es2015) 章節以瞭解 `babel` 選項的更多詳情。

## 文件

測試是併發執行的，你可以選擇同步或非同步執行測試，在返回一個 promise 或 [observable](https://github.com/zenparsing/zen-observable) 時你才需要考慮使用同步。

我們*強烈*推薦使用 [async 函數](#支援-async)，它讓非同步程式碼簡潔更具可讀性，並且它隱式返回一個 promise 讓你無需手動建立。

如果你不能使用 promise 或者 observales，你可以通過這樣定義你的測試 `test.cb([title], fn)` 來啟用 “callback 模式”。通過這種方式聲明的測試*必須*手動新增 `t.end()` 來結束，這種模式的主要目的是為了測試 callback 方式的 API。

你必須同時將所有測試都定義為同步，它們不能定義在 `setTimeout`，`setImmediate` 等裡面。

測試檔案在當前資料夾中被執行，所以 [`process.cwd()`](https://nodejs.org/api/process.html#process_process_cwd) 和 [`__dirname`](https://nodejs.org/api/globals.html#globals_dirname) 總是相同。你可以使用相對路徑代替 `path.join(__dirname, 'relative/path')` 操作。

### 建立測試

你可以通過呼叫 AVA 中匯入的 `test` 方法來建立一個測試。提供可選的標題和 callback 函數，函數將在你執行測試時被呼叫。它會傳遞一個 [執行物件](#t) 作為其第一個且唯一的一個參數。按照慣例這個參數名字為 `t`。

```js
import test from 'ava';

test('my passing test', t => {
    t.pass();
});
```

#### 標題

標題是可選的，意味著你可以這樣：

```js
test(t => {
    t.pass();
});
```

如果你有多個測試的話建議寫上測試標題。

如果你沒有提供測試標題，但 callback 是一個有名字的函數，那這個名字將作為測試標題：

```js
test(function name(t) {
    t.pass();
});
```

### 斷言計劃

斷言計劃確保測試只能通過指定次數的斷言，它們可以幫你捕捉測試太早退出的情況，如果太多斷言被執行的話它們也會讓測試失敗，如果在你的 callback 或迴圈中有斷言的話斷言計劃會比較有用。

請注意：不像 [`tap`](https://www.npmjs.com/package/tap) 和 [`tape`](https://www.npmjs.com/package/tape)，當斷言計劃數達到了指定數量 AVA 並*不*會自動結束。

這些例子將會通過測試：

```js
test(t => {
    t.plan(1);

    return Promise.resolve(3).then(n => {
        t.is(n, 3);
    });
});

test.cb(t => {
    t.plan(1);

    someAsyncFunction(() => {
        t.pass();
        t.end();
    });
});
```

這些則不會：

```js
test(t => {
    t.plan(2);

    for (let i = 0; i < 3; i++) {
        t.true(i < 3);
    }
}); // 失敗，3 個斷言被執行，但計劃只有 2 個

test(t => {
    t.plan(1);

    someAsyncFunction(() => {
        t.pass();
    });
}); // 失敗，在斷言執行之前測試會因為同步而提前結束。
```

### 序列執行測試

測試預設是併發執行的，這很棒。但有時你必須寫不是併發執行的測試。

這種情況比較少見，但你可以使用 `.serial` 修飾符，它將強制讓那些測試在併發的測試前*先*序列執行。

```js
test.serial(t => {
    t.pass();
});
```

注意這隻影響一個特定測試檔案裡面的測試，AVA 將仍然同時執行多個測試檔案，除非你傳遞 [`--serial` CLI 標誌](#cli)。

### 執行指定的測試

在開發中只執行少量指定的測試非常有用，這可以通過使用 `.only` 修飾符來完成。

```js
test('will not be run', t => {
    t.fail();
});

test.only('will be run', t => {
    t.pass();
});
```

`.only` 影響所有測試檔案，所以你在一個檔案裡面使用了它，那其他測試檔案裡的測試將不會執行。

### 執行匹配標題的測試

`--match` 標誌允許你只執行包含匹配標題的測試，這可以通過簡單的通配模式來做到，模式是大小寫敏感的，詳情請看 [`matcher`](https://github.com/sindresorhus/matcher)。

匹配以 `foo` 結尾的標題：

```console
$ ava --match='*foo'
```

匹配以 `foo` 開頭的標題：

```console
$ ava --match='foo*'
```

匹配包含了 `foo` 的標題：

```console
$ ava --match='*foo*'
```

匹配*精確*等於 `foo` 的標題（儘管大小寫敏感）：

```console
$ ava --match='foo'
```

匹配不包含了 `foo` 的標題：

```console
$ ava --match='!*foo*'
```

匹配以 `foo` 開頭並且以 `bar` 結束的標題：

```console
$ ava --match='foo*bar'
```

匹配以 `foo` 開頭或者以 `bar` 結束的標題：

```console
$ ava --match='foo*' --match='*bar'
```

注意，匹配模式優先順序高於 `.only` 修飾符，只有帶明確標題的測試可以被匹配，當使用 `--match` 時，沒有標題的或者標題是從 callback 函數名來的測試都會被跳過。

下面是當使用一個匹配模式 `*oo*` 來匹配測試的結果：

```js
test('foo will run', t => {
    t.pass();
});

test('moo will also run', t => {
    t.pass();
});

test.only('boo will run but not exclusively', t => {
    t.pass();
});

// 不會執行，沒有標題
test(function (t) {
    t.fail();
});

// 不會執行，沒有明確定義的標題
test(function foo(t) {
    t.fail();
});
```

### 跳過測試

有時候失敗的測試一時難以修復，你可以使用 `.skip` 來告訴 AVA 跳過這些測試。它們仍然會顯示在輸出結果中（標識為 skipped），但不會被執行。

```js
test.skip('will not be run', t => {
    t.fail();
});
```

你必須指定 callback 函數。

### 測試佔位符 ("todo")

當你計劃寫一個測試的時候你可以使用 `.todo` 修飾符，像跳過測試一樣這些佔位符也會顯示在輸出結果中，它們只要求一個標題，你不能指定 callback 函數。

```js
test.todo('will think about writing this later');
```

### Before & after 鉤子

AVA 讓你可以註冊在測試之前和之後跑的鉤子，這允許你執行 setup 和 teardow 程式碼。

`test.before()` 在你的測試檔案中註冊了一個鉤子並在第一個測試前執行，同樣地，`test.after()` 註冊了一個鉤子在最後一個測試後執行。

`test.beforeEach()` 在你的測試檔案中註冊了一個鉤子並在每個測試前執行，同樣地，`test.afterEach()` 註冊了一個鉤子在每個測試後執行。

像 `test()` 這些方法一樣接收一個可選的標題和一個 callback 函數，如果你的鉤子失敗了標題就會顯示出來，callback 函數傳遞一個 [執行物件](#t)。

`before` 在 `beforeEach` 之前執行，`afterEach` 在 `after` 之前執行，同一類的鉤子按照它們定義的順序來執行。

```js
test.before(t => {
    // 這個會在所有測試前執行
});

test.before(t => {
    // 這個會在上面的方法後面執行，但在測試之前執行
});

test.after('cleanup', t => {
    // 這個會在所有測試之後執行
});

test.beforeEach(t => {
    // 這個會在每個測試之前執行
});

test.afterEach(t => {
    // 這個會在每個測試之後執行
});

test(t => {
    // 正常的測試
});
```

鉤子可以同步或非同步，就像測試一樣。讓鉤子非同步返回一個 promise 或 observable，使用一個 async 函數，或者通過 `test.cb.before()`，`test.cb.beforeEach()` 等來啟用 callback 模式。

```js
test.before(async t => {
    await promiseFn();
});

test.after(t => {
    return new Promise(/* ... */);
});

test.cb.beforeEach(t => {
    setTimeout(t.end);
});

test.afterEach.cb(t => {
    setTimeout(t.end);
});
```

請記住，`beforeEach` 和 `afterEach` 鉤子在一個測試之前和之後執行，並且預設情況下測試是併發執行的，如果你需要為每個測試設定一個全局的狀態（比如 `console.log` 的 spying [例子](https://github.com/avajs/ava/issues/560)），你需要確保這些測試是[序列執行](#序列執行測試)的。

記住，AVA 執行每個測試檔案會有各自單獨的程序，你可能不需要在 `after` 鉤子中清理全局狀態，因為它只會在程序退出前被呼叫。

`beforeEach` 和 `afterEach` 鉤子可以共享測試的上下文：

```js
test.beforeEach(t => {
    t.context.data = generateUniqueData();
});

test(t => {
    t.is(t.context.data + 'bar', 'foobar');
});
```

預設情況下 `t.context` 是一個物件，但你可以重新賦值：

```js
test.beforeEach(t => {
    t.context = 'unicorn';
});

test(t => {
    t.is(t.context, 'unicorn');
});
```

上下文共享在 `before` 和 `after` 鉤子中*不*可用。

### 連線測試修飾符

你可以以任何順序使用 `.serial`，`.only` 和 `.skip`，與 `test`，`before`，`after`，`beforeEach` 和`afterEach` 一起，比如：

```js
test.before.skip(...);
test.skip.after(...);
test.serial.only(...);
test.only.serial(...);
```

這意味著你可以在每個測試或鉤子的末尾臨時新增 `.skip` 或 `.only`，而不用做其他的修改。

### 自定義斷言

你可以使用其他斷言庫來替代內建的斷言庫，當斷言失敗可以讓其拋出異常。

但這樣做的話你將得不到[內建斷言庫](#斷言)的良好體驗，同時你也將不會用到[斷言計劃](#斷言計劃） (『看#25](https://github.com/avajs/ava/issues/25))。

```js
import assert from 'assert';

test(t => {
    assert(true);
});
```

### 支援 ES2015

AVA 通過 [Babel 6](https://babeljs.io) 內建支援 ES2015，只需要用 ES2015 的方式寫你測試，不需要額外的配置。你可以在你的工程中使用任何的 Babel 版本，我們使用我們捆綁的 Babel，帶有 [`es2015`](https://babeljs.io/docs/plugins/preset-es2015/) 和 [`stage-2`](https://babeljs.io/docs/plugins/preset-stage-2/) 設定，和 [`espower`](https://github.com/power-assert-js/babel-plugin-espower) 和 [`transform-runtime`](https://babeljs.io/docs/plugins/transform-runtime/) 外掛一樣。

類似的 Babel 配置同樣適用於 AVA，如下：

```json
{
  "presets": [
    "es2015",
    "stage-2",
  ],
  "plugins": [
    "espower",
    "transform-runtime"
  ]
}
```

你可以自定義 AVA 如何通過 [`package.json` 配置](#配置) `babel` 選項來轉換測試檔案，例如你可以這樣來覆蓋配置：

```json
{
  "ava": {
    "babel": {
      "presets": [
        "es2015",
        "stage-0",
        "react"
      ]
    }
  },
}
```

你也可以使用特殊的 `"inherit"` 關鍵字，這讓 AVA 遵從你的 [`.babelrc` 或 `package.json` 檔案](https://babeljs.io/docs/usage/babelrc/) 中的 Babel 配置，這種方式讓測試檔案的轉換方式和與原始檔的相同，可以無需在 AVA 中重複配置。

 ```json
{
    "babel": {
        "presets": [
            "es2015",
            "stage-0",
            "react"
        ]
    },
    "ava": {
        "babel": "inherit",
    },
}
```

注意，AVA *總是*應用 [`espower`](https://github.com/power-assert-js/babel-plugin-espower) 和 [`transform-runtime`](https://babeljs.io/docs/plugins/transform-runtime/) 外掛。

### 支援 TypeScript

AVA 支援 TypeScript，你必須自己配置轉換規則，當你在你的 `tsconfig.json` 檔案中把 `module` 設為 `commonjs`，TypeScipt 將自動為 AVA 找到類型定義。你應該把 `target` 設定為 `es2015` 來使用 promises 和 async 函數。

### 轉換匯入模組

AVA 現在只轉換需要執行的測試，*它不會轉換那些在測試中你 `import` 的模組*，這可能不是你所期望的但這就是現在的工作方案。

如果你使用 Babel 你可以使用它的 [require 鉤子](https://babeljs.io/docs/usage/require/) 來實時轉換匯入模組，執行 AVA 帶上 `--require babel-register` （請看 [CLI](#cli)) 參數或[在你的 `package.json` 裡配置](#配置)。

你也可以在另外一個程序裡轉換你的模組，並且參考你的轉換檔案而不是你的測試源碼。

### 支援 Promise

如果你在測試裡返回一個 promise，你不需要在測試裡明確的結束測試，當 promise resolve 的時候它會自己結束。

```js
test(t => {
    return somePromise().then(result => {
        t.is(result, 'unicorn');
    });
});
```

### 支援 Generator

AVA 自帶對 [generator 函數](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Statements/function*) 的內建支援。

```js
test(function * (t) {
    const value = yield generatorFn();
    t.true(value);
});
```

### 支援 Async

AVA 自帶對 [async functions](https://tc39.github.io/ecmascript-asyncawait/) *(async/await)* 的內建支援。

```js
test(async function (t) {
    const value = await promiseFn();
    t.true(value);
});

// 非同步箭頭函數
test(async t => {
    const value = await promiseFn();
    t.true(value);
});
```

### 支援 Observable

AVA 自帶對 [observables](https://github.com/zenparsing/es-observable) 的內建支援。如果你從測試中返回一個 observable，AVA 會自動消費它使其在測試結束前完成。

*你不需要使用 “callback 模式" 或者呼叫 `t.end()`。*

```js
test(t => {
    t.plan(3);
    return Observable.of(1, 2, 3, 4, 5, 6)
        .filter(n => {
            // 只有奇數
            return n % 2 === 0;
        })
        .map(() => t.pass());
});
```

### 支援 Callback

當使用 node-style, error-first 等 callback API 時，AVA 支援使用 `t.end` 作為最後一個 callback 函數。AVA 將把任何為真的值傳遞給 `t.end` 其實變成一個 error。注意，`t.end` 要求 “callback 模式”，這個可以通過使用 `test.cb` 鏈來啟用。

```js
test.cb(t => {
    // t.end 自動檢查第一個參數是否為錯誤
    fs.readFile('data.txt', t.end);
});
```

### 可選的 TAP 輸出

AVA 可以通過 `--tap` 選項來生成 TAP 的輸出，可以選擇任意的 [TAP 報告](https://github.com/sindresorhus/awesome-tap#reporters)。

```console
$ ava --tap | tap-nyan
```

<img src="https://github.com/avajs/ava/blob/master/media/tap-output.png" width="398">

### 簡明的堆棧跟蹤

AVA 會在堆棧跟蹤資訊裡面自動移除不相關的行，讓你更快地找到錯誤的原因。

<img src="https://github.com/avajs/ava/blob/master/media/stack-traces.png" width="300">

## API

### `test([title], implementation)`
### `test.serial([title], implementation)`
### `test.cb([title], implementation)`
### `test.only([title], implementation)`
### `test.skip([title], implementation)`
### `test.todo(title)`
### `test.failing([title], implementation)`
### `test.before([title], implementation)`
### `test.after([title], implementation)`
### `test.beforeEach([title], implementation)`
### `test.afterEach([title], implementation)`

#### `title`

類型：`string`

測試標題

#### `callback(t)`

類型：`function`

應該包含實際的測試。

##### `t`

類型：`object`

特定測試的執行物件，每個測試的 callback 接收到一個不同的物件，包含[斷言](#斷言)，`.plan(count)` 和 `.end()` 等方法。`t.context` 可以包含 `beforeEach` 鉤子中的共享狀態。

###### `t.plan(count)`

計劃有多少斷言將在測試中被執行，如果實際斷言的數量沒有匹配計劃數，那麼測試將失敗，詳情請見[斷言計劃](#斷言計劃)。

###### `t.end()`

結束測試，只在 `test.cb()` 中有效。

## 斷言

斷言也被包含在 [執行物件](#t) 中，可以提供給每個測試 callback：

```js
test(t => {
    t.truthy('unicorn'); // 斷言
});
```

如果單個測試中有多個斷言同時失敗了，那 AVA 只會顯示*第一個*。

### `.pass([message])`

測試通過。

### `.fail([message])`

斷言失敗。

### `.truthy(value, [message])`

斷言 `value` 是否是真值。

### `.falsy(value, [message])`

斷言 `value` 是否是假值。

### `.true(value, [message])`

斷言 `value` 是否是 `true`。

### `.false(value, [message])`

斷言 `value` 是否是 `false`。

### `.is(value, expected, [message])`

斷言 `value` 是否和 `expected` 相等。

### `.not(value, expected, [message])`

斷言 `value` 是否和 `expected` 不等。

### `.deepEqual(value, expected, [message])`

斷言 `value` 是否和 `expected` 深度相等。

### `.notDeepEqual(value, expected, [message])`

斷言 `value` 是否和 `expected` 深度不等。

### `.throws(function|promise, [error, [message]])`

斷言 `function` 拋出一個異常，或者 `promise` reject 一個錯誤。

`error` 可以是一個構造器，正則，錯誤資訊或者驗證函數。

返回由 `function` 拋出的異常或 `promise` 的拒絕原因。

### `.notThrows(function|promise, [message])`

斷言 `function` 沒有拋出一個異常，或者 `promise` resolve。

### `.regex(contents, regex, [message])`

斷言 `contents` 匹配 `regex`。

### `.notRegex(contents, regex, [message])`

斷言 `contents` 不匹配 `regex`。

### `.ifError(error, [message])`

斷言 `error` 是假值。

### 跳過斷言

通過 `skip` 修飾符可以跳過任何斷言，跳過的斷言仍然會被計數，所以不需要去改變你的斷言計劃數量。

```js
test(t => {
    t.plan(2);
    t.skip.is(foo(), 5); // 當跳過時不需要改變你的計劃數
    t.is(1, 1);
});
```

### 強化斷言資訊

AVA 自帶內建的 [`power-assert`](https://github.com/power-assert-js/power-assert)，給你更多的描述性斷言資訊，它閱讀你的測試程式碼並從中試圖推斷出更多資訊。

我們來舉個例子，使用 Node 的標準 [`斷言` 庫](https://nodejs.org/api/assert.html)。

```js
const a = /foo/;
const b = 'bar';
const c = 'baz';
require('assert').ok(a.test(b) || b === c);
```

如果你把程式碼貼上到 Node REPL 中會返回：

```
AssertionError: false == true
```

而在 AVA 中，這個測試：

```js
test(t => {
    const a = /foo/;
    const b = 'bar';
    const c = 'baz';
    t.true(a.test(b) || b === c);
});
```

將會輸出：

```
t.true(a.test(b) || b === c)
       |    |     |     |
       |    "bar" "bar" "baz"
       false
```

## 隔離程序

每個測試檔案都會在一個獨立的 Node 程序中執行，這樣讓你可以在一個測試檔案中改變全局狀態或將其覆蓋一個內建的全局狀態，而不會影響其他測試檔案。這樣更加有效地利用現代的多核處理器，讓多個測試可以併發地執行。

## 小貼士

### 臨時檔案

併發執行測試面臨著許多挑戰，檔案的 IO 操作就是其中一個。

一般來講，序列測試在當前資料夾中建立臨時資料夾，然後會在測試結束的時候清理它們。但這種做法在併發測試中不起作用，因為測試會一起操作這個資料夾而導致衝突。正確的做法是為每個測試建立一個新的臨時資料夾，使用 [`tempfile`](https://github.com/sindresorhus/tempfile) 和 [`temp-write`](https://github.com/sindresorhus/temp-write) 模組可能會有幫助。

### 偵錯

AVA 預設情況下是併發執行測試，這樣在偵錯資訊時並不是最理想的，你可以通過設定 `--serial` 選項來讓測試序列執行。

```console
$ ava --serial
```

### 程式碼覆蓋率

你不能使用 [`istanbul`](https://github.com/gotwarlost/istanbul) 來做程式碼覆蓋率因為 AVA [處理過這些測試檔案](#隔離程序），你可以使用 [`nyc`](https://github.com/bcoe/nyc) 來代替，你可以把它看做是支援子程序的 `istanbul`。

從版本 `5.0.0` 開始，在報告覆蓋率時為你的程式碼使用原生對映，而不是轉換後的程式碼。確保你測試的程式碼包括了一個內聯的原生對映或者引用了一個原生對映檔案。如果你使用了 `babel-register`，你可以在你的 Babel 配置中將 `sourceMaps` 設定為 `inline`。

## FAQ

### 為什麼不用 `mocha`，`tape`，`tap`？

Mocha 要求你使用隱式全局變數比如 `describe` 和 `it` 作為其預設介面（這是大部分人使用的）。這樣做不是很好，並且序列執行測試沒有程序隔離，使得測試十分緩慢。

Tape 和 tap 是非常好的。AVA 在它們的語法中得到大量啟發，但它們也是序列執行測試，它們的預設 [TAP](https://testanything.org) 輸出不是非常友好，因此你總是需要使用額外的 tap 報告。

與它們不同的是，AVA 可以併發執行測試，為每個測試檔案提供獨立程序，它的預設報告簡單明瞭，並且 AVA 也支援通過 CLI 標誌來輸出一個 TAP 報告。

### 我如何使用自定義的報告？

AVA 支援 TAP 格式，所以它相容任何 [TAP 報告](https://github.com/sindresorhus/awesome-tap#reporters)，使用 [`--tap` 標誌](#可選的-tap-輸出)來啟用 TAP 輸出。

### 項目名字要怎麼寫才是正確的？如何發音？

AVA，不是 Ava，也不是 ava，發音 [`/ˈeɪvə/` ay-və](https://github.com/avajs/ava/blob/master/media/pronunciation.m4a?raw=true)。

### 項目背景圖片是什麼意思？

它是[處女座星系](https://zh.wikipedia.org/wiki/%E4%BB%99%E5%A5%B3%E5%BA%A7%E6%98%9F%E7%B3%BB)。

### 併發和並行有什麼不同？

[併發不是並行，併發可以並行。](https://stackoverflow.com/q/1050222)

## 祕方

- [程式碼覆蓋率](docs/recipes/code-coverage.md)
- [觀察模式](docs/recipes/watch-mode.md)
- [端點測試](docs/recipes/endpoint-testing.md)
- [什麼時候使用 `t.plan()`](docs/recipes/when-to-use-plan.md)
- [瀏覽器測試](docs/recipes/browser-testing.md)
- [TypeScript](docs/recipes/typescript.md)

## 支援

- [Stack Overflow](https://stackoverflow.com/questions/tagged/ava)
- [Gitter chat](https://gitter.im/avajs/ava)
- [Twitter](https://twitter.com/ava__js)

## 相關

- [sublime-ava](https://github.com/avajs/sublime-ava) - AVA 測試的程式碼片段
- [atom-ava](https://github.com/avajs/atom-ava) - AVA 測試的程式碼片段
- [vscode-ava](https://github.com/samverschueren/vscode-ava) - AVA 測試的程式碼片段
- [eslint-plugin-ava](https://github.com/avajs/eslint-plugin-ava) - AVA 測試的程式碼規則
- [gulp-ava](https://github.com/avajs/gulp-ava) - 用 gulp 執行測試
- [grunt-ava](https://github.com/avajs/grunt-ava) - 用 grunt 執行測試
- [fly-ava](https://github.com/pine/fly-ava) - 用 fly 執行測試
- [start-ava](https://github.com/start-runner/ava) - 用 start 執行測試

[更多...](https://github.com/avajs/awesome-ava#packages)

## 連結

- [購買 AVA 貼紙](https://www.stickermule.com/user/1070705604/stickers)
- [Awesome 列表](https://github.com/avajs/awesome-ava)

## 團隊

[![Sindre Sorhus](https://avatars.githubusercontent.com/u/170270?s=130)](http://sindresorhus.com) | [![Vadim Demedes](https://avatars.githubusercontent.com/u/697676?s=130)](https://github.com/vdemedes) | [![James Talmage](https://avatars.githubusercontent.com/u/4082216?s=130)](https://github.com/jamestalmage) | [![Mark Wubben](https://avatars.githubusercontent.com/u/33538?s=130)](https://novemberborn.net)
---|---|---|---|---
[Sindre Sorhus](http://sindresorhus.com) | [Vadim Demedes](https://github.com/vdemedes) | [James Talmage](https://github.com/jamestalmage) | [Mark Wubben](https://novemberborn.net)

### 前任

- [Kevin Mårtensson](https://github.com/kevva)

<div align="center">
    <br>
    <br>
    <br>
    <img src="https://cdn.rawgit.com/avajs/ava/fe1cea1ca3d2c8518c0cc39ec8be592beab90558/media/logo.svg" width="200" alt="AVA">
    <br>
    <br>
</div>
