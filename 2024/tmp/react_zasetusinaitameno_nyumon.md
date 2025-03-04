## 1章
- Reactで挫折する人はjavascriptの基礎知識が足りていない
- 

## 3章
- まずは概念を理解

- webpack,Vite などのモジュールバンドラーを使用
- Babel,SWCなどのトランスパイラを使用

- DOM
  - document object model
  - htmlを解釈して木構造で表現したもの
  - これまでは画面の表示変えたり追加したりする際はDOMを直接操作していた
    - 素のjsやjqueryなどがまさにそれをしている

  - この従来のDOM操作の何が問題なのかというとレンダリングコスト高い（画面お表示が遅くなったり）、コードが複雑化してどこで何してるか意味不明になる
  - この解決のために作られたのが仮想DOM
    - jsのオブジェクトで仮想的に作られたDOMのこと
    - いきなりDOMを操作するのではなく、仮想DOMのバージョンの前後で差分を検出して差分だけを実際のDOMに反映する
    - よってレンダリングコスト削減や高速化が実現できている
- パッケージマネージャーについて
  - かつてのjavascript開発では単一のｊｓファイルに処理を全て記述していた
    - 処理が複雑化するにつれコードがカオスか
    - コードの再利用ができなくなっていく
  - そこからjsファイルを大きなまとまりの処理ごとに作り、それを読み込んで使うように改善されていった
    - これによってコードの再利用や共通化ができるようになっていった
    ```js
      <script src="js/vendor/jquery.min.js"></script>
      <script src="js/vendor/jquery.cookie.js"></script>
      <script src="js/vendor/jquery.easing.1.3.js"></script>
      <script src="js/common.js"></script>
      <script src="js/util.js"></script>
      <script src="js/app.js"></script>
    ```
    - しかし以前として問題はあった。
      - jsファイルの読み込み順を意識しないとエラーが起きてしまったり
      - 特定のファイルで使用している変数や関数がどのファイルで使用してるのかわからない
      - って感じでまだまだ厳しーーーーって面あった
  - そこから試行錯誤ありモダンjavascriptではnpmやyarnなどのパッケージマネージャーを使っている
    - こいつを使うとこれまでの問題を解決してくれる
    - 読み込み順でエラーが起きないように依存関係を依存関係を勝手に解決してくれたり
    - import先が明示的にわかる
    - 世界中で公開されているパッケージをコマンド1つで利用可能だったり
    - チーム内の共有も簡単に似合ったり

  
  ![alt text](../../image/image25.png)
  - NPM
    - オンライン上にさまざまなパッケージが公開されている場所がある。そこからコマンド使って自分のPCにモジュールをインストールする
  - package.json
    - 何をどのバージョンでインストールしたかが記録されるファイル
  - lock
    - インストールしたモジュールが裏で使っている他のモジュールやそのバージョンなど。ここは直接編集しない
  - node_modules
    - 各モジュールの実態
    - サイズが膨大。
    - githubにはあげない

- ECMAjavascriptとは
  - jsはこんなルールで書くぜ！っていう規格のこと。
  - European computer manufactuar association
  - バージョンのよびかた ES5と西暦での呼び方 ES2014が同じだったりして明らかにややこしいので西暦での呼び方が推奨されている

- モジュールバンドラー
  - 複数のファイルを1つにまとめるためのもの。
  - js,css,imageなどを1つにまとめる
  - この技術が生まれた経緯は、
  - 「開発時にjsファイルを細かく機能ごとにわけて開発したほうがいいね！」
  - 再利用性も開発効率も上がるしね！
  - でも本番環境でファイル分かれてる必要なくね？？
  - 読み込み数も増えてパフォーマンス的に悪そう
  - じゃあ本番にじゃあ本番にビルドするときに1つにまとめてくれるもの作るか！
  - 依存関係は？
  - それも判定していい感じにまとめてくれるものにしよう！
- トランスパイラー
  - javascriptの可惜意識方を古い記法に変えてくれる
  - ES6でめっちゃ新しい気泡増えたやん！！
  - どんどん使おう！
  - 待て！対応してないブラウザがいっぱいあるみたいや、、
  - I Eとか。。
  - くそ〜。じゃあ諦めて古い記法でかくかー
  - いや、開発は新しい記法でやって、実行時は古い記法でやれアバいいのでは？？
  - それだ！

## 4章
- テンプレート文字列
  - 従来のjsでは文字列結合が「＋」でしかできなかった。結構めんどくさい。
  - これを簡単位できるようにしたのがテンプレート文字列
- アロー関数
  - ES2015で追加された新しい関数の定義方法
- 分割代入
  - ES2015で追加
  - 以下のようにオブジェクト内のものをそれぞれの変数に抜き出して、そのまま使えるやつ。
  ```js
  const myProfile ={
    name:"ジャケえ",
    age:31
  }

  const message1=`名前は${myProfile.name}です。年齢は${myProfile.age}です`
  console.log(message1)

  const {name, age} = myProfile
  const message2=`名前は${name}です。年齢は${age}です`
  console.log(message2)
  ```

  - 配列の場合はこんか書き方でやる。
  ```js
  const myProfile2 =[222,333,444]

  const [val1,val2,val3]= myProfile2
  console.log(val1)
  console.log(val2)
  console.log(val3)
  ```

  - オブジェクト内のキーとバリューが同じ場合は省略できる。
  ```js
  const age3=21

  const myProfile3 ={
    name3,
    age3
  }
  const myProfile4={name3, age3}

  console.log(name3)
  console.log(age3)
  ```

  - スプレッド構文（配列の展開）
  ```js
  const arr1=[1,2,3,4]
  console.log(arr1)// [ 1, 2, 3, 4 ]
  console.log(...arr1)// 1 2 3 4   配列の中身を順番に処理してくれる


  const sumFunc = (num1 , num2)=> console.log(num1+num2)
  sumFunc(arr1[0], arr1[1]) //3
  sumFunc(...arr1) //3

  ```
  - スプレッド構文（まとめて受け取る）
  ```js
  const arr2 =[1,2,3,4,5]
  const [num1,num2,...num3]=arr2// num1に1, num2に2, num3に残りを配列として分割代入するってやつ
  console.log(num1)// 1
  console.log(num2)// 2
  console.log(num3)// [3,4,5]
  ```

  - スプレッド構文（配列のコピー、結合）
  ```js
  const arr4=[10,20]
  const arr5=[30,40]

  const arr6 = [...arr4]
  const arr7 = [...arr4, ...arr5]
  const arr8 = arr4

  console.log(arr6)//[ 10, 20 ]
  console.log(arr7)// [ 10, 20, 30, 40 ]
  console.log(arr8)//[ 10, 20 ] arr6と出力結果は変わらないが、こちらはarr4の三勝を引き継いでいるので、結果同じ場所を参照していることにあんる
  ```

## 5章
  - 指定した要素の1つ上隣の要素を取得
  previousElementSibling

  - 指定した要素の1つした隣の要素を取得
  nextElementSibling

  - 指定した要素を親要素の中から一番近いものを取得
  closest

  - 子要素の中で１番目のものを取得
  firstElementChild

  - 子要素を削除
  removeChild