# <b>01.</b> はじめに

Laravel ブートキャンプへようこそ！このガイドでは、最新の Laravel アプリケーションをゼロから構築する方法について説明します。フレームワークを調べるために、*Chirper* と呼ばれるマイクロブログプラットフォームを構築します。

## Blade または JavaScript を選択できます！

<!-- Laravel is incredibly flexible, allowing you to build your front-end with a wide variety of technologies to suit your needs. For this tutorial, we have prepared a few choices for you. -->

Laravel は非常に柔軟で、ニーズに合わせてさまざまなテクノロジーを使用してフロントエンドを構築できます。このチュートリアルでは、いくつかの選択肢を用意しました。

### Blade

[Blade](https://laravel.com/docs/blade) は、Laravel に含まれているシンプルでありながら強力なテンプレート エンジンです。HTML はサーバー側でレンダリングされるため、データベースから動的コンテンツを簡単に含めることができます。また、[Tailwind CSS](https://tailwindcss.com/) を使用して見栄えを良くします。

どこから始めればよいかわからない場合は、Blade が最も簡単だと思います。いつでも戻ってきて、JavaScript を使用して Chirper を再度ビルドできます。

```blade filename=welcome.blade.php
Greetings {{ $friend }}, let's build Chirper with Blade!
```

### JavaScript と Inertia

JavaScript を使用したい場合は、 [Vue](https://vuejs.org/) と [React](https://reactjs.org/) の両方のコードサンプルを提供します。また、 [Inertia](https://inertiajs.com/) を使用してすべてを接続し、[Tailwind CSS](https://tailwindcss.com/) を使用して見栄えを良くします。

どちらを選択すればよいかわからない場合は、初心者にやさしく非常に強力な Vue の大ファンです。ブートキャンプが終了したら、いつでも別のスタックで再試行できます。

スタックを選択してください。

```vue tab=Vue filename=Welcome.vue
<template><!-- [tl! .hidden] -->
<Welcome>
    Hey {{ friend }}, let's build Chirper with Vue!
</Welcome>
</template><!-- [tl! .hidden] -->
```

```jsx tab=React filename=Welcome.jsx
<Welcome>
    Nice to see you {friend}, let's build Chirper with React!
</Welcome>
```

<!-- At any point you may view the code for either framework to see what life is like on the other side, just be sure not to mix the code in your project. -->

いずれかのフレームワークのコードをいつでも表示して、反対側での動作を確認できますが、プロジェクト内でコードを混在させないでください。

<!-- [Build Chirper with JavaScript and Inertia...](/inertia/installation) -->
