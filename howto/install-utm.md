
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

### ゲストOS（Ubuntu）のインストール


----
[TCP/IP＆ネットワークコマンド入門 サポートページ](https://nisim-m.github.io/tcpipcmdbook/)
