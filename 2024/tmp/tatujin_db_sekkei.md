## 良かったところ
『スッキリわかるSQL入門』で学んだ基礎をさらに深め、より深くなデータベース設計を学べた点が大きな収穫でした。特に、エンティティの生成から正規化、そしてER図の作成までのプロセスを体系的に理解できたことで、これまで何となく行っていたテーブル設計が、明確な理論に基づいたものに進化した点が大きな収穫かと思います。
また、SQLの文法をすでに学んでいたおかげで、例示されたSQL文もすらすらと理解でき、学習がスムーズに進みました。
また、私自身、DB設計の正規化について、「第何正規化」など意識せずとも、自然と正規化できるものでは？と思っていたのですが、理論家クリスデイトの以下の引用の記載があり勉強になりました。
```
前述のように、正規化理論は基本的に常識にすぎないという理由で批判されてきた。......
優秀な設計者であれば、たとえBCNFの予備知識がなかったとしても、この関係変数を射影SPとSSに「自然に」分解するだろう。
だが、「自然に」とはどういう意味なのか。設計者は「自然な」設計を選ぶうえで、どのような「原理」を用いているのだろうか？
答えは、正規化の原理とまったく同じである。つまり、優秀な設計者は、そうした原理を正式に学んだことがなくても、その名前を言い当てることができなくても、そうした原理がすでに頭の中にある。
したがって、原理は確かに常識だが、それらは正規化された常識なのである。正規化を批判する人は、そういった点を見逃している。
それらの概念が実は常識すぎることを（実際はもっともらしく）主張するが、結して、常識の意味を正確かつ形所に述べることができない。
```
今回学んだ理論は非常に学びになったと感じましたが、実際に業務で設計を行うことで、知識が定着し、実践的なスキルとして身につくのかなと感じました。
## 悪かったところ(もしあれば)

## 学んだこと
### 1章
- スキーマ(schema) もともとは「枠組みをもった計画」という意味のギリシア語が語源
  - 古代ギリシャ語の「σχημα」から来ている。意味は「形」や「見かけ」で、現代英語では、構想やモデルを表す用語としても使われています。
  - ある特定の事柄や概念に関する知識や情報の「枠組み」や「構造」を指す。これは、過去の経験や知識を基にして、新しい情報を処理したり、問題を解決したりする際に使う。例えば、スキーマを持っている人は、情報を理解しやすくなったり、問題を迅速に解決できる可能性が高くなる。
  - スキーマには3種類存在する
  - 外部スキーマ
    - データベースの構造（スキーマ）のうち、ユーザー側やアプリケーション側から見た構造のこと
    - データベースから必要なデータを抽出した結果
    - 外部スキーマを「ビュー」と呼ぶ。MVCモデルの概念における「ビュー」はユーザーが操作する見た目の画面だが、SQLのビューはユーザーが見やすいようにデータベースから抽出したデータ。
    - ユーザーがデータベースと対話する際のインタフェースを提供する部分で、ビュー層とも呼ばれています。
    - 実際にアプリを使うユーザが見る多くの画面の中で、どの画面にDB上のどのデータが必要でどのデータが不要かを設計すること。
  - 概念スキーマ
    - 開発者から見たデータベースのこと。主にER図のこと。
    - 概念スキーマの設計を「論理設計」ともいう
    - 概念スキーマの具体的な要素として「エンティティ」「属性」「リレーションシップ」「製約」がある。
        - エンティティ
            - ERで書くあの矢印で繋がってる四角箱。
            - 基本的にDBテーブル単位
            - 「顧客」「注文」「製品」などなど
        - 属性
            - エンティティが持つプロパティのこと。言い換えるとDBテーブルが持つカラムのこと
            - 「顧客ID」「名前」「住所」などなど
        - リレーションシップ
            - ER図でいうところの１体１とか１体多とかの話のこと
        - 制約
            - DBテーブルのDDL描くときのあの制約のこと。
            - 「主キー」「not null」「check」「unique」などなど
  - 内部スキーマ
    - データを一元管理する層。データベースサーバのこと。「物理設計」ともいう
    - 正直あんまピンときてない。
  - エンジニアの端くれになろうと思うのなら、マニュアルを熟読することを面倒に思ってはいけない。
