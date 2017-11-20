# <font color="orange">Laravel-5.2</font>
這份文件主要是簡單敘述以及提到一些關於 laravel 5.2 會遇到的問題以及解決方法！

## 1. 線上專案下載後重新建構 ##
1. 進入從 git 或 bitbuckit...等其他線上版控下載下來的專案

2. <br>
(a). 如果本地端與線上端的 PHP 版本不同(如: PHP5 和 PHP7 )，則須先刪除 <font color="red">`composer.lock`</font>
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
(e). 編輯 <font color="red">`.env`</font>，基本設定主要是 database 的部分：
<br>
              `DB_DATABASE= database 的名字`<br>
              `DB_USERNAME= database 的使用者`<br>
              `DB_PASSWORD= database 的密碼`<br>
<br>
(f). 輸入 <font color="blue">`php artisan serve`</font>，執行專案，預設路徑：<font color="red">http://localhost:8000</font>
<br>

              -----------------基本上到這邊為止就可以將一個專案執行起來-----------------

## 2. 關於 Cache ##
laravel 5.2 有時候會遇到需要清除 cache 的時候，以下列出常見清除 cache 的方法<br>

(a).<br>
狀況：<font color="red">`修改`</font> 或 <font color="red">`新增`</font> 或 <font color="red">`刪除`</font> blade 網頁<br>
輸入：<font color="blue">`php artisan view:clear`</font><br>

(b).<br>
狀況：變更 <font color="red">`.env`</font><br>
輸入：<font color="blue">`php artisan config:cache`</font><br>

(c).<br>
狀況：變更 <font color="red">`route.php`</font><br>
輸入：<font color="blue">`php artisan route:clear`</font> (個人覺得不太需要使用到)<br>

(d).<br>
狀況：變更一些資料的內容並希望能夠及時更新<br>
輸入：<font color="blue">`php artisan cache:clear`</font><br>

## 3. 關於 Model ##
1. 單獨建立新 model(例如 User )，輸入 <font color="blue">`php artisan make:model User`</font><br>，就會在<font color="red">`database/migrations`</font> 生成 users 資料表
<br><br>
2. 建立 model (例如 User ) 時，想要一起產生一個資料庫遷移，輸入 <font color="blue">`php artisan make:model User -m`</font><br><br>
3. 如果想要在目前的 model 裡增加 name 欄位，則直接執行方法 1，重點在下面紅色標記的 create 要改成 table <br><br>
修改前：
        `Schema::`<font color="red">`create`</font>`('votes', function (Blueprint $table) {`<br>
         `$table->string('name');`<br>
`});`
<br><br>
修改後：
        `Schema::`<font color="red">`table`</font>`('votes', function (Blueprint $table) {`<br>
         `$table->string('name');`<br>
`});`

## 3. 關於 Controller ##
1. 建立新 Controller (例如 UserController )，輸入 <font color="blue">`php artisan make:controller UserController`</font><br>就會在<font color="red">`app/Http/Controllers`</font> 生成 UserController

## 4. 關於 Request ##
1. 建立新 Request (例如 UserRequest )，輸入 <font color="blue">`php artisan make:request UserRequest`</font><br>就會在<font color="red">`app/Http/Requests`</font> 生成 UserRequest 

## 5. 關於 route ##
laravel 5.2 的 route 都統一寫在 <font color="red">`app/Http`</font> 底下的 <font color="blue">`route.php`</font><br>

1. 例如回到首頁的路由 ( '/' )，<br>
`Route::get('/'), function(){
return '這是首頁'};`<br>

2. 如果有使用 blade 網頁，可寫成如下：<br>
`Route::get('/'), function(){
return view('main')};`<br>

3. 多個一樣的路徑，可以將其 Group 起來，例如有('/api/main'，'/api/info'，'/api/article')，可寫成如下：<br>
`Route::group(['prefix' => 'api'], function () {`<br>
        `Route::get('main', function(){
return view('main')};`<br>
        `Route::get('info',function(){
return view('info')};`<br>
        `Route::get('article',function(){
return view('article')};`<br>
`});`

4. 多個一樣的路徑，所使用的 controller 也一樣的話，可以改寫成如下較為簡潔的寫法<br>
`Route::group(['prefix' => 'api'], function () {`<br>
        `Route::get('main', UserController@main);`<br>
        `Route::get('info', UserController@info);`<br>
        `Route::get('article', UserController@article);`<br>
`});`<br>
        <font color="red">***其中 @main,@info,@article 個別代表 UserController 裡的 public function***</font>