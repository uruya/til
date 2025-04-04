
# 基本的な操作
## git init - リポジトリを初期化
- gitでバージョン管理を始めるには、リポジトリの初期化を行う必要がある。
- .gitディレクトリに現在のディレクトリ以下を管理するリポジトリデータが格納される。
- 
```bash
$ git init
```

## git status - リポジトリの状態を確認
- ワークツリーやリポジトリに対する操作をすると、状態は次々に変化する。現在の状態を確認しながらGitを操作していくことが基本中の基本。
```bash
$ git status
```

## git add - ステージ領域へファイルを追加
- ステージ領域と呼ばれる場所にファイルを登録する。
```bash
$ git add README.md
```

## git commit - リポジトリの歴史を記録
- ステージ領域に登録されている時点のファイル群を実際にリポジトリの歴史として記録するコマンド
- この記録を元にファイルをワーキングツリーに復元したりすることが可能になる
```bash
$ git commit README.md
```
- 詳細なコミットメッセージを記述するには-m をつけずにgit commit コマンドを実行する。

## git log - コミットログを確認
- リポジトリにコミットされたログを確認できる。誰がいつコミットやマージをして、どのような差分が発生したかなど。
```bash
$ git log
```
- 一行目の要約だけのメッセージ一覧を表示するには、git logコマンドに --pretty=short をつける。
```bash
$ git log --pretty=short
```
- git logコマンドにディレクトリ名を与えるとそのディレクトリ以下のログを表示。
```bash
$ git log README.md
```
- ファイルの差分を表示。末尾に指定のファイル名を入力するとそのファイルだけのコミットログの差分を確認できる。
```bash
$ git log -p
```

## git diff - 変更差分を確認
- ワークツリー・ステージ領域・最新コミット間の差分を確認できる。
- 変更ファイルがあり、git addしていない状態で行うと現在のワークツリーとステージ領域の差分が確認できる
- git addした状態でワークツリーと最新コミットの差分を確認するためには以下のようにする
```bash
$ git diff HEAD
```

# ブランチの操作
## git branch - ブランチを一覧表示
- * が表示されているブランチが現在のブランチ。
```bash
$ git branch
```

## git checkout -b ブランチを作成し、切り替える
- 現在のブランチ状態から新しいブランチを作成する。
```bash
$ git checkout -b feature-A
```
 - 一つ前のブランチに切り替える。
```bash
$ git checkout -
```

## トピックブランチ
- 1つのトピック（テーマ）に集中して、他の作業は一切行わないブランチのこと。
- 例えば他のバグなどを発見してしまった場合は新たに別のブランチを新たに作成して、そのブランチで修正する。

## 統合ブランチ
 - トピックブランチの分岐元になり、併合する先となるブランチのこと。mainブランチのことで中途半端な変更を含まず、いつでも他人に見せられるブランチ。
 - 複数のバージョンリリースを扱う場合、統合ブランチは複数あることになる。

## git merge - ブランチをマージ
- ブランチからマージしたことを明確にれきにし残すためにマージコミットを作成のため、--no-ffオプションを付与してマージする。
```bash
$ git merge --no-ff feature-A
```

## git log --graph - ブランチを資格的に確認する
- トピックブランチでコミットされた内容がマージされたことがよくわかり、トピックブランチとして分岐され統合されている様子がわかりやすく表現されている。

# コミットを変更する操作
## git reset - 歴史を戻る
- 戻りたい場所の8種を与えることによって、その時の状態を完全に復元する
```bash
$ git reset --hard ed5d75bfe18a3cbc8eb083bc9e3f217bb83b2e0a
```

## git reflog - 現在のリポジトリで行われた作業のログを確認できる。歴史を戻す前のハッシュを見つけて、git reset --hardコマンドで歴史を戻す前の状態を復元できる。

## git commit --amend - コミットメッセージを修正
 - vim形式でファイル操作

## git commit -am - addコマンドとcommitコマンドを同時
```bash
$ git commit -am "Add feature-C"
```

## git rebase -i - 歴史を押し潰して改変
- まずはスペルミスなどあった際にコミットまで行う。
- 以下のコマンドで現在のブランチのHEADを含めた2つまでの歴史を対象としてエディタが立ち上がる。
```bash
$ git rebase -i HEAD~2
```
- 対象のコミット部分の左隣にあるpickと書いてある部分を削除して、fixupと書き換えて、エディタを保存する。

# リモートリポジトリへの送信
## git remote add - リモートリポジトリを登録
```bash
$ git remote add origin https://github.com/uruya/git-tutorial.git
```

## git push - リモートリポジトリへ送信
```bash
$ git push -u origin main
```
- mainブランチ以外のブランチへ送信する
```bash
$ git checkout -u origin fature-D
```

# リモートリポジトリから取得
## git clone - リモートリポジトリを取得
- リモートリポジトリを取得する。
```bash
$ git clone git@github.com:uruya/git-tutorial.git
```
- リモートのfeature-Dブランチをチェックアウトする。
```bash
$ git checkout -b feature-D origin/feature-D
```
- 今までと同様にコミットし、feature-Dブランチをpushする。
```bash
$ git push
```

## git pull - 最新のリモートリポジトリブランチを取得

# GitHubの機能
## ブランチ間の差分を見る
ex. https://github.com/rails/rails/compare/4-0-stable...3-2-stable

## 数日前の差分を見る
ex. masterブランチで7日間の間の差分を見たい場合
https://github.com/rails/rails/compare/master@%7B7.day.ago%7D...master

## 指定日からの差分を見る
ex. masterブランチで2013年1月1日からの差分が見たい場合
https://github.com/rails/rails/compare/master@%7B2013-01-01%7D...master
