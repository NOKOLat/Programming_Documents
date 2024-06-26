# 3. UART通信(serial)
## 目標
- Serialの基本的な機能を使えるようななる
- Arduino IDEのシリアルモニターの使い方を学ぶ

## シリアル通信とは

## シリアルモニタの使用方法
### シリアルモニタとは
PCがマイコンから受信したデータを表示したり，PCからマイコンへデータを送信するためのものである．  
### 使用方法
- 画面右上のボタンもしくはメニューバーのツールからシリアルモニタを選択することでシリアルモニタを開く
    - シリアルモニタは画面下部に表示される
- シリアルモニタの上部にはPCから送信する文字列を入力するボックスと送信文字列の改行コード，通信速度の設定がある
    - 通信速度はソースコードで指定した値(9600bps)にする必要がある
- 受信した文字列はシリアルモニタに表示される
![](./res/serialMonitor.png)
### その他
Arduino IDEに内蔵されているシリアルモニタ以外にも様々なシリアルモニタが存在する．
Arduino IDE内蔵のものでは，文字列以外のデータを表示することには適さない．
デバックモードを有効にしたTera TermやYAT, CuteComなどを使用したほうが，シリアルモニタとしての自由度は高い．

## コード解説
### `void Serial.begin(unsigned long baud, byte config)`
第1引数ではシリアル通信の通信速度を決定する．*9600*や*115200*がよく使用される．
第2引数はシリアル通信のフォーマットを決定する．デフォルト値を使用する場合は省略して構わない．
フォーマットについては別途，解説する．

### `size_t Serial.println(const String &s)`
第１引数に送信したい文字列を入れることで文字列の送信をする．
この関数では送信文字列の最後に改行コードが自動で追加される．
引数の型はいくつかあり，String型以外でも使用可能なので調べて使って欲しい．

### `size_t Serial.print(const String &s)`
第１引数に送信したい文字列を入れることで文字列の送信をする．
引数の型はいくつかあり，String型以外でも使用可能なので調べて使って欲しい．

### `int Serial.available()`
Serial通信で受信したデータの長さを取得する．返り値が*0*でないときは，何かしらのデータを受信したときである．

`while(!Serial.available());`とすることで，データを受信するまで待機することができる．

### `String Serial.readString()`
Serial通信で受信したデータを文字列として取得するための関数である．

### `String::trim()` (`str.trim()`)
受信した文字列の最後についている改行コードを切り取るために使用している．

## コードの実行結果
シリアルモニタを開いた状態でプログラムを書き込むと（マイコンをリセットすると）はじめに「Hello world」と表示される．
2行目には「waiting...」と表示され，マイコンは文字列の受信待機状態となる．
シリアルモニタからメッセージ「*任意のメッセージ*」を送信すると，「Received: *任意のメッセージ*」と表示される．

## Source Code
```c++
void setup() {
  // put your setup code here, to run once:
  Serial.begin(9600, SERIAL_8N1);
  
  Serial.println("hello world");

}

void loop() {
  // put your main code here, to run repeatedly:
  Serial.println("waiting...");
  String str;
  while(!Serial.available());
  str = Serial.readString();
  str.trim();
  Serial.print("Receive : ");
  Serial.println(str);
}
```