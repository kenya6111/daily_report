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
- 1000分=16.8時間
- 1日30分ずつで33日かかる計算。
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
    - Vueインスタンス内でVueインスタンス内のプロパティを呼ぶときはthisを使う。
    
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
        <p>{{ sayHi2()}}</p>
        p
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
                    },
                    sayHi2: function(){
                        return this.message;
                    }
                }
            })

        </script>
    </body>
    </html>
    ```
- ディレクティブについて
    - Vue.jsにおける特別な属性のこと。
    - vuejs専用のディレクティブ（属性）は先頭に「v-」がついている
    - 以下のソースではv-textを使っている。v-text="message"と{{ messsage }}は同じ意味
    - 以下のソースではv-bindも使っている.htmlの属性をVueインスタンスのプロパティによって束縛する役割があります。
    - v-bindはコロン『:』に置き換えることができます。（省略記法）
    - dataプロパティのmyNameの値を「太郎」にすると、テキストボックスの中身(= value)は「太郎」になりますが、
    テキストボックスの中身(= value)を「太郎」にしても、dataプロパティのmyNameの値は「太郎」になりません。
    v-bind：Vueインスタンス内のデータ → html属性の単方向のみ
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
            <p v-text="message"></p>
            <a v-bind:href="url">google </a>
            <a :href="url">google </a>
        </div>
        <script>
            new Vue({
                el:'#app',
                data: {
                    message: 'helloworld',
                    url: 'https://google.com',
                },
                methods: {
                }
            })

        </script>
    </body>
    </html>
    ```
    - 「v-once」→一回だけ描画する

    - v-bind={}とすることで、バインドするものをまとめてかける。
    - :[変数]とするとこで、属性名も設定できる。
    - Vueインスタンス側でオブジェクト作ってバインドするやり方もできる。
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
            <a :[attribute]="url"> google </a>
            <a v-bind="{href:urlTwitter,id:number}">Twitter</a>
            <a v-bind="twitterObject">Twitter</a>
        </div>
        <script>
            new Vue({
                el:'#app',
                data: {
                    attribute:'href',
                    url:'https://google.com',
                    urlTwitter:'https://twitter.com',
                    number:30,
                    twitterObject:{
                        href:'https://twitter.com',
                        id:30
                    }
                }
            })

        </script>
    </body>
    </html>
    ```
    - v-on
        - DOMが提供するイベントが発生したものを検知
        - v-on:イベント名="式または関数名"とすることで、クリック時に処理が実行される。
        - 関数名には（）はつけなくてもつけてもどっちでも良い。今回は関数は引数がないのでつけていない。
        - イベントのリファレンス雨はこちら（https://developer.mozilla.org/ja/docs/Web/Events）
        - v-on:mousemoveで対象の要素にマウスが載っている時にイベントが発火し続ける
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
            <p>{{number}}   回クリックされています</p>
            <button v-on:click="number+=1">ボタン</button>
            <button v-on:click="countup">ボタン</button>
            <p v-on:mousemove="changemouseposition">マウスを乗せてください</p>
            <p>X:{{x}} Y:{{y}}</p>
            <button v-on:click="countup">ボタン</button>
            
        </div>
        <script>
            new Vue({
                el:'#app',
                data: {
                    number:0
                },
                methods:{
                    countup: function(){
                        this.number+=1
                    },
                    changemouseposition: function(event){
                        console.log(event)
                        this.x=event.clientX
                        this.y=event.clientY
                    }
                }
            })

        </script>
    </body>
    </html>
    ```
    - イベントオブジェクトとは
        - そのイベントの情報が入ったオブジェクトのこと。
        - 上記のようにイベントオブジェクトは関数で自動で受け取れていたが、じゃあeventオブジェクト以外に、自分で引数を渡す設定したときにどんな書き方なんの？って時、以下のように書く。（$event）
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
            <p>{{number}}   回クリックされています</p>
            <p>X:{{x}} Y:{{y}}</p>
            <button v-on:click="countup(2)">ボタン</button>
            <p v-on:mousemove="changemouseposition(3,$event)">マウスを乗せてください</p>
        </div>
        <script>
            new Vue({
                el:'#app',
                data: {
                    number:0,
                    x:0,
                    y:0,
                },
                methods:{
                    countup: function(times){
                        this.number+=1*times
                    },
                    changemouseposition: function(divideNumber,event){
                        console.log(event)
                        this.x=event.clientX/divideNumber
                        this.y=event.clientY/divideNumber
                    }
                }
            })

        </script>
    </body>
    </html>
    ```

- イベント修飾子
    - stoppropagation
        - 以下のように「クリックしないでください」のエリアだけイベント起こしたくない時は、spanタグの属性として、stopPropagation()をするメソッドを持たせる
        - 毎回stoppropagationの関数をmethodsに書くのは冗長すぎるので、vueはmousemove.stopとするだけで同じ動きを実装できる
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
            <p>{{number}}   回クリックされています</p>
            <p>X:{{x}} Y:{{y}}</p>
            <button v-on:click="countup(2)">ボタン</button>
            <p v-on:mousemove="changemouseposition(3,$event)">マウスを乗せてください
                <!-- <span v-on:mousemove="noEvent">クリックしないでください</span> -->
                <span v-on:mousemove.stop="">クリックしないでください</span>
            </p>
        </div>
        <script>
            new Vue({
                el:'#app',
                data: {
                    number:0,
                    x:0,
                    y:0,
                },
                methods:{
                    countup: function(times){
                        this.number+=1*times
                    },
                    changemouseposition: function(divideNumber,event){
                        console.log(event)
                        this.x=event.clientX/divideNumber
                        this.y=event.clientY/divideNumber
                    },
                    noEvent: function(){
                        event.stopPropagation()
                    }
                }
            })

        </script>
    </body>
    </html>
    ```

    - preventDefault
        - デフォルトの動きを止めてくれる
        - 下記の例ではaタグのクリック時の挙動をpreventで止めている。よってクリックしても飛ばない。
        - これも同じく専用のmethodsを書くのが冗長すぎるので、v-on:click.preventで同じ機能を実装できる
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
                <a v-on:click="noEvent" href="https://google.com">Google</a>
                <a v-on:click.prevent href="https://google.com">Google</a>
            </div>
            <script>
                new Vue({
                    el:'#app',
                    data: {
                    },
                    methods:{
                        noEvent: function(){
                            event.preventDefault()
                        }
                    }
                })

            </script>
        </body>
        </html>
        ```

- キー修飾子
    - キーボードに対するDOMイベントに対して使える修飾子
    - keyup→キーを押し離した時
    - 以下の例では、1つ目のinputタグでは、キーがアップした時にmyAlertが呼ばれる・
    - 2つ目のinputでは、enterキーのみに対して、キーが上がった時にmyAlertが発火するようになる。
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
            <input type="text" v-on:keyup="myAlert">
            <input type="text" v-on:keyup.enter="myAlert">
        </div>
        <script>
            new Vue({
                el:'#app',
                data: {
                },
                methods:{
                    myAlert: function(){
                        console.log(111)
                    }
                }
            })

        </script>
    </body>
    </html>
    ```

