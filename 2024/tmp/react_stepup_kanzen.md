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
  - 