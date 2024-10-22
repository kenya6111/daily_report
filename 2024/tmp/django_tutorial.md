## Django の概要
    - 
## クイックインストールガイド
## はじめての Django アプリ作成、その 1
- 今回のチュートリアルを実施する用の適当なディレクトリを作成
```
mkdir mysite
```

- mysiteディレクトリに移動
```
cd mysite
```

- プロジェクトを作成する
```
python3 -m django startproject mysite .
```


- 以下のコマンドでサーバを起動(manage.pyのあるディレクトリで実行すること)
```
python3 manage.py runserver
```
- 以下のURLにブラウザからアクセスする。Djangoのウェルカムページが表示される。
```
http://localhost:8000/
```
- 「polls」というアプリケーションを作成する
```
k_tanaka@tanakaknyanoAir mysite % python3 manage.py startapp polls

k_tanaka@tanakaknyanoAir mysite % ls
db.sqlite3      manage.py       mysite          polls
```


- polls/views.pyに以下のソースを記載
```py
from django.http import HttpResponse

def index(request):
    return HttpResponse("Hello, world. You're at the polls index.")
```


- pollsディレクトリ配下にurls.pyを作成
```
polls/
    __init__.py
    admin.py
    apps.py
    migrations/
        __init__.py
    models.py
    tests.py
    urls.py
    views.py
```

- urls.pyに以下のソースを記載
```py
from django.urls import path

from . import views

urlpatterns = [
    path("", views.index, name="index"),
]
```

- プロジェクト（mysite）配下のurls.pyに以下に修正

```py
from django.contrib import admin
from django.urls import include, path

urlpatterns = [
    path("polls/", include("polls.urls")),
    path("admin/", admin.site.urls),
]
```



- 「http://localhost:8000/polls」にアクセス
    - 画面に「Hello, world. You're at the polls index.」と表示されれば成功
## はじめての Django アプリ作成、その2

- migrate コマンドは INSTALLED_APPS の設定を参照するとともに、 mysite/settings.py ファイルのデータベース設定に従って必要なすべてのデータベースのテーブルを作成します。
```
k_tanaka@tanakaknyanoAir mysite % python3 manage.py migrate
```

- polls/models.pyに以下の内容を記載

```py
polls/models.py¶
from django.db import models


class Question(models.Model):
    question_text = models.CharField(max_length=200)
    pub_date = models.DateTimeField("date published")


class Choice(models.Model):
    question = models.ForeignKey(Question, on_delete=models.CASCADE)
    choice_text = models.CharField(max_length=200)
    votes = models.IntegerField(default=0)
```

- mysite/setting.pyに以下の記載を修正
```py

INSTALLED_APPS = [
    "polls.apps.PollsConfig",
    'django.contrib.admin',
    'django.contrib.auth',
    'django.contrib.contenttypes',
    'django.contrib.sessions',
    'django.contrib.messages',
    'django.contrib.staticfiles',
]
```

- makemigrations を実行することで、Djangoにモデルに変更があったこと(この場合、新しいものを作成しました)を伝え、そして変更を マイグレーション の形で保存することができました。
```
python manage.py makemigrations polls
```

- migrate を再度実行し、 モデルのテーブルをデータベースに作成
```
$ python3 manage.py migrate
```

- shellで遊んでみる
```
k_tanaka@tanakaknyanoAir mysite % python3 manage.py shell
Python 3.9.6 (default, Feb  3 2024, 15:58:27) 
[Clang 15.0.0 (clang-1500.3.9.4)] on darwin
Type "help", "copyright", "credits" or "license" for more information.
(InteractiveConsole)
>>> from polls.models import Choice, Question
>>> Question.objects.all()
<QuerySet []>
>>> from django.utils import timezone
>>>  q = Question(question_text="What's new?", pub_date=timezone.now())
  File "<console>", line 1
    q = Question(question_text="What's new?", pub_date=timezone.now())
IndentationError: unexpected indent
>>> q = Question(question_text="What's new?", pub_date=timezone.now())
>>> q.save()
>>> q.id
1
>>> q.question_text
"What's new?"
>>> q.pub_date
datetime.datetime(2024, 10, 15, 12, 24, 7, 807229, tzinfo=datetime.timezone.utc)
>>> q.question_text = "What's up?"
>>> q.save()
>>> q.question_text
"What's up?"
>>> Question.objects.all()
<QuerySet [<Question: Question object (1)>]>
>>> q = Question(question_text="testtest", pub_date=timezone.now())
>>> q.save()
>>> Question.objects.all()
<QuerySet [<Question: Question object (1)>, <Question: Question object (2)>]>
>>> 
```

- polls/models.pyに以下の内容を記載
```py

from django.db import models


class Question(models.Model):
    # ...
    def __str__(self):
        return self.question_text


class Choice(models.Model):
    # ...
    def __str__(self):
        return self.choice_text
```


