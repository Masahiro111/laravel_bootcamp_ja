[TOC]

# <b>05.</b> Editing Chirps

Let's add a feature that's missing from other popular bird-themed microblogging platforms &mdash; the ability to edit Chirps!

チャープの編集

他の人気のある鳥をテーマにしたマイクロブログ プラットフォームにはない機能を追加しましょう — チャープを編集する機能です!

## Routing

First we will update our routes file to enable the `chirps.edit` and `chirps.update` routes for our resource controller. The `chirps.edit` route will display the form for editing a Chirp, while the `chirps.update` route will accept the data from the form and update the model:

ルーティング

まず、ルート ファイルを更新して、リソース コントローラーのルートを有効chirps.editにします。ルートは Chirp を編集するためのフォームを表示しますが、chirps.updateルートはフォームからデータを受け取り、モデルを更新します。chirps.editchirps.update

```php filename=routes/web.php
<?php
// [tl! collapse:start]
use App\Http\Controllers\ChirpController;
use Illuminate\Support\Facades\Route;

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

Route::get('/dashboard', function () {
    return view('dashboard');
})->middleware(['auth', 'verified'])->name('dashboard');
// [tl! collapse:end]
Route::resource('chirps', ChirpController::class)
    ->only(['index', 'store'])// [tl! remove]
    ->only(['index', 'store', 'edit', 'update'])// [tl! add]
    ->middleware(['auth', 'verified']);
// [tl! collapse:start]
require __DIR__.'/auth.php';
// [tl! collapse:end]
```

Our route table for this controller now looks like this:

このコントローラーのルート テーブルは次のようになります。

Verb      | URI                    | Action       | Route Name
----------|------------------------|--------------|---------------------
GET       | `/chirps`              | index        | chirps.index
POST      | `/chirps`              | store        | chirps.store
GET       | `/chirps/{chirp}/edit` | edit         | chirps.edit
PUT/PATCH | `/chirps/{chirp}`      | update       | chirps.update

動詞 	URI 	アクション 	路線名
得る 	/chirps 	索引 	チャープインデックス
役職 	/chirps 	お店 	チャープスストア
得る 	/chirps/{chirp}/edit 	編集 	チャープ編集
プット/パッチ 	/chirps/{chirp} 	アップデート 	チャープ更新

## Linking to the edit page

Next, let's link our new `chirps.edit` route. We'll use the `x-dropdown` component that comes with Breeze, which we'll only display to the Chirp author. We'll also display an indication if a Chirp has been edited by comparing the Chirp's `created_at` date with its `updated_at` date:

編集ページへのリンク

chirps.edit次に、新しいルートをリンクしましょう。x-dropdownBreeze に付属のコンポーネントを使用します。これは、Chirp 作成者にのみ表示されます。created_atまた、Chirp の日付とその日付を比較することで、Chirp が編集されているかどうかも表示しupdated_atます。

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

        <div class="mt-6 bg-white shadow-sm rounded-lg divide-y">
            @foreach ($chirps as $chirp)
                <div class="p-6 flex space-x-2">
                    <svg xmlns="http://www.w3.org/2000/svg" class="h-6 w-6 text-gray-600 -scale-x-100" fill="none" viewBox="0 0 24 24" stroke="currentColor" stroke-width="2">
                        <path stroke-linecap="round" stroke-linejoin="round" d="M8 12h.01M12 12h.01M16 12h.01M21 12c0 4.418-4.03 8-9 8a9.863 9.863 0 01-4.255-.949L3 20l1.395-3.72C3.512 15.042 3 13.574 3 12c0-4.418 4.03-8 9-8s9 3.582 9 8z" />
                    </svg>
                    <div class="flex-1">
                        <div class="flex justify-between items-center">
                            <div>
                                <span class="text-gray-800">{{ $chirp->user->name }}</span>
                                <small class="ml-2 text-sm text-gray-600">{{ $chirp->created_at->format('j M Y, g:i a') }}</small>
                                @unless ($chirp->created_at->eq($chirp->updated_at))<!-- [tl! add:start] -->
                                    <small class="text-sm text-gray-600"> &middot; {{ __('edited') }}</small>
                                @endunless<!-- [tl! add:end] -->
                            </div>
                            @if ($chirp->user->is(auth()->user()))<!-- [tl! add:start] -->
                                <x-dropdown>
                                    <x-slot name="trigger">
                                        <button>
                                            <svg xmlns="http://www.w3.org/2000/svg" class="h-4 w-4 text-gray-400" viewBox="0 0 20 20" fill="currentColor">
                                                <path d="M6 10a2 2 0 11-4 0 2 2 0 014 0zM12 10a2 2 0 11-4 0 2 2 0 014 0zM16 12a2 2 0 100-4 2 2 0 000 4z" />
                                            </svg>
                                        </button>
                                    </x-slot>
                                    <x-slot name="content">
                                        <x-dropdown-link :href="route('chirps.edit', $chirp)">
                                            {{ __('Edit') }}
                                        </x-dropdown-link>
                                    </x-slot>
                                </x-dropdown>
                            @endif<!-- [tl! add:end] -->
                        </div>
                        <p class="mt-4 text-lg text-gray-900">{{ $chirp->message }}</p>
                    </div>
                </div>
            @endforeach
        </div>
    </div>
