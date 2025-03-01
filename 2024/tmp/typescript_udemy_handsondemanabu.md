## 1章
- https://www.udemy.com/course/ts-for-js-developers/learn/lecture/17754234#overview
    - TypeScript is a superset of JavaScript that compiles to clean JavaScript output.
        - superset = A set which includes another set or sets
        - 1つの集合あるイワ複数の集合を含む大きな集合

    ![alt text](../../image/image23.png)
    - javascriptでもできることはtypescriptでもできる
    - typescriptの開発元はマイクロソフト
    - 「:」はtypescriptの世界では型を指定するときに用いるもの
## 2章
- nodejsはjavascriptの実行環境のこと
- なぜnodejsがいるのか
    - typescriptのコードは素のtypescriptのコードのままで実行できない
    - なのでtsコードをjsに変換する・これをコンパイル・トランスパイルという。
    - これには専用のコンパイラのライブラリが必要。
    - そのライブラリの実行にnodejsの環境が必要

    - あとtsからjsに変換されたjsのコードを実行するためにも必要

    - LTS=「ｌong time support」といって、長期間サポートが付いてますよって、動作保証してますよって意味。推奨版。
    - 開発現場でCurrent使うことはほぼない

## 3章
- gitリポジトリ作成

## 4章
- package.json
    - これからtypescriptの開発環境を作る上で、Nodeのパッケージを入れていく。そこでｐackageの管理をやる必要があ流のだが、そのパッケージ管理をやってくれるのが、パッケージjson。

    - 「npm init 」コマンドでpackage.jsonを作成できる
    - 「npm info typescript」で現時点でのタイプスクリプトのバージョンの最新を確認
    ```txt
    $ npm info typescript

    typescript@5.7.3 | Apache-2.0 | deps: none | versions: 3357
    TypeScript is a language for application scale JavaScript development
    https://www.typescriptlang.org/

    keywords: TypeScript, Microsoft, compiler, language, javascript

    bin: tsc, tsserver

    dist
    .tarball: https://registry.npmjs.org/typescript/-/typescript-5.7.3.tgz
    .shasum: 919b44a7dbb8583a9b856d162be24a54bf80073e
    .integrity: sha512-84MVSjMEHP+FQRPy3pX9sTVV/INIex71s9TL2Gm5FG/WG1SqXeKyZ0k7/blY/4FdOzI12CBy1vGc4og/eus0fw==
    .unpackedSize: 22.7 MB

    maintainers:
    - typescript-bot <typescript@microsoft.com>
    - weswigham <wwigham@gmail.com>
    - sanders_n <nathan@shively-sanders.com>
    - andrewbranch <andrew@wheream.io>
    - minestarks <mineyalc@microsoft.com>
    - rbuckton <rbuckton@chronicles.org>
    - sheetalkamat <shkamat@microsoft.com>
    - typescript-deploys <typescript-design@microsoft.com>

    dist-tags:
    beta: 5.8.0-beta                          latest: 5.7.3                             tag-for-publishing-older-releases: 4.1.6
    dev: 3.9.4                                next: 5.9.0-dev.20250223
    insiders: 4.6.2-insiders.20220225         rc: 5.8.1-rc

    published a month ago by typescript-bot <typescript@microsoft.com>
    ```

    - 以下のコマンドでpackage.jsonにtypescriptを入れる
    ```txt
    $ npm install --save-dev typescript@5.7.3
    added 1 package, and audited 2 packages in 743ms

    found 0 vulnerabilities
    ```

    - package.jsonのdevDependenciesに以下の情報が追記された
    - って感じでpackage.jsonではどのようなパッケージを導入したか管理できる
    ```txt
    "devDependencies": {
        "typescript": "^5.7.3"
    }
    ```

    - tsc src/install-typescript.tsて感じでコンパイルする(ちなみにtsc=typescript compileって意味)
    - コンパイルするとinstall-typescript.jsが生成されている
    - 「node src/install-typescript.js」と打てばjsファイル実行できまっせ〜
    - 何気なく打っているtscコマンドの実態はなんなのか。-----------------------------------**記事①ここから**
        - 「where tsc」
            ```txt
            where tsc
            /usr/local/bin/tsc
            /usr/local/bin/tsc
            ```
            - なるほど、ここにあると。
        - とりあえず詳細情報を出してみる
            - 「ls -l /usr/local/bin/tsc」
            ```txt
            lrwxr-xr-x@ 1 root  wheel  38 Feb 23 19:57 /usr/local/bin/tsc -> ../lib/node_modules/typescript/bin/tsc
            ```
            - なるほど、シンボリックリンクが設定されていて、実際は../lib/node_modules/typescript/bin/tscのパスが実行されているみたいです
            - ls → ファイルやディレクトリの一覧を表示 するコマンド。
            - -l → 詳細情報（long format）を表示 するオプション。

            | フィールド | 説明 |
            |-----------|------|
            | `l` | **シンボリックリンク**（通常は `-` だが、`l` はリンクを意味する） |
            | `rwxr-xr-x@` | **パーミッション（アクセス権）**（後述） |
            | `1` | **ハードリンクの数**（通常ファイルなら1） |
            | `root` | **所有者（このファイルの持ち主）** |
            | `wheel` | **所有グループ**（macOSでは `wheel` は管理者権限） |
            | `38` | **ファイルサイズ（バイト単位）** |
            | `Feb 23 19:57` | **最終更新日時** |
            | `/usr/local/bin/tsc` | **シンボリックリンクの名前**（`tsc` コマンド） |
            | `-> ../lib/node_modules/typescript/bin/tsc` | **リンク先の実態** |
            -------------------------------------------------------------------------ここまで
        - 
