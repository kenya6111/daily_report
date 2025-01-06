# 見積もり
    - 922分（Vue3のみ）＝約15.5時間
    - 1日30minずつ消化していき、大体１ヶ月で完遂。
## 1章
- Vuejsはブラウザ側で実行されてブラウザ側でUIを構築する
- ページごとにサーバにアクセスするみたいな一般的なWEBの動きとは少し異なる
- 一回だけHTMLのページをリクエストで取得して、それ以降は新たにリクエストしたりせずにひたすらブラウザ側だけでUIの処理をするみたいなアプリのことをSPA(シングルページアプリケーション)という。
- ほとんどの処理を書いておいたのであとはここだけ書いておいてください。そしたらいい感じに動くようになりますよっていうのがフレームワーク

## 2章
- Vueはコンポーネントというものをたくさん作って、それをうまく配置することで1つのアプリケーションを作っているんです。（それぞれのファイルは.vueファイルで、単一ファイルコンポーネントという。SFCともいう。）
- ブラウザはシンプルなHTML,CSS,JSしか理解できない。なのでブラウザが理解できるようにvueファイルはjsのファイルとかに変換する必要がある

- Vite(ビート)はVue→ｊｓの変換を裏側で自動でやってくれる
- nodejs → 単純にｊｓを実行するソフトウェア。

- npm create vue@latest コマンドでvueプロジェクトが作成できる
- vite.config.js
    - viteのせっていファイル
    - viteでvueを使う設定などがされている
- package.json
    - project全体のさまざまな設定が書かれている
    - devDependencies → 開発の時だけ使うものが列挙される
    - このpackage.jsonに書かれたものたちは、プロジェクトにインストールは最初されていない。ただ書かれただけ。なので以下のコマンドでインストールする。
        - npm install 
    - インストールしたものはnode-modulesというディレクトrに入ってくる。
    - package.jsonのscriptsに書かれたものはコマンドに関するやつら。npm runの後ろに書いて実行できる
    ```json
      "scripts": {
        "dev": "vite",
        "build": "vite build",
        "preview": "vite preview",
        "lint": "eslint . --fix",
        "format": "prettier --write src/"
    },
    ```
    - npm run dev サーバ起動
    - npm run build　開発終わってリリースするよって時に使う。
        - こちら時効するとdistフォルダができる。このフォルダは公開用のものをコードが最適化されて詰められている。このdistフォルダをホスティングしてあげればリリーすできる
    - npm run preview
        - distフォルダの中身を確かめる。
    - npm run lint 
        - コードが正しく書けているかチェック
    - npm run format
        - lint と似ていてコードを綺麗にフォーマットしてくれる

- package-lock.json
    - npm install した時にinstallされたこのnode-modulesの中にあるパッケージたちの厳密なバージョンが「記録されている」
- jsconfig.json
    - vscodeが効率よくコード管理するために必要な設定ファイル
- editorconfig
    - vscodeだけじゃなくさまざまなエディタに一貫したコーディングスタイルを適用させるためのやつ
- index.html
    - プロジェクト全体のWebアプリの元になるhtmlファイルになります。
- prettier.json
    - prettierの設定ファイル
- srcフォルダ
    - ここが一番開発者にとってメイン
    - このソースフォルダ内のファイルに関してはビートはさまざまな変換処理をやってくれる
    - 逆にビートに何も操作されたくないファイルを置きたいときはpublicフォルダ内にファイル置く
- .vscodeフォルダ
    - vscodeの設定をするためのフォルダ
    - extentions.jsonファイルにはおすすめのVSCODEの拡張機能が一覧で表示されている。これがあるためにvscode起動時に右下におすすめの拡張機能はこれですってポップアップでていた、
    - settings.jsonには↑でインストールしたeslintやprettierのvscodeの拡張機能に対する設定がかかれている。
    - 拡張機能（vue.volar）
        - vueファイルを認識するための機能。ないとvueファイルが真っ白になる
