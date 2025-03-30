## 3章（レンダリングの仕組みとその最適化）
- 親コンポーネントが際レンダリングされても、ｐropsが変更されない限りは子コンポーネントが際レンダリングされないようにする。
  - memoでコンポーネントを描こう
  - propsが変更されない限りはこのコンポーネントは際レンダリングしないですよって意味になる
  - 以下そのソース。
  ```jsx
  import { memo } from "react";
  const style = {
    width: "100%",
    height: "200px",
    backgroundColor: "khaki",
  };

  export const ChildArea = memo((props) => {
    console.log("childrenが際レンダリングされた");
    const { open } = props;

    const data = [...Array(1).keys()];
    data.forEach(() => {
      console.log("...");
    });
    return (
      <>
        {open ? (
          <div style={style}>
            <p>子コンポーネント</p>
          </div>
        ) : null}
      </>
    );
  });

  ```

  - memo化したんに、props変わってないはずなのに、子コンポーネントが際レンダリングされる
    - 原因はonClickClose。propsで子コンポーネントに渡しているが、アロー関数のは毎回新しい関数を生成しているため、ｐropsが変更されたと判断されている
    - なので子コンポーネントが更新されている。
  - App.jsx
  ```jsx
  import { useState } from "react";
  import { ChildArea } from "./ChildArea";
  import "./styles.css";

  export default function App() {
    console.log("親が際レンダリングされた");
    const [count, setCount] = useState(0);
    const [text, setText] = useState("");
    const [open, setOpen] = useState(false);

    const onChangeText = (event) => {
      setText(event.target.value);
    };
    const onClickOpen = () => {
      setOpen(!open);
    };
    const onClickClose = () => {
      setOpen(false);
    };

    return (
      <div className="App">
        <input type="text" value={text} onChange={onChangeText} />
        <br />
        <br />
        <button onClick={onClickOpen}>表示</button>
        <ChildArea open={open} onClickClose={onClickClose} />
      </div>
    );
  }

  ```

  - ChildArea.jsx
  ```jsx
  import { memo } from "react";
  const style = {
    width: "100%",
    height: "200px",
    backgroundColor: "khaki",
  };

  export const ChildArea = memo((props) => {
    console.log("childrenが際レンダリングされた");
    const { open, onClickClose } = props;

    // const data = [...Array(1).keys()];
    // data.forEach(() => {
    //   console.log("...");
    // });
    return (
      <>
        {open ? (
          <div style={style}>
            <p>子コンポーネント</p>
            <button onClick={onClickClose}>閉じる</button>
          </div>
        ) : null}
      </>
    );
  });

  ```

  - そんな時に使うのがuseCallback。
    - 同じものを使いまわしてねって指示ができる。
    - useEffectと同じで第二引数の配列が見張る値。空であれば、最初に生成したものをずっと使うという設定になる。
    - 
    ```jsx
    import { useCallback, useState } from "react";
    import { ChildArea } from "./ChildArea";
    import "./styles.css";

    export default function App() {
      console.log("親が際レンダリングされた");
      const [count, setCount] = useState(0);
      const [text, setText] = useState("");
      const [open, setOpen] = useState(false);

      const onChangeText = (event) => {
        setText(event.target.value);
      };
      const onClickOpen = () => {
        setOpen(!open);
      };
      const onClickClose = useCallback(() => {
        setOpen(false);
      }, []);

      return (
        <div className="App">
          <input type="text" value={text} onChange={onChangeText} />
          <br />
          <br />
          <button onClick={onClickOpen}>表示</button>
          <ChildArea open={open} onClickClose={onClickClose} />
        </div>
      );
    }

    ```

    - このようにコンポーネントのmemo化、そこに関数を渡す場合は関数のmemo化。この2つを組み合わせる必要がある。基本的にはこれで際レンダリングを最適化できる
    - ちなみに変数のmemo化があって、useMemoってやつでできる。変数自体のメモ化
    - 最初に読み込まれた時にだけ計算されてそれ以降は４が使われていくって感じ
    ```jsx
    const temp = useMemo(()=>{1+3, []})
    ```
    - このお用に最適化してより軽いReactを最適化する、
  