- ts-node
    - tscコマンドでtsをコンパイルして、さらにjsをnodeコマンドで実行していたが、それを一発でやってくれるやつ

    ```txt
    ----------------------------------------------
    $ npm info ts-node

    ts-node@10.9.2 | MIT | deps: 13 | versions: 128
    TypeScript execution environment and REPL for node.js, with source map support
    https://typestrong.org/ts-node

    keywords: typescript, node, runtime, environment, ts, compiler

    bin: ts-node, ts-script, ts-node-cwd, ts-node-esm, ts-node-script, ts-node-transpile-only

    dist
    .tarball: https://registry.npmjs.org/ts-node/-/ts-node-10.9.2.tgz
    .shasum: 70f021c9e185bccdca820e26dc413805c101c71f
    .integrity: sha512-f0FFpIdcHgn8zcPSbf1dRevwt047YMnaiJM3u2w2RewrB+fob/zePZcrOyQoLMMO7aBIddLcQIEK5dYjkLnGrQ==
    .unpackedSize: 757.3 kB

    dependencies:
    @cspotcode/source-map-support: ^0.8.0 acorn-walk: ^8.1.1                    make-error: ^1.1.1
    @tsconfig/node10: ^1.0.7              acorn: ^8.4.1                         v8-compile-cache-lib: ^3.0.1
    @tsconfig/node12: ^1.0.7              arg: ^4.1.0                           yn: 3.1.1
    @tsconfig/node14: ^1.0.0              create-require: ^1.1.0
    @tsconfig/node16: ^1.0.2              diff: ^4.0.1

    maintainers:
    - blakeembrey <hello@blakeembrey.com>
    - cspotcode <cspotcode@gmail.com>

    dist-tags:
    beta: 11.0.0-beta.1  latest: 10.9.2

    published a year ago by blakeembrey <hello@blakeembrey.com>

    -----------------------------------------------------
    $ npm install --save-dev ts-node@10.9.2

    added 19 packages, and audited 21 packages in 3s

    found 0 vulnerabilities
    ```

    - npx ts-node　外貨のエラーで動かなかった。
    ```txt
    npx ts-node src/install-typescript.ts
    TypeError: Unknown file extension ".ts" for /Users/k_tanaka/hc/typescript-udemy-handsondemanabu/typescript-for-javascript-developers/src/install-typescript.ts
        at Object.getFileProtocolModuleFormat [as file:] (node:internal/modules/esm/get_format:218:9)
        at defaultGetFormat (node:internal/modules/esm/get_format:244:36)
        at defaultLoad (node:internal/modules/esm/load:122:22)
        at async ModuleLoader.loadAndTranslate (node:internal/modules/esm/loader:479:32)
        at async ModuleJob._link (node:internal/modules/esm/module_job:112:19) {
    code: 'ERR_UNKNOWN_FILE_EXTENSION'
    }
    ```
    - 以下サイトを参考にts-nodeではなく、tsxというパッケージを使うといけた。
    - https://qiita.com/shunii/items/1863bb65a463783b3213

