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

## github pages の公開設定

repository の settings -> Pages -> Build and deployment

Source を `GitHub Actions` に変更

![image](https://i.imgur.com/HWLIITS.png)

## github workflow の設定

`.github/workflows/gh-pages.yml`

```yml
name: Deploy Hugo site to Pages

on:
  # Runs on pushes targeting the default branch
  push:
    branches:
      - master

  # Allows you to run this workflow manually from the Actions tab
  workflow_dispatch:

# Sets permissions of the GITHUB_TOKEN to allow deployment to GitHub Pages
permissions:
  contents: read
  pages: write
  id-token: write

# Allow only one concurrent deployment, skipping runs queued between the run in-progress and latest queued.
# However, do NOT cancel in-progress runs as we want to allow these production deployments to complete.
concurrency:
  group: "pages"
  cancel-in-progress: false

# Default to bash
defaults:
  run:
    shell: bash

jobs:
  # Build job
  build:
    runs-on: ubuntu-latest
    env:
      HUGO_VERSION: 0.111.3
    steps:
      - name: Install Hugo CLI
        run: |
          wget -O ${{ runner.temp }}/hugo.deb https://github.com/gohugoio/hugo/releases/download/v${HUGO_VERSION}/hugo_extended_${HUGO_VERSION}_linux-amd64.deb \
          && sudo dpkg -i ${{ runner.temp }}/hugo.deb
      - name: Install Dart Sass Embedded
        run: sudo snap install dart-sass-embedded
      - name: Checkout
        uses: actions/checkout@v3
        with:
          submodules: recursive
          fetch-depth: 0
      - name: Setup Pages
        id: pages
        uses: actions/configure-pages@v3
      - name: Install Node.js dependencies
        run: "[[ -f package-lock.json || -f npm-shrinkwrap.json ]] && npm ci || true"
      - name: Build with Hugo
        env:
          # For maximum backward compatibility with Hugo modules
          HUGO_ENVIRONMENT: production
          HUGO_ENV: production
        run: |
          hugo \
            --gc \
            --minify \
            --baseURL "${{ steps.pages.outputs.base_url }}/"
      - name: Upload artifact
        uses: actions/upload-pages-artifact@v1
        with:
          path: ./public

  # Deployment job
  deploy:
    environment:
      name: github-pages
      url: ${{ steps.deployment.outputs.page_url }}
    runs-on: ubuntu-latest
    needs: build
    steps:
      - name: Deploy to GitHub Pages
        id: deployment
        uses: actions/deploy-pages@v2
```

この状態で git push すると Github Action によって build が実行される
