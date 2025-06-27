# Lesson2 : シリアル通信を使用

## 1. シリアル通信とは
[このページ](https://nokolat.github.io/Programming_Documents/communication/#uart)
をご覧ください

## 2.Hello World
下のようなプログラムを書こう
```c++
void setup(){
    Serial.begin(9600);
}

void loop(){
    Serial.println("Hello World");
}
```
### コードの説明 
- Serial.begin(9600);

    - Serial.begin(baud rate);
    - baud rateはいくつか存在する

- Serial.println("Hello World");

    - Serial.println("文字列");
    - printだと改行なし、printlnだと改行あり
    - ""で囲む。変数を表示するときはそのまま""を使わずに書く

### 実行しよう
- 実行した後、画面右上のシリアルモニタをクリックして起動
![](res/lesson2-serial/setup-serialmonitor.png)

- シリアルモニタの右にあるbaud rateをSerial.beginで設定した値に変更
![](res/lesson2-serial/setup-baud.png)

### 実行結果
- シリアルモニタに永遠とHello Worldが表示される
![](res/lesson2-serial/result-HelloWorld.png)

**マイコンからパソコンに文字列を表示することに成功しました！**

## 3.Serial.printとASCII
シリアルモニタで入力した文字を表示するプログラムを書いてみよう
```c++
void setup(){
    Serial.begin(9600);
}

void loop(){
    if(Serial.available() > 0){
        int data = Serial.read();
        Serial.println(data, DEC);
    }
}
```

### コードの説明
- if(Serial.available() > 0){};
    - Serial.availableはシリアルを受信したときに受信したバイト数を返す関数（64 byteまで格納可能）
    - 受け取ると値が一つずつ減り受信したデータがなくなると0になる
    - 受信していないときは0を返す

- Serial.read();
    - Serial.read()は受信したデータを読み込む関数
    - 1 byteだけ読み込む

### 実行しよう
- シリアルモニタの入力画面で「a」と打ってenter
![](res/lesson2-serial/inputserial.png)

### 実行結果
- 「a」ではなく「97」「10」とでてきてしまう
![](res/lesson2-serial/outputserial.png)

### ASCIIコード
- Serial.printはASCIIコードとして処理している
- ASCIIコードとはアルファベットや記をが0 ~ 127の数字で表す
([ASCIIコード表](https://www3.nit.ac.jp/~tamura/ex2/ascii.html))
- 例えば「"a"」はASCIIコードでは「97」、「"\n"(改行)」は「10」で表される
- Serial.print(data, DED);は入力されたデータを10進数の数字としてASCIIコードに変換し送信する
    - data = "a"; のとき"a"はASCIIコードで10であるから送信されるデータは"10"(文字列)となる
- Serial.print(data); は入力されたデータをそのまま送信する
- 「Hello World」が正しく出力されたのは、Serial.printでは文字列はそのまま扱われるから

- ```Serial.print(data, DEC);``` を```Serial.write(data);``` に書き換えて実行してみよう

### 実行しよう
- シリアルモニタの入力画面で「a」と打ってenter
![](res/lesson2-serial/inputserial.png)

### 実行結果
- ちゃんと「a」と出力されました
![](res/lesson2-serial/correct-outputserial.png)

### コードの説明
- Serial.write();
    - Serial.read();でバイトに変換されたものをもう一度文字に変換する関数

## まとめ
Serial.printとASCIIコードについてやりました。同じ文字を送っているのに表示が違うのはややこしいかもしれませんがシリアル通信は今後もよく使うので頑張りましょう

次回はセンサーを使います！

加速度や気圧などの値を見ることができます！