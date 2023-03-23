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

### 3章 OpenAI Gym
**現在はOpenAIはGymの開発を行っておらず，そのGymもGymnasiumにフォークされている**

OpenAI Gym は，OpenAI が提供している強化学習用のツールキット
- エージェントと環境間の共通インタフェース
  - シンプル
  - 応用が効く
- 基本的な環境(シナリオ)の提供
- 比較可能性
  - 強化学習アルゴリズムで結果を比較可能
- 再現性
  - 環境に対する厳密なバージョン管理
- 学習状況の冠詞
  - Monitor 機能によるステップ毎の記録

Gymが提供している環境
- Algorithmic: 文字列を操作する方策を学習する環境
- Atari: Atariの100本以上のゲーム環境
- Box2D: OSの2D物理エンジンで開発された連続的な制御タスクを行う環境
  - 二足歩行ロボット，月面着陸，レースカーなど
- Classic control: 古典的な制御タスク
  - CartPole, MountainCar, etc.
- MuJoCo: 3D物理エンジンで開発された連続的な制御タスクを行う環境
  - 二足歩行ロボットの歩行・走行
  - ロボット工学のRLでは有名だが，商用ライセンスが必要
- Robotics: ロボットアームやロボットハンドの環境(MuJoCoを使用)
- Toy text: テキストベースの古典的なタスクを行う環境
  - Frozen Lake, Taxi, etc.

Env クラスの主なメソッド
- `make(id)`: 環境のインスタンス生成
- `reset()`: 環境のリセット
  - エピソード開始時に呼ぶ
  - 戻り値はエピソードの最初の状態
- `step(action)`: 環境の1ステップ実行
  - 戻り値は`results`(tuple型)
    - state(object), reward(float), done(bool), info(dict)
- `render()`: 環境の描画
  - render mode
    - "human": ディスプレイ/ターミナルに描画
    - "rgb_array": ピクセル画像のRGB配列を戻り値として返す
    - "ansi": テキストを戻り値として返す
- `close()`: 環境の終了
- `seed()`: 乱数シードの指定

Env クラスの主なプロパティ
- `action_space`: 行動空間
- `observation_space`: 状態空間

環境IDの命名規則
- "v"の後ろの数字がバージョン名
- "ram"がある場合は，AtariのゲームにおけるRAMの情報が環境から帰ってくる
- "deterministic"がある場合は，行動が4フレーム実行され，その後の状態が返される
- "NoFrameskip"がある場合は，1フレームごとに行動が実行され状態が返される
- "deterministic"も"NoFrameskip"がない場合は，行動が`n`フレーム繰り返される
  - `n={2,3,4}`

環境がどのような行動と状態を持っているかは，ソースコードを見るしかない

行動空間と状態空間の型
- Box クラス
  - low(最小)とhigh(最大)の間の連続値，float型
- Discrete クラス
  - 0から n-1 の離散値，int型
- MultiBinary クラス
  - 0 or 1，int型
- MultiDiscrete クラス
  - Discrete の配列
