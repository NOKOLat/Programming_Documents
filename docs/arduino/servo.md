# 4. SERVO
## 目標
- サーボモータを制御する原理を習得する
- マイコンからサーボモータを制御できるようになる

## サーボモータの制御方法
サーボモータは信号線に[**PWM**](https://www.rohm.co.jp/electronics-basics/micon/mi_what11)を入力することで制御する．
PWMのパルス幅とサーボモータの角度が対応している．
PWMの周期はおよそ2500usであり，使用するパルス幅は500usから2200usである．

## コード解説
### `Servo servo;`
`Servo`クラスのインスタンスを作成している．
クラス(class)はc++の機能の1つで，c言語の構造体に似たものである．
関数と変数を1つのまとまりとして，新しい型を定義している．

### `servo.attach(7)`
D7ピンをサーボモータに接続し使用することを設定している．

### `servo.writeMicroseconds(int value)`
PWMのパルス幅をミリ秒単位で設定する．最大値は`2500`くらいのはず．

## Source Code
```c++
#include <Servo.h> 

Servo servo;

void setup() {
  // put your setup code here, to run once:
  Serial.begin(9600);
  servo.attach(7);
}

void loop() {
  // put your main code here, to run repeatedly:
  static uint16_t count = 0;
  uint16_t value = (500*sin(count++/1000.0*2.0*M_PI)+1600);

  servo.writeMicroseconds(value);
  Serial.println(value);
  delay(2);
}
```