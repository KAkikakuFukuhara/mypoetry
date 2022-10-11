# (Python) パッケージ管理ツール poetry に関して

poetry は pythonプロジェクトで使用されているライブラリを管理するために使用されるツールである。  

- [1. はじめに](#1-はじめに)
- [2. poetryのメリット](#2-poetryのメリット)
- [3. poetryを始めよう](#3-poetryを始めよう)
  - [3.1. インストール方法](#31-インストール方法)
  - [3.2. 使い方](#32-使い方)
  - [3.3. 問題等](#33-問題等)

## 1. はじめに

python はパッケージが豊富に存在し、またライブラリのインストールが容易であるプログラム言語である。  
pythonプロジェクトでは`pip`を使用してライブラリをインストールし開発環境を用意してから、開発を行う。  
プロジェクト開発者以外の人間や使用しているPC以外のPCでこのプロジェクトを使用したい場合は、前述した開発環境を再現する必要がある。  
開発環境の再現方法として pythonでは、`pip` を使用した方法が知られている。  
この方法は、 pipでインストールしたライブラリを書き込んだ `requirements.txt` というファイルを読み込んで行う方法である。  
`requirements.txt`の作成方法は以下のコマンドを用いることで作成することができる。
```bash
pip freeze > requirements.txt
```
また、`requirements.txt`の読み込みは`pip install -r requirements.txt`コマンドを用いることで行うことができる。
```bash
pip install -r requirements.txt
```

このように pytnon ではライブラリのインストールや開発環境の再現等を行うことができる。  
しかし一方で下記のようないくつかの問題がある。  
- pythonのバージョンを設定できない。
  - このプロジェクトはpython3.6?, 3.7?
- ライブラリの依存ライブラリを全てrequirements.txtに書き込んでしまう
  - あるライブラリをインストールした時どの依存ライブラリがインストールされたかのを記録しない。
- ライブラリ毎の依存関係を考慮しない（後からpipでインストールしたライブラリを優先する)
- etc...

それらの不満点を解決+αしてくれるツールの一つとして **poetry** がある（他にもいくつかある）。


## 2. poetryのメリット

poetry は、pythonのライブラリ管理ツールである。  

poetry は、pythonの仮想環境を利用してpythonの開発環境を分離する。  
これによって開発環境同士が衝突しない（anacondaの開発環境のように)  

poetry は開発環境の情報を`pyproject.toml`ファイルに保存する。  
このファイルはpythonの公式が配布しているPEPに定義されているpythonの開発環境情報を記述したファイルである。  
この ファイルには、使用してもよい pyhonのバージョンの設定や使用しているライブラリの依存関係を考慮に入れた記述をすることができる。  
例えば以下のようなモノが`pyproject.toml`ファイルの例である。  
使用しているライブラリの情報が`[tools.poetry.dependecies]`に記載してあることが分かる。  
これによると、このプロジェクトでは `python3.7` を用いて、`tensoflow==2.8`のライブラリをインストールしていることが分かる。  

```toml
[tool.poetry]
name = "tf28"
version = "0.1.0"
description = ""
authors = ["Tarou Tanaka <tarou.tanaka@gmail.com>"]

[tool.poetry.dependencies]
python = ">=3.7, <3.8"
tensorflow = "2.8"

[tool.poetry.dev-dependencies]

[build-system]
requires = ["poetry-core>=1.0.0"]
build-backend = "poetry.core.masonry.api"
```

また以下のコマンドを用いることでライブラリの依存関係についても把握することができる。  
```bash
$ poetry show --tree

tensorflow 2.8.0 TensorFlow is an open source machine learning framework for everyone.
├── absl-py >=0.4.0
├── astunparse >=1.6.0
│   └── six >=1.6.1,<2.0 
├── flatbuffers >=1.12
├── gast >=0.2.1
├── google-pasta >=0.1.1
│   └── six * 
├── grpcio >=1.24.3,<2.0
│   └── six >=1.5.2 
├── h5py >=2.9.0
│   └── numpy >=1.14.5 
├── keras >=2.8.0rc0,<2.9
├── keras-preprocessing >=1.1.1
│   ├── numpy >=1.9.1 
│   └── six >=1.9.0 
├── libclang >=9.0.1
├── numpy >=1.20
├── opt-einsum >=2.3.2
│   └── numpy >=1.7 
├── protobuf >=3.9.2
├── six >=1.12.0
├── tensorboard >=2.8,<2.9
│   ├── absl-py >=0.4 
│   ├── google-auth >=1.6.3,<3 
│   │   ├── cachetools >=2.0.0,<6.0 
│   │   ├── pyasn1-modules >=0.2.1 
│   │   │   └── pyasn1 >=0.4.6,<0.5.0 
│   │   ├── rsa >=3.1.4,<5 
│   │   │   └── pyasn1 >=0.1.3 (circular dependency aborted here)
│   │   └── six >=1.9.0 
│   ├── google-auth-oauthlib >=0.4.1,<0.5 
│   │   ├── google-auth >=1.0.0 (circular dependency aborted here)
│   │   └── requests-oauthlib >=0.7.0 
│   │       ├── oauthlib >=3.0.0 
│   │       └── requests >=2.0.0 
│   │           ├── certifi >=2017.4.17 
│   │           ├── charset-normalizer >=2,<3 
│   │           ├── idna >=2.5,<4 
│   │           └── urllib3 >=1.21.1,<1.27 
│   ├── grpcio >=1.24.3 
│   │   └── six >=1.5.2 (circular dependency aborted here)
│   ├── markdown >=2.6.8 
│   │   └── importlib-metadata >=4.4 
│   │       ├── typing-extensions >=3.6.4 
│   │       └── zipp >=0.5 
│   ├── numpy >=1.12.0 
│   ├── protobuf >=3.6.0 
│   ├── requests >=2.21.0,<3 (circular dependency aborted here)
│   ├── tensorboard-data-server >=0.6.0,<0.7.0 
│   ├── tensorboard-plugin-wit >=1.6.0 
│   └── werkzeug >=0.11.15 
│       └── markupsafe >=2.1.1 
├── tensorflow-io-gcs-filesystem >=0.23.1
├── termcolor >=1.1.0
├── tf-estimator-nightly 2.8.0.dev2021122109
├── typing-extensions >=3.6.6
└── wrapt >=1.11.0
```

## 3. poetryを始めよう

### 3.1. インストール方法

docs/install.md に記載予定

### 3.2. 使い方

docs/tutorial.mdに記載予定

### 3.3. 問題等

docsに記載予定