### 2章
- 概念スキーマ＝論理設計。論理＝論理手して筋が通っているという意味ではなく、物理層の制約にとらわれないということ。DBの実装レベルの話が無関係であること
    - 物理層の設計とは、DBサーバーのCPUパフォーマンス、ストレージ（一般的にはハードディスク）のデータ格納場所、使っているDBMSのデータ型やSQL構文などの具体的かつ実装的なレベルの条件のこと。

- 論理設計のステップは→エンティティの抽出→エンティティの定義→正規化→ER図の作成
    - エンティティとは
        - 現実世界におけるデータの集合体。「顧客」「社員」「店舗」など実態を伴うものと、「税」「注文履歴」「操作履歴」など、物理的な実態を伴わない概念としてのエンティティがある。
    - エンティティの抽出
        - 作るシステムにどのようなエンティティ(＝データ)が必要かを考える工程。どういうデータを扱う必要があるか。なので顧客やシステム利用者と要件を詰めていく必要がある。
    - エンティティの定義
        - 各エンティティ（＝テーブル）が実際にどのような値（カラム）を持つのかを定義すること。
    - 正規化
        - エンティティについてシステムの利用がスムーズになるようにすること
        - 論理設計の結果を受けて、データを格納するための具体的な格納方法や領域を決めること。
- 物理設計のステップは「テーブル定義」「インデックス定義」「ハードウェアのサイジング」「ストレージでの冗長構成決定」「ファイルの物理配置決定」
    - テーブル定義
        - 論理設計で作った概念スキーマ（＝ER図）をもとにDBMS内に格納するテーブルの単位に変換していくこと。
        - 論理設計で作ったものを論理モデルといい、物理設計でのモデルを物理モデルという。
    - インデックス定義
        - ほんの索引で探した方がより早く探せるようにインデックスを定義する
    - ハードウェアのサイジング
        - システムで使うデータサイズを見積り、それに十分な容量のストレージを選定すること(キャパシティ)。サービス終了時のデータ量を見積もってストレージの容量を選定
        これには2つのアプローチがあり、安全に余裕を持ったサイジングをするか、もしくは容量が足りなくなっても簡単に容量を追加できる構成にしておく方法。
        後者のような容量が足りなくなった時など、負荷のかかった時に柔軟にシステムを増強できる性質を拡張性という
        - またシステムが十分に動くだけのCPUやメモリのスペックの選定など(パフォーマンス)
            -性能要件→システムなどが期待通りに動作するための必要なパフォーマンスの条件や基準のこと。これには主に2つの指標がある
                - 処理時間（何秒以内に処理が終了すること）＝どれだけ早いか
                - スループット（単位時間あたりにどれだけの量の処理をこなせるか）＝どれだけ多いか
    - ストレージでの冗長構成決定
        - 冗長＝何か障害があった時のために余裕を持たせておくこと。
        - 可能な限り高い障害耐性を持つシステムを構築する必要がある
        - ここで出てくるのがRAID
            - RAID→redundant array of independent disks（独立したディスクの冗長配列）
            - 基本の考え方は複数のディスクにデータを書き込むようにして、一方のディスクが壊れても残りのディスクが生きていればデータを失わないように保全するというもの
                -  RAID0→データを複数のディスクにバラバラに保存すること
                    データがいろんなディスクに分散してるので、こっちのディスクのデータを処理する間に同時にこっちのディスクに入ってるデータを処理したりできる→同時に処理が進む。→処理速度アップする。ただディスクがどれか1つでも壊れると復元できない。冗長性ゼロ。 逆にもし1つのディスクし火なくてそこに全部データ入ってたら、どのデータを処理するにしてもその1つのディスクに行くので、順番に処理を待つ必要がある。
                - RAID1
                    特徴はミラーリング。同じデータをもう一つのディスクにまるまるコピーする。なのでかたほうのディスクが壊れてもデータを復元できる。譲渡ゆ性が２倍。しかし書き込みの際にディスクを2つとも毎秋更新しないといけないので処理速度は微妙になる
                - RAID5→最低ディスク３本で構成する。ディスクが壊れても残ったディスクとパリティがあれば復元できる。
                - RAID10 RAID0,RAID1のいいとこどり。
