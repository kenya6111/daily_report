## 3章（レンダリングの仕組みとその最適化）
- 再レンダリングが走るタイミング
  - stateが更新されたコンポー年とは際レンダリング
  - ｐropsが変更されたコンポーネントは際レンダリング
  - 際レンダリングされたコンポーネントの子コンポーネントは際レンダリング
- stateかわる→コンポーネントの再レンダリングが走る→その時に変更差分を検知して→画面に反映

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
    - 第二引数が変わった場合のみ、第一引数の関数を再生成するって指示ができる。
    - useEffectと同じで第二引数の配列が見張る値。空であれば、最初に生成したものをずっと使うという設定になる。
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
  - exactは、完全一致にしますよって感じのやつ。exactがないと、/配下のパスは全てｈomeのコンポーネントが表示されるようになってしまう
  ```jsx
  import { Home } from "./Home";
  import { Page1 } from "./Page1";
  import { Page2 } from "./Page2";
  import "./styles.css";
  import { BrowserRouter, Link, Switch } from "react-router-dom";
  import { Route } from "react-router-dom/cjs/react-router-dom.min";

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
        <Switch>
          <Route exact path="/">
            <Home />
          </Route>
          <Route path="/page1">
            <Page1 />
          </Route>
          <Route path="/page2">
            <Page2 />
          </Route>
        </Switch>
      </BrowserRouter>
    );
  }
  ```

- ネストされたページ遷移
    - App.js
    ```js
      import { Home } from "./Home";
      import { Page1 } from "./Page1";
      import { Page2 } from "./Page2";
      import "./styles.css";
      import { BrowserRouter, Link, Switch, Route } from "react-router-dom";
      import { Page1DetailA } from "./Page1DetailA";
      import { Page1DetailB } from "./Page1DetailB";

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
            <Switch>
              <Route exact path="/">
                <Home />
              </Route>
              <Route
                path="/page1"
                render={({ match: { url } }) => (
                  <Switch>
                    {console.log(url)}
                    <Route exact path={url}>
                      <Page1 />
                    </Route>
                    <Route exact path={`/${url}/detailA`}>
                      <Page1DetailA />
                    </Route>
                    <Route exact path={`/${url}/detailB`}>
                      <Page1DetailB />
                    </Route>
                  </Switch>
                )}
              />
              <Route path="/page2">
                <Page2 />
              </Route>
            </Switch>
          </BrowserRouter>
        );
      }

    ```
    - Page1.jsx
    ```jsx
    import { BrowserRouter, Link, Switch } from "react-router-dom";
    export const Page1 = () => {
      return (
        <div>
          <h1>Page1desu---</h1>
          <Link to="/page1/detailA">DetailA</Link>
          <br />
          <Link to="/page1/detailB">DetailB</Link>
        </div>
      );
    };
    ```

    - Page1DetailA.jsx
    ```jsx
    export const Page1DetailA = () => {
      return (
        <div>
          <h1>Page1DetailAdesu---</h1>
        </div>
      );
    };
    ```

    - Page1DetailB.jsx
    ```jsx
    export const Page1DetailB = () => {
      return (
        <div>
          <h1>Page1DetailBdesu---</h1>
        </div>
      );
    };
    ```

