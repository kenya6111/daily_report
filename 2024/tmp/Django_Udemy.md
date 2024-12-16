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
    - ↑を実行したディレクトリにて、指定したアプリ名のフォルダが作られる。
    - 末尾に .をつけて実行すると指定フォルダは作られずその配下の生成されるファイル群が直接コマンド実行ディレクトリ配下に生成される。

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

- 画面で入力した値を元にタスクの色を変更する挙動を実装
    - まずmodelにpriorityというフィールドを作成
    - CharFieldのアプションにchoicesというタプルのリストを追加してあげる
    - 右が画面に出る値。左が登録される値。
    　```py

    from django.db import models

    CHOICE = (('danger', 'high'),('warning','normal'),('primary','low'))
    # Create your models here.
    class TodoModel(models.Model):
        title = models.CharField(max_length=100)
        memo = models.TextField()
        priority = models.CharField(
            max_length=50,
            choices = CHOICE)
        def __str__(self):
            return self.title
    ```
    - htmlファイルで{{}}を使ってpriorityフィールドの値をそのままbootstrapのクラス名として使って色を制御する流れ。
    ```html
     <div class="alert alert-{{ item.priority }}" role="alert">
        <p>
            {{ item.title}}
        </p>
        <a class="btn btn-info " role="button" aria-disabled="true">編集画面へ</a>
        <a class="btn btn-success " role="button" aria-disabled="true">削除画面へ</a>
        <a class="btn btn-primary " role="button" aria-disabled="true">詳細画面へ</a>
    </div>
    ```
- CreateView
    - まずurls.pyにcreateのパth（）を記載する
    ```py

    from django.contrib import admin
    from django.urls import include, path
    from .views import TodoList, TodoDetail, TodoCreate
    urlpatterns = [
        path('list/', TodoList.as_view() ),
        path('detail/<int:pk>', TodoDetail.as_view() ),
        path('create/', TodoCreate.as_view() ),
    ]
    ```

    - 次にviews.pyにTodoCreateのクラスベースドビューを作る。
    - この際に、CreateViewを使ったクラスベースドビュー内では、fields というオプションを記載する必要がある。このフィールドに画面から入力するフィールドを記載する。
    このfieldsがないとエラーになる
    ```py
    from django.views.generic import ListView, DetailView, CreateView
    from django.shortcuts import render
    from todo.models import TodoModel

    # Create your views here.
    class TodoList(ListView):
        template_name ='list.html'
        model = TodoModel

    class TodoDetail(DetailView):
        template_name ='detail.html'
        model = TodoModel

    class TodoCreate(CreateView):
        template_name ='create.html'
        model = TodoModel
        fields = ('title','memo','priority','duedate')
    ```

    - 最後にhtmlを作成する。
        - {{ form.as_p}}を書かないと↑で指定したfieldsの入力欄が出てこないので注意。
        - csrfのやつも忘れずに書く
        - 以下の状態でもまだエラーが出る
    ```html
    {% extends 'base.html' %}

    {% block content %}
    <form action="" method="POST"> {% csrf_token %} 
        {{ form.as_p }}
        <input type="submit" value="create">
    </form>
    {% endblock content %}
    ```
    - サブミット後の遷移先の画面はどこに行けばいいかわかりませんってエラーが出る。
        - なので遷移先をしていしてあげる。
            - success_urlを指定してあげる(これで遷移先の画面を設定できる。)
        ```py
        class TodoCreate(CreateView):
            template_name ='create.html'
            model = TodoModel
            fields = ('title','memo','priority','duedate')
            success_url = reverse_lazy('list')
        ```
        - 引数のlistってのはurls.pyのpath()メソッドないのnameオプションを指す。
        ```py
        from django.contrib import admin
        from django.urls import include, path
        from .views import TodoList, TodoDetail, TodoCreate
        urlpatterns = [
            path('list/', TodoList.as_view(), name='list' ),
            path('detail/<int:pk>', TodoDetail.as_view(), name='detail'),
            path('create/', TodoCreate.as_view(),name='create' ),
        ]
        ```
- DeleteView
    - まずurls.pyを修正する
    ```py
    from django.contrib import admin
    from django.urls import include, path
    from .views import TodoList, TodoDetail, TodoCreate, TodoDelete
    urlpatterns = [
        path('list/', TodoList.as_view(), name='list' ),
        path('detail/<int:pk>', TodoDetail.as_view(), name='detail'),
        path('create/', TodoCreate.as_view(),name='create' ),
        path('delete/<int:pk>', TodoDelete.as_view(),name='delete' ),
    ]
    ```
    - views.pyを修正
    ```py
    class TodoDelete(DeleteView):
        template_name ='delete.html'
        model = TodoModel
        success_url = reverse_lazy('list')
    ```

    - delete.htmlを作成
    ```html
    {% extends 'base.html' %}

    {% block content %}
    <form action="" method="POST"> {% csrf_token %} 
        <input type="submit" value="delete">
    </form>
    {% endblock content %}
    ```