- 可用性とは、システムやネットワーク、データなどが障害なく継続的に稼働し、必要なときにいつでも利用できる状態であることを指します可用性を示す指標として稼働率（%）が用いられ、システムが稼働している時間と停止している時間の割合で算出されます。たとえば、あるシステムが10時間稼働し、そのうち7時間が稼働していた場合、稼働率は70%となる。

- クラスタシステムとは、複数のサーバをネットワークで接続して1つのシステムとして運用するシステム
のこと。サーバの冗長システム構築することをクラスタリングと言う。
クラスタシステムでは、１台のサーバで障害が発生しても、別のサーバで通信を継続して処理できるのでシステム全体が停止することなく、障害復旧の交換作業中などの間もサービスを提供することができる。 クライアントからは１代のマシンに見えるようにする。クラスタの前にロードバランさを挟み、クラスタ内のノードらに分散させて処理する。クラスタリングは、水平方向のスケーリングとも呼ばれる。需要が増加した場合、クラスタにサーバを追加するだけで、ロード バランサがより大きなサーバ グループにリクエストを分散する。対照的に、垂直方向のスケーリングではサーバを、1秒あたり大量のリクエストを処理できる、より強力なサーバに置き換える必要があり、通常は高価なアプローチになる。
### 3章
- テーブル名は英語ならば複数形、複数名詞でかける。
- 外部キーは人間の親子関係と同じ。外部キーを持つテーブルが子。参照先が親。親のテーブルはこの状態を機にすることなく変更可能。
- 第一正規形（＝１セルにつき値は1つ）
    - 1つのセルには1つのデータしか含まない（スカラ値という。）
    - １テーブルには１エンティティ分の情報を入れるべき。
    - なぜRDBではスカラ値しか許されないか。
        - 主キーを定義しても各列を一意に特定できないため。
    - 関数従属性
        - ある値が決まれば他の値も決まること。y=axのようなこと
    - 以下のような非正規形から

    | 顧客名 | 商品名 | 購入日 |
    | ---- | ---- | ---- |
    | 田中 | 本, ノート, ペン | 2024-09-10 |
    | 鈴木 | パソコン | 2024-09-11 |
    | 佐藤 | ノート,ぺん | 2024-09-12 |

    - 以下のような第一正規形に変えることで、「田中さん」「佐藤さん」の行が分割されて、それぞれの商品が個別の行として特定できる

    | 顧客名 | 商品名 | 購入日 |
    | ---- | ---- | ---- |
    | 田中 | 本 | 2024-09-10 |
    | 田中 | ノート | 2024-09-10 |
    | 田中 | ペン | 2024-09-10 |
    | 鈴木 | パソコン | 2024-09-11 |
    | 佐藤 | ノート | 2024-09-12 |
    | 佐藤 | ペン | 2024-09-12 |

- 第二正規形(=部分従属を解消し完全従属のみのテーブルを作る)
    - 複合キーに対してそれ以外の列が片方のキーにしか従属しない場合は部分従属になってしまうのでそれらを完全従属に修正する
    - 部分従属の解消にはテーブルの分割
    - 言い換えると異なるエンティティはテーブルレベルでもちゃんと分離すること
    - 第２正規化のメリット
        - 部分従属が残ったままだと、修正箇所が増える（＝データ変更時に修正箇所が多くなる＆情報のご登録が発生）点を防げる
    - 正規化の逆操作は結合処理

    - 第二正規形に準拠していないテーブルの例
        - このテーブルでは、主キーである「顧客名＋商品名」に対して、「顧客住所」が「顧客名」にのみ依存しており、部分関数従属が発生しています。


    | 顧客名 | 商品名  | 購入日   | 顧客住所      |
    |--------|---------|----------|---------------|
    | 田中   | 本      | 2024-09-10 | 東京都渋谷区   |
    | 田中   | ノート  | 2024-09-10 | 東京都渋谷区   |
    | 田中   | ペン    | 2024-09-10 | 東京都渋谷区   |
    | 鈴木   | パソコン| 2024-09-11 | 東京都新宿区   |
    | 佐藤   | ノート  | 2024-09-12 | 東京都品川区   |
    | 佐藤   | ペン    | 2024-09-12 | 東京都品川区   |


    - 2NFに準拠させたテーブル

    1. **顧客情報テーブル**

    | 顧客名 | 顧客住所      |
    |--------|---------------|
    | 田中   | 東京都渋谷区   |
    | 鈴木   | 東京都新宿区   |
    | 佐藤   | 東京都品川区   |

    2. **購入記録テーブル**

    | 顧客名 | 商品名  | 購入日   |
    |--------|---------|----------|
    | 田中   | 本      | 2024-09-10 |
    | 田中   | ノート  | 2024-09-10 |
    | 田中   | ペン    | 2024-09-10 |
    | 鈴木   | パソコン| 2024-09-11 |
    | 佐藤   | ノート  | 2024-09-12 |
    | 佐藤   | ペン    | 2024-09-12 |

    部分従属を排除し、非キー属性が主キー全体に完全に依存するようにしました。!

