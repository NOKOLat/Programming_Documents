# 環境構築とLチカ
STMマイコンのプログラムを書く際に必要となるソフトウェアのインストールからテストコードの作成・実行までを解説する．
## ソフトウェアのインストール
ここでは，マイコンのプログラを書くにあたって有益なソフトウェアを紹介する．本ドキュメントでは使用しないソフトウェアも入っているため，ここに紹介するソフトウェアをすべてインストールする必要はない．ただし，[STM32CubeIDE](#stm32cubeide)だけは必ずインストールしてほしい．


ST社のソフトウェアのダウンロードには名前，メールアドレスの入力もしくは，ログインが必要となる．
ここでは，[アカウントを作成する](https://www.st.com/content/st_com/ja/user-registration.html?referrer=https%3a%2f%2fwww.st.com%2fcontent%2fst_com%2fja.html)ことを推奨する．
### [STM32CubeIDE](https://www.st.com/ja/development-tools/stm32cubeide.html)
本ソフトウェアはSTM32用統合開発環境であり，ペリフェラル設定からプログラムの書き込み，デバックまでを一貫して行うことができる．STMマイコンの開発を行ううえでほぼ必須となるソフトウェアである．

[webサイト](https://www.st.com/ja/development-tools/stm32cubeide.html)にアクセスしたら **ソフトウェアの入手** より各自の環境に合う最新のものをダウンロード，インストールしてほしい．Windowsであれば **STM32CubeIDE-Win** が該当する．

### [STM Studio](https://www.st.com/ja/development-tools/stm-studio-stm32.html)
グローバル変数の値をほぼリアルタイムでモニタリングするためのソフトウェアである．記事の執筆時点において *NRND* となっているため，近いうちに提供が終了となる可能性がある．後継のソフトウェアとして[STM32CubeMonitor](#stm32cubemonitor)が存在する．

### [STM32CubeMonitor](https://www.st.com/ja/development-tools/stm32cubeprog.html)
動作中のマイコンをモニタリングするためのソフトウェアである．筆者は本ソフトウェアにあまり触れていないため詳しいことは分からない．

### [STM32CubeProgrammer](https://www.st.com/ja/development-tools/stm32cubeprog.html)
実行ファイルをマイコンに書き込むために使用するソフトウェアである．マイコンへの書き込みはSTM32CubeIDEでも可能であるが，こちらの方が高機能である．

## STM32CubeIDEを使ってLチカをする
「Lチカ」とはLEDを点滅させることを言う．プログラミングを学んだことがあれば "Hello World" に出会ったことがあると思うが，Lチカはそれのマイコン版だと考えてほしい．

### STM32CubeIDEの起動
![](_res/environmentBuilding/start_cubeide.avif)  
1. STM32CubeIDEを起動すると，Launcherが表示される  
2. **workspace**の選択をする
>"workspace" とはSTM32CubeIDEで作成するプロジェクトを置く場所である．デフォルトのまま `Launch` を押して問題ない．</dd>
3. STM32CubeIDEが起動が完了する
>初回起動時は `Infomation Center` が開かれているが，特に必要ないのでそのタブは消してよい．

### Projectの作成
1. メニューバーから `File/New/STM32 Project` を選択する
>データのダウンロードが自動で行われることがある
2. マイコンの選択を行う
>   1. `Board Selector`タブを選択する
>   2. `Commercial Part Number`に "NUCLEO-F446RE"と入力する
>   3. ボードを選択する
>   4. 次へ進む
>   ![](_res/environmentBuilding/Board-select.png)
3. `Project Name`を入力する
>![](_res/environmentBuilding/projectName.png)

以上で，プロジェクトの作成は終わりである．3の後でいくつかダイアログボックスが表示されることがあるが，すべて `Yes/OK` を選べば問題ない．

### ペリフェラルの設定
ここでは，マイコンに搭載されている機能のうち **何を使用するか** と **どのピンを使用するか** を設定する．今回はNucleoボードに搭載されているLEDを使用するため，ここでの設定事項は特にないが，画面の説明だけ記載しておく．

赤で囲われた部分にはマイコンが搭載している機能が一覧形式で表示されている．ここから設定したい項目を選択すると，黄色で囲われた部分が表示される．ここでは，各機能の設定ができる．

青で囲われた部分ではマイコンのどのピンを使用するかを確認・設定できる．また，各ピンにはPA0,PA1,...,PC15,... のように名前が割り当てられている．この名前はソースコードを書く際に，ピンを指定するために必要となる．

`PA5`（下の列の左から5番目） に `LD2[Green Led]`とある．このピンが今回のプログラムで点滅させるLEDに接続されている．いまは特に設定することはないのでこのタブは閉じてよい．

![](_res/environmentSetUp/cubeMX.png)

### プログラミング
プロジェクトを作成したとき，ペリフェラルの設定を変更したときに，その設定を反映したプログラムが自動生成される．自動生成されたコードとユーザーが書いたコードを区別するために，自動生成されたファイルにはユーザーがコードを書いていい範囲が決まっているため注意が必要である．
`/* USER CODE BEGIN xxxxx */` と `/* USER CODE END xxxxx */` の間の記述はユーザーのコードとして認識される．

`Project Name/Core/Src/main.c`にプログラムを書いていく．編集が必要な部分を以下に示す．

```c
 /* USER CODE BEGIN WHILE */
  while (1)
  {
    HAL_GPIO_WritePin(GPIOA, GPIO_PIN_5, GPIO_PIN_SET);
    HAL_Delay(500);
    HAL_GPIO_WritePin(GPIOA, GPIO_PIN_5, GPIO_PIN_RESET);
    HAL_Delay(500);
    /* USER CODE END WHILE */

    /* USER CODE BEGIN 3 */
  }
  /* USER CODE END 3 */
```

## Refarences
- [Sample Code (Github)](https://github.com/NOKOLat/LED-Brink)
