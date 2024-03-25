# Tera Termの使い方
## Tera Termとは
**Tera Term**はシリアルモニタとして使用できるWindows向けのソフトウェアの１つです．シリアルモニタ以外にも，Raspberry PiとSSHで接続するといった使い方もあります．

## セットアップ
1. [リリースページ(GitHub)](https://github.com/TeraTermProject/osdn-download/releases)より`teraterm-4.107.exe`をダウンロードする
2. `teraterm-4.107.exe`を実行する
3. `<TeraTermのインストールパス>/TERATERM.INI`を編集し，デバックモードを有効にする
> `Debug = off`を`Debug = on`にする．
> 
> デバックモードを有効にすることで，シリアルで受信したデータを16進数で表示することができる．
> そのため，シリアル通信を利用したマイコンのデバックに有効である．