# 7. USBメモリ(初級編)
## 目標
- USBメモリにファイルを作成することができるようになる
- 作成したファイルに文字を書きこむことができるようになる

## CubeMXの設定
1. 使用するピンを選択する
>今回は`PA11`を`USB_OTG_FS_DM`に，`PA12`を`USB_OTG_FS_DP`に設定する
>![](_res\USB_Pinout.png)

2. USB_OTG_FSの設定
>`mode`を`Host_Only`にする
>![](_res\USB_OTG_FS_Config.png)

3. USB_HOSTの有効化と設定
>`Middleware and Software Packs`から`USB_HOST`を選択
>`Class for FS IP`を`Mass Storage Host Class`に設定
>`Platform Settings`の`IPs or Compornents`を`GPIO Output`に設定
>`Platform Settings`の`Found Solutions`を`PA5`に設定
>![](_res\USB_Host_Config.png)

4. FATFSの有効化と設定
>`mode`は`USB Disk`を選択
>`Set Defines`の`Physical Drive Parameters`にある`MAX_SS`を`4096`に`MIN_SS`を`512`にする
>![](_res\FATFS_Config.png)

5. Clock周りの問題解決
>`Clock Configuration`を開くとエラーメッセージが出るのでYesを選択
>`Resolve Clock issues`を実行

## 期待される動作
USBメモリを挿した後しばらくしてからLEDが点灯する．
LED点灯後にUSBメモリを取り外してUSBメモリの中身を確認する．
USBメモリに書き込みたい内容の文章が書き込まれていたら成功．

## マイコンとUSB端子の接続
- USBのメス端子の`D+`にマイコンの`USB_OTG_FS_DP`を`D-`に`USB_OTG_FS_DM`を接続
- USBのメス端子のVbusには5Vを供給

## プログラミング
今回は`wrapper.hpp`や`wrapper.cpp`は作らない．
USBメモリの初回接続時には`USB_Init()`が呼び出される．
`USB_Init()`内で呼び出される`USBH_UserProcess()`内に処理を書くことでユーザー定義の処理を行うことができる．

## FATFSについての解説
FATFSとはファイルシステムを扱う際にハードウェアとソフトウェアの間でいい感じにしてくれるミドルウェアであり，USBメモリやSDカードを扱うことが可能である．

FATFSについての詳細やここで紹介しきれない関数群については[こちら](http://elm-chan.org/fsw/ff/)をご覧ください．

FATFSの公式ドキュメントには[日本語版アプリケーションノート](https://irtos.sourceforge.net/FAT32_ChaN/doc/ja/appnote.html)や[日本語版関数まとめ](https://irtos.sourceforge.net/FAT32_ChaN/doc/00index_j.html)もあるが英語版に比べて情報がやや古く，関数の引数が異なるなど重大な問題を抱えている．

### `f_mount(FATFS* fs, const TCHAR* path, BYTE opt)`
FATFSでは1つのドライブに対して1つのワークエリアを必要とするため，この関数を用いてワークスペースの登録，削除を行う．

第3引数ではオプションを指定できるがおそらく基本0で良いと思われる．詳しくは[こちら](http://elm-chan.org/fsw/ff/)をご覧ください．

### `f_open(FIL* fp, const TCHAR* path, BYTE mode)`
C言語の`f_open()`とほとんど同じ．

第3引数のモードの指定はC言語と若干異なるので[こちら](http://elm-chan.org/fsw/ff/doc/open.html)の表を参照

### `f_write(FIL* fp, const void* buff, UINT btw, UINT* bw)`
C言語の`f_write()`とほとんど同じ．

第3引数で書きこむデータのサイズを渡す．

第4引数で書き込んだ文字数を記録するための関数のポインタを渡す．

### `f_close(FIL* fp)`
C言語の`f_close()`とほとんど同じ．

この関数が実行されるまではフラッシュメモリのファイルに書き込むデータ類はキャッシュにあり，この関数が実行されることでフラッシュメモリに実際にデータが書き込まれる．

この関数を実行した後に同じファイルへ操作を行う際には再び`f_open()`を呼び出す必要がある．
なお、次に紹介する`f_sync()`を用いればその限りではない．

### `f_sync(FIL* fp)`
上で紹介した`f_close()`のうちフラッシュメモリへのデータの書き込みのみを抽出した関数．

これを用いることでフラッシュメモリにデータを書き込みつつ`f_open()`を用いずに同じファイルへの操作を継続できる．

## サンプルコード
### include
`usb_host.c`の28行目で`fatfs.h`をインクルードする
```c++
/* USER CODE BEGIN Includes */
# include "fatfs.h"
/* USER CODE END Includes */
```

### USBH_UserProcess()
`USBH_UserProcess`は`USb_Host/App/usb_host.c`の116行目にある．
fatfs.hによって導入される関数の引数(FATFS型の構造体やFIL型の構造体など)はfatfs.hで定義されている
```c++
f_mount(&USBHFatFS, (TCHAR *)USBHPath, 0);
f_open(&USBHFile, "test.txt", FA_CREATE_ALWAYS | FA_WRITE);
char text[]="You succeeded in writing text on USB-Memory\n";
f_write(&USBHFile, text, sizeof(text), (void *)&byteswritten);
f_close(&USBHFile);
HAL_GPIO_TogglePin(LD2_GPIO_Port, LD2_Pin);
```
