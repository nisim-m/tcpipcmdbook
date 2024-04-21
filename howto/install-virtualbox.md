
[TCP/IP＆ネットワークコマンド入門 サポートページ](https://nisim-m.github.io/tcpipcmdbook/) ～学習用環境（Windows + VirtualBox + Ubuntu）～
# VirtualBox + Ubuntu

<!-- TOC -->

1. [VirtualBox + Ubuntu](#virtualbox--ubuntu)
   1. [ファイルのダウンロード](#ファイルのダウンロード)
      1. [VirtualBox](#virtualbox)
      2. [UbunutuのISOイメージ](#ubunutuのisoイメージ)
   2. [VirtualBoxのインストール](#virtualboxのインストール)
   3. [仮想マシンの作成](#仮想マシンの作成)
      1. [名前とオペレーティングシステムの選択](#名前とオペレーティングシステムの選択)
      2. [メモリーサイズ、ハードディスクサイズ](#メモリーサイズハードディスクサイズ)
   4. [ゲストOS（Ubuntu）のインストール](#ゲストosubuntuのインストール)
      1. [インストール時の設定](#インストール時の設定)
      2. [再起動](#再起動)
      3. [再起動後の設定](#再起動後の設定)
         1. [Ubuntuデスクトップ](#ubuntuデスクトップ)
         2. [端末アプリ](#端末アプリ)
      4. [Guest Additionsのインストール](#guest-additionsのインストール)
         1. [解像度の変更](#解像度の変更)
         2. [クリップボードの共有](#クリップボードの共有)
      5. [Ubuntuの設定](#ubuntuの設定)
      6. [ネットワークが切断されてしまった場合](#ネットワークが切断されてしまった場合)
      7. [スナップショットの活用](#スナップショットの活用)

<!-- /TOC -->

## ファイルのダウンロード

### VirtualBox

[https://www.virtualbox.org/wiki/Downloads](https://www.virtualbox.org/wiki/Downloads)
**VirtualBox xx.xx.xx platform packages**にOS別のダウンロードリンクがあるので、Windows Hostsをクリックしてインストーラーをダウンロードしてください。本書では7.0.12を使用しています。

![VirtualBox Downloadページ](2024-04-20-23-48-17.png)

### UbunutuのISOイメージ

インストール用のイメージファイルは [https://jp.ubuntu.com/download](https://jp.ubuntu.com/download) からダウンロードできます。本書では、Ubuntu Desktop 22.04.3 LTS（`ubuntu-22.04.3-desktop-amd64.iso`）を使用しています。

![Ubuntu Downloadページ](2024-04-20-23-58-05.png)

## VirtualBoxのインストール

VirtualBoxのインストーラーを実行し、画面に従ってインストールしてください。

<small>*※<a href="https://nisim-m.github.io/linuxcmdbook/howto/images/2023-01-02-17-16-54.png">“Oracle VM VirtualBox 7.x.x needs the Microsoft Visual C++ 2019 Redistributable Packaging being installed first.”のようなメッセージ</a>が表示された場合、Microsoftのサイトからダウンロードしてインストールしてください。（<a href="https://visualstudio.microsoft.com/ja/downloads/">https://visualstudio.microsoft.com/ja/downloads/</a> “Microsoft Visual C++ Redistributable for Visual Studio 2022” <a href="https://nisim-m.github.io/linuxcmdbook/howto/install-vcpp.html">画面例）</a>*</small>

画面例（Windows環境の状態やVirtualBoxのバージョンによって異なる画面が表示される可能性があります）：

![VirtualBoxインストール画面](images/2024-04-21-01-33-01.png)

![VirtualBoxインストール画面](images/2024-04-21-01-34-25.png)

![VirtualBoxインストール画面](images/2024-04-21-01-35-02.png)

![VirtualBoxインストール画面](images/2024-04-21-01-35-41.png)

![VirtualBoxインストール画面](images/2024-04-21-01-36-13.png)

![VirtualBoxインストール画面](images/2024-04-21-01-38-01.png)

## 仮想マシンの作成

VirtualBoxを実行し、仮想マシン(M)→新規(N)で仮想マシンを作成、ゲストOS（Ubuntu）をインストールします。

![](images/2024-04-21-01-48-24.png)

### 名前とオペレーティングシステムの選択

仮想マシンの名前を決めて、<a href="#ubunutuのisoイメージ">ダウンロードしたUbuntuのISOイメージ</a>を選択します。
「自動インストールをスキップ」にチェックマークを入れて、「次へ」で進みます。

❶「名前」を入力（ここでは「Ubuntu 1」としています）
❷「ISOイメージ」でUbuntuのISOイメージを選択
❸「自動インストールをスキップ」にチェックマークを入れる※

<small>*※VirtualBoxのバージョンによっては「Skip Unattended Installation」と表示されます。この場合もチェックマークを入れてください。*</small>

![](images/2024-04-21-01-54-04.png)


### メモリーサイズ、ハードディスクサイズ

この他の設定はデフォルトのままで問題ありません。

ゲストOSに割り当てるメモリーのサイズは、たくさん割り当てることでゲストOSが快適に動作するようになりますが、その分、ホストOSの動作が犠牲になります。
本書のネットワークコマンドを試すだけであれば、1024～2048MB程度で問題ありません。ハードディスクのサイズも25GBで問題ありませんが、本書で紹介している以外のソフトウェアも試してみたいという場合はもう少し大きくしておく方が扱いやすいでしょう。ディスクスペースは仮想OS側で使用した分だけが消費されます。

![](images/2024-04-21-01-55-06.png)

![](images/2024-04-21-02-04-43.png)

![](images/2024-04-21-02-09-49.png)

## ゲストOS（Ubuntu）のインストール

「起動」をクリックすると、仮想マシンの作成時に選択たISOイメージからUbuntuが起動するので、画面に従ってインストールを行います。

起動をクリック
![](images/2024-04-21-02-13-42.png)

![](images/2024-04-21-02-23-32.png)

![](images/2024-04-21-02-37-52.png)

起動メニューが表示（Try or Install Ubuntuが選択された状態になっている）
![](images/2024-04-21-02-45-34.png)

画面をクリックすると「キャプチャー」についての説明が表示される
![](images/2024-04-21-02-44-37.png)

「Try or Install Ubuntu」を選択した状態でEnter
![](images/2024-04-21-02-38-55.png)

Ubuntuが起動する
![](images/2024-04-21-02-43-13.png)

Welcomeメニューが表示されたら左側の言語一覧をスクロール
![](images/2024-04-21-02-51-46.png)

日本語を選択して「Ubuntuをインストール」をクリック
![](images/2024-04-21-02-52-41.png)

### インストール時の設定

画面に従ってインストールを進めます。「続ける」というボタンは画面の右下に表示されています。 
<small>*※ボタンが画面上に表示されない場合はAlt+F7でスクロール（<a href="https://nisim-m.github.io/linuxcmdbook/howto/altf7.html">画面例</a>）*</small>

キーボードを選択して「続ける」をクリック
![](images/2024-04-21-02-57-04.png)

「通常のインストール」のまま「続ける」をクリック
![](images/2024-04-21-02-59-45.png)
<small>*※本書の学習範囲の場合「最小インストール」でも問題ありません。アプリケーションはインストール後に適宜追加可能です。*</small>

「ディスクを削除してUbuntuをインストール」のまま「インストール」をクリック
![](images/2024-04-21-03-03-56.png)
<small>*ここで言う「ディスクを削除」とは仮想ディスクのことで、実環境には影響しません。*</small>

「ディスクに変更を書き込みますか？」というメッセージが表示されるので「続ける」をクリック
![](images/2024-04-21-03-07-34.png)

地域を選択して「続ける」をクリック
![](images/2024-04-21-03-10-25.png)

ユーザー名、コンピューター名、パスワードを入力して「続ける」をクリック
![](images/2024-04-21-03-20-22.png)

（インストール中）
![](images/2024-04-21-03-22-29.png)

### 再起動

再起動を促すメッセージが表示されるので、「今すぐ再起動する」をクリック

![](images/2024-04-21-03-37-49.png)

（"Please remove the installation medium, then press ENTER:"と表示されている場合は画面をクリックしてEnterを押す）
![](images/2024-04-21-03-42-22.png)

### 再起動後の設定

再起動するとGUI画面が表示されるのでインストールを完了させます。

ユーザーを選択してパスワードを入力
![](images/2024-04-21-03-45-54.png)

![](images/2024-04-21-03-46-25.png)

オンラインアカウントへの接続（任意）※学習用の環境なので接続は不要
![](images/2024-04-21-03-47-16.png)

Ubuntu Proの有効化（任意）※学習用の環境なので不要
![](images/2024-04-21-03-48-11.png)

エラーリポートの送信（任意）
![](images/2024-04-21-03-49-28.png)

位置情報サービスの設定（任意）
![](images/2024-04-21-03-54-51.png)

完了
![](images/2024-04-21-03-55-38.png)

![](images/2024-04-21-03-56-36.png)

※セットアップ中に「ソフトウェアの更新」が入った場合は「あとで再起動」として、セットアップを完了させてください。

![](images/2024-04-21-03-51-07.png)

![](images/2024-04-21-03-51-57.png)

![](images/2024-04-21-03-53-45.png)


#### Ubuntuデスクトップ

Ubuntuデスクトップは以下の様な画面構成になっています。

![](images/2024-04-21-10-37-42.png)

#### 端末アプリ

コマンドは「端末」アプリケーションで入力して実行します。アプリはdockに登録できます。

![](images/2024-04-21-10-30-13.png)

### Guest Additionsのインストール

「Guest Additions」をインストールすると、ホストOSと端末の間でのコピー&ペーストが可能になります。

「デバイス」→「Guest Additions CDイメージの挿入」を選択
![](images/2024-04-21-10-45-41.png)

「autorun\.sh」を右クリック→「プログラムとして実行」
![](images/2024-04-21-10-51-44.png)

ゲストOSインストール時に設定したパスワードを入力
![](images/2024-04-21-10-53-16.png)


<small>このメッセージが表示されない場合、autorun.shが実行できていない可能性があります。端末で `sudo /media/ユーザー名/VBox_GAs_バージョン /VBoxLinuxAdditions.run` を実行してください。
`sudoスペース/me`<kbd>Tab</kbd>`ユーザー名/V`<kbd>Tab</kbd>`VBoxL`<kbd>Tab</kbd><kbd>Enter</kbd>で実行できます（コマンド補完：本文参照）。</small>

（autorun.sh実行中）
![](images/2024-04-21-10-59-47.png)

途中で解像度が変わり、「Press Return to close this window...」と表示されたらEnterキーを押して終了
![](images/2024-04-21-11-00-13.png)

CD（仮想ディスク）を取り出して再起動
![](images/2024-04-21-11-05-01.png)

![](images/2024-04-21-11-06-45.png)

![](images/2024-04-21-11-09-27.png)

#### 解像度の変更
解像度は再起動後「設定」から変更できます。

「設定」をクリック
![](images/2024-04-21-11-13-05.png)

スクロールして「ディスプレイ」を探し、クリックして「解像度」を変更
![](images/2024-04-21-11-18-41.png)

「適用」で保存
![](images/2024-04-21-11-21-22.png)

![](images/2024-04-21-11-23-24.png)

<small>※解像度はVirtualBoxの「表示」メニューや、ウィンドウサイズの変更でも調整可能</small>

#### クリップボードの共有

「Guest Additions」をインストールしてUbuntuを再起動し、「デバイス」→「クリップボードの共有」を有効にすることで、ホストOSと端末の間でのコピー&ペーストが可能になります。

デバイス→クリップボードの共有で設定
![](images/2024-04-21-11-28-31.png)

### Ubuntuの設定

学習の性質上、画面を見ているが操作はしない、という時間が長くなりがちです。デフォルトでは操作していないと5分で画面がオフになりますが、この時間は「電源管理」の「省電力オプション」で設定できます。

![](images/2024-04-21-11-24-41.png)

デスクトップの設定は「Ubuntuソフトウェア」の「Extension Manager」でもカスタマイズできます（例：Hide ClockエクステンションでTopパネルのカレンダーを非表示にする、等）。

### ネットワークが切断されてしまった場合

VirtualBoxのUbuntuを起動したままホストOSがスリープ状態になると、復帰後のUbuntuでネットワークが使えなくなることがあります。

VirtualBoxの「デバイス」メニューで「ネットワーク」の「ネットワークアダプターを接続」をクリックしていったん切断し、再度、「ネットワークアダプターを接続」をクリックして接続し直してください。

![](images/2024-04-21-12-05-06.png)

Ubuntuの再起動でも元の状態に戻ります。よくわからない時はいったん再起動してみてください。


### スナップショットの活用

VirtualBoxでは、任意のタイミングでゲストOSのスナップショットを作成しておくことができます。本書の学習ではあまり必要ありませんが、興味がある方は以下を参考にしてください。

<a href="https://nisim-m.github.io/linuxcmdbook/howto/install-ubuntu.html#%E3%82%B9%E3%83%8A%E3%83%83%E3%83%97%E3%82%B7%E3%83%A7%E3%83%83%E3%83%88%E3%81%AE%E6%B4%BB%E7%94%A8">スナップショットの活用（Linux1＋コマンド入門サポートページ内）</a>



----


[TCP/IP＆ネットワークコマンド入門 サポートページ](https://nisim-m.github.io/tcpipcmdbook/)
