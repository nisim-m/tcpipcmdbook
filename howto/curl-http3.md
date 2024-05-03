
[TCP/IP＆ネットワークコマンド入門 サポートページ](https://nisim-m.github.io/tcpipcmdbook/) ～補足事項～
# HTTP3に対応したcurlコマンド

<!-- TOC -->

<!-- /TOC -->

2台目の仮想マシンのホスト名（コンピューターの名前）を変更します。仮想マシンを起動して以下のコマンドを実行してください。ここでは、ubuntu2という名前にしています。

<code>sudo hostnamectl set-hostname ubuntu2</code>

ゲストネットワーク： `10.0.2.0/24`
DHCP割り当て開始アドレス： `10.0.2.1`
DHCP割り当て終了アドレス： `10.0.2.254`


~~~shell
ping www.example.com
wsl sudo ip netns exec u2 tcpdump -nlUw - | "c:\Program Files\Wireshark\Wireshark.exe" -ki - 2>NUL
~~~

~~~shell-session
$ ping www.example.com
PING www.example.com (93.184.216.34) 56(84) bytes of data.
64 bytes from 93.184.216.34 (93.184.216.34): icmp_seq=1 ttl=53 time=110 ms
64 bytes from 93.184.216.34 (93.184.216.34): icmp_seq=2 ttl=53 time=111 ms
64 bytes from 93.184.216.34 (93.184.216.34): icmp_seq=3 ttl=53 time=110 ms
64 bytes from 93.184.216.34 (93.184.216.34): icmp_seq=4 ttl=53 time=109 ms
~~~

----
[TCP/IP＆ネットワークコマンド入門 サポートページ](https://nisim-m.github.io/tcpipcmdbook/)