</x-app-layout>
```

## Creating the edit form

Let's create a new Blade view with a form for editing a Chirp. This is similar to the form for creating Chirps, except we'll post to the `chirps.update` route and use the `@method` directive to specify that we're making a "PATCH" request. We'll also pre-populate the field with the existing Chirp message:

編集フォームの作成

Chirp を編集するためのフォームを持つ新しい Blade ビューを作成しましょう。これは Chirp を作成するためのフォームに似ていますが、chirps.updateルートに投稿し、@methodディレクティブを使用して「PATCH」リクエストを作成していることを指定する点が異なります。また、フィールドに既存の Chirp メッセージを事前入力します。

```blade filename=resources/views/chirps/edit.blade.php
<x-app-layout>
    <div class="max-w-2xl mx-auto p-4 sm:p-6 lg:p-8">
        <form method="POST" action="{{ route('chirps.update', $chirp) }}">
            @csrf
            @method('patch')
            <textarea
                name="message"
                class="block w-full border-gray-300 focus:border-indigo-300 focus:ring focus:ring-indigo-200 focus:ring-opacity-50 rounded-md shadow-sm"
            >{{ old('message', $chirp->message) }}</textarea>
            <x-input-error :messages="$errors->get('message')" class="mt-2" />
            <div class="mt-4 space-x-2">
                <x-primary-button>{{ __('Save') }}</x-primary-button>
                <a href="{{ route('chirps.index') }}">{{ __('Cancel') }}</a>
            </div>
        </form>
    </div>
</x-app-layout>
```

## Updating our controller

Let's update the `edit` method on our `ChirpController` to display our form. Laravel will automatically load the Chirp model from the database using [route model binding](https://laravel.com/docs/9.x/routing#route-model-binding) so we can pass it straight to the view.

We'll then update the `update` method to validate the request and update the database.

Even though we're only displaying the edit button to the author of the Chirp, we still need to make sure the user accessing these routes is authorized:

コントローラーの更新

フォームを表示するためにeditメソッドを更新しましょう。Laravel は、ルート モデル バインディングChirpControllerを使用してデータベースから Chirp モデルを自動的にロードするため、ビューに直接渡すことができます。

次に、updateメソッドを更新してリクエストを検証し、データベースを更新します。

Chirp の作成者にのみ編集ボタンを表示していますが、これらのルートにアクセスするユーザーが承認されていることを確認する必要があります。

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
        return view('chirps.index', [
            'chirps' => Chirp::with('user')->latest()->get(),
        ]);
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

    /**
     * Store a newly created resource in storage.
     *
     * @param  \Illuminate\Http\Request  $request
     * @return \Illuminate\Http\Response
     */
    public function store(Request $request)
    {
        $validated = $request->validate([
            'message' => 'required|string|max:255',
        ]);

        $request->user()->chirps()->create($validated);

        return redirect(route('chirps.index'));
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
    // [tl! collapse:end]
    /**
     * Show the form for editing the specified resource.
     *
     * @param  \App\Models\Chirp  $chirp
     * @return \Illuminate\Http\Response
     */
    public function edit(Chirp $chirp)
    {
        //
        $this->authorize('update', $chirp);// [tl! remove:-1,1 add:start]

        return view('chirps.edit', [
            'chirp' => $chirp,
        ]);// [tl! add:end]
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
        $this->authorize('update', $chirp);// [tl! remove:-1,1 add:start]

        $validated = $request->validate([
            'message' => 'required|string|max:255',
        ]);

        $chirp->update($validated);

        return redirect(route('chirps.index'));// [tl! add:end]
    }
    // [tl! collapse:start]
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

> **Note**
> You may have noticed the validation rules are duplicated with the `store` method. You might consider extracting them using Laravel's [Form Request Validation](https://laravel.com/docs/validation#form-request-validation), which makes it easy to re-use validation rules and to keep your controllers light.

store検証ルールがメソッドと重複していることに気付いたかもしれません。Laravel のForm Request Validationを使用してそれらを抽出することを検討してください。これにより、検証ルールを簡単に再利用し、コントローラーを軽量に保つことができます。

## Authorization

By default, the `authorize` method will prevent *everyone* from being able to update the Chirp. We can specify who is allowed to update it by creating a [Model Policy](https://laravel.com/docs/authorization#creating-policies) with the following command:

認可

デフォルトでは、このauthorizeメソッドは全員が Chirp を更新できないようにします。次のコマンドでモデル ポリシーを作成することにより、更新を許可するユーザーを指定できます。

```shell
php artisan make:policy ChirpPolicy --model=Chirp
```

This will create a policy class at `app/Policies/ChirpPolicy.php` which we can update to specify that only the author is authorized to update a Chirp:

app/Policies/ChirpPolicy.phpこれにより、作成者のみが Chirp の更新を許可されていることを指定するために更新できるポリシー クラスが作成されます。

```php filename=app/Policies/ChirpPolicy.php
<?php
// [tl! collapse:start]
namespace App\Policies;

use App\Models\Chirp;
use App\Models\User;
use Illuminate\Auth\Access\HandlesAuthorization;
// [tl! collapse:end]
class ChirpPolicy
{
    // [tl! collapse:start]
    use HandlesAuthorization;

    /**
     * Determine whether the user can view any models.
     *
     * @param  \App\Models\User  $user
     * @return \Illuminate\Auth\Access\Response|bool
     */
    public function viewAny(User $user)
    {
        //
    }

    /**
     * Determine whether the user can view the model.
     *
     * @param  \App\Models\User  $user
     * @param  \App\Models\Chirp  $chirp
     * @return \Illuminate\Auth\Access\Response|bool
     */
    public function view(User $user, Chirp $chirp)
    {
        //
    }

    /**
     * Determine whether the user can create models.
     *
     * @param  \App\Models\User  $user
     * @return \Illuminate\Auth\Access\Response|bool
     */
    public function create(User $user)
    {
        //
    }
    // [tl! collapse:end]
    /**
     * Determine whether the user can update the model.
     *
     * @param  \App\Models\User  $user
     * @param  \App\Models\Chirp  $chirp
     * @return \Illuminate\Auth\Access\Response|bool
     */
    public function update(User $user, Chirp $chirp)
    {
        //
        return $chirp->user()->is($user);// [tl! remove:-1,1 add]
    }
    // [tl! collapse:start]
    /**
     * Determine whether the user can delete the model.
     *
     * @param  \App\Models\User  $user
     * @param  \App\Models\Chirp  $chirp
     * @return \Illuminate\Auth\Access\Response|bool
     */
    public function delete(User $user, Chirp $chirp)
    {
        //
    }

    /**
     * Determine whether the user can restore the model.
     *
     * @param  \App\Models\User  $user
     * @param  \App\Models\Chirp  $chirp
     * @return \Illuminate\Auth\Access\Response|bool
     */
    public function restore(User $user, Chirp $chirp)
    {
        //
    }

    /**
     * Determine whether the user can permanently delete the model.
     *
     * @param  \App\Models\User  $user
     * @param  \App\Models\Chirp  $chirp
     * @return \Illuminate\Auth\Access\Response|bool
     */
    public function forceDelete(User $user, Chirp $chirp)
    {
        //
    }
    // [tl! collapse:end]
}
```

## Testing it out

Time to test it out! Go ahead and edit a few Chirps using the dropdown menu. If you register another user account, you'll see that only the author of a Chirp can edit it.

テストしてみる

それをテストする時間です！ドロップダウン メニューを使用していくつかのチャープを編集します。別のユーザー アカウントを登録すると、Chirp の作成者のみが編集できることがわかります。

<img src="/img/screenshots/chirp-editted-blade.png" alt="An editted chirp" class="rounded-lg border dark:border-none shadow-lg" />

[Continue to allow deleting of Chirps...](/blade/deleting-chirps)
チャープの削除を許可し続ける...