# setup

## install

```
brew install hugo
```

## create project

```
hugo new site tech-blog
cd tech-blog
code .
```

## git

```
git init
```

## add theme

```
git submodule add https://github.com/theNewDynamic/gohugo-theme-ananke themes/ananke
git submodule add https://github.com/Vimux/Mainroad themes/mainroad
echo "theme = 'ananke'" >> hugo.toml
```

## add content

```
hugo new article/1.md
```

`1.md` は以下のように

```md
---
title: "最初に作ったページ"
date: 2023-06-16T15:59:24+09:00
featured_image: "/images/article/1_blogger_man.png"
---

## ブログ開設！

というわけで、この度、ブログをはじめました。

よろしくどーぞ

{{< tweet user="rinda2001" id="1664084172670140417" >}}
```

tweet の部分は twitter ユーザ ID(@抜き) と、tweet の ID を指定している

## 共通レイアウト

```
touch content/_index.md
```

`_index.md` は以下のように

```md
---
description: "気ままで、テックな、ゆる投稿"
---

# テック寄りなブログです

技術発信や気づき、日々の備忘録として

不定期でブログを書いています。
```

## hugo.toml の編集

```
baseURL = 'http://localhost:1313/'
languageCode = 'ja'
languageName = 'Japanese'
defaultContentLanguage = 'ja'
hasCJKLanguage = true
title = 'ゆるふわブログ'
theme = 'ananke'

[params]
  text_color = 'dark-gray'
  background_color_class = 'bg-navy'
  author = 'Akihiro Takamura'
  favicon = '/images/profile.png'
  site_logo = '/images/profile.png'

[[params.ananke_socials]]
  name = 'twitter'
  url  = 'https://twitter.com/rinda2001'

```

## 画像の用意

`/static` 配下に画像を配置

## ローカルサーバ実行　

```
hugo server
```

# GitHub Pages で公開

## repository の作成

github から repository を作成する

この時、repository name を以下のようにする

- `username.github.io` で公開する場合
  - `username.github.io` という名前でリポジトリを作る
- `username.github.io/hoge` で公開する場合
  - `hoge` という名前でリポジトリを作る

また、無料で GitHub Page を使うため、リポジトリは `public` にする

## base url の変更

`hugo.toml` の `baseUrl` をさっき作ったページにする

```
baseURL = "https://xxxxxxxx.github.io/hoge"
```

## github workflow の設定

`.github/workflows/gh-pages.yml`

```yml
name: github pages

on: ["push", "workflow_dispatch"]

jobs:
  deploy:
    runs-on: ubuntu-18.04
    steps:
      - uses: actions/checkout@v2
        with:
          submodules: true # Fetch Hugo themes (true OR recursive)
          fetch-depth: 0 # Fetch all history for .GitInfo and .Lastmod

      - name: Setup Hugo
        uses: peaceiris/actions-hugo@v2
        with:
          hugo-version: "0.113.0"
          extended: true

      - name: Build
        run: hugo --minify -F

      - name: Deploy
        uses: peaceiris/actions-gh-pages@v3
        with:
          github_token: ${{ secrets.GITHUB_TOKEN }}
          publish_dir: ./public
          user_name: "github-actions[bot]"
          user_email: "github-actions[bot]@users.noreply.github.com"
          commit_message: ${{ github.event.head_commit.message }}
```

この状態で git push すると Github Action によって build が実行される

## github pages の公開設定

repository の settings -> Pages -> Build and deployment

Source を `GitHub Actions` に変更

![image](https://i.imgur.com/HWLIITS.png)
