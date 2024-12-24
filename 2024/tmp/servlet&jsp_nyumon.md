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
      - サーブレットクラス作成後にサーバが起動できなくなる不具合が起こった
          - [こちら](https://sukkiri.jp/books/sukkiri_servlet4/sukkiri_servlet4_appendix/%e5%8b%95%e7%9a%84web%e3%83%97%e3%83%ad%e3%82%b8%e3%82%a7%e3%82%af%e3%83%88%e3%81%ae%e4%bd%9c%e6%88%90.html)に記載のWTPプラグインの不具合をeclipse導入時に直しておく必要があったみたい。直すとすんなり行けた

## 4章
- JSｐファイルはリクエストされるとサーブレットクラスに変換されるため、サーブレットクラスでできることはＪＳＰファイルでもできる。
- スクリプトレット <%%>
  - JAVAのコードを埋め込めるやつ
  ```jsp
  <%@ page language="java" contentType="text/html; charset=UTF-8"
    pageEncoding="UTF-8"%>
  <% int x=10; %>>
  <!DOCTYPE html>
  <html>
  <head>
  <meta charset="UTF-8">
  <title>Insert title here</title>
  </head>
  <body>
  jsp!jsp!

  <% System.out.println(x); %>
  <% for(int i=0; i<x;i++) {%>
  <p>こんにちは</p>
  <% } %>
  </body>
  </html>
  ```
  ![image](https://github.com/user-attachments/assets/37fc64a9-0445-41d1-af54-e75fe315ab99)


  - スクリプト式
    - 変数などを出力できるやつ
    ```jsp
    <%@ page language="java" contentType="text/html; charset=UTF-8"
    pageEncoding="UTF-8" %>
    <%@ page import="java.util.Date,java.text.SimpleDateFormat" %>
    <%
    // 運勢をランダムで決定
    String[] luckArray = { "超スッキリ", "スッキリ", "最悪" };
    int index = (int) (Math.random() * 3);
    String luck = luckArray[index];
    
    // 実行日を取得
    Date date = new Date();
    SimpleDateFormat sdf = new SimpleDateFormat("MM月dd日");
    String today = sdf.format(date);
    
    int a=100;
    int sum=a*2;
    %>
    <!DOCTYPE html>
    <html>
    <head>
    <meta charset="UTF-8">
    <title>スッキリ占い</title>
    </head>
    <body>
    <p><%= today %>の運勢は｢<%= luck %>｣です</p>
    <%= a %>+<%= a %>=<%= sum %>
    </body>
    </html>
    ```
    ![image](https://github.com/user-attachments/assets/4f0c3f14-2e21-4216-aa8f-68dad9c8d5ad)

  - pageディレクティブ
    - jspファイルに関して様々な設定をする。<%@  @%>
    ```jsp
    <%@ page contentType="text/html; charset=UTF-8" @%>
    
    ```



## 5章
- Webページ内の入力項目の塊がフォーム。
- formタグ内にsubmitタイプの要素は最低1つは必要
- formタグのmethod属性にはGET,POSTどちらか設定できる
    - GETは何かしら新しい情報をとてくる時
    - POSTはフォームに入力した情報を登録するような時に使う

- GET,POSTの違い
    - GET→リクエストパラメータがブラウザのUELの?の後ろに表示されて見える
    - POSTパラメータは利用者側からは見えない
- 上記の特性から以下のようにも使い分けられる
    - GET→送信した結果を保存共有したいとき。ブックマークやSNSderiyou
    - POST→データをアドレスバーに表示したくない時（見られてはいけない情報。個人情報。機密情報）
- formをでPOST→サーブレットクラスでリクエスト受け取る→サーブレットでｈｔｍｌを出力
- formタグのaction属性の名称をwebservlet(xxx)に持つサーブレットへリクエストが行く
```jsp
<%@ page language="java" contentType="text/html; charset=UTF-8" 
    pageEncoding="UTF-8" %>
    <!DOCTYPE html>
    <html>
    <head>
    <meta charset="UTF-8">
    <title>ユーザー登録もどき</title>
    </head>
    <body>
    <form action="FormServlet" method="post">
    名前：<br>
    <input type="text" name="name"><br>
    性別：<br>
    男<input type="radio" name="gender" value="0">
    女<input type="radio" name="gender" value="1">
    <input type="submit" value="登録">
    <input type="hidden" value="ひでゅん" name="para" >
    </form>
    </body>
    </html>
    ```
    ```java
    package servlet;
    
    import java.io.IOException;
    import java.io.PrintWriter;
    
    import jakarta.servlet.ServletException;
    import jakarta.servlet.annotation.WebServlet;
    import jakarta.servlet.http.HttpServlet;
    import jakarta.servlet.http.HttpServletRequest;
    import jakarta.servlet.http.HttpServletResponse;
    
    /**
     * Servlet implementation class FormServlet
     */
    @WebServlet("/FormServlet")
    public class FormServlet extends HttpServlet {
    	private static final long serialVersionUID = 1L;
    
    	/**
    	 * @see HttpServlet#doPost(HttpServletRequest request, HttpServletResponse response)
    	 */
    	 protected void doPost(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
    		    // リクエストパラメータを取得
    		    request.setCharacterEncoding("UTF-8");
    		    String name = request.getParameter("name");
    		    String gender = request.getParameter("gender");
    		    String para = request.getParameter("para");
    
    		    // リクエストパラメータをチェック
    		    String errorMsg = "";
    		    if (name == null || name.length() == 0) {
    		      errorMsg += "名前が入力されていません<br>";
    		    }
    		    if (gender == null || gender.length() == 0) {
    		      errorMsg += "性別が選択されていません<br>";
    		    } else {
    		      if (gender.equals("0")) { gender = "男性"; }
    		      else if (gender.equals("1")) { gender = "女性"; }
    		    }
    
    		    // 表示するメッセージを設定
    		    String msg = name + "さん（" + gender + "）を登録しました";
    		    if (errorMsg.length() != 0) {
    		      msg = errorMsg;
    		    }
    
    		    // HTMLを出力
    		    response.setContentType("text/html; charset=UTF-8");
    		    PrintWriter out = response.getWriter();
    		    out.println("<!DOCTYPE html >");
    		    out.println("<html>");
    		    out.println("<head>");
    		    out.println("<meta charset=\"UTF-8\">");
    		    out.println("<title>ユーザー登録結果</title>");
    		    out.println("</head>");
    		    out.println("<body>");
    		    out.println("<p>" + msg + "</p>");
    		    out.println("<p>" + para + "</p>");
    		    out.println("</body>");
    		    out.println("</html>");
    		  }
    
            }
```

- 初期表示

![image](https://github.com/user-attachments/assets/3ec903ac-08cf-49e7-ba32-bcc9a7793287)

- 入力しサブミット

![image](https://github.com/user-attachments/assets/5a687e34-602f-4144-a8f2-77bab1fcd0a0)

- サーブレットでhtmlが生成され描画される

![image](https://github.com/user-attachments/assets/bf271847-8bf4-49f0-a99b-eafea2669f9b)


## 6章
- MVCもでるにしたがってアプリを作ると、ブラウザからリクエストされるのは基本的にサーブレットクラスになる。
-  JSpファイルはサーブレットからフォワードされて動くのが前提となる。
- WEB-INFフォルダ配下に配置されたファイルを、ブラウザは直接リクエストできない。
- redirect（ブラウザからリクエストを送ると、サーブレットクラスから「ここにリクエストをしなさい」というめいれいがレスポンスされる→命令を受けたブラウザは指示された先に自動的に再リクエストを送る→再リクエストの結果がブラウザに表示される。）
- ブラウザからhttp://localhost:8080/example4/fowardServletにアクセスしてJSPファイルを表示という流れだが、リダイレクトはブラウザから一回サーブレットにアクセスして、そのサーブレットがブラウザに、別のこっちのサーブレットにいってちょ！（http://localhost:8080/example4/fowardServlet２）ってやること。
- なのでフォワードは→リクエストしてレスポンスしての１往復
- リダイレクトはそれを２往復する

## 7章
- スコープに保存できるのはインスタンス
- 原則、JavaBeansというクラスのインスタンスを保存する。
- JavaBeansのるーる
  - 直列化可能である（Serializableを実装している）
  - クラスはpublicパッケージに所属している
  - publicで引数のないコンストラクタを持つ
  - フィールドはカプセルかする
- そもそものはなし、JavaBeansの役割は関連のある複数の情報をひとまとめにして保持すること
- リクエストスコープ
  - リクエスト毎に生成されるスコープ
  - request.setAttribute("属性名",インスタンス)  
  - リクエストスコープに保存したインスタンスはレスポンスが返されると消滅する

## 8章
- セッションスコープ
- 7章のリクエストスコープはリクエストレスポンス間しか存続しない
- そこでリクエストをまたいでも存続する奴がセッションスコープ
- セッションスコープは有効期間は開発者が決められる
- セッションスコープの正体はjakarta.servlet.http.HttpSession
- Httpsessionインスタンスはユーザごと（ブラウザ毎）に作成される
- この際、アプリケーションサーバ内ではセッションIDというユーザ（ブラウザ）固有のIDを新たに発行しHttpsessionインスタンスとブラウザに保存する。
- このセッションIDの設定されたブラウザは、それ以降のリクエストの際に自動でその設定されたセッションＩＤを送信する。
- サーバ側は送られてきたセッションＩＤを見て、手元のインスタンスの中から合致するＩＤのインスタンスを取りに行くという流れ、
- セッションスコープは、とても便利そうに皓ｋまで来ると聞こえるが、欠点もある。便利だからと言って多用しすぎると、メモリうｈ足になりサーバの停止になる危険性がある
- 

## 9章
- アプリケーションスコープ
