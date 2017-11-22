# <font color="orange">Laravel 5.2</font>

這份文件主要是簡單敘述以及提到一些關於 laravel 5.2 會遇到的問題以及解決方法！

<font color="red">內容會不斷更新</font>

## Change History
| Date       | Description                  | Author  |
| ---------- | ---------------------------- | ------- |
| 2017-11-20 | 新增--1. 線上專案下載後重新建構  | blaze0207 |
|            | 新增--2. 關於 Cache           | blaze0207 |
|            | 新增--3. 關於 Model           | blaze0207 |
|            | 新增--4. 關於 Controller      | blaze0207 |
|            | 新增--5. 關於 Request         | blaze0207 |
|            | 新增--6. 關於 route           | blaze0207 |
|            | 新增--7. 如何在 Blade 使用 Form & HTML 語法 | blaze0207 |
|            | 新增--8. laravel 使用 Google 或 Facebook 登入 | blaze0207 |
| 2017-11-21 | 新增--9. 如何自訂義驗證規則     | blaze0207 |
|| 新增--10. 如何利用 faker 產生測試資料 | blaze0207 |

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

## 4. 關於 Controller ##
1. 建立新 Controller (例如 UserController )，輸入 <font color="blue">`php artisan make:controller UserController`</font>就會在<font color="red">`app/Http/Controllers`</font> 生成 UserController

