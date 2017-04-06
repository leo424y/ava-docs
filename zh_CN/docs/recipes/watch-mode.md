___
**備註**

這是 [watch-mode.md](https://github.com/avajs/ava/blob/master/docs/recipes/watch-mode.md) 的簡體中文翻譯。這個[連結](https://github.com/avajs/ava/compare/82c02bce80696547db0387dec243ddb470c8bce7...master#diff-92da4f3d087d796fdf4a45be88586b62) 用來檢視本翻譯與 AVA 的 master 分支是否有差別（如果你沒有看到`watch-mode.md`發生變化，那就意味著這份翻譯文件是最新的）。
___

# 觀察模式

翻譯：[Français](https://github.com/avajs/ava-docs/blob/master/fr_FR/docs/recipes/watch-mode.md), [Italiano](https://github.com/avajs/ava-docs/blob/master/it_IT/docs/recipes/watch-mode.md), [Русский](https://github.com/avajs/ava-docs/blob/master/ru_RU/docs/recipes/watch-mode.md), [簡體中文](https://github.com/avajs/ava-docs/blob/master/zh_CN/docs/recipes/watch-mode.md)


AVA 自帶了一個聰明的觀察模式，它會觀察那些改變了的檔案並執行受到改變影響的測試。

## 執行測試時啟用觀察模式

你可以通過使用`--watch`或`-w`標誌來啟用觀察模式，如果你是全局安裝的 AVA：

```console
$ ava --watch
```

如果你將 AVA 配置在`package.json`中，像這樣：

```json
{
  "scripts": {
    "test": "ava"
  }
}
```

你可以這樣執行：

```console
$ npm test -- --watch
```

你可以設定一個特殊的指令碼：

```json
{
  "scripts": {
    "test": "ava",
    "test:watch": "ava --watch"
  }
}
```

然後使用：

```console
$ npm run test:watch
```

## 要求

AVA 使用 [`chokidar`] 來作為檔案觀察器，它被配置為可選的依賴庫，因為`chokidar`有時候無法安裝，如果`chokidar`安裝失敗那麼觀察模式就不可用，然後你將看到一條這樣的資訊：

> The optional dependency chokidar failed to install and is required for --watch. Chokidar is likely not supported on your platform.

請參考 [`chokidar`文件][`chokidar`] 瞭解如何解決這個問題。

## 原始檔和測試檔案

在 AVA 中*原始檔*和*測試檔案*是有差別的，正如你所想的一樣，*測試檔案*包含了你的測試，*原始檔*是需要支援測試執行的其他所有檔案，是你的原始碼或者測試資料。

預設情況下 AVA 觀察測試檔案，`package.json`和其他的`.js`檔案的改變，它會忽略由 [`ignore-by-default`] 包提供的[特定資料夾](https://github.com/novemberborn/ignore-by-default/blob/master/index.js) 下的檔案。

你可以使用 [`--source` CLI 標誌]或`package.json`檔案的`ava`屬性為原始檔配置模式，注意如果你從 [`ignore-by-default`] 中指定了一個負模式目錄，那麼忽略將不再有效，所以你可能想要在你的配置裡重複這些操作。

如果你的測試會寫入磁碟，那麼它們會跟蹤觀察器來返回你的測試，這種情況下你需要使用`--source`標誌。

## 依賴跟蹤

AVA 跟蹤測試檔案依賴的原始檔，如果你改變的原始檔只有一個測試被依賴，那麼就只會返回這個測試，如果它不能識別哪個測試檔案依賴了這個被修改的原始檔，那麼它會返回所有的測試。

依賴跟蹤在 required 模式中有效，支援自定義繼承和轉換，使用 [`--require` CLI 標誌]而不是從你的測試檔案來幫助你載入它們。使用`fs`模組來訪問的檔案不會被跟蹤。

## 手動返回所有測試

你可以在 console 中，通過在<kbd>Enter</kbd>後面列印<kbd>r</kbd>來快速返回所有測試，

## 偵錯

有時候觀察模式會出現一些意想不到的事情，比如當你只執行一個測試時會返回所有測試，為了調查原因你可以啟用偵錯模式：

```console
$ DEBUG=ava:watcher npm test -- --watch
```

在 Windows 裡這樣用：

```console
$ set DEBUG=ava:watcher
$ npm test -- --watch
```

## 幫助我們改善觀察模式

觀察模式比較新並且現在處於初期階段，請[報告](https://github.com/avajs/ava/issues) 任何你遇到問題，謝謝！

[`chokidar`]: https://github.com/paulmillr/chokidar
[`ignore-by-default`]: https://github.com/novemberborn/ignore-by-default
[`--require` CLI 標誌]: https://github.com/avajs/ava-docs/blob/master/zh_CN/readme.md#cli
[`--source` CLI 標誌]: https://github.com/avajs/ava-docs/blob/master/zh_CN/readme.md#cli
