# 3. UART通信(serial)
## 目標
- Serialの基本的な機能を使えるようななる
- Arduino IDEのシリアルモニターの使い方を学ぶ

## シリアル通信とは

## コード解説
### `void Serial.begin(unsigned long baud, byte config)`
第1引数ではシリアル通信の通信速度を決定する．*9600*や*115200*がよく使用される．
第2引数はシリアル通信のフォーマットを決定する．デフォルト値を使用する場合は省略して構わない．
フォーマットについては別途，解説する．

### `size_t Serial.println(const String &s)`
第１引数に送信したい文字列を入れることで文字列の送信をする．
この関数では送信文字列の最後に改行コードが自動で追加される．
引数の型はいくつかあり，String型以外でも使用可能なので調べて使って欲しい．

### `int Serial.available()`
Serial通信で受信したデータの長さを取得する．返り値が*0*でないときは，何かしらのデータを受信したときである．

`while(!Serial.available());`とすることで，データを受信するまで待機することができる．

### `String Serial.readString()`
Serial通信で受信したデータを文字列として取得するための関数である．

### `str.trim()`
文字列の最後についている改行コードを切り取るために使用している．

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
  while(!Serial.available());
  String　str = Serial.readString();
  str.trim();
  Serial.print("Received Str is \"");
  Serial.print(str);
  Serial.println("\"");
}
```