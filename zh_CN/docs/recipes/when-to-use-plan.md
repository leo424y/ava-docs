___
**備註**

這是 [when-to-use-plan.md](https://github.com/avajs/ava/blob/master/docs/recipes/when-to-use-plan.md) 的簡體中文翻譯。這個[連結](https://github.com/avajs/ava/compare/89767ec3b6174e59d37faaadb50cfa3c0d58bda6...master#diff-0c25d982e94d600cb6b8e438a0e67169) 用來檢視本翻譯與 AVA 的 master 分支是否有差別（如果你沒有看到`when-to-use-plan.md`發生變化，那就意味著這份翻譯文件是最新的）。
___

# 什麼時候使用`t.plan()`

翻譯：[Español](https://github.com/avajs/ava-docs/blob/master/es_ES/docs/recipes/when-to-use-plan.md), [Français](https://github.com/avajs/ava-docs/blob/master/fr_FR/docs/recipes/when-to-use-plan.md), [Italiano](https://github.com/avajs/ava-docs/blob/master/it_IT/docs/recipes/when-to-use-plan.md), [日本語](https://github.com/avajs/ava-docs/blob/master/ja_JP/docs/recipes/when-to-use-plan.md),  [Português](https://github.com/avajs/ava-docs/blob/master/pt_BR/docs/recipes/when-to-use-plan.md), [Русский](https://github.com/avajs/ava-docs/blob/master/ru_RU/docs/recipes/when-to-use-plan.md), [簡體中文](https://github.com/avajs/ava-docs/blob/master/zh_CN/docs/recipes/when-to-use-plan.md)

AVA 和 [`tap`](https://github.com/tapjs/node-tap)/[`tape`](https://github.com/substack/tape) 的一個主要不同是`t.plan()`的行為。在 AVA 中`t.plan()`只是用來斷言期望的斷言數是否正確，並不會自動結束測試。

## `t.plan()`不好的用法

很多從`tap/tape`過渡過來的使用者習慣在每個測試裡大量使用`t.plan()`，儘管如此，在 AVA 裡我們不認為這是一個“最佳實踐”，我們反而認為`t.plan()`只有在少數情況才能提供其價值。

### 沒有分支的同步測試

在大部分同步測試中`t.plan()`是沒有必要的。

```js
test(t => {
    // 不好：這裡沒有分支，t.plan 是無意義的
    t.plan(2);

    t.is(1 + 1, 2);
    t.is(2 + 2, 4);
});
```

`t.plan()`在這裡並沒有提供任何價值，並且如果你決定新增或刪除斷言時會帶來額外的工作。

### Promises 期望得到 resolve

```js
test(t => {
    t.plan(1);

    return somePromise().then(result => {
        t.is(result, 'foo');
    });
});
```

簡單看一下，這個測試充分利用了`t.plan()`的優勢，因為這裡面涉及到了非同步 promise 處理。儘管如此在這個測試裡還有一些問題：

1. `t.plan()`用在這裡大概是為了防止`somePromise()`可能被 reject 的情況，但返回一個 reject 的 promise 測試始終會失敗。

2. 使用`async`/`await`可能會更好。

```js
test(async t => {
    t.is(await somePromise(), 'foo');
});
```

## `t.plan()`好的用法

`t.plan()`有很多可接受的用法。

### Promise 帶上一個`.catch()`塊

```js
test(t => {
    t.plan(2);

    return shouldRejectWithFoo().catch(reason => {
        t.is(reason.message, 'Hello') // 如果你關心的是 message 的話使用 t.throws() 更好
        t.is(reason.foo, 'bar');
    });
});
```

在這裡，`t.plan()`用來確保`catch`塊中的程式碼被執行，在大部分情況下，你更應該使用`t.throws()`斷言，但這是一個可以接受的用法，因為`t.throws()`只允許你斷言錯誤的`message`屬性。

### 確保一個 catch 語句發生

```js
test(t => {
    t.plan(2);

    try {
        shouldThrow();
    } catch (err) {
        t.is(err.message, 'Hello') // 如果你關心的是 message 的話使用 t.throws() 更好
        t.is(err.foo, 'bar');
    }
});
```

像上面這種`try`/`catch`的情況，使用`t.throws()`一般是個更好的選擇，但它只允許你斷言錯誤的`message`屬性。

### 確保多個 callback 被真正呼叫

```js
test.cb(t => {
    t.plan(2);

    const callbackA = () => {
        t.pass();
        t.end();
    };

    const callbackB = () => t.pass();

    bThenA(callbackA, callbackB);
});
```

上面確保`callbackB`先被呼叫（且只呼叫一次），緊接著呼叫`callbackA`，其他的組合不會滿足計劃。

### 測試帶有分支語句

在大部分情況下，在測試中使用複雜的分支是一個壞主意，一個明顯的例外是用來測試自動生成的東西（可能來自一個 JSON 文件）。下面的`t.plan()`用來確保 JSON 輸入的正確性：

```js
const testData = require('./fixtures/test-definitions.json');

testData.forEach(testDefinition => {
    test(t => {
        const result = functionUnderTest(testDefinition.input);

        // testDefinition 應該有一個`foo`或`bar`的預期而不是同時滿足它們
        t.plan(1);

        if (testDefinition.foo) {
            t.is(result.foo, testDefinition.foo);
        }

        if (testDefinition.bar) {
            t.is(result.bar, testDefinition.foo);
        }
    });
});
```

## 總結

`t.plan()`有很多有效的使用方法，但不應該被盲目使用。一個好的經驗法則是你的*測試*沒有簡單的，容易推斷的，程式碼流的話你可以使用它。測試在 callback 裡帶有斷言，`if`/`then`語句，`for`/`while`迴圈，並且（在一些情況下）`try`/`catch`塊，都可以考慮使用`t.plan()`。
