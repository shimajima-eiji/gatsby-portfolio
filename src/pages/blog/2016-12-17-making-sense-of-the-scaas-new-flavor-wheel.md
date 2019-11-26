---
templateKey: blog-post
title: Jekyll+GitHubPagesからGatsby+Netlify+HeadlessCMSブログに移行します！
date: 2019-11-26T07:04:10.000Z
description: >-
  Pagesを使っていましたが、記事一つ追加するにもビルドが遅すぎたので、今後はGatsbyJS+Netlify+NetlifyCMSに移行したブログを活用していきます。
featuredpost: false
featuredimage: /img/flavor_wheel.jpg
tags:
  - tech
---
移行しました、と言わないのは今の段階で環境を作ったばかりだからです。
とりあえず先行で連絡まで。

後で読み返す時に、またこれからGatsbyJSを使って静的サイトジェネレーターによるブログやサイトを作りたい人向けに、Jekyllからの移行手順も載せておきます。

## 環境構築
[NetlifyCMSの公式ページ](https://www.netlifycms.org/)でGet Startedとやると、ウィザードっぽいものが起動して設定だけで環境ができます。
この時に適用されるスターターがNetlifyCMSだという点だけ認識しておけば後で対応が楽になります。

forkしたリポジトリはローカルにcloneして確認できます。
**思わずnpm upgradeしたくなりますが、バージョンの互換性がないため実行に失敗します。**
他のサイト等を参考にする場合、npm installで入ってくるパッケージのバージョンには注意してください。

## PWA化
```
npm install --save gatsby-plugin-manifest
npm install --save gatsby-plugin-offline
```
でpackage.jsonを更新して、
gatsby-config.jsのplugins部分に以下を追記します。
```
  plugins: [
    {
      resolve: `gatsby-plugin-manifest`,
      options: {
        name: `サイト名`,
        short_name: `スマホの表示`,
        start_url: `/`,
        background_color: `#6b37bf`,
        theme_color: `#6b37bf`,
        display: `standalone`,
        icon: `src/img/logo.svg`,
      },
    },
    `gatsby-plugin-offline`
  }
```
NetlifyCMSのスターターを使っている場合、別のプラグインも最初から入っているので、既に導入済みかもしれません。
執筆時点では入ってないので、手動で追加しました。

その後、通常の手順でサーバーを起動します。

### 補足：実行コマンド
多くのサイトではyarnコマンドですが、私はWindows（Sub system for Linux）なので、`gatsby develop`で実行しました。

なお、WSLだとaptなりyumで入れられるnpm自体のバージョンが古すぎるので、nodebrewからnodejsを入れるようにしましょう。

```
# curlぐらい使えると思うけど念のため
apt update
apt upgrade
apt install curl

# nodebrewを入れてnodejsをインストール
curl -L git.io/nodebrew | perl - setup
echo "export PATH=$HOME/.nodebrew/current/bin:$PATH" >>~/.bashrc
source ~/.bashrc

nodebrew install latest
nodebrew use latest
npm --version
```

## 動作確認
- http://localhost:8000/
- http://localhost:8000/___graphql
- http://localhost:8000/admin/
に接続できれば問題ないです。

主に使うのは上と下ぐらいで、真ん中のGraphQLは何か異常が起こった時ぐらいでしょうか。
エラーメッセージが出てくる場合、大体は設定周りがおかしいのでログを読んで一つずつ対応していきましょう。

## よくある勘違い
Jekyll+GitHubPagesから移行しましたが、ソースコードはGitHub上に残ります。
残るという表現は適切ではなくて、GitHub Pagesに上げたものをNetlifyにデプロイするので順番が逆です。

## 注釈記法が使えない？
この辺りは拡張機能で解決できそうですが、現状だと注釈記法が使えない事がわかっています。
jekyll時代だとフル活用していたので、これを対応する必要があります。

## どう考えても初心者向けではない
GitHub Pagesを使っていた時は直感的だったんですが、このやり方は環境構築から手間なので、何か考えないとダメですね。
