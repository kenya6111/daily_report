## 1章（Djangoとは？）

## 2章（アーキテクチャ）
![alt text](image-13.png)

## 3章(プロジェクト構成)
![alt text](image-14.png)
- 作成したアプリは丸ごと入れ替えができるように作るのがコツ。他アプリとなるべく依存しないお湯に作ること。

- 1. 「startproject」 コマンドでひな 型 プロジェクトを 作成 する
- 2. 「startapp」 コマンドでひな 型 アプリケーションを 作成 する
- 3. 設定 ファイルでアプリケーションを 有効化 する
- 4. アプリケーションの 構成要素（ モデル、 テンプレート、 ビューなど） を 実装 する

## 4章
![alt text](image-15.png)

- settings.pyの「ROOT_URLCONF = 'config.urls'」で、指定したものが、ルートURLConf（URLディスパッチャに最初に読まれるもの）になる。

- パスコンバータ
    - urls.pyのパスのけつに書いてたパラメータのアレのこと。（path("admin/edit/<int:pk>", ItemEdit.as_view(), name="edit")）
    - 「int」「str」「path」「slug」「uuid」のなどがある・
    - パスコンバータを省略すると、デフォルトのstrが適用される。
    ![alt text](image-16.png)

- URLパターンの書き方。
    - urlパターンの左端には「/」をつけない
    - 右端には「/」をつける
        - 右端につけないとプロジェクト→アプリへパスを繋いでる際に「/」が出ないのでルーティングエラーが起こる
    - 
## 5章
![alt text](image-17.png)

- クラスベースドビューは、関数ベースビュとは違い、GETじゃPOSTかはgetメソッド/postメソッドを定義しさえすればいい。
- 筆者t系にはクラスベースユーがおすすめらしい
    - 見通しの良いビューを書きたいのであれば、「django.views.generic.View」を直接継承すればいい
    - 「汎用ビュー（Generic View）」などの様々なクラスが提供されている
    - お決まりの処理を再利用できるMixinクラスがいくつも用意されている。
![alt text](image-18.png)
- Clasy Class-Views(CCBV)のサイトは、そERぞれの汎用ビュークラスでオーバーライドできる変数やメソッドが見やすく整理されていて便利

- ログイン済みのじょうたいとは、以下のような状態である。
    - 1.ユーザ固有の情報を照合することでユーザが特定されており
    - 2.リクエストごとにユーザの特定作業をやり直さなくてもユーザ情報を引き継げる
- 1はユーザ認証パスワードなど様々な方式でユーザの本人確認が行われる
- 2は多くの場合、ブラウザのクッキーとサーバ側のセッションを利用してリクエストを跨いだ引き継ぎを実現する。これは一般にユーザセッション管理と呼ばれる
![alt text](image-19.png)
- 図に示したように、受付窓口では、はじめに運転免許証などを使って本人確認をおこない  ます。いったん本人確認の手続きを済ませてしまえば、あとは本人を特定するための「受  付番号」でやり取りすれば、面倒な本人確認を何度もやり直す必要はありません

![alt text](image-20.png)
- ①で送信されたユーザー名・パスワードと、事前に登録されたユーザーテーブルのユー  ザー名・パスワードを照合することで本人確認をおこないます （②）。 本人確認が済んだ  ら、ログイン情報を「セッション」というサーバ側の保存領域に格納し、キーとなる「セッ  ションID」 と呼ばれるランダムな文字列を発行します。この「セッションID」 はログイ  ン時のレスポンス （③） でブラウザに返され、ブラウザ側の 「Cookie」 という保存領域に  格納されます。ブラウザの仕様により、 Cookie に保存されたセッションID はリクエス  トのたびに送信される仕組みになっているため、 ④以降では逐一本人確認をしなくてもロ  グイン済みユーザーの一連の操作として扱われるようになるので

## 6章(モデル Model)
![alt text](image-21.png)

- 一対一のリレーション
    - ある種類のデータに対して他の種類のデータが必ず一つ（あるいはゼロ）しか関連づかない関係。
        - 本と本の詳細情報
        - システム利用者とショッピングカート
    - django.db.models.fields.related.OneToOneFieldを使う。
    ```python
    from django.db import models
    class Book(models.Model):  """ 本 モデル"""
        class Meta:
            db_table = 'book'
        title = models.CharField(verbose_name='タイトル', max_length=255)

    class BookStock(models.Model):
        """ 本 の 在庫 モデル"""
        class Meta:
            db_table = 'book_stock'  53
        book = models.OneToOneField(Book, verbose_name='本',  on_delete=models.CASCADE)
        quantity = models.IntegerField(verbose_name='在庫数', default=0)
    ```