- 第三正規形
    - 推移的関数従属
        - 2段階の関数従属があること

    ### 第三正規形に準拠していないテーブルの例

    | 顧客名 | 顧客住所      | 郵便番号    |
    |--------|---------------|-------------|
    | 田中   | 東京都渋谷区   | 150-0001    |
    | 鈴木   | 東京都新宿区   | 160-0022    |
    | 佐藤   | 東京都品川区   | 140-0014    |

    このテーブルでは、「郵便番号」が「顧客住所」に依存していますが、「顧客住所」は主キーではありません。これが推移的従属の例です。

    ### 第3正規形に準拠させたテーブル

    1. **顧客情報テーブル**

    | 顧客名 | 顧客住所      |
    |--------|---------------|
    | 田中   | 東京都渋谷区   |
    | 鈴木   | 東京都新宿区   |
    | 佐藤   | 東京都品川区   |

    2. **住所情報テーブル**

    | 顧客住所      | 郵便番号    |
    |---------------|-------------|
    | 東京都渋谷区   | 150-0001    |
    | 東京都新宿区   | 160-0022    |
    | 東京都品川区   | 140-0014    |

    非キー属性が他の非キー属性に依存しないように、住所情報を分離。

### 4章
- テーブル（エンティティ）の数が増えていくと、テーブル同士の関係がわかりにくくなり、設計に支障をきたす。この問題を解決するために、テーブル同士の関係を記述する道具がER図
- 「多対多」を「１対多」に分解するときに必要になるエンティティを「関連実態」と呼ぶ

### 5章
- 正規化の功罪
    - 正規化＝情報を複数のテーブルに分割させる行為なので、単独のテーブルだけでは必要な情報を取れることが少なくなる
    - よってSQL結合処理を行うのだが、結合は非常にコストの高い操作。結合するテーブル数、およびテーブルのレコード数が増えるほど処理時間がかかる。正規化によるパフォーマンスの劣化の多くがこの結合によるもの。
    - 正規化とパフォーマンスは強いトレードオフの関係にあるとおいうこと。つまり厳しく正規化すればパフォーマンスが悪化し、パフォーマンスを求め非正規化すれば、データの不整合が起きやすくなる。
    - 非正規化はあくまでも最後の手段。つまり十分に正規化された設計を諦めて良いのはパフォーマンスを向上させるためその他全ての戦略が要件を見たさない時だけ。
    - 逆に言うと最初は必ず正規するべきと言うこと。正規化の字数は高ければ高いほどよい。そして、他手段でのパフォーマンス改善が図れないかを検討し、最後の手段として正規化は行う。

