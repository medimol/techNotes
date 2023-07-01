# Python の開発環境まとめ

## 結論から
Anaconda を使わない場合(推奨)
- バージョン管理: pyenv
  - `python3`を使用して導入し，以降は`python`を使用する
- パッケージ管理: poetry
- 仮想環境管理: poetry

Anaconda を使う場合
- バージョン管理: conda
- パッケージ管理: 基本conda, なければ pip3
- 仮想環境管理: conda

## pyenv と poetry の使い方まとめ
pyenv
- インストール可能なバージョン一覧: `pyenv install --list`
- インストール: `pyenv install {version}`
- 一覧: `pyenv versions`
- バージョン切り替え(global): `pyenv global {version}`
- バージョン切り替え(local): `pyenv local {version}`
  - poetry使っているなら，そちらでプロジェクトごとのバージョンを指定する
  - ディレクトリ単位(`.python-version`が作成される)
- pythonの場所確認: `pyenv which {python}`

poetry
- 自身のアップデート: `poetry self update`
- プロジェクト作成: `poetry new {project-name}`
  - 既存の物がある場合: `poetry init`
- パッケージのインストール: `poetry add {package-name}`
- パッケージのアンインストール: `poetry remove {package-name}`
- パッケージのアップデート: `poetry update`
  - 確認したい場合は `--dry-run`
- パッケージ一覧: `poetry show`
- 環境内でPythonバージョンを指定: `poetry env use {version}`
- セットアップ: `poetry install`
  - 開発で使うパッケージがいらないなら `--no-dev`
- 仮想環境での実行: `poetry run python {file}`
  - `poetry shell` や `python {file}` を用いると，仮想環境のシェルが立ち上がる
    - 前者は実行時のみ仮想環境に，後者はずっと
- `virtualenvs.prefer-active-python` を `true` でpyenvのPythonバージョンに合わせてくれる

## ツールまとめ
ディストリビューション
- Anaconda: データサイエンス向け

パッケージマネージャー
- conda: Anaconda専用
- pip / pip3

バージョン管理ツール
- pyenv

## ツール詳細
pip / pip3
- Python標準のパッケージ管理ツール
- [PyPI](https://pypi.org/)(The Python Package Index)に登録されたパッケージが利用可
- パッケージはwheel形式
- 「そのpipが動いているPython環境に」パッケージをインストールする
- 推奨: `python3 -m pip install {package}`
- `pip install {package}`なら，**pip3 を使ったほうがいい**
  - pip3 であれば，確実にPython3の環境にインストールされる
- `sudo` は控える(linuxシステムで使用されているパッケージがある)

Anaconda, conda
- データサイエンス向けのパッケージを同梱したPythonディストリビューション
- Anaconda Cloud のリポジトリ
- pipと競合する可能性あり (なおpipも同梱されている)

pyenv
- Pythonのバージョン管理ツール
- 1 environment / version

pyenv-virtualenv
- pyenvのプラグイン
- pyenvのPythonへのvirtualenvでの仮想環境をpyenvで切り替え
  - バージョン切り替えと同じようにpyenvで仮想環境を切り替えれる

virtualenv
- 仮想環境を構築するツール
- サードパーティー製
- 仮想環境ごとに独立してパッケージをインストールできる

`python3 -m venv` (旧pyvenv)
- 仮想環境を構築するツール
- Python標準 (virtualenvの機能を標準に移植)
- 仮想環境ごとに独立してパッケージをインストールできる
- virtualenv より計量

pipenv
- プロジェクト管理ツール
- virtualenv と pip を包含
- `pipfile` と `Pipfile.lock` による環境の再現
  - Node.js や yarn のよう
- Pyenv との組み合わせで，任意のPythonバージョンを指定できる

poetry
- プロジェクト管理ツール
- pipenvと機能はほぼ同じ
- 追加機能
  - パッケージのビルド・公開(setup.py, requirements.txt, Pipfileなしで)

### Anacondaに対する議論
デメリット
- 対応パッケージが少なく，その場合はpipを使うことになるが，競合する可能性がある
  - conda-forge(有志によるリポジトリ)があるが，pipを使っていればこの問題は発生しない
- 重い
  - Miniconda があるけど

メリット
- all in one である(基本的なパッケージ，バージョン管理，仮想環境)
- Numba (高速な実行コンパイラ) や matplotlib は Anaconda の仕様を推奨
  - 依存ライブラリが複雑らしい
- 環境に応じた最も良いものをインストールする
  - Anaconda でインストールした numpy は早い

### 使い分け
Python環境
- WindowsネイティブでデータサイエンスするならAnaconda
- Windowsネイティブでデータサイエンスしないなら公式
- Linux，macOSはリポジトリのPython3

ツール
- 基本的に pyenv と pipenv

## pyenv, venv, poetry の調査

`python`コマンドと`python3`コマンドの使い分け
- `python`コマンド: Python2系or3系が使われる
- `python3`コマンド: Python3系が使われる
- 現在は3系が主流なので，`python3`コマンドを使うのが無難
- 古いライブラリの中には2系を使用しているものもあり，このような仕様になっている

であれば，pyenvを使用したときに`python`コマンドのPythonバージョンが変化し，`python3`コマンドのPythonバージョンが変化しないのは問題なのでは?
- ドキュメントには特に明記されてない
- pyenvでは2系のバージョンも持ってこれるし，`python`コマンドのバージョンが変わるのはそう
- Python2系を使用しているプログラムが困ったことになるけどそれはさっさと3系にしろよという話ではある
- であれば`python3`コマンドの扱いをどのようにすればいいか悩むな．ほっとけばいいか

venv
- プロジェクトごとに環境を作る
- activate: `source env_name/bin/activate`

venvであれば，activateをすれば，プロジェクトに関係のないファイルもその環境を使って実行できる．
poetryでも同じことができないか?
- conda も activate すれば常にその環境が使用される
- 本来仮想環境はそれを使用するプロジェクト内で使用されるべきであり，それ以外は普通にローカル環境で実行すればよい

poetry
- 既存の仮想環境を読み取ってくれる
- 仮想環境がなければ作ってくれる
- `virtualenvs.prefer-active-python`を`true`にすることで，pyenvのPythonバージョンを使用してくれる
  - 普通は，poetryがインストールされたPythonバージョンを使用する
- `poetry env use /full/path/to/python` で，明示的に使用するバージョンを指定できる
  - この指定を解除したい場合: `poetry env use system`

### まとめ
- `python3`はpyenv導入後は使用せず，`python`コマンドを使用する
- プロジェクトにはpoetryの仮想環境を使用し，それ以外はローカル環境を使用する
