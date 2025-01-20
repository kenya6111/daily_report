    ## クイックスタート
- reactはコンポーネントで構成されている
- `MyButton` `MyButton2` を宣言したら、別のコンポーネントにネストできます。
- <MyButton /> が大文字で始まっていることに注意してください。こうすることで、React のコンポーネントであるということを示しています。React のコンポーネント名は常に大文字で始める必要があり、HTML タグは小文字でなければなりません。
- コンポーネントは複数の JSX タグを return することはできません。`<div>...</div>` や空の `<>...</>` ラッパのような共通の親要素で囲む必要があります。
```html
<!DOCTYPE html>
<html>
  <head>
    <meta charset="UTF-8" />
    <title>Hello World</title>
    <script src="https://unpkg.com/react@18/umd/react.development.js"></script>
    <script src="https://unpkg.com/react-dom@18/umd/react-dom.development.js"></script>

    <!-- Don't use this in production: -->
    <script src="https://unpkg.com/@babel/standalone/babel.min.js"></script>
</head>
<body>
    <div id="root"></div>
    <script type="text/babel">
    function MyButton() {
        return (
            <button>I'm a button</button>
        );
    }
    function MyButton2() {
        return (
            <button>I'm a button2!!</button>
        );
    }
    function MyApp() {
            return (
                <div>
                    <h1>Hello, world!</h1>
                    <MyButton />
                    <MyButton2 />
                </div>
        );
    }

    const container = document.getElementById('root');
    const root = ReactDOM.createRoot(container);
    root.render(<MyApp />);

    </script>
</body>
</html>
```

![alt text](../../image/image.png)

- 下記の例では、`style={{}}` は特別な構文ではなく、`style={ }` という JSX の波括弧内にある通常の `{} オブジェクト`です。スタイルが JavaScript 変数に依存する場合は、style 属性を使うことができます。

```html
<!DOCTYPE html>
<html>
  <head>
    <meta charset="UTF-8" />
    <title>Hello World</title>
    <script src="https://unpkg.com/react@18/umd/react.development.js"></script>
    <script src="https://unpkg.com/react-dom@18/umd/react-dom.development.js"></script>

    <!-- Don't use this in production: -->
    <script src="https://unpkg.com/@babel/standalone/babel.min.js"></script>
</head>
<body>
    <div id="root"></div>
    <script type="text/babel">
    const user = {
        name: 'Hedy Lamarr',
        imageUrl: '/image.png',
        imageSize: 90,
    };
    function MyButton() {
        return (
            <button>I'm a button</button>
        );
    }
    function MyButton2() {
        return (
            <button>I'm a button2!!</button>
        );
    }
    function MyApp() {
            return (
                <div>
                    <h1>Hello, world!</h1>
                    <MyButton />
                    <MyButton2 />
                    <h1>{user.name}</h1>
                    <img
                        className="avatar"
                        src={user.imageUrl}
                        alt={'Photo of ' + user.name}
                        style={{
                            width: user.imageSize,
                            height: user.imageSize
                        }}
                    />
                </div>
        );
    }

    const container = document.getElementById('root');
    const root = ReactDOM.createRoot(container);
    root.render(<MyApp />);

    </script>
</body>
</html>
```


- 条件付きレンダー
    - React には、条件分岐を書くための特別な構文は存在しません。代わりに、通常の JavaScript コードを書くときに使うのと同じ手法を使います
```html

    <div id="root"></div>
    <script type="text/babel">
        function MyButton() {
            return (
                <button>I'm a button</button>
            );
        }
        function MyButton2() {
            return (
                <button>I'm a button2!!</button>
            );
        }
        let content;
        let isLoggedIn =false
        if (isLoggedIn) {
            content = <MyButton />;
        } else {
            content = <MyButton2 />;
        }
    function MyApp() {
            return (
                <div>
                    <h1>Hello, world!</h1>
                    {content}

                    // コンパクトなコードをお望みの場合は、条件 ? 演算子を使用できます
                    {isLoggedIn ? (
                        <MyButton />
                    ) : (
                        <MyButton2 />
                    )}

                    // else 側の分岐が不要な場合は、短い論理 && 構文を使用することもできる
                    {isLoggedIn && <MyButton2 />}
                </div>
        );
    }
    const container = document.getElementById('root');
    const root = ReactDOM.createRoot(container);
    root.render(<MyApp />);
    </script>
```

- リストのレンダー
    - コンポーネントのリストをレンダーする場合は、for ループ や 配列の map() 関数 といった JavaScript の機能を使って行います
    - `<li>` に `key` 属性があることに注意してください。リスト内の各項目には、兄弟の中でそれを一意に識別するための文字列または数値を渡す必要があります。通常、`key` はデータから来るはずで、データベース上の ID などが該当します。React は、後でアイテムを挿入、削除、並べ替えることがあった際に、何が起こったかを key を使って把握します。

```html
    <div id="root"></div>
    <script type="text/babel">
        const products = [
            { title: 'Cabbage', isFruit: false, id: 1 },
            { title: 'Garlic', isFruit: false, id: 2 },
            { title: 'Apple', isFruit: true, id: 3 },
        ];

        const listItems = products.map(product =>
            <li
            key={product.id}
            style={{
                color: product.isFruit ? 'magenta' : 'darkgreen'
            }}
            >
            {product.title}
            </li>
        );
    function MyApp() {
        return (
            <ul>{listItems}</ul>
        );
    }
    const container = document.getElementById('root');
    const root = ReactDOM.createRoot(container);
    root.render(<MyApp />);
    </script>
```