（56分消化 2025/1/2時点）

- beatで開発されるアプリはindex.htmlがベースになっているとはさっき言った。
    - そのindex.htmlではmain.jsを読まれているのでmain.jsが実行される。

    ```js
    // main.js
    import {createApp} from 'vue' //node-modules内のvueフォルダ内のcreateAppを持ってきてくれる

    import App from './App.vue'

    createApp(App).mount('#app')// ちなみにcreateAppは関数になっており、これでvueのプロジェクトを作成する
    ```

    - App.vueには以下のように記載。これで一旦画面描画される。
    - vueはscript,template,styleタグで成り立つものである。
    ```vue
    <script setup>
        const username="tanaka"
        console.log(username)
    </script>
    <template>
        <h1>title</h1>
    </template>
    <style>
        h1{
        color:red;
        }</style>
    ```

    - scriptタグ内で書いた変数はtemplateタグ内で以下のように表示できる。このように自動で更新してくれる機能を「ホットモジュールリプレースメント」という。
    ```vue
    <script setup>
        const username="tanaka"
        console.log(username)
    </script>
    <template>
        <h1>title</h1>
        <h1>{{  username }}</h1>
    </template>
    <style>
        h1{
            color:red;
        }
    </style>
    ```

    - 上記では変数を入れているが実際は「式」が評価されて画面に出ている
    ```vue

    <template>
    <h1>title</h1>
    <h1>{{  username }}</h1>
    <h1>{{  price }}</h1>
    <h1>{{  price - 1 }}</h1>
    </template>
    ```

    - tempalteのボタンタグに@click="script内の関数名"とすることでボタンクリック時にscriptタグ内の関数を呼び出せる。
- リアクティビティ
    - 以下の例ではボタン押下時にscript内のprinteがインクリメントされるが、画面表示のpriceは9.99のまま。これはリアクティブじゃない。
    ```vue
    <script setup>
        const username="tanaka"
        console.log(username)
        let price=9.99
        function increment (){
        price += 1
        console.log(price)
        }
    </script>
    <template>
        <h1>title</h1>
        <h1>{{  username }}</h1>
        <h1>{{  price }}</h1>
        <h1>{{  price - 1 }}</h1>
        <button @click="increment">increment</button>
    </template>
    <style>
        h1{
        color:red;
        }
    </style>
    ```
    ![alt text](<スクリーンショット 2025-01-03 17.59.47.png>)


    - リアクティブにするにはリアクティブにしたい値をref()で囲む。で、するとpriceはrefオブジェクト内のvalueプロパティに格納されるので、更新する際はprice.value+=1という感じでrefオブジェクトのvalueプロパティに対して更新する。templateの方は.valueはつけなくてOK。
    ```vue
    <script setup>
        import {ref} from 'vue'
        const username="tanaka"
        console.log(username)
        let price=ref(9.99)
        console.log(price)

        const info=ref({
            para:100,
            para2:200
        })
        function increment (){
            info.value.para+=100
            price.value += 1
            console.log(price)
        }
    </script>
    <template>
        <h1>title</h1>
        <h1>{{  username }}</h1>
        <h1>{{  price }}</h1>
        <h1>{{  price - 1 }}</h1>
        <h1>{{  info.para }}</h1>
        <button @click="increment">increment</button>
    </template>
    <style>
        h1{
        color:red;
        }</style>
    ```
    - ref()で囲んだ値の返り血をrefオブジェクトという。
    ![alt text](<スクリーンショット 2025-01-03 18.04.26.png>)

（32分消化 2025/1/3時点）

