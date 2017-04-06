___
**備註**

這是 [code-coverage.md](https://github.com/avajs/ava/blob/master/docs/recipes/code-coverage.md) 的簡體中文翻譯。這個[連結](https://github.com/avajs/ava/compare/1868204c1901f45b4f66a520ef6486fdd71fe1d2...master#diff-b3aa0c81a407f54f636a1cf5a619a4a6) 用來檢視本翻譯與 AVA 的 master 分支是否有差別（如果你沒有看到`code-coverage.md`發生變化，那就意味著這份翻譯文件是最新的）。
___

# 程式碼覆蓋率

翻譯：[Español](https://github.com/avajs/ava-docs/blob/master/es_ES/docs/recipes/code-coverage.md), [Français](https://github.com/avajs/ava-docs/blob/master/fr_FR/docs/recipes/code-coverage.md), [Italiano](https://github.com/avajs/ava-docs/blob/master/it_IT/docs/recipes/code-coverage.md), [日本語](https://github.com/avajs/ava-docs/blob/master/ja_JP/docs/recipes/code-coverage.md), [Português](https://github.com/avajs/ava-docs/blob/master/pt_BR/docs/recipes/code-coverage.md), [Русский](https://github.com/avajs/ava-docs/blob/master/ru_RU/docs/recipes/code-coverage.md), [簡體中文](https://github.com/avajs/ava-docs/blob/master/zh_CN/docs/recipes/code-coverage.md)


因為 AVA[重新處理了測試檔案][process-isolation]，所以你不能使用 [`istanbul`] 來做程式碼覆蓋率，但你可以使用 [`nyc`] 來完成，它是支援子程序的 [`istanbul`]。

## 設定

首先安裝 NYC：

```
$ npm install nyc --save-dev
```

然後新增`.nyc_output`和`coverage`資料夾到你的`.gitignore`檔案。

`.gitignore`:

```
node_modules
coverage
.nyc_output
```

## ES5 覆蓋率

使用 NYC 很簡單就可以提供使用 ES5 來寫的生產程式碼的覆蓋率，只需要在測試指令碼前面加上`nyc`：

```json
{
  "scripts": {
    "test": "nyc ava"
  }
}
```

就是這樣！

如果你想要建立 HTML 覆蓋率報告，或者上傳覆蓋率資料到 Coveralls，你應該跳過下面的那些章節。

## ES2015 覆蓋率

使用 Babel 來轉換生產程式碼有點複雜，這裡我們把它分成幾個步驟。

### 配置 Babel

首先，我們需要一個 Babel 配置，下面是一個例子，你可以修改它以適應你的需要。

`package.json`:
```json
{
    "babel": {
        "presets": ["es2015"],
        "plugins": ["transform-runtime"],
        "ignore": "test.js",
        "env": {
            "development": {
                "sourceMaps": "inline"
            }
        }
    }
}
```

例子中有 2 點比較重要：

1. 我們忽略測試檔案，因為 AVA 已經為你做了轉換處理了。

2. 我們為開發環境指定`inline`（內聯）原生對映，為了正確的生成覆蓋率這個很重要，使用 Babel 配置中的`env`屬性可以讓我們在生產構建中取消原生對映。


### 建立一個構建指令碼

因為你可能不希望在生產程式碼中`inline`原生對映，你需要在構建指令碼中指定一個可代替的環境變數：

`package.json`

```json
{
    "scripts": {
        "build": "BABEL_ENV=production babel --out-dir=dist index.js"
    }
}
```

> 警告：`BABEL_ENV=production`在 Windows 中不可用，你必須使用`set`關鍵字（`set BABEL_ENV=production`），如果是跨平臺構建，請檢查 [`cross-env`]。

注意，構建指令碼中 AVA 的部分真的很少，它只是一個如何使用 Babel 的`env`配置來操作你的配置以相容 AVA 的示例。

### 使用 Babel require 鉤子

要使用 Babel require 鉤子，請在`package.json`中的 AVA 配置裡將`require`屬性設定為`babel-core/register`。

```json
{
    "ava": {
        "require": ["babel-core/register"]
    }
}
```

*注意*：你也可以在命令列裡設定 require 鉤子：`ava --require=babel-core/register`。儘管如此，配置在`package.json`裡面可以讓你不用重複地寫標誌。

### 把所有東西放在一起

結合上面的步驟，你的`package.json`最後可能是這個樣子：

```json
{
    "scripts": {
        "test": "nyc ava",
        "build": "BABEL_ENV=production babel --out-dir=dist index.js"
    },
    "babel": {
        "presets": ["es2015"],
        "plugins": ["transform-runtime"],
        "ignore": "test.js",
        "env": {
            "development": {
                "sourceMaps": "inline"
            }
        }
    },
    "ava": {
        "require": ["babel-core/register"]
    }
}
```


## HTML 報告

NYC 在`.nyc_ouput`資料夾中為每個程序建立一個`json`的覆蓋率檔案。

把這些檔案組合成一個可閱讀的 HTML 報告，可以通過下面的方法來做：

```
$ ./node_modules/.bin/nyc report --reporter=html
```

或者，使用 npm 指令碼來代替列印命令列：

```json
{
    "scripts": {
        "report": "nyc report --reporter=html"
    }
}
```

這樣會在`coverage`資料夾中輸出一個 HTML 檔案。


## 託管覆蓋率報告

### Travis CI & Coveralls

首先，你需要登入 [coveralls.io] 並啟用你的項目庫。

一旦完成，新增 [`coveralls`] 到開發依賴庫：

```
$ npm install coveralls --save-dev
```

然後新增下面的程式碼到你的`.travis.yml`：

```yaml
after_success:
    - './node_modules/.bin/nyc report --reporter=text-lcov | ./node_modules/.bin/coveralls'
```

你的覆蓋率報告將在你的 Travis 完成後很快地出現在 coveralls 上面。

[`babel`]:      https://github.com/babel/babel
[coveralls.io]: https://coveralls.io
[`coveralls`]:  https://github.com/nickmerwin/node-coveralls
[`cross-env`]:  https://github.com/kentcdodds/cross-env
[process-isolation]: https://github.com/avajs/ava-docs/blob/master/zh_CN/readme.md#隔離程序
[`istanbul`]:   https://github.com/gotwarlost/istanbul
[`nyc`]:        https://github.com/bcoe/nyc