- ts-node-dev
    - Compiles your TS app and restarts when files are modified.
    - ファイルが変更されたらソースをコンパイルし、それを実行してくれる

    - ts-node-dev実行時に以下エラー発生
    ```txt
    $ sudo npm install --save-dev ts-node-dev@2.0.0
    npm warn deprecated inflight@1.0.6: This module is not supported, and leaks memory. Do not use it. Check out lru-cache if you want a good and tested way to coalesce async requests by a key value, which is much more comprehensive and powerful.
    npm warn deprecated rimraf@2.7.1: Rimraf versions prior to v4 are no longer supported
    npm warn deprecated glob@7.2.3: Glob versions prior to v9 are no longer supported

    added 47 packages, and audited 73 packages in 900ms

    11 packages are looking for funding
    run `npm fund` for details

    found 0 vulnerabilities
    $ npx ts-node-dev --respawn src/install-typescript.ts
    [INFO] 01:44:47 ts-node-dev ver. 2.0.0 (using ts-node ver. 10.9.2, typescript ver. 5.7.3)
    Compilation error in /Users/k_tanaka/hc/typescript-udemy-handsondemanabu/typescript-for-javascript-developers/src/install-typescript.ts
    Error: Must use import to load ES Module: /Users/k_tanaka/hc/typescript-udemy-handsondemanabu/typescript-for-javascript-developers/src/install-typescript.ts
        at Object.<anonymous> (/Users/k_tanaka/hc/typescript-udemy-handsondemanabu/typescript-for-javascript-developers/src/install-typescript.ts:1:7)
        at Module.<anonymous> (node:internal/modules/cjs/loader:1546:14)
        at Module._compile (/Users/k_tanaka/hc/typescript-udemy-handsondemanabu/typescript-for-javascript-developers/node_modules/source-map-support/source-map-support.js:568:25)
        at Module.m._compile (/private/var/folders/3f/fn8dr_n5791c7wmj9txs5r6r0000gn/T/ts-node-dev-hook-8816110685144951.js:69:33)
        at node:internal/modules/cjs/loader:1689:10
        at require.extensions..jsx.require.extensions..js (/private/var/folders/3f/fn8dr_n5791c7wmj9txs5r6r0000gn/T/ts-node-dev-hook-8816110685144951.js:114:20)
        at require.extensions.<computed> (/private/var/folders/3f/fn8dr_n5791c7wmj9txs5r6r0000gn/T/ts-node-dev-hook-8816110685144951.js:71:20)
        at Object.nodeDevHook [as .ts] (/Users/k_tanaka/hc/typescript-udemy-handsondemanabu/typescript-for-javascript-developers/node_modules/ts-node-dev/lib/hook.js:63:13)
        at Module.load (node:internal/modules/cjs/loader:1318:32)
        at Function._load (node:internal/modules/cjs/loader:1128:12)
    [ERROR] 01:44:47 Error: Must use import to load ES Module: /Users/k_tanaka/hc/typescript-udemy-handsondemanabu/typescript-for-javascript-developers/src/install-typescript.ts

    ^C
    ```

    - 「 Must use import to load ES Module:」？
    - どうやらTypeScript の実行時に ES モジュール (ESM) と CommonJS (CJS) の違いによって発生しているみたい。
    - まあ要はimportが使えないぞって言われてる＝commonjsんモジュールを使う設定にしろって意味かなってことで、
    - package.json を見てみると
     "type": "module" を設定していた。。
    type:moduleでESMを使うせってになるらしい。
    - なのでcommonjsの設定にしてみる
    "type": "commonjs",
    - 再度動かし見てると
    - うまく動きました。
    ```txt
    $ npx ts-node-dev --respawn src/install-typescript.ts  ←ここ！！！
    [INFO] 01:47:12 ts-node-dev ver. 2.0.0 (using ts-node ver. 10.9.2, typescript ver. 5.7.3)
    { message: 'hello typescript' }
    [INFO] 01:47:35 Restarting: /Users/k_tanaka/hc/typescript-udemy-handsondemanabu/typescript-for-javascript-developers/src/install-typescript.ts has been modified
    { message: 'hello typescript' }
    ```

## 5章
- vscode自体の設定はjson形式で一括管理されている
    - setttings→preferrenceに行って検索窓に「settings json」と検索すれば　edit setings.jsonとかが出てくる
- tsconfig.json
    - https://www.typescriptlang.org/docs/handbook/tsconfig-json.html#handbook-content
    - 「tsc --init」でtsconfig.jsonを生成できる

## 6章