## ４章さまざまなcssの当て方
- inline style
  - それぞれのタグなのかでスタイルを書いて、その中にスタイルを記述したオブジェクトを当てて適応させるやつ
  ```jsx
    export const InlineStyle = () => {
    const containerStyle = {
      border: "solid 2px #392eff",
      borderRadius: "20px",
      padding: "8px",
      margin: "8px",
      display: "flex",
      justifyContent: "space-around",
      alignItems: "center",
    };
    const titleStyle = {
      margin: "0",
      color: "#3d84a8",
    };
    const buttonStyle = {
      backgroundColor: "#abedd8",
      border: "none",
      padding: "8px",
      borderRadius: "8px",
    };
    return (
      <div style={containerStyle}>
        <p style={titleStyle}>- Inline style -</p>
        <button style={buttonStyle}> FIGHT!! </button>
      </div>
    );
  };
  ```

- CSS Module
  - コンポーネントに対応する形でｃｓｓファイルを用意して、それを読み込んで使うやつ

  - 以下のようにcssを書いたscssファイルを作る
  - CssModules.module.scss
  ```scss
    .container {
      border: solid 2px #392eff;
      border-radius: 20px;
      padding: 8px;
      margin: 8px;
      display: flex;
      justify-content: space-around;
      alignitems: center;
    }

    .title {
      margin: 0;
      color: #3d84a8;
    }

    .button {
      background-color: #abedd8;
      border: none;
      padding: 8px;
      border-radius: 8px;
      &:hover {
        background-color: #46cdcf;
        color: #fff;
      }
    }

  ```
  - そんで使いたいコンポーネントで読み込む。以下のようにして適応する
  ```jsx
  import classes from "./CssModules.module.scss";

  export const CssModules = () => {
    return (
      <div className={classes.container}>
        <p className={classes.title}>--Css Modules</p>
        <button className={classes.button}>FIGHT</button>
      </div>
    );
  };

  ```