- v-onディレクティブの引数を[]を使って動的に設定する。
    - 以下のようにv-on以降のDOMイベントをdataで持っておいて、[]で動的に設定できる。
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
            <p>{{number}}</p>
            <button v-on:[event]="countUp">カウントアップ</button>
        </div>
        <script>
            new Vue({
                el:'#app',
                data: {
                    number:0,
                    event: 'click'
                },
                methods:{
                    countUp: function(){
                        this.number+=1
                    }
                }
            })

        </script>
    </body>
    </html>
    ```
- v-on の省略記法
    - v:bindは「:」で省略できたが、v-onは@で省略できる
    - これはどちらかの記法に統一した方が綺麗になる
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
            <p>{{number}}</p>
            <button @click="countUp">カウントアップ</button>
            <button @[event]="countUp">カウントアップ</button>
        </div>
        <script>
            new Vue({
                el:'#app',
                data: {
                    number:0,
                    event: 'click'
                },
                methods:{
                    countUp: function(){
                        this.number+=1
                    }
                }
            })

        </script>
    </body>
    </html>
    ```
- v-model
    - 双方向データバインディングを作成するために用いる
    - 今まではVueインスタンスの値を変える → tempalteの値が変わるのバインドはしていたが、逆はなかった
    - これの両方向のバインドをできるのがv-model
    - これで、画面の初期値でinput欄に「こんにちは」（VUeインスタンス→temmpalteへのバインディング）が表示され、
    inputに値を画面から入れると、その下の{{messagen}}の表示が変わる。
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
            <input type="text" v-model="message">
            <p>{{message}}</p>
        </div>
        <script>
            new Vue({
                el:'#app',
                data: {
                    message:"こんにちは",
                },
                methods:{
                }
            })

        </script>
    </body>
    </html>
    ```
- computedプロパティー(算出プロパティ)
    - 動的なプロパティを用いるときに使うもの
    - dataは動的なものを表現することはできない。あくまでも初期値を扱うだけ。
    - computedプロパティは関数を書くがあくまでもプロパティなので、returnで値を返す必要がある。
    - computedからdataにアクセス時もthisを使う。
    - 以下lessThanThreeを実装したもの。numberのdataプロパティを見て、動的に表示値を切り替えている。
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
            <button v-on:click="countUp">カウントアップ</button>
            <p>{{ number }}</p>
            <p>{{number > 3 ? '3より大きい':'3より小さい'}}</p>
            <p>{{ lessThanThree }}</p>
        </div>
        <script>
            new Vue({
                el:'#app',
                data: {
                    number:0
                },
                methods:{
                    countUp: function(){
                        this.number+=1
                    }
                },
                computed:{
                    lessThanThree: function(){
                        return this.number > 3 ? '3より上':'3以下'
                    }
                }
            })

        </script>
    </body>
    </html>
    ```

    - 同じような挙動はcomputedを使わなくてもmethodsでも同じことができる。じゃあmethodsとcomputedの違いはなんなのか。
        - 違いはいつ関数が実行されているか。
        - methods(lessThanThreeMethod)は以下のようにthis.numberを関数内で見ているが、numberではなくotherNumberをカウントアップしても呼ばれてしまう。
        - つまりmethodsは画面が再描画される時に毎回呼ばれてしまうものなのだ。
        - ソースが大規模になるにつれて、ちょっとでも書き換わるとmethodsが全部呼ばれてってのはめちゃくちゃ面倒。問題。毎回毎回大量のメソッドが買い変わってしまう。
        - それに対処するためにcomputedプロパティ。なので、Vueの開発者は、computedという、その関数内でもつプロパティが書き換わった時のみ実行される仕組みを作った。
        - なので、numberが変わった場合はcomputedのプロパティ（）
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
                <button v-on:click="countUp">カウントアップ</button>
                <button v-on:click="otherNumber +=1 ">別のカウントアップ</button>
                <p>{{ number }}</p>
                <p>{{ otherNumber }}</p>
                <p>{{ lessThanThree }}</p>
                <p>{{ lessThanThreeMethod() }}</p>
            </div>
            <script>
                new Vue({
                    el:'#app',
                    data: {
                        number:0,
                        otherNumber:0
                    },
                    methods:{
                        countUp: function(){
                            this.number+=1
                        },
                        lessThanThreeMethod: function(){
                            console.log("methods!")
                            return this.number > 3 ? '3より上':'3以下'
                        }
                    },
                    computed:{
                        lessThanThree: function(){
                            console.log("computed!")
                            return this.number > 3 ? '3より上':'3以下'
                        }
                    }
                })

            </script>
        </body>
        </html>
        ```

- ウォッチャ
    - 特定のデータが変わった時、に何か処理を実行したい
    - 例えばdatapウロパティ内のある変数が変わった時に処理を実行したいって時にウォッチャを使う。
    - ウォッチャとcomuptedは何が違うのか。
        - 基本的にはウォッチャは使用しない。基本的にはcomputedを使用してほしい。
        - computedでも庇いきれないところがあり、それはcomputedはあくまでもプロパティで、テンプレート内でそのcomputedのメソッドを書かないと、たとえカウントアップしてnumberが変わっても実行されない。
        - 一方ウォッチャは値が変われば絶対に呼ばれる。

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
                <button v-on:click="countUp">カウントアップ</button>
                <p>{{ number }}</p>
                <!-- <p>{{ lessThanThree }}</p> -->ここ記述消したからlessThanThreeは呼ばれない。
            </div>
            <script>
                new Vue({
                    el:'#app',
                    data: {
                        number:0,
                        otherNumber:0
                    },
                    methods:{
                        countUp: function(){
                            this.number+=1
                        },
                    },
                    computed:{
                        lessThanThree: function(){
                            console.log("computed!")
                            return this.number > 3 ? '3より上':'3以下'
                        }
                    }
                })

            </script>
        </body>
        </html>
        ```
        - ウォッチャはよく非同期の処理で使われる。
        - vueインスタンス内の非同期処理の中ではthisは使わない
        - 以下の例ではウォッチャを使って3秒後にnumberをゼロに上書きしている。ただ基本的にウォッチャではなく、computedを使うのがベストプラクティス。
        - computedでやりたいことができないってk時にウォッチャを使う。
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
                <button v-on:click="countUp">カウントアップ</button>
                <p>{{ number }}</p>
                <!-- <p>{{ lessThanThree }}</p> -->
            </div>
            <script>
                new Vue({
                    el:'#app',
                    data: {
                        number:0,
                        otherNumber:0
                    },
                    methods:{
                        countUp: function(){
                            this.number+=1
                        },
                    },
                    computed:{
                        lessThanThree: function(){
                            console.log("computed!")
                            return this.number > 3 ? '3より上':'3以下'
                        }
                    },
                    watch:{
                        number: function(){
                            console.log(11)
                            var vm = this;
                            setTimeout(function(){
                                vm.number=0

                            },3000)
                        }
                    }
                })

            </script>
        </body>
        </html>
        ```
- ()ってどこでつけるの？？二重カッコないとか@clickの時とか。
    - 二重カッコ内では
        - computedはカッコつけずに
        - methodsは必ずつける、
    - v-onとか@clickの時はカッコつけてもつけなくてもどっちでもいい！
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
            <p>{{number}}</p>
            <button v-on:click="countUp">カウントアップ</button>
            <button v-on:click="countUp()">カウントアップ</button>
            <p>{{ doubleCounterComputed }}</p>
            <p>{{ doubleCOunterMethod() }}</p>
            <!-- <p>{{ lessThanThree }}</p> -->
        </div>
        <script>
            new Vue({
                el:'#app',
                data: {
                    number:0,
                    otherNumber:0
                },
                methods:{
                    countUp: function(){
                        this.number+=1
                    },
                    doubleCOunterMethod: function(){
                        return this.number * 2
                    }
                },
                computed:{
                    doubleCounterComputed: function(){
                        return this.number * 2
                    }
                }
            })

        </script>
    </body>
    </html>
    ```