- 正規化以外の冗長性排除よるパフォーマンス劣化
    - サマリデータの冗長性排除によるパターン
        - ## 受注(ver1.0)
        | 受注ID | 受注日       | 注文者名義  |
        |--------|--------------|-------------|
        | 0001   | 2012-01-05   | 岡野 徹     |
        | 0002   | 2012-01-05   | 浜田 健一   |
        | 0003   | 2012-01-06   | 石井 慶子   |
        | 0004   | 2012-01-07   | 若山 みどり |
        | 0005   | 2012-01-07   | 庄野 弘一   |
        | 0006   | 2012-01-11   | 若山 みどり |
        | 0007   | 2012-01-12   | 岡野 徹     |

        - ## 受注明細(ver1.0)

        | 受注ID | 受注明細連番 | 商品名           |
        |--------|--------------|------------------|
        | 0001   | 1            | マカロン         |
        | 0001   | 2            | 紅茶             |
        | 0001   | 3            | チョコ詰め合わせ |
        | 0002   | 1            | 日本茶           |
        | 0002   | 2            | ティーポット     |
        | 0002   | 3            | 米               |
        | 0003   | 1            | 米               |
        | 0004   | 1            | アイロン         |
        | 0004   | 2            | ネクタイ         |
        | 0005   | 1            | チョコ詰め合わせ |
        | 0005   | 2            | 紅茶             |
        | 0005   | 3            | クッキーセット   |
        | 0006   | 1            | 牛肉             |
        | 0006   | 2            | 鍋セット         |
        | 0007   | 1            | 米               |
        - 上記のテーブルで、「受注日ごとの注文された商品数」を取得したいとき以下のSQLになる
        ```sql
        select 受注.受注日
               COUNT(*) AS 注文数
        FROM 受注
        INNER JOIN 受注明細 ON 受注.受注ID = 受注明細.受注ID
        GROUP BY 受注.受注日
        ```
        - こちら結合をしているためパフォーマンス悪化。実際の運用では注文明細テーブルのレコード数は受注件数✖️注文１件あたりの平均商品数分のレコードができる。ので現実的には非常に大きなテーブルを結合吸うことになりパフォーマンスに影響が出る
        - そこで、受注テーブルに商品数(日毎の注文商品数をsumしたもの)
        - ## 受注(ver1.1)
        | 受注ID | 受注日       | 注文者名義  | 商品数|
        |--------|--------------|-------------|-------|
        | 0001   | 2012-01-05   | 岡野 徹     |7
        | 0002   | 2012-01-05   | 浜田 健一   |7
        | 0003   | 2012-01-06   | 石井 慶子   |1
        | 0004   | 2012-01-07   | 若山 みどり |5
        | 0005   | 2012-01-07   | 庄野 弘一   |5
        | 0006   | 2012-01-11   | 若山 みどり |2
        | 0007   | 2012-01-12   | 岡野 徹     |1

        - 商品数というサマリデータカラムを追加することで冗長性を持たせることで結合処理は必要なくない理、パフォーマンスが向上する。（推移的関数従属性ができてしまうので第３正規系ではなくなる。受注ID→受注日→商品数）

    - 選択条件の冗長性排除によるパターン
        - 上記の「受注(ver1.0)」と「受注明細」テーブルで受注日が2012-01-06 ~ 2012-01-07の期間に注文された商品の一覧を出力する場合
        ```sql
        select 受注.受注ID
               商品明細.商品名
        FROM 受注
        INNER JOIN 受注明細 ON 受注.受注ID = 受注明細.受注ID
        WHERE 受注.受注日 BETWEEN '2012-01-06' AND '2012-01-07';
        ```
        - こちらのSQL文は問題の解としては問題ないがやはり結合しているためコストが高い。
        - ## 受注明細(ver1.1)

        | 受注ID | 受注明細連番 | 商品名           |受注日|
        |--------|--------------|------------------|---------|
        | 0001   | 1            | マカロン         |2012-01-05
        | 0001   | 2            | 紅茶             |2012-01-05
        | 0001   | 3            | チョコ詰め合わせ |2012-01-05
        | 0002   | 1            | 日本茶           |2012-01-05
        | 0002   | 2            | ティーポット     |2012-01-05
        | 0002   | 3            | 米               |2012-01-05
        | 0003   | 1            | 米               |2012-01-06
        | 0004   | 1            | アイロン         |2012-01-07
        | 0004   | 2            | ネクタイ         |2012-01-07
        | 0005   | 1            | チョコ詰め合わせ |2012-01-07
        | 0005   | 2            | 紅茶             |2012-01-07
        | 0005   | 3            | クッキーセット   |2012-01-07
        | 0006   | 1            | 牛肉             |2012-01-11
        | 0006   | 2            | 鍋セット         |2012-01-11
        | 0007   | 1            | 米               |2012-01-12

        - 上記のテーブルに変更することで、先ほどのSQLから結合は必要なくなる。
        - ただ、受注ID→受注日という部分従属性が生まれてしまうため第２正規形ではなくなってしまう。
        - このように非正規化をすることで検索のパフォーマンスの向上が図れる
        - ただ、大原則として正規化は可能な限り高次元にしておくものであり、非正規化によるパフォーマンス向上は最後の手段である。
    
    - 上記の非正規化によるリスクを見ていく
        - 更新時のデータ不整合はご存知だろう
        - 更新時のパフォーマンス
            - 受注テーブルに商品数のカラムを追加したため、受注テーブルに新規レコードを毎登録する際は毎回商品数カラムの値を集計して登録する必要がある。
            - また、受注したとしても、ユーザが注文キャンセルした場合のことも考えると、受注テーブルへの定期的な更新が必要。こうした更新処理の負荷も考慮する必要がある。
