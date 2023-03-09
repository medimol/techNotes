# LaTeX
## Basics
- TeX: Donald Ervin Knuth によって開発された組版システム (文字や図版などの要素を紙面に配置するシステム)
- LaTeX: Lesilie Lamport によってTeXの上に構築された文書処理システム
- pTeX, pLaTeX: TeX、LaTeX をアスキー(現KADOKAWA)が日本語化したもの． p は "publishing" から．
- upTeX, upLaTeX: pTeX、LaTeX の内部を Unicode 化したもの．
- LuaTeX, XeTeX: Unicode に完全対応したもの．これを用いた LuaLaTeX、XeLaTeX でも日本語出力が可能．

現在は LaTeX 2e (日本語版は pLaTeX 2e) が最新版．LaTeX 2.09 とは別物．

DVIファイル
- DeVice Independent
- 印刷に必要なフォントや座標位置などの情報が記録されている、デバイスに独立なイメージファイル
    - 画像ファイル情報は含まれていない

### 処理の流れ
1. texファイルの作成
2. latexシステム (uplatex, platexなど) によってコンパイルし、DVIファイルを作成する
    - エラーがある場合、`?` のプロンプトによってユーザーの指示を待つ
3. dvipdfmx でDVIファイルからPDFファイルを生成する

### upTeX, upLaTeX
`-guess-input-enc` による文字コードの自動判別機能で間違った文字コードに判別される場合がある
    - オプション `-kanji=utf8 -no-guess-input-enc` を指定する

### BibTeX
- upTeXに対応しているのがupBibTeX

処理の流れ
1. `$ platex bunken`
    - tex から .dvi, .aux, .log ができる
2. `$ pbibtex bunken`
    - .aux から .bbl, .blg ができる
3. `$ platex bunken`
4. `$ platex bunken`

### biber
BibLaTexユーザーへのBibTeXのコマンドラインツールとしての代替
- 完全なunicode対応

#### BibLaTeX
BibTeXの後継
- バックエンドとして、biberやBibTeXを使用する
- 言語ごと書体の切り替えや、ソート順の指定などができる

使い方(bibtexがバックエンド)
- preamble
```
\usepackage[backend=bibtex, style=numeric]{biblatex}
\addbibresource{myreference.bib}
\addbibresource{biblio.bib}
```
- 表示したい場所
```
\printbibliography[title=参考文献]
```
- 注意点あり(詳しくは[ここ](https://konn-san.com/prog/why-not-latexmk.html))

biberをバックエンドに
```
\usepackage[backend=biber, style=numeric]{biblatex}
```
- `$biber = 'biber --bblencoding=utf8 -u -U --output_safechars';`
    - 意味: 「.bblファイルのエンコードはUTF-8、入出力共にUTF-8を使って、アクセント記号とかヤバめの記号はTeXにエンコードする」
    - やばめとは: 欧米の特殊文字が間違った書体になる場合がある

## Install & Update (Ubuntu, WSL2)
ref: [TEX Wiki](https://texwiki.texjp.org/?Linux#texliveinstall)

1. `$ wget http://mirror.ctan.org/systems/texlive/tlnet/install-tl-unx.tar.gz`
2. `$ tar xvf install-tl-unx.tar.gz`
3. `$ cd intall-tl-2*`
    - texlive will be installed under `/usr/local`
4. `$ sudo ./install-tl -no-gui -repository https://mirror.ctan.org/systems/texlive/tlnet/`
    - Enter command: I
5. Add symbolic link under `/usr/local/bin`
    - `$ sudo /usr/local/texlive/????/bin/*/tlmgr path add`

Update
- `$ sudo tlmgr update --self --all`

## latexmk
### 基本
参照(https://qiita.com/sigma7641/items/fc2638f92d5915326b4a)

プレースホルダ
- `%B`: 生成ベース名(拡張子なし)
- `%O`: オプション
- `%D`: 出力ファイル(拡張子込み)
- `%S`: TeXソース名(拡張子込み)

変数
- `$pdf_mode`:
    - `0`, `1`: `$pdflatex`でtexからPDFを作成
    - `2`: `ps2pdf`でPSからPDFを作成
    - `3`: `dvipdf`でDVIからPDFを作成

オプション
- `-c`: 中間ファイルの削除
- `-C`: 中間ファイルも出力ファイルも削除
- `-f`: エラーで停止しない
- `-pv`: コンパイル後に `$pdf_previewer` で指定したものでPDFをプレビューする
- `-pvc`: preview continually, 更新の際に継続ビルド
