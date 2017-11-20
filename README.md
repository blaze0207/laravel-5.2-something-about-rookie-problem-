# <font color="orange">Laravel-5.2</font>
這份文件主要是簡單敘述以及提到一些關於 laravel 5.2 會遇到的問題以及解決方法！

## 1. 線上專案下載後重新建構 ##
1. 進入從 git 或 bitbuckit...等其他線上版控下載下來的專案

2. <br>
(a). 如果本地端與線上端的 PHP 版本不同(如: PHP5 和 PHP7 )，則須先刪除 composer.lock
<br>
<br>
(b). 輸入 <font color="blue">`composer install`</font> ，讓專案執行安裝程序
<br>
<br>
(c). 輸入 <font color="blue">`cp .env.example .env`</font>，複製 .env.example 成 .env
<br>
<br>
(d). 輸入 <font color="blue">`php artisan key:generate`</font>，產生一組加密的 APP_KEY
<br>
<br>
(e). 編輯 `.env`，基本的設定主要是 database 的部分：
<br>
              <font color="blue">`DB_DATABASE=輸入 database 的 名字`</font><br>
              <font color="blue">`DB_USERNAME=輸入 database 的 使用者`</font><br>
              <font color="blue">`DB_PASSWORD=輸入 database 的 密碼`</font><br>
<br>
(f). 輸入 php artisan serve，執行專案，預設路徑：http://localhost:8000
<br>

-----------------基本上到這邊為止就可以將一個專案執行起來-----------------

## 2. 關於 Cache ##
laravel 5.2 有時候會遇到需要清除 cache 的時候，以下列出常見清除 cache 的方法<br>

1.<br>
狀況：<font color="red">`修改`</font> 或 <font color="red">`新增`</font> 或 <font color="red">`刪除`</font> blade 網頁<br>
輸入：<font color="blue">`php artisan view:clear`</font><br>

2.<br>
狀況：變更 <font color="red">`.env`</font><br>
輸入：<font color="blue">`php artisan config:cache`</font><br>

3.<br>
狀況：變更 <font color="red">`route.php`</font><br>
輸入：<font color="blue">`php artisan route:clear`</font> (個人覺得不太需要使用到)<br>

4.<br>
狀況：變更一些資料的內容並希望能夠及時更新<br>
輸入：<font color="blue">`php artisan cache:clear`</font><br>