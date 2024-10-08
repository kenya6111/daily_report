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



## TODOアプリ

- まずsetting.pyのTEMPLATESのDIRSを設定する。（htmlなどのファイルの保存場所を指定する）
- BASE_DIRのtemplatesというディレクトリにhtmlを保存するという意味。
    - すなわちmanage.pyファイルのある階層にtemplatsディレクトリを作成する。
```py
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


- 次にstartappコマンドで作ったtodoAppというアプリケーションを作ったよってことをDjangoに知らせる。


```py
INSTALLED_APPS = [
    'django.contrib.admin',
    'django.contrib.auth',
    'django.contrib.contenttypes',
    'django.contrib.sessions',
    'django.contrib.messages',
    'django.contrib.staticfiles',
    'todo.apps.TodoCOnfig'
]
```

- お次はURLのつなぎこみ。
    - プロジェクトのurls.pyで最初にURLを受けて、先頭のURLに合致すレバそれ以降をアプリurls.pyni渡す
    - include()を使ってアプリのurlsを呼び出す

    ```py

        from django.contrib import admin
        from django.urls import include, path

        urlpatterns = [
            path('admin/', admin.site.urls),
            path('',include('todo.urls')),
        ]

    ```

- アプリの方にurls.pyを作成
- → これで初期設定完了
- models.pyはDjangoとデータベースの橋渡しの役割。
    - modelファイルの中身のモデルは、classで定義して書く。
    - 引数はmodels.Modelというモジュールを継承して書く
    - 中身にカラムの設定値を記載
    ```py
    from django.db import models
    class TodoModel(models.Model):
        title = models.CharField(max_length=100)
        memo = models.TextField()
    ```

- 次にmigrate,makemigrationコマンドを実行
    - migrateはDBの中にテーブルを実際に作成する
    - makemigarations
        - models.pyファイルの中身に基づいて設計図を作る。
        - 履歴を残す。
        - 変更した履歴を残していくコマンド。
        - 間違えた時にDBの状態をその時に戻せるように。
        - なので変更履歴を記録するコマンドと言える
    - ここでINSTALLED_APP(projectのsettings.py)に作成したアプリの登録をしていないとモデルのマイグレーションが行われないので注意

- 上記コマンドではDBにテーブルは作られるが、管理画面では表示されない。
    - アプリのadmin.pyに、このテーブルを使いますよ〜って登録させる設定が必要
    - こんな感じでモデルの登録をする
    ```Py
        from django.contrib import admin

        from todo.models import TodoModel

        # Register your models here.
        admin.site.register(TodoModel)
    ```
    - 管理画面でTodoModelが表示されるようになる
- 管理画面からデータ登録した際に 「TodoModel object(1)」というふうな表示になっており見づらい。。
    - 以下のようにｍodels.pyのクラス内に __str__()メソッドを作成する
    - このメソッドにより管理画面でレコード作成した際にそのレコードの表示がreturnした部分のあたいになる。
    ```py
    from django.db import models

    # Create your models here.
    class TodoModel(models.Model):
        title = models.CharField(max_length=100)
        memo = models.TextField()

        def __str__(self):
            return self.title
    ```

- DjangoはCRUDの各操作に対してViewを準備してくれている
    - このViewを使ってviews.pyのクラスビューを作成する
    - CREATE → CreateView
    - READ → ListView(データの一覧の表示用),DetailView
    - UPDATE → UpdateView
    - DELETE → Delete`View

- 早速一覧を表示するListViewを使って一覧画面を作る
    - Classベースドviewを使う時は.as_view()をつけるのを忘れずに。
    - urls.py(アプリ)
    ```py
    from django.contrib import admin
    from django.urls import include, path
    from .views import TodoList
    urlpatterns = [
        path('list/', TodoList.as_view() ),
    ]
    ```

    - views.py（アプリ）
    - views.pyファイルでclassbasedviewを作る場合は中身にtemplatenameとmodelの指定をする
    - ちなみにクラスベースドビューではhtml側ではobject_listという名前でモデルの値を取れるルール
    ```py
    from django.views.generic.list import ListView
    from django.shortcuts import render
    from todo.models import TodoModel

    class TodoList(ListView):
    template_name ='list.html'
    model = TodoModel
    ```
    - ここまできたらあとはhtmlファイルを作成するだけ。
        - views.pyの使うクラスやメソッドで指定したテンプレートネームのhtmlファイルを作成
        - {%%} forやifの条件分岐などのpythonの処理を書く時に使う
        - {{}}　→どのオブジェクトのどのフィールドを表示する時に使う
        ```html
        {% for item in object_list %}
        <li>
            {{ item.title }}
            {{ item.memo }}
        </li>

        {% endfor %}
        ```
- お次はDetailView。個別のデータを表示することに特化したテンプレートビュー。
    - まずurls.pyにｐath()を追加
    - DetailViewの時はpath()の第一引数に<int:pk>をつける。じゃないとどのおぶじぇくとを見ればいいかDjangoが困ってしまう
    ```py

    from django.contrib import admin
    from django.urls import include, path
    from .views import TodoList, TodoDetail
    urlpatterns = [
        path('list/', TodoList.as_view() ),
        path('detail/<int:pk>', TodoDetail.as_view() ),
    ]

    ```
    - 同じようにviews.pyも更新
    ```py
    from django.views.generic import ListView, DetailView

    from django.shortcuts import render

    from todo.models import TodoModel

    # Create your views here.
    class TodoList(ListView):
        template_name ='list.html'
        model = TodoModel

    class TodoDetail(DetailView):
        template_name ='detail.html'
        model = TodoModel
    ```
    - DetailViewではobjectという名称でモデルの値を取れる