### 6章
- インデックス
    - インデックスを使うと、本の索引のように欲しいデータに直接アクセスできる
    - SQLには「欲しいデータ」を書き「データ取得の道筋」は書かない。(whatを書きhowは書かない)
    - そういう意味で、SQLはカーナビと同じである。（「ここにいきたい」を入力するだけで、どのようにいくかはお任せ。）
    - SQLではデータの取得経路は自動で決定してくれる
    - このようのその存在をユーザが意識しなくても良い、でもそれがないとダメな「空気」のような性質を「透過性」という。

- B-tree
    - 頻繁に利用するインデックスの１種
    - B-treeの長所は平均点の高さ
    - インデックスを付与するテーブルは大規模なものにする（１万レコードが目安）
    - インデックスを作成するときはカーディなりの高い列に対して作成する
        - カーディナリ = その列に入る値の種類（性別列なら「男」「女」「その他」があルのでカーディなりティは３。日にちであれば１年で３６５のカーディなりティがある）
        - その列を指定したときに5%未満に絞れるカーディなリティの列にインデックス付与
        - ただし値が平均的に分散していることが前提
- オプティマイザと実行計画
    - テーブルにアクセスする流れは以下の通り
        - ユーザがSQL実行 → パーサ → オプティマイザ →カタログマネージャ→ オプティマイザ →テーブル
        - パーサ(parser)とはSQLが適切な構文であるかどうかチェックする
        - オプティマイザとはSQLのアクセスパス、すなわち実行計画を決めるもの
        - 実行計画を決める際に必要になるのが統計情報。オプティマイザはカタログマネージというモジュールに統計情報の照介をかける。統計情報を受け取ったのち、たくさんの経路の中から最適な最短経路を選択する
        - 
- ビットマップインデックスとハッシュマップインデックス
    - 以下のサイトの解説がわかりやすい
    - https://zenn.dev/suzuki_hoge/books/2022-12-database-index-9520da88d02c4f/viewer/6-others


