## section11

                                            
- ブラウザ → httpリクエスト → サーバ → urls.py
                            ↑        ↓
                            ↑←←←  views.py   →   models.py
                                    ↓↑       ←
                                    ↓↑
                                    x.html
                                    y.html
                                    z.html
- python3 -m django startproject helloworldproject でプロジェクト作成
- python3 manage.py runserver
    -  このコマンドでサーバ起動。
- Djangoのいろんな便利な機能をmanage.pyを使って実行していく。（runserver,DBの作成,）
- init.py
    - 一連のディレクトリをpythonパッケージですよ〜ってpythonに認識させるためのもの
    ディレクトリ内にこのファイルがあるおかげでimport などができる
- asgi.py wsgi.py
    - あまり意識しないでいいいファイル。asgi はwsgiの発展版。asgiはDjangoの3.0から新しく導入された機能。　
    - wsgi = web server gateway interface
        - webサーバとDjangoのコードを取り持つ
- settings.py
    - Djangoの全体的な設定のために使われる
- urls.py
    - webサーバからリクエストを受けた時に最初に受ける場所。

- BASE_DIR = 画像やcssファイルを保存する際のパス指定で基準として使うパス。
    - デフォルトはmanage.pyが入っている場所になる
- SECRET_KEY 
    - ユーザのパスワードを作る際に使う暗号化された文字列。
    - これは第三者に見られはいけない。これを使ってパスワードを作っていることがバレるとまずい、
- ALLOWED_HOST
    - webサーバからリクエストを受けるときに どのサーバからアクセスを許可するか、を設定する
- INSTALLED_APPS
    - start appコマンドでアプリケーションを作成した際に追加すべき項目
- ROOT_URLCONF
    - Djangoがリクエストを受けた際に一番初めに使うurls.pyを書く
- urls.pyの
    ```py
    urlpatterns = [
        path('admin/', admin.site.urls),
    ]
    ```
    - adminという「url」を受け取った場合に、admin.site.urlsを呼び出す
    - 第２引数のほうは、views.pyファイルで定義するクラス名もしくは関数名を入れる
    - アクセスしてきたurlと合致しているものをurlpatternsの上から順番に見ていく。
    - pythonではいかなるものもオブジェクトとして扱うという通り、このurls.pyのファイルにおいてもオブジェクトを受け取り、オブジェクトを返す。
        - HTTPリクエストオブジェクトを受け取り、そのままview.pyに送り views.pyはHTTPレスポンスp武ジェクトをサーバに返す流れ
- class based view
    - こっちは記述が少なくて済む。その分カスタマイズの制限あり
    - 
- function based view
    - １から書くので手間かかるが、実現できる範囲が広い