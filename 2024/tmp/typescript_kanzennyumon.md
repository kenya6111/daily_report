- 動的型付け言語は変数宣言時に型を宣言せずに、値を格納し実行時に値が決定されるもの。
- 実行時はTypescriptをjavascriptに変換して（コンパイルして）実行している
- jsの拡張言語なのでtsファイルにｊｓをそのままかける

- 「tsc index.ts」というコマンドでtsファイルをjsファイルにコンパイルできる（jsファイルが作成される）

- typescriptをjavascriptにコンパイルする際に、設定ファイルを記述しておくことで。どのようにコンパイルするか設定しておくことができる
    - 「tsc --init」コマンド実行で、「tsconfig.json」が生成される
    - rootDir→「typescriptのファイルがある場所を指し示すやつ」
    - outDir →「コンパイルしたjsを格納する場所を指し示す設定」

    - さっきは「tsc index.ts」でコンパイルしたが
    - 単に「tsc」とだけでファイル指定なしだと全てのｔｓがコンパイルされる

- 以下のような型になる
```ts
let num: number = 10;
let nickname: string = "kenya";
let age: number = 29;
let isActive: boolean = true;
```
- 値で型がわかる場合は型を省略もできる(マウスを変数にホバーすると、一応型がみれる)
```ts
let num = 10;
let nickname = "kenya";
let age = 29;
let isActive = true;
```

- anyという型
    - どんな値も受け入れる型
    - 最初に代入した値を別の値にて上書きすることが可能
    ```ts
    let something: any="aa"
    something=123
    ```
    - anyはtypescriptでの型定義を無視する行為でtypescriptの恩恵を受けられないので、極力使用しないようにしましょう

- 配列の型
    - 以下のように要素の型とスクエアブラケットを書く
    ```ts
    let fruits : string[] =["aaa","bbb","ccc"]
    ```

- 関数での型
    - 以下のような感じで宣言する
    ```ts
    const add =(a: number, b:number)=>{
        return a+b;
    }
    ```

    - 値を返す場合はブラケットの後に返却する型を宣言する（値を返却する際は明示的に返り血の方を描くのが良いとされている）
    ```ts
    const add =(a: number, b:number): number =>{
        return a+b;
    }
    ```

    - 値を返さない場合はブラケットの後にvoidを記述する(ただ、voidの場合は省略することが多い)
    ```ts
    const outputString = (valval:number):void =>{
        console.log(valval)
    }
    ```
    ```ts
    const outputString = (valval:number):void =>{
        console.log(valval)
    }
    ```
- オブジェクトの場合の型
    - オブジェクトのプロパティの型をuserの横っちょに並べる。
    - メソッドは型というか返り値を描く。今回はreturnしないのでvoid
    ```ts
    let user:{name:string, age:number, sayHello:()=>void} ={
        name:"tarou",
        age:25,
        sayHello:()=>{
            console.log(1234)
        }
    }
    ```

- 型エイリアス
    - Typescriptでは型に名前をつけることができる
    - 型エイリアスを作ることによって同じオブジェクトを作る際に繰り返し同じ方を宣言する必要がなくなるし、その型宣言を別ファイルに切り出すこともできる
    - type (エイリアス名(パスカルケース)) = 型
    ```ts
    type User = {name:string, age:number, sayHello:()=>void}
    let user:User={
        name:"tarou",
        age:25,
        sayHello:()=>{
            console.log(1234)
        }
    }
    ```

- ユニオン型
    - 特定の変数や引数に複数の型が入るときはユニオン型が使われる
    - 例えば以下の関数でvalに文字列型と数値型が入ることが想定される場合は（パイプラインでstringとnumberを書く
    ```ts
    const outputNumStr = (val)=>{
    }
    ```

    - パイプラインで型をつなぎ、複数の型を受け取ることができる
    ```ts
    const outputNumStr = (val: string | number)=>{
        console.log(val)
    }
    ```

- リテラル型
    - 上記のユニオン型と同じパイプラインを使って
    - 特定の値だけを代入可能にできるってやつ
    - 以下の例では10か20だけ代入できる
    ```ts
    let num1: 10 | 20 = 10
    ```

- インターセクション型
    - 2つの型を結合したい場合
    - このように宣言することで2つの型エイリアスを結合したオブジェクトを生成できる
    ```ts
    type Base ={
        id:number;
        createdAt:Date;
        updatedAt:Date;
    }

    type Product ={
        name:string;
        price:number;
    }

    let product2: Base & Product = {
        id:1,
        name:'tanaka',
        price:12,
        createdAt: new Date(),
        updatedAt: new Date()
    }
    ```