- クラスの動的制御
    - 以下のようにバインディングすることでクラスの適用を動的に切り替えることができる。
    - computed内で判別処理を書いて、テンプレートにはcomputedのプロパティだけ書くと見やすくなる。
    - class="{red:isActive, 'bg-blue':!isActive}"のようにオブジェクト内のキーがケバブケースの場合はシングルクォーテーションで囲む必要がある。
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
            <h1 :class="{red: isActive, 'bg-blue':!isActive}">hellowolrd</h1>
            <h1 :class="classObject">hello</h1>
            <button @click="isActive = ! isActive">切り替え</button>
        </div>
        <script>
            new Vue({
                el:'#app',
                data: {
                    isActive:true
                },
                methods:{
                },
                computed:{
                    classObject: function(){
                        return {
                            red: this.isActive,
                            'bg-blue': !this.isActive
                        }
                    }
                }
            })

        </script>
        <style>
            .red{
                color:red;
            }
            .bg-blue{
                background-color: blue;
            }
        </style>
    </body>
    </html>
    ```
- クラスをバインディングするとき、配列を使う
    - 
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
            <h1 :class="[color, bg]">hello</h1>
            <h1 :class="[{red: isActive}, bg]">hello</h1>
            <button @click="isActive = ! isActive">切り替え</button>
        </div>
        <script>
            new Vue({
                el:'#app',
                data: {
                    isActive:true,
                    color:'red',
                    bg:'bg-blue'
                },
                methods:{
                },
                computed:{
                    classObject: function(){
                        return {
                            red: this.isActive,
                            'bg-blue': !this.isActive
                        }
                    }
                }
            })

        </script>
        <style>
            .red{
                color:red;
            }
            .bg-blue{
                background-color: blue;
            }
        </style>
    </body>
    </html>
    ```
