---
title: 前端框架，business state 與 UI state
date: 2021-12-12 07:11:01
tags:
---
「為甚麼要使用前端框架？直接用 jQuery 寫完前端不行嗎？」

很多新手 web developer 會問這個問題。

其實在很多情況下，直接用 jQuery 是沒什麼問題的。

至於什麼情況下可以？什麼情況下不行？

這就需要有 business state 與 UI state 的觀念來幫助判斷。

# business state 與 UI state 的定義

不論 html / css 也就是 UI 外觀如何，在任何頁面的背後，都會有類似這樣的物件，可以表達目前頁面「商業邏輯會用到的狀態」

```
{
  items: [
    {
        name: 'product 1',
        price: 100,
    },
    {
        name: 'product 2',
        price: 300,
    },
  ]
  sum: 400
}
```

這種「狀態」我稱之為 business state。

以上是一個購物網站頁面常常有的 business state。

與之相關的，在這種頁面會有這樣的 html /css

```
<div>
    <div>Product 1: $100</div>
    <div>Product 2: $300</div>
    <hr />
    <div>You total cost: $400</div>
</div>
```

這種「畫面上看到的狀態」我稱之為 UI state。

# business state 與 UI state 的關係

完全新手在寫網頁的時候，可能根本沒寫到 business state 的那種 json object。

但就算你只有寫 html / css，其實背後也有隱含著 business state，只是每次你需要的時候都直接去 DOM 裡面 parse 出來而已。

當一個頁面的 business state 與 UI state 是 1-to-1 關係的時候，其實用 jQuery 快速寫完這頁面就可以了。

前端網頁開發麻煩的地方在於，很多時候他們不是 1-to-1 的關係，而是 1-to-many。

同樣的購物車 business state，在畫面上可能有 navbar 的彈出式購物車，同時畫面中央的商品庫存數量要跟著增減。

當這類的關係複雜起來的時候，如果只使用 jQuery，那在每次更新 business state 的時候，還要記得去更新很多地方的 UI state。

互動情況複雜的時候，很容易就會出現 bug，所以在這種情況，就會需要前端框架的幫助。

# One way data flow

前端框架至少會提供「One way data flow」的功能，也就是你只要把 business state 整理好、準備好、顧好，再講清楚 business state -> UI state 的對應，框架就會自動幫你把 html 自動更新。

我曾經對此寫過一篇詳細的跨框架分析比較文章：

[簡單聊一下 one-way data flow、two-way data binding 與前端框架
](https://devs.tw/post/40)

# 結語

業界很多網頁的狀況其實很單純，business state 與 UI state 都是很小、很單純的 1-to-1 對應。

這種情況下，根本不需要用到前端框架，切記不要為了用而用一個工具。

當代前端框架的功能遠不止於此，但是 business state 與 UI state 的觀念卻是每一個前端人必備的觀念。

在判斷一個中小型 web application 需不需要引入前端框架的時候，這個觀念也會有非常大的幫助。
