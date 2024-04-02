# Lesson1 : LEDをチカチカさせよう

## 1. 基本的な操作
Arduino IDEを起動して新規スケッチを作ると下のような画面が出てくる
![](res/lesson1-LED/normalscreen.png)

- void setup(){} : 一回だけ実行される。設定とかを書く。
- void loop(){} : 繰り返し実行される。設定以外のものを書く。

## 2.LEDを光らせよう
下のようなプログラムを書こう
```c++
void setup(){
    pinMode(2, OUTPUT);
}

void loop(){
    digitalWrite(2, HIGH);
}
```
### コードの説明 
- pinMode(2, OUTPUT);

    pinMode(使用したいピン, OUTPUT or INPUT)

    ピンは基板であらかじめ決められている番号

    使用するピンは自由に決めることができる（今回は基板についているLEDを使うので2 pin）

    LEDは出力なのでOUTPUT

    ピンは一度決めたらそのまま使用するのでsetupに書く

    **参考 esp32 miniのピン配置**
    ![](res/lesson1-LED/esp32pin.png)
    GPIO2と書いてあるピンが使用したピン

    横にLED0とあるためもともとLEDをして使用することができる

- digitalWrite(2, HIGH);

    digitalWrite(使用したいピン, HIGH or LOW);

    ピンはpinModeで設定したピンのみ使用できる

    HIGHなら5 V, LOWなら0 Vを出力（HIGHなら光る）

    LEDは継続的に光らせたいのでloopに書く（この場合はsetupでも変わらないがloopに書こう）

### マイコンに書き込んでみよう
- ボードを選択

ツール→ボード→esp32→ESP32 Dev Module
![](res/lesson1-LED/board.png)

- ポートを選択

基板とパソコンをUSBで接続

ツール→ポート→COMX(Xはパソコンによって異なる)
![](res/lesson1-LED/port.png)

- 書き込み

左上にある「→」をクリック

コンパイルが始まって正常にコンパイルされると書き込みが始まる

隣にあるチェックマークをクリックするとコンパイルだけしてくれる

基板を接続していない状態でも使用できるのでコードの確認のときとかに使おう
![](res/lesson1-LED/exe.png)

**うまくいくと赤く光っているLEDの隣のLEDが青色に光ります！**

## 3.LEDをチカチカさせよう
さっきのプログラムを少し変えて下のようにしよう
```c++
void setup(){
    pinMode(2, OUTPUT);
}

void loop(){
    digitalWrite(2, HIGH);
    delay(1000);
    digitalWrite(2, LOW);
    delay(1000);
}
```

### コードの説明
- delay(1000);

delay(mill sedonds);

()内の単位はミリ秒

今回は1000 ミリ秒なので1 秒

delayの中の秒数だけloop関数がその位置で止まる

このプログラムだとHIGHになった後一秒おいてLOW

**一秒ごとに点滅しましたか？**

**次回はシリアル通信と呼ばれるものをやります！**

**次のページからは応用編です。余裕があったら見てください**