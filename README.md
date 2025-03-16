git練習
===
<!-- toc --><!-- markdown-toc start - Don't edit this section. Run M-x markdown-toc-refresh-toc -->
**Table of Contents**

- [git練習](#git)
- [1.gitコマンド](#1git)
  - [1.1.add/commit/push関連](#11addcommitpush)
  - [1.2.ブランチ関連 (branch/checkout)](#12-branchcheckout)
  - [1.3.フェッチする (fetch)](#13-fetch)
  - [1.4.マージする (merge)](#14-merge)
  - [1.5.変更を元の状態に戻す/打ち消す (reset/revert)](#15-resetrevert)
  - [1.6.ログを表示する (log)](#16-log)
  - [1.7.タグを付ける (tag)](#17-tag)
  - [1.8.変更中の内容を退避させる (stash)](#18-stash)
  - [1.9.rebase関連 (rebase/cherry pick)](#19rebase-rebasecherry-pick)
  - [1.10.その他](#110)
- [2.メモ](#2)
- [3.Githubを用いた開発の流れ](#3github)

<!-- markdown-toc end -->

gitをちゃんと使えるように勉強する

# 1.gitコマンド
## 1.1.add/commit/push関連
| No. | コマンド                        | 説明                                                                     |
|----:|:--------------------------------|:-------------------------------------------------------------------------|
|   1 | git add `-u`                    | すでにステージングに登録されているファイルをステージングへ               |
|   2 | git commit `-a` -m <メッセージ> | add と commit を一括で実行                                               |
|   3 | git ls-files                    | ステージングエリアのファイルを表示                                       |
|   4 | git rm `--cached` <file名>      | ステージングエリアから削除. ワークツリーには残ったまま |
|   5 | `git push origin <ブランチ名>`  | ローカルリポジトリの変更をリモートリポジトリに反映                       |



## 1.2.ブランチ関連 (branch/checkout)
| No. | コマンド                                  | 説明                                                                         |
|----:|:------------------------------------------|:-----------------------------------------------------------------------------|
|   1 | git branch <ブランチ名>                   | ブランチを作成する                                                           |
|   2 | git checkout `-b` <ブランチ名>            | ブランチの作成とブランチ切り替えを一括で行う                                 |
|   3 | git branch `-m` <変更前> <変更後>         | ブランチ名を変更する                                                         |
|   4 | git branch <ブランチ名> <SHA-1ハッシュ値> | コミットを指定してブランチを作成する                                         |
|   5 | git branch `-d` <ブランチ名>              | 指定したローカルのブランチを削除する                                         |
|   6 | git branch `-D` <ブランチ名>              | 指定したブランチを削除する(マージされているcommitが残っている場合も削除可能) |
|   7 | git push origin -d <ブランチ名>           | 指定した`リモート`のブランチを削除する                                       |
|   8 | git fetch --prune                         | リモートリポジトリには存在しないリモート追跡ブランチをローカルから削除する   |
|   9 | git branch                                | ブランチ一覧を表示する                                                       |
|  10 | git branch -vv                            | 上流ブランチ一覧を表示する                                                   |




## 1.3.フェッチする (fetch)
- fetch: リモートリポジトリのコミット履歴をローカルリポジトリに反映する
  - `ローカルリポジトリのHEADの位置には変更を加えない`
	- -> 最新のコミットにHEADを移動させたい
		-> 早送りマージする git merge origin main
- pull = fetch + merge


| No. | コマンド                      | 説明                                                                       |
|----:|:------------------------------|:---------------------------------------------------------------------------|
|   1 | git fetch origin <ブランチ名> | 指定したブランチをフェッチする                                             |
|   2 | git fetch origin              | 全てのブランチをフェッチする                                               |
|   3 | git fetch origin              | ローカルリポジトリのHEADが指すブランチの上流ブランチをフェッチする         |



## 1.4.マージする (merge)
- mainブランチにはGithub等でプルリクエスト(プルリク, マージリクエスト)を出してマージすることが多いので, ローカルでは基本的にmainにマージしない.

| No. | コマンド                         | 説明                                              |
|----:|:---------------------------------|:--------------------------------------------------|
|   1 | git merge <ブランチ名>           | 現在いるブランチに指定したブランチをマージする    |
|   2 | git merge `--no-ff` <ブランチ名> | fast-forwardマージできる場合でも3-wayマージを行う |


## 1.5.変更を元の状態に戻す/打ち消す (reset/revert)
- 共同作業においてcommit履歴の削除は危険なので `git reset` ではなく `git revert` の方が使われる. さらにrevertだと"削除したこと"をcommit履歴として残すことができるメリットもある. 

| No. | コマンド                                          | 説明                                                                                                 |
|----:|:--------------------------------------------------|:-----------------------------------------------------------------------------------------------------|
|   1 | git revert <コミット番号>                         | 指定したcommitを打ち消すcommitを追加する                                                             |
|   2 | git reset `--hard` <コミット番号>                 | 指定したコミットにHEADと`ブランチ`を移動する(`ワークツリーとステージングエリアにHEADの内容をコピー`) |
|   3 | git reset `--soft` <コミット番号>                 | 指定したコミットにHEADと`ブランチ`を移動する(ワークツリーとステージングエリアの状態はそのまま)       |
|   4 | git reset <コミット番号>                          | 指定したコミットにHEADと`ブランチ`を移動する(`ステージングエリアにHEADの内容をコピー`)               |
|   5 | git reset --hard `reflogの出力であるHEAD@{1}など` | 指定した操作の直後の状態に戻す                                                                       |
|   6 | git checkout <コミット番号> `--` <ファイル名>     | HEADは動かさず, 指定したファイルの状態を指定したコミット時点のものにする                             |
|   7 | git checkout <コミット番号>                       | 指定したコミットが完了した状態まで戻す(HEADを動かす)                                                 |


## 1.6.ログを表示する (log)
| No. | コマンド            | 説明                                             |
|----:|:--------------------|:-------------------------------------------------|
|   1 | git log `--oneline` | sha1ハッシュ値とコミットメッセージだけを表示する |
|   2 | git log `--graph`   | コミット履歴を分かりやすく表示する               |
|   3 | git reflog          | Gitの操作履歴を表示する                          |


## 1.7.タグを付ける (tag)
| No. | コマンド                                            | 説明                                     |
|----:|:----------------------------------------------------|:-----------------------------------------|
|   1 | git tag -n                                          | タグの一覧を表示する                     |
|   2 | git tag <tag名>                                     | HEADが指すコミットにタグを付ける         |
|   3 | git tag <tag名> <コミット番号>                      | 指定したコミットにタグを付ける           |
|   4 | git tag `-a` <tag名> -m <メッセージ> <コミット番号> | 指定したコミットに`注釈付き`タグを付ける |


## 1.8.変更中の内容を退避させる (stash)
| No. | コマンド                | 説明                                                    |
|----:|:------------------------|:--------------------------------------------------------|
|   1 | git stash               | ワークツリーの変更をstashに退避する                     |
|   2 | git stash list          | stashの中身を表示する                                   |
|   3 | git stash pop           | stashの中身を取り出す(ワークツリーのみ)                 |
|   4 | git stash pop `--index` | stashの中身を取り出す(ワークツリーとステージングエリア) |
|   5 | git stash clear         | stashに入っている全ての変更を削除する                   |


## 1.9.rebase関連 (rebase/cherry pick)
rebase: ブランチの根本を移動するイメージ  
charry-pick: 指定したコミットの内容を取り入れる機能  
　
| No. | コマンド | 説明 |
|----:|:---------|:-----|
| 1    | git cherry-pick main     | ブランチ(main)の内容を現在のブランチに取り込む     |





## 1.10.その他
| No. | コマンド               | 説明                                                     |
|----:|:-----------------------|:---------------------------------------------------------|
|   1 | git blame <ファイル名> | 各行を最後に変更した日時, 人物, どのコミットかを表示する |




# 2.メモ
- detached HEAD: HEADがどのブランチも指していない状態. この状態でcommitをしてもブランチを伸ばせない. 
- `~`はHEADの何個前の親かを表す
  - `HEAD~1` : HEADの親
  - `HEAD~2` : HEADの親の親
- `^`は何番目の親かを表す
  - `HEAD^` : HEADの親
  - `HEAD^^` : HEAD^の親
  - `HEAD^^2` : HEAD^の2番目の親
- プルリクはブランチをブランチへマージするのを依頼する. プルリクを出して指摘を受け修正したとき, 修正後に新たにプルリクを出す必要はない. 
- origin: リモートリポジトリのデフォルト名


# 3.Githubを用いた開発の流れ
1. Githubでリポジトリを作成する.
1. リポジトリをローカルにクローンする. 
   ```
   git clone <Githubで取得したリポジトリのURL>
   ```
1. 作業用にブランチを切り, そのブランチへ移動する. 
   ```
   git branch <ブランチ名>
   git checkout <ブランチ名>
   ```
   もしくは
   ```
   git checkout -b <ブランチ名>
   ```
1. コードに手を加える. 
1. ステージングエリアにコミットしたいファイルを登録する.
   ```
   git add <変更を加えたファイル名>
   ```
1. git addで登録したステージングエリアのファイルを更新する. 
   ```
   git commit -m "<コミットメッセージ>"
   ```
1. ローカルリポジトリの内容をリモートリポジトリに反映させる. 
   ```
   git push origin <ブランチ名>
   ```
1. GithubでPull Requestを出す. 
1. GithubでMergeする. 
1. 不要になったブランチをリモートリポジトリとローカルリポジトリから削除する. 
   ```
   git push origin -d <ブランチ名>
<<<<<<< HEAD
=======
   git branch -d <ブランチ名>
>>>>>>> d7b58b1 (ブランチ削除の説明)
   git fetch --prune
   ```





