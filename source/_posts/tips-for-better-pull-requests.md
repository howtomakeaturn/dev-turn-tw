---
title: 關於 PR (Pull Request) 的改善提案
date: 2021-11-16 17:01:10
tags:
---
前陣子寫了一則簡單的 PR 改善提案，給合作的其中一個技術團隊。

順手分享一下，或許會有幫助。

## 當前問題

現在發出 PR 之後，幾乎只有 author 跟 reviewer 會互相討論，然後就 merge 了。

其他人可能也對各種 PR 有興趣，但是沒辦法參與討論。因為：

1. author 創建 PR 的時候腦中只想著 reviewer
2. 所以其他人打開 PR 不太確定「正在解決什麼問題」
3. 其他人打開各種 PR 的時候，需要都 git pull 到本機然後 db migrate 才有辦法知道 PR 內容

## 改善目標

希望發出 PR 之後，讓更多的 developers 有機會參與討論。

不只是 author 跟 reviewer 兩個人互相討論就結束。

讓發 PR 變成一種「分享我的作品」「歡迎大家來欣賞」「一起給點意見」的感覺。

## 改善方法

一、要讓看 PR 的人不用 git pull 就有信心可以按下 merge

發 PR 的人請在 PR 內附上簡單幾張「螢幕截圖」或者「螢幕錄影」，稍微證明一下這個 PR 不是亂寫的即可。

二、要讓看 PR 的人不用四處打聽、詢問，就知道這個 PR 的目標

發 PR 的人請在 PR 內寫清楚這個 PR 在解決什麼問題。

跟這個 PR 相關的連結、討論文件會議記錄、手寫筆記拍照，都可以一併附上來。

三、責任的主要歸屬

發 PR 的 author 是需要對 PR 主要負責的人。不管有多少 developer 七嘴八舌地參與討論，在 reviewer 按下 merge 之後，這些七嘴八舌 developer 跟 reviewer 只是給意見而已，主要還是 PR author 確保 PR 可靠。

## 範例

沒有任何公版或是格式需要套用。發揮創意達成以上效果即可。

以下隨便舉例：

```
# Summary

- this pr wants to solve blah blah
- [link to a hackmd note]
- [link to another note]
- [link to external references]
- this closes #18
- this closes #19

# Screenshots

[IMG1]
[IMG2]
[IMG3]

# Notes & Todos

- `blah blah` needs more refactoring in the future
- some technical debts exist in `blah blah`
```

## 補充說明

- 此提案只是大方向，不需要死板執行。
- 如果是趕時間的 hotfix 或是很小的 tweak，其實照原本方式趕快找個人 review 就 merge 即可。
- 在 PR 比較大、這 PR 有點像心血結晶、甚至像藝術作品一樣想分享給更多 developer 的時候，再這樣做即可。
- 實務上通常會配合 issue tracker 執行，就是 PR 內會寫明跟哪些 issue 有關。
- 預期這會讓創建 PR 的時間增加不少。
- 創建 PR 跟寫 code 原理類似：花越多時間去寫好，會讓這 PR 變得更 independent。沒有上下文 context 的 dependency。讓看 PR 的人可以獨立直接消化掉。
- 實務上 open source project 就是這樣執行的。下次逛 github 的時候可以留意一下人家 PR 的寫法。

------

特別感謝 Bible Tang 對這篇文章的啟發。