- スタイル属性をスタイル属性をオブジェクトを用いて動的にバインディングする。
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
        <h1 :style="{color:textColor, 'background-color':bgColor}">hello</h1>
    </div>
    <script>
        new Vue({
            el:'#app',
            data: {
                textColor:'red',
                bgColor:'blue',
            },
        })

    </script>
</body>
</html>
```

- 先ほどstyke属性に直接値を書いていたが、dataプロパティーのオブジェクトに書いて、バインドするという手法もある
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
        <h1 :style="styleObject">hello</h1>
    </div>
    <script>
        new Vue({
            el:'#app',
            data: {
                styleObject:{
                    color: 'red',
                    'background-color':'blue'
                }
            },
        })

    </script>
</body>
</html>
```
- 複数のスタイルオブジェクトを配列構文を用いて適応させる
    - 以下のようにdataプロパティに定義したオブジェクトを配列で適用できる。
｀``html
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
        <h1 :style="[styleObject,baseStyles]">hello</h1>
    </div>
    <script>
        new Vue({
            el:'#app',
            data: {
                styleObject:{
                    color: 'red',
                    'background-color':'blue'
                },
                baseStyles:{
                    fontSize:'600px'
                }
            },
        })

    </script>
</body>
</html>
```

## 21章(条件付きレンダリングとリストレンダリング)
- v-if
    ```html
    - v-if の値がtrueなら表示。falseなら非表示にできる
    <body>
        <script src="https://cdn.jsdelivr.net/npm/vue@2.7.16/dist/vue.js"></script>
        <div id="app">
            <p v-if="ok">OK</p>
            <p v-if="ng">NG</p>
        </div>
    <script>
        new Vue({
            el:'#app',
            data: {
                ok:false,
                ng:false
            },
        })
    </script>

    </body>
    </html>
    ```

    - v-elseでfalseの場合の表示を制御できる
    - v-else はv-ifやv-else-ifの後に書かないといけない
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
            <p v-if="ok">OK</p>
            <p v-else>not OK</p>
            <p v-if="ng">NG</p>
        </div>
    <script>
        new Vue({
            el:'#app',
            data: {
                ok:false,
                ng:false
            },
        })
    </script>

    </body>
    </html>
    ```

    - v-else-ifも使える。 else ifと一緒ってだけ、
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
            <p v-if="ok">OK</p>
            <p v-else-if="maybeok">maybe OK</p>
            <p v-else>not OK</p>
            <p v-if="ng">NG</p>
        </div>
    <script>
        new Vue({
            el:'#app',
            data: {
                ok:false,
                maybeok:false,
                ng:false,
            },
        })
    </script>

    </body>
    </html>
    ```

    - templateタグにまとめてv-ifを書いて表示非表示を制御することができる
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
            <button @click="ok = !ok">ボタン</button>
            <template v-if="ok">
                <p>OK</p>
                <p>OK</p>
                <p>OK</p>
                <p>OK</p>
                <p>OK</p>
            </template>
        </div>
        <script>
            new Vue({
                el:'#app',
                data: {
                    ok:true,
                    maybeok:false,
                    ng:false,
                },
            })
        </script>
    </body>
    </html>
    ```

