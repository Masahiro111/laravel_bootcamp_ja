[TOC]

# <b>08.</b> Deploying

デプロイ

ギアを切り替えて、新しい Laravel アプリケーションをデプロイする方法を見てみましょう。

Let's switch gears and look at how we can deploy our new Laravel application.

## Choosing a provider

Laravel can be deployed to any modern PHP hosting environment that meets Laravel's modest [server requirements](https://laravel.com/docs/deployment#server-requirements). However, configuring and managing a web server and database server takes our attention away from building applications and delivering value to our users. That's why we built [Laravel Forge](https://forge.laravel.com/?ref=bootcamp.laravel.com) and [Laravel Vapor](https://vapor.laravel.com/?ref=bootcamp.laravel.com).

プロバイダーの選択

Laravel は、Laravel の適度なサーバー要件を満たす最新の PHP ホスティング環境にデプロイできます。ただし、Web サーバーとデータベース サーバーの構成と管理は、アプリケーションの構築とユーザーへの価値の提供から注意をそらします。それが、 Laravel ForgeとLaravel Vaporを構築した理由です。

#### Laravel Forge

Laravel Forge can create servers on various infrastructure providers such as DigitalOcean, Linode, AWS, and more. In addition, Forge installs and manages all of the tools needed to build robust Laravel applications, such as Nginx, MySQL, Redis, Memcached, Beanstalk, and more.

ララベルフォージ

Laravel Forge は、DigitalOcean、Linode、AWS などのさまざまなインフラストラクチャ プロバイダー上にサーバーを作成できます。さらに、Forge は、Nginx、MySQL、Redis、Memcached、Beanstalk など、堅牢な Laravel アプリケーションを構築するために必要なすべてのツールをインストールして管理します。

#### Laravel Vapor

Laravel Vapor is a serverless deployment platform for Laravel, powered by AWS. Launch your Laravel infrastructure on Vapor and fall in love with the scalable simplicity of serverless.

Both are fantastic options, but today we're going with Forge for it's simplicity, choice of providers, and for being budget-friendly on smaller applications. You can always move to Vapor later if you want the scalability of serverless.

ララベル蒸気

Laravel Vapor は、AWS を利用した Laravel のサーバーレス デプロイ プラットフォームです。Vapor で Laravel インフラストラクチャを起動し、サーバーレスのスケーラブルなシンプルさに夢中になります。

どちらも素晴らしいオプションですが、今日は、シンプルさ、プロバイダーの選択、および小規模なアプリケーションで予算にやさしいという理由から、Forge を使用します。サーバーレスのスケーラビリティが必要な場合は、後でいつでも Vapor に移行できます。

Sign up for your free trial with [Laravel Forge](https://forge.laravel.com/?ref=bootcamp.laravel.com) and then pick your server provider:

Laravel Forgeの無料トライアルにサインアップしてから、サーバー プロバイダーを選択します。

* [DigitalOcean](https://try.digitalocean.com/freetrialoffer/) <small>($100 free credit available)</small>
* [Linode](https://www.linode.com/) <small>($50 free credit available)</small>
* [AWS](https://aws.amazon.com/free/) <small>(free tier available)</small>
* [Vultr](https://www.vultr.com/promo/try50/) <small>($50 free credit available)</small>
* [Hetzner](https://www.hetzner.com/)
* Custom VPS server

If you're not sure who to pick, we recommend [DigitalOcean](https://try.digitalocean.com/freetrialoffer/) for their generous credit, great user interface, and excellent features.


    DigitalOcean ($100 の無料クレジットが利用可能)
    Linode ($50 の無料クレジットが利用可能)
    AWS (無料利用枠あり)
    Vultr ($50 の無料クレジットが利用可能)
    ヘッツナー
    カスタム VPS サーバー

誰を選ぶべきかわからない場合は、寛大なクレジット、優れたユーザー インターフェイス、優れた機能を備えたDigitalOceanをお勧めします。

## Connecting to source control

Forge needs to know where to find your application's code, so you'll need an account with a source control provider, such as [GitHub](https://github.com/), [GitLab](https://gitlab.com/), or [Bitbucket](https://bitbucket.com).

You may then connect Forge to your provider by selecting it on Forge's welcome screen, or by visiting the *Source Control* section of your Forge account.

ソース管理への接続

Forge はアプリケーションのコードがどこにあるかを知る必要があるため、GitHub、GitLab、またはBitbucketなどのソース管理プロバイダーのアカウントが必要になります。

次に、Forge のようこそ画面でプロバイダーを選択するか、Forge アカウントの [ソース管理]セクションにアクセスして、Forge をプロバイダーに接続できます。

<img src="/img/screenshots/forge-source-control.png" alt="Source control screenshot" class="rounded-lg border dark:border-none shadow-lg" />

## Connecting to your server provider

Forge will need the API key for your server provider so that it can build your servers. You can connect to your server provider on the Forge welcome screen, or by visiting the *Server Providers* section of your Forge account.

サーバープロバイダーへの接続

Forge は、サーバーを構築できるように、サーバー プロバイダーの API キーを必要とします。Forge のようこそ画面で、またはForge アカウントの[サーバー プロバイダー]セクションにアクセスして、サーバー プロバイダーに接続できます。

<img src="/img/screenshots/forge-server-providers.png" alt="Server providers screenshot" class="rounded-lg border dark:border-none shadow-lg" />

Follow the instructions to create API credentials for Forge with your selected provider, and then enter the details to continue.

指示に従って、選択したプロバイダーで Forge の API 資格情報を作成し、詳細を入力して続行します。

## Creating a server

Now that Forge is connected to your source control and server providers, we're ready to create our first server!

From the *Servers* page, click the *Create Server* button.

Next, select your server provider. You'll be presented with several options depending on your provider. The default options will be perfect for deploying Chirper, but we recommend reviewing all of the available options for your chosen provider to ensure everything matches your requirements and budget.

サーバーの作成

Forge がソース管理およびサーバー プロバイダーに接続されたので、最初のサーバーを作成する準備が整いました。

[サーバー]ページで、[サーバーの作成]ボタンをクリックします。

次に、サーバー プロバイダーを選択します。プロバイダーに応じて、いくつかのオプションが表示されます。デフォルトのオプションはChirperの展開に最適ですが、選択したプロバイダーで利用可能なすべてのオプションを確認して、すべてが要件と予算に合っていることを確認することをお勧めします.

<img src="/img/screenshots/forge-create-server.png" alt="Create server screenshot" class="rounded-lg border dark:border-none shadow-lg" />

> **Note**
> We recommend starting with an "App Server", which will provision everything you need all on one server. You may destroy the server when you're finished to avoid unnecessary costs.



必要なものすべてを 1 つのサーバーにプロビジョニングする「アプリケーション サーバー」から始めることをお勧めします。不必要なコストを避けるために、終了時にサーバーを破棄することができます。


Creating a server takes about 10 minutes, depending on your provider and the options selected.

サーバーの作成には、プロバイダーと選択したオプションに応じて、約 10 分かかります。

## Creating a site (optional)

Forge will automatically create a "default" site on your new server. This is perfect for deploying our application because we can visit it using the server's public IP address instead of purchasing a domain name.

If you would like to use a domain name then we recommend visiting the *Sites* tab of your server to delete the "default" site and create a new site. You may create as many sites as you need. You'll also have the option to create a database for your new site.

サイトの作成 (オプション)

Forge は、新しいサーバー上に「デフォルト」サイトを自動的に作成します。これは、ドメイン名を購入する代わりにサーバーのパブリック IP アドレスを使用してアプリケーションにアクセスできるため、アプリケーションのデプロイに最適です。

ドメイン名を使用したい場合は、サーバーの [サイト] タブにアクセスして、「既定の」サイトを削除し、新しいサイトを作成することをお勧めします。必要な数のサイトを作成できます。新しいサイト用のデータベースを作成するオプションもあります。

<img src="/img/screenshots/forge-new-site.png" alt="New site screenshot" class="rounded-lg border dark:border-none shadow-lg" />

Select your site, and then click on it's name in the heading to visit your site and see Forge's default site page.

サイトを選択し、見出しの名前をクリックしてサイトにアクセスし、Forge のデフォルト サイト ページを表示します。

## Creating a database

If you're using the "default" site, you will need to create a database for your application. You may create databases on the *Database* tab of your server.

データベースの作成

「デフォルト」サイトを使用している場合は、アプリケーション用のデータベースを作成する必要があります。サーバーの [データベース] タブでデータベースを作成できます。

<img src="/img/screenshots/forge-add-database.png" alt="Add database screenshot" class="rounded-lg border dark:border-none shadow-lg" />

You may leave the username and password fields empty to use the credentials generated by Forge when building your server.

サーバーの構築時に Forge によって生成された資格情報を使用するために、ユーザー名とパスワードのフィールドを空のままにしておくことができます。

## Installing a repository

We're ready to install a source control repository on our new site. If you haven't already, make sure to create a git repository for your application and push the source code up to the source control provider that you connected to Forge earlier.

Choose your site from the your servers *Sites* tab and then click *Git Repository*. Enter your repository name (e.g. `taylorotwell/chirper`) and select your branch. Be sure that "Install Composer Dependencies" is checked, and then install your repository to continue.

リポジトリのインストール

新しいサイトにソース管理リポジトリをインストールする準備が整いました。まだ行っていない場合は、アプリケーションの git リポジトリを作成し、以前に Forge に接続したソース管理プロバイダーにソース コードをプッシュしてください。

サーバーの Sitesタブからサイトを選択し、 Git Repositoryをクリックします。リポジトリ名 (例: taylorotwell/chirper) を入力し、ブランチを選択します。「Install Composer Dependencies」がチェックされていることを確認してから、リポジトリをインストールして続行します。

<img src="/img/screenshots/forge-install-repository.png" alt="Install repository screenshot" class="rounded-lg border dark:border-none shadow-lg" />

## Configuring your environment file

When installing your repository, Forge will copy the `.env.example` environment file from your repository. You may review your environment configuration file from the *Edit Files* menu of your site.

環境ファイルの構成

リポジトリをインストールすると、Forge はリポジトリ.env.exampleから環境ファイルをコピーします。サイトの [ファイルの編集] メニューから環境構成ファイルを確認できます。

<img src="/img/screenshots/forge-site-environment.png" alt="Site environment screenshot" class="rounded-lg border dark:border-none shadow-lg" />

You should carefully review all of the values to ensure everything is configured correctly for your production environment.

Ensure that the `DB_*` values match the database you created earlier. If you created a dedicated database username and password, be sure to configure those as well.

If you would like to send emails from your application, you will need to review the `MAIL_*` values and add any additional variables that may be required for your chosen [mail provider](https://laravel.com/docs/mail#configuration).

If you would like to run a queue worker, ensure that the `QUEUE_CONNECTION` value matches your desired queue connection and that you have installed and configured any [prerequisites](https://laravel.com/docs/queues#driver-prerequisites).

環境ファイルの構成

リポジトリをインストールすると、Forge はリポジトリ.env.exampleから環境ファイルをコピーします。サイトの [ファイルの編集] メニューから環境構成ファイルを確認できます。

## Configuring your deploy script

デプロイ スクリプトの構成

サイトの[アプリ] タブで、Forge が作成したデプロイ スクリプトを確認できます。

On the *App* tab for your site you may review the deploy script that Forge created for you.

<img src="/img/screenshots/forge-deploy-script.png" alt="Deploy script screenshot" class="rounded-lg border dark:border-none shadow-lg" />

Our application is using Node dependencies and Vite, so we'll need to add steps to install the dependencies and build our assets:

私たちのアプリケーションは Node の依存関係と Vite を使用しているため、依存関係をインストールしてアセットを構築する手順を追加する必要があります。

```env
cd /home/forge/default
git pull origin $FORGE_SITE_BRANCH

$FORGE_COMPOSER install --no-interaction --prefer-dist --optimize-autoloader
npm ci
npm run build
#[tl! add:-2,2]
( flock -w 10 9 || exit 1
    echo 'Restarting FPM...'; sudo -S service $FORGE_PHP_FPM reload ) 9>/tmp/fpmlock

if [ -f artisan ]; then
    $FORGE_PHP artisan migrate --force
fi
```

## Running a queue worker (optional)

If you've chosen to configure a [queue worker](https://laravel.com/docs/queues), you may now instruct Forge to start and manage the worker on the *Queue* tab of your site.

キュー ワーカーの実行 (オプション)

キュー ワーカーを構成することを選択した場合は、サイトの [キュー] タブでワーカーを開始および管理するように Forge に指示できます。

<img src="/img/screenshots/forge-new-worker.png" alt="New worker screenshot" class="rounded-lg border dark:border-none shadow-lg" />

## Running a task scheduler (optional)

If you've chosen to use Laravel's [task scheduling](https://laravel.com/docs/scheduling), you may configure Forge to run the scheduler on the *Scheduler* tab of your server. You will need to set the frequency to "every minute" so that the scheduler will check for due tasks on any schedule you may configure.

タスク スケジューラの実行 (オプション)

Laravel のタスク スケジューリングを使用することを選択した場合は、サーバーの [スケジューラ] タブでスケジューラを実行するように Forge を構成できます。スケジューラーが構成可能なスケジュールで期日タスクをチェックするように、頻度を「毎分」に設定する必要があります。

<img src="/img/screenshots/forge-new-scheduled-job.png" alt="New scheduled job screenshot" class="rounded-lg border dark:border-none shadow-lg" />

## Deploying

We're ready to deploy our application for the first time! Press the *Deploy Now* button, and then sit back while Forge runs your deploy script.

デプロイ中

初めてアプリケーションをデプロイする準備ができました! [今すぐデプロイ] ボタンを押して、Forge がデプロイ スクリプトを実行している間、座ってください。

> **Note**
> You may enable *Quick Deploy* to automatically deploy your application every time you push to your main branch&mdash;perfect for a [continuous deployment](https://en.wikipedia.org/wiki/Continuous_deployment) environment.

Quick Deployを有効にして、メイン ブランチにプッシュするたびにアプリケーションを自動的にデプロイできます。これは、継続的なデプロイ環境に最適です。

You may review your deployment history on the *Deployments* tab.

Once complete, visit your site again to confirm everything has deployed successfully.

[展開] タブで展開履歴を確認できます。

完了したら、もう一度サイトにアクセスして、すべてが正常にデプロイされたことを確認します。

> **Warning**
> Don't forget to destroy your server from the *Meta* tab once you're finished with it to avoid any unnecessary charges from your server provider.



サーバープロバイダーからの不要な料金を避けるために、サーバーの使用が終了したら、メタタブからサーバーを破棄することを忘れないでください.

[Continue to conclusion and next steps...](/conclusion)

結論と次のステップに進みます...