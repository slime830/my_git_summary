# Git コマンドまとめ

## 基本操作

- 初期化  
   任意のディレクトリをローカルリポジトリ化する。

  ```bash
  git init
  ```

  対象ディレクトリ直下で実行

- 現在の状態を確認する

  ```bash
  git status
  ```

- クローン  
  リモートリポジトリと同一の内容のローカルリポジトリを作る

  ```bash
  git clone URL
  ```

- リモートリポジトリの更新を、ローカルリポジトリに取り込む

  ```bash
  git pull remote_name remote_branch_name
  ```

  - `remote_name` : リモートリポジトリの指定。「origin」が一般的。

  - `remote_branch_name` ：リモートリポジトリ側のブランチ指定。

- ローカルリポジトリの更新を、リモートリポジトリに反映

  ```bash
  git push remote_name remote_branch_name
  ```

  - `remote_name` : リモートリポジトリの指定。「origin」が一般的。詳しくは後述

  - `remote_branch_name` ：リモートリポジトリ側のブランチ指定。詳しくは後述

- ファイル名変更、ファイル移動

  ```bash
  git mv old_filepath new_filepath
  ```

  - `old_filepath`：移動させるファイルパス（移動元）
  - `new_filepath`：移動先のファイルパス（移動先）

  「切り取り」のイメージ

  UNIX の`mv`コマンドを理解した方が早い。Windows コマンドプロンプトで`mv`使えないけど。
  よし、まずは powershell を使おう。そして慣れたら Linux（WSL2） にしよう(布教)

- ファイル削除

  ```bash
  git rm filepath
  ```

  - `filepath`：削除するファイルパス

  Git の管理上から除外し、ファイル自体も削除する。

## ブランチ操作

- ブランチ一覧確認

  ```bash
  git branch
  ```

- ブランチ作成

  ```bash
  git branch branch_name
  ```

  `branch_name`という名のブランチができる

- ブランチ移動
  ```bash
  git checkout branch_name
  ```
  `branch_name`という名のブランチに移動する

## merge

- マージ
  ```bash
  git merge branch_name
  ```
  `branch_name`の内容を、カレントブランチに取り込む（マージする）

## rebase

- ブランチを「繋げる」（マージとは違う）

  ```bash
  git rebase branch_name
  ```

  - `branch_name`：つなげ元にするブランチ名

  ※「繋げる」とは？rebase の挙動とは？

  ```
  ブランチX ⇒ コミットA ⇒ コミットB
  　　⇓
  ブランチY ⇒ コミットC ⇒ コミットD
  ```

  このような開発をしていた時に、それぞれ

  - rebase の場合

  ```
  ブランチX ⇒ コミットA ⇒ コミットB ⇒ コミットC ⇒ コミットD
  ```

  - merge の場合

  ```
  ブランチX ⇒ コミットA ⇒ コミットB ⇒ マージコミット
  　　⇓                               ⇗
  ブランチY ⇒ コミットC ⇒ コミットD ⇗
  ```

  rebase はブランチ X でブランチ Y のコミットをやり直してるイメージ。

  merge はブランチ Y の全ての変更を、一度のコミットで取り込んでるイメージ

- 複数コミットを 1 つに纏める
  ```bash
  git rebase -i commit_id
  ```
  - `commit_id`：纏め始める 1 つ前のコミット ID（ハッシュ値）

## コミット一覧の確認

- コミット一覧の確認
  ```bash
  git log
  ```

## リモートリポジトリの設定

- リモートリポジトリ一覧確認

  ```bash
  git remote -v
  ```

- リモートリポジトリを設定

  ```bash
  git remote add remote_name remote_repository
  ```

  - `remote_name` : リモートリポジトリの登録名。一般的には`origin`

  - `remote_repository` ：2 つの指定方法
    - URL :例：https://github.com/slime830/slime830.github.io.git
    - SSH :例：git@github.com:slime830/slime830.github.io.git

  どちらでも可。

  ただし、GitHub はパスワード認証を廃止したので、URL だと多分 push できない

  SSH は SSH で、Key Set を作って GitHub に登録して…ってやる。めんどくさいね

  GitHub への SSH 接続方法は[こっち](./github_ssh.md)に纏めます

## コミットの取り消し

- リセット

  ```bash
  git reset option commit_id
  ```

  `commit_id` で指定したコミットまで戻る。

  - `option`：リセット取り消し方法を指定する

    - `--hard`:対象ファイルの中身が、戻ったコミットの内容に自動更新される

    - `--soft`：対象ファイルの中身は、コマンド実行前後で変化しない

  - `commit_id`：コミットを指定する
    - コミット ID（ハッシュ値 ：`git log`でとってくる
    - 先頭からいくつか ： `HEAD^` のように、"HEAD"+ ("^" × n)で、先頭から n 番目のコミットまで戻る

## ユーザ指定

- ユーザ名確認
  ```bash
  git config --global user.name
  ```
- ユーザ名変更

  ```bash
  git config --global user.name "user_name"
  ```

- メールアドレス確認

  ```bash
  git config --global user.email
  ```

- ユーザ名確認
  ```bash
  git config --global user.email "hogehoge@hoge.co.jp"
  ```

## 特殊ファイル

- .gitkeep  
  空ファイル

  Git の挙動上、「空ディレクトリ」は Git の管理から外れる

  しかし、「空ディレクトリが存在していることを Git に認識させたい」時が来ると、困る

  その際には「.gitkeep」というファイルを作成して、Git に認識させる。
  ファイル名は本来なんでもいいけど、「意図的に空ファイル使ってます」っていう作法として、「.gitkeep」にする

- .gitignore  
  Git の追跡対象外にするファイルを記述する

  以下例を交えて

  ```
  /hoge.txt
  /hoge_directory/*
  /hoge_directory2/*.txt
  !/hoge_directory/keep.txt
  ```

  1. リポジトリ直下の`hoge.txt`を追跡対象外にする
  2. `hoge_directory`の中にある全てのファイルを追跡対象外にする。
  3. `hoge_directory2`の中にある全ての`txt`ファイルを追跡対象外にする
  4. `hoge_dicectory`の中にある全てのファイルが追跡対象外になっているが、`keep.txt`だけは追跡対象外としない

  「\*」はワイルドカードと呼ばれます。これは「任意の文字の 0 以上の連続」を表します。

  先頭に「!」を付けると、逆に追跡する（追跡対象外としない）ファイルを表します。

  追跡対象外とされたファイルは、`add`や`commit`の対象になりません
