
[TCP/IP＆ネットワークコマンド入門 サポートページ](https://nisim-m.github.io/tcpipcmdbook/) ～学習用環境（macOS + UTM + Ubuntu）～
# UTM + Ubuntu

<!-- TOC -->

1. [UTM + Ubuntu](#utm--ubuntu)
   1. [ファイルのダウンロード](#ファイルのダウンロード)
      1. [UTM](#utm)
      2. [UbuntuのISOイメージ](#ubuntuのisoイメージ)
      3. [UTMのインストールと起動](#utmのインストールと起動)
      4. [仮想マシンの作成](#仮想マシンの作成)
      5. [ゲストOS（Ubuntu）のインストール](#ゲストosubuntuのインストール)
      6. [Ubuntuデスクトップ](#ubuntuデスクトップ)
      7. [端末アプリ](#端末アプリ)

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

### UTMのインストールと起動

ダウンロードしたUTM.dmgをダブルクリックで開き、UTMをApplicationsフォルダにドラッグ＆ドロップします。

![](images/2024-04-21-16-32-21.png)

アプリケーションフォルダのUTMをダブルクリックで起動
![](images/2024-04-21-16-34-19.png)

初回は確認メッセージが表示されるので「開く」をクリック
![](images/2024-04-21-16-34-57.png)

![](images/2024-04-21-16-36-24.png)

### 仮想マシンの作成

「新規仮想マシンを作成」をクリック
![](images/2024-04-21-16-45-26.png)

「仮想化」をクリック
![](images/2024-04-21-16-46-26.png)

![](images/2024-04-21-16-47-04.png)

「選択」でダウンロードしたISOイメージを選択して「続ける」
![](images/2024-04-21-16-55-42.png)


![](images/2024-04-21-16-51-25.png)

![](images/2024-04-21-16-52-13.png)

![](images/2024-04-21-16-53-10.png)


![](images/2024-04-21-16-58-14.png)

### ゲストOS（Ubuntu）のインストール

![](images/2024-04-21-16-59-05.png)

![](images/2024-04-21-16-59-28.png)

![](images/2024-04-21-17-01-28.png)

![](images/2024-04-21-17-01-57.png)

![](images/2024-04-21-17-03-36.png)

![](images/2024-04-21-17-05-46.png)

![](images/2024-04-21-17-08-03.png)

![](images/2024-04-21-17-10-04.png)

![](images/2024-04-21-17-11-56.png)

![](images/2024-04-21-17-13-28.png)

![](images/2024-04-21-17-14-48.png)

![](images/2024-04-21-17-23-38.png)

![](images/2024-04-21-17-25-26.png)

![](images/2024-04-21-17-27-18.png)

![](images/2024-04-21-17-34-37.png)

![](images/2024-04-21-17-35-11.png)

![](images/2024-04-21-17-37-14.png)



画面が黒くなったら
ドライブイメージのオプション（右から3番目、光学ディスクのアイコン）をクリックして「CD/DVD（ISO）イメージ」を「取り出す」
再起動ボタン　◁　（左側、名前の脇にあるアイコン）をクリックして再起動

![](images/2024-04-21-17-44-49.png)

![](images/2024-04-21-17-52-19.png)

![](images/2024-04-21-17-54-26.png)

![](images/2024-04-21-17-55-02.png)

![](images/2024-04-21-17-56-16.png)

![](images/2024-04-21-18-00-49.png)

![](images/2024-04-21-18-05-37.png)

![](images/2024-04-21-18-14-12.png)

![](images/2024-04-21-18-15-52.png)

![](images/2024-04-21-18-17-00.png)

![](images/2024-04-21-18-17-47.png)

![](images/2024-04-21-18-18-32.png)


### Ubuntuデスクトップ
Ubuntuデスクトップは以下の様な画面構成になっています。

![](images/2024-04-21-18-25-27.png)

### 端末アプリ

コマンドは「端末」アプリケーションで入力して実行します。アプリはdockに登録できます

![](images/2024-04-21-18-24-09.png)

----
[TCP/IP＆ネットワークコマンド入門 サポートページ](https://nisim-m.github.io/tcpipcmdbook/)
