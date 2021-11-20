---
title: Global Functions 還是非常好用，沒必要完全不寫
date: 2021-11-20 16:09:18
tags:
---
近幾年跟不同團隊合作，我發現大家似乎都不再使用 global functions 了。

前人的經驗告訴我們，濫用 global functions 會讓程式碼變得很難維護。

大家有改過 wordpress 的 source code 嗎？

滿滿的 global functions。非常難維護，但是功能強大。

其實，只要稍微注意一些原則，適當的使用，還是非常好用的。

以 Laravel 為例，內建有提供一堆好用的 global functions

https://laravel.com/docs/5.8/helpers

在講求開發速度的現代，這種寫了就用的簡單函式，當然也可以自己多寫幾個。

我在使用 Laravel 開發的時候，所有專案都會在 composer.json 加上這段

```
"autoload": {
    ...
    "files": [
        "app/helpers.php"
    ]
},
```

然後在 helpers.php 裡面就可以快速寫點輔助函式

```
<?php

// NOTE @howtomakeaturn: if you just want to find a place to write
// some functions quickly, this is the place!
// you can always refactor them later!

function testFunction1()
{
    return 'test 1';
}

function testFunction2()
{
    return 'test 2';
}
```

在兩種情況下，這個檔案特別好用

# 1. utility function 輔助函式

字串處理、陣列處理，之類的，目的單純、會常常用到的函式，放這裡非常適合。

或者你就想放一些 global shared 的 value，但是懶得使用 framework 的 config 機制時，放這也很適合。

# 2. 沒時間做好 abstraction 抽象化，但又想找地方放、才有重用性

有些邏輯，一時想不清楚該放到哪個類別、使用什麼 design pattern。

這種時候，隨便寫個 function 就是了。之後要重構也很方便。

不要小看這種寫法，在古早時代，整個系統都是這樣寫的。稍微寫一些也無妨。

那麼有什麼注意事項呢？

# 不要有 state，不要有 side-effect

讓函式單純一點，每次傳同樣 input 都會得到同樣 output。

除此之外，不要有 side-effect。也就是 function 內不要去更新到 function 外部的 state。

讓 call 這些 function 成為一件單純的事情。

# 結語

如果不太確定哪些邏輯適合寫成 global functions，參考 laravel 的 helpers 就對了。

適當的寫點 function 來幫助建構你的系統，還是非常好用的，沒必要完全不寫。