![alt text](../../image/image2.png)

- イベントに応答する
    - コンポーネントの中でイベントハンドラ関数を宣言することで、イベントに応答できます
    - onClick={handleClick} の末尾に括弧がないことに注意してください！ そこでイベントハンドラ関数を呼び出すわけではありません。渡すだけです。ユーザがボタンをクリックしたときに、React がイベントハンドラを呼び出します。
```html
    <div id="root"></div>
    <script type="text/babel">
        function handleClick() {
            alert('You clicked me!');
        }
    function MyApp() {
        return (
            <button onClick={handleClick}>
            Click me
            </button>
        );
    }
    const container = document.getElementById('root');
    const root = ReactDOM.createRoot(container);
    root.render(<MyApp />);
    </script>
```

- 画面更新
    - しばしば、コンポーネントに情報を「記憶」させて表示したいことがあります。例えば、ボタンがクリックされた回数を数えて覚えておきたい場合です。これを行うには、コンポーネントに state を追加します。

    - まず、React から useState をインポートします。

    ```html
    import { useState } from 'react';
    ```

    - これでコンポーネント内にstate変数を設定できる
        - useState からは 2 つのものが得られます。現在の state (count) と、それを更新するための関数 (setCount) です。名前は何でも構いませんが、慣習的には [something, setSomething] のように記述します。
        - ボタンが初めて表示されるとき、count は 0 になります。これは useState() に 0 を渡したからです。state を変更したいときは、setCount() を呼び出し、新しい値を渡します。このボタンをクリックすると、カウンタが増加します
        - 各ボタンがそれぞれ count という state を「記憶」し、他のボタンに影響を与えないことに注意してください。
        - use で始まる関数は、フック (Hook) と呼ばれます。useState は React が提供する組み込みのフックです

    ```html
    function MyButton() {
        const [count, setCount] = useState(0);
    // ...
    }
    ```


    - 以下、上記の実装例
    ```html
    <!DOCTYPE html>
    <html>
    <head>
        <meta charset="UTF-8" />
        <title>Hello World</title>
        <script src="https://unpkg.com/react@18/umd/react.development.js"></script>
        <script src="https://unpkg.com/react-dom@18/umd/react-dom.development.js"></script>

        <!-- Don't use this in production: -->
        <script src="https://unpkg.com/@babel/standalone/babel.min.js"></script>
        </head>
        <body>
            <div id="root"></div>
            <script type="text/babel">

            function MyButton() {
                const [count, setCount] = React.useState(0)

                function handleClick() {
                    setCount(count + 1)
                }

                return (
                    <button onClick={handleClick}>
                        clicked {count} times
                    </button>
                );
            }

            function MyApp() {
                return (
                    <div>
                        <h1>Counters that update separately</h1>
                        <MyButton />
                        <MyButton />
                    </div>
                );
            }
            const container = document.getElementById('root');
            const root = ReactDOM.createRoot(container);
            root.render(<MyApp />);
            </script>
        </body>
        </html>
    ```
    ![alt text](../../image/image3.png)

    - 以下は`useState`を2ボタン共通で管理するように修正したもの。どちらのボタンを押しても、両方のボタンのカウントがカウントアップされる。
        - `MyApp` から各 `MyButton` に `state` を渡し、共有のクリックハンドラも一緒に渡します。以前に `<img>` のような組み込みタグで行ったときと同様、JSX の波括弧を使うことで `MyButton` に情報を渡すことができます。
        - このように渡される情報は `props` と呼ばれます。`MyApp` コンポーネントは `count` 状態と `handleClick` イベントハンドラを保持しており、それらをどちらも `props` として各ボタンに渡します。
        - ボタンをクリックすると、onClick ハンドラが発火します。各ボタンの onClick プロパティは MyApp 内の handleClick 関数となっているので、その中のコードが実行されます。そのコードは setCount(count + 1) を呼び出し、count という state 変数をインクリメントします。新しい count の値が各ボタンに props として渡されるため、すべてのボタンに新しい値が表示されます。この手法は「state のリフトアップ（持ち上げ）」と呼ばれています。リフトアップすることで、state をコンポーネント間で共有できました
    ```html
    <!DOCTYPE html>
    <html>
    <head>
        <meta charset="UTF-8" />
        <title>Hello World</title>
        <script src="https://unpkg.com/react@18/umd/react.development.js"></script>
        <script src="https://unpkg.com/react-dom@18/umd/react-dom.development.js"></script>

        <!-- Don't use this in production: -->
        <script src="https://unpkg.com/@babel/standalone/babel.min.js"></script>
    </head>
    <body>
        <div id="root"></div>
        <script type="text/babel">

        function MyButton({count,onClick }) {
            return (
                <button onClick={onClick}>
                    clicked {count} times
                </button>
            );
        }

        function MyApp() {
            const [count, setCount] = React.useState(0);

            function handleClick() {
                setCount(count + 1);
            }
            return (
                <div>
                    <h1>Counters that update separately</h1>
                    <MyButton count={count} onClick={handleClick}/>
                    <MyButton count={count} onClick={handleClick}/>
                </div>
            );
        }
        const container = document.getElementById('root');
        const root = ReactDOM.createRoot(container);
        root.render(<MyApp />);
        </script>
    </body>
    </html>
    ```
    ![alt text](../../image/image4.png)

## チュートリアル : 三目並べ
