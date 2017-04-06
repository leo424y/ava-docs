___
**備註**

這是 [contributing.md](https://github.com/avajs/ava/blob/master/contributing.md) 的簡體中文翻譯。這個[連結](https://github.com/avajs/ava/compare/89767ec3b6174e59d37faaadb50cfa3c0d58bda6...master#diff-cc4aac3e9be04e0413c9520f223b493c) 用來檢視本翻譯與 AVA 的 master 分支是否有差別（如果你沒有看到`contributing.md`發生變化，那就意味著這份翻譯文件是最新的）。
___
# 向 AVA 貢獻

✨ 感謝向 AVA 作出貢獻！ ✨

請注意，這個項目釋出帶有[貢獻者的行為準則](code-of-conduct.md)，參與這個項目你需要同意並遵守其中的條款。

翻譯：[Español](https://github.com/avajs/ava-docs/blob/master/es_ES/contributing.md), [Français](https://github.com/avajs/ava-docs/blob/master/fr_FR/contributing.md), [Italiano](https://github.com/avajs/ava-docs/blob/master/it_IT/contributing.md), [日本語](https://github.com/avajs/ava-docs/blob/master/ja_JP/contributing.md), [Português](https://github.com/avajs/ava-docs/blob/master/pt_BR/contributing.md), [Русский](https://github.com/avajs/ava-docs/blob/master/ru_RU/contributing.md), [簡體中文](https://github.com/avajs/ava-docs/blob/master/zh_CN/contributing.md)

## 我怎麼貢獻？

### 改進文件

作為 AVA 的使用者你是幫助我們改進文件的最佳候選人，修改拼寫錯誤，修復錯誤，更好的解釋，更多的例子等等。為一些可以改進的事情提問題，[幫助我們翻譯文件](https://github.com/avajs/ava-docs)，不管任何事情，即使是改善現在這個文件。

### 改善問題

一些問題在建立時缺少資訊，不能重現，描述太過簡單無效，幫助我們將他們變得更容易處理，因為處理問題需要大量時間，我們情願將時間花在修復缺陷和新增新功能上面。

### 在問題中給出反饋

我們總是在問題跟蹤器上尋找更多的討論意見，這是一個影響 AVA 未來發展方向的好機會。

### 在我們的聊天室中閒聊

我們有一個[聊天室](https://gitter.im/avajs/ava)，可以進到裡面潛水，跟我們聊天，或者幫助其他人。

### 提交問題

- 問題跟蹤器是針對問題的，請使用我們的[聊天室](https://gitter.im/avajs/ava) 或者 [Stack Overflow](https://stackoverflow.com/questions/tagged/ava) 來尋求支援。
- 在新開一個問題之前先搜尋以前的問題。
- 確保你使用的是最新版本的 AVA。
- 用一個清晰和描述性好的標題。
- 包含儘可能多的資訊：重現問題的步驟，錯誤的資訊，Node.js 的版本，作業系統等。
- 在問題的描述上你花越多時間，越多的資訊將給到我們。
- [最好的問題提交方式是提供一個失敗的測試案例。](https://twitter.com/sindresorhus/status/579306280495357953)

### 提交一個 pull rquest

- 重大的修改最好是先開一個問題來進行討論，這樣可以避免你做一些不必要的工作。
- 對於長期遠大的任務，你應該將你所做的工作在社羣中提出來並儘快得到反饋，儘快開一個能證明你想法的最簡版本的 pull request。在前期，不需要把事情做得完美，或者 100% 完成，只需要在標題新增一個 [WIP] 字首，然後描述哪些工作你需要繼續做的。這樣評審人員就不會挑剔其中的小細節或者指出哪些你已經知道的改進點。
- 新功能應該具備測試和文件。
- 不要包含不相關的修改。
- 在提交 pull request 之前檢查程式碼和執行測試，通過執行`$ npm test`命令來完成。
- 在一個[主題分支](https://github.com/dchelimsky/rspec/wiki/Topic-Branches) 中提交 pull request 而不是 master 分支。
- 為 pull request 和 commit 使用一個清晰和描述性強的標題。
- 寫一個讓人信服的描述來說明為什麼我們要接受你的 pull request。說服我們是你的工作，需要回答“為什麼”並提供用例。
- 你可能被要求修改你的 pull request，但絕對沒有必要去新開一個 pull request，[只需要更新原來那個就可以了。](https://github.com/RichardLitt/docs/blob/master/amending-a-commit-guide.md)
