
[TCP/IP＆ネットワークコマンド入門 サポートページ](https://nisim-m.github.io/tcpipcmdbook/) ～学習用環境（Windows + VirtualBox + Ubuntu）～
# VirtualBox + Ubuntu

<!-- TOC -->

1. [ファイルのダウンロード](#ファイルのダウンロード)
   1. [VirtualBox](#virtualbox)
   2. [UbunutuのISOイメージ](#ubunutuのisoイメージ)
2. [VirtualBoxのインストール](#virtualboxのインストール)
3. [仮想マシンの作成](#仮想マシンの作成)
   1. [名前とオペレーティングシステムの選択](#名前とオペレーティングシステムの選択)
   2. [メモリーサイズ、ハードディスクサイズ](#メモリーサイズハードディスクサイズ)
4. [ゲストOS（Ubuntu）のインストール](#ゲストosubuntuのインストール)
   1. [インストール時の設定](#インストール時の設定)
   2. [再起動後の設定](#再起動後の設定)
      1. [Ubuntuデスクトップ](#ubuntuデスクトップ)
      2. [端末アプリ](#端末アプリ)
   3. [Guest Additionsのインストール](#guest-additionsのインストール)
      1. [解像度の変更](#解像度の変更)
      2. [クリップボードの共有](#クリップボードの共有)
5. [スナップショットの活用](#スナップショットの活用)
6. [Ubuntuの設定](#ubuntuの設定)
   1. [フォルダ名をアルファベットにする](#フォルダ名をアルファベットにする)
7. [ネットワークが切断されてしまった場合](#ネットワークが切断されてしまった場合)

<!-- /TOC -->

## ファイルのダウンロード

### VirtualBox

[https://www.virtualbox.org/wiki/Downloads](https://www.virtualbox.org/wiki/Downloads)
**VirtualBox xx.xx.xx platform packages**にOS別のダウンロードリンクがあるので、Windows Hostsをクリックしてインストーラーをダウンロードしてください。本書では7.0.12を使用しています。

![VirtualBox Downloadページ](images/2024-04-20-23-48-17.png)

### UbunutuのISOイメージ

インストール用のイメージファイルは [https://jp.ubuntu.com/download](https://jp.ubuntu.com/download) からダウンロードできます。本書では、Ubuntu Desktop 22.04.3 LTS（`ubuntu-22.04.3-desktop-amd64.iso`）を使用しています。

![Ubuntu Downloadページ](images/2024-04-20-23-58-05.png)

## VirtualBoxのインストール

VirtualBoxのインストーラーを実行し、画面に従ってインストールしてください。

<small>*※<a href="https://nisim-m.github.io/linuxcmdbook/howto/images/2023-01-02-17-16-54.png">“Oracle VM VirtualBox 7.x.x needs the Microsoft Visual C++ 2019 Redistributable Packaging being installed first.”のようなメッセージ</a>が表示された場合、Microsoftのサイトからダウンロードしてインストールしてください。（<a href="https://visualstudio.microsoft.com/ja/downloads/">https://visualstudio.microsoft.com/ja/downloads/</a> “Microsoft Visual C++ Redistributable for Visual Studio 2022” <a href="https://nisim-m.github.io/linuxcmdbook/howto/install-vcpp.html">画面例）</a>*</small>

<div class="imgtitle">VirtualBoxのインストール（Windows環境の状態やVirtualBoxのバージョンによって異なる画面が表示される可能性があります）</div>
<a href="images/2024-04-21-01-33-01.png"><img src="images/2024-04-21-01-33-01.png" width="300"/></a>


<a href="images/2024-04-21-01-34-25.png"><img src="images/2024-04-21-01-34-25.png" width="300"/></a>

<a href="images/2024-04-21-01-35-02.png"><img src="images/2024-04-21-01-35-02.png" width="300"/></a>

<a href="images/2024-04-21-01-35-41.png"><img src="images/2024-04-21-01-35-41.png" width="300"/></a>

<a href="images/2024-04-21-01-36-13.png"><img src="images/2024-04-21-01-36-13.png" width="300"/></a>

<a href="images/2024-04-21-01-38-01.png"><img src="images/2024-04-21-01-38-01.png" width="300"/></a>

## 仮想マシンの作成

VirtualBoxを実行し、仮想マシン(M)→新規(N)で仮想マシンを作成、ゲストOS（Ubuntu）をインストールします。

<a href="images/2024-04-21-01-48-24.png"><img src="images/2024-04-21-01-48-24.png" width="300"/></a>

### 名前とオペレーティングシステムの選択

仮想マシンの名前を決めて、<a href="#ubunutuのisoイメージ">ダウンロードしたUbuntuのISOイメージ</a>を選択します。
「自動インストールをスキップ」にチェックマークを入れて、「次へ」で進みます。

❶「名前」を入力（ここでは「Ubuntu 1」としています）
❷「ISOイメージ」でUbuntuのISOイメージを選択
❸「自動インストールをスキップ」にチェックマークを入れる※

<small>*※VirtualBoxのバージョンによっては「Skip Unattended Installation」と表示されます。この場合もチェックマークを入れてください。*</small>

<a href="images/2024-04-21-01-54-04.png"><img src="images/2024-04-21-01-54-04.png" width="300"/></a>


### メモリーサイズ、ハードディスクサイズ

この他の設定はデフォルトのままで問題ありません。

ゲストOSに割り当てるメモリーのサイズは、たくさん割り当てることでゲストOSが快適に動作するようになりますが、その分、ホストOSの動作が犠牲になります。
本書のネットワークコマンドを試すだけであれば、1024～2048MB程度で問題ありません。ハードディスクのサイズも25GBで問題ありませんが、本書で紹介している以外のソフトウェアも試してみたいという場合はもう少し大きくしておく方が扱いやすいでしょう。ディスクスペースは仮想OS側で使用した分だけが消費されます。

<a href="images/2024-04-21-01-55-06.png"><img src="images/2024-04-21-01-55-06.png" width="300"/></a>

<a href="images/2024-04-21-02-04-43.png"><img src="images/2024-04-21-02-04-43.png" width="300"/></a>

<a href="images/2024-04-21-02-09-49.png"><img src="images/2024-04-21-02-09-49.png" width="300"/></a>

## ゲストOS（Ubuntu）のインストール

「起動」をクリックすると、仮想マシンの作成時に選択たISOイメージからUbuntuが起動するので、画面に従ってインストールを行います。

<div class="imgtitle">起動をクリック</div>
<a href="images/2024-04-21-02-13-42.png"><img src="images/2024-04-21-02-13-42.png" width="300"/></a>

<a href="images/2024-04-21-02-23-32.png"><img src="images/2024-04-21-02-23-32.png" width="300"/></a>

<a href="images/2024-04-21-02-37-52.png"><img src="images/2024-04-21-02-37-52.png" width="300"/></a>

<div class="imgtitle">起動メニューが表示（Try or Install Ubuntuが選択された状態になっている）</div>
<a href="images/2024-04-21-02-45-34.png"><img src="images/2024-04-21-02-45-34.png" width="400"/></a>

<div class="imgtitle">画面をクリックすると「キャプチャー」についての説明が表示される</div>
<a href="images/2024-04-21-02-44-37.png"><img src="images/2024-04-21-02-44-37.png"/></a>

<small>ゲストOSの画面をクリックすると、キー操作やマウス入力をゲストOSが受け取る状態（キャプチャーされた状態）となります。ホストOS側を操作したい場合は、右側のCtrlキーを押します。このキーを**ホストキー**と言い、VirtualBoxの右下にも表示されています。</small>

<div class="imgtitle">「Try or Install Ubuntu」を選択した状態で<kbd>Enter</kbd></div>
<a href="images/2024-04-21-02-38-55.png"><img src="images/2024-04-21-02-38-55.png" width="400"/></a>

<div class="imgtitle">Ubuntuが起動する</div>
<a href="images/2024-04-21-02-43-13.png"><img src="images/2024-04-21-02-43-13.png" width="400"/></a>

<div class="imgtitle">Welcomeメニューが表示されたら左側の言語一覧をスクロール</div>
<a href="images/2024-04-21-02-51-46.png"><img src="images/2024-04-21-02-51-46.png" width="400"/></a>

<div class="imgtitle">日本語を選択して「Ubuntuをインストール」をクリック</div>
<a href="images/2024-04-21-02-52-41.png"><img src="images/2024-04-21-02-52-41.png" width="400"/></a>

### インストール時の設定

画面に従ってインストールを進めます。「続ける」というボタンは画面の右下に表示されています。 
<small>*※ボタンが画面上に表示されない場合はAlt+F7でスクロール（<a href="https://nisim-m.github.io/linuxcmdbook/howto/altf7.html">画面例</a>）*</small>

<div class="imgtitle">キーボードを選択して「続ける」をクリック</div>
<a href="images/2024-04-21-02-57-04.png"><img src="images/2024-04-21-02-57-04.png" width="400"/></a>

<div class="imgtitle">「通常のインストール」のまま「続ける」をクリック</div>
<a href="images/2024-04-21-02-59-45.png"><img src="images/2024-04-21-02-59-45.png" width="400"/></a>

<small>*※本書の学習範囲の場合「最小インストール」でも問題ありません。アプリケーションはインストール後に適宜追加可能です。*</small>

<div class="imgtitle">「ディスクを削除してUbuntuをインストール」のまま「インストール」をクリック</div>
<a href="images/2024-04-21-03-03-56.png"><img src="images/2024-04-21-03-03-56.png" width="400"/></a>

<small>*ここで言う「ディスク」とは仮想ディスクのことで、実環境には影響しません。*</small>

<div class="imgtitle">「ディスクに変更を書き込みますか？」というメッセージが表示されるので「続ける」をクリック</div>
<a href="images/2024-04-21-03-07-34.png"><img src="images/2024-04-21-03-07-34.png" width="400"/></a>

<div class="imgtitle">地域を選択して「続ける」をクリック</div>
<a href="images/2024-04-21-03-10-25.png"><img src="images/2024-04-21-03-10-25.png" width="400"/></a>

<div class="imgtitle">ユーザー名、コンピューター名、パスワードを入力して「続ける」をクリック</div>
<a href="images/2024-04-21-03-20-22.png"><img src="images/2024-04-21-03-20-22.png" width="400"/></a>

<div class="imgtitle">（インストール中）</div>
<a href="images/2024-04-21-03-22-29.png"><img src="images/2024-04-21-03-22-29.png" width="400"/></a>

<div class="imgtitle">再起動を促すメッセージが表示されるので、「今すぐ再起動する」をクリック</div>
<a href="images/2024-04-21-03-37-49.png"><img src="images/2024-04-21-03-37-49.png" width="400"/></a>

<div class="imgtitle">（"Please remove the installation medium, then press ENTER:"と表示されている場合は画面をクリックしてEnterを押す）</div>
<a href="images/2024-04-21-03-42-22.png"><img src="images/2024-04-21-03-42-22.png" width="400"/></a>

### 再起動後の設定

再起動するとGUI画面が表示されるのでインストールを完了させます。

<div class="imgtitle">ユーザーを選択してパスワードを入力</div>
<a href="images/2024-04-21-03-45-54.png"><img src="images/2024-04-21-03-45-54.png" width="400"/></a>

<a href="images/2024-04-21-03-46-25.png"><img src="images/2024-04-21-03-46-25.png" width="400"/></a>

<div class="imgtitle">オンラインアカウントへの接続（任意）※学習用の環境なので接続は不要</div>
<a href="images/2024-04-21-03-47-16.png"><img src="images/2024-04-21-03-47-16.png" width="400"/></a>

<div class="imgtitle">Ubuntu Proの有効化（任意）※学習用の環境なので不要</div>
<a href="images/2024-04-21-03-48-11.png"><img src="images/2024-04-21-03-48-11.png" width="400"/></a>

<div class="imgtitle">エラーリポートの送信（任意）</div>
<a href="images/2024-04-21-03-49-28.png"><img src="images/2024-04-21-03-49-28.png" width="400"/></a>

<div class="imgtitle">位置情報サービスの設定（任意）</div>
<a href="images/2024-04-21-03-54-51.png"><img src="images/2024-04-21-03-54-51.png" width="400"/></a>

<div class="imgtitle">完了</div>
<a href="images/2024-04-21-03-55-38.png"><img src="images/2024-04-21-03-55-38.png" width="400"/></a>

<a href="images/2024-04-21-03-56-36.png"><img src="images/2024-04-21-03-56-36.png" width="400"/></a>

※セットアップ中に「ソフトウェアの更新」が入った場合は「あとで再起動」として、セットアップを完了させてください。

<a href="images/2024-04-21-03-51-07.png"><img src="images/2024-04-21-03-51-07.png" width="400"/></a>

<a href="images/2024-04-21-03-51-57.png"><img src="images/2024-04-21-03-51-57.png" width="400"/></a>

<a href="images/2024-04-21-03-53-45.png"><img src="images/2024-04-21-03-53-45.png" width="400"/></a>


#### Ubuntuデスクトップ

Ubuntuデスクトップは以下の様な画面構成になっています。

<a href="images/2024-04-21-10-37-42.png"><img src="images/2024-04-21-10-37-42.png" width="400"/></a>

#### 端末アプリ

コマンドは「端末」アプリケーションで入力して実行します。アプリはdockに登録できます。

<a href="images/2024-04-21-10-30-13.png"><img src="images/2024-04-21-10-30-13.png" width="400"/></a>

### Guest Additionsのインストール

「Guest Additions」をインストールすると、ホストOSと端末の間でのコピー&ペーストが可能になります。

<div class="imgtitle">「デバイス」→「Guest Additions CDイメージの挿入」を選択</div>
<a href="images/2024-04-21-10-45-41.png"><img src="images/2024-04-21-10-45-41.png" width="400"/></a>

<div class="imgtitle">「autorun\.sh」を右クリック→「プログラムとして実行」</div>
<a href="images/2024-04-21-10-51-44.png"><img src="images/2024-04-21-10-51-44.png" width="400"/></a>

<div class="imgtitle">ゲストOSインストール時に設定したパスワードを入力</div>
<a href="images/2024-04-21-10-53-16.png"><img src="images/2024-04-21-10-53-16.png" width="400"/></a>


<small>このメッセージが表示されない場合、autorun.shが実行できていない可能性があります。端末で `sudo /media/ユーザー名/VBox_GAs_バージョン /VBoxLinuxAdditions.run` を実行してください。
`sudoスペース/me`<kbd>Tab</kbd>`ユーザー名/V`<kbd>Tab</kbd>`VBoxL`<kbd>Tab</kbd><kbd>Enter</kbd>で実行できます（コマンド補完：本文参照）。</small>

<div class="imgtitle">（autorun.sh実行中）</div>
<a href="images/2024-04-21-10-59-47.png"><img src="images/2024-04-21-10-59-47.png" width="400"/></a>

<div class="imgtitle">途中で解像度が変わり、「Press Return to close this window...」と表示されたら<kbd>Enterキー</kbd>を押して終了</div>
<a href="images/2024-04-21-11-00-13.png"><img src="images/2024-04-21-11-00-13.png" width="250"/></a>

<div class="imgtitle">CD（仮想ディスク）を取り出して再起動</div>
<a href="images/2024-04-21-11-05-01.png"><img src="images/2024-04-21-11-05-01.png" width="250"/></a>

<a href="images/2024-04-21-11-06-45.png"><img src="images/2024-04-21-11-06-45.png" width="250"/></a>

<a href="images/2024-04-21-11-09-27.png"><img src="images/2024-04-21-11-09-27.png" width="250"/></a>

#### 解像度の変更
解像度は再起動後「設定」から変更できます。

<div class="imgtitle">「設定」をクリック</div>
<a href="images/2024-04-21-11-13-05.png"><img src="images/2024-04-21-11-13-05.png" width="250"/></a>

<div class="imgtitle">スクロールして「ディスプレイ」を探し、クリックして「解像度」を変更</div>
<a href="images/2024-04-21-11-18-41.png"><img src="images/2024-04-21-11-18-41.png" width="250"/></a>

<div class="imgtitle">「適用」で保存</div>
<a href="images/2024-04-21-11-21-22.png"><img src="images/2024-04-21-11-21-22.png" width="250"/></a>

<a href="images/2024-04-21-11-23-24.png"><img src="images/2024-04-21-11-23-24.png" width="400"/></a>

<small>※解像度はVirtualBoxの「表示」メニューや、ウィンドウサイズの変更でも調整可能</small>

#### クリップボードの共有

「Guest Additions」をインストールしてUbuntuを再起動し、「デバイス」→「クリップボードの共有」を有効にすることで、ホストOSと端末の間でのコピー&ペーストが可能になります。

<div class="imgtitle">デバイス→クリップボードの共有で設定</div>
<a href="images/2024-04-21-11-28-31.png"><img src="images/2024-04-21-11-28-31.png" width="400"/></a>

## スナップショットの活用

VirtualBoxでは、任意のタイミングでゲストOSのスナップショットを作成しておくことができます。本書の学習ではあまり必要ありませんが、興味がある方は以下を参考にしてください。

<a href="https://nisim-m.github.io/linuxcmdbook/howto/install-ubuntu.html#%E3%82%B9%E3%83%8A%E3%83%83%E3%83%97%E3%82%B7%E3%83%A7%E3%83%83%E3%83%88%E3%81%AE%E6%B4%BB%E7%94%A8">スナップショットの活用（Linux1＋コマンド入門サポートページ内）</a>

## Ubuntuの設定

学習の性質上、画面を見ているが操作はしない、という時間が長くなりがちです。デフォルトでは操作していないと5分で画面がオフになりますが、この時間は「電源管理」の「省電力オプション」で設定できます。

<a href="images/2024-04-21-11-24-41.png"><img src="images/2024-04-21-11-24-41.png" width="400"/></a>

デスクトップの設定は「Ubuntuソフトウェア」の「Extension Manager」でもカスタマイズできます（例：Hide ClockエクステンションでTopパネルのカレンダーを非表示にする、等）。


### フォルダ名をアルファベットにする

ユーザーフォルダ（ユーザーのホームディレクトリ）にある「書類」や「ピクチャ」などのフォルダは、WindowsやmacOSの場合、実体はDocumentsやPicturesなどのアルファベットで付けられた名前になっていますが、日本語用にインストールしたUbuntuデスクトップの場合は実体も「書類」など日本語の名前になっています。本書ではコマンドラインでこれらのフォルダを扱うことはありませんが、今後、コマンドラインでほかの操作にも慣れていこうという場合、アルファベットの名前の方が扱いやすいでしょう。変更する場合は以下のコマンドを実行し画面の指示に従ってください。

<code>
LANG=C xdg-user-dirs-gtk-update
</code>

<small>※`LANG=C`スペース`xdg-u`<kbd>Tab</kbd>`s-g`<kbd>Tab</kbd><kbd>Enter</kbd>で実行できます。パスワード入力を求められたらUbuntuインストール時のパスワードを入力してください。</small>

<div class="imgtitle">Update standard folders～というメッセージが表示されるので「Update Names」をクリック</div>
<a href="images/2024-04-21-18-31-00.png"><img src="images/2024-04-21-18-31-00.png" width="400"/></a>

<div class="imgtitle">ディレクトリ（フォルダ）名が変更される</div>
<a href="images/2024-04-21-18-31-32.png"><img src="images/2024-04-21-18-31-32.png" width="400"/></a>

再起動後あらためてフォルダ名の変更を確認するメッセージが表示されたら「次回から表示しない」にチェックマークを入れて「古い名前のままにする」をクリックしてください。

<a href="images/2024-04-21-19-05-56.png"><img src="images/2024-04-21-19-05-56.png" width="400"/></a>

## ネットワークが切断されてしまった場合

VirtualBoxのUbuntuを起動したままホストOSがスリープ状態になると、復帰後のUbuntuでネットワークが使えなくなることがあります。

VirtualBoxの「デバイス」メニューで「ネットワーク」の「ネットワークアダプターを接続」をクリックしていったん切断し、再度、「ネットワークアダプターを接続」をクリックして接続し直してください。

<a href="images/2024-04-21-12-05-06.png"><img src="images/2024-04-21-12-05-06.png"/></a>

Ubuntuの再起動でも元の状態に戻ります。よくわからない時はいったん再起動してみてください。

----


[TCP/IP＆ネットワークコマンド入門 サポートページ](https://nisim-m.github.io/tcpipcmdbook/)