- reactive()を使ってオブジェクトをリアクティブにする
    - reactive()を使うと参照時に「.value」を挟まないでよくなる。
    ```vue
    <script setup>
    import {ref, reactive} from 'vue'
    const username="tanaka"
    console.log(username)
    let price=ref(9.99)
    console.log(price)
    const info=ref({
        para:100,
        para2:200
    })

    const instructor = reactive({ // 新規追加
        name : 'tanaken',
        age : 25,
    })
    function increment (){
        info.value.para+=100
        price.value += 1
        instructor.age +=1// .valueを挟まないで良くなる。
        console.log(price)
    }

    </script>
    <template>
        <h1>title</h1>
        <h1>{{  username }}</h1>
        <h1>{{  price }}</h1>
        <h1>{{  price - 1 }}</h1>
        <h1>aaaa: {{  info.para }}</h1>
        <h1>bbbb:{{ instructor.age }}</h1>
        <h1>test!!{{ 3+4 }}</h1>
        <button @click="increment">increment</button>
    </template>
    <style>
        h1{
            color:red;
        }</style>
    ```
    - ちなみに公式ドキュメントでは基本t系にref関数を使うように記載がある。
# 3章
- templateタグ内の記載はViteなどによってjsのソースに変換される。
- templateタグ内の書き方をhtmlではなく、テンプレート構文という。そのテンプレート構文がhtmlの文法のルールに従うようになっている。

- 上記でも使用した二重波括弧内では単一の式のみ使える。if文みたいな構文は使えない
- また、templateでscript内の変数を参照する場合、script内でトップレベル（グローバルレベル）で宣言した変数雨でなければ参照できな
```vue
<script setup>
const count =10
const count2 =15
</script>
<template>
  <h1>{{ 3+4 }}</h1>
  <h1>{{ count+4 }}</h1>
  <h1>{{ count2+4 }}</h1>
  <button @click="increment">increment</button>
</template>
<style>
h1{
  color:red;
}</style>
```

（32分消化 2025/1/4時点）

