
[TCP/IP＆ネットワークコマンド入門 サポートページ](https://nisim-m.github.io/tcpipcmdbook/) ～補足事項～
# HTTP3に対応したcurlコマンド

構築方法の解説は本書の範囲を超えるため、手順紹介のみ行っています。

<!-- TOC -->
1. [概要と方針](#概要と方針)
   1. [git clone 実行時の方針](#git-clone-実行時の方針)
2. [開発ツールをインストール](#開発ツールをインストール)
3. [quictlsの取得と構築](#quictlsの取得と構築)
4. [nghttp2の取得と構築](#nghttp2の取得と構築)
5. [nghttp3の取得と構築](#nghttp3の取得と構築)
6. [ngtcp2の取得と構築](#ngtcp2の取得と構築)
7. [curlの取得と構築](#curlの取得と構築)
   1. [作成したcurlコマンドの確認](#作成したcurlコマンドの確認)
<!-- /TOC -->

## 概要と方針

「HTTP3 (and QUIC)」（URL：<a href="https://github.com/curl/curl/blob/master/docs/HTTP3.md">https://github.com/curl/curl/blob/master/docs/HTTP3.md</a>、以下“ドキュメント”と呼ぶ）に従い構築する。

- ngtcp2版で構築
- http2とhttp3を有効にしたい
- `<somewhere1>`は `/opt/curl` とする
- 作業フォルダを作成し、その中でソースを取得して構築、今回はホームディレクトリ下にworkというディレクトリを作成して使用

<div class="imgtitle">実行コマンド（curl用のディレクトリを作成、ホームディレクトリに移動してworkディレクトリを作成しworkディレクトリに移動）</div>
~~~
sudo mkdir /opt/curl
cd
mkdir work
cd work
~~~

~~~console:実行画面サンプル
$ sudo mkdir /opt/curl
[sudo] study のパスワード:	#パスワードを入力
$ cd            #ホームディレクトリに移動
$ mkdir work    #workディレクトリを作成
$ cd work       #workディレクトリに移る
~/work$         #プロンプトに~/workと表示されている、~はホームディレクトリを示す記号
~~~

### git clone 実行時の方針

`git clone プロジェクトのURL`でソースコードを取得して、ドキュメントに従って構築。

ドキュメントでは`git clone --depth 1 -b ～ http://github.com/～`としている。`--depth 1`は履歴一つ分（＝最新版）のみ取得するオプション。`-b`はブランチを指定するオプションで、ドキュメントではバージョンを指定してる。

- まずは最新版で試してみて、ダメだったらドキュメントにあるバージョンを使用する

<small>➡GitとGitHub、およびソースコードからの構築については開発「Linux＋コマンド入門」第5章「パッケージ管理 必要なモノを揃えられるようにしよう」の最後に取り上げているのでお持ちの方はご参照ください。ここで扱っている内容は“最初の1歩”なので、特に構築時の各コマンドやオプションの意味、なぜ必要なのかといった事柄については専門書をあたってください。Gitについては「Pro Git 2nd Edition」の日本語訳が <a href="https://git-scm.com/book/ja/v2">https://git-scm.com/book/ja/v2</a>でダウンロードできます。</small>

## 開発ツールをインストール

必要なパッケージ等を確認（まずは各サイトの概要で確認、エラーが出たらサイト内の詳細説明を見に行く）
- [https://github.com/quictls/openssl](https://github.com/quictls/openssl)
- [https://github.com/nghttp2/nghttp2](https://github.com/nghttp2/nghttp2) 「pkg-config」との記載あり
- [https://github.com/ngtcp2/nghttp3](https://github.com/ngtcp2/nghttp3)
- [https://github.com/ngtcp2/ngtcp2](https://github.com/ngtcp2/ngtcp2)「pkg-config、autoconf、automake、autotools-dev、libtool」
- [https://github.com/curl/curl](https://github.com/curl/curl)

実行コマンド
~~~
sudo apt install gcc make pkg-config autoconf automake autotools-dev libtool
~~~

（実行画面サンプル）
~~~console
$ sudo apt install gcc make pkg-config autoconf automake autotools-dev libtool
[sudo] study のパスワード:  #パスワードを入力してEnter
パッケージリストを読み込んでいます... 完了
依存関係ツリーを作成しています... 完了        
状態情報を読み取っています... 完了        
autoconf はすでに最新バージョン (2.71-2) です。
automake はすでに最新バージョン (1:1.16.5-1.3) です。
automake は手動でインストールしたと設定されました。
・・・
以下のパッケージが新たにインストールされます:
  libdpkg-perl libfile-fcntllock-perl make pkg-config
アップグレード: 0 個、新規インストール: 4 個、削除: 0 個、保留: 0 個。
499 kB のアーカイブを取得する必要があります。
この操作後に追加で 3,090 kB のディスク容量が消費されます。
続行しますか? [Y/n] y   #yと入力してEnter
取得:1 http://security.ubuntu.com/ubuntu jammy-security/main amd64 libdpkg-perl all 1.21.1ubuntu2.1 [237 kB]
取得:2 http://jp.archive.ubuntu.com/ubuntu jammy/main amd64 libfile-fcntllock-perl amd64 0.22-3build7 [33.9 kB]
取得:3 http://jp.archive.ubuntu.com/ubuntu jammy/main amd64 make amd64 4.3-4.1build1 [180 kB]
取得:4 http://jp.archive.ubuntu.com/ubuntu jammy/main amd64 pkg-config amd64 0.29.2-1ubuntu3 [48.2 kB]
499 kB を 2秒 で取得しました (275 kB/s)
・・・
pkg-config (0.29.2-1ubuntu3) を展開しています...
libfile-fcntllock-perl (0.22-3build7) を設定しています ...
make (4.3-4.1build1) を設定しています ...
libdpkg-perl (1.21.1ubuntu2.1) を設定しています ...
pkg-config (0.29.2-1ubuntu3) を設定しています ...
man-db (2.10.2-1) のトリガを処理しています ...
$ 
~~~

## quictlsの取得と構築

OpenSSLにQUICプロトコルのサポートを追加したフォーク（fork、既存プロジェクトから分岐して作られたプロジェクト）
<q>This is a fork of OpenSSL to enable QUIC.</q>

実行コマンド（ソースコード入手、入手したディレクトリに移動、構築してインストール、workディレクトリに戻る）
~~~
git clone --depth 1 https://github.com/quictls/openssl
cd openssl
./config enable-tls1_3 --prefix=/opt/curl
make
sudo make install
cd ..
~~~

（実行画面サンプル）
~~~console
~~~

## nghttp2の取得と構築

実行コマンド（ソースコード入手、入手したディレクトリに移動、構築してインストール、workディレクトリに戻る）
~~~
git clone --depth 1 https://github.com/nghttp2/nghttp2
cd nghttp2
autoreconf -fi
./configure --enable-lib-only --prefix=/opt/curl 
make
sudo make install
cd ..
~~~

（実行画面サンプル）
~~~console
~~~

## nghttp3の取得と構築

取得したnghttp3ディレクトリの中で`git submodule update --init`を実行してから構築している点に注意。
`git submodule update --init`はサブモジュールの更新と初期化を行うコマンド。

実行コマンド（ソースコード入手、入手したディレクトリに移動、構築してインストール、workディレクトリに戻る）
~~~
git clone --depth 1 https://github.com/ngtcp2/nghttp3
cd nghttp3
git submodule update --init
autoreconf -fi
./configure --enable-lib-only --prefix=/opt/curl
make
sudo make install
cd ..
~~~

（実行画面サンプル）
~~~console
~~~

## ngtcp2の取得と構築

実行コマンド（ソースコード入手、入手したディレクトリに移動、構築してインストール、workディレクトリに戻る）
~~~
git clone --depth 1 https://github.com/ngtcp2/ngtcp2
cd ngtcp2
autoreconf -fi
./configure PKG_CONFIG_PATH=/opt/curl/lib64/pkgconfig:/opt/curl/lib64/pkgconfig LDFLAGS="-Wl,-rpath,/opt/curl/lib64" --prefix=/opt/curl --enable-lib-only
make
sudo make install
cd ..
~~~

（実行画面サンプル）
~~~console
~~~

## curlの取得と構築

実行コマンド（ソースコード入手、入手したディレクトリに移動、構築してインストール、workディレクトリに戻る）
~~~
git clone --depth 1 https://github.com/curl/curl
cd curl
autoreconf -fi
LDFLAGS="-Wl,-rpath,/opt/curl/lib64" ./configure --with-openssl=/opt/curl --with-nghttp2=/opt/curl --with-nghttp3=/opt/curl --with-ngtcp2=/opt/curl
make
sudo make install
cd ..
~~~

（実行画面サンプル）
~~~console
~~~

### 作成したcurlコマンドの確認




----
[TCP/IP＆ネットワークコマンド入門 サポートページ](https://nisim-m.github.io/tcpipcmdbook/)
