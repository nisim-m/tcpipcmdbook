console
~~~console
$ sudo apt install gcc make pkg-config autoconf automake autotools-dev libtool
[sudo] study のパスワード:  👈パスワードを入力してEnter
パッケージリストを読み込んでいます... 完了
依存関係ツリーを作成しています... 完了        
状態情報を読み取っています... 完了        
・・・
以下のパッケージが新たにインストールされます:
  libdpkg-perl libfile-fcntllock-perl make pkg-config
~~~

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
~~~~


~~~console
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


bash
~~~bash
$ sudo apt install gcc make pkg-config autoconf automake autotools-dev libtool
[sudo] study のパスワード:  👈パスワードを入力してEnter
パッケージリストを読み込んでいます... 完了
依存関係ツリーを作成しています... 完了        
状態情報を読み取っています... 完了        
・・・
以下のパッケージが新たにインストールされます:
  libdpkg-perl libfile-fcntllock-perl make pkg-config
~~~

~~~bash
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
~~~~


~~~bash
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
