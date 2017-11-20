# <font color="orange">Laravel 5.2</font>
這份文件主要是簡單敘述以及提到一些關於 laravel 5.2 會遇到的問題以及解決方法！

## 1. 線上專案下載後重新建構 ##
1. 進入從 git 或 bitbuckit...等其他線上版控下載下來的專案

2. (a). 如果本地端與線上端的 PHP 版本不同(如: PHP5 和 PHP7 )，則須先刪除 <font color="red">`composer.lock`</font>

	(b). 輸入 <font color="blue">`composer install`</font> ，讓專案執行安裝程序

	(c). 輸入 <font color="blue">`cp .env.example .env`</font>，複製 .env.example 成 .env

	(d). 輸入 <font color="blue">`php artisan key:generate`</font>，產生一組加密的 APP_KEY

	(e). 編輯 <font color="red">`.env`</font>，基本設定主要是 database 的部分：

	```php
	DB_DATABASE=database 的名字
	DB_USERNAME=database 的使用者
	DB_PASSWORD=database 的密碼
	```

	(f). <font color="red">如果此專案有 database</font>，輸入：<font color="blue">`php artisan migrate`</font>，若無則可略過此步驟
	
	(g). 輸入 <font color="blue">`php artisan serve`</font>，執行專案，預設路徑：<font color="red">http://localhost:8000</font>

              -----------------基本上到這邊為止就可以將一個專案執行起來-----------------

## 2. 關於 Cache ##
laravel 5.2 有時候會遇到需要清除 cache 的時候，以下列出常見清除 cache 的方法<br>

(a). <font color="red">`修改`</font> 或 <font color="red">`新增`</font> 或 <font color="red">`刪除`</font> blade 網頁，輸入：<font color="blue">`php artisan view:clear`</font>

(b). 變更 <font color="red">`.env`</font>，輸入：<font color="blue">`php artisan config:cache`</font>

(c). 變更 <font color="red">`route.php`</font>，輸入：<font color="blue">`php artisan route:clear`</font> (個人覺得不太需要使用到)

(d). 變更一些資料的內容並希望能夠及時更新，輸入：<font color="blue">`php artisan cache:clear`</font>

## 3. 關於 Model ##
1. 單獨建立新 model (例如 User )，輸入 <font color="blue">`php artisan make:model User`</font><br>，就會在<font color="red">`database/migrations`</font> 生成 users 資料表

2. 建立 model (例如 User ) 時，想要一起產生一個資料庫遷移，輸入 <font color="blue">`php artisan make:model User -m`</font>

3. 如果想要在目前的 model 裡增加 name 欄位，則直接執行方法 1，重點在下面紅色標記的 create 要改成 table

	> <font color="red">修改前</font>

	```php
	Schema::create('votes', function (Blueprint $table) {
		$table->string('name');
	});

	```
	> <font color="green">修改後</font>

	```php
	Schema::table('votes', function (Blueprint $table) {
		$table->string('name');
	});

	```

