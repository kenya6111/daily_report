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
- イベント修飾子と似たものとしてキー修飾子がある
    - これはキーボード系のイベントで使える
    - @keyupでキーが上がった時、@keydownでキーが押された時に設定した式が発動する。
    ```vue
    <script setup>
    import {ref} from 'vue'
    const count = ref(0)
    </script>
    <template>
    <h1>{{ count }}</h1>
        <input type="text" @keydown="count++" @keyup="count++">
    </template>
    <style>
    </style>
    ```
    - 特定のキーに対しての時(以下の場合はスペースをダウンした時)
    ```vue
    <script setup>
    import {ref} from 'vue'
    const count = ref(0)
    </script>
    <template>
    <h1>{{ count }}</h1>
    <input type="text" @keydown.space="count++">
    </template>
    <style>
    </style>
    ```
- ディレクティブの構造
    - 「v-on:click.prevent="changeData"」
    - 「v-on」が名前。「click」が引数。「prevent」が修飾子。「changeData」が値。
- 角かっこ([])を使ってディレクティブの引数にscriptのデータを指定する方法
    - @の後ろに[変数名]で指定する V
    ```vue
    <script setup>
    import {ref} from 'vue'
    const count = ref(0)
    const eventName = 'keyup'
    </script>
    <template>
    <h1>{{ count }}</h1>
    <input type="text" @[eventName].space.delete="count++" >
    </template>
    <style>
    </style>
    ```
- v-modelを使用してInputに入力した値をscriptでの変数と同期させる
```vue
<script setup>
    import {ref} from 'vue'
    const userInput=ref('')
</script>
<template>
  <input v-model="userInput" type="text" >
  <h1>{{ userInput }}</h1>
</template>
<style>
</style>

```
- 式もreactiveに扱いたいときはcomputed()を使う。
    - 以下の例ではinputに4以上入れれば{{ evaluation}}の表示がBadからGoodに切り替わる。
    - computedは.valueにアクセスするたびにcomputedが実行される
    - あとcomputed内のrefオブジェクトが1つでも変更されればcomputedが評価して最新の値を算出してくれる優れもの。ようは値を監視してくれる
```vue
<script setup>
    import { ref,computed } from 'vue'
    const score =ref(0)
    const evaluation = computed(()=>{
      return score.value > 3 ? 'Good' : 'Bad'
    })
    console.log(evaluation.value)
</script>
<template>
  <h1>{{ evaluation }}</h1>
  <input v-model="score" type="text" >
  <h1>{{  score }}</h1>
</template>
<style>
</style>
```
    - computedでは、要は値を監視して、変更あれば実行して最新お値を評価して出すって役割なので、computedで算出した値を変更はしてはいけないってか普通しない。読み取り専用って感じ。またcomputed関数内で外部の変数を変更するお湯なことはしない。あくまで最新の値を見て評価するだけ
    - computedはcomputed関数内のreactiveなデータを監視して変更されればcomputedを実行するという仕組みだが、その変更を監視する仕組みをリアクティブエフェクトという。
    - このリアクティブエフェクトはcomputed以外でも使われており、それは「watcher」とテンプレートで使われている
    - テンプレートでのreactiveeffectは、変更を検知したら再レンダリングする。
    - computedの結果であるevaluationがtemplate内で使われていない場合、毎回最新の値で評価しても無駄なので、（表示しなくて誰もミない）computedは時効されなくなる
（35分消化 2025/1/7時点）




## 19章 (はじめに Vue2)
- vueの特徴
    - 簡単、柔軟、高性能
    - jsfiddle
        - html css jsをすぐ試して遊べるやつ
    - 
    ```html
    <script src="https://cdn.jsdelivr.net/npm/vue@2.7.11/dist/vue.js"></script>

    <div id="app">
    <p>
    hello world
    </p>
    </div>
    ```
    ```js
    new Vue({
	    el: '#app'
    })
    ```
- 上記のソースの解説
    - htmlでVueのCDNを読み込んで、
    - jsでnew Vue()でVueを使いますって宣言しました、
    - new Vue()でVueインスタンスを宣言
    - その中のel: '#app'で、htmlのidがappに対してvueインスタンスを使いますよって宣言している
    - elは要素という意味
    ```html
    <script src="https://cdn.jsdelivr.net/npm/vue@2.7.11/dist/vue.js"></script>
    <div id="app">
    <p>
    {{message}}
    </p>

    </div>
    ```

    ```js
    new Vue({
	el: '#app',
    data: {
        message:'hello world vue'
    }
    })
    ```
- 上記ソースの解説
    - Vueインスタンスではデータを持つことができ、dataというプロパティを持てる
    - このデータ内に定義したものをhtmlで{{ }}で表示できる


    ```html
    <script src="https://cdn.jsdelivr.net/npm/vue@2.7.11/dist/vue.js"></script>

    <div id="app">
    <p>
    {{message}}
    </p>
    <button v-on:click="reverseMessage">メッセージ反転
    </button>

    </div>
    ```
    ```js
    new Vue({
	el: '#app',
    data: {
        message:'hello world vue'
    },
    methods:{
        reverseMessage: function(){
            this.message = this.message.split('').reverse().join('')
            }
    }
    })
    ```
    - v-on
        - vueが用意している特別の属性
        - v-on:clickとすることで、クリック時に上記ソースでいうreverseMessageが呼ばれる。
    - VUeインスタンスは加えてmethodsという領域も持つことができ、いろんな処理を定義できる。
        - methodsはいろんな関数が並べられている場所。
    - methods内の関数内でthis.messageとす流書き方で、Vueインスタンス内のmessageプロパティにアクセスしますという意味になる

# 20章（これがVuejsの基礎、テンプレート構文だ！ Vue2）
- そもそもテンプレートとは何か
    - さっきのソースでいう、以下の部分
    - これはあくまでもhtmlではなく「テンプレート」を書いている
    - vueがこのテンプレートを読んで見て、最終的にhtmlとして出してくれる。
    - なので私たちはhtmlを書いているのではない。
    ```html
        <div id="app">
                <p>{{ message }}</p>
            <button v-on:click="reverseMessage">メッセエージ反転</button>
        </div>
    ```

- new Vue()はvueの心臓のような部分
- テンプレートでかく二重中括弧の中身はVueインスタンス内のdataプロパティ内で持つ値である
    - data.messageと書きたくなるが、Vueではテンプレートで直接data内のプロパティをかける
    - {{}} ないは式を書くことができる、jsのね。（でもあくまで単一の式だけ書ける）
    - methods内の関数も{{}}内で呼ぶことができる
    ```html
    <!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Document</title>
</head>
<body>
    <script src="https://cdn.jsdelivr.net/npm/vue@2.7.16/dist/vue.js"></script>
    <div id="app">
        <p>{{ message }}</p>
        <p>{{ number + 3 }}</p>
        <p>{{ ok? 'YES': 'NO'}}</p>
        <p>{{ sayHi()}}</p>
        <button v-on:click="reverseMessage">メッセエージ反転</button>
        </div>
        <script>
            new Vue({
                el:'#app',
                data: {
                    message: 'helloworld',
                    number:3,
                    ok:true
                },
                methods: {
                    reverseMessage: function(){
                        this.message = this.message.split('').reverse().join("")
                    },
                    sayHi: function(){
                        return 'Hi'
                    }
                }
            })

        </script>
    </body>
    </html>
    ```