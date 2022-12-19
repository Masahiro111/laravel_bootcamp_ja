[TOC]

# <b>02.</b> インストール

## Laravelのインストール

### クイックインストール

ローカル マシンに PHP と Composer が既にインストールされている場合は、Composer を介して新しい Laravel プロジェクトを作成できます。

```shell
composer create-project laravel/laravel chirper
```

プロジェクトが作成されたら、Laravel の Artisan CLI serve コマンドを使用して、Laravel のローカル開発サーバーを起動します。

```none
cd chirper

php artisan serve
```

<!-- Once you have started the Artisan development server, your application will be accessible in your web browser at [http://localhost:8000](http://localhost:8000). -->

Artisan 開発サーバーを起動すると、アプリケーションは Web ブラウザーで [http://localhost:8000](http://localhost:8000) にアクセスできるようになります。

<img src="/img/screenshots/fresh.png" alt="A fresh Laravel installation" class="dark:hidden rounded-lg border shadow-lg" />
<img src="/img/screenshots/fresh-dark.png" alt="A fresh Laravel installation" class="hidden dark:block rounded-lg border-gray-700 shadow-lg" />

<!-- For simplicity, you may use SQLite to store your application's data. To instruct Laravel to use SQLite instead of MySQL, update your new application's `.env` file and remove all of the `DB_*` environment variables except for the `DB_CONNECTION` variable, which should be set to `sqlite`: -->

簡単にするために、SQLite を使用してアプリケーションのデータを保存できます。MySQL の代わりに SQLite を使用するように Laravel に指示するには、新しいアプリケーションの `.env` ファイルを更新し、次のように `DB_CONNECTION` 変数を除くすべての `DB_*` 環境変数を削除します。

```ini
DB_CONNECTION=sqlite
```

### Docker によるインストール

PHP がローカルにインストールされていない場合は、[Laravel Sail](https://laravel.com/docs/sail) を使用してアプリケーションを開発できます。これは、すべてのオペレーティング システムと互換性のある Laravel のデフォルトの [Docker](https://docs.docker.com/get-docker/) 開発環境と対話するための軽量コマンドライン インターフェイスです。始める前に、オペレーティング システム用のDockerをインストールしてください。別のインストール方法については、完全な [インストールガイド](https://laravel.com/docs/installation) をご覧ください。

Laravel をインストールする最も簡単な方法は `laravel.build`です。新しい Laravel アプリケーションをダウンロードして作成する当社のサービスを使用することです。ターミナルを起動し、次のコマンドを実行します。

```shell
curl -s "https://laravel.build/chirper" | bash
```

<!-- Sail installation may take several minutes while Sail's application containers are built on your local machine. -->

<!-- By default, the installer will pre-configure Laravel Sail with a number of useful services for your application, including a MySQL database server. You may [customize the Sail services](https://laravel.com/docs/installation#choosing-your-sail-services) if needed.

After the project has been created, you can navigate to the application directory and start Laravel Sail: -->

Sail のインストールには、Sail のアプリケーション コンテナがローカル マシン上に構築されるまで数分かかる場合があります。

デフォルトでは、インストーラーは、MySQL データベースサーバーを含む、アプリケーションに役立つ多数のサービスを使用して Laravel Sail を事前設定します。必要に応じて、[Sail サービスをカスタマイズ](https://laravel.com/docs/installation#choosing-your-sail-services) できます。

プロジェクトが作成されたら、アプリケーション ディレクトリに移動して Laravel Sail を起動できます。

```shell
cd chirper

./vendor/bin/sail up
```

> **メモ**
> Sail のコマンドをより簡単に実行できる [シェル エイリアス](https://laravel.com/docs/sail#configuring-a-shell-alias) を作成 できます。

 <!-- When developing applications using Sail, you may execute Artisan, NPM, and Composer commands via the Sail CLI instead of invoking them directly: -->

 Sail を使用してアプリケーションを開発する場合、Artisan、NPM、および Composer コマンドを直接呼び出す代わりに、Sail CLI を介して実行できます。

```shell
./vendor/bin/sail php --version
./vendor/bin/sail artisan --version
./vendor/bin/sail composer --version
./vendor/bin/sail npm --version
```

<!-- Once the application's Docker containers have been started, you can access the application in your web browser at: [http://localhost](http://localhost). -->

アプリケーションの Docker コンテナーが開始されると、Web ブラウザーで [http://localhost](http://localhost) からアプリケーションにアクセスできます。

<img src="/img/screenshots/fresh.png" alt="A fresh Laravel installation" class="dark:hidden rounded-lg border shadow-lg" />
<img src="/img/screenshots/fresh-dark.png" alt="A fresh Laravel installation" class="hidden dark:block rounded-lg border-gray-700 shadow-lg" />

## Laravel Breeze のインストール

<!-- Next, we will give your application a head-start by installing [Laravel Breeze](https://laravel.com/docs/starter-kits#laravel-breeze), a minimal, simple implementation of all of Laravel's authentication features, including login, registration, password reset, email verification, and password confirmation. Once installed, you are welcome to customize the components to suit your needs. -->

次に、ログイン、登録、パスワードのリセット、電子メールの検証、パスワードの確認など、Laravel のすべての認証機能の最小限でシンプルな実装である [Laravel Breeze](https://laravel.com/docs/starter-kits#laravel-breeze) をインストールすることで、アプリケーションに有利なスタートを切ります。インストールしたら、必要に応じてコンポーネントをカスタマイズできます。

<!-- Laravel Breeze offers several options for your view layer, including Blade templates, or [Vue](https://vuejs.org/) and [React](https://reactjs.org/) with [Inertia](https://inertiajs.com/). For this tutorial, we'll be using Blade. -->

Laravel Breeze は、Blade テンプレートや [Vue](https://vuejs.org/) 、 [React](https://reactjs.org/) や [Inertia](https://inertiajs.com/) など、ビュー レイヤーにいくつかのオプションを提供します。このチュートリアルでは、Blade を使用します。

<!-- Open a new terminal in your `chirper` project directory and install your chosen stack with the given commands: -->

`chirper` プロジェクト ディレクトリで新しいターミナルを開き、指定されたコマンドで選択したスタックをインストールします。

```shell
composer require laravel/breeze --dev

php artisan breeze:install blade
```

<!-- Breeze will install and configure your front-end dependencies for you, so we just need to start the Vite development server to automatically recompile our CSS and refresh the browser when we make changes to our Blade templates: -->

Breeze がフロントエンドの依存関係をインストールして構成するため、Vite 開発サーバーを起動して、Blade テンプレートに変更を加えたときに CSS を自動的に再コンパイルし、ブラウザーを更新するだけです。

```shell
npm run dev
```

<!-- Finally, open another terminal in your `chirper` project directory and run the initial database migrations to populate the database with the default tables from Laravel and Breeze: -->

最後に、`chirper` プロジェクト ディレクトリで別のターミナルを開き、最初のデータベース マイグレーションを実行して、データベースに Laravel と Breeze のデフォルト テーブルを設定します。

```shell
php artisan migrate
```

<!-- If you refresh your new Laravel application in the browser, you should now see a "Register" link at the top-right. Follow that to see the registration form provided by Laravel Breeze. -->

ブラウザで新しい Laravel アプリケーションを更新すると、右上に「Register」リンクが表示されます。それに従って、Laravel Breeze が提供する登録フォームを表示します。

<img src="/img/screenshots/register.png" alt="Laravel registration page" class="rounded-lg border dark:border-none shadow-lg" />

<!-- Register yourself an account and log in! -->

アカウントを登録してログインしてください！

<!-- [Continue to start creating Chirps...](/blade/creating-chirps) -->
