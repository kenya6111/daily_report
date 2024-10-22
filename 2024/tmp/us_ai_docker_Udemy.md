## Linuxの基礎
- bash zsh sh→これらはShell(殻)。
- このshellを通じてカーネル(核)に命令を出す。
- ターミナルを使ってシェルに命令を出して、そのシェルがカーネルに命令を出す。
- なので、ターミナルはあくまでも bashとかzshとかのシェルを動かすためのアプリ。


- 環境変数というのは、OSの上で動くプロセスが情報を共有するための変数
- echo $SHELLで今使っているシェルが何かを確認できる(大体みんなbash。最近買った人はzsh)
```
echo $SHELL
/bin/zsh
```

- export で環境変数を作成できる
- これらのコマンドもシェルを通してカーネルに命令を出している。
```
export AGE=20
echo $AGE
20
```

- cd（current directly）
- pwd(print working directory)
