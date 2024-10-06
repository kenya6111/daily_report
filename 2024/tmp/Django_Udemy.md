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
    - views.pyファイルのクラスにて、引数に〇〇View（TemplateView等）を記述する。これを継承で持ってくる。このがクラスベースドビューにおいて特異。
- function based view
    - １から書くので手間かかるが、実現できる範囲が広い
```py
## setting.pyのDIRSにhtmlファイルのありかを指定する必要があある。
TEMPLATES = [
    {
        'BACKEND': 'django.template.backends.django.DjangoTemplates',
        'DIRS': [BASE_DIR / 'templates'],
        'APP_DIRS': True,
        'OPTIONS': {
            'context_processors': [
                'django.template.context_processors.debug',
                'django.template.context_processors.request',
                'django.contrib.auth.context_processors.auth',
                'django.contrib.messages.context_processors.messages',
            ],
        },
    },
]
```

- クラスベースドビューの場合（HelloworldClass）、path（）の第二引数には
view.pyファイルの該当のクラス名＋「.as_view()」と各ルール。
```py
## urls.py 
urlpatterns = [
    path('admin/', admin.site.urls),
    path('helloworld/', helloworldfunction),
    path('HelloworldClass/', HelloworldClass.as_view())
]

```

- リクエストはまずプロジェクトのurls.pyファイルが受け取り、その次にプロジェクト内の各アプリケーションに割り振って行ってる

- python3 manage.py startapp helloworldapp  でアプリケーション作成

- アプリを作ったらプロジェクトに対して、こんなアプリを作成しましたよって教えてあげる必要がある、
    - p路ジェクトのsetting.pyのINSTALLED_APPSに追加する
    - 「アプリ名」＋「.apps」＋「.頭大文字アプリ名＋頭大文字Config」
    ```py
    INSTALLED_APPS = [
        'django.contrib.admin',
        'django.contrib.auth',
        'django.contrib.contenttypes',
        'django.contrib.sessions',
        'django.contrib.messages',
        'django.contrib.staticfiles',
        'helloworldapp.apps.HelloworldappConfig'
    ]
    ```
- プロジェクト側のurls.py
    - プロジェクト側で受けたurlはアプリにそのまま引き継がれはしない
    ```py
        urlpatterns =[
            ("url" → アプリのurl)
        ]

        ## 実際は以下の通り
        urlpatterns = [
            path('admin/', admin.site.urls),
            path('helloworld/', helloworldfunction),
            path('helloworld2/', HelloworldClass.as_view()),
            path('helloapp/', include('helloworldapp.urls'))
            ## アプリの名前＋urlsをinclude()に渡したものを書く
        ]
    ```
    - アプリの名前＋urlsをinclude()に渡したものを書く
- アプリ側のurls.py
    ```py
    urlpatterns =[
        ("url" → "views.py"の関数名またはクラス名)
    ]

    ## 実際のurls.py
    from django.contrib import admin
    from django.urls import include, path
    from .views  import helloworldappview
    urlpatterns = [
        path('helloworldapp/', helloworldappview),
    ]
    ```

    - 上記でviews.pyファイルのクラスかもしくは関数を呼び出す