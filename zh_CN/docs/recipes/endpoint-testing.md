___
**備註**

這是 [endpoint-testing.md](https://github.com/avajs/ava/blob/master/docs/recipes/endpoint-testing.md) 的簡體中文翻譯。這個[連結](https://github.com/avajs/ava/compare/89767ec3b6174e59d37faaadb50cfa3c0d58bda6...master#diff-aee54ab6a703c02779edb3ebbb35e96f) 用來檢視本翻譯與 AVA 的 master 分支是否有差別（如果你沒有看到`endpoint-testing.md`發生變化，那就意味著這份翻譯文件是最新的）。
___

# 端點測試

翻譯：[Español](https://github.com/avajs/ava-docs/blob/master/es_ES/docs/recipes/endpoint-testing.md), [Français](https://github.com/avajs/ava-docs/blob/master/fr_FR/docs/recipes/endpoint-testing.md), [Italiano](https://github.com/avajs/ava-docs/blob/master/it_IT/docs/recipes/endpoint-testing.md), [日本語](https://github.com/avajs/ava-docs/blob/master/ja_JP/docs/recipes/endpoint-testing.md), [Português](https://github.com/avajs/ava-docs/blob/master/pt_BR/docs/recipes/endpoint-testing.md), [Русский](https://github.com/avajs/ava-docs/blob/master/ru_RU/docs/recipes/endpoint-testing.md), [簡體中文](https://github.com/avajs/ava-docs/blob/master/zh_CN/docs/recipes/endpoint-testing.md)

AVA 沒有內嵌的方法可以來做端點測試，但你可以用其他斷言庫來做，讓我們用 [`supertest-as-promised`](https://github.com/WhoopInc/supertest-as-promised) 來看看。

因為測試是併發執行的，所以最好是為每個測試建立一個新的伺服器例項，如果所有測試都引用同一個例項，那例項可能會被不同的測試改變狀態。這可以在`test.beforeEach`和`t.context`裡完成，或者簡單的工廠方法：

```js
function makeApp() {
    const app = express();
    app.post('/signup', signupHandler);
    return app;
}
```

然後，將你的伺服器注入到測試超類中，主要的點是用 promise 或 async/await 語法來代替測試超類的`end`方法：

```js
test('signup:Success', async t => {
    t.plan(2);

    const res = await request(makeApp())
        .post('/signup')
        .send({email: 'ava@rocks.com', password: '123123'});

    t.is(res.status, 200);
    t.is(res.body.email, 'ava@rocks.com');
});
```