- v-show
    - v-ifはfalseの時に要素がそもそもレンダリングされない（検証でそもそも要素がない）が、
    - v-showの場合はfalseの時レンダリングはされ、styleでdisplay:noneが設定され非表示になる
    - v-showはv-else的な構文がないのでそこは不便
    - v-showは要素自体は描画するのは先ほど書いた通り。そこからfalseならstyleにdisplaynoneを設定するので初期描画のコストが高いっていうクソなところもある
    - 逆にv-ifは要素自体をDOMから削除したり追加したりしてるのでボタンでさっき表示非表示を切り替えていたが、そのコストが高い。
    - なのでカチカチカチカチ表示非表示を切り開ける場合は「v-show」、初期表示時から条件がほぼ変わらないものは「v-if」を使うのが良い
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
            <button @click="ok = !ok">ボタン</button>
            <template>
                <p v-show="ok">show</p>
                <p v-if="ok">if</p>
            </template>
        </div>
        <script>
            new Vue({
                el:'#app',
                data: {
                    ok:true,
                    maybeok:false,
                    ng:false,
                },
            })
        </script>
    </body>
    </html>
    ```

- v-for
    - v-forでv-forをつけた要素を値の要素分生成できる
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
            <template>
                <li v-for="fruit in fruits">
                    {{ fruit }}
                </li>
            </template>
        </div>
        <script>
            new Vue({
                el:'#app',
                data: {
                    fruits:['りんご','バナナ','ブドウ']
                },
            })
        </script>
    </body>
    </html>
    ```
    - v-forは第二引数にindexも取れる
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
            <template>
                <li v-for="(fruit, index) in fruits">
                    {{ fruit }}
                    {{index}}
                </li>
                
            </template>
        </div>
        <script>
            new Vue({
                el:'#app',
                data: {
                    fruits:['りんご','バナナ','ブドウ']
                },
            })
        </script>
    </body>
    </html>
    ```

    - objectももちろんんv-forできる
    - objectのv-forでは第2引数にkeyをとる仕様になっている。
    - 第3引数はindexをとる仕様になっている
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
            <template>
                <ul>
                    <li v-for="(value, key, index) in object">
                        {{key}}:
                        {{ value}}
                        {{index}}
                    </li>
                </ul>
            </template>
        </div>
        <script>
            new Vue({
                el:'#app',
                data: {
                    fruits:['りんご','バナナ','ブドウ'],
                    object:{
                        name:'田中',
                        age:25,
                        detail:"サウナ大好き"
                    }
                },
            })
        </script>
    </body>
    </html>
    ```

    -  v-forは整数値においても適用できる
    - 基本的に in をvueでは使うがofでも大丈夫
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
            <template>
                <ul>
                    <li v-for="n in 10">
                        {{n}}
                    </li>
                </ul>
            </template>
        </div>
        <script>
            new Vue({
                el:'#app',
                data: {
                },
            })
        </script>
    </body>
    </html>
    ```

    - 最後に、v-forディレクト部を使う時は必ずキー属性を設定すること。
    - 予期しないバグが多々生まれるから。
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
            <template>
                <ul>
                    <li v-for="(value, index) in fruits" :key="value">
                        <p> {{value}}</p>
                        <input type="text">
                        <p>{{index}}</p>
                    </li>
                </ul>
            </template>
            <button @click="remove">先頭を削除</button>
        </div>
        <script>
            new Vue({
                el:'#app',
                data: {
                    fruits:['りんご','バナナ','ブドウ'],
                    object:{
                        name:'田中',
                        age:25,
                        detail:"サウナ大好き"
                    }
                },
                methods:{
                    remove:function(){
                        this.fruits.shift()
                    }
                }
            })
        </script>
    </body>
    </html>
    ```


## 22章（Vueインスタンスとその内部構造はこうなっている）
- Vueインスタンスって複数あってもいいの？？とか仮装ドム、リアクティブシステムとか、内部構造を解明していく。

- Vueインスタンスは複数作ることができる
    - できるだけ1つのVueインスタンスにまとめるのが好ましい
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
        <div id="app1">
            {{message}}
        </div>
        <div id="app2">
            {{message}}
        </div>
        <script>
            new Vue({
                el:'#app1',
                data: {
                    message:'インスタンス1'
                },
                methods:{
                    remove:function(){
                        this.fruits.shift()
                    }
                }
            })

            new Vue({
                el:'#app2',
                data:{
                    message:'インスタンス2'
                },
                methods:{

                }
            })
        </script>
    </body>
    </html>
    ```