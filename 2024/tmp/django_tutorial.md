## Django の概要
    - 
## クイックインストールガイド
## はじめての Django アプリ作成、その 1
## はじめての Django アプリ作成、その2

- migrate コマンドは INSTALLED_APPS の設定を参照するとともに、 mysite/settings.py ファイルのデータベース設定に従って必要なすべてのデータベースのテーブルを作成します。

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
- makemigrations を実行することで、Djangoにモデルに変更があったこと(この場合、新しいものを作成しました)を伝え、そして変更を マイグレーション の形で保存することができました。


## はじめての Django アプリ作成、その 3
## はじめての Django アプリ作成、その 4
## はじめての Django アプリ作成、その 5
## はじめての Django アプリ作成、その 6
## はじめての Django アプリ作成、その 7
## はじめてのDjangoアプリ作成、その8
## 高度なチュートリアル: 再利用可能アプリの書き方
## 次のステップへ
## Django への初めてのパッチを書く