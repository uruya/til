
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

# Git Flow
1. git-flowのインストール
```bash
$ brew install git-flow
```

2. GitHubでリポジトリ作成しclone後、git flowの初期設定を行う
```bash
$ git flow init -d
```
- masterブランチ
常に正常にソフトウェアが動作する状態のブランチ。直接変更してコミットすることはない。
リリース可能な状態まで開発された別のブランチがマージされる先になるが、それは製品をリリースする時のみ。

- developブランチ
開発中であるコードの中心となるブランチ。開発者が直接コードを変更してコミットすることはない。
developブランチを起点として、新たにfeatureブランチを作成し、そのブランチで機能の開発や修正などのコードを書く作業をする。
developブランチはプログラマがコードを書くためにfeatureブランチを作成する最新の開発コードが維持されるブランチ。

- featureブランチで行うこと
1. (developブランチを最新にして)developブランチからfeatureブランチを作成
```bash
$ git flow feature start add-user
```
2. featureブランチで機能を実装する
3. GitHubを通して、developブランチへPull Requestを送る(Pull Request作成時にGitHub上でマージ先をdevelopに設定)
4. ほかの開発者のレビューを受け、developブランチにPull Requestがマージされる

- releaseブランチで行うこと
1. developブランチに移動し、コードを最新化
```bash
$ git checkout develop
$ git pull
```
2. releaseブランチを開始
```bash
$ git flow release start '1.0.0'
```
3. ブランチでの作業を行う
このブランチではリリースするための準備に関するコミットのみにする。例えばメタ情報として付加されるバージョン番号の変更など。
ステージング環境などにデプロイして、テスト工程などを実施しその上で見つかったバグ修正などもこのタイミングで行う。
4. リリースとマージを行う
releaseブランチがmasterブランチにマージされる。マージする際のコミットメッセージが求められるが特になければ初期のまま保存して終了。
```bash
$ git flow release finish '1.0.0'
```
次にマージされたmasterブランチにバージョンと同じ番号のタグが発行される。
このバージョンに対するコミットメッセージを書く。
その後、リリースブランチの状態をdevelopブランチにマージする。
5. バージョンのタグ確認
```bash
$ git tag
```
6. リモートリポジトリに反映
develop・masterブランチ、タグ情報をpushする。
```bash
$ git push origin develop
$ git checkout master
$ git push origin master
$ git push --tags
```
7. masterブランチをリリースして終了
- hotfixブランチで行うこと
現在リリースされているバージョンに対して、バグや脆弱性が発見され、次期のリリースまで待つことができずすぐに解決しなければならないときに緊急対処としてりようするもの。
よって、リリースされたバージョンからのタグやmasterブランチからブランチを作成することになる。
これにより、developブランチで別の開発者が製品の修正を行うことができる。
1. ブランチを作成する
```bash
$ git fetch origin
$ git flow hotfix start '1.0.1' '1.0.0'
```
2. 脆弱性の修正対応などをしてコミットし、hotfixブランチをGitHubのリモートリポジトリにpushして、masterブランチへpull request送信する。
```bash
$ git push origin hotfix/1.0.1
```
3. タグの作成とリリースを行う
Pull Requestがmasterブランチに取り込まれたとする。
GitHub上でReleaseから1.0.1のタグを作成する。
作成したら手元のリポジトリでもタグを取得すれば、1.0.1が作成されていることを確認できる。
```bash
$ git fetch origin
$ git tag
```
5. hotfixブランチからdevelopブランチへマージする
