
[TCP/IP＆ネットワークコマンド入門 サポートページ](https://nisim-m.github.io/tcpipcmdbook/) ～学習用環境（macOS + UTM + Ubuntu）～
# UTM + Ubuntu

<!-- TOC -->

1. [ファイルのダウンロード](#ファイルのダウンロード)
   1. [UTM](#utm)
   2. [UbuntuのISOイメージ](#ubuntuのisoイメージ)
2. [UTMのインストールと起動](#utmのインストールと起動)
3. [仮想マシンの作成](#仮想マシンの作成)
   1. [メモリーとハードディスクのサイズ](#メモリーとハードディスクのサイズ)
   2. [名前を設定して保存](#名前を設定して保存)
4. [ゲストOS（Ubuntu）のインストール](#ゲストosubuntuのインストール)
   1. [インストール時の設定](#インストール時の設定)
   2. [再起動後の設定](#再起動後の設定)
   3. [Ubuntuデスクトップ](#ubuntuデスクトップ)
      1. [端末アプリ](#端末アプリ)

<!-- /TOC -->

## ファイルのダウンロード

macOS環境では、UTMと**ARM版**Ubuntuで学習環境を作成できます。

<small>*※Intel Macの場合はVirtualBoxが使用できるので、<a href="install-virtualbox.html">Windowsのページ</a>を参照してください。*</small>

![](images/2024-04-21-12-51-24.png)

### UTM

UTMは[https://mac.getutm.app/](https://mac.getutm.app/) で公開されています。無償版はDownloadボタンでダウンロードできます。違いはApp Storeで自動更新されるかどうかのみで機能は同じです（<a href="https://mac.getutm.app/#:~:text=difference">サイトの説明</a>より）。

### UbuntuのISOイメージ

**ARM版**Ubuntuのインストール用イメージファイルは [https://cdimage.ubuntu.com/jammy/daily-live/current/](https://cdimage.ubuntu.com/jammy/daily-live/current/) からダウンロードできます。**ARM**と書かれている方を選択してください。本書では、`jammy-desktop-arm64.iso`を使用しています。

![](images/2024-04-21-12-52-42.png)

## UTMのインストールと起動

ダウンロードしたUTM.dmgをダブルクリックで開き、UTMをApplicationsフォルダにドラッグ＆ドロップします。

<div class="imgtitle">アプリケーションフォルダにドラッグ＆ドロップ</div>
<a href="images/2024-04-21-16-32-21.png"><img src="images/2024-04-21-16-32-21.png" width="300"/></a>

<div class="imgtitle">アプリケーションフォルダのUTMをダブルクリックで起動</div>
<a href="images/2024-04-21-16-34-19.png"><img src="images/2024-04-21-16-34-19.png" width="400"/></a>

<div class="imgtitle">初回は確認メッセージが表示されるので「開く」をクリック</div>
<a href="images/2024-04-21-16-34-57.png"><img src="images/2024-04-21-16-34-57.png"/></a>

<a href="images/2024-04-21-16-36-24.png"><img src="images/2024-04-21-16-36-24.png" width="300"/></a>

## 仮想マシンの作成

「新規仮想マシンを作成」で仮想マシンを作成し、ゲストOS（Ubuntu）をインストールします。

<div class="imgtitle">「新規仮想マシンを作成」をクリック</div>
<a href="images/2024-04-21-16-45-26.png"><img src="images/2024-04-21-16-45-26.png" width="300"/></a>

<div class="imgtitle">「仮想化」を選択</div>
<a href="images/2024-04-21-16-46-26.png"><img src="images/2024-04-21-16-46-26.png" width="300"/></a>

<div class="imgtitle">「Linux」を選択</div>
<a href="images/2024-04-21-16-47-04.png"><img src="images/2024-04-21-16-47-04.png" width="400"/></a>

<div class="imgtitle">「選択」でダウンロードしたISOイメージを選択して「続ける」</div>
<a href="images/2024-04-21-16-55-42.png"><img src="images/2024-04-21-16-55-42.png" width="300"/></a>

### メモリーとハードディスクのサイズ

デフォルトのままで問題ありません。

ゲストOSに割り当てるメモリーのサイズは、たくさん割り当てることでゲストOSが快適に動作するようになりますが、その分、ホストOSの動作が犠牲になります。 本書のネットワークコマンドを試すだけであれば、1024～2048MB程度で問題ありません。ハードディスクのサイズも25GBで問題ありませんが、本書で紹介している以外のソフトウェアも試してみたいという場合はもう少し大きくしておく方が扱いやすいでしょう。ディスクスペースは仮想OS側で使用した分だけが消費されます。

<a href="images/2024-04-21-16-51-25.png"><img src="images/2024-04-21-16-51-25.png" width="300"/></a>

<a href="images/2024-04-21-16-52-13.png"><img src="images/2024-04-21-16-52-13.png" width="300"/></a>

<a href="images/2024-04-21-16-53-10.png"><img src="images/2024-04-21-16-53-10.png" width="300"/></a>

### 名前を設定して保存

仮想マシンの名前を決めて「保存」をクリックします。ここでは「Ubuntu 1」としています。名前は後でも変更可能です。

<a href="images/2024-04-21-16-58-14.png"><img src="images/2024-04-21-16-58-14.png" width="300"/></a>

## ゲストOS（Ubuntu）のインストール

仮想マシンの電源を入れるとISOイメージからUbuntuが起動します。
デスクトップにインストール用のアイコンがあるのでダブルクリックで実行し、画面に従ってインストールしてください。

<div class="imgtitle">▶をクリックして電源ON</div>
<a href="images/2024-04-21-16-59-05.png"><img src="images/2024-04-21-16-59-05.png" width="300"/></a>

<a href="images/2024-04-21-16-59-28.png"><img src="images/2024-04-21-16-59-28.png" width="300"/></a>

<div class="imgtitle">「Try or Install Ubuntu」を選択した状態で<kbd>return</kbd></div>
<a href="images/2024-04-21-17-01-28.png"><img src="images/2024-04-21-17-01-28.png" width="400"/></a>

<div class="imgtitle">（起動中）</div>
<a href="images/2024-04-21-17-01-57.png"><img src="images/2024-04-21-17-01-57.png" width="400"/></a>

<div class="imgtitle">アクセス許可のメッセージが表示されたら適宜「許可しない」または「OK」をクリック（本書ではマイクを使用しません）</div>
<a href="images/2024-04-21-17-03-36.png"><img src="images/2024-04-21-17-03-36.png"/></a>

起動中にエラーメッセージが表示されることがあります。ここでは無視して問題ありません。

<a href="images/2024-04-21-17-05-46.png"><img src="images/2024-04-21-17-05-46.png" width="400"/></a>

デスクトップにインストール用のアイコン（ここでは「Install Ubuntu 22.04.3 LTS」）があるのでダブルクリックで実行し、画面に従ってインストールしてください。

<div class="imgtitle">「Install Ubuntu 22.04.3 LTS」を実行</div>
<a href="images/2024-04-21-17-08-03.png"><img src="images/2024-04-21-17-08-03.png" width="400"/></a>

<div class="imgtitle">Welcomeメニューが表示されたら左側の言語一覧をスクロール</div>
<a href="images/2024-04-21-17-10-04.png"><img src="images/2024-04-21-17-10-04.png" width="400"/></a>

<div class="imgtitle">日本語を選択して「続ける」をクリック</div>
<a href="images/2024-04-21-17-11-56.png"><img src="images/2024-04-21-17-11-56.png" width="400"/></a>

### インストール時の設定

画面に従ってインストールを進めます。「続ける」というボタンは画面の右下に表示されています。

<div class="imgtitle">キーボードを選択して「続ける」をクリック</div>
<a href="images/2024-04-21-17-13-28.png"><img src="images/2024-04-21-17-13-28.png" width="400"/></a>

<div class="imgtitle">「通常のインストール」のまま「続ける」をクリック</div>
<a href="images/2024-04-21-17-14-48.png"><img src="images/2024-04-21-17-14-48.png" width="400"/></a>

<small>※本書の学習範囲の場合「最小インストール」でも問題ありません。アプリケーションはインストール後に適宜追加可能です。</small>

<div class="imgtitle">「ディスクを削除してUbuntuをインストール」のまま「インストール」をクリック</div>
<a href="images/2024-04-21-17-23-38.png"><img src="images/2024-04-21-17-23-38.png" width="400"/></a>

<small>※ここで言う「ディスク」とは仮想ディスクのことで、実環境には影響しません。</small>

<div class="imgtitle">「ディスクに変更を書き込みますか？」というメッセージが表示されるので「続ける」をクリック</div>
<a href="images/2024-04-21-17-25-26.png"><img src="images/2024-04-21-17-25-26.png" width="400"/></a>

<div class="imgtitle">地域を選択して「続ける」をクリック</div>
<a href="images/2024-04-21-17-27-18.png"><img src="images/2024-04-21-17-27-18.png" width="400"/></a>

<div class="imgtitle">ユーザー名、コンピューター名、パスワードを入力して「続ける」をクリック</div>
<a href="images/2024-04-21-17-34-37.png"><img src="images/2024-04-21-17-34-37.png" width="400"/></a>

<div class="imgtitle">（インストール中）</div>
<a href="images/2024-04-21-17-35-11.png"><img src="images/2024-04-21-17-35-11.png" width="400"/></a>

<div class="imgtitle">再起動を促すメッセージが表示されるので、「今すぐ再起動する」をクリック</div>
<a href="images/2024-04-21-17-37-14.png"><img src="images/2024-04-21-17-37-14.png" width="400"/></a>

再起動時、画面が黒くなったら
❶ドライブイメージのオプション（右から3番目、光学ディスクのアイコン）をクリックして「CD/DVD（ISO）イメージ」を「取り出す」
❷再起動ボタン（左側、名前の脇にある◁のアイコン）をクリックして再起動

<a href="images/2024-04-21-17-52-19.png"><img src="images/2024-04-21-17-52-19.png" width="400"/></a>

<div class="imgtitle">（再起動中）</div>
<a href="images/2024-04-21-17-54-26.png"><img src="images/2024-04-21-17-54-26.png" width="300"/></a>

### 再起動後の設定
再起動するとGUI画面が表示されるのでインストールを完了させます。

<div class="imgtitle">ユーザーを選択してパスワードを入力</div>
<a href="images/2024-04-21-17-55-02.png"><img src="images/2024-04-21-17-55-02.png" width="400"/></a>

<a href="images/2024-04-21-17-56-16.png"><img src="images/2024-04-21-17-56-16.png" width="400"/></a>

起動後に内部エラーのメッセージが表示されることがあります。レポートの送信は任意です。

<a href="images/2024-04-21-18-00-49.png"><img src="images/2024-04-21-18-00-49.png" width="400"/></a>

<div class="imgtitle">オンラインアカウントへの接続（任意）※学習用の環境なので接続は不要</div>
<a href="images/2024-04-21-18-05-37.png"><img src="images/2024-04-21-18-05-37.png" width="400"/></a>

<div class="imgtitle">Ubuntu Proの有効化（任意）※学習用の環境なので不要</div>
<a href="images/2024-04-21-18-14-12.png"><img src="images/2024-04-21-18-14-12.png" width="400"/></a>

<div class="imgtitle">エラーリポートの送信（任意）</div>
<a href="images/2024-04-21-18-15-52.png"><img src="images/2024-04-21-18-15-52.png" width="400"/></a>

<div class="imgtitle">位置情報サービスの設定（任意）</div>
<a href="images/2024-04-21-18-17-00.png"><img src="images/2024-04-21-18-17-00.png" width="400"/></a>

<div class="imgtitle">完了</div>
<a href="images/2024-04-21-18-17-47.png"><img src="images/2024-04-21-18-17-47.png" width="400"/></a>

<a href="images/2024-04-21-18-18-32.png"><img src="images/2024-04-21-18-18-32.png" width="400"/></a>

### Ubuntuデスクトップ
Ubuntuデスクトップは以下の様な画面構成になっています。

<a href="images/2024-04-21-18-25-27.png"><img src="images/2024-04-21-18-25-27.png" width="400"/></a>

#### 端末アプリ

コマンドは「端末」アプリケーションで入力して実行します。アプリはdockに登録できます。

<a href="images/2024-04-21-18-24-09.png"><img src="images/2024-04-21-18-24-09.png" width="400"/></a>

このままで問題なく使用できますが、画面オフまでの時間などを設定すると使い勝手が良くなります。設定方法は「<a href="./install-virtualbox.html#ubuntuの設定">Ubuntuの設定</a>」を参照してください。

----
[TCP/IP＆ネットワークコマンド入門 サポートページ](https://nisim-m.github.io/tcpipcmdbook/)
