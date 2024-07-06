## ■コードブロック

```ruby:testets.java
puts 'The best way to log and share programmers knowledge.'
```

```diff_ruby
- puts 'The best way to log and share programmers.'
+ puts 'The best way to log and share programmers knowledge.'
```

```diff_javascript
- console.log('hello')
+ console.log('world')
```


## ■コードスパン
- `puts 'Qiita'` と書くことでインライン表示することも可能です。<br>
`` `バッククオート` `` や ``` ``2連続バッククオート`` ``` も記述できます。
`#ffce44`<br>
`rgb(255,0,0)`<br>
`rgba(0,255,0,0.4)`<br>
`hsl(100, 10%, 10%)`<br>
`hsla(100, 24%, 40%, 0.5)`<br>

## ■テキストの装飾
- Headings - 見出し
# これはH1タグです
## これはH2タグです
###### これはH6タグです

_ か * で囲むとHTMLのemタグになります。Qiitaでは` *italic type* になります。
__ か ** で囲むとHTMLのstrongタグになります。Qiitaでは **太字** になります。
打ち消し線を使うには ~~ で囲みます。 ~~打ち消し~~
<!-- open属性なし -->
<details><summary>サンプルコード（open属性なし）</summary>

(上に空行が必要)

```rb
puts 'Hello, World'
```
</details>

<!-- open属性あり -->
<details open><summary>サンプルコード（open属性あり）</summary>

(上に空行が必要)

```rb
puts 'Hello, World'
```
</details>

:::note info
インフォメーション
infoは省略可能です。
:::

:::note warn
警告
○○に注意してください。
:::

:::note alert
より強い警告
○○しないでください。
:::

## ■リスト
- リスト
* リスト
+ リスト
3\. リスト
4\. リスト
エスケープされています

<dl>
  <dt>リンゴ</dt>
  <dd>赤いフルーツ</dd>
  <dt>オレンジ</dt>
  <dd>橙色のフルーツ</dd>
</dl>

<dl>
  <dt>リンゴ</dt>
  <dd> とても<strong>赤い</strong>フルーツ </dd>
</dl>

## ■チェックボックス
- [ ] タスク1
- [x] タスク2

## ■引用

> 文頭に>を置くことで引用になります。
> 複数行にまたがる場合、改行のたびにこの記号を置く必要があります。
> **引用の上下にはリストと同じく空行がないと正しく表示されません**
> 引用の中に別のMarkdownを使用することも可能です。
> 
> > これはネストされた引用です。
> > > これはさらにネストされた引用です


### ■水平線
* * *
***
*****
- - -
---------------------------------------

## ■リンク
[Qiita](http://qiita.com "Qiita Home")

[Qiita](http://qiita.com)

[ここ][link-1] と [この][link-1] リンクは同じになります。
[link-1] という書き方もできます。

[link-1]: http://qiita.com/


## ■画像埋め込み
![Qiita](https://qiita-image-store.s3.amazonaws.com/0/45617/015bd058-7ea0-e6a5-b9cb-36a4fb38e59c.png "Qiita")

## ■テーブル
| Left align | Right align | Center align |
|:-----------|------------:|:------------:|
| This       | This        | This         |
| column     | column      | column       |
| will       | will        | will         |
| be         | be          | be           |
| left       | right       | center       |
| aligned    | aligned     | aligned      |


## ■数式の挿入

```math
\left( \sum_{k=1}^n a_k b_k \right)^{\!\!2} \leq
\left( \sum_{k=1}^n a_k^2 \right) \left( \sum_{k=1}^n b_k^2 \right)
```

$a = \{1, 2, 3\}$

$a = \\{1, 2, 3\\}$
## ■
## ■
## ■
## ■
## ■
## ■
## ■
https://x.com/Qiita/status/1365218441011990531


## ■その他
\# H1
エスケープされています
