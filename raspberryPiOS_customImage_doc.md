# RaspberryPi OSのカスタムイメージについて

## 目次

- [作成環境](#作成環境)
- [カスタム内容](#カスタム内容)
- [使い方](#使い方)
- [テストで使う場合](#テストで使う場合)
- [(おまけ)カスタムイメージの作り方](#おまけカスタムイメージの作り方)

## 作成環境

カスタムイメージの作成やテストは以下の環境で実施済みです。

- Windows10 Home 64bit
- Ubuntu20.04 64bit
- [SDFormatter](https://www.sdcard.org/jp/downloads/formatter_4/)
- [Win32 Disk Imager](https://sourceforge.net/projects/win32diskimager/)
- MicroSD 16GB(8GB以上であればインストール可能のハズ。未テスト)  

## カスタム内容

現在のカスタムイメージ(v1.0)についての詳細は以下の通りです。

- ベース  
	Raspberri Pi OS Lite 2020-08-20 (Kernel version: 5.4)  

- 変更点  
  - SSHポートの変更  
  - RaspberryPiデフォルトのユーザ名、パスワードの変更  
  - 公開鍵の設定
  - 温度データをgithubにコミットするためのgit設定(ダミーユーザの設定)  
  - cron  
    - MyDNSへのIP通知シェル(2時間毎の0分)
    - 温度の測定(15分毎)
    - 測定した温度データのコミット&プッシュ(3時間毎の5分)

## 使い方

作成済みのRaspberryPi OSのカスタムイメージを使用する場合は、  
通常のRaspberryPi iOSイメージのインストールと同様にインストールできます。  
詳細な手順などは、それぞれ検索してください。  

1. RaspberryPi OSのカスタムイメージを[Download](https://drive.google.com/file/d/1axgIbWABdEwuXMoO-NZU4pfbGTPdrRoA/view?usp=sharing)します。

2. SDFormatterで、microSDカードをフォーマットし、  
   Win32SDiskImagerでOSのインストール完了。  
   自動で容量に応じたパーティションサイズに拡張されます。  

## テストで使う場合

テストで使用する場合は以下の手順でcronを停止させてください。  
cronが動作したままの場合、本番環境のラズパイにSSHで接続できなくなる可能性があります。  

1. terminalを立ち上げて、以下のコマンドを入力してください。
   ```
   $ crontab -e  
   ```
2. 以下の行をコメントアウトしてください。
   ```
   0 */2 * * * /home/kot-pi/mydns-notice.sh
   5 */3 * * * /home/kot-pi/data-push.sh
   */15 * * * * cd /home/kot-pi/measurementData/test; /home/kot-pi/measurementData/test/Temp_Humidi
   ```
   ↓
   以下のように修正します。
   ```
   #0 */2 * * * /home/kot-pi/mydns-notice.sh
   #5 */3 * * * /home/kot-pi/data-push.sh
   #*/15 * * * * cd /home/kot-pi/measurementData/test; /home/kot-pi/measurementData/test/Temp_Humidi
   ```

## (おまけ)カスタムイメージの作り方

ブログにまとめてあるので[こちら](https://gari30.hatenablog.com/entry/2020/09/26/055225)を見てください。
