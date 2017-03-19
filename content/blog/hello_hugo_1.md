+++
categories = ["GitHub Pages"]
tags = ["Hugo"]
slug = "hello_hugo_1"
date = "2017-03-19T21:19:14+09:00"
title = "GitHub PagesにHugoでブログを作りました①"
banner = "img/placeholder.png"
+++


GitHub Pagesでブログ公開できるようにしました。

試行錯誤しながら作っているので備忘録もかねて書いていきます。


## できるようになること
- Macのローカル環境でプレビュー
- `_MY_ACCOUNT_.github.io/blog`に公開


## Hugoセットアップ
MacにHugoをインストールします。

```sh
$ brew install hugo
```

`github.io/blog`で公開したいので、リポジトリ名を意識して`hugo new`します。

```sh
$ hugo new site blog
$ cd blog
```

[テーマ](http://themes.gohugo.io/)を選んでインストールします。

ものによってはBlog向きではないのもあるので、いろいろ見てみてください。

[Universal](http://themes.gohugo.io/hugo-universal-theme/)を使いました。

```sh
$ git clone --depth 1 --recursive https://github.com/devcows/hugo-universal-theme.git themes
```

themesディレクトリにcloneされるので、テーマ用データ中の`config.toml`をblogディレクトリ直下にコピーします。

```sh
blog/
  ...
  themes/
    hugo-universal-theme/
      exampleSite/
        content/
        data/
        static/
        .gitignore
        config.toml
```

## 記事を作ってプレビューしてみる
まずは記事を作ります。

`hugo new`で作ったmdファイルは、contentディレクトリ配下に生成されます。

インストールしたテーマによってcontent配下の名前は変わってくるので確認しておいてください。

```sh
$ hugo new blog/hello_world.md
$ vi content/blog/hello_world.md
```

```md
+++
tags = ["hugo"]
categories = ["hello world"]
+++

# Hello World
hello world
```

ここまでできたら、さっそくプレビューしてみましょう。

ローカル環境でwebサーバが起動するので、`localhost:1313`でアクセス可能です。

```sh
$ hugo server
```

## Github Pagesで公開する
Gighub Pagesでなんらかのサイト公開する方法は大きく3種類あります。

1. `_MY_ACCOUNT_.github.io`リポジトリを作成して、masterブランチにサイトデータをpushする
2. 好きな名前のリポジトリを作成して、`gh-pages`ブランチにサイトデータをpushする
3. 2.と同様に好きな名前リポジトリを作成して、`docs`ディレクトリにサイトデータをpushする

1.は、`_MY_ACCOUNT_.github.io`でアクセスするwebサイトを公開できます。2.と3.は`_MY_ACCOUNT_.github.io/_repository名_`でアクセスするwebサイトを公開できます。

今回は3.の方法で`rvillage.github.io/blog`に公開するので、Githubで`blog`リポジトリを作成しておきます。

そのリポジトリに、`hugo new`で生成した`blog`ディレクトリをpushします。

```sh
$ git commit --message "Initial commit"
$ git push
```

次に公開用データを作ってpushします。

まずは、docsディレクトリの作成と、`config.toml`を修正します。

```sh
$ mkdir docs
```

```toml:config.toml
# config.toml
baseurl = "https://rvillage.github.io/blog/"
publishDir = "docs"
```

次に、サイトデータを生成します。

```sh
$ hugo
```

docsディレクトリ配下にサイトデータが生成されるので、pushします。

```sh
$ git commit --message "Publishing hello_world.md"
$ git push
```

pushしたら、githubのblogリポジトリページで、`Settings -> GitHub Pages -> Source`で`master branch /docs folder`を設定します。

`_MY_ACCOUNT_.github.io/blog`にアクセスすると、作成したサイトが表示されます。

## まとめ
これで、Github Pagesでサイト公開できるようになったと思います。

ただ、あくまでサイト公開できただけでまだまだ（仮）レベルのものです。

ざっとやりたいことを考えてみてもたくさん出てきますので、第②回に続きます。

1. ブログタイトル
2. バナー
3. ソーシャルリンク
4. Google Analytics

## 参考サイト
- https://gohugo.io/tutorials/github-pages-blog/#deployment-via-docs-folder-on-master-branch