- UpdateView
    - urls.pyに追加
    ```py
    path('update/<int:pk>', TodoUpdate.as_view(),name='update' ),
    ```
    - view.pyに追加
    ```py
    class TodoUpdate(UpdateView):
    template_name ='update.html'
    model = TodoModel
    fields = ('title','memo','priority','duedate')
    success_url = reverse_lazy('list')
    ```
    - htmlファイルを記載
    ```html
    {% extends 'base.html' %}

    {% block content %}
    <form action="" method="POST"> {% csrf_token %} 
        {{ form.as_p }}
        <input type="submit" value="update">
    </form>
    {% endblock content %}
    ```

- ボタンの遷移先（aタグのhref属性）を設定する
    - 以下の形で、{% url 'update' item.pk %}という感じで書く。
        - 'update'のところはurls.pyのname属性の値を指定する。 
    ```html
    {% extends 'base.html'%}

    {% block header %}
    <div class="p-5 mb-4 bg-body-tertiary rounded-3">
        <div class="container-fluid py-5">
        <h1 class="display-5 fw-bold">TodoList</h1>
        <p class="col-md-8 fs-4">TodoListを作成して生産的な毎日を過ごしましょう</p>
        </div>
    </div>

    {% endblock header%}

    {% block content%}
    <div class="container">

        {% for item in object_list %}
        <div class="alert alert-{{ item.priority }}" role="alert">
            <p>
                {{ item.title}}
            </p>
            <a href="{% url 'update' item.pk %}" class="btn btn-info " role="button" aria-disabled="true">編集画面へ</a>
            <a href="{% url 'delete' item.pk %}" class="btn btn-success " role="button" aria-disabled="true">削除画面へ</a>
            <a href="{% url 'detail' item.pk %}" class="btn btn-primary " role="button" aria-disabled="true">詳細画面へ</a>
        </div>
        {% endfor %}
    </div>
    {% endblock content%}

    ```

    - DONE!!!


## 社内SNSアプリ


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
    'boardapp.apps.BoardappConfig'
]
```


- projectのurls.pyにadmin以外のURLが来た際にアプリのurlに飛ぶように設定
```py
from django.contrib import admin
from django.urls import include, path

