# ドキュメントの書き方
本頁では本ドキュメントを更新するに当たって必要と思われることを述べる．
まず，環境構築のような初回のみ必要なことについて述べる．

## 初期設定
本ドキュメントはmarkdown形式で執筆しており，githubを用いて管理している．
また，mkdocsを使用し,github pagesにて公開している．

1. [github](https://github.com/NOKOLat/Programming_Documents)からリポジトリをクローンする  
   `git clone git@github.com:NOKOLat/Programming_Documents.git`
2. クローンしたフォルダに移動する  
    `cd Programming_Documents`
3. git-flowの設定をする  
   ターミナルもしくはSourceTreeのようなGUIツールから設定できる
    - `git flow init`コマンドを使用する  
    - GUIツールのGit Flowボタンを押す
```
*/Programing_DOcuments$ git flow init

Which branch should be used for bringing forth production releases? 
- master
Branch name for production releases: [master]
Branch name for "next release" development: [develop]

How to name your supporting branch prefixes?
Feature branches? [feature/]
Bugfix branches? [bugfix/]
Release branches? [release/]
Hotfix branches? [hotfix/]
Support branches? [support/]
Version tag prefix? []
Hooks and filters directory? [F:/tmp/Programming_Documents/.git/hooks]
```
4. mkdocsに必要なpythonライブラリをインストールする  
   `pip install -r ./requirements.txt`

## 新規ページの作成
1. 作業用の`feature/xxx`ブランチを作成する．
    `xxx`は編集内容を端的に表す単語がよい (ESP32, STM32など)  
    すでにブランチが存在するときはチェックアウトする
2. docsフォルダ内の適切なパスにマークダウンファイル(`*.md`)を作成する
3. mkdocs.ymlの51行目移行の適切な位置に作成したファイルを追記する  
    追記内容は，デプロイ後の画面左側に作成される目次に反映される
4. プレビューを確認しながらマークダウンファイルを編集する  
    `mkdocs serve`コマンドでローカルサーバーを立ち上げる  
    コマンド出力からリンクを確認し，ブラウザで表示する
    ![](./_res/res_mkdocs_serve.png)

## ページの編集
1. 作業用の`feature/xxx`ブランチを作成する．
    `xxx`は編集内容を端的に表す単語がよい (ESP32, STM32など)  
    すでにブランチが存在するときはチェックアウトする
2. プレビューを確認しながらファイルを編集する  
    `mkdocs serve`コマンドでローカルサーバーを立ち上げる  
    コマンド出力からリンクを確認し，ブラウザで表示する
    ![](./_res/res_mkdocs_serve.png)

## webページへの反映
1. 編集が完了したファイルをコミットする  
      コミットは適宜行うとよい
2. 作業用ブランチをpushする
3. github上でpull requestを作成し，レビューをうける
4. レビューが終わりmasterにマージされると編集がwebページに反映される