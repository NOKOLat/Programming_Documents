# UART（Serial通信）とツールの使い方
## 目標
- STM32CubeIDEにおけるプロジェクト作成時の定形作業をできるようになる
  - [プロジェクトの作成](#プロジェクト作成)から[Wrapperの作成](#wrapperファイルの作成)までを定形作業とする
- HALライブラリの規則を知る
- UARTを使えるようになる

## プロジェクト作成
1. メニューバーから `File/New/STM32 Project` を選択する
>データのダウンロードが自動で行われることがある
2. マイコンの選択を行う
>   2. `Commercial Part Number`に "F446RE"と入力する
>   3. `STM32F446RET6`を選択する
>   4. 次へ進む
1. `Project Name`を入力する
> プロジェクト名は自分で分かれば何でも良いです．
> 特にこだわりがないなら`UART`としておきましょう．
1. `Targeted Language`を`c++`にする

## マイコンの設定（CubeMX）
1. conecctibityからUSART2を選択する
2. Modeを`Asynchronous`に設定する
3. `Baud Rate`を`115200`に設定する
4. マイコンの`PA2, PA3`がUART機能に割り当てられていることを確認する
>![](_res/uart_cubeMX.png)
5. 写真を参考にチェックを入れる
>このチェックを入れることを前提としてライブラリの作成等をしているので，忘れないようにしましょう．
>![](_res/cubeMX_codeGenerator.png)
マイコンの設定は`<Project Name>.ioc`のように拡張子が**ioc**のファイルに保存されている．このファイルを開くとマイコンの設定の画面が開く．

## プログラミング
### Wrapperファイルの作成
本環境ではmain関数がmain.c（C言語）に作成されるため，c++の機能を使用するためにWrapperを作成する必要がある．
1. `Core/Inc`に`wrapper.hpp`を作る
2. `Core/Src`に`wrapper.cpp`を作る

### wrapper.hpp
c++とc言語を繋ぐための関数のプロトタイプ宣言をしている．`extern "C" {}`で囲われた関数は内部的にc言語と同様に定義されるため，C言語のファイルから呼び出すことが可能である．基本的には，必要な機能はどのようなプロジェクトでも同じであるため，ファイルの中身はコピーして使いまわすことが多い．
### wrapper.cpp
Arduino環境でソースコードを書くときと同様にコードを書くファイルである．

## HALライブラリの解説
### 命名規則
STMマイコンのプログラムで使用する関数群のことを"**HALライブラリ**(HAL)"と呼ぶ．関数の命名規則は一貫して以下に従っている．
```
HAL_<periferal name>_<behavior>(<periferal handler>,...);
```
'<periferal name>'は`UART`や`GPIO`などのマイコンの機能の名前である．  
`<behaviot>`はどのような動作をするかを示している．例えば，通信で受信をするときには`Receive`のようになる．
`<behaviot>`は複数のアンダーバーで区切られていることがある．


### `HAL_UART_Transmit(UART_HandleTypeDef *huart, const uint8_t *pData, uint16_t Size, uint32_t Timeout)`
UARTでデータを送信するための関数である．`pData`には送信したい配列の先頭アドレスを渡す．`Size`は送信データの長さ(byte)である．
`Timeout`で指定した時間(ms)だけ通信が終了しなかった場合に関数を終了する．`Timeout`は通信処理が上手くいかないときに処理全体がフリーズすることを防ぐためにある．
`huart`にはペリフェラルのハンドラーのポインターを渡す．コード生成の時点でuart1であれば`huart1`のように自動で宣言されている．

## 期待する動作
本プログラムはシリアルモニタに接続した状態で起動することを想定している．  
起動時に`Hello World`と表示され，その後，`count:<number>`と表示され<number>がカウントアップされていく．

## サンプルコード
### wrapper.hpp
```c++
#ifndef INC_WRAPPER_HPP_
#define INC_WRAPPER_HPP_

#ifdef __cplusplus
extern "C" {
#endif

void init(void);
void loop(void);

#ifdef __cplusplus
};
#endif

#endif /* INC_WRAPPER_HPP_ */
```
### wrapper.cpp
```c++
#include "usart.h"
#include <string>


void init(){
	uint8_t str[] = "Hello World";
	HAL_UART_Transmit(&huart2, str,11,100);
}

void loop(){
	static uint16_t count = 0;
	std::string str = "count:"+std::to_string(count++);
	HAL_UART_Transmit(&huart2, (uint8_t *)str.c_str(),str.length(),100);
}
```

### main.c
```c
/* Private includes ----------------------------------------------------------*/
/* USER CODE BEGIN Includes */
#include "wrapper.hpp"
/* USER CODE END Includes */
```
```c
  /* USER CODE BEGIN 2 */
  init();
  /* USER CODE END 2 */

  /* Infinite loop */
  /* USER CODE BEGIN WHILE */
  while (1)
  {
	  loop();
    /* USER CODE END WHILE */

    /* USER CODE BEGIN 3 */
  }
  /* USER CODE END 3 */
```
## Reference
[Sample code](https://github.com/NOKOLat/STM32Document_Serial)