- オプショナルプロパティ
    - オブジェクトのプロパティを任意のプロパティ（そのプロパティが存在しなくてもいいもの）と宣言できる
    - プロパティ名の後方に「？」をつけることで宣言できる
    - 以下の例ではurlをオプショナルプロパティに設定している
    ```ts
    type Meeting ={
        startAt: Date;
        kind: "office" | "tel" | "web";
        url?: string
    }

    const meeting1:Meeting={
        startAt:new Date(),
        kind: "office"
    }
    const meeting2:Meeting={
        startAt:new Date(),
        kind: "web",
        url:"https://sample.com"
    }
    ```
    - オプショナルプロパティにはundefined はセットできるが、nullはセットできない
        - nullをセットしたい場合はオプショナルプロパティの型エイリアスに パイプラインでnullを追加しておく
        ```ts
        type Meeting ={
            startAt: Date;
            kind: "office" | "tel" | "web";
            url?: string | null
        }
        ```
        - このようなオプショナルプロパティや値がnullの場合に、そのプロパティからメソッドを呼び出そうとするとエラーになる
            - 以下のようにオプショナルのプロパティを呼び出そうとしてもnullの場合があるので、以下はエラーと判定される
            ```ts
            console.log(meeting3.url.length)
            ```
            - そのため、オプショナルプロパティがnull出ないか、undefinedでないかを確認する必要がある
                ```ts
                if(meeting3.url !== null && meeting3.url !== undefined){
                    console.log(meeting3.url.length)
                }
                ```
                - 上記の記述はオプショナルチェーンで簡略化できる
                    - その値が存在しない可能せがある場合に？をつけてメソッドを呼び出すことでエラーを出さずにundefinedが返却される
                    - urlの後ろに?をつける
                    - urlがある場合は文字列の長さが返却されるが
                    - urlがない場合はundefinedが返却される
                    ```ts
                        console.log(meeting3.url?.length)
                    ```

- 型アサーション
    - エディター上で値から自動で型推論してくれるが、値がどの型になるか推論で機ニア場合がある
    - document.getElementById()なんかは、取得する要素がdivタグかとかhtmlタグかとかを実行するまで判定できない
    - そういった場合に型推論を上書きすることができる。これを型アサーションという

    - 代入する値の後に as とかいて、その後ろに型を宣言する。
    - これはinputタグの方になるので、valueはinputタグが持つプロパティなのでえらーがでずよびだせる
    - 型アサーションを外すとエラーとなる


- ジェネリクス
    - 抽象的な型数を定義して実際に利用されるまで型が確定しない関数を実現するためののもの
    - 文字列と数値をそのまま返す関数がそれぞれある。内部的な処理は同じだが引数と返却型が異なるので関数を2つ定義する必要がある
    - この場合にremainValueのように関数を定義し、メソッド名 <型> ()のように呼び出してやることができるｓ
    ```ts
    const remaiString =(val: string): string=>{
        return val;
    }
    const remaiNum =(val: number): number=>{
        return val;
    }


    const remainValue =<T>(val:T): T =>{
        return val;
    }
    remainValue<string>("aaa");
    remainValue<number>(11);
    ```


    - 一応、「T」のジェネリクス部分は別にTじゃなくてもいいのだが（SAMPLEとかでもいい）、慣習的にTとすること
    ```ts
    const remainValue2 =<SAMPLE>(val:SAMPLE): SAMPLE =>{
        return val;
    }
    ```

- キーオブ
    - キーオブは、オブジェクトの型からプロパティ名を型として返却する演算子
    - 例えば以下のようなメソッドがあったとして、keyには3つのキーを書いているが、ここに書くのは結局、OneDayMealsのキーだよね？
    ```ts
    type OneDayMeals = {
        morning:string,
        lunch:string,
        dinner:string
    };
    const meall:OneDayMeals = {
        morning:"フレンチトースト",
        lunch:"焼きそば",
        dinner:"とんかつ",
    };

    const outputValueLength =(key: "morning" | "lunch" | "dinner")=>{
        console.log(meall[key].length)
    }
    ```

    - なので、以下のようにkeyofを使ってかける
    ```ts
    const outputValueLength =(key: keyof OneDayMeals)=>{
        console.log(meall[key].length)
    }
    ```