- URLパラメータ
  Page2のURLparameterをクリック時にパラメータを表示する

  - App.js
  ```jsx
  import "./styles.css";
  import { BrowserRouter, Link } from "react-router-dom";
  import { Router } from "./router/Router";

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

  - Router.jsx
  ```jsx
  import { Home } from "../Home";
  import { Switch, Route } from "react-router-dom";
  import { page1Routes } from "./Page1Routes";
  import { page2Routes } from "./Page2Routes";
  export const Router = () => {
    return (
      <Switch>
        <Route exact path="/">
          <Home />
        </Route>
        <Route
          path="/page1"
          render={({ match: { url } }) => (
            <Switch>
              {page1Routes.map((route) => (
                <Route
                  key={route.path}
                  exact={route.exact}
                  path={`${url}${route.path}`}
                >
                  {route.children}
                </Route>
              ))}
            </Switch>
          )}
        />
        <Route
          path="/page2"
          render={({ match: { url } }) => (
            <Switch>
              {page2Routes.map((route) => (
                <Route
                  key={route.path}
                  exact={route.exact}
                  path={`${url}${route.path}`}
                >
                  {route.children}
                </Route>
              ))}
            </Switch>
          )}
        />
      </Switch>
    );
  };
  ```
  - Page2Routes.jsx
  ```jsx
  import { Page2 } from "../Page2";
  import { URLParameter } from "../URLParameter";

  export const page2Routes = [
    {
      path: "/",
      exact: true,
      children: <Page2 />,
    },
    {
      path: "/:id",
      exact: false,
      children: <URLParameter />,
    },
  ];
  ```


  - Page2.jsx
  ```jsx
  import { Link } from "react-router-dom";
  export const Page2 = () => {
    return (
      <div>
        <h1>Page2desu---</h1>
        <Link to="page2/100">URL parameter</Link>
      </div>
    );
  };
  ```


- クエリパラメータ
  - リーティング自体に特別な設定は必要ない

  - Page2.jsx
  ```jsx
  import { Link } from "react-router-dom";
  export const Page2 = () => {
    return (
      <div>
        <h1>Page2desu---</h1>
        <Link to="page2/100">URL parameter</Link>
        <br />
        <Link to="page2/100?name=hogehoge">Query parameter</Link>
      </div>
    );
  };
  ```

  - UrlParameter.jsx
  ```jsx
  import { useParams, useLocation } from "react-router-dom";
  export const URLParameter = () => {
    const { id } = useParams();
    const location = useLocation();
    console.log(location);
    return (
      <div>
        <h1>URLparameterpagedesu!</h1>
        <p>パラメータ{id}</p>
      </div>
    );
  };
  ```

  - 上記のような感じで書くとlocationのsearch にクエリパラメータが入ってくる
  ```txt
  {pathname: '/page2/100', search: '?name=hogehoge', hash: '', state: undefined}
  hash: ""
  pathname: "/page2/100"
  search: "?name=hogehoge"
  state: undefined
  [[Prototype]]: Object
  ```


  - 最初から以下のように展開して代入すれば楽
  ```jsx
  import { useParams, useLocation } from "react-router-dom";
  export const URLParameter = () => {
    const { id } = useParams();
    const { search } = useLocation();
    console.log(search);
    return (
      <div>
        <h1>URLparameterpagedesu!</h1>
        <p>パラメータ{id}</p>
      </div>
    );
  };

  ```

  - URLparameter.jsx
    - react-router-domから、useParamsを使って受け取ることができる
  ```jsx
  import { useParams } from "react-router-dom";
  export const URLParameter = () => {
    const { id } = useParams();
    return (
      <div>
        <h1>URLparameterpagedesu!</h1>
        <p>パラメータ{id}</p>
      </div>
    );
  };
  ```


  - URLParameter.jsx
    - こんな感じでクエリパラメータを取り出せる
  ```jsx
  import { useParams, useLocation } from "react-router-dom";
  export const URLParameter = () => {
    const { id } = useParams();
    const { search } = useLocation();
    const query = new URLSearchParams(search);
    console.log(query);
    return (
      <div>
        <h1>URLparameterpagedesu!</h1>
        <p>パラメータ{id}</p>
        <p>クエリー{query.get("name")}</p>
      </div>
    );
  };

  ```


- stateを渡すページ遷移
  - page1で取得した情報をpage1Detailに渡すという感じで行く
  - Linkタグ（aタグ的なやつ）のto属性にオブジェクトでいろいろ設定値を含んだ値を設定できる
  - Page1.jsx
  ```jsx
  import { BrowserRouter, Link, Switch } from "react-router-dom";
  export const Page1 = () => {
    const arr = [...Array(100).keys()];
    console.log(arr);
    return (
      <div>
        <h1>Page1desu---</h1>
        <Link to={{ pathname: "/page1/detailA", state: arr }}>DetailA</Link>
        <br />
        <Link to="/page1/detailB">DetailB</Link>
      </div>
    );
  };
  ```

  - stateで渡ってきた情報を受け取るときは、useLocationで受け取ることができる
  - Page1DetailA
  ```jsx
  import { useLocation } from "react-router-dom";
  export const Page1DetailA = () => {
    const { state } = useLocation();
    console.log(state);
    return (
      <div>
        <h1>Page1DetailAdesu---</h1>
      </div>
    );
  };
  ```

- Linkを使わないページ遷移
  - button押した時にDetailAに遷移する方法を解説
  - useHistoryというフックを使えば、ボタン押下時に画面遷移！とかが実現できる
  - useHistoryから値を取り出してpushするだけ
  - Page1.jsx
  ```jsx
  import { BrowserRouter, Link, Switch, useHistory } from "react-router-dom";
  export const Page1 = () => {
    const arr = [...Array(100).keys()];
    console.log(arr);
    const history = useHistory();
    const onClickDetailA = () => {
      history.push("/page1/detailA");
    };
    return (
      <div>
        <h1>Page1desu---</h1>
        <Link to={{ pathname: "/page1/detailA", state: arr }}>DetailA</Link>
        <br />
        <Link to="/page1/detailB">DetailB</Link>
        <br />
        <button onClick={onClickDetailA}>DetailA</button>
      </div>
    );
  };
  ```

  - なんでhistoryって名前なん？？
  ```txt
  ✅ なぜ「history」という名前なの？
  これは、React Router が HTML5の History API（window.history）をラップして使ってるからなんです。

  🔍 そもそも History API って何？
  ブラウザが持ってる機能で、例えば以下のようなことができます：
  window.history.pushState(...) → URLを変える（けどページはリロードしない）
  window.history.back() → 前のページに戻る
  window.history.forward() → 次のページへ

  つまり、「ユーザーの移動履歴（＝History）を管理する仕組み」のこと！
  React Router はこれをベースにルーティングしてるから、useHistory っていう名前になってるわけ。

  💡 React Router v6 以降では…
  ちなみに、React Router v6 からは useHistory じゃなくて useNavigate っていう名前に変わりました！

  理由はまさに君の疑問と同じで、「historyって名前、ちょっと意味わかりづらくね？」って声が多かったから！

  jsx
  // React Router v6
  import { useNavigate } from "react-router-dom";
  const navigate = useNavigate();
  navigate("/page1/detailA");
  ```
## 6章（Atomic design）
  - Atomic designとは
    - BradFrostが考案したデザインシステム
    - 画面要素を５段階に分け組み合わせることでUIを実現
    - コンポーネント化された要素が画面を構成してるという考え方
    - Atom, molecule, organism, template ,Pages
      - Atom（原子）水素とかの原子記号
        - それ以上分解できない要素（ボタン、テキスト入力部分、アイコンなどなど）
      - molecule(分子)h2o
        - Atomの組み合わせで意味を持つパーツ
        - アイコン＋メニュー名
        - プロフィール画像＋テキストボックス
      - organism(有機体)
        - AtomやMoleculeの塊で意味を持つ要素群
        - twitterのサイドバーのメニュー欄
        - 1つのツイートエリア
        - 1つのツイート
      - template(ページのレイアウトを表現する要素)
        - 実際のデータは持たない 
        - サイドメニュー、センターエリア、トピックwリアなど領域のこと？ワイヤーフレームの各領域のことかな
      - page
        - 最終的に表示される１画面
        - 遷移ごとにほゆじされる各画面
  - Atomic design で分割していく時に大事な考え方が、このコンポーネントの役割はなんなのかということ。
    - 画面の主要となるボタンのデザインを定義して、そのボタンに表示する文言(ラベル)は変数で受け取って、コンポーネントを使い回すってやり方でアトム作ったりする
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
    - ユーザーのプロフィールカードくらいのまとまり感。有機体。
    - organismsのフォルダは配下にはどの属性の有機体かのフォルダを作っていく


## 7章
- グローバルばstate
  - どのコンポーネントやページからでも参照でき、どのページからでも更新できる
  - useStateはそのコンポーネント内で使えてpropsで渡すなどしているが。
  



## 8章
JSON Placeholder

- 「https://jsonplaceholder.typicode.com/todos/1」のようにJSONを返すURLを公開してくれていて、
- とりあえずAPI読んでデータ取れるか確認したいって時に使う 

- 以下のような感じでaxiosを使ってデータを撮ってこれる
- このようにわざわざバックエンドのサーバ立てずとも、データの取得を試せる
```jsx
import "./styles.css";
import axios from "axios";
export default function App() {
  const onClickusers = () => {
    axios
      .get("https://jsonplaceholder.typicode.com/todos")
      .then((res) => {
        console.log(res);
      })
      .catch((err) => {
        console.log(err);
      });
  };
  return (
    <div className="App">
      <button onClick={onClickusers}>users</button>
    </div>
  );
}

```