urlpatterns = [
    path('admin/', admin.site.urls),
    path('', include('boardapp.urls')),
]
```

- todoアプリの時はクラスベースおビューを使ったが、今回はファンクションベースドビューを使っていく。
- signupが画面の作成
    - まずappのurls.pyにpath()を追加。
        - 第二引数はviews.pyに書くファンクションベースドビューの関数名を記載
    ```py
    from django.contrib import admin
    from django.urls import path
    from .views import signupfunc

    urlpatterns = [
        path('signup/', signupfunc),
    ]
    ```
    - 次にappのviews.pyにファンクションベースドビューを記載
        - render()の引数はrequest,html,modelの順で記載
        - request.methodでGETかPOSTかを判定
    ```py
    
    def signupfunc(request):
        # return HttpResponse("aaaa")
        if request.method == 'POST':
            print("this is post")
        else:
            print("this is not post")
            
        return render(request, 'signup.html', {'some':100})
    ```

    - User.objects.all()でUserテーブルの値を全て取得
    - User.objects.get()でUserテーブルのレコードを指定して取得
    ```py

    from django.contrib.auth.models import User

    def signupfunc(request):
        object_list  = User.objects.all()
        userdamdin  = User.objects.get(username='admin')
        print(object_list)
        print(userdamdin)
        print(userdamdin.email)
        if request.method == 'POST':
            print("this is post")
        else:
            print("this is not post")

        return render(request, 'signup.html', {'some':100})
    ```

    - 画面でformのPOSTで投げた値は、request.POST[〇〇]で取得できる
    - User.objects.create_user()でユーザを登録できる
    - 登録できなかった場合はrender()でデータでerrorを返してhtml側で{{ error }}でエラーメッセージを表示する。
    ```py
    from django.shortcuts import render
    from django.contrib.auth.models import User
    def signupfunc(request):
        if request.method == 'POST':
            username = request.POST['username']
            password = request.POST['password']
            try:
                user = User.objects.create_user(username, 'aaa@example.com', password)
            except IntegrityError:
                return render(request, 'signup.html',{'error':'このユーザはすでに登録されています'})

        else:
            print("this is not post")
        return render(request, 'signup.html', {'some':100})

    ```
- ログイン
    - ログイン処理
        - authenticateメソッドを使う。
    ```py
    def loginfunc(request):
    if request.method == 'POST':
        username = request.POST["username"]
        password = request.POST["password"]
        user = authenticate(request, username=username, password=password)
        if user is not None:
            login(request, user)
            return render(request, 'login.html', {'context':'logged in !!'})
        else:
            return render(request, 'login.html', {'context':' not logged in !!'})

    return render(request, 'login.html', {'context':'get method '})
    ```


- render→違うビューを呼び出さない。
- redirect→違うビューを呼び出す。
    - redirect()ではurls.pyのｎame属性を指定
    - 何らかの処理が終わって違う画面に遷移する際に使う

- makemigrations→マイグレーションファイルを作成
- migrate→実際もDBに反映
- Modelを作成したあとはappのadmin.pyに以下の設定を記載(modelを登録する)
    ```py
    from django.contrib import admin

    from boardapp.models import BoardModel

    # Register your models here.
    admin.site.register(BoardModel)
    ```


- modelsから撮った値を画面に渡して表示
    ```py
    def listfunc(request):
        object_list = BoardModel.objects.all()
        return render(request, 'list.html', {'object_list':object_list})
    ```

    ```html
    {% extends 'base.html'%}

    {% block header %}
    <div class="alert alert-primary" role="alert">
        <h3>
            社内SNS
        </h3>
        </div>
    {% endblock header %}

    {% block content %}
    <div class="container">
        {% for item in object_list %}
        <div class="alert alert-success" role="alert">
            <p>タイトル :{{item.title}} </p>
            <p>投稿者 : {{item.author}}</p>
            <a class="btn btn-primary " role="button" aria-disabled="true">Primary link</a>
            <a class="btn btn-secondary " role="button" aria-disabled="true">Link</a>
        </div>
        {% endfor %}
    </div>
    {% endblock content %}
    ```

- 画像の取り扱い
    - projectのsettings.pyに以下の設定を記載
    - MEDIA_ROOTはメディアふぁいるを保存する場所を指定(tempaltesの設定を同じ容量で)
    - MEDIA＿URLはurlと画像のファイルを結びつける
    ```py

        MEDIA_ROOT = BASE_DIR/ 'media'
        MEDIA_URL = 'medi/'
    ```

    - static()では第一引数が入力されたときに、第二引数の場所から画像を持ってくる
    - adminの画面で画像名称クリックの際に以下のようのURLが飛ぶ
    - MEDIA_URLの値がパスに含まれる。
    ```
    http://localhost:8000/medi/スクリーンショット_2024-10-06_11.13.52.png
    ```
    ```py
    from django.contrib import admin
    from django.urls import include, path

    from boardproject import settings

    urlpatterns = [
        path('admin/', admin.site.urls),
        path('', include('boardapp.urls')),
    ] + static(settings.MEDIA_URL, document_root=settings.MEDIA_ROOT)
    ```


- 静的ファイルの取り扱い
    - projectのurls.pyに以下を追加
    ```py
    from django.contrib import admin
    from django.urls import include, path
    from django.conf import settings
    from django.conf.urls.static import static

    urlpatterns = [
        path('admin/', admin.site.urls),
        path('', include('boardapp.urls')),
    ] + static(settings.MEDIA_URL, document_root=settings.MEDIA_ROOT)
    + static(settings.STATIC_URL, document_root=settings.STATIC_ROOT)

    ```
    - projectのsettings.pyに以下を追記
    ```py
        #本番環境に適用するために使われDIRSのファイルを全てここにコピーする
        STATIC_ROOT = BASE_DIR/ 'staticfiles/'

        STATIC_URL = 'sta/'
        STATICFILES_DIRS = [str(BASE_DIR / 'static')]#複数のアプリ用いるcss

    ```

    - html では以下のようにhrefで呼び出して使う
    ```html
    <link href="{static 'style.css' }" rel="stylesheet">
    ```

- ログイン状態を判定する機能２種類
    - ①login_requredデコレータ
        - デコレータ → その関数が呼びだされる前に実行されるもの
        - view.pyで以下のようにビューにつけるだけ。
        ```txt
        login_required() は下記の処理を行います:
        もしユーザがログインしていなければ、settings.LOGIN_URL にリダイレクトし、クエリ文字列に現在の絶対パスを渡します。リダイレクト先の例: /accounts/login/?next=/polls/3/
        もしユーザがログインしていれば、通常通りビューを処理します。ビューの
        ```
        ```py
        @login_required
        def listfunc(request):
            object_list = BoardModel.objects.all()
            return render(request, 'list.html', {'object_list':object_list})
        ```

        - projectのsettings.pynにも以下を記載
        ```py
        LOGIN_URL = 'login'
        ```

        - login_requiredデコレータをつけたビュー関数が、ログインしてない状態だとLOGIN_URLの値をｎame属性にもつpath()にいき、ログインしているなら通常通りそのビュー関数を実行する

    - ② テンプレートの中に書いていく
        - 以下のような感じでテンプレートに{% if user.is_authenticated%}を使って条件分岐する
    ```html
    {% block content %}
    {% if user.is_authenticated%}
    <div class="container">
        {% for item in object_list %}
        <div class="alert alert-success" role="alert">
            <p>タイトル :{{item.title}} </p>
            <p>投稿者 : {{item.author}}</p>
            <a class="btn btn-primary " role="button" aria-disabled="true">Primary link</a>
            <a class="btn btn-secondary " role="button" aria-disabled="true">Link</a>
        </div>
        {% endfor %}
    </div>
    {% else %}
    please login

    {% endif %}
    {% endblock content %}
    ```

- ログアウト機能
    - ログアウトのpathを呼び出すボタンを作る。
    ```
        #この部分
        <a href="{%url 'logout'%}">logout</a>
    ```
    ```html
    {% extends 'base.html'%}

    {% block header %}
    <div class="alert alert-primary" role="alert">
        <h3>
            社内SNS
        </h3>
        </div>
    {% endblock header %}

    {% block content %}
    {% if user.is_authenticated%}
    <div class="container">
        {% for item in object_list %}
        <div class="alert alert-success" role="alert">
            <p>タイトル :{{item.title}} </p>
            <p>投稿者 : {{item.author}}</p>
            <a class="btn btn-primary " role="button" aria-disabled="true">Primary link</a>
            <a class="btn btn-secondary " role="button" aria-disabled="true">Link</a>
        </div>
        {% endfor %}
        <a href="{%url 'logout'%}">logout</a>
    </div>
    {% else %}
    please login
    {% endif %}
    {% endblock content %}
    ```

    - urls.pyに以下のpath()を追加する
    ```py
    path('logout/', logoutfunc, name="logout"),
    ```

    - ビューに以下を追加して完成
    ```py
    def logoutfunc(request):
        logout(request)
        return redirect('login')
    ```
- 詳細画面の作成

    - まずpath()の作成
    ```py
    path('detail/<int:pk>', detailfunc, name="detail"),
    ```

    - 次にviewの作成
    ```py
    def detailfunc(request, pk):
        object = get_object_or_404(BoardModel, pk=pk)
        return render(request, 'detail.html', {'object':object})
    ```

    - htmlの作成(detail.html)
    - 画像を表示する部分はちょっと注意
    ```
     src="{{object.sns_image.url}}"
    ```

    ```html
    {% extends 'base.html'%}
    {% block header %}
    <div class="alert alert-primary" role="alert">
        <h3>
            detailpage
        </h3>
        </div>
    {% endblock header %}
    {% block content %}
    {% if user.is_authenticated%}
    <div class="container">
        <div class="alert alert-success" role="alert">
            <p>タイトル :{{object.title}} </p>
            <p>投稿者 : {{object.author}}</p>
            <p><img src="{{object.sns_image.url}}" width="400" alt=""></p>
            <a class="btn btn-primary " role="button" aria-disabled="true">いいねする</a>
            <a class="btn btn-secondary " role="button" aria-disabled="true">既読にする</a>
        </div>
    </div>
    {% else %}
    please login
    {% endif %}
    {% endblock content %}
    ```

- list画面の各要素をクリック時に詳細に遷移する
    - 以下のように、aタグを追記。hrefにdetailのビューを呼ぶ記述追加。pkも渡すこと。
    ```html
    <p>タイトル :<a href="{% url 'detail' item.pk %}">{{item.title}}</a>  </p>
    ```

- いいね機能
    - まずｐath()を作成
    ```py

    path('good/<int:pk>', goodfunc, name="good")
    ```

    - 次にviewを作成
    ```py
    def goodfunc(request, pk):
        object = BoardModel.objects.get(pk=pk)
        object.good += 1
        object.save()
        return redirect('list')
    ```

    - detail画面のいいねボタンにaタグで以下のようにhrefを記載
    ```html
    <a href="{% url 'good' object.pk %}" class="btn btn-primary " role="button" aria-disabled="true">いいねする</a>
        
    ```


- 既読機能



- nullはDBに入ってくる値の話
- blankはformで入力の際の話