2. 關於 [Controller](https://laravel.tw/docs/5.2/controllers) 更詳細的內容

## 5. 關於 Request ##
1. 建立新 Request (例如 UserRequest )，輸入 <font color="blue">`php artisan make:request UserRequest`</font>就會在<font color="red">`app/Http/Requests`</font> 生成 UserRequest

2. 關於 [Request](https://laravel.tw/docs/5.2/requests) 更詳細的內容

## 6. 關於 route ##
laravel 5.2 的 route 都統一寫在 <font color="red">`app/Http`</font> 底下的 <font color="blue">`route.php`</font><br>

1. 例如回到首頁的路由 ( '/' )

	```php
	Route::get('/'), function() {
		return '這是首頁'
	};
	```

2. 如有使用 blade 模板開發網頁(例如：main.blade.php )，可寫成如下：

	```php
	Route::get('/'), function() {
		return view('main')
	};
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

## 7. 如何在 Blade 使用 Form & HTML 語法 ##
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

## 8. laravel 使用 Google 或 Facebook 登入 ##
laravel 有一個套件叫做 [laravel/socialite](https://github.com/laravel/socialite/tree/2.0)，已經滿完美的結合 Google，Facebook，Twiter，GitHub...等登入系統，以下將會簡單示範如何使用 Facebook 登入

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

7. 創建一個 Controller，這邊以 FacebookController 命名，輸入：<font color="blue">`php artisan make:controller FacebookController`</font>

8. 在 FacebookController 新增以下兩個 function：

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

## 9. 如何自定義驗證規則 (範例:年齡驗證) ##
在 laravel 5.2 中提供了可以自定義驗證規則的方法，以下我用簡單的年齡是否滿十八歲來當作範例

1. 首先到 <font color="red">`app/Providers`</font>，底下有一個檔案 <font color="blue">`AppServiceProvider.php`</font>


2. 內容預設如下：

	```php

	class AppServiceProvider extends ServiceProvider
	{
	    public function boot()
	    {

	    }

	    public function register()
	    {
	        //
	    }
	}

	```

3. 寫入關於年齡驗證的規則與回應，以現在 <font color="red">2017</font> 當標準，也就是說 <font color="red">19991231</font> 後出生的人都算未滿十八歲
	> 記得在最上面要引入<font color="blue">`use Illuminate\Support\Facades\Validator;`</font>，不然會報錯說 Validator 找不到或是未定義<br>
	> <font color="blue">birthday </font>：自定義規則的名稱<br>
	> <font color="blue">$attribute</font>：要被驗證的屬性名<br>
	> <font color="blue">$value </font>：要被驗證的值<br>

	```php
	class AppServiceProvider extends ServiceProvider
		{
		    public function boot()
		    {
				Validator::extend('birthday', function($attribute, $value) {
		            if (! is_string($value) && ! is_numeric($value)) {
		                return false;
		            }

		            $value > 19991231 ? false : true
		        });
		    }

		    public function register()
		    {
		        //
		    }
		}
	```
	到這邊基本上一個簡易的年齡驗證就完成了，剩下就是去 request 裡面直接使用，如下所示

4. 到 <font color="red">`app/Http/Requests`</font>，建立 <font color="blue">UserRequests.php</font>，可以直接用前面自定義好的 <font color="green">`'birthday'`</font> 驗證規則，如下：

	```php
	public function rules()
	{
		return [
			'birthday' => ['required', 'birthday'],
		];
	}

	public function messages() {
		return [
			'birthday.required' => '出生日期不能為空',
			'birthday.birthday' => '您未滿十八歲'
		];
	}
	```
5. 關於 [自訂驗證規則](https://laravel.tw/docs/5.2/validation#custom-validation-rules) 更詳細的內容

## 10. 如何利用 faker 產生測試資料 ##
laravel 5.2 提供了一個功能 [fzaninotto/Faker](https://github.com/fzaninotto/Faker)，讓開發者可以快速產生測試資料，對於開發中的測試及網頁顯示非常的便利，以下我會用基本的 User 資料來當作範例

1. 建立一個新的專案，輸入：<font color="blue">`composer create-project laravel/laravel learn-factory "5.2.*"`</font>

2. 編輯 <font color="red">`.env`</font>

	```php
	DB_DATABASE=database 的名字
	DB_USERNAME=database 的使用者
	DB_PASSWORD=database 的密碼
	```

3. 更新 faker，輸入：<font color="blue">`composer require fzaninotto/faker`</font>

4. 進入 <font color="red">`database/migrations`</font>，修改 laravel 在創建的時候預設的 <font color="red">users</font> 資料表，修改如下：

	```php
	class CreateUsersTable extends Migration
	{
		public function up()
		{
			Schema::create('users', function (Blueprint $table) {
				$table->increments('id');
				$table->string('name');
				$table->string('phone');
				$table->string('email')->unique();
				$table->string('address');
				$table->timestamps();
			});
		}

		public function down()
		{
			Schema::drop('users');
		}
	}
	```
	
5. 進入 <font color="red">`database/factories`</font>，修改 <font color="red">`ModelFactory.php`</font>，如下：
	> 這邊主要是去定義假資料的型態以及內容，更詳細有關 <font color="blue">$faker</font> 有哪些方法可用，可以直接看原始碼 <font color="blue">`Generator.php`</font>，路徑：<font color="red">`/vendor/fzaninotto/faker/src/Faker/Generator.php`</font>

	```php
		$factory->define(App\User::class, function (Faker\Generator $faker) {
			return [
				'name' => $faker->name,
				'phone' => '09'.$faker->randomNumber(8, true),
				'email' => $faker->unique()->safeEmail,
				'address' => $faker->address,
			];
		});
	```
	
6. 在這邊我們需要指定測試資料的呈現方式是 <font color="red">中文</font>，於是我們到 <font color="red">`app/Providers`</font>，修改<font color="blue">`AppServiceProvider.php`</font> 如下：

	> 引入 <font color="red">`use Faker\Generator as FakerGenerator;`</font>
	
	> 引入 <font color="red">`use Faker\Factory as FakerFactory;`</font>

	```php
	public function boot()
    {
        $this->app->singleton(FakerGenerator::class, function() {
            return FakerFactory::create('zh_TW');
        });

    }
	```

7. 輸入：<font color="blue">`php artisan migrate`</font> 建立 table 
8. 輸入：<font color="blue">`php artisan tinker`</font> 進入 laravel 內建的 REPL (read-eval-print-loop)，REPL是指交互式命令行界面
9. 進入後，輸入： <font color="blue">`namespace App`</font>，接著再輸入：<font color="blue">`factory(User::class, 5)->make();`</font>，如果有產生如下圖的資料就表示一切都正確接上囉！

	![正確](https://i.imgur.com/tQZARfp.png)
	> <font color="red">**注意這邊我是先用 make() 來確定一切正確無誤**</font>

10. 最後要寫入資料庫則是要改成輸入：<font color="blue">`factory(User::class, 5)->create();`</font>， 如果一切正確無誤就會如下圖所示：

	![正確寫入資料庫](https://i.imgur.com/Mgn1w0V.png)
	
	![正確寫入資料庫](https://i.imgur.com/vXAlyZW.png)