## 2章
サーブレットとはjavaを用いてサーバ再度プログラムを実現する技術。サーブレットクラスというブラウザから実行できるクラスを使用してサーバ再度プログラムを実現します。


## 3章
- サーブレット
    - サーブラットはブラウザからのリクエストで実行され、その実行結果をHTMLで出力し返す。ブラウザからリクエストが届いた時にその場でhtmｌをつくって返す。
    - servletクラス(javaファイル)にてhtml要素を記述し、そのままそれを画面に表示する
    - ```java
        package servlet;
        import java.io.IOException;
        import java.io.PrintWriter;
        import java.text.SimpleDateFormat;
        import java.util.Date;
        
        import jakarta.servlet.ServletException;
        import jakarta.servlet.annotation.WebServlet;
        import jakarta.servlet.http.HttpServlet;
        import jakarta.servlet.http.HttpServletRequest;
        import jakarta.servlet.http.HttpServletResponse;

        @WebServlet("/UranaiServlet")
        public class UranaiServlet extends HttpServlet {
          private static final long serialVersionUID = 1L;
        
      protected void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
            // 運勢をランダムで決定
            String[] luckArray = { "超スッキリ", "スッキリ", "最悪" };
            int index = (int) (Math.random() * 3);
            String luck = luckArray[index];
            // 実行日を取得
            Date date = new Date();
            SimpleDateFormat sdf = new SimpleDateFormat("MM月dd日");
            String today = sdf.format(date);
        
            // HTMLを出力
            response.setContentType("text/html; charset=UTF-8");
            PrintWriter out = response.getWriter();
            out.println("<!DOCTYPE html>");
            out.println("<html>");
            out.println("<head>");
            out.println("<meta charset=\"UTF-8\" />");
            out.println("<title>スッキリ占い</title>");
            out.println("</head>");
            out.println("<body>");
            out.println("<p>" + today + "の運勢は「" + luck + "」です</p>");
            out.println("</body>");
            out.println("</html>");
          }
      }
      ```
      - http://localhost:8080/example3/UranaiServletにアクセスして画面が表示された！
      - (http://<サーバ名>/<アプリケーション名>/<クラス名>)

## 5章
- formタグ内にsubmitタイプの要素は最低1つは必要
- formタグのmethod属性にはGET,POSTどちらか設定できる
    - GETは何かしら新しい情報をとてくる時
    - POSTはフォームにぬ湯力した情報を登録するような時に使う

- GET,POSTの違い
    - GET→リクエストパラメータがブラウザのUELの?の後ろに表示されて見える
    - POSTパラメータは利用者側からは見えない
- 上記の特性から以下のようにも使い分けられる
    - GET→送信した結果を保存共有したいとき。ブックマークやSNSderiyou
    - POST→データをアドレスバーに表示したくない時（見られてはいけない情報。個人情報。機密情報）
