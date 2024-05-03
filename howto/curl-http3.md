
[TCP/IP＆ネットワークコマンド入門 サポートページ](https://nisim-m.github.io/tcpipcmdbook/) ～補足事項～
# HTTP3に対応したcurlコマンド

「4.4 HTTP/HTTPSの通信を見てみよう」内「参考：curlによるHTTP/3通信」（本文p.221）用
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

<div class="codetitle">実行コマンド（curl用のディレクトリを作成、ホームディレクトリに移動してworkディレクトリを作成しworkディレクトリに移動）</div>
~~~
sudo mkdir /opt/curl
cd
mkdir work
cd work
~~~

<div class="codetitle">実行画面サンプル</div>
~~~console
$ sudo mkdir /opt/curl
[sudo] study のパスワード:	👈パスワードを入力してEnter
$ cd            ←ホームディレクトリに移動
$ mkdir work    ←workディレクトリを作成
$ cd work       ←workディレクトリに移る
~/work$         ←プロンプトに~/workと表示されている、~はホームディレクトリを示す記号
~~~

### git clone 実行時の方針

`git clone プロジェクトのURL`でソースコードを取得して、ドキュメントに従って構築。

ドキュメントでは`git clone --depth 1 -b ～ http://github.com/～`としている。`--depth 1`は履歴一つ分（＝最新版）のみ取得するオプション。`-b`はブランチを指定するオプションで、ドキュメントではバージョンを指定してる。

- まずは最新版で試してみて、ダメだったらドキュメントにあるバージョンを使用する

<small>➡GitとGitHub、およびソースコードからの構築については姉妹本<a href="https://gihyo.jp/book/2021/978-4-297-12024-5">「Linux＋コマンド入門」</a>第5章「パッケージ管理 必要なモノを揃えられるようにしよう」の最後に取り上げているのでお持ちの方はご参照ください。ここで扱っている内容は“最初の1歩”なので、特に構築時の各コマンドやオプションの意味、なぜ必要なのかといった事柄については専門書をあたってください。Gitについては「Pro Git 2nd Edition」の日本語訳が <a href="https://git-scm.com/book/ja/v2">https://git-scm.com/book/ja/v2</a>でダウンロードできます。</small>

## 開発ツールをインストール

必要なパッケージ等を確認（まずは各サイトの概要で確認、エラーが出たらサイト内の詳細説明を見に行く）
- [https://github.com/quictls/openssl](https://github.com/quictls/openssl)
- [https://github.com/nghttp2/nghttp2](https://github.com/nghttp2/nghttp2) 「pkg-config」との記載あり
- [https://github.com/ngtcp2/nghttp3](https://github.com/ngtcp2/nghttp3)
- [https://github.com/ngtcp2/ngtcp2](https://github.com/ngtcp2/ngtcp2)「pkg-config、autoconf、automake、autotools-dev、libtool」
- [https://github.com/curl/curl](https://github.com/curl/curl)

<div class="codetitle">実行コマンド</div>
~~~
sudo apt install gcc make pkg-config autoconf automake autotools-dev libtool
~~~

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
続行しますか? [Y/n] y   👈yと入力してEnter
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

<div class="codetitle">実行コマンド（ソースコード入手、入手したディレクトリに移動、構築してインストール、workディレクトリに戻る）</div>
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
~/work$ git clone --depth 1 https://github.com/quictls/openssl
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
~/work/openssl$ ./config enable-tls1_3 --prefix=/opt/curl
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
~/work/openssl$ make
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
~/work/openssl$ エラーメッセージ（「syntax error」や「～はありません」等）が出ていなければOK
~/work/openssl$ sudo make install
[sudo] study のパスワード:  👈パスワードを入力してEnter
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
~/work/openssl$ cd ..   （workディレクトリに戻る、「..」は親ディレクトリを表す記号）
~/work$
~~~

## nghttp2の取得と構築

<div class="codetitle">実行コマンド（ソースコード入手、入手したディレクトリに移動、構築してインストール、workディレクトリに戻る）</div>
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
~/work$ git clone --depth 1 https://github.com/nghttp2/nghttp2
Cloning into 'nghttp2'...
remote: Enumerating objects: 647, done.
remote: Counting objects: 100% (647/647), done.
remote: Compressing objects: 100% (561/561), done.
remote: Total 647 (delta 161), reused 379 (delta 71), pack-reused 0
Receiving objects: 100% (647/647), 1.20 MiB | 2.53 MiB/s, done.
Resolving deltas: 100% (161/161), done.
~/work$              （nghttp2ディレクトリが生成されている）
~/work$ cd nghttp2/  （nghttp2ディレクトリへ移動）
~/work/nghttp2$ autoreconf -fi
libtoolize: putting auxiliary files in AC_CONFIG_AUX_DIR, '.'.
libtoolize: copying file './ltmain.sh'
libtoolize: putting macros in AC_CONFIG_MACRO_DIRS, 'm4'.
・・・
configure.ac:41: installing './install-sh'
configure.ac:41: installing './missing'
Makefile.am: installing './INSTALL'
examples/Makefile.am: installing './depcomp'
parallel-tests: installing './test-driver'
~/work/nghttp2$ ./configure --enable-lib-only --prefix=/opt/curl
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

~/work/nghttp2$ make
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
[sudo] study のパスワード: 
Making install in lib
make[1]: ディレクトリ '/home/study/work/nghttp2/lib' に入ります
Making install in includes
・・・
make[2]: 'install-exec-am' に対して行うべき事はありません.
 /usr/bin/mkdir -p '/opt/curl/share/doc/nghttp2'
 /usr/bin/install -c -m 644 README.rst '/opt/curl/share/doc/nghttp2'
make[2]: ディレクトリ '/home/study/work/nghttp2' から出ます
make[1]: ディレクトリ '/home/study/work/nghttp2' から出ます
~/work/nghttp2$ cd ..
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
~/work$ cd nghttp3/  （nghttp3ディレクトリへ移動する）
~/work/nghttp3$ git submodule update --init  サブモジュールの取得と更新更新
Submodule 'lib/sfparse' (https://github.com/ngtcp2/sfparse) registered for path 'lib/sfparse'
Submodule 'tests/munit' (https://github.com/ngtcp2/munit) registered for path 'tests/munit'
Cloning into '/home/study/work/nghttp3/lib/sfparse'...
Cloning into '/home/study/work/nghttp3/tests/munit'...
Submodule path 'lib/sfparse': checked out '6e1572691c66bb6c7e7c784330f0c676164fbdde'
Submodule path 'tests/munit': checked out '7f53fea8901089d46233302b3af35bf8be93cfc5'
~/work/nghttp3$ autoreconf -fi   （この後はいままでと同じ手順）
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
~/work/nghttp3$ 
~/work/nghttp3$ ./configure --enable-lib-only --prefix=/opt/curl
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

~/work/nghttp3$ make
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
~/work/nghttp3$ sudo make install
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
~/work/nghttp3$
~~~

## ngtcp2の取得と構築

<div class="codetitle">実行コマンド（ソースコード入手、入手したディレクトリに移動、構築してインストール、workディレクトリに戻る）</div>
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
~~~

## curlの取得と構築

<div class="codetitle">実行コマンド（ソースコード入手、入手したディレクトリに移動、構築してインストール、workディレクトリに戻る）</div>
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
~~~

### 作成したcurlコマンドの確認




----
[TCP/IP＆ネットワークコマンド入門 サポートページ](https://nisim-m.github.io/tcpipcmdbook/)
