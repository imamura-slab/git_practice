git練習
===
<!-- toc --><!-- markdown-toc start - Don't edit this section. Run M-x markdown-toc-refresh-toc -->
**Table of Contents**

- [git練習](#git)
- [1.知らなかったコマンド](#1)
  - [1.1.使いそうなコマンド](#11)
  - [1.2.あんまり使わなさそうなコマンド](#12)

<!-- markdown-toc end -->

# 1.知らなかったコマンド
## 1.1.使いそうなコマンド
| No. | コマンド                        | 説明                                                   |
|----:|:--------------------------------|:-------------------------------------------------------|
|   1 | git ls-files                    | ステージングエリアのファイルを表示                     |
|   2 | git rm `--cached` <file名>        | ステージングエリアから削除. ワークツリーには残ったまま |
|   3 | git merge <ブランチ名>          | 現在いるブランチに指定したブランチをマージする         |
|   4 | git branch `-d` <ブランチ名>    | 指定したブランチを削除する                             |
|   5 | git push origin -d <ブランチ名> | 指定したリモートのブランチを削除する                   |



## 1.2.あんまり使わなさそうなコマンド

| No. | コマンド                          | 説明                                                                         |
|----:|:----------------------------------|:-----------------------------------------------------------------------------|
|   1 | git commit `-a` -m "xxx"          | add と commit を一括で実行                                                   |
|   2 | git add `-u`                      | すでにステージングに登録されているファイルをステージングへ                   |
|   3 | git branch `-m` <変更前> <変更後> | ブランチ名を変更する                                                         |
|   4 | git branch `-D` <ブランチ名>      | 指定したブランチを削除する(マージされているcommitが残っている場合も削除可能) |
|   5 | git checkout `-b` <ブランチ名>    | ブランチの作成とcheckoutを一括で行う                                         |