4. 關於 [Model](https://laravel.tw/docs/5.2/eloquent) 更詳細的內容

## 3. 關於 Controller ##
1. 建立新 Controller (例如 UserController )，輸入 <font color="blue">`php artisan make:controller UserController`</font>就會在<font color="red">`app/Http/Controllers`</font> 生成 UserController

2. 關於 [Controller](https://laravel.tw/docs/5.2/controllers) 更詳細的內容

## 4. 關於 Request ##
1. 建立新 Request (例如 UserRequest )，輸入 <font color="blue">`php artisan make:request UserRequest`</font>就會在<font color="red">`app/Http/Requests`</font> 生成 UserRequest

2. 關於 [Request](https://laravel.tw/docs/5.2/requests) 更詳細的內容

## 5. 關於 route ##
laravel 5.2 的 route 都統一寫在 <font color="red">`app/Http`</font> 底下的 <font color="blue">`route.php`</font><br>

1. 例如回到首頁的路由 ( '/' )

	```php
	Route::get('/'), function() {
		return '這是首頁'};
	```

2. 如有使用 blade 模板開發網頁(例如：main.blade.php )，可寫成如下：

	```php
	Route::get('/'), function() {
		return view('main')};
	```
3. 當有多組相同的前綴路徑，可以將其 Group 起來，例如有('/api/main'，'/api/info'，'/api/article')，可寫成如下：

	```php
	Route::group(['prefix' => 'api'], function () {
		Route::get('main', function(){
		return view('main')};

		Route::get('info',function(){
		return view('info')};

		Route::get('article',function(){
		return view('article')};
	});
	```

4. 當有多組相同的前綴路徑，且所使用的 Controller 也一樣，可改寫成如下較為簡潔的寫法

	```php
	Route::group(['prefix' => 'api'], function () {
		Route::get('main', UserController@main);
		Route::get('info', UserController@info);
		Route::get('article', UserController@article);
	});
	```
        <font color="red">***其中 @main,@info,@article 個別代表 UserController 裡的 public function***</font>

5. 關於 [route](https://laravel.tw/docs/5.2/routing) 更詳細的內容

## 5. 如何在 Blade 使用 Form & HTML 語法 ##
要能夠在 blade 使用 `{!!Form!!}` 必須要安裝套件，方法如下：<br>

1. 輸入 <font color="blue">`composer require "laravelcollective/html":"5.2.*"`</font>

2. 新增 provider 到 <font color="red">`config/app.php`</font> 裡面，如下所示：

	```php
	'providers' => [
		Collective\Html\HtmlServiceProvider::class,
	],
	```

3. 新增 aliases 到 <font color="red">`config/app.php`</font> 裡面，如下所示：

	```php
	'providers' => [
		'Form' => Collective\Html\FormFacade::class,
		'Html' => Collective\Html\HtmlFacade::class,
	],
	```

4. 關於 [Blade 模板](https://laravel.tw/docs/5.2/blade) 更詳細的內容

## 6. laravel 使用 Google 或 Facebook 登入 ##
laravel 有一個套件叫做 [laravel/socialite](https://github.com/laravel/socialite/tree/2.0)，已經滿完美的結合 Google，Facebook，Twiter，GitHub 和 Twiter 的登入系統，以下將會簡單示範如何使用 Facebook 登入

1. 請先到 [Facebook developer](https://developers.facebook.com/docs/facebook-login/) 申請登入系統 API<br>
教學連結：[如何申請建立 Facebook APP ID 應用程式ID](https://sofree.cc/apply-facebook-app-id/)(此篇教學為網路上搜尋，引用自--香腸炒魷魚)

2. 輸入：<font color="blue">`composer require laravel/socialite`</font>

3. 新增 provider 到 <font color="red">`config/app.php`</font>

	```php
	'providers' => [
		Laravel\Socialite\SocialiteServiceProvider::class,
	],
	```
4. 新增 aliases 到 <font color="red">`config/app.php`</font> 裡面，如下所示：

	```php
	'providers' => [
		'Socialite' => Laravel\Socialite\Facades\Socialite::class,
	],
	```
5. 新增以下三段程式碼到 <font color="red">`config`</font>底下的 <font color="blue">services.php</font><br>

	```php
	'facebook' => [
	    'client_id' => env('FB_CLIENT_ID'),
	    'client_secret' => env('FB_CLIENT_SECRET'),
	    'redirect' => env('FB_REDIRECT')
	],
	```
6. 編輯 <font color="red">`.env`</font>，在最下面新增您在 Facebook 申請的登入資訊，會有以下三項：

	```php
	FB_CLIENT_ID=填入您所申請的 fb_client_id
	FB_CLIENT_SECRET=填入您所申請的 fb_app_screct
	FB_REDIRECT=填入您要 redirect 的路徑
	```

7. 創建一個 Controller，這邊以 LoginController 命名，輸入：<font color="blue">`php artisan make:controller FacebookController`</font>

8. 在 LoginController 新增以下兩個 function：

	```php
	class FacebookController extends Controller
	{
		public function redirect()
		{
			return Socialite::driver('facebook')->redirect();
		}

		public function callback()
		{
			$user = Socialite::driver('facebook')->user();
			dd($user);
		}
	}
	```

9. 接著在 <font color="blue">`route.php`</font> 新增以下 2 組路由

	```php
	Route::get('auth/facebook', 'FacebookController@redirect')->name('google.login');
	Route::get('auth/facebook/callback', 'FacebookController@callback');
	```

10. 執行 <font color="blue">`php artisan serve`</font>，打開瀏覽器輸入：<font color="red">`http://localhost:8000/facebook`</font>，就會順利看到 Facebook callback 回來的登入資訊，如下圖所示：

	![登入資訊](https://user-images.githubusercontent.com/25587423/33012053-b6434012-ce1a-11e7-8e1c-313855a0edd0.png)
> 此資訊表示 Facebook 登入成功
> <br>
> Google 登入步驟跟 Facebook 登入一模一樣，只需將上述 facebook 的地方改成 google 即可