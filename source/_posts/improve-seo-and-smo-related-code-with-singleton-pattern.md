---
title: 網站可以使用 Singleton Pattern 改善 SEO (Search Engine Optimization) 與 SMO (Social Media Optimization) 相關程式碼
date: 2021-11-21 11:04:38
tags:
---
在寫 SEO/SMO 相關程式碼的時候，我發現很多使用 laravel/php 的開發者會在 layout 相關檔案這樣寫：

```
@if (isset($product))
    <title>{{ $product->name }}產品資訊</title>
    <meta name="description" content="{{ $product->description }}">

    <meta property='og:title' content="{{ $product->name }}產品資訊">
    <meta property='og:description' content="{{ $product->description }}">
@elseif (isset($category))
    <title>{{ $category->name }}類別資訊</title>
    <meta name="description" content="{{ $category->description }}">

    <meta property='og:title' content="{{ $category->name }}類別資訊">
    <meta property='og:description' content="{{ $category->description }}">
@elseif (isset($user))
    <title>{{ $user->name }}用戶資訊</title>
    <meta name="description" content="{{ $user->description }}">

    <meta property='og:title' content="{{ $user->name }}用戶資訊">
    <meta property='og:description' content="{{ $user->description }}">
@else
    <title>歡迎來到 blah 購物網站！</title>
    <meta name="description" content="亞洲最優質購物網站！">

    <meta property='og:title' content="歡迎來到 blah 購物網站！">
    <meta property='og:description' content="亞洲最優質購物網站！">
@endif
```

這種寫法對於小網站還行，網站稍大就會發現幾個明顯的問題

- layout 檔案打開，想要看個排版，卻要先看上百行的這種 seo/smo 程式碼
- 有時會希望某個 controller 底下的不同 function 回傳不同的 seo/smo meta tag，希望可以在 controller 內設定
- 有時會希望某個 controller 底下如果什麼都沒設定，預設也不要傳跟首頁一樣的 meta tag，讓 seo/smo 至少可分出大方向

古典的 singleton pattern 被許多開發者忽視許久。其實在這邊可以派上用場。

以 laravel 為例，以下是我在所有專案都會使用的程式碼

layout.blade.php

```
<title>{{ AppCore::getOpenGraphTitle() }}</title>
<meta name="description" content="{{ AppCore::getOpenGraphDescription() }}">

<meta property='og:title' content="{{ AppCore::getOpenGraphTitle() }}">
<meta property='og:description' content="{{ AppCore::getOpenGraphDescription() }}">
<meta property='og:image' content="{{ AppCore::getOpenGraphImage() }}">

<meta name="twitter:card" content="photo" />
<meta name="twitter:title" content="{{ AppCore::getOpenGraphTitle() }}" />
<meta name="twitter:description" content="{{ AppCore::getOpenGraphDescription() }}" />
<meta name="twitter:image" content="{{ AppCore::getOpenGraphImage() }}" />
```

config/app.php

```
'aliases' => [
    ...
    'AppCore' => App\AppCoreFacade::class,
],
```

app/AppCoreFacade.php

```
<?php

namespace App;

use Illuminate\Support\Facades\Facade;
use App\AppCore as RealAppCore;

class AppCoreFacade extends Facade
{
    protected static function getFacadeAccessor()
    {
        return RealAppCore::class;
    }
}
```

app/AppCore.php

```
<?php

namespace App;

class AppCore
{
    protected $openGraphTitle = null;

    protected $openGraphDescription = null;

    protected $openGraphImage = null;

    function setOpenGraphTitle($title)
    {
        $this->openGraphTitle = $title;
    }

    function setOpenGraphDescription($description)
    {
        $this->openGraphDescription = $description;
    }

    function setOpenGraphImage($image)
    {
        $this->openGraphImage = $image;
    }

    function getOpenGraphTitle()
    {
        if ($this->openGraphTitle) return $this->openGraphTitle;

        return '歡迎來到 blah 購物網站！';
    }

    function getOpenGraphDescription()
    {
        if ($this->openGraphDescription) return $this->openGraphDescription;

        return '亞洲最優質購物網站！';
    }

    function getOpenGraphImage()
    {
        if ($this->openGraphImage) return $this->openGraphImage;

        return url('/ms-icon-310x310.png');
    }
}
```

如此一來，seo/smo 的設定就變成在各個 controller 內決定了！

ProductController.php

```
function viewProduct(Request $request)
{
    AppCore::setOpenGraphTitle($product->name . '產品資訊');

    AppCore::setOpenGraphDescription($product->description);
    ...
}
```

可以在不同的 controller 內，寫各種複雜的判斷邏輯都沒關係！

也可以在 constructor 內設定適合的內容，對相關頁面做出初步的 seo/smo，接著一層一層覆蓋、做出更精準的 seo/smo 即可！

CategoryController.php

```
class CategoryController extends Controller
{
    function __construct()
    {
        AppCore::setOpenGraphTitle('分類瀏覽 - blah 購物網');

        AppCore::setOpenGraphDescription('更方便的搜尋商品');
    }

    function viewCategory(Request $request)
    {
        AppCore::setOpenGraphTitle($category->name);

        AppCore::setOpenGraphDescription($category->description);

        ...
    }

    function viewCategoryTag(Request $request)
    {
        // even I didn't setup the seo/smo, I still have better tags
        // rather than just return homepage tags!

        ...
    }
}
```

# 結語

Singleton Pattern 如果濫用，會出現一些像是 global state 的糟糕情況，讓程式碼難以維護。

但是 Singleton Pattern 在建構網站的時候有很多很棒的使用場景，本文的例子只是一個起點，你一定會找到其他也很適合的使用場景。
