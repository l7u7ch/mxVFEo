---
title: 'AstroJS プロジェクトで Expressive Code を利用してシンタックスハイライトを表示する'
publishedAt: '2024-02-08 13:42'
updatedAt: '2024-02-08 13:42'
heroImage: './3545A811447C5F6F132BB1C0BE93E630.webp'
---

## 1. はじめに

AstroJS では、デフォルトで ShikiJS とPrismJS が組み込まれているため、追加のパッケージをインストールすることなく、Markdown 内のコードブロックでシンタックスハイライトを表示させることが出来ます。しかし、タイトルやマーカー、コピーボタンを表示されるには、追加でパッケージをインストールしたり、コードを追加しなくてはなりません。

Expressive Code は、シンタックスハイライトに加えて、タイトルやマーカー、コピーボタンの表示など、コードブロックを強化する様々な機能を提供しているパッケージです。本記事では、Expressive Code を AstroJS プロジェクトにインストールする方法と簡単な使い方について記述します。

## 2. インストール

Expressive Code を AstroJS プロジェクトにインストールする方法は簡単です。Expressive Code が公開しているインストールガイド [^1] に従って、[1] `yarn create astro` コマンドと [2] `yarn astro add astro-expressive-code` コマンドを実行するだけで自動的にインストールされます。既に AstroJS プロジェクトが存在している場合は `yarn create astro` コマンドを省略してください。

[^1]: Expressive Code, Installing Expressive Code：https://expressive-code.com/installation/

```bash {"1":1} {"2":49}
$ yarn create astro
yarn create v1.22.21
[1/4] Resolving packages...
[2/4] Fetching packages...
[3/4] Linking dependencies...
[4/4] Building fresh packages...

success Installed "create-astro@4.7.2" with binaries:
      - create-astro
[########################################] 40/40
 astro   Launch sequence initiated.

   dir   Where should we create your new project?
         ./wandering-wasp

  tmpl   How would you like to start your new project?
         Use blog template

    ts   Do you plan to write TypeScript?
         Yes

   use   How strict should TypeScript be?
         Strict

  deps   Install dependencies?
         Yes

   git   Initialize a new git repository?
         No
      ◼  Sounds good! You can always run git init manually.

      ✔  Project initialized!
         ■ Template copied
         ■ TypeScript customized
         ■ Dependencies installed

  next   Liftoff confirmed. Explore your project!

         Enter your project directory using cd ./wandering-wasp
         Run yarn dev to start the dev server. CTRL+C to stop.
         Add frameworks like react or tailwind using astro add.

         Stuck? Join us at https://astro.build/chat

╭─────╮  Houston:
│ ◠ ◡ ◠  Good luck out there, astronaut! 🚀
╰─────╯
$ cd terrestrial-trappist/
$ yarn astro add astro-expressive-code
yarn run v1.22.21
warning package.json: No license field
$ astro add astro-expressive-code
⚠ astro-expressive-code is not an official Astro package. Use at your own risk!
✔ Continue? … yes
✔ Resolving with third party packages...
02:54:07
  Astro will run the following command:
  If you skip this step, you can always run it yourself later

 ╭─────────────────────────────────────────╮
 │ yarn add astro-expressive-code@^0.32.4  │
 ╰─────────────────────────────────────────╯

✔ Continue? … yes
✔ Installing dependencies...
02:54:09
   success  Configuration up-to-date.
```

マニュアルでインストールする場合は、`astro-expressive-code` パッケージをインストールして、`astro.config.mjs` に以下のコードを追加すれば完了です。

```bash
$ yarn add astro-expressive-code
```

```js title="astro.config.mjs" {2, 5}
import { defineConfig } from 'astro/config'
import expressiveCode from 'astro-expressive-code'

export default defineConfig({
  integrations: [expressiveCode()],
})
```

## 3. サンプルコード

` ``` ` でコードを囲うことで、コードブロックが表示されます。また、言語を指定することで、シンタックスハイライトが表示されます。下記の例では JavaScript を指定していますが、その他に対応している言語に関しては、ShikiJS の README [^2] を参照ください。Expressive Code では、デフォルトでコピーボタンは付与されるので、ユーザーはマウスオーバーして表示されるボタンをクリックすることで、コードを簡単にコピーすることが出来ます。また、オプションコードを追記することで、タイトルとマーカーを表示させることが出来ます。

[^2]: GitHub, ShikiJS, tm-grammars：https://github.com/shikijs/textmate-grammars-themes/blob/main/packages/tm-grammars/README.md

````md title="*.md"
```js title="line-markers.js" del={2} ins={3-4} {6}
function demo() {
  console.log('this line is marked as deleted')
  // This line and the next one are marked as inserted
  console.log('this is the second inserted line')

  return 'this line uses the neutral default marker type'
}
```
````

```js title="line-markers.js" del={2} ins={3-4} {6}
function demo() {
  console.log('this line is marked as deleted')
  // This line and the next one are marked as inserted
  console.log('this is the second inserted line')

  return 'this line uses the neutral default marker type'
}
```

上記の例では行単位でマーカーを表示させていますが、Expressive Code では、文字単位でマーカーを表示させることも出来ます。

````md title="*.md"
```js "return true;" ins="inserted" del="deleted"
function demo() {
  console.log('These are inserted and deleted marker types')
  // The return statement uses the default marker type
  return true
}
```
````

```js "return true" ins="inserted" del="deleted"
function demo() {
  console.log('These are inserted and deleted marker types')
  // The return statement uses the default marker type
  return true
}
```

また、Expressive Code では、言語に Bash や PowerShell などのスクリプト言語を設定すると、自動的にフレームがターミナル風のものに差し替えられます。オシャレで素敵ですね。ちなみに、本ブログサイトで表示される macOS 風ターミナルフレームは、ろぼいん氏のブログ記事 [^5] を参考にカスタマイズしています。

[^5]: Markdownでリッチなコードブロックを実現する「Expressive Code」：https://roboin.io/article/2023/12/16/how-to-use-expressive-code-in-markdown-and-astro/

````md
```bash
echo "This terminal frame has no title"
```

```powershell title="PowerShell terminal example"
Write-Output "This one has a title!"
```
````

```bash
echo "This terminal frame has no title"
```

```powershell title="PowerShell terminal example"
Write-Output "This one has a title!"
```

上記で挙げた例は、Expressive Code が提供している機能の一部です。更に詳しい機能や仕様を知りたい方は、Expressive Code の公式ドキュメント [^3] を参照ください。

[^3]: Expressive Code：https://expressive-code.com/

## 4. おわりに

ここまで、Expressive Code を AstroJS プロジェクトにインストールする方法と簡単な使い方について記述してきました。PrismJS に追加パッケージをインストールして、CSS をチクチク書いていた頃と比較すると、コマンド一発で全てが揃うのは革命的に便利です。唯一惜しいところは、行番号を表示させる機能が実装されていない点です。ですが、現在進行形で議論 [^4] が進んでいます。そのため、近い将来に実装されると思います。

[^4]: GitHub, Feature: Number of Lines, Issue #37：https://github.com/expressive-code/expressive-code/issues/37