### 7章
- 論理設計のやってはいけない(真似してはいけない設計のパターン)
    - `プログラミングというのは設計をプログラミング言語に翻訳する作業`であって、プログラミング自身がシステムの品質を左右するすることは相対的に稀
    - `戦略の失敗を戦術で取り返すことはできない`
    - 配列方による非スカラ値
        - 以下、配列型の値のあるINSERT文
        ```sql
        INSERT INTO 〇〇 VALUES('0001', '田中', '{太郎,花子,梅子}')
        ```
        - 配列型は利用しない。第一正規形を守る
    - ダブルミーニング
        - 1つの列に複数の意味の値を入れてしまうこと
        | 年度 | 社員名 | 列１ |列２|
        |--------|--------------|------------------|---------|
        | 2001   | 田中            | 178         |67
        | 2001   | 石井         | 167            |48
        | 2001   | 大塚         | 166 |52
        | 2001   | 八神         | 182           |21
        | 2001   | 江部洲本          | 158    |33
        | 2001   | 泊            | 155             |22
        - 列１は大体「身長」の列だとわかる。第２の列は体重列、、と思いきや違う。年齢？？
        といった感じで途中で意味が変わっている
    
    - 単一参照
        - ### 単一参照テーブル（雑多なコード体系の寄せ集め）

        | コードタイプ | コード値  | コード内容  |
        |--------------|-----------|-------------|
        | comp_cd      | C0001     | A 商事       |
        | comp_cd      | C0002     | B 化学       |
        | comp_cd      | C0003     | C 建設       |
        | comp_cd      | C9999     | Z 興業       |
        | pref_cd      | 01        | 北海道       |
        | pref_cd      | 02        | 青森         |
        | ...          | ...       | ...          |
        | pref_cd      | 47        | 沖縄         |
        | sex_cd       | 0         | 不明         |
        | sex_cd       | 1         | 男           |
        | sex_cd       | 2         | 女           |
        | sex_cd       | 9         | 適用不能     |

        - 同じ構造のマスタテーブルを一緒のテーブルにまとめちゃえばいいじゃん！な発想から生まれたバッドノウハウ
        - 上記のテーブルは3つの同じ構図のマスタ（どのマスタも「ID」＋「名称」の構造）を寄せ集めたもの
        - なんのマスタかを判別するのに「コードタイプ」列を追加している。
        - メリット
            - マスタが減るのでER図やスキーマがシンプルに。
            - コード検索のSQLの共通化が可能
        - デメリット
            - 各マスタのデータ構造（「ID」＋「名称」）は同じではあるが、値の文字列の長さマスタによって当たり前だが変わるので、余裕を持ってかなり大きめの可変長文字列の長さを指定する必要がある
            - ER図はシンプルにはなるが、可読性は下がる
            - 1つのテーブルに集約するので種類と数の多さによっては検索パフォーマンスが下がる
    - テーブル分割
        - 水平分割(レコード単位でテーブルを分割すること)
            ### 売上げ

            | 年度  | 会社コード | 売上げ (億円) |
            |-------|------------|---------------|
            | 2001  | C0001      | 50            |
            | 2001  | C0002      | 52            |
            | 2001  | C0003      | 55            |
            | 2001  | C0004      | 46            |
            | 2002  | C0001      | 52            |
            | 2002  | C0002      | 55            |
            | 2002  | C0003      | 60            |
            | 2002  | C0004      | 47            |
            | 2003  | C0001      | 46            |
            | 2003  | C0002      | 52            |
            | 2003  | C0003      | 44            |
            | 2003  | C0004      | 60            |
            
            - 上記のテーブルを以下のように分割すること

            ### 売上げ(2001)
            | 年度  | 会社コード | 売上げ (億円) |
            |-------|------------|---------------|
            | 2001  | C0001      | 50            |
            | 2001  | C0002      | 52            |
            | 2001  | C0003      | 55            |

            ### 売上げ(2002)
            | 年度  | 会社コード | 売上げ (億円) |
            |-------|------------|---------------|
            | 2002  | C0001      | 52            |
            | 2002  | C0002      | 55            |
            | 2002  | C0003      | 60            |
            | 2002  | C0004      | 47            |

            ### 売上げ(2003)
            | 年度  | 会社コード | 売上げ (億円) |
            |-------|------------|---------------|
            | 2003  | C0001      | 46            |
            | 2003  | C0002      | 52            |
            | 2003  | C0003      | 44            |
            | 2003  | C0004      | 60            |

            - SQLが常に１年毎にしか「売り上げ」テーブルにアクセスしないのであれば分割することでパフォーマンスの改善になる
            - 欠点
                - テーブルのスキーマを変更する必要がある場合、全ての年ごとのテーブルに対して更新する必要がある
                - 各年のテーブルに対して同じクエリを書くので、コードが冗長になる（同じようなクエリを複数回書く必要がある）、メンテナンス性が低下（クエリに修正が入る場合に多くの箇所を修正しないといけなくなる）
                - 毎年テーブルを新たに作る必要がある
