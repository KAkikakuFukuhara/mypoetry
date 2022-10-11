# issue

- [1. 概要](#1-概要)
- [2. 問題と調査](#2-問題と調査)
- [3. 解決](#3-解決)
- [4. その後の進捗](#4-その後の進捗)
  - [4.1. バージョンアップ(v1.2.2) date:221011](#41-バージョンアップv122-date221011)
  - [4.2. chumpyがインストールできない理由に関して date:221011](#42-chumpyがインストールできない理由に関して-date221011)

## 1. 概要

ライブラリmmposeをインストールしようとすると`No module named pip`というエラーが発生してインストールできなかった。  

poetryのバージョン1.2.1（221010にlatest)ではインストールできなかったのでダウングレードを実施。  
アンインストール後に、`get-poetry.py`を使用してpoetry(v1.1.15)をインストールして解決した。  

## 2. 問題と調査

PCの移動に伴い新しくpoetryをインストールして環境構築を行った。  
ライブラリmmposeをインストールしようとすると`No module named pip`というエラーが発生してインストールできなかった。  

詳しく見てみると依存ライブラリである`chumpy`のインストール時に発生している。  
内部的にサブプロセスを呼び出して`chumpy`のインストールを試みているようだが、この処理の部分で`No module named pip`というエラーが発生しているようだ。  
具体的なエラーメッセージ
```bash
fukuhara:/tmp/chumpy-env$ poetry add chumpy
Using version ^0.70 for chumpy

Updating dependencies
Resolving dependencies... (0.3s)

Writing lock file

Package operations: 4 installs, 0 updates, 0 removals

  • Installing numpy (1.21.6)
  • Installing scipy (1.7.3)
  • Installing six (1.16.0)
  • Installing chumpy (0.70): Failed

  CalledProcessError

  Command '['/tmp/chumpy-env/.venv/bin/python', '-m', 'pip', 'install', '--use-pep517', '--disable-pip-version-check', '--prefix', '/tmp/chumpy-env/.venv', '--no-deps', '/home/fukuhara/.cache/pypoetry/artifacts/b2/10/b0/13e9c1cd35c416f871478974adbb83617614a493729e901cccaf75992b/chumpy-0.70.tar.gz']' returned non-zero exit status 1.

  at ~/.pyenv/versions/3.7.10/lib/python3.7/subprocess.py:512 in run
       508│             raise
       509│         retcode = process.poll()
       510│         if check and retcode:
       511│             raise CalledProcessError(retcode, process.args,
    →  512│                                      output=stdout, stderr=stderr)
       513│     return CompletedProcess(process.args, retcode, stdout, stderr)
       514│ 
       515│ 
       516│ def list2cmdline(seq):

The following error occurred when trying to handle this error:


  EnvCommandError

  Command ['/tmp/chumpy-env/.venv/bin/python', '-m', 'pip', 'install', '--use-pep517', '--disable-pip-version-check', '--prefix', '/tmp/chumpy-env/.venv', '--no-deps', '/home/fukuhara/.cache/pypoetry/artifacts/b2/10/b0/13e9c1cd35c416f871478974adbb83617614a493729e901cccaf75992b/chumpy-0.70.tar.gz'] errored with the following return code 1, and output: 
  Processing /home/fukuhara/.cache/pypoetry/artifacts/b2/10/b0/13e9c1cd35c416f871478974adbb83617614a493729e901cccaf75992b/chumpy-0.70.tar.gz
    Installing build dependencies: started
    Installing build dependencies: finished with status 'done'
    Getting requirements to build wheel: started
    Getting requirements to build wheel: finished with status 'error'
    error: subprocess-exited-with-error
    
    × Getting requirements to build wheel did not run successfully.
    │ exit code: 1
    ╰─> [23 lines of output]
        Traceback (most recent call last):
          File "<string>", line 9, in <module>
        ModuleNotFoundError: No module named 'pip'
        
        During handling of the above exception, another exception occurred:
        
        Traceback (most recent call last):
          File "/tmp/chumpy-env/.venv/lib/python3.8/site-packages/pip/_vendor/pep517/in_process/_in_process.py", line 363, in <module>
            main()
          File "/tmp/chumpy-env/.venv/lib/python3.8/site-packages/pip/_vendor/pep517/in_process/_in_process.py", line 345, in main
            json_out['return_val'] = hook(**hook_input['kwargs'])
          File "/tmp/chumpy-env/.venv/lib/python3.8/site-packages/pip/_vendor/pep517/in_process/_in_process.py", line 130, in get_requires_for_build_wheel
            return hook(config_settings)
          File "/tmp/pip-build-env-zodwv6h0/overlay/lib/python3.8/site-packages/setuptools/build_meta.py", line 338, in get_requires_for_build_wheel
            return self._get_build_requires(config_settings, requirements=['wheel'])
          File "/tmp/pip-build-env-zodwv6h0/overlay/lib/python3.8/site-packages/setuptools/build_meta.py", line 320, in _get_build_requires
            self.run_setup()
          File "/tmp/pip-build-env-zodwv6h0/overlay/lib/python3.8/site-packages/setuptools/build_meta.py", line 482, in run_setup
            super(_BuildMetaLegacyBackend,
          File "/tmp/pip-build-env-zodwv6h0/overlay/lib/python3.8/site-packages/setuptools/build_meta.py", line 335, in run_setup
            exec(code, locals())
          File "<string>", line 11, in <module>
        ModuleNotFoundError: No module named 'pip'
        [end of output]
    
    note: This error originates from a subprocess, and is likely not a problem with pip.
  error: subprocess-exited-with-error
  
  × Getting requirements to build wheel did not run successfully.
  │ exit code: 1
  ╰─> See above for output.
  
  note: This error originates from a subprocess, and is likely not a problem with pip.
  

  at ~/.local/share/pypoetry/venv/lib/python3.7/site-packages/poetry/utils/env.py:1476 in _run
      1472│                 output = subprocess.check_output(
      1473│                     command, stderr=subprocess.STDOUT, env=env, **kwargs
      1474│                 )
      1475│         except CalledProcessError as e:
    → 1476│             raise EnvCommandError(e, input=input_)
      1477│ 
      1478│         return decode(output)
      1479│ 
      1480│     def execute(self, bin: str, *args: str, **kwargs: Any) -> int:

The following error occurred when trying to handle this error:


  PoetryException

  Failed to install /home/fukuhara/.cache/pypoetry/artifacts/b2/10/b0/13e9c1cd35c416f871478974adbb83617614a493729e901cccaf75992b/chumpy-0.70.tar.gz

  at ~/.local/share/pypoetry/venv/lib/python3.7/site-packages/poetry/utils/pip.py:51 in pip_install
       47│ 
       48│     try:
       49│         return environment.run_pip(*args)
       50│     except EnvCommandError as e:
    →  51│         raise PoetryException(f"Failed to install {path.as_posix()}") from e
       52│ 

```

前回使用していたPCでは、このような問題は発生していなかったのでダウングレードを試みた。  
ダウングレードは公式のドキュメント　https://python-poetry.org/docs/　を参考に行う。  
ダウングレードは1)アンインストール2)再度インストールの流れで実行する。  
以下コマンドでダウングレード  
```bash
curl -sSL https://install.python-poetry.org | python3 - --uninstall
```
以下コマンドでインストール。今回はひとつ下のマイナーバージョンの最新のモノ(v1.1.15)をインストー
ル。  
```bash
curl -sSL https://install.python-poetry.org | python3 - --version 1.1.15
```
ダウングレードに成功したので、`chumpy`をインストールを試してみる。  
```bash
poetry add chumpy
```
問題なくインストールできた。  

しかし、別の問題が発生した。  
筆者は`pyenv+poetry`を用いて仮想環境を作成している。  
上記の方法ではpyenvでの仮想環境の切り替えができず、poetryをインストールした際のpythonのバージョンに固定させるようである。  
この問題はpoetryバージョン1.2.xでは解決している。  
以下のURLを参考に以下のコマンドを実行することで解決することができるが、poetryバージョン1.x.xでは解決していない。  
https://github.com/python-poetry/poetry/issues/4101
```bash
poetry config virtualenvs.prefer-active-python true
```

これにより以下のような相反する問題が発生していることが分かる。  
- poetry(v1.2.1 )では`chumpy`のインストールができないが、pyenvでの仮想環境の切り替えは可能。
- poetry(v1.1.15)では`chumpy`のインストールは可能であるが、pyenvでの仮想環境の切り替えは不可能。

## 3. 解決

一時的な解決ではあるが、古い方のインストール方法を用いるとこの問題は解決した。  
一時的な解決である理由は、古い方のインストール方法は現在推奨されておらず、2023に廃止される予定であるからだ。  

作業の流れは1)ダウングレード2)古い方法でインストールである。  
まずはダウングレード。  
```bash
curl -sSL https://install.python-poetry.org | python3 - --uninstall
```
次に古い方法でのインストールを行う。  
インストール方法は公式のv1.1のドキュメント https://python-poetry.org/docs/1.1/ に従う。  
古い方法ではv1.2.xに対応していないようなのでv1.1.15をインストールする。  
```bash
curl -sSL https://raw.githubusercontent.com/python-poetry/poetry/master/get-poetry.py | python - --version 1.1.15
```

poetryがインストールできたら`chumpy`のインストールを試す。
```bash
poetry add chumpy
```
無事にインストールできた。
またpyenvの仮想環境の切り替えも問題なく行える。

## 4. その後の進捗

### 4.1. バージョンアップ(v1.2.2) date:221011
22/10/11にpoetry(v1.2.2)がリリースされたので試してみたが、インストールできなかった。  

### 4.2. chumpyがインストールできない理由に関して date:221011
以下のissueを見ると(v1.2.1)ではインストール時に内部的に`--use-pep517`オプションを用いていることが原因のようだ。(v1.1.15)では使用していない？らしい。  
https://github.com/python-poetry/poetry/issues/6712
実際に`--use-pep517`を用いて`chumpy`のインストールを試してみると同じエラーが発生した。  
```bash
pip install --use-pep517 chumpy
```
対策っぽいモノがこれ https://github.com/python-poetry/poetry/issues/3433 .  
現時点で問題が挙がったばかりなので、とりあえず古いインストール方法を使用することにする。  
対策っぽいモノは使用しない  
