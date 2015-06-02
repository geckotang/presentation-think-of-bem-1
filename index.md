## BEMおじさんは考えた vol.1

----

## BEMを使う人が増えた

最近、色々なウェブサイトを view-source: して見ると、BEMを用いた設計が増えてる気がする

----

## でもそれって

本当にBEMできてるの？

<small>
  ※以降、@GeckoTangのBEMの解釈なので、必ずしも正解ではありません。<br>
  ※また、勝手にHTMLから読み取ってるので、区切り文字の解釈に間違いがあるかもしれません。
</small>

----

## あるある1：蛙の子は蛙の子は蛙...

```html
<div class="block">
  <div class="block__body">
    <div class="block__article">
      <div class="block__article__inner">
<!-- 略 -->
```

**Block\__Element\__Element**になってるぅううう

--------

### 解決案1：子の子であっても並列に

```html
<div class="block">
  <div class="block__body">
    <div class="block__article">
      <div class="block__article-inner">
<!-- 略 -->
```

- `.block`というBlockがあって、
  - `.block__body`というElementが属している
  - `.block__article`というElementが属している
  - `.block__article-inner`というElementが属している

とする。

また、`.block__article`と`.block__article-inner`に依存関係はない。

--------

### 解決案2：粒度を変える

```html
<div class="block">
  <div class="block__body">
    <div class="article">
      <div class="article__inner">
<!-- 略 -->
```

- `.block`というBlockがあって、
  - `.block__body`というElementが属している
    - `.article`というBlockがあって、
      - `.article__inner`というElementが属している

とする。

さらに粒度を細かくし、`.block`と`.article`を別のBlockにしてあげる。

--------

### 解決案3：粒度を変える(2)

```html
<div class="block">
  <div class="block__body">
    <div class="block__article  article">
      <div class="article__inner">
<!-- 略 -->
```

- `.block`というBlockがあって、
  - `.block__body`というElementが属している
    - `.block__article`というElementは、`.article`というBlockでもあり、
      - `.article__inner`というElementが属している

BEMでは、ElementとBlockが混ざった状態が、許可されています。

----

## あるある2：迷子

```html
<!-- 略 -->
<div class="block">
  <div class="article__item">
    <div class="article__title">...</div>
    <div class="article__image">...</div>
<!-- 略 -->
```

Blockが不在で、Elementだけが存在する

--------

### 解決案1：親をちゃんと置く

```html
<!-- 略 -->
<div class="block">
  <div class="block__item">
    <div class="article">
      <div class="article__title">...</div>
      <div class="article__image">...</div>
<!-- 略 -->
```

もしくは、

```html
<!-- 略 -->
<div class="block">
  <div class="block__item  article">
    <div class="article__title">...</div>
    <div class="article__image">...</div>
<!-- 略 -->
```

とかにする。

--------

### 解決案2：粒度を細かくする

```html
<!-- 略 -->
<div class="block">
  <div class="article">
    <div class="title">...</div>
    <div class="image">...</div>
<!-- 略 -->
```

もし、Elementだけを使いたい、使い回したいみたいなことがあれば、それはElementではなくBlockに。

<small>※上記のコードは、極端な例ですが。</small>

--------

### 解決案3：親がいないと破綻するようにする

```css
.article { /* ... */ }
.article__title { /* ... */ }
.article__image { /* ... */ }
```

詳細度が同じなので、本来はこれが良いが、

```css
.article { /* ... */ }
.article .article__title { /* ... */ }
.article .article__image { /* ... */ }
```

- Elementが、Blockに属していないとスタイルが当たらない。
- Element単体で使われた時に、スタイルが当たらないので、間違いにきづける。

<small>※詳細度が上がるのであんまりやりたくはない。</small>

----

## まとめ

- Block\__Element\__Elementになるようなら、粒度を見なおす
- 親が不在のElementはダメ、それはBlockなのでは？
- Blockの粒度はなるべく細かくすると幸せになれる（気がする）
