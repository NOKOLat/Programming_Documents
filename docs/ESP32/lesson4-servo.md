# Lesson4 : サーボを使おう

## 1. サーボとは
- ラジコン飛行機の翼を動かすためなどに使うモーター
- 角度や回転速度を指定することでしてした通りに動作しその状態を保持しようとする
- 航空研ではE-MAXデジタルマイクロサーボ ES9051やES9251などを使用している
- PWM制御をしている

## 2.ライブラリのインストール
1. 「esp32 servo library」と検索して[githubのサイト](https://github.com/jkb-git/ESP32Servo)にアクセスしよう
1. 「code→download zip」でライブラリのzipファイルをダウンロード
1. [zipファイルのライブラリをインストールする方法](lesson3-sensor.md)でライブラリをインストールしよう 

**これでサーボを使う準備ができました！**

## 3.サーボを動かすプログラムを書いてみよう
下のプログラムを書いてみよう
```c++
#include <ESP32_Servo.h>

Servo servotest;

void setup() {
  servotest.attach(5);
}

void loop() {
  servotest.write(0);
  delay(3000);
  servotest.write(180);
  delay(3000);
}
```
### コードの説明
- include
    - ヘッダファイルのインクルード
    - 「;」は必要ない
- Servo servotest;
    - Servo サーボの名前;
    - 使用するサーボに名前を付けて変数のように宣言する
- servotest.attach(5);
    - サーボ名.attach(ピン番号);
    - サーボに使用するピンを指定する
    - esp32 miniなら「GPIO」などをサーボを動かすために使用することができる
- servotest.write(0);
    - servotest.write(0~180);
    - サーボを動かす値を指定する
    - 0 ~ 180の値を入れる

### 配線
- サーボには「信号線」「5V線」「グランド線(GND)」がある
- それぞれ信号線は白や黄色、5V線は赤色、GND線は黒色や茶色とだいたい色は決まっている
![](res/lesson4-servo/servo.png)
- 信号線をGPIO5に、5VをVCCに、GNDはGNDにつなごう

### 実行してみよう
- サーボが3秒ごとに180°動くはずです

## まとめ
サーボを動かすことに成功しました！

次はサーボをプロポからの信号で動かしてみましょう！