- v-から始まる特別な属性
    - vue-jsはテンプレート構文として特別な属性を用意している。そういうのをディレクティブといって、必ず「v-」から始まる
    - vueは「v-」から始まる属性をいろいろ用意している
        - 以下の例ではv-textの書き方と{{ count}}の書き方は同じ結果になる
    ```vue
    <script setup>
    import {ref} from 'vue'
    let count =ref(10)
        function increment(){
        count.value +=10
    }
    </script>
    <template>
        <h1>{{ count }}</h1>
        <h1 v-text="count"></h1>
        <button @click="increment">increment</button>
    </template>
    <style>
        h1{
            color:red;
        }
    </style>

    ```

    - v-html
        - v-htmlに渡されたrefオブジェクトに格納された文字列（htmlの記述）をhtmlと認識し画面に表示してくれる
        - 以下の例では{{ message}}の方はそのまま「<h1>message</h1>」と出力され、
        - v-htmlの方はhtmlとして画面異表示される。
        ```vue
        <script setup>
            import {ref} from 'vue'
            let message =ref("<h1>message</h1>")
        </script>
        <template>
            <h1>{{ message }}</h1>
            <h1 v-html="message"></h1>
        </template>
        <style>
            h1{
                color:red;
            }
        </style>
        ```
        - ただ、v-htmlに適用する値は信頼できるもの（すなわちユーザの入力したものを入れてはいけない）のみ適用という風雨にしないといけない
        - 出ないとクロスサイトスクリプティングになってしまう。
    - v-bind
        - 普通のタグ内のTextContentにjsscriptタグ内の変数を表示させるときは二重波括弧「{{}}」でいいが属性値に表示させるときは「v-bind」を使う
        - 逆にv-bind使わず属性名＝”{{ ref変数 }}”ってのは機能しない
        - 省略した書き方で「v-bind」を省いて「:」だけ残した書き方もでき、esLintではこちらが推奨される
        - v-bindを複数の属性に一気に適用させるにはv-bind="{id:vueID, href:vueURL}"のように書く
        ```vue
        <script setup>
            import {ref} from 'vue'
            const vueURL =ref("https://vuejs.org")
            const vueID =ref("vueLink")
        </script>
        <template>
            <button v-on:click="">button</button>
        </template>
        <style>
            h1{
                color:red;
            }</style>
        ```
    - v-on(何かが起きた時に何かをしたい)
        - v-on:click="式"でクリック時に挙動を設定できる。
        - 省略記法として「v-on:」を「@」に置き換えられる
        ```vue
        <script setup>
            import {ref} from 'vue'
            let count=ref(10)
            function countUp(){
            count.value+=10
            }
        </script>
        <template>
        <h1>{{ count }}</h1>
        <button v-on:click="count+=100">increment</button>
        <button @click="count+=100">increment</button>
        <button @click="countUp">increment</button>
        </template>
        <style>
        h1{
        color:red;
        }</style>
        ```
        - @click(v-on:click)の部分をイベント、="count+=10"(="countUp")の部分をハンドラという
        - count++のように直接処理を書くパターンをインラインハンドラ、関数を書くぱたーんをメソッドハンドラという
    - イベントオブジェクトの取得
        - クリックイベントのようなブラウザが発生させているイベントは「イベント」が発生した時に同時に「イベントオブジェクト」というそのイベントの情報を持ったオブジェクトを生成している。vuejsはそれを取得できる。
        - v-onで呼び出した関数の引数には仮引数にeventが書かれなくともeventを呼び出せる。
        - tempalteでは$eventで絵べんTのオブジェクトを取得できている
        ```vue
        <script setup>
            import {ref} from 'vue'
            let count=ref(10)
            function countUp(){
                count.value+=10
                console.log(event)
                console.log(event.clientX)
                console.log(event.clientY)
            }
        </script>
        <template>
            <h1>{{ count }}</h1>
            <button v-on:click="count+=100">increment</button>
            <button @click="count = $event.clientX">increment</button>
            <button @click="count+=100">increment</button>
            <button @click="countUp">increment</button>
        </template>
        <style>
            h1{
            color:red;
            }
        </style>
        ```
        - 引数を和すには以下の通り
        ```vue
        <script setup>
            import {ref} from 'vue'
            let count=ref(10)
            function countUp(num){
            count.value+=num
            }
        </script>
        <template>
        <h1>{{ count }}</h1>
        <button @click="countUp(5000)">increment</button>

        </template>
        <style>
        h1{
        color:red;
        }</style>

        ```
    - stop prevent
        - preventDefaultはデフォルトの挙動を防ぐ
        - 以下だと、aタグにpreventDefaultしているので、aタグを押下しても画面遷移しない。
        ```vue
        <script setup>
            import {ref} from 'vue'
        </script>
        <template>
        <h1>{{ count }}</h1>
        <a href="https://vuejs.org" @click="$event.preventDefault()">increment</a>
        <button >increment</button>
        </template>
        <style>
        h1{
            color:red;
        }
        </style>
        ```

        - 以下の例ではstopPropagationをしている
        - vueでは@clickのついた以下のbuttonタグを押したら、親要素のdivの@clickも呼ばれる仕様になっている
        - Propagation=「伝播」であり、button押した時に親要素の@clickも呼ばれるような電波を止める働きがある。
        ```vue
        <script setup>
            import {ref} from 'vue'
            function testbutton(){
            console.log("testbutton")
            }
            function testdiv(){
            console.log("testdiv")
            }
        </script>
        <template>
        <h1>{{ count }}</h1>
        <a href="https://vuejs.org" @click="$event.preventDefault()">increment</a>
        <div @click="testdiv">
            <button @click="testbutton">increment</button>
            <button >increment</button>
            <button @click="$event.stopPropagation()">increment</button>
        </div>
        </template>
        <style>
        h1{
            color:red;
        }
        </style>

        ```

        - 両者とも省略して以下のようにかける
        ```vue
        <a href="https://vuejs.org" @click.prevent="">increment</a>
        <button @click.stop="">increment</button>
        ```
（30分消化 2025/1/6時点）