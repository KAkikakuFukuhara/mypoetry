# tutorial

poetry の使い方のチュートリアルを記載する。  

- [1. チュートリアルに関して](#1-チュートリアルに関して)
- [2. 記載していないこと](#2-記載していないこと)
  - [2.1. `poetry shell` で仮想環境に移動できない時](#21-poetry-shell-で仮想環境に移動できない時)
  - [2.2. pipのバージョンが古いと怒られる時](#22-pipのバージョンが古いと怒られる時)
  - [2.3. pipでプロジェクトをインストールできる。](#23-pipでプロジェクトをインストールできる)
  - [2.4. poetry.lock ファイルは　.gitignore を用いて追跡対象から外す](#24-poetrylock-ファイルはgitignore-を用いて追跡対象から外す)

## 1. チュートリアルに関して

書くのが面倒くさいのでどのように使うかは以下のサイトを確認してほしい。  
バージョンが少し古い記事となるが、基本的な使い方は変わらない。  

- https://qiita.com/yokohama4580/items/dc6ba7259e99cad0dd65
- https://qiita.com/ksato9700/items/b893cf1db83605898d8a
- https://qiita.com/shun198/items/97483a227f288ad58112

分からないことが合ったら公式ドキュメントで調べる。  
- https://python-poetry.org/docs/

## 2. 記載していないこと

### 2.1. `poetry shell` で仮想環境に移動できない時

偶に `poetry shell` コマンドで仮想環境に移動しようとした時に、’既にアクティベートされています'のような意味合いの英語のメッセージが発生して仮想環境に移動できない。  
その時は 以下のようにコマンドを行うと仮想環境に移動できる。  
```bash
$ source .venv/bin/activate
(hoge)$
```

### 2.2. pipのバージョンが古いと怒られる時

pip のバージョンが古い等怒られる場合は以下のようにして pip自体をバージョンアップできる。  
```bash
$ poetry shell
$ pip install -U pip setuptools
$ poetry add hogefuga
```

### 2.3. pipでプロジェクトをインストールできる。

`poetry init`などでプロジェクトに作成される`pyproject.toml`ファイルは pip を用いることでインストールすることができ、また poetry がインストールされていない環境でもインストールが可能である。  
以下の用にするとインストールできる。
```bash
$ ls
pyproject.toml

$ python -m venv .venv
$ source .venv/bin/activate

$ pip install -U pip setuptools
$ pip install -e .
```

### 2.4. poetry.lock ファイルは　.gitignore を用いて追跡対象から外す

poetry.lock ファイルは依存ライブラリ等を含めたライブラリのバージョン情報などを管理しているファイルである。  
そのため頻繁に内容が更新されるので git で追跡対象にするのはオススメしない。  
以下のようにして.gitignoreに追加して追跡対象から外す。  
```bash
echo "poetry.lock" >> .gitignore
```
