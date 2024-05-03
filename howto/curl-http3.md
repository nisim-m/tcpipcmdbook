
[TCP/IP＆ネットワークコマンド入門 サポートページ](https://nisim-m.github.io/tcpipcmdbook/) ～補足事項～
# HTTP3に対応したcurlコマンド

構築方法の解説は本書の範囲を超えるため、手順紹介のみ行っています。

<!-- TOC -->
1. [概要と方針](#概要と方針)

<!-- /TOC -->

## 概要と方針

「HTTP3 (and QUIC)」（URL：<a href="https://github.com/curl/curl/blob/master/docs/HTTP3.md">https://github.com/curl/curl/blob/master/docs/HTTP3.md</a>、以下“ドキュメント”と呼ぶ）に従い構築する。

- ngtcp2版で構築
- http2とhttp3を有効にしたい
- `<somewhere1>`は `/opt/curl` とする
- 作業フォルダを作成し、その中でソースを取得して構築、今回はホームディレクトリ下にworkというディレクトリを作成して使用

~~~
sudo mkdir /opt/curl
cd
mkdir work
cd work
~~~

~~~console
$ sudo mkdir /opt/curl
[sudo] study のパスワード:	←パスワードを入力
$ cd    #ホームディレクトリに移動
$ mkdir work
$ cd work
~/work$
~~~

### git clone 実行時の方針

`git clone プロジェクトのURL`でソースコードを取得して、ドキュメントに従って構築

ドキュメントでは`git clone --depth 1 -b ～ http://github.com/～`としている。`--depth 1`は履歴一つ分（＝最新版）のみ取得するオプション。`-b`はブランチを指定するオプションで、ドキュメントではバージョンを指定してる。

- まずは最新版で試してみて、ダメだったらドキュメントにあるバージョンを使用する


➡GitとGitHub、およびソースコードからの構築については開発「Linux＋コマンド入門」第5章「パッケージ管理 必要なモノを揃えられるようにしよう」の最後に取り上げているのでお持ちの方はご参照ください。ここで扱っている内容は“最初の1歩”なので、特に構築時の各コマンドやオプションの意味、なぜ必要なのかといった事柄については専門書をあたってください。Gitについては「Pro Git 2nd Edition」の日本語訳が <a href="https://git-scm.com/book/ja/v2">https://git-scm.com/book/ja/v2</a>でダウンロードできます。

## 開発ツールをインストール

必要なパッケージ等を確認
[https://github.com/quictls/openssl](https://github.com/quictls/openssl)
https://github.com/nghttp2/nghttp2 「pkg-config」
https://github.com/ngtcp2/nghttp3
https://github.com/ngtcp2/ngtcp2「pkg-config、autoconf、automake、autotools-dev、libtool」
https://github.com/curl/curl

~~~
sudo apt install gcc make pkg-config autoconf automake autotools-dev libtool
~~~

## quictlsの取得と構築

OpenSSLにQUICプロトコルのサポートを追加したフォーク（fork、既存プロジェクトから分岐して作られたプロジェクト）
<q>This is a fork of OpenSSL to enable QUIC.</q>

~~~
git clone --depth 1 https://github.com/quictls/openssl
cd openssl
./config enable-tls1_3 --prefix=/opt/curl
make
sudo make install
cd ..
~~~


## nghttp2の取得と構築

~~~
git clone --depth 1 https://github.com/nghttp2/nghttp2
cd nghttp2
autoreconf -fi
./configure --enable-lib-only --prefix=/opt/curl 
make
sudo make install
cd ..
~~~

## nghttp3の取得と構築

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

## ngtcp2の取得と構築

~~~
git clone --depth 1 https://github.com/ngtcp2/ngtcp2
cd ngtcp2
autoreconf -fi
./configure PKG_CONFIG_PATH=/opt/curl/lib64/pkgconfig:/opt/curl/lib64/pkgconfig LDFLAGS="-Wl,-rpath,/opt/curl/lib64" --prefix=/opt/curl --enable-lib-only
make
sudo make install
cd ..
~~~
## curlの取得と構築

~~~
git clone --depth 1 https://github.com/curl/curl
cd curl
autoreconf -fi
LDFLAGS="-Wl,-rpath,/opt/curl/lib64" ./configure --with-openssl=/opt/curl --with-nghttp2=/opt/curl --with-nghttp3=/opt/curl --with-ngtcp2=/opt/curl
make
sudo make install
cd ..
~~~

----
[TCP/IP＆ネットワークコマンド入門 サポートページ](https://nisim-m.github.io/tcpipcmdbook/)
