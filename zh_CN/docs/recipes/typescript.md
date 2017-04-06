___
**備註**

這是 [typescript.md](https://github.com/avajs/ava/blob/master/docs/recipes/typescript.md) 的簡體中文翻譯。這個[連結](https://github.com/avajs/ava/compare/8e2f3dca177a4283ad882596d3c1425cabb998ef...master#diff-60cce07a584082115d230f2e3d571ad6) 用來檢視本翻譯與 AVA 的 master 分支是否有差別（如果你沒有看到`typescript.md`發生變化，那就意味著這份翻譯文件是最新的）。
___

# TypeScript

翻譯：[Français](https://github.com/avajs/ava-docs/blob/master/fr_FR/docs/recipes/typescript.md), [Italiano](https://github.com/avajs/ava-docs/blob/master/it_IT/docs/recipes/typescript.md), [Русский](https://github.com/avajs/ava-docs/blob/master/ru_RU/docs/recipes/typescript.md), [簡體中文](https://github.com/avajs/ava-docs/blob/master/zh_CN/docs/recipes/typescript.md)

AVA 捆綁了一個 TypeScript 定義檔案，讓開發人員可以瞭解如何用 TypeScript 寫測試。

## 設定

首先安裝 TypeScript 編譯器 [tsc](https://github.com/Microsoft/TypeScript)。

```
$ npm install --save-dev tsc
```

建立一個 [`tsconfig.json`](https://github.com/Microsoft/TypeScript/wiki/tsconfig.json) 檔案，檔案指定編譯器是用來編譯工程或者測試檔案。

```json
{
    "compilerOptions": {
        "module": "commonjs",
        "target": "es2015"
    }
}
```

在`package.json`檔案裡新增一個`test`指令碼，在執行 AVA 前先編譯工程。

```json
{
  "scripts": {
    "test": "tsc && ava"
  }
}
```


## 新增測試

建立一個`test.ts`檔案。

```ts
import test from 'ava';

async function fn() {
    return Promise.resolve('foo');
}

test(async (t) => {
    t.is(await fn(), 'foo');
});
```


## 執行測試

```
$ npm test
```
