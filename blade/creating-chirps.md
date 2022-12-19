[TOC]

# <b>03.</b> Chirps を作ってみよう！

<!-- You're now ready to start building your new application! Let's allow our users to post short messages called *Chirps*. -->

これで、新しいアプリケーションの構築を開始する準備が整いました! ユーザーが **Chirps** という短いメッセージを投稿できるようにしましょう。

## モデル、マイグレーション、およびコントローラー

<!-- To allow users to post Chirps, we will need to create models, migrations, and controllers. Let's explore each of these concepts a little deeper: -->

ユーザーが Chirp を投稿できるようにするには、モデル、マイグレーション、およびコントローラーを作成する必要があります。これらの各概念をもう少し詳しく見てみましょう。

* [Models](https://laravel.com/docs/eloquent)  
モデルは、データベース内のテーブルを操作するための強力で楽しいインターフェイスを提供します。
* [Migrations](https://laravel.com/docs/migrations)  
マイグレーションにより、データベース内のテーブルを簡単に作成および変更できます。アプリケーションが実行されるすべての場所に同じデータベース構造が存在することを保証します。
* [Controllers](https://laravel.com/docs/controllers)  
コントローラーは、アプリケーションに対する要求を処理し、応答を返す役割を果たします。

<!-- Almost every feature you build will involve all of these pieces working together in harmony, so the `artisan make:model` command can create them all for you at once. -->
構築するほぼすべての機能には、これらのすべての要素が調和して動作することが含まれるため、`artisan make:model` コマンドを使用すると、それらすべてを一度に作成できます。

<!-- Let's create a model, migration, and resource controller for our Chirps with the following command: -->
次のコマンドを使用して、Chirp のモデル、移行、およびリソース コントローラーを作成しましょう。

```shell
php artisan make:model -mrc Chirp
```

> **Note**
> 「 php artisan make:model --help 」コマンドを実行すると、使用可能なすべてのオプションを表示できます。

このコマンドにより、次の 3 つのファイルが作成されます。

* `app/Models/Chirp.php` 
Eloquent (エロクワント) モデル
* `database/migrations/<timestamp>_create_chirps_table.php`
データベース テーブルを作成するマイグレーション ファイル
* `app/Http/Controller/ChirpController.php`
リクエストを受け取ってレスポンスを返す HTTP コントローラー

このコマンドにより、次の 3 つのファイルが作成されます。

## ルーティング

<!-- We will also need to create URLs for our controller. We can do this by adding "routes", which are managed in the `routes` directory of your project. Because we're using a resource controller, we can use a single `Route::resource()` statement to define all of the routes following a conventional URL structure. -->

コントローラーへの URL を作成する必要があります。これを行うには、プロジェクトの `routes` ディレクトリで管理されるファイルにルートを定義する作業をします。リソース コントローラーを使用しているため、単一の `Route::resource()` ステートメントを使用して、従来の URL 構造に従うすべてのルートを定義できます。

<!-- To start with, we are going to enable two routes: -->
まず、2 つのルートを有効にします。

<!-- * The `index` route will display our form and a listing of Chirps. -->
* `index` ルートには、フォームとチャープのリストが表示されます。
<!-- * The `store` route will be used for saving new Chirps. -->
* `store` ルートは、新しい Chirps を保存するために使用されます。

<!-- We are also going to place these routes behind two [middleware](https://laravel.com/docs/middleware): -->
また、これらのルートを 2 つの [ミドルウェア](https://laravel.com/docs/middleware) の背後に配置します。

<!-- * The `auth` middleware ensures that only logged-in users can access the route. -->
* `auth` ミドルウェアは、ログインしたユーザーのみがルートにアクセスできるようにします。
* 電子メール検証を有効にする場合、[verified ミドルウェア](https://laravel.com/docs/verification) が使用されます。
<!-- * The `verified` middleware will be used if you decide to enable [email verification](https://laravel.com/docs/verification). -->

```php filename=routes/web.php
<?php

use App\Http\Controllers\ChirpController;// [tl! add]
use Illuminate\Support\Facades\Route;
// [tl! collapse:start]
/*
|--------------------------------------------------------------------------
| Web Routes
|--------------------------------------------------------------------------
|
| Here is where you can register web routes for your application. These
| routes are loaded by the RouteServiceProvider within a group which
| contains the "web" middleware group. Now create something great!
|
*/

Route::get('/', function () {
    return view('welcome');
});
// [tl! collapse:end]
Route::get('/dashboard', function () {
    return view('dashboard');
})->middleware(['auth', 'verified'])->name('dashboard');

Route::resource('chirps', ChirpController::class)// [tl! add:start]
    ->only(['index', 'store'])
    ->middleware(['auth', 'verified']);// [tl! add:end]

require __DIR__.'/auth.php';
```

<!-- This will create the following routes: -->
以下の様なルートが作成されます

動詞      | URI                    | アクション       | ルート名
----------|------------------------|--------------|---------------------
GET       | `/chirps`              | index        | chirps.index
POST      | `/chirps`              | store        | chirps.store

> **Note**
> `php artisan route:list` コマンドを実行すると、アプリケーションのすべてのルートを表示できます

<!-- Let's test our route and controller by returning a test message from the `index` method of our new `ChirpController` class: -->

新しい `ChirpController` クラスの `index` メソッドからテスト メッセージを返して、ルートとコントローラーをテストしましょう。

```php filename=app/Http/Controllers/ChirpController.php
<?php
// [tl! collapse:start]
namespace App\Http\Controllers;

use App\Models\Chirp;
use Illuminate\Http\Request;
// [tl! collapse:end]
class ChirpController extends Controller
{
    /**
     * Display a listing of the resource.
     *
     * @return \Illuminate\Http\Response
     */
    public function index()
    {
        //
        return 'Hello, World!';// [tl! remove:-1,1 add]
    }
    // [tl! collapse:start]
    /**
     * Show the form for creating a new resource.
     *
     * @return \Illuminate\Http\Response
     */
    public function create()
    {
        //
    }

    /**
     * Store a newly created resource in storage.
     *
     * @param  \Illuminate\Http\Request  $request
     * @return \Illuminate\Http\Response
     */
    public function store(Request $request)
    {
        //
    }

    /**
     * Display the specified resource.
     *
     * @param  \App\Models\Chirp  $chirp
     * @return \Illuminate\Http\Response
     */
    public function show(Chirp $chirp)
    {
        //
    }

    /**
     * Show the form for editing the specified resource.
     *
     * @param  \App\Models\Chirp  $chirp
     * @return \Illuminate\Http\Response
     */
    public function edit(Chirp $chirp)
    {
        //
    }

    /**
     * Update the specified resource in storage.
     *
     * @param  \Illuminate\Http\Request  $request
     * @param  \App\Models\Chirp  $chirp
     * @return \Illuminate\Http\Response
     */
    public function update(Request $request, Chirp $chirp)
    {
        //
    }

    /**
     * Remove the specified resource from storage.
     *
     * @param  \App\Models\Chirp  $chirp
     * @return \Illuminate\Http\Response
     */
    public function destroy(Chirp $chirp)
    {
        //
    }
    // [tl! collapse:end]
}
```

<!-- If you are still logged in from earlier, you should see your message when navigating to [http://localhost:8000/chirps](http://localhost:8000/chirps), or [http://localhost/chirps](http://localhost/chirps) if you're using Sail! -->

以前からまだログインしている場合は、[http://localhost:8000/chirps](http://localhost:8000/chirps) 、またはSail を使用している場合は [http://localhost/chirps](http://localhost/chirps) に移動すると、メッセージが表示されます。

## Blade

<!-- Not impressed yet? Let's update the `index` method of our `ChirpController` class to render a Blade view: -->

まだ感動していませんか？ `ChirpController` クラスの `index` メソッドを更新して、Blade ビューをレンダリングしましょう。

```php filename=app/Http/Controllers/ChirpController.php
<?php
// [tl! collapse:start]
namespace App\Http\Controllers;

use App\Models\Chirp;
use Illuminate\Http\Request;
// [tl! collapse:end]
class ChirpController extends Controller
{
    /**
     * Display a listing of the resource.
     *
     * @return \Illuminate\Http\Response
     */
    public function index()
    {
        return 'Hello, World!';// [tl! remove]
        return view('chirps.index');// [tl! add]
    }
    // [tl! collapse:start]
    /**
     * Show the form for creating a new resource.
     *
     * @return \Illuminate\Http\Response
     */
    public function create()
    {
        //
    }

    /**
     * Store a newly created resource in storage.
     *
     * @param  \Illuminate\Http\Request  $request
     * @return \Illuminate\Http\Response
     */
    public function store(Request $request)
    {
        //
    }

    /**
     * Display the specified resource.
     *
     * @param  \App\Models\Chirp  $chirp
     * @return \Illuminate\Http\Response
     */
    public function show(Chirp $chirp)
    {
        //
    }

    /**
     * Show the form for editing the specified resource.
     *
     * @param  \App\Models\Chirp  $chirp
     * @return \Illuminate\Http\Response
     */
    public function edit(Chirp $chirp)
    {
        //
    }

    /**
     * Update the specified resource in storage.
     *
     * @param  \Illuminate\Http\Request  $request
     * @param  \App\Models\Chirp  $chirp
     * @return \Illuminate\Http\Response
     */
    public function update(Request $request, Chirp $chirp)
    {
        //
    }

    /**
     * Remove the specified resource from storage.
     *
     * @param  \App\Models\Chirp  $chirp
     * @return \Illuminate\Http\Response
     */
    public function destroy(Chirp $chirp)
    {
        //
    }
    // [tl! collapse:end]
}
```

<!-- We can then create our Blade view template with a form for creating new Chirps: -->

その後、新しい Chirps を作成するためのフォームを使用して Blade ビュー テンプレートを作成できます。

```blade filename=resources/views/chirps/index.blade.php
<x-app-layout>
    <div class="max-w-2xl mx-auto p-4 sm:p-6 lg:p-8">
        <form method="POST" action="{{ route('chirps.store') }}">
            @csrf
            <textarea
                name="message"
                placeholder="{{ __('What\'s on your mind?') }}"
                class="block w-full border-gray-300 focus:border-indigo-300 focus:ring focus:ring-indigo-200 focus:ring-opacity-50 rounded-md shadow-sm"
            >{{ old('message') }}</textarea>
            <x-input-error :messages="$errors->get('message')" class="mt-2" />
            <x-primary-button class="mt-4">{{ __('Chirp') }}</x-primary-button>
        </form>
    </div>
</x-app-layout>
```

<!-- That's it! Refresh the page in your browser to see your new form rendered in the default layout provided by Breeze! -->

これだけです！ブラウザでページを更新して、Breeze が提供するデフォルト レイアウトでレンダリングされた新しいフォームを確認します。

<img src="/img/screenshots/chirp-form.png" alt="Chirp form" class="rounded-lg border dark:border-none shadow-lg" />

<!-- If your screenshot doesn't look quite like the above, you may need to stop and start the Vite development server for Tailwind to detect the CSS classes in the new file we just created. -->

スクリーンショットが上記のように見えない場合は、作成したばかりの新しいファイルで CSS クラスを検出するために、Tailwind の Vite 開発サーバーを停止してから起動する必要がある場合があります。

<!-- From this point forward, any changes we make to our Blade templates will be automatically refreshed in the browser whenever the Vite development server is running via `npm run dev`. -->

これ以降、Blade テンプレートに加えた変更は、Vite 開発サーバーが を `npm run dev` 介して実行されているときはいつでも、ブラウザーで自動的に更新されます。

## ナビゲーション メニュー

<!-- Let's take a moment to add a link to the navigation menu provided by Breeze. -->

Breeze が提供するナビゲーション メニューへのリンクを追加してみましょう。

<!-- Update the `navigation.blade.php` component provided by Breeze to add a menu item for desktop screens: -->

Breeze が提供する `navigation.blade.php` コンポーネントを更新して、デスクトップ画面のメニュー項目を追加します。

```blade filename=resources/views/layouts/navigation.blade.php
<div class="hidden space-x-8 sm:-my-px sm:ml-10 sm:flex">
    <x-nav-link :href="route('dashboard')" :active="request()->routeIs('dashboard')">
        {{ __('Dashboard') }}
    </x-nav-link>
    <x-nav-link :href="route('chirps.index')" :active="request()->routeIs('chirps.index')"><!-- [tl! add:start] -->
        {{ __('Chirps') }}
    </x-nav-link><!-- [tl! add:end] -->
</div>
```

<!-- And also for mobile screens: -->
また、モバイル画面の場合:

```blade filename=resources/views/layouts/navigation.blade.php
<div class="pt-2 pb-3 space-y-1">
    <x-responsive-nav-link :href="route('dashboard')" :active="request()->routeIs('dashboard')">
        {{ __('Dashboard') }}
    </x-responsive-nav-link>
    <x-responsive-nav-link :href="route('chirps.index')" :active="request()->routeIs('chirps.index')"><!-- [tl! add:start] -->
        {{ __('Chirps') }}
    </x-responsive-nav-link><!-- [tl! add:end] -->
</div>
```

## Chirp を保存する

<!-- Our form has been configured to post messages to the `chirps.store` route that we created earlier. Let's update the `store` method on our `ChirpController` class to validate the data and create a new Chirp: -->

フォームは、前に作成した `chirps.store` ルートにメッセージを投稿するように構成されています。`ChirpController` クラスの `store` メソッドを更新して、データを検証し、新しい Chirp を作成しましょう。

```php filename=app/Http/Controllers/ChirpController.php
<?php
// [tl! collapse:start]
namespace App\Http\Controllers;

use App\Models\Chirp;
use Illuminate\Http\Request;
// [tl! collapse:end]
class ChirpController extends Controller
{
    // [tl! collapse:start]
    /**
     * Display a listing of the resource.
     *
     * @return \Illuminate\Http\Response
     */
    public function index()
    {
        return view('chirps.index');
    }

    /**
     * Show the form for creating a new resource.
     *
     * @return \Illuminate\Http\Response
     */
    public function create()
    {
        //
    }
    // [tl! collapse:end]
    /**
     * Store a newly created resource in storage.
     *
     * @param  \Illuminate\Http\Request  $request
     * @return \Illuminate\Http\Response
     */
    public function store(Request $request)
    {
        //
        $validated = $request->validate([// [tl! remove:-1,1 add:start]
            'message' => 'required|string|max:255',
        ]);

        $request->user()->chirps()->create($validated);

        return redirect(route('chirps.index'));// [tl! add:end]
    }
    // [tl! collapse:start]
    /**
     * Display the specified resource.
     *
     * @param  \App\Models\Chirp  $chirp
     * @return \Illuminate\Http\Response
     */
    public function show(Chirp $chirp)
    {
        //
    }

    /**
     * Show the form for editing the specified resource.
     *
     * @param  \App\Models\Chirp  $chirp
     * @return \Illuminate\Http\Response
     */
    public function edit(Chirp $chirp)
    {
        //
    }

    /**
     * Update the specified resource in storage.
     *
     * @param  \Illuminate\Http\Request  $request
     * @param  \App\Models\Chirp  $chirp
     * @return \Illuminate\Http\Response
     */
    public function update(Request $request, Chirp $chirp)
    {
        //
    }

    /**
     * Remove the specified resource from storage.
     *
     * @param  \App\Models\Chirp  $chirp
     * @return \Illuminate\Http\Response
     */
    public function destroy(Chirp $chirp)
    {
        //
    }
    // [tl! collapse:end]
}
```

<!-- We're using Laravel's powerful validation feature to ensure that the user provides a message and that it won't exceed the 255 character limit of the database column we'll be creating. -->

Laravel の強力な検証機能を使用して、ユーザーがメッセージを提供し、作成するデータベース列の 255 文字の制限を超えないようにします。

<!-- We're then creating a record that will belong to the logged in user by leveraging a `chirps` relationship. We will define that relationship soon. -->

次に、`chirps` リレーションを利用して、ログインしているユーザーに属するレコードを作成します。その関係をすぐに定義します。

<!-- Finally, we can return a redirect response to send users back to the `chirps.index` route. -->

最後に、リダイレクトレスポンスを返して、ユーザーを `chirps.index` ルートに戻すことができます。

## リレーションの作成

<!-- You may have noticed in the previous step that we called a `chirps` method on the `$request->user()` object. We need to create this method on our `User` model to define a ["has many"](https://laravel.com/docs/eloquent-relationships#one-to-many) relationship: -->

前のステップで、`$request->user()` オブジェクトの `chirps` メソッドを呼び出したことに気付いたかもしれません。このメソッドを作成するには `User` モデルに ["has many"](https://laravel.com/docs/eloquent-relationships#one-to-many) 関係を定義する必要があります。

```php filename=app/Models/User.php
<?php
// [tl! collapse:start]
namespace App\Models;

// use Illuminate\Contracts\Auth\MustVerifyEmail;
use Illuminate\Database\Eloquent\Factories\HasFactory;
use Illuminate\Foundation\Auth\User as Authenticatable;
use Illuminate\Notifications\Notifiable;
use Laravel\Sanctum\HasApiTokens;
// [tl! collapse:end]
class User extends Authenticatable
{
    // [tl! collapse:start]
    use HasApiTokens, HasFactory, Notifiable;

    /**
     * The attributes that are mass assignable.
     *
     * @var array<int, string>
     */
    protected $fillable = [
        'name',
        'email',
        'password',
    ];

    /**
     * The attributes that should be hidden for serialization.
     *
     * @var array<int, string>
     */
    protected $hidden = [
        'password',
        'remember_token',
    ];

    /**
     * The attributes that should be cast.
     *
     * @var array<string, string>
     */
    protected $casts = [
        'email_verified_at' => 'datetime',
    ];
    // [tl! collapse:end]
    public function chirps()// [tl! add:start]
    {
        return $this->hasMany(Chirp::class);
    }// [tl! add:end]
}
```

> **Note**
> Laravel では、[Eloquent Relationships](https://laravel.com/docs/eloquent-relationships) ドキュメント で詳細を読むことができる、さまざまなタイプのモデル関係を提供します。

## 保護の一括割当

<!-- Passing all of the data from a request to your model can be risky. Imagine you have a page where users can edit their profiles. If you were to pass the entire request to the model, then a user could edit *any* column they like, such as an `is_admin` column. This is called a [mass assignment vulnerability](https://en.wikipedia.org/wiki/Mass_assignment_vulnerability). -->

リクエストからモデルにすべてのデータを渡すのは危険です。ユーザーが自分のプロフィールを編集できるページがあるとします。リクエスト全体をモデルに渡す場合、ユーザーは `is_admin` 列など、好きな列を編集できます。これは [一括割り当て脆弱性 (mass assignment vulnerability)](https://en.wikipedia.org/wiki/Mass_assignment_vulnerability) と呼ばれます。

<!-- Laravel protects you from accidentally doing this by blocking mass assignment by default. Mass assignment is very convenient though, as it prevents you from having to assign each attribute one-by-one. We can enable mass assignment for safe attributes by marking them as "fillable". -->

Laravel は、デフォルトで一括代入をブロックすることで、誤ってこれを行うことを防ぐ機能があります。一括割り当ては、各属性を 1 つずつ割り当てる必要がないため、非常に便利です。"fillable" としてマークすることで、安全な属性の一括割り当てを有効にすることができます。

<!-- Let's add the `$fillable` property to our `Chirp` model to enable mass-assignment for the `message` attribute: -->

`$fillable` プロパティを `Chirp` モデルに追加して、`message` 属性の一括割り当てを有効にしましょう。

```php filename=app/Models/Chirp.php
<?php
// [tl! collapse:start]
namespace App\Models;

use Illuminate\Database\Eloquent\Factories\HasFactory;
use Illuminate\Database\Eloquent\Model;
// [tl! collapse:end]
class Chirp extends Model
{
    // [tl! collapse:start]
    use HasFactory;
    // [tl! collapse:end]
    protected $fillable = [// [tl! add:start]
        'message',
    ];// [tl! add:end]
}
```

<!-- You can learn more about Laravel's mass assignment protection in the [documentation](https://laravel.com/docs/eloquent#mass-assignment). -->

Laravel のプロテクションの一括代入について詳しくは、[ドキュメント](https://laravel.com/docs/eloquent#mass-assignment) を参照してください。

## マイグレーションの更新

<!-- The only thing missing is extra columns in our database to store the relationship between a `Chirp` and its `User` and the message itself. Remember the database migration we created earlier? It's time to open that file to add some extra columns: -->

唯一欠けているのは、データベース内の追加の列で `Chirp` と `User`とメッセージの関係を格納するためのものです。前に作成したデータベースのマイグレーションファイルを覚えていますか? そのファイルを開いて列を追加します。

```php filename=databases/migration/&amp;lt;timestamp&amp;gt;_create_chirps_table.php
<?php
// [tl! collapse:start]
use Illuminate\Database\Migrations\Migration;
use Illuminate\Database\Schema\Blueprint;
use Illuminate\Support\Facades\Schema;
// [tl! collapse:end]
return new class extends Migration
{
    /**
     * Run the migrations.
     *
     * @return void
     */
    public function up()
    {
        Schema::create('chirps', function (Blueprint $table) {
            $table->id();
            $table->foreignId('user_id')->constrained()->cascadeOnDelete();// [tl! add]
            $table->string('message');// [tl! add]
            $table->timestamps();
        });
    }
    // [tl! collapse:start]
    /**
     * Reverse the migrations.
     *
     * @return void
     */
    public function down()
    {
        Schema::dropIfExists('chirps');
    }
    // [tl! collapse:end]
};
```

<!-- We haven't migrated the database since we added this migration, so let do it now: -->

このマイグレーションを追加してからデータベースをマイグレートしていないので、今すぐ実行しましょう。

```shell
php artisan migrate
```

> **Warning**
各データベースのマイグレーションは 1 回だけ実行されます。テーブルにさらに変更を加えるには、別のマイグレーション ファイルを作成する必要があります。開発中に、デプロイされていないマイグレーションを更新し、データベースを最初から再構築したい場合は `php artisan migrate:fresh` コマンドを使用してデータベースを最初から再構築することができます。

<!-- > Each database migration will only be run once. To make additional changes to a table, you will need to create another migration. During development, you may wish to update an undeployed migration and rebuild your database from scratch using the `php artisan migrate:fresh` command. -->


## テストしてみる

<!-- We're now ready to send a Chirp using the form we just created! We won't be able to see the result yet because we haven't displayed existing Chirps on the page. -->

<!-- テストしてみる -->

これで、作成したばかりのフォームを使用して Chirp を送信する準備が整いました! ページに既存の Chirps を表示していないため、まだ結果を確認できません。

<img src="/img/screenshots/chirp-form-filled.png" alt="Chirp form" class="rounded-lg border dark:border-none shadow-lg" />

<!-- If you leave the message field empty, or enter more than 255 characters, then you'll see the validation in action. -->

メッセージ フィールドを空のままにするか、255 文字を超える文字を入力すると、バリデーションが実行されます。

### Artisan Tinker コマンド

<!-- This is great time to learn about [Artisan Tinker](https://laravel.com/docs/artisan#tinker), a *REPL* ([Read-eval-print loop](https://en.wikipedia.org/wiki/Read%E2%80%93eval%E2%80%93print_loop)) where you can execute arbitrary PHP code in your Laravel application. -->

これは、Laravel アプリケーションで任意の PHP コードを実行できる REPL ([Read-eval-print loop](https://en.wikipedia.org/wiki/Read%E2%80%93eval%E2%80%93print_loop)) である [Artisan Tinker](https://laravel.com/docs/artisan#tinker) について学ぶ絶好の機会です。

コンソールで、新しい tinker セッションを開始します。

<!-- In your console, start a new tinker session: -->

```shell
php artisan tinker
```

<!-- Next, execute the following code to display the Chirps in your database: -->
次に、次のコードを実行して、データベース内の Chirps を表示します。

```shell
Chirp::all();
```

```
=> Illuminate\Database\Eloquent\Collection {#4512
     all: [
       App\Models\Chirp {#4514
         id: 1,
         user_id: 1,
         message: "I'm building Chirper with Laravel!",
         created_at: "2022-08-24 13:37:00",
         updated_at: "2022-08-24 13:37:00",
       },
     ],
   }
```

<!-- You may exit Tinker by using the `exit` command, or by pressing <kbd>Ctrl</kbd> + <kbd>c</kbd>. -->
exit コマンドを使用するか、`Ctrl + c` を押して Tinker を終了できます。

<!-- [Continue to start showing Chirps...](/blade/showing-chirps) -->