- 多対一
    - 一つのものに複数が紐づくときの関係
    - 一方から見たときに相手を「同時に2つ以上」紐づけることができるかどうかで相手が「多」かどうかが決まります。双方のエンティティから確認してみるとわかりやすいです。
    - 例えば「本」「出版社」の関係。一つの出版社から見たときに、複数の本を出版するのはあり得る。ので本は「多」になる。

    - 一方、一冊の本から見た時に出版社は一つしか関連付かないはずなので出版社は「一」になる。なので本と出版社の関係は「多対一」となる。
    - 他にも「注文（多）」と「システム利用者（一）」
    - 「注文明細（多）」と「注文（一）」
    - 「従業員（多）」と「部署（一）」
    - 「ブログ記事（多）」と「ブログの投稿者（一）」
    - 一般に多対一のリレーションでは「一」側のテーブルの主キーを参照する外部キーを「多」側のテーブルに設けることで実現すr椿井が多い

    ```python
    from django.db import models  
    class Publisher(models.Model):  
        """出版社モデル"""  
        class Meta:  
            db_table = 'publisher'  
            name = models.CharField(verbose_name='出版社名', 
            max_length=255)  
    class Book(models.Model):  
        """本モデル"""  
        class Meta:  
            db_table = 'book'  
        title = models.CharField(verbose_name='タイトル', max_length=255)  publisher = models.ForeignKey(Publisher, verbose_name='出版社',  on_delete=models.PROTECT)
    ```
    - 外部キーのカラムのon_deleteプロパティに関連先のオブジェクトが削除された場合の自身のオブジェクトがどのような挙動をするかの設定をする
    ![alt text](image-22.png)

- 多対多
    - 本と著者の場合
    ![alt text](image-23.png)
    - 実際のDBテーブル設計では中間テーブルを作ることで多対多のリレーションを実現するのが一般的。
    - Djangoでは、一方のモデルフィールドでManyToManyFieldをつけるだけで対応が可能
    ```python
    from django.db import models  
    class Author(models.Model):  
        """著者モデル"""  
        class Meta:  
            db_table = 'author'  
        name = models.CharField(verbose_name='著者名', max_length=255)  
    
    class Book(models.Model):  
        """本モデル"""  
        class Meta:  d
            b_table = 'book'  
        title = models.CharField(verbose_name='タイトル', max_length=255)  authors = models.ManyToManyField(Author, verbose_name='著者')
    ```
    ![alt text](image-24.png)

- モデルマネージャ
    - DBのテーブルレベルのクエリ操作をするためのクエリセットAPIのインタフェースを持っている
    - モデルマネージャは通常、モデルクラスにobjectという属性で用意されている
    ```python
    from django.db import models
    class Publisher(models.Model):
        """出版社モデル"""
        class Meta:
            db_table = 'publisher'
        name = models.CharField(verbose_name='出版社名', max_length=255)
        def __str__(self):
            return self.name
    class Author(models.Model):
        """著者モデル"""
        class Meta:
            db_table = 'author'
        name = models.CharField(verbose_name='著者名', max_length=255)
        def __str__(self):
            return self.name
    class Book(models.Model):
        """本モデル"""
        class Meta:
            db_table = 'book'
        title = models.CharField(verbose_name='タイトル', max_length=255,  unique=True)
        publisher = models.ForeignKey(Publisher, verbose_name='出版社',  on_delete=models.PROTECT)
        authors = models.ManyToManyField(Author, verbose_name='著者')
        price = models.IntegerField(verbose_name='価格', null=True, blank=True)
        description = models.TextField(verbose_name='概要', null=True, blank=True)
        publish_date = models.DateField(verbose_name='出版日')
        def __str__(self):
            return self.title
    ```

    - 単体のモデルオブジェクトを取る
    ```python
    Book.objects.get(title='Djangoの本')
    ```
        - 検索結果が0の場合はモデルクラスのDoesNotExist例外発生する
        - 検索結果が2件以上の場合はMultipleObjectsReturnedが発生する
    - 複数のモデルオブジェクトを取得する
    ```python
    Book.objects.all() 
    ```
        - 検索結果が0の倍は例外は発生しない
    
    - filterメソッド
        ```python
        Book.objects.filter(title='Djangoの本')
        ```

        - AND条件
        ```python
        Book.objects.filter(title='Djangoの本', price=1500)
        ```

        - OR条件
        ```python
        Book.objects.filter(Q(title='Djangoの本') | Q(price=1000))
        ```

        - 「>」大なり
        ```python
        Book.objects.filter(price__gt=1000)
        ```
        - 「<」小なり
        ```python
        Book.objects.filter(price__lt=1000)
        ```
        - 「>=」
        ```python
        Book.objects.filter(price__gte=1000)
        ```
        - 「<=」
        ```python
        Book.objects.filter(price__lte=1000)
        ```
        - 「IN」
        ```python
        Book.objects.filter(price__in=[1000,1500])
        ```
        - 「LIKE」
        ```python
        Book.objects.filter(title__icontains='Django')
        ```

        - リレーション先のモデルを使った条件検索
            - 通常のSQLはJOINを使うが、DjangoでOneToOneField, ManyToManyFieldを使ってリレーションを定義している場合は簡単
            - 「__」でフィールド名と関連先のモデルのフィールド名を繋ぐだけで検索時にモデルの定義に従って自動的にJOINをしてくれる
            ```python
            Book.objects.filter(publisher__name='自費出版者')
            ```
            - これは以下のSQLと同じ
            ```sql
            SELECT
            *
            FROM book b
            INNER JOIN publisher p
            ON b.publisher_id = p.id
            WHERE p.name = ' 自費出版社';
            ```
    - exists
        - レコードが存在するかどうかをTrueかFalseで返す
        ```python
        >>> Book.objects.exists()  
        True  
        >>> Book.objects.filter(title__icontains='Flask').exists()
        False
        ```

    - count
        - レコードの件数を返します
        ```python
        >>> Book.objects.count()
        3
        >>> Book.objects.filter(price=1500).count()
        2
        ```
    - order_by