- 以下コマンドで管理サイト用のユーザを作成する
```
 python manage.py createsuperuser
```


- サーバを起動
```
$ python manage.py runserver

```

- ブラウザから「http://localhost:8000/admin/」にアクセス
    →管理側サイトが表示される

- 管理側サイトから先ほど作ったモデルを操作するにはpolls/admin.pyに以下の記載を追加

```py
from .models import Question
admin.site.register(Question)
```

- 再度管理側サイトを覗くと、model.pyに記載したテーブルが表示される

## はじめての Django アプリ作成、その 3

- polls/views.pyに以下のビューを追加
```py
def detail(request, question_id):
    return HttpResponse("You're looking at question %s." % question_id)


def results(request, question_id):
    response = "You're looking at the results of question %s."
    return HttpResponse(response % question_id)


def vote(request, question_id):
    return HttpResponse("You're voting on question %s." % question_id)
```

- polls.urls.pyに以下のｐathを追加
```py
from django.urls import path

from . import views

urlpatterns = [
    # ex: /polls/
    path("", views.index, name="index"),
    # ex: /polls/5/
    path("<int:question_id>/", views.detail, name="detail"),
    # ex: /polls/5/results/
    path("<int:question_id>/results/", views.results, name="results"),
    # ex: /polls/5/vote/
    path("<int:question_id>/vote/", views.vote, name="vote"),
]
```

- 以下のURLにアクセスすると各ビューに設定した文言が表示されます
http://localhost:8000/polls/2/
http://localhost:8000/polls/2/results/
http://localhost:8000/polls/2/vote/

- views.pyのindexビューを以下のように編集。
```py
def index(request):
    latest_question_list = Question.objects.order_by("-pub_date")[:5]
    template = loader.get_template("polls/index.html")
    context = {
        "latest_question_list": latest_question_list,
    }
    return HttpResponse(template.render(context, request))
```
- polls/templates/polls/index.htmlを作成
```html
{% if latest_question_list %}
    <ul>
    {% for question in latest_question_list %}
        <li><a href="/polls/{{ question.id }}/">{{ question.question_text }}</a></li>
    {% endfor %}
    </ul>
{% else %}
    <p>No polls are available.</p>
{% endif %}
```
- http://localhost:8000/polls/　にアクセスするとQuestionテーブルのレコードがリスト形式で表示される


- polls/views.py
- 以下のようにrender()を使ったビューの実装方法もある。
```py
from django.shortcuts import render

from .models import Question


def index(request):
    latest_question_list = Question.objects.order_by("-pub_date")[:5]
    context = {"latest_question_list": latest_question_list}
    return render(request, "polls/index.html", context)
```

- detailのビューを以下
get() を実行し、オブジェクトが存在しない場合には Http404 を送出することは非常によく使われるイディオムです。
```py
def detail(request, question_id):
    try:
        question = Question.objects.get(pk=question_id)
    except Question.DoesNotExist:
        raise Http404("Question does not exist")
    return render(request, "polls/detail.html", {"question": question})
```

- 上記はget_object_or_404()を使って省力して記述できる。
```py
def detail(request, question_id):
    question = get_object_or_404(Question, pk=question_id)
    return render(request, "polls/detail.html", {"question": question})

```

- polls/templates/polls/detail.htmlを以下のように修正。
    - choice_set()はそのquestionに紐づくchoiceが取得される。
```py
<h1>{{ question.question_text }}</h1>
<ul>
{% for choice in question.choice_set.all %}
    <li>{{ choice.choice_text }}</li>
{% endfor %}
</ul>
```

- polls/index.htmlを以下のように修正
    - 以下のように書くことでurls.pyのnameの値がdetailのpath()を呼び出せる。
```html
<li><a href="{% url 'detail' question.id %}">{{ question.question_text }}</a></li>
```

    - pollsのurls.pyにて名前空間を設定し、他のアプリのurls.py特別できるようにしてあげる
```py 
# polls/urls.py
from django.urls import path

from . import views

app_name = "polls"
urlpatterns = [
    path("", views.index, name="index"),
    path("<int:question_id>/", views.detail, name="detail"),
    path("<int:question_id>/results/", views.results, name="results"),
    path("<int:question_id>/vote/", views.vote, name="vote"),
]
```



    - 今回はアプリが1つだけなので、上記の書き方で、pollsのurls.pyが呼び出されるが、アプリが複数の場合、どのアプリのurls.pyかを指定する必要がある。それがいか。
```html
# polls/templates/polls/index.html
<li><a href="{% url 'polls:detail' question.id %}">{{ question.question_text }}</a></li>
```
## はじめての Django アプリ作成、その 4

- polls/templates/polls/detail.htmlを以下のように修正
    - legend
        - <fieldset>でグループ化されたフォームの入力項目に対して、タイトルを付けるためのタグ。
    - forloop.counter は、 for タグのループが何度実行されたかを表す値。
    
```html
<form action="{% url 'polls:vote' question.id %}" method="post">
{% csrf_token %}
<fieldset>
    <legend><h1>{{ question.question_text }}</h1></legend>
    {% if error_message %}<p><strong>{{ error_message }}</strong></p>{% endif %}
    {% for choice in question.choice_set.all %}
        <input type="radio" name="choice" id="choice{{ forloop.counter }}" value="{{ choice.id }}">
        <label for="choice{{ forloop.counter }}">{{ choice.choice_text }}</label><br>
    {% endfor %}
</fieldset>

<input type="submit" value="Vote">
</form>
```


- polls/views.pyのvote関数を以下のように修正
```py
from django.http import HttpResponse, HttpResponseRedirect
from django.shortcuts import get_object_or_404, render
from django.urls import reverse

from .models import Choice, Question


# ...
def vote(request, question_id):
    question = get_object_or_404(Question, pk=question_id)
    try:
        selected_choice = question.choice_set.get(pk=request.POST["choice"])
    except (KeyError, Choice.DoesNotExist):
        # Redisplay the question voting form.
        return render(
            request,
            "polls/detail.html",
            {
                "question": question,
                "error_message": "You didn't select a choice.",
            },
        )
    else:
        selected_choice.votes += 1
        selected_choice.save()
        # Always return an HttpResponseRedirect after successfully dealing
        # with POST data. This prevents data from being posted twice if a
        # user hits the Back button.
        return HttpResponseRedirect(reverse("polls:results", args=(question.id,)))
```

- polls/view.pyに以下の関数を追加
```py
def results(request, question_id):
    # response = "You're looking at the results of question %s."
    # return HttpResponse(response % question_id)
    question = get_object_or_404(Question, pk=question_id)
    return render(request, "polls/results.html", {"question": question})
```

- polls/templates/polls/results.htmlを作成

```html
<h1>{{ question.question_text }}</h1>

<ul>
{% for choice in question.choice_set.all %}
    <li>{{ choice.choice_text }} -- {{ choice.votes }} vote{{ choice.votes|pluralize }}</li>
{% endfor %}
</ul>

<a href="{% url 'polls:detail' question.id %}">Vote again?</a>
```


- polls/urls.pyを以下のように修正( <question_id> から <pk> に変更)

```py
from django.urls import path

from . import views

app_name = "polls"
urlpatterns = [
    path("", views.IndexView.as_view(), name="index"),
    path("<int:pk>/", views.DetailView.as_view(), name="detail"),
    path("<int:pk>/results/", views.ResultsView.as_view(), name="results"),
    path("<int:question_id>/vote/", views.vote, name="vote"),
]
```



## はじめての Django アプリ作成、その 5
- polls/tests.py
    - ssertIs(a, b) というメソッドは、「a is b」となるかどうかのテストを実行しており、was_published_recently() の出力が False となっていれば、テストは成功となる
    ```py
    import datetime

    from django.test import TestCase
    from django.utils import timezone

    from .models import Question


    class QuestionModelTests(TestCase):
        def test_was_published_recently_with_future_question(self):
            """
            was_published_recently() returns False for questions whose pub_date
            is in the future.
            """
            # 現在日時から３０日後の日時を取得
            time = timezone.now() + datetime.timedelta(days=30)

            # 上記の日時を使ってQuestionインスタンスを作成
            future_question = Question(pub_date=time)

            # 上記のQuestionインスタンスをつかってwas_published_recently()を呼び出し真偽値を確認
            self.assertIs(future_question.was_published_recently(), False)

            # modelsのwas_published_recentlyに定義した通り、pub_dateが1日前から現在日時までの間であればtrueが返される

            # そのwas_published_recentlyの返り血がFalseなら上記テスト自体はTrueとはんていする仕様
    ```




## はじめての Django アプリ作成、その 6
- templatesと同じようにpollsディレクトリはいかにstaticディレクトリを作る。
- そこへstyle.cssも作成
    - polls.static/polls/style.css
- style.cssに以下を記載
```css
li a {
    color: green;
}
```

- style.cssをhtmlで読み込む
```html
{% load static %}

<link rel="stylesheet" href="{% static 'polls/style.css' %}">
```
- cssが適用される

-  polls/static/polls/ ディレクトリの中に images サブディレクトリを 作ります。
```css
body {
    background: white url("images/background.png") no-repeat;
}
 ```

- polls/static/polls/ ディレクトリの中に images サブディレクトリを作成
polls/static/polls/images/background.png となるようにする。
polls/static/polls/style.cssに以下の内容を追記

```css
body {
    background: white url("images/background.png") no-repeat;
}
```

ー http://localhost:8000/polls/ をリロードすると、読み込んだ背景画像がスクリーンの左上部に確認できる
## はじめての Django アプリ作成、その 7
## はじめてのDjangoアプリ作成、その8
## 高度なチュートリアル: 再利用可能アプリの書き方
## 次のステップへ
## Django への初めてのパッチを書く