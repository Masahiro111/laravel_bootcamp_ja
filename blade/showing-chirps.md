[TOC]

# <b>04.</b> Showing Chirps

In the previous step we added the ability to create Chirps, now we're ready to display them!

チャープを見せる

前のステップで、チャープを作成する機能を追加しました。これで、チャープを表示する準備が整いました。

## Retrieving the Chirps

Let's update the `index` method our `ChirpController` class to pass Chirps from every user to our index page:

鳴き声の回収

indexクラスのメソッドを更新してChirpController、すべてのユーザーからインデックス ページに Chirp を渡します。

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
        return view('chirps.index');// [tl! remove]
        return view('chirps.index', [// [tl! add:start]
            'chirps' => Chirp::with('user')->latest()->get(),
        ]);// [tl! add:end]
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
        $validated = $request->validateWithBag('store', [
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

Here we've used Eloquent's `with` method to [eager-load](https://laravel.com/docs/eloquent-relationships#eager-loading) every Chirp's associated user. We've also used the `latest` scope to return the records in reverse-chronological order.

ここでは、Eloquent のwith方法を使用して、すべての Chirp に関連付けられたユーザーを熱心にロードしました。また、スコープを使用しlatestてレコードを新しい順に返しました。

> **Note**
> Returning all Chirps at once won't scale in production. Take a look at Laravel's powerful [pagination](https://laravel.com/docs/pagination) to improve performance.

一度にすべての Chirp を返すと、本番環境ではスケーリングされません。パフォーマンスを向上させるための Laravel の強力なページネーションをご覧ください。

## Connecting users to Chirps

We've instructed Laravel to return the `user` relationship so that we can display the name of the Chirp's author. But, the Chirp's `user` relationship hasn't been defined yet. To fix this, let's add a new ["belongs to"](https://laravel.com/docs/eloquent-relationships#one-to-many-inverse) relationship to our `Chirp` model:

ユーザーを Chirp に接続する

userChirpの作成者の名前を表示できるように、Laravelに関係を返すように指示しました。しかし、チャープのuser関係はまだ定義されていません。これを修正するには、モデルに新しい「belongs to」関係を追加しましょう。Chirp

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

    protected $fillable = [
        'message',
    ];
    // [tl! collapse:end]
    public function user()// [tl! add:start]
    {
        return $this->belongsTo(User::class);
    }// [tl! add:end]
}
```

This relationship is the inverse of the "has many" relationship we created earlier on the `User` model.

Userこの関係は、先にモデルで作成した「has many」関係の逆です。

## Updating our view

Next, let's update our `chirps.index` component to display the Chirps below our form:

ビューの更新

chirps.index次に、フォームの下にチャープを表示するようにコンポーネントを更新しましょう。

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
            <x-input-error :messages="$errors->store->get('message')" class="mt-2" />
            <x-primary-button class="mt-4">{{ __('Chirp') }}</x-primary-button>
        </form>

        <div class="mt-6 bg-white shadow-sm rounded-lg divide-y"><!-- [tl! add:start] -->
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
                            </div>
                        </div>
                        <p class="mt-4 text-lg text-gray-900">{{ $chirp->message }}</p>
                    </div>
                </div>
            @endforeach
        </div><!-- [tl! add:end] -->
    </div>
</x-app-layout>
```

Now take a look in your browser to see the message you Chirped earlier!

ブラウザを見て、先ほど Chirped したメッセージを確認してください。

<img src="/img/screenshots/chirp-index-blade.png" alt="Chirp listing" class="rounded-lg border dark:border-none shadow-lg" />

Feel free to Chirp some more, or even register another account and start a conversation!

[Continue to allow editing of Chirps...](/blade/editing-chirps)

もっと気軽にチャープしたり、別のアカウントを登録して会話を始めたりすることもできます!

チャープの編集を許可し続ける...