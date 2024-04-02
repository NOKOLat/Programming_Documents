# Gitの使い方
## Gitとは
Gitはバージョン管理システムの1つである．主にソースコードの管理に使用している．また，部内大会のルールブックの公開，質問の受付にも使用した．

GitにはPC上に保存するローカルリポジトリと，GitHubなどのオンラインサービス上に保存するリモートリポジトリがある．
ローカルで編集内容をもとにバージョンを作成する操作を**commit**という．
ローカルの**commit**をリモートにアップロードする操作を**push**という．
リモート上の変更をローカルにダウンロードする操作を**pull**という．
リモートリポジトリとローカルリポジトリを比較して，ローカルに反映されていない変更がリモート上に存在するかを確認する操作を**fetch**という．

Gitはコマンドで使用するほうが良いが，GUIアプリケーションも存在している．Gitに不慣れなうちはGUIアプリケーションを使うほうがわかりやすい．Windows向けなら，[soruce tree](https://www.sourcetreeapp.com/)の使用をおすすめします．

## Githubの初期設定
ref : [Github Docs / Connecting to GitHub with SSH](https://docs.github.com/en/authentication/connecting-to-github-with-ssh)

1. SSH keyの生成
> ターミナルで以下の`ssh-keygen`を実行する．
2. keyファイルのリネーム
>.sshフォルダにある`id_rsa`を任意の名前 (ex: `github-key`)に変更する
>.sshフォルダにある`id_rsa.pub`を任意の名前 (ex: `github-key.pub`)に変更する
3. 公開キーの登録
>   1. github-key.pubファイルを開いて内容をコピー
>   2. githubで`New SSH Key`を選択
>   3. Title : `任意の名前`
>   4. Key : 1.でコピーしたものを貼り付ける

windowsだとAuthorized keyに登録する必要があるらしい

