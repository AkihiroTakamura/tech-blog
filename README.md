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