### ８章
- グレーノウハウ
    - 主キーが決められない、または主キーとして不十分な場合
        - そもそも主キーになるような一意になるキーがない
        - 一意になるキーはあるが、サイクリック
        - 一意キーはあるが、途中で対象が変わる
    - サロゲートキー
        - surrogate · 代理人｛だいりにん｝、代行人｛だいこう にん｝
        - 実際のデータから導出されるものではなく、システムが人工的に生成する主キー。例えば、オートインクリメントの整数やUUIDなど
        - 一般的な原則としては極力代理キーの使用は避け、自然キーによる解決を図るべき。
        それは代理キーは論理的に不要なキーであるため。
        - オートナンバーを主キーに使う＝データモデルを書いている証拠
    - 自然キーにより解決するためには、「タイムスタンプ」「インターバル」を使って複合キーにするものがある。
    - オートナンバリングについて
        - シーケンスとID列ではシーケンスの方が柔軟で拡張性が高い。
        - 自動採番をアプリケーション側で実装するのは「車輪の再発明」なのでNG。
    - ビューを使用する際には、ビューの背後にデーブルが存在することを意識すること。この意識が薄いと、多段ビューによりパフォーマンス悪化につながる。
    - 多段ビューのような過度に複雑な作りはシステムをダメにする。「KISSの原則」に従うべき。
        - KISSの原則；Keep It Simple, Stupid.(単純にしておけ、馬鹿者)
    - データクレンジングの重要性
        - 一意キーの存在しないデータは不適切なキーを生むので、データクレンジングが必要。
        - 名寄せをサボると、ダブルマスタを産むのでデータクレンジングが必要

### 9章
- 隣接リストモデル
    - 同じテーブルの違うカラムの値を自己参照しているもの。（上司カラムにその社員の社員コードを設定している）
    ### 組織図
    | 社員 | 上司  |
    | ---- | ----- |
    | アダム | なし |
    | イブ | アダム |
    | セト | アダム |
    | カイン | セト |
    | アベル | セト |
    | ノア | セト |
    | ヨブ | カイン |

- 入れ子集合モデル
    - 各ノードを円とみなす。子の円は親の円の中に存在する。
    - ルートノード一番外側の円なので（left_valueが最小かつright_valueが最大のレコードがルートリーフ）
    - リーフノード＝自身の円の中に円が存在しない。
    ### 家電テーブル
    | id  | name            | left_value | right_value |
    |-----|-----------------|------------|-------------|
    | 1   | 家電            | 1          | 10          |
    | 2   | テレビ          | 2          | 5           |
    | 3   | スマートテレビ   | 3          | 4           |
    | 4   | 冷蔵庫          | 6          | 7           |
    | 5   | 洗濯機          | 8          | 9           |

    - ルートを求める
        ```sql
        select * from 家電テーブル
        where left_value=1;
        ```
    - リーフを求める
        ```sql
        
        ```

## 難しかったこと
- 正規化について
理論としては理解できたものの、どこまで細かく分割するかの判断は難しいと感じました。特に、正規化によってデータの冗長性は排除できるものの、パフォーマンスや可読性とのバランスをアプリケーションの要件を元に加減する必要のある点が難しいかなと感じました。

- ある程度業務で実際にやってみないとイメージがつきにくい部分があると思うので、業務経験を積む＆都度繰り返し読むようにするといいなと感じました
<!-- - 隣接リストモデル
    - データベースで 階層構造やツリー構造を表現する方法の一つ。特に、親子関係をシンプルに表現したいときに使われるモデル
    ### 家電テーブル
    | id  | name            | parent_id |
    |-----|-----------------|-----------|
    | 1   | 家電            | NULL      |
    | 2   | テレビ          | 1         |
    | 3   | 冷蔵庫          | 1         |
    | 4   | スマートテレビ   | 2         |
    | 5   | 洗濯機          | 1         |

- 入れ子集合モデル
    - ノードを円として表したもの左端が円の左端、右端が円の右端。
    ### 家電テーブル

    | id  | name            | left_value | right_value |
    |-----|-----------------|------------|-------------|
    | 1   | 家電            | 1          | 10          |
    | 2   | テレビ          | 2          | 5           |
    | 3   | スマートテレビ   | 3          | 4           |
    | 4   | 冷蔵庫          | 6          | 7           |
    | 5   | 洗濯機          | 8          | 9           |

    - リーフノード＝子ノードが存在しない
    -  -->