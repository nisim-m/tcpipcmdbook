[TCP/IP＆ネットワークコマンド入門 サポートページ](https://nisim-m.github.io/tcpipcmdbook/) ～学習用環境（WSL）～
# WSL (Windows Subsystem for Linux)

<!-- TOC -->

1. [WSL (Windows Subsystem for Linux)](#wsl-windows-subsystem-for-linux)
   1. [Windows機能の有効化](#windows機能の有効化)
   2. [Ubuntuのインストール](#ubuntuのインストール)
      1. [WSLの終了](#wslの終了)
      2. [WSLを使用するには](#wslを使用するには)

<!-- /TOC -->

## Windows機能の有効化
WSLを使用するには、「Windowsの機能の有効化または無効化」で「Linux用Windowsサブシステム」と「仮想マシンプラットフォーム」を有効にして再起動します。
<div class="imgtitle">「Windowsの機能の有効化または無効化」を開く（スタートメニューで「Windowsの機能」を検索）>
<a href="images/2024-04-26-03-20-16.png"><img src="images/2024-04-26-03-20-16.png" width="300"/></a>

<div class="imgtitle">「Linux用Windowsサブシステム」にチェックマークを入れる>
<a href="images/2024-04-26-03-15-29.png"><img src="images/2024-04-26-03-15-29.png" width="300"/></a>

<div class="imgtitle">「仮想マシンプラットフォーム」にチェックマークを入れる>
<a href="images/2024-04-26-03-22-34.png"><img src="images/2024-04-26-03-22-34.png" width="300"/></a>

<small>※VMware（VirtualBoxと同じくGUIベースの仮想環境を構築できる仮想化ソフトウェア）とWSLを混在させる場合、上記2つに加えて「Hyper-V」と「Windowsハイパーバイザープラットフォーム」にもチェックマークを入れて有効にする必要があります。</small>
<div class="imgtitle">再起動>
<a href="images/2024-04-26-03-23-05.png"><img src="images/2024-04-26-03-23-05.png" width="300"/></a>

## Ubuntuのインストール
再起動したらコマンドラインでwsl --install –d Ubuntuを実行してUbuntuをインストールして再起動します。
<div class="imgtitle">「コマンドプロンプト」を開く（スタートメニューで「コマンド」または「cmd」を検索）>
<a href="images/2024-04-26-03-25-48.png"><img src="images/2024-04-26-03-25-48.png" width="300"/></a>

<div class="imgtitle">`wsl --install –d Ubuntu`と入力して<kbd>Enter</kbd>>
<a href="images/2024-04-26-03-23-57.png"><img src="images/2024-04-26-03-23-57.png" width="300"/></a>

<div class="imgtitle">（インストール中）>
<a href="images/2024-04-26-03-27-11.png"><img src="images/2024-04-26-03-27-11.png" width="300"/></a>

<div class="imgtitle">（インストール中）>
<a href="images/2024-04-26-03-27-30.png"><img src="images/2024-04-26-03-27-30.png" width="300"/></a>

<div class="imgtitle">（インストールが完了した）>
<a href="images/2024-04-26-03-28-09.png"><img src="images/2024-04-26-03-28-09.png" width="300"/></a>

<div class="imgtitle">スタートメニューから再起動>
<a href="images/2024-04-26-03-30-07.png"><img src="images/2024-04-26-03-30-07.png" width="300"/></a>

再起動するとコマンドプロンプトの画面が開き、Linuxの初期設定が始まります。
しばらく待つと「Enter new Unix username:」と表示されるので、WSL利用時のユーザー名を決めて入力してください。
Windowsのユーザーと共通でも異なる名前でも問題ありませんが、アルファベットの小文字にしておくと入力しやすく扱いやすくなります。
<div class="imgtitle">WSL用のユーザー名を入力して<kbd>Enter</kbd>>
<a href="images/2024-04-26-03-34-04.png"><img src="images/2024-04-26-03-34-04.png" width="300"/></a>

ユーザー名を入力してEnter を押すと「New Password:」と表示されるので、いま作成したユーザー用のパスワードを決めて入力します。
<div class="imgtitle">WSL用のパスワードを入力して<kbd>Enter</kbd>>
<a href="images/2024-04-26-03-35-13.png"><img src="images/2024-04-26-03-35-13.png" width="300"/></a>

<div class="imgtitle">確認のため再度パスワードを入力して<kbd>Enter</kbd>>
<a href="images/2024-04-26-03-35-20.png"><img src="images/2024-04-26-03-35-20.png" width="300"/></a>

<div class="imgtitle">セットアップ完了>
<a href="images/2024-04-26-03-37-29.png"><img src="images/2024-04-26-03-37-29.png" width="300"/></a>

プロンプトの記号が「$」に変わり、この後使用するipコマンドやncコマンドなどのLinuxコマンドが実行可能になります。
（➡p.21 0.4　学習用の環境を準備しよう「WSLの活用」➡p.44 0.10　Windowsコマンドライン「WSL」）

### WSLの終了

`exit`<kbd>Enter</kbd>でWSLが終了し、Windowsのコマンドプロンプト（>）に戻ります。
もう一度`exit`<kbd>Enter</kbd>と実行するとコマンドプロンプトが終了します。
※`exit`コマンドを使わずコマンドプロンプトのウィンドウを閉じても問題ありません。


### WSLを使用するには

コマンドプロンプトで`wsl`<kbd>Enter</kbd>、またはスタートメニューでWSLを検索して実行します。

両者の違いは実行時のカレントディレクトリで、前者はwslコマンド実行時のカレントディレクトリ（スタートメニューからコマンドプロンプトを実行した場合はWindowsユーザーのホームディレクトリ、標準ではC:\Users\Windowsのユーザー名）、後者はwslユーザーのホームディレクトリとなります。
本書ではコマンドプロンプトからwslコマンドを実行する方法を取っていますが、ネットワークコマンドの実行という点に於いては両者の違いはありません。お好みの方法で実行してください。

----
[TCP/IP＆ネットワークコマンド入門 サポートページ](https://nisim-m.github.io/tcpipcmdbook/)
