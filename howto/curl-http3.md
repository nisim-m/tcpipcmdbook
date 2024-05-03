
[TCP/IP＆ネットワークコマンド入門 サポートページ](https://nisim-m.github.io/tcpipcmdbook/) ～補足事項～
# HTTP3に対応したcurlコマンド

「4.4 HTTP/HTTPSの通信を見てみよう」内「参考：curlによるHTTP/3通信」（本文p.221）用
構築方法の解説は本書の範囲を超えるため、手順紹介のみ行っています。

<!-- TOC -->
1. [概要と方針](#概要と方針)
   1. [git cloneの方針](#git-cloneの方針)
2. [構築に必要なパッケージのインストール](#構築に必要なパッケージのインストール)
3. [ディレクトリの準備](#ディレクトリの準備)
4. [quictlsの取得と構築](#quictlsの取得と構築)
5. [nghttp2の取得と構築](#nghttp2の取得と構築)
6. [nghttp3の取得と構築](#nghttp3の取得と構築)
7. [ngtcp2の取得と構築](#ngtcp2の取得と構築)
8. [curlの取得と構築](#curlの取得と構築)
   1. [libcurl4のアンインストール](#libcurl4のアンインストール)
   2. [作成したcurlコマンドの確認](#作成したcurlコマンドの確認)
<!-- /TOC -->

## 概要と方針

<a href="https://github.com/curl/curl/blob/master/docs/HTTP3.md">「HTTP3 (and QUIC)」</a>、以下“ドキュメント”と呼ぶ）に従い構築する。<br />
[URL: https://github.com/curl/curl/blob/master/docs/HTTP3.md](https://github.com/curl/curl/blob/master/docs/HTTP3.md)

- ngtcp2版で構築
- http2とhttp3を有効にしたい
- ドキュメントの`--prefix`指定にある`<somewhere1～3>`は `/opt/curl` とする
- 作業フォルダを作成しその中でソースを取得して構築、今回はホームディレクトリ下に`work`というディレクトリを作成して使用

### git cloneの方針

`git clone プロジェクトのURL`でソースコードを取得して、ドキュメントに従って構築。

ドキュメントでは`git clone --depth 1 -b ～ http://github.com/～`としている。`--depth 1`は履歴一つ分（＝最新版）のみ取得するオプション。
`-b`はブランチを指定するオプションで、ブランチ名はプロジェクトによって異なるが、今回`git clone`の対象はブランチ名＝バージョンとなっており、
ドキュメントではブランチを指定することで構築に使用するバージョンを示している。

- まずは最新版（`-b`指定無し）で試してみて、ダメだったらドキュメントにあるバージョンを使用する

<small>➡GitとGitHub、およびソースコードからの構築については姉妹本<a href="https://gihyo.jp/book/2021/978-4-297-12024-5">「Linux＋コマンド入門」</a>第5章「パッケージ管理 必要なモノを揃えられるようにしよう」の最後に取り上げているのでお持ちの方はご参照ください。ここで扱っている内容は“最初の1歩”なので、特に構築時の各コマンドやオプションの意味、なぜ必要なのかといった事柄については専門書をあたってください。Gitについては「Pro Git 2nd Edition」の日本語訳が <a href="https://git-scm.com/book/ja/v2">https://git-scm.com/book/ja/v2</a>でダウンロードできます。</small>

## 構築に必要なパッケージのインストール

必要なパッケージ等を確認（まずは各サイトの概要で確認、エラーが出たらサイト内の詳細説明を見に行く）
- [https://github.com/quictls/openssl](https://github.com/quictls/openssl)
- [https://github.com/nghttp2/nghttp2](https://github.com/nghttp2/nghttp2) 「pkg-configが必要」との記載あり
- [https://github.com/ngtcp2/nghttp3](https://github.com/ngtcp2/nghttp3)
- [https://github.com/ngtcp2/ngtcp2](https://github.com/ngtcp2/ngtcp2)「pkg-config autoconf automake autotools-dev libtool」
- [https://github.com/curl/curl](https://github.com/curl/curl)

<div class="codetitle">実行コマンド</div>
~~~
sudo apt install gcc make pkg-config autoconf automake autotools-dev libtool
~~~

以下は実行している様子のサンプルです。実行時の参考にしてください。途中の「・・・」は省略を示しています（以下同）。

<div class="codetitle">実行画面サンプル</div>
~~~console
$ sudo apt install gcc make pkg-config autoconf automake autotools-dev libtool
[sudo] study のパスワード:  👈パスワードを入力してEnter
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
続行しますか? [Y/n]    👈Enterで実行（大文字＝デフォルトなのでEnterのみで y を選択した扱いになる）
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

## ディレクトリの準備

- curl用のディレクトリを作成
- ホームディレクトリに移動してworkディレクトリを作成し、workディレクトリに移動

<div class="codetitle">実行コマンド</div>
~~~
sudo mkdir /opt/curl
cd
mkdir work
cd work
~~~

以下、実行時のディレクトリ（カレントディレクトリ）は`~/work$`のように示します。`~`はホームディレクトリ、`~/work`はホームディレクトリ下のworkディレクトリを意味しています。
カレントディレクトリを問わず実行できる場合は`$`としています。

<div class="codetitle">実行画面サンプル</div>
~~~console
$ sudo mkdir /opt/curl
[sudo] study のパスワード:	👈パスワードを入力してEnter
$ cd            ←ホームディレクトリに移動
~$ mkdir work    ←workディレクトリを作成
~$ cd work       ←workディレクトリに移る
~/work$         ←プロンプトに~/workと表示されている、~はホームディレクトリを示す記号
~~~

## quictlsの取得と構築

quictlsはOpenSSLにQUICプロトコルのサポートを追加したフォーク（fork、既存プロジェクトから分岐して作られたプロジェクト）。
<q>This is a fork of OpenSSL to enable QUIC.</q>

処理の概要：
`git clone`でソースコードを入手、カレントディレクトリをソースコードのディレクトリに移し、構築前の設定を実行。
`make`で構築し`make instal`でインストールして、元のディレクトリ(~/work)に戻る。

<div class="codetitle">実行コマンド</div>
~~~
git clone --depth 1 https://github.com/quictls/openssl
cd openssl
./config enable-tls1_3 --prefix=/opt/curl
make
sudo make install
cd ..
~~~

<div class="codetitle">実行画面サンプル（ホームディレクトリ下のworkで実行）</div>
~~~console
~/work$ git clone --depth 1 https://github.com/quictls/openssl 👈ソースコードの取得
Cloning into 'openssl'...
remote: Enumerating objects: 24632, done.
remote: Counting objects: 100% (24632/24632), done.
remote: Compressing objects: 100% (19676/19676), done.
remote: Total 24632 (delta 1913), reused 20916 (delta 1500), pack-reused 0
Receiving objects: 100% (24632/24632), 22.32 MiB | 4.61 MiB/s, done.
Resolving deltas: 100% (1913/1913), done.
Updating files: 100% (24479/24479), done.
~/work$                 （opensslディレクトリが生成されている）
~/work$ cd openssl/     （opensslディレクトリへ移動）
~/work/openssl$ ./config enable-tls1_3 --prefix=/opt/curl  👈構築用の設定を実施
Configuring OpenSSL version 3.1.5+quic for target linux-x86_64
Using os-specific seed configuration
Created configdata.pm
Running configdata.pm
Created Makefile.in
Created Makefile
Created include/openssl/configuration.h

**********************************************************************
***                                                                ***
***   OpenSSL has been successfully configured                     ***
***                                                                ***
・・・
***                                                                ***
**********************************************************************
~/work/openssl$ make 👈構築
/usr/bin/perl "-I." -Mconfigdata "util/dofile.pl" "-oMakefile" include/crypto/bn_conf.h.in ・・・
/usr/bin/perl "-I." -Mconfigdata "util/dofile.pl" "-oMakefile" include/crypto/dso_conf.h.in ・・・
・・・
gcc  -I. -Iinclude -Iapps/include  -fPIC -pthread -m64 -Wa,--noexecstack -Wall -O3 -DOPENSSL_USE_NODELETE -DL_ENDIAN -DOPENSSL_PIC -DOPENSSLDIR="\"/opt/curl/ssl\"" -DENGINESDIR="\"/opt/curl/lib64/engines-81.3\"" -DMODULESDIR="\"/opt/curl/lib64/ossl-modules\"" -DOPENSSL_BUILDING_OPENSSL -DNDEBUG  -MMD -MF apps/lib/libapps-lib-app_libctx.d.tmp -MT apps/lib/libapps-lib-app_libctx.o -c -o apps/lib/libapps-lib-app_libctx.o apps/lib/app_libctx.c
gcc  -I. -Iinclude -Iapps/include  -fPIC -pthread -m64 -Wa,--noexecstack -Wall -O3 -DOPENSSL_USE_NODELETE -DL_ENDIAN -DOPENSSL_PIC -DOPENSSLDIR="\"/opt/curl/ssl\"" -DENGINESDIR="\"/opt/curl/lib64/engines-81.3\"" -DMODULESDIR="\"/opt/curl/lib64/ossl-modules\"" -DOPENSSL_BUILDING_OPENSSL -DNDEBUG  -MMD -MF apps/lib/libapps-lib-app_params.d.tmp -MT apps/lib/libapps-lib-app_params.o -c -o apps/lib/libapps-lib-app_params.o apps/lib/app_params.c
gcc  -I. -Iinclude -Iapps/include  -fPIC -pthread -m64 -Wa,--noexecstack -Wall -O3 -DOPENSSL_USE_NODELETE -DL_ENDIAN -DOPENSSL_PIC -DOPENSSLDIR="\"/opt/curl/ssl\"" -DENGINESDIR="\"/opt/curl/lib64/engines-81.3\"" -DMODULESDIR="\"/opt/curl/lib64/ossl-modules\"" -DOPENSSL_BUILDING_OPENSSL -DNDEBUG  -MMD -MF apps/lib/libapps-lib-app_provider.d.tmp -MT apps/lib/libapps-lib-app_provider.o -c -o apps/lib/libapps-lib-app_provider.o apps/lib/app_provider.c
・・・
/usr/bin/perl "-I." -Mconfigdata "util/dofile.pl" \
    "-oMakefile" util/wrap.pl.in ・・・
chmod a+x util/wrap.pl
make[1]: Leaving directory '/home/study/work/openssl'
~/work/openssl$ （「syntax error」や「～はありません」等のエラーが出ていなければOK）
~/work/openssl$ sudo make install   👈インストール
[sudo] study のパスワード:  👈パスワードを入力してEnter（直前に実行したsudoから一定時間内の場合は入力を求められない、以下同）
make depend && make _build_libs
make[1]: Entering directory '/home/study/work/openssl'
make[1]: Leaving directory '/home/study/work/openssl'
make[1]: Entering directory '/home/study/work/openssl'
make[1]: Leaving directory '/home/study/work/openssl'
created directory `/opt/curl/lib64'
*** Installing runtime libraries
・・・
install doc/html/man7/proxy-certificates.html ・・・
install doc/html/man7/ssl.html ・・・
install doc/html/man7/x509.html ・・・
~/work/openssl$         （エラーメッセージが出ていなければOK）
~/work/openssl$ cd ..   👈workディレクトリに戻る（「..」は親ディレクトリを表す記号）
~/work$
~~~

## nghttp2の取得と構築

作業の流れは先ほどと同じだが構築用の設定に使用しているコマンド及びオプションが異なる。

<div class="codetitle">実行コマンド</div>
~~~
git clone --depth 1 https://github.com/nghttp2/nghttp2
cd nghttp2
autoreconf -fi
./configure --enable-lib-only --prefix=/opt/curl 
make
sudo make install
cd ..
~~~

<div class="codetitle">実行画面サンプル（ホームディレクトリ下のworkで実行）</div>
~~~console
~/work$ git clone --depth 1 https://github.com/nghttp2/nghttp2 👈ソースコードを取得
Cloning into 'nghttp2'...
remote: Enumerating objects: 647, done.
remote: Counting objects: 100% (647/647), done.
remote: Compressing objects: 100% (561/561), done.
remote: Total 647 (delta 161), reused 379 (delta 71), pack-reused 0
Receiving objects: 100% (647/647), 1.20 MiB | 2.53 MiB/s, done.
Resolving deltas: 100% (161/161), done.
~/work$              （nghttp2ディレクトリが生成されている）
~/work$ cd nghttp2/  （nghttp2ディレクトリへ移動）
~/work/nghttp2$ autoreconf -fi   👈構築用の設定ファイルの準備
libtoolize: putting auxiliary files in AC_CONFIG_AUX_DIR, '.'.
libtoolize: copying file './ltmain.sh'
libtoolize: putting macros in AC_CONFIG_MACRO_DIRS, 'm4'.
・・・
configure.ac:41: installing './install-sh'
configure.ac:41: installing './missing'
Makefile.am: installing './INSTALL'
examples/Makefile.am: installing './depcomp'
parallel-tests: installing './test-driver'
~/work/nghttp2$ ./configure --enable-lib-only --prefix=/opt/curl  👈構築用の設定を実施
checking for gcc... gcc
checking whether the C compiler works... yes
checking for C compiler default output file name... a.out
・・・
    Third-party:
      http-parser:    no
      MRuby:          no (CFLAGS='' LIBS='')
      Neverbleed:     no
    Features:
      Applications:   no
      HPACK tools:    no
      Examples:       no
      Threading:      no
      HTTP/3 (EXPERIMENTAL): no

~/work/nghttp2$ make 👈構築
make  all-recursive
make[1]: ディレクトリ '/home/study/work/nghttp2' に入ります
Making all in lib
make[2]: ディレクトリ '/home/study/work/nghttp2/lib' に入ります
Making all in includes
・・・
Making all in script
make[2]: ディレクトリ '/home/study/work/nghttp2/script' に入ります
make[2]: 'all' に対して行うべき事はありません.
make[2]: ディレクトリ '/home/study/work/nghttp2/script' から出ます
make[2]: ディレクトリ '/home/study/work/nghttp2' に入ります
make[2]: ディレクトリ '/home/study/work/nghttp2' から出ます
make[1]: ディレクトリ '/home/study/work/nghttp2' から出ます
~/work/nghttp2$ sudo make install
[sudo] study のパスワード:  👈パスワードを入力してEnter
Making install in lib
make[1]: ディレクトリ '/home/study/work/nghttp2/lib' に入ります
Making install in includes
・・・
make[2]: 'install-exec-am' に対して行うべき事はありません.
 /usr/bin/mkdir -p '/opt/curl/share/doc/nghttp2'
 /usr/bin/install -c -m 644 README.rst '/opt/curl/share/doc/nghttp2'
make[2]: ディレクトリ '/home/study/work/nghttp2' から出ます
make[1]: ディレクトリ '/home/study/work/nghttp2' から出ます
~/work/nghttp2$ cd ..   👈workディレクトリに戻る
~/work$
~~~

## nghttp3の取得と構築

取得したnghttp3ディレクトリの中で`git submodule update --init`を実行してから構築している点に注意。
`git submodule update --init`はサブモジュールの更新と初期化を行うコマンド。

<div class="codetitle">実行コマンド（ソースコード入手、入手したディレクトリに移動、構築してインストール、workディレクトリに戻る</div>
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

<div class="codetitle">実行画面サンプル（ホームディレクトリ下のworkで実行）</div>
~~~console
~/work$ git clone --depth 1 https://github.com/ngtcp2/nghttp3
Cloning into 'nghttp3'...
remote: Enumerating objects: 154, done.
remote: Counting objects: 100% (154/154), done.
remote: Compressing objects: 100% (144/144), done.
remote: Total 154 (delta 48), reused 57 (delta 3), pack-reused 0
Receiving objects: 100% (154/154), 225.00 KiB | 2.74 MiB/s, done.
Resolving deltas: 100% (48/48), done.
~/work$              （nghttp3ディレクトリが生成されている）
~/work$ cd nghttp3/  👈nghttp3ディレクトリへ移動
~/work/nghttp3$ git submodule update --init  サブモジュールの取得と更新更新
Submodule 'lib/sfparse' (https://github.com/ngtcp2/sfparse) registered for path 'lib/sfparse'
Submodule 'tests/munit' (https://github.com/ngtcp2/munit) registered for path 'tests/munit'
Cloning into '/home/study/work/nghttp3/lib/sfparse'...
Cloning into '/home/study/work/nghttp3/tests/munit'...
Submodule path 'lib/sfparse': checked out '6e1572691c66bb6c7e7c784330f0c676164fbdde'
Submodule path 'tests/munit': checked out '7f53fea8901089d46233302b3af35bf8be93cfc5'
~/work/nghttp3$ autoreconf -fi     👈構築用の設定ファイルの準備
libtoolize: putting auxiliary files in AC_CONFIG_AUX_DIR, '.'.
libtoolize: copying file './ltmain.sh'
libtoolize: putting macros in AC_CONFIG_MACRO_DIRS, 'm4'.
libtoolize: copying file 'm4/libtool.m4'
libtoolize: copying file 'm4/ltoptions.m4'
libtoolize: copying file 'm4/ltsugar.m4'
libtoolize: copying file 'm4/ltversion.m4'
libtoolize: copying file 'm4/lt~obsolete.m4'
configure.ac:30: installing './compile'
configure.ac:33: installing './config.guess'
configure.ac:33: installing './config.sub'
configure.ac:39: installing './install-sh'
configure.ac:39: installing './missing'
Makefile.am: installing './INSTALL'
examples/Makefile.am: installing './depcomp'
parallel-tests: installing './test-driver'
~/work/nghttp3$ ./configure --enable-lib-only --prefix=/opt/curl  👈構築用の設定を実施
checking for gcc... gcc
checking whether the C compiler works... yes
checking for C compiler default output file name... a.out
checking for suffix of executables... 
・・・
    Library:
      Shared:         yes
      Static:         yes
    Debug:
      Debug:          no (CFLAGS='')
    Library only:     yes
    Examples:         no

~/work/nghttp3$ make 👈構築
make  all-recursive
make[1]: ディレクトリ '/home/study/work/nghttp3' に入ります
Making all in lib
make[2]: ディレクトリ '/home/study/work/nghttp3/lib' に入ります
Making all in includes
・・・
Making all in examples
make[2]: ディレクトリ '/home/study/work/nghttp3/examples' に入ります
make[2]: 'all' に対して行うべき事はありません.
make[2]: ディレクトリ '/home/study/work/nghttp3/examples' から出ます
make[2]: ディレクトリ '/home/study/work/nghttp3' に入ります
make[2]: ディレクトリ '/home/study/work/nghttp3' から出ます
make[1]: ディレクトリ '/home/study/work/nghttp3' から出ます
~/work/nghttp3$ 
~/work/nghttp3$ sudo make install   👈インストール
[sudo] study のパスワード:  👈パスワードを入力してEnter
Making install in lib
make[1]: ディレクトリ '/home/study/work/nghttp3/lib' に入ります
Making install in includes
make[2]: ディレクトリ '/home/study/work/nghttp3/lib/includes' に入ります
make[3]: ディレクトリ '/home/study/work/nghttp3/lib/includes' に入ります
make[3]: 'install-exec-am' に対して行うべき事はありません.
・・・
make[2]: 'install-exec-am' に対して行うべき事はありません.
 /usr/bin/mkdir -p '/opt/curl/share/doc/nghttp3'
 /usr/bin/install -c -m 644 README.rst '/opt/curl/share/doc/nghttp3'
make[2]: ディレクトリ '/home/study/work/nghttp3' から出ます
make[1]: ディレクトリ '/home/study/work/nghttp3' から出ます
~/work/nghttp3$ cd ..
~/work$
~~~

## ngtcp2の取得と構築

<div class="codetitle">実行コマンド</div>
~~~
git clone --depth 1 https://github.com/ngtcp2/ngtcp2
cd ngtcp2
autoreconf -fi
./configure PKG_CONFIG_PATH=/opt/curl/lib64/pkgconfig:/opt/curl/lib64/pkgconfig LDFLAGS="-Wl,-rpath,/opt/curl/lib64" --prefix=/opt/curl --enable-lib-only
make
sudo make install
cd ..
~~~

<div class="codetitle">実行画面サンプル（ホームディレクトリ下のworkで実行）</div>
~~~console
~/work$ git clone --depth 1 https://github.com/ngtcp2/ngtcp2   👈ソースコードを入手
Cloning into 'ngtcp2'...
remote: Enumerating objects: 435, done.
remote: Counting objects: 100% (435/435), done.
remote: Compressing objects: 100% (337/337), done.
remote: Total 435 (delta 163), reused 192 (delta 65), pack-reused 0
Receiving objects: 100% (435/435), 700.04 KiB | 3.38 MiB/s, done.
Resolving deltas: 100% (163/163), done.
~/work$ cd ngtcp2/   👈
~/work/ngtcp2$ autoreconf -fi 👈構築用の設定ファイルの準備
libtoolize: putting auxiliary files in AC_CONFIG_AUX_DIR, '.'.
libtoolize: copying file './ltmain.sh'
libtoolize: putting macros in AC_CONFIG_MACRO_DIRS, 'm4'.
libtoolize: copying file 'm4/libtool.m4'
libtoolize: copying file 'm4/ltoptions.m4'
libtoolize: copying file 'm4/ltsugar.m4'
libtoolize: copying file 'm4/ltversion.m4'
libtoolize: copying file 'm4/lt~obsolete.m4'
configure.ac:29: installing './compile'
configure.ac:32: installing './config.guess'
configure.ac:32: installing './config.sub'
configure.ac:38: installing './install-sh'
configure.ac:38: installing './missing'
Makefile.am: installing './INSTALL'
crypto/boringssl/Makefile.am: installing './depcomp'
parallel-tests: installing './test-driver'
~/work/ngtcp2$ ./configure PKG_CONFIG_PATH=/opt/curl/lib64/pkgconfig:/opt/curl/lib64/pkgconfig LDFLAGS="-Wl,-rpath,/opt/curl/lib64"   --prefix=/opt/curl --enable-lib-only 👈構築用の設定を実施
checking for gcc... gcc
checking whether the C compiler works... yes
checking for C compiler default output file name... a.out
checking for suffix of executables... 
・・・

    Libs:
      OpenSSL:        yes (CFLAGS='-I/opt/curl/include' LIBS='-L/opt/curl/lib64 -lssl -lcrypto')
      Libev:          no (CFLAGS='' LIBS='')
      Libnghttp3:     no (CFLAGS='' LIBS='')
      Jemalloc:       no (CFLAGS='' LIBS='')
      GnuTLS:         no (CFLAGS='' LIBS='')
      BoringSSL:      no (CFLAGS='' LIBS='')
      Picotls:        no (CFLAGS='' LIBS='')
      wolfSSL:        no (CFLAGS='' LIBS='')
      Libbrotlienc:   no (CFLAGS="' LIBS='')
      Libbrotlidec:   no (CFLAGS="' LIBS='')
    Examples:         no

~/work/ngtcp2$ make  👈構築
make  all-recursive
make[1]: ディレクトリ '/home/study/work/ngtcp2' に入ります
Making all in lib
make[2]: ディレクトリ '/home/study/work/ngtcp2/lib' に入ります
Making all in includes
・・・
make[3]: 'all-am' に対して行うべき事はありません.
make[3]: ディレクトリ '/home/study/work/ngtcp2/crypto' から出ます
make[2]: ディレクトリ '/home/study/work/ngtcp2/crypto' から出ます
make[2]: ディレクトリ '/home/study/work/ngtcp2' に入ります
make[2]: ディレクトリ '/home/study/work/ngtcp2' から出ます
make[1]: ディレクトリ '/home/study/work/ngtcp2' から出ます
~/work/ngtcp2$ sudo make install 👈インストール
[sudo] study のパスワード:  👈パスワードを入力してEnter
Making install in lib
make[1]: ディレクトリ '/home/study/work/ngtcp2/lib' に入ります
Making install in includes
make[2]: ディレクトリ '/home/study/work/ngtcp2/lib/includes' に入ります
make[3]: ディレクトリ '/home/study/work/ngtcp2/lib/includes' に入ります
make[3]: 'install-exec-am' に対して行うべき事はありません.
・・・

make[2]: 'install-exec-am' に対して行うべき事はありません.
 /usr/bin/mkdir -p '/opt/curl/share/doc/ngtcp2'
 /usr/bin/install -c -m 644 README.rst '/opt/curl/share/doc/ngtcp2'
make[2]: ディレクトリ '/home/study/work/ngtcp2' から出ます
make[1]: ディレクトリ '/home/study/work/ngtcp2' から出ます
~/work/ngtcp2$ cd .. 👈workディレクトリに戻る
~/work$ 
~~~

## curlの取得と構築

<div class="codetitle">実行コマンド</div>
~~~
git clone --depth 1 https://github.com/curl/curl
cd curl
autoreconf -fi
LDFLAGS="-Wl,-rpath,/opt/curl/lib64" ./configure --with-openssl=/opt/curl --with-nghttp2=/opt/curl --with-nghttp3=/opt/curl --with-ngtcp2=/opt/curl
make
sudo make install
cd ..
~~~

<div class="codetitle">実行画面サンプル（ホームディレクトリ下のworkで実行）</div>
~~~console
~/work$ git clone --depth 1 https://github.com/curl/curl 👈ソースコードの入手
Cloning into 'curl'...
remote: Enumerating objects: 3961, done.
remote: Counting objects: 100% (3961/3961), done.
remote: Compressing objects: 100% (3304/3304), done.
remote: Total 3961 (delta 1245), reused 1295 (delta 643), pack-reused 0
Receiving objects: 100% (3961/3961), 4.42 MiB | 3.81 MiB/s, done.
Resolving deltas: 100% (1245/1245), done.
~/work$ （curlディレクトリが生成されている）
~/work$ cd curl/  👈curlディレクトリに移動
~/work/curl$ autoreconf -fi   👈構築用の設定ファイルの準備
libtoolize: putting auxiliary files in '.'.
libtoolize: copying file './ltmain.sh'
libtoolize: putting macros in AC_CONFIG_MACRO_DIRS, 'm4'.
libtoolize: copying file 'm4/libtool.m4'
libtoolize: copying file 'm4/ltoptions.m4'
libtoolize: copying file 'm4/ltsugar.m4'
libtoolize: copying file 'm4/ltversion.m4'
libtoolize: copying file 'm4/lt~obsolete.m4'
libtoolize: Remember to add 'LT_INIT' to configure.ac.
configure.ac:124: installing './compile'
configure.ac:426: installing './config.guess'
configure.ac:426: installing './config.sub'
configure.ac:124: installing './install-sh'
configure.ac:130: installing './missing'
docs/examples/Makefile.am: installing './depcomp'
~/work/curl$ LDFLAGS="-Wl,-rpath,/opt/curl/lib64" ./configure --with-openssl=/opt/curl --with-nghttp2=/opt/curl --with-nghttp3=/opt/curl --with-ngtcp2=/opt/curl  👈構築用の設定の実施
checking whether to enable maintainer-specific portions of Makefiles... no
checking whether make supports nested variables... yes
checking whether to enable debug build options... no
checking whether to enable compiler optimizer... (assumed) yes
・・・

configure: Configured to build curl/libcurl:

  Host setup:       x86_64-pc-linux-gnu
  Install prefix:   /usr/local
  Compiler:         gcc
   CFLAGS:          -Werror-implicit-function-declaration -O2 -Wno-system-headers
   CPPFLAGS:        -isystem /opt/curl/include -isystem /opt/curl/include -isystem /opt/curl/include -isystem /opt/curl/include -isystem /opt/curl/include
   LDFLAGS:         -Wl,-rpath,/opt/curl/lib64 -L/opt/curl/lib64 -L/opt/curl/lib -L/opt/curl/lib -L/opt/curl/lib -L/opt/curl/lib
   LIBS:            -lnghttp3 -lngtcp2_crypto_quictls -lngtcp2 -lnghttp2 -lssl -lcrypto -lssl -lcrypto

  curl version:     8.8.0-DEV
  SSL:              enabled (OpenSSL v3+)
  SSH:              no      (--with-{libssh,libssh2})
  zlib:             no      (--with-zlib)
  brotli:           no      (--with-brotli)
  zstd:             no      (--with-zstd)
  GSS-API:          no      (--with-gssapi)
  GSASL:            no      (libgsasl not found)
  TLS-SRP:          enabled
  resolver:         POSIX threaded
  IPv6:             enabled
  Unix sockets:     enabled
  IDN:              no      (--with-{libidn2,winidn})
  Build docs:       enabled (--disable-docs)
  Build libcurl:    Shared=yes, Static=yes
  Built-in manual:  enabled
  --libcurl option: enabled (--disable-libcurl-option)
  Verbose errors:   enabled (--disable-verbose)
  Code coverage:    disabled
  SSPI:             no      (--enable-sspi)
  ca cert bundle:   /etc/ssl/certs/ca-certificates.crt
  ca cert path:     /etc/ssl/certs
  ca fallback:      no
  LDAP:             no      (--enable-ldap / --with-ldap-lib / --with-lber-lib)
  LDAPS:            no      (--enable-ldaps)
  RTSP:             enabled
  RTMP:             no      (--with-librtmp)
  PSL:              no      (--with-libpsl)
  Alt-svc:          enabled (--disable-alt-svc)
  Headers API:      enabled (--disable-headers-api)
  HSTS:             enabled (--disable-hsts)
  HTTP1:            enabled (internal)
  HTTP2:            enabled (nghttp2)
  HTTP3:            enabled (ngtcp2 + nghttp3)
  ECH:              no      (--enable-ech)
  WebSockets:       no      (--enable-websockets)
  Protocols:        DICT FILE FTP FTPS GOPHER GOPHERS HTTP HTTPS IMAP IMAPS IPFS IPNS MQTT POP3 POP3S RTSP SMB SMBS SMTP SMTPS TELNET TFTP
  Features:         AsynchDNS HSTS HTTP2 HTTP3 HTTPS-proxy IPv6 Largefile NTLM SSL TLS-SRP UnixSockets alt-svc threadsafe

~/work/curl$ make 👈構築
Making all in lib
make[1]: ディレクトリ '/home/study/work/curl/lib' に入ります
make  all-am
make[2]: ディレクトリ '/home/study/work/curl/lib' に入ります
  CC       libcurl_la-altsvc.lo
  CC       libcurl_la-amigaos.lo
  CC       libcurl_la-asyn-ares.lo
  CC       libcurl_la-asyn-thread.lo
  CC       libcurl_la-base64.lo
・・・
Making all in scripts
make[1]: ディレクトリ '/home/study/work/curl/scripts' に入ります
make[1]: 'all' に対して行うべき事はありません.
make[1]: ディレクトリ '/home/study/work/curl/scripts' から出ます
make[1]: ディレクトリ '/home/study/work/curl' に入ります
make[1]: 'all-am' に対して行うべき事はありません.
make[1]: ディレクトリ '/home/study/work/curl' から出ます
~/work/curl$ sudo make install   👈インストール
[sudo] study のパスワード: 
Making install in lib
make[1]: ディレクトリ '/home/study/work/curl/lib' に入ります
make[2]: ディレクトリ '/home/study/work/curl/lib' に入ります
 /usr/bin/mkdir -p '/usr/local/lib'
・・・
----------------------------------------------------------------------
Libraries have been installed in:
   /usr/local/lib

If you ever happen to want to link against installed libraries
in a given directory, LIBDIR, you must either use libtool, and
specify the full pathname of the library, or use the '-LLIBDIR'
flag during linking and do at least one of the following:
   - add LIBDIR to the 'LD_LIBRARY_PATH' environment variable
     during execution
   - add LIBDIR to the 'LD_RUN_PATH' environment variable
     during linking
   - use the '-Wl,-rpath -Wl,LIBDIR' linker flag
   - have your system administrator add LIBDIR to '/etc/ld.so.conf'

See any operating system documentation about shared libraries for
more information, such as the ld(1) and ld.so(8) manual pages.
----------------------------------------------------------------------
・・・
make[6]: ディレクトリ '/home/study/work/curl/docs/libcurl' から出ます
make[5]: ディレクトリ '/home/study/work/curl/docs/libcurl' から出ます
make[4]: ディレクトリ '/home/study/work/curl/docs/libcurl' から出ます
make[3]: ディレクトリ '/home/study/work/curl' から出ます
make[2]: ディレクトリ '/home/study/work/curl' から出ます
make[1]: ディレクトリ '/home/study/work/curl' から出ます
~/work/curl$ cd .. 👈workディレクトリに戻る
~/work$ 
~~~

### libcurl4のアンインストール

別のバージョンのcurlがインストールされている場合、ライブラリ参照のエラーが出ることがあります。
この場合は、libcurl4パッケージをアンインストールしてください。

~~~
sudo apt remove libcurl4
~~~

<div class="codetitle">確認している様子</div>
~~~console
$ which curl         👈curlコマンドの位置を確認（後述）
/usr/local/bin/curl  
$ curl --version     👈curlコマンドのバージョンを確認
curl: symbol lookup error: curl: undefined symbol: curl_easy_header
（参照エラーが起きている？！）
$ ldd /usr/local/bin/curl | grep curl
	libcurl.so.4 => /lib/x86_64-linux-gnu/libcurl.so.4 (0x000071c82024e000)
（今回作成した/usr/local/lib/libcurl.so.4とは異なるファイルを参照している？！）
$ sudo apt remove libcurl4 👈libcurl4パッケージを削除
パッケージリストを読み込んでいます... 完了
依存関係ツリーを作成しています... 完了        
状態情報を読み取っています... 完了        
以下のパッケージが自動でインストールされましたが、もう必要とされていません:　※
・・・
これを削除するには 'sudo apt autoremove' を利用してください。
以下のパッケージは「削除」されます:
  colord curl gnome-control-center gvfs-backends hplip libcurl4 libgphoto2-6 libsane1 sane-utils
  shotwell simple-scan transmission-gtk ubuntu-desktop ubuntu-desktop-minimal
アップグレード: 0 個、新規インストール: 0 個、削除: 14 個、保留: 0 個。
この操作後に 35.0 MB のディスク容量が解放されます。
続行しますか? [Y/n]  👈Enterで実行
(データベースを読み込んでいます ... 現在 214259 個のファイルとディレクトリがインストールされていま
す。)
・・・
$ curl --version  👈あらためてcurlコマンドのバージョンを確認
curl 8.8.0-DEV (x86_64-pc-linux-gnu) libcurl/8.8.0-DEV quictls/3.1.5 nghttp2/1.62.0-DEV ngtcp2/1.5.0-DEV nghttp3/1.3.0-DEV
Release-Date: [unreleased]
Protocols: dict file ftp ftps gopher gophers http https imap imaps ipfs ipns mqtt pop3 pop3s rtsp smb smbs smtp smtps telnet tftp
Features: alt-svc AsynchDNS HSTS HTTP2 HTTP3 HTTPS-proxy IPv6 Largefile NTLM SSL threadsafe TLS-SRP UnixSockets
（無事実行できるようになり、HTTP2, HTTP3が有効になっている）
~~~
※apt実行時に不要なパッケージが表示されることがある。メッセージの通り`sudo apt autoremove`で削除可能

### 作成したcurlコマンドの確認

**❶ コマンドラインで`curl`を実行した際に、“どの”curlが動いているかを確認**

- 今回`make install`でインストールしたcurlはデフォルトで`/usr/local/bin`にインストールされる（※curlの`./configure`実行時に`--prefix`で指定可能）
- curlコマンドを`sudo apt install ～`でインストールしている場合、`/usr/bin`にインストールされる
- 同名コマンドがある場合の優先順位は環境変数PATHやalias設定によって変わる（通常は`/usr/local/bin`の方が優先度が高い）
- `which curl`でどこにある`curl`が実行されるか確認できる
- 別のcurlが動いてしまう場合も、`/usr/local/bin/curl`のようにディレクトリ名を付ければ実行できる

~~~
which curl
~~~

~~~console
$ which curl
/usr/local/bin/curl  👈/usr/local/binのcurlが実行されることを確認
~~~

**❷ `curl --version`でcurlコマンドのバージョンとサポートしているプロトコルを確認**

- HTTP3が入っていない場合は途中でエラーが出ていないか、`./configure`でオプションを指定できているか等を確認
- ./configureをやり直した場合は、`make clean`を実行してから`make`と`sudo make install`を実行する
- 仮想環境で試している場合はいったん変更を破棄して最初から入れ直すことができる

~~~
curl --version
~~~

~~~console
$ curl --version  👈curlコマンドのバージョンを確認
curl 8.8.0-DEV (x86_64-pc-linux-gnu) libcurl/8.8.0-DEV quictls/3.1.5 nghttp2/1.62.0-DEV ngtcp2/1.5.0-DEV nghttp3/1.3.0-DEV
Release-Date: [unreleased]
Protocols: dict file ftp ftps gopher gophers http https imap imaps ipfs ipns mqtt pop3 pop3s rtsp smb smbs smtp smtps telnet tftp
Features: alt-svc AsynchDNS HSTS HTTP2 HTTP3 HTTPS-proxy IPv6 Largefile NTLM SSL threadsafe TLS-SRP UnixSockets
（無事実行できるようになり、HTTP2, HTTP3が有効になっている）
~~~

HTTP3が有効になっている場合は、--http3オプションが使用可能。
**※筆者が執筆時にWSL環境で構築した`curl 8.6.0-DEV`は`--HTTP1.1`、`--HTTP2`、`--HTTP3`のように大文字でも指定できましたが（小文字も使用可能）、
今回仮想環境で再構築した`curl 8.8.0-DEV`は下記実行例のように小文字で指定する必要がありました。**

~~~
$ curl -v --http3 https://www.example.com  👈HTTP/3で取得
* Host www.example.com:443 was resolved.
* IPv6: 2606:2800:21f:cb07:6820:80da:af6b:8b2c
* IPv4: 93.184.215.14
・・・
* using HTTP/3
* [HTTP/3] [0] OPENED stream for https://www.example.com/
* [HTTP/3] [0] [:method: GET]
* [HTTP/3] [0] [:scheme: https]
* [HTTP/3] [0] [:authority: www.example.com]
* [HTTP/3] [0] [:path: /]
* [HTTP/3] [0] [user-agent: curl/8.8.0-DEV]
* [HTTP/3] [0] [accept: */*]
> GET / HTTP/3
> Host: www.example.com
> User-Agent: curl/8.8.0-DEV
> Accept: */*
> 
* Request completely sent off
* old SSL session ID is stale, removing
< HTTP/3 200 
< accept-ranges: bytes
・・・
~~~

----
[TCP/IP＆ネットワークコマンド入門 サポートページ](https://nisim-m.github.io/tcpipcmdbook/)
