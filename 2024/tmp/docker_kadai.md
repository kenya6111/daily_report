- 以下のリポジトリを開き、右上にある`Use this template`で`Create a new repository`を選択し、自分のgithubアカウント配下にリポジトリを作成
https://github.com/ihatov08/rails7_docker_template

- ローカルのディレクトリ（どこでも良い）にgit cloneを用いクローンする
  - git clone <リポジトリURL>

- 以下のファイルをあらかじめ作成しておく
    - Dockerfile
    - Dockerfile.lock
    - docker-compose.lock

- Dockerfileをエディタで開く
    - 以下の内容を記載
        -
- docker-compose.ymlを開く(このファイルはそもそもdocker runコマンドをする際にオプションが多くて毎回鬱コマンドが長くなってしまうので、そのオプション群をまとめてこのファイルに書けるようにしたもの。打つコマンドはdocker compose などだけで完結する。また、今回のように複数のコンテナ（dbやwebのコンテナ）を起動したいときにdocker composeを使う。)
    - 以下の内容を記載
        -
- クローンしたプロジェクト > config > database.ymlを開く
    - 以下のように修正

- 冒頭で作成したdocker-compose.ymlを以下のように修正
    -
- docker-composeのインストール


- docker-compose exec web bash
    - webコンテナ内に入る
- rails db:create
    - dbを作成する
- rails g scaffold product name:string price:integer vendor:string
    - tableを作成

- rails db:migrate
    - migrate実行

- docker-compose up -dコマンド実行
    - -dはデタッチモードでやるの意

- docker-compose upで起動後、脱出した後は、exitになっているので
docker-compose startをした後にdocker-compose exec web bashとやるとbash できそうできたりする

- 現状、composer-lockのweb側だけ書いて、無事docker-composer upだできた状態。localhost3000にアクセスするとDBにアクセスできないのでエラーが起きる状態。
- docker-compose.ymlのdbを書いてー、docker-compose upでどちらも起動できるようにして、画面に正しく描画されて〜って流れかな。

