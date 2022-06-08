## 初期設定（ユーザ名＆メールアドレスを.gitconfig に記録）

```
$ git config --global user.name "ユーザ名"
$ git config --global user.email メールアドレス
```

## git の管理下にする

自分が git にアップしたいディレクトリに移動して下記のコマンドを入力する

```
$ git init
```

## ワークツリーとインデックスの状態を確認するコマンド

```
$ git status
```

例えば、最初何もしなかった時に入力すると

```
＄ git status

# Initial commit
#
# Untracked files:
#   (use "git add <file>..." to include in what will be committed)
#
#     sample.txt
nothing added to commit but untracked files present (use "git add" to track)
```

こういう風に出る
しかし、対象となる sample.txt をインデックスに追加すると

```
$ git add sample.txt
$ git status
# On branch master
#
# Initial commit
#
# Changes to be committed:
#   (use "git rm --cached <file>..." to unstage)
#
#     new file:   sample.txt
```

状態が変わる
さらに、commit コマンドを書くと

```
$ git commit -m "first commit"
[master (root-commit) 116a286] first commit
0 files changed, 0 insertions(+), 0 deletions(-)
create mode 100644 sample.txt

$ git status
# On branch master
nothing to commit (working directory clean)
```

push できる状態になる
