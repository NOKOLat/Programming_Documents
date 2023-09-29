# Lチカ
## LEDの点滅 
[source code](#source-code)と同じプログラムを作成してください．任意に変更して構いませんが，スケッチの名前は`Nucleo64_LED`とします．

### ボードの選択
- ボードを`Nucleo-64`に設定する
![](res/boardSelection.png)

- **Board Part Number**を`Nucleo F411RE`に設定する．
使用するボードが違う場合は適宜変更すること．
![](res/boardPartNumber.png)

### プログラムのコンパイルおよび書き込み
画面左上の**書き込み**ボタンを押す

### コードの解説
#### `void setup()`
Arduinoにおいて，起動した際に1回のみ実行される関数
#### `void loop()`
`setup()`の実行後に，繰り返し実行される関数
#### `void pinMode(uint32_t ulPin, uint32_t ulMode)`
`ulPin`で指定したピンのモードを設定するための関数である．
モードは以下のものがある．

- *INPUT* 
    - 入力モード
- *OUTPUT*
    - 出力モード
- *INPUT_PULLUP*
    - 入力モード
    - プルアップ　
- *INPUT_FLOATING*
    - 入力モード
    - フローティング
- *INPUT_PULLDOWN*
    - 入力モード
    - プルダウン
- *OUTPUT_OPEN_DRAIN*
    - 出力モード
    - オープンドレイン
        
#### `void digitalWrite(uint32_t ulPin, uint32_t dwVal)`
`ulPin`で指定したピンを*HIGH*もしくは*LOW*に設定する．
いま，*HIGH*にするとLEDが点灯する．

## スイッチと同期したLEDの点灯
最も簡単な入力であるスイッチをし，スイッチを押しているときLEDが点灯するプログラムを作成する．

### コード解説
#### `int digitalRead(uint32_t ulPin)`
`ulPin`で指定したピンが*HIGH*であるか，*LOW*であるかを取得する．
ピンを*PULLUP*にしているので，*HIGH*のときスイッチは押されていない．スイッチが押されると*LOW*になる．


## source code
### LEDの点滅
``` c++ 
void setup() {
  // put your setup code here, to run once:
  pinMode(13, OUTPUT);
}

void loop() {
  // put your main code here, to run repeatedly:
  digitalWrite(13, HIGH);
  delay(200);
  digitalWrite(13, LOW);
  delay(200);
}
```
### スイッチと同期したLEDの点灯
```c++
void setup() {
  // put your setup code here, to run once:
  pinMode(13, OUTPUT);
  pinMode(12, INPUT_PULLUP);
}

void loop() {
  // put your main code here, to run repeatedly:
  if(digitalRead(12)){
    digitalWrite(13, HIGH);
  }else{
    digitalWrite(13, LOW);
  }
}
```