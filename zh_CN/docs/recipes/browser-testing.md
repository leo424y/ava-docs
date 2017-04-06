___
**備註**

這是 [browser-testing.md](https://github.com/avajs/ava/blob/master/docs/recipes/browser-testing.md) 的簡體中文翻譯。這個[連結](https://github.com/avajs/ava/compare/8e2f3dca177a4283ad882596d3c1425cabb998ef...master#diff-9d3d394077fa7f97cbbb0fefc098ac60) 用來檢視本翻譯與 AVA 的 master 分支是否有差別（如果你沒有看到`browser-testing.md`發生變化，那就意味著這份翻譯文件是最新的）。
___

# 設定 AVA 做瀏覽器測試

翻譯：[Français](https://github.com/avajs/ava-docs/blob/master/fr_FR/docs/recipes/browser-testing.md), [Italiano](https://github.com/avajs/ava-docs/blob/master/it_IT/docs/recipes/browser-testing.md) [Русский](https://github.com/avajs/ava-docs/blob/master/ru_RU/docs/recipes/browser-testing.md), [簡體中文](https://github.com/avajs/ava-docs/blob/master/zh_CN/docs/recipes/browser-testing.md)



AVA[還](https://github.com/avajs/ava/issues/24) 不支援在瀏覽器中執行測試。一些庫要求瀏覽器指定全局變數（`window`, `document`, `navigator`等等）。
React 就是其中的一個例子，最低要求如果你想用 ReactDOM.render 和用 ReactTestUTils 模擬事件。

這個祕方讓需要模擬瀏覽器環境的庫可以工作。

## 安裝 jsdom

安裝 [jsdom](https://github.com/tmpvar/jsdom)。

> 一個 WHATWG DOM 和 HTML 標準的 JavaScript 實現，給 node.js 使用的。

```
$ npm install --save-dev jsdom
```

## 設定 jsdom

建立一個 helper 檔案並放在`test/helpers`資料夾中，這樣確保 AVA 不會把它當成測試來處理。

`test/helpers/setup-browser-env.js`:

```js
global.document = require('jsdom').jsdom('<body></body>');
global.window = document.defaultView;
global.navigator = window.navigator;
```

## 配置測試使用 jsdom

配置 AVA，將`require`設定為 helper 檔案，這樣每個測試執行前都會先載入它。

`package.json`:

```json
{
  "ava": {
    "require": [
      "./test/helpers/setup-browser-env.js"
    ]
  }
}
```

## 享受！

編寫你的測試並享受一個模擬的 window 物件吧。

`test/my.react.test.js`:

```js
import test from 'ava';
import React from 'react';
import {render} from 'react-dom';
import {Simulate} from 'react-addons-test-utils';
import sinon from 'sinon';
import CustomInput from './components/custom-input.jsx';

test('Input calls onBlur', t => {
    const onUserBlur = sinon.spy();
    const input = render(
        React.createElement(CustomInput, onUserBlur),
        div
    );

    Simulate.blur(input);

    t.true(onUserBlur.calledOnce);
});
```