- Styled Jsx
  - cssをｊsx内で書くパターン。styleタグ内で、波括弧とバッククオぉーテーション内に通常のcssを書く
  ```jsx
  export const StyledJsx = () => {
    return (
      <>
        <div className="container">
          <p className="title"> styled jsx</p>
          <button className="button">fight!</button>
        </div>
        <style jsx="true">
          {`
            .container {
              border: solid 2px #392eff;
              border-radius: 20px;
              padding: 8px;
              margin: 8px;
              display: flex;
              justify-content: space-around;
              alignitems: center;
            }
            .title {
              margin:0;
              color:#3d84a8;
            }
            .button {
              background-color: #abedd8;
              border: none;
              padding: 8px;
              border-radius: 8px;
              &:hover {
                background-color: #46cdcf;
                color: #fff;
              }
          `}
        </style>
      </>
    );
  };

  ```

- styled-components
  - styleを当てた要素を作って、直接タグとして使うイメージ。
  ```jsx
  import styled from "styled-components";

  export const StyledComponents = () => {
    return (
      <Container>
        <STitle>styled components</STitle>
        <SButton>fight!</SButton>
      </Container>
    );
  };

  const Container = styled.div`
    border: solid 2px #392eff;
    border-radius: 20px;
    padding: 8px;
    margin: 8px;
    display: flex;
    justify-content: space-around;
    alignitems: center;
  `;

  const STitle = styled.p`
    margin: 0;
    color: #3d84a8;
  `;

  const SButton = styled.button`
    background-color: #abedd8;
    border: none;
    padding: 8px;
    border-radius: 8px;
    &:hover {
      background-color: #46cdcf;
      color: #fff;
    }
  `;

  ```

## 5章(ルーティングの基礎)

- 基本的に画面遷移
  - BrawserrouterDomタグで囲った配下でルーHTイングが有効化される仕組み
  - Linkタグがaタグっぽい実装をしている。(toはどのパスに飛ぶかって意味の実装)
  - Routesタグでどのパスの場合にどのコンポーネントを出すかの設定をする
  ```jsx
  import { BrowserRouter, Link, Routes, Route } from "react-router-dom";

  import { Home } from "./Home";
  import { Page1 } from "./Page1";
  import { Page2 } from "./Page2";
  import "./styles.css";

  export default function App() {
    return (
      <BrowserRouter>
        <div className="App">
          <Link to="/">Home</Link>
          <br />
          <Link to="/page1">Page1</Link>
          <br />
          <Link to="/page2">Page2</Link>
          <Home />
          <Page1 />
          <Page2 />
        </div>
        <Routes>
          <Route exact path="/" element={<Home />} />
          <Route path="/page1" element={<Page1 />} />
          <Route path="/page2" element={<Page2 />} />
        </Routes>
      </BrowserRouter>
    );
  }

  ```

- ネストされたページ遷移
  - ちなみにreact-router-dom:6.30.0を使用

  - propsの渡し方、受け取り方
    - 親側ではいつも通りタグに渡す変数を記載。
    - 受け取るこコンポーネント側ではpropsで引数として受け取り、中身を参照して出力。
    - App.js
    ```js
      import { BrowserRouter, Link, Routes, Route } from "react-router-dom";

      import { Home } from "./Home";
      import { Page1 } from "./Page1";
      import { Page1DetailA } from "./Page1DetailA";
      import { Page1DetailB } from "./Page1DetailB";
      import { Page2 } from "./Page2";
      import "./styles.css";

      export default function App() {
        const homeMessage = "Welcome to the Home Page!";
        const page1Message = "Welcome to the page1Message Page!";←ここ！！！！！！
        return (
          <BrowserRouter>
            <div className="App">
              <Link to="/">Home</Link>
              <br />
              <Link to="/page1">Page1</Link>
              <br />
              <Link to="/page2">Page2</Link>
            </div>

            <Routes>
              <Route path="/" element={<Home message={homeMessage} />} />
              <Route path="/page1/*" element={<Page1 message={page1Message} />} />←ここ！！！！！！
              <Route path="/page1/detailA" element={<Page1DetailA />} />
              <Route path="/page1/detailB" element={<Page1DetailB />} />
              <Route path="/page2/*" element={<Page2 />} />
            </Routes>
          </BrowserRouter>
        );
      }
    ```

    - Page1.jsx
    ```jsx
    import {
      BrowserRouter,
      Link,
      Routes,
      Route,
      Outlet,
      useParams,
    } from "react-router-dom";
    import { Page1DetailA } from "./Page1DetailA";
    import { Page1DetailB } from "./Page1DetailB";
    export const Page1 = (props) => {←ここ！！！！！！
      return (
        <div>
          <h1>Page1ページです</h1>
          <p>{props.message}</p>←ここ！！！！！！


          <br />
          <Link to="detailA" replace>
            detailA
          </Link>
          <br />
          <Link to="detailB" replace>
            detailB
          </Link>
        </div>
      );
    };

    ```

  - Routes以下の切り出し、整理
    - src/App.jsx
    ```jsx
    import { BrowserRouter, Link, Routes, Route } from "react-router-dom";
    import { Router } from "./router/Router";

    import "./styles.css";

    export default function App() {
      return (
        <BrowserRouter>
          <div className="App">
            <Link to="/">Home</Link>
            <br />
            <Link to="/page1">Page1</Link>
            <br />
            <Link to="/page2">Page2</Link>
          </div>
          <Router />
        </BrowserRouter>
      );
    }

    ```
    - src/router/Router.jsx
    ```jsx
    import { BrowserRouter, Link, Routes, Route } from "react-router-dom";

    import { Home } from "../Home";
    import { Page1 } from "../Page1";
    import { Page1DetailA } from "../Page1DetailA";
    import { Page1DetailB } from "../Page1DetailB";
    import { Page2 } from "../Page2";
    import { page1Routes } from "./Page1Routes";

    export const Router = () => {
      const homeMessage = "Welcome to the Home Page!";
      const page1Message = "Welcome to the page1Message Page!";
      return (
        <Routes>
          {page1Routes.map((route) => {
            return (
              <Route key={route.path} path={route.path} element={route.children} />
            );
          })}
        </Routes>
      );
    };
    ```

    - src/router/Page1Routes.jsx
    ```jsx
    import { Home } from "../Home";
    import { Page1 } from "../Page1";
    import { Page1DetailA } from "../Page1DetailA";
    import { Page1DetailB } from "../Page1DetailB";
    import { Page2 } from "../Page2";

    export const page1Routes = [
      {
        path: "",
        children: <Home />,
      },
      {
        path: "/page1",
        children: <Page1 />,
      },
      {
        path: "/page1/detailA",
        children: <Page1DetailA />,
      },
      {
        path: "/page1/detailB",
        children: <Page1DetailB />,
      },
      {
        path: "/page2",
        children: <Page2 />,
      },
    ];
    ```
  - URLパラメータを使う
    - 結論、パスに「:id」とかを入れるやつ。クエリパラメータとは違う
    - 結局はファイルごとに分割してよみこんでるだけ。１ファイルで書こうと思えば全然書ける。
    - Linkがaタグみたいな役割で、Routesがこのパスの時にこのコンポーネント表示しますよって設定するやつ
    - いかでは「https://kjkymc.csb.app/page2/100」とかにアクセス時にパラメータ渡される
    - src/router/Router.jsx
    ```jsx
    import { Routes, Route } from "react-router-dom";
    import { page1Routes } from "./Page1Routes";
    import { page2Routes } from "./Page2Routes";

    export const Router = () => {
      return (
        <Routes>
          {page1Routes.map((route) => {
            return (
              <Route key={route.path} path={route.path} element={route.children} />
            );
          })}
          {page2Routes.map((route) => {
            return (
              <Route key={route.path} path={route.path} element={route.children} />
            );
          })}
        </Routes>
      );
    };

    ```

    - src/router/Page2Routes.jsx
    ```jsx
    import { Page2 } from "../Page2";
    import { UrlParameter } from "../UrlParameter";

    export const page2Routes = [
      {
        path: "/page2",
        children: <  />,
      },
      {
        path: "/page2/:id", ←ここ！！
        children: <UrlParameter />,
      },
    ];
    ```

    - src/UrlParameters.jsx
    ```jsx
    import { useParams } from "react-router-dom";
    export const UrlParameter = () => {
      const { id } = useParams();
      return (
        <div>
          <h1>UrlParameterです</h1>
          <p>パラメータは{id}です</p>
        </div>
      );
    };
    ```

  - クエリパラメータを使う
    - クエリパラメータの扱い方を記載。
    - src/UrlParameter.jsx
    ```jsx
    import { useParams, useLocation } from "react-router-dom";
    export const UrlParameter = () => {
      const { id } = useParams();
      const { search } = useLocation(); // クエリパラメータ取得できるやつ
      const query = new URLSearchParams(search); // クエリパラメータを扱うための便利なメソッドを提供するやつ
      console.log(search);
      console.log(query);
      return (
        <div>
          <h1>UrlParameterです</h1>
          <p>パラメータは{id}です</p>
          <p>パラメータは{query.get("name")}です</p> //←これ追加！！！！！！！！！！！！！！！！！！！！
        </div>
      );
    };
    ```
  
  - state
    - コンポーネントの状態を渡したいなーってときは、タグのto属性の他、state属性をつけて
    - 受け取るときはuseLocation()を使って受け取る
    - src/Page1.jsx
    ```jsx
    import {
      BrowserRouter,
      Link,
      Routes,
      Route,
      Outlet,
      useParams,
    } from "react-router-dom";
    import { Page1DetailA } from "./Page1DetailA";
    import { Page1DetailB } from "./Page1DetailB";
    export const Page1 = (props) => {
      const arr = [...Array(100).keys()];
      return (
        <div>
          <h1>Page1ページです</h1>
          <p>{props.message}</p>
          <br />
          <Link to="detailA">detailA</Link>
          <br />
          <Link to="detailB">detailB</Link>
          <br />

          <Link to="detailA" state={arr}> ←ここ追加！！！！！
            detailA(state)
          </Link>
        </div>
      );
    };
    ```

    - src/Page1Detail.jsx
    ```jsx
    import { useLocation } from "react-router-dom";

    export const Page1DetailA = () => {
      const { state } = useLocation();←ここ追加！！！！！
      console.log(state);←ここ追加！！！！！
      return (
        <div>
          <h1>Page1DetailAです！</h1>
        </div>
      );
    };
    ```
    
  - useNavigate
    - Linｋを使わずに、ｊｓから画面遷移する場合はこれ使う。
    - 
    ```jsx
    import { BrowserRouter, Link, useNavigate } from "react-router-dom";
    export const Page1 = (props) => {
      const arr = [...Array(100).keys()];
      const navigate = useNavigate();←ここ追加！！！！！
      const onClickDetailA = () => navigate("detailA");←ここ追加！！！！！
      return (
        <div>
          <h1>Page1ページです</h1>
          <p>{props.message}</p>
          <br />
          <Link to="detailA">detailA</Link>
          <br />
          <Link to="detailB">detailB</Link>
          <br />

          <Link to="detailA" state={arr}>
            detailA(state)
          </Link>
          <br />
          <button onClick={onClickDetailA}>DetailA</button>←ここ追加！！！！！
        </div>
      );
    };

    ```


## 6章（Atomic design）
  - Atomic designとは
    - BradFrostが考案したデザインシステム
    - 画面要素を５段階に分け組み合わせることでUIを実現
    - コンポーネント化された要素が画面を構成してるという考え方
    - Atom, molecule, organism, template ,Pages
      - Atom（原子）
        - それ以上分解できない要素（ボタン、テキスト入力部分、アイコンなどなど）
      - molecule(分子)
        - Atomの組み合わせで意味を持つパーツ
        - アイコン＋メニュー名
        - プロフィール画像＋テキストボックス
      - organism(有機体)
        - AtomやMoleculeの塊で意味を持つ要素群
        - twitterのサイドバーのメニュー
        - 1つのツイートエリア
      - template(ページのレイアウトを表現する要素)
        - 実際のデータは持たない
        - サイドメニュー、センターエリア、トピックwリアなど領域のこと？ワイヤーフレームの各領域のことかな
      - page遷移ごとにほゆじされる各画面
  - Atomic design で分割していく時に大事な考え方が、このコンポーネントの役割はなんなのかということ。
    - 画面の主要となるボタンのデザインを定義して、そのボタンに表示する文言は変数で受け取って、コンポーネントを使い回すってやり方でアトム作ったりする
  - 一旦Atomであるbuttonコンポーネントを作ってみる
    - App.jsx
    ```jsx
    import { PrimaryButton } from "./components/atoms/button/PrimaryButton";
    import { SecondaryButton } from "./components/atoms/button/SecondaryButton";
    import "./styles.css";
    export default function App() {
      return (
        <div className="App">
          <PrimaryButton>テスト</PrimaryButton>
          <SecondaryButton>検索</SecondaryButton>
        </div>
      );
    }
    ```

    - PrimaryButton.jsx
    ```jsx
    import styled from "styled-components";
    import { BaseButton } from "./BaseButton";
    export const PrimaryButton = (props) => {
      const { children } = props;
      return <SButton>{children}</SButton>;
    };
    const SButton = styled.button`
      background-color: #40514e;
      color: #fff;
      padding: 6px 24px;
      border: none;
      outline: none;
      &:hover {
        cursor: hover;
        opacity: 0.8;
      }
      border-radius: 9999px;
    `;
    ```

    - SecondaryButton
    ```jsx
    import styled from "styled-components";
    import { BaseButton } from "./BaseButton";
    export const SecondaryButton = (props) => {
      const { children } = props;
      return <SButton>{children}</SButton>;
    };
    const SButton = styled.button`
      background-color: #11999e;
      color: #fff;
      padding: 6px 24px;
      border: none;
      outline: none;
      &:hover {
        cursor: hover;
        opacity: 0.8;
      }
      border-radius: 9999px;
    `;
    ```

    - 上記で一旦入りの異なる2つのボタンコンポーネントができた。
    - だがそれぞれのボタンにおいて、backgroundcolor以外のCSSは同じ。
    - こいうときは2つのボタンにおいて共通のcssを１箇所に共通のものとして持たせるのが保守性が高くなる
    - よって以下のコンポーネントを作る
    - BaseButton.jsx
    ```jsx
    import styled from "styled-components";
    export const BaseButton = styled.button`
      background-color: #40514e;
      color: #fff;
      padding: 6px 24px;
      border: none;
      outline: none;
      &:hover {
        cursor: hover;
        opacity: 0.8;
      }
      border-radius: 9999px;
    `;
    ```

    - じょうきのbaseを２ボタンそれぞれで取り込む
    - PrimaryButton.jsx
    ```jsx
    import styled from "styled-components";
    import { BaseButton } from "./BaseButton";
    export const PrimaryButton = (props) => {
      const { children } = props;
      return <SButton>{children}</SButton>;
    };
    const SButton = styled(BaseButton)`
      background-color: #40514e;
    `;
    ```
  - moleculeを作ってみる(完全な部品とまではいかないがAtomを組み合わせたもの)
    - 今回はinput要素＋検索ボタンのAtom組み合わせ
    - src/components/atoms/molecule/SearchInput.jsx
    ```jsx
    import { PrimaryButton } from "../atoms/button/PrimaryButton";
    import styled from "styled-components";

    import { Inputt } from "../atoms/input/Inputt";
    export const SearchInput = () => {
      return (
        <SContainer>
          <Inputt placeholder="検索条件を入力してください" />
          <SButtonWrapper>
            <PrimaryButton>検索</PrimaryButton>
          </SButtonWrapper>
        </SContainer>
      );
    };
    const SContainer = styled.div`
      display: flex;
      align-items: center;
    `;
    const SButtonWrapper = styled.div`
      padding-left: 8px;
    `;
    ```

    - src/components/atoms/input/Input.jsx
    ```jsx
    import styled from "styled-components";
    export const Inputt = (props) => {
      const { placeholder } = props; //← 親から変数的にもらい動的に生成
      return <SInput type="text" placeholder={placeholder} />;
    };
    const SInput = styled.input`
      padding: 8px 16px;
      border: solid #ddd 1px;
      border-radius: 9999px;
      outline: none;
    `;
    ```

  - Organism
    - 