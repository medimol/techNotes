# OpenAI Gym/Baselines 深層学習・強化学習人工知能プログラミング実践入門
布留川 英一. 株式会社ボーンデジタル, 2020.02.25.

Finish reading at:

##### Purpose
強化学習の実戦経験を積まなければならない

## Notes

### 1章 深層学習と強化学習の概要
即時報酬と遅延報酬
- 即時報酬: 行動直後に発生する報酬
- 遅延報酬: 遅れて発生する報酬

利益と価値
- 利益: すべての報酬和．これを最大化することが求められる
  - 割引報酬和など，環境からの報酬に対してエージェントの価値観が考慮される
- 価値: 不確定な未来の収益を，状態と方策を固定した場合の条件付き収益
  - 価値を学習する(価値が大きくなる条件を探す)

1. 価値の最大化
2. 収益の最大化
3. 多くの報酬を得られる方策

Stable Baselines は内部的にTensorFlowを使っている

開発フレームワーク
- OpenAI Gym: 強化学習用のツールキット
  - エージェントと環境のインタフェース
- OpenAI Baselines & Stable Baselines
  - 強化学習アルゴリズムの実装セット
  - Stable Baselines は改良版
- Gym Retro
  - レトロゲームを OpenAI Gym の環境として利用するためのライブラリ

### 2章 Pythonの開発環境の準備
私は Anaconda ではなく pyenv と poetry を使う

GPU版のTensorFlow
- 必要なもの
  - NVIDIA の GPU をもつパソコン
    - AMD でも動かす方法はあるらしい
  - NVIDIAドライバと CUDA と cuDNN のインストール
    - CUDA: NVIDIAが提供しているGPU向けの統合開発環境
    - cuDNN: NVIDIAが提供している深層学習用のライブラリ
  - tensorflow-gpu のインストール

TPU: tensor processing unit

Google Colab にはホスト型ランタイムがある
- 自身のリソースも提供できる

#### Python
数値型
- int, float, bool, complex
- complex: 複素数

比較演算子
- a と b が異なる表現
  - `a != b`
  - `a <> b`
  - `a is not b`

三項演算子
`aviator = "maverick" if age > 40 else "rooster"

文字の埋め込み
- `'複数の変数 = {}, {}, {:.2f}'.format(a, b, c)'`
  - 浮動小数点の桁数指定: `:.2f`は小数点以下2桁

lambda式
- 関数を式として扱い，変数に代入できるようにする
- 書式: `lambda 引数:戻り値のある関数`
```py
lambda_radian = (lambda x:x / 180 * 3.14)
lambda_radian(90)
```
