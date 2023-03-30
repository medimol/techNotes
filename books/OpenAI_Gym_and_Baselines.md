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

### 4章 Stable Baselines
OpenAI Baselines と Stable Baselines の違い
- OpenAI Baselines のフォークであり，リファクタリングして使い勝手を良くしたものが Stable Baselines
  - RLアルゴリズムの共通インタフェース
  - ドキュメントの提供
  - IPython/Notebook のサポート

MPI (message passing interface): 並列コンピューティングを利用するための標準化された規格および実装
- Stable Baselines にはDDPG, GAIL, PPO1, PPO2 で使用されている

現在，Stable Baselines はメンテナンス中であり，Stable Baselines3 の仕様が推奨されている

`hello_baselines.py` についての解説
- PPO2: マルチプロセッシングで訓練可能な強化学習アルゴリズム
  - 環境を，「ベクトル化環境」でラップしなければならない
- ベクトル化環境
  - DummyVecEnv: 全環境を同じプロセスで実行
  - SubprocVecEnv: 各環境を個別のプロセスで実行
  - 引数で環境を返す関数を指定
- モデル: 任意の強化学習アルゴリズムによって，入力に対する正しい出力を学習するオブジェクト
  - 引数: 方策，環境，ログの詳細表示(verbose)
    - 方策
      - 'MlpPolicy': MLPを使用する方策，入力が特徴量の場合に適切
      - 'MlpLstmPolicy': MLPとLSTMを使用する方策
      - 'MlpLnLstmPolicy': MLPと layer normalized LSTM を使用する方策
      - 'CnnPolicy': CNNを使用する方策，入力が画像データの場合に適切
      - 'CnnLstmPolicy': CNNとLSTMを使用する方策
      - 'CnnLnLstmPolicy': CNNと layer noramlized LSTM を使用する方策
    - verbose
      - `0`: なし
      - `1`: 訓練情報
      - `2`: TensorFlowデバッグ
  - 今回
    - 方策: 'MlpPolicy'
    - 環境: env
    - ログの詳細表示: `1`
  - 生成されるモデルのメソッド
    - `learn()`: 訓練
      - `total_timesteps`で訓練するステップ数を指定
    - `predict()`: 推論
      - 引数: 現在の状態
      - 戻り値: 行動，LSTM状態
      - PPOやA2Cなどでは，確率的に行動するが，`deterministic=True`で決定的にすることができる(パフォーマンス向上を大いに期待できる)
    - `save()`: モデルの保存
    - `load()`: モデルの読み込み
- SB3への修正
  - PPO2は廃止されているため，PPOに
  - action space への Dict型の利用は非対応というエラーが出る
    - `make_vec_env`の使用で解決．これを使ったほうが楽

強化学習における推論と予測の違い
- 推論(inference): 現在の一部の情報をもとに，現在の全体の値を算出
- 予測(prediction): 現在の情報をもとに，未来のある値を算出

PPOの学習時のログ
- `approxkl`: 新しい方策から古い方策へのKullback-Leibler発散尺度
- `clipfrac`: クリップ範囲ハイパーパラメータが使用される回数の割合
- `explained_variance`: 誤差の分散
- `fps`: FPS
- `n_updates`: 更新回数
- `policy_entropy`: 方策のエントロピー
- `serial_timesteps`: 1つの環境でのタイムステップ数
- `time_elapsed`: 経過時間
- `total_timesteps`: 全環境でのタイムステップの数
- `value_loss`: 価値関数更新時の平均損失

SB3以外の強化学習ライブラリ
- Coach
  - TensorFlow 対応．実装済みアルゴリズムが豊富
- RLLib
  - TensorFlow, PyTorch 対応．分散アルゴリズムが充実
- Dopamine
  - TensorFlow 対応．
- SB3はソースコードが読みやすい

強化学習の分類
- モデルベース (e.g. AlphaZero)
- モデルフリー
  - オンポリシー (e.g. VPG, TRPO, PPO)
  - オフポリシー (e.g. DDPG, TD3, SAC)

モデルベース/モデルフリー
- 環境モデル(状態遷移と報酬を予測する関数)を使用(or学習)できるかどうか
  - 使用する -> モデルベース
  - 使用しない -> モデルフリー
- モデルベース
  - 利点: エージェントが先を考え，可能な選択肢の範囲で何が起こるかを見て，行動を決定できる
    - MCTS (Monte Carlo tree search)を使用した AlphaZero
      - サンプル効率が大幅に向上(学習に必要なステップ数が少ない)
  - 欠点: 適応できる問題が少ない(通常は環境の真のモデルは使えない)
    - 経験からの環境モデルの学習は偏りが発生する可能性がある
      - その結果，学習した環境モデルには適応できるが，実環境に適応できない可能性がある
      - 環境モデルの学習は単純な時間増加では難しい
- モデルフリー
  - 利点: 実装および調整が容易
    - モデルの使用によるサンプル効率の潜在的な向上は見られない
  - オンポリシー (on-policy)
    - 現在のポリシーで得られた経験のみを利用して，新しいポリシーを予測する
    - 過去の経験を利用しない -> サンプル効率は低い
    - 過去のポリシーによる経験は利用しないため，学習が安定(時間で改善)する
    - e.g. VPG (vanilla policy gradient), 1992
      - 高い報酬が得られる行動を優先し，低い報酬しか得られない行動を避けるように方策を最適化する手法
    - e.g. TRPO (trust region policy optimization), 2015
      - VPGの学習が安定するように改善
      - 方策の更新をパラメータ空間の範囲内にすることで，大幅な更新を抑制
    - e.g. PPO (proximal policy optimization), 2017
      - TRPOの計算量を削減するように改善
      - 使いやすさとパフォーマンスのバランスがGood
  - オフポリシー (off-policy)
    - 過去の経験を利用して現在のポリシーを予測する
    - 過去の経験を利用 -> サンプル効率は高い
    - 経験的には性能が得られるが，保証がなく潜在的に脆弱で不安定
    - e.g. DDPG (deep deterministic policy gradient), 2015
      - Q関数と方策を同時に学習する手法
      - i.e. 連続行動空間に対応したDQN
    - e.g. TD3 (twin delayed DDPG), 2018
      - DDPGの学習が安定するように改良
    - e.g. SAC (soft actor-critic), 2019
      - Q関数を用いた手法で
      - エントロピーの概念を導入
        - 探索と活用のトレードオフの概念を表現
        - 増加で探索，減少で活用
        - 局所最適解に陥るのを防ぐ

その他RLアルゴリズムの紹介
- DQN: Q学習と深層学習の組み合わせ
- A3C (asynchronous advantage actor-critic): 方策(actor)と価値関数(critic)の両方を利用
  - 一般的に，Actor-Criticと呼ばれる
- A2C (advantage actor-critic): A3Cを分散同期に
- ACER (actor-critic with experience replay): A3Cをオフポリシーにして，経験再生を利用できるように
- ACKTR (actor critic using Kronecker-Factored Trust Region): TRPOとActor-Criticの組み合わせ
- HER (hindsight experience replay): 失敗からも学ぶ手法，何かしらの目標を達成させることで，結果を置換させて学習

模倣学習
- 教師による一連の行動(デモ)を模倣する手法
- デモと同じ行動を採ることで報酬がもらえるRL(教師あり学習)
- 強化学習の支援に利用(大まかな方針を教える)
- e.g. BC (behavioral cloning), 1999
  - 状態ごとのデモの行動を教師あり学習で学習
  - 高速，デモを忠実に学習，多数のデモが必要
  - SBでは事前学習が可能であり，BCが利用できる(メソッドとして実装されている)
- e.g. GAIL (generative adversarial imitation learning), 2016
  - GAN (generative adversarial network) によって模倣学習を行う
    - Generator による教師データに近いデータを生成
    - Discriminator による教師データとGeneratorによるデータの識別
  - 少ないデモで効果，事前学習が不可(SBの制限)，画像による訓練は未対応(SBの制限)

平均報酬と平均エピソード長のログ出力(`monitor.py`)
- Monitor
  - 報酬，エピソード長，時間を`monitor.csv`に出力する環境ラッパー
- ターミナルの出力に以下が追加
  - `ep_len_mean`: 平均報酬
    - 増加が求められている
  - `ep_reward_mean`: 平均エピソード長
    - 今回のタスクでは，増加が求められている
- Pandas によるCSVの読み込みと，Matplotlib によるグラフ作成

モデルの保存と読み込み
- 保存: `model.learn` 後に `model.save('name')`
  - zip形式で保存される
  - 中身:
    - `data`: JSON, ハイパーパラメータの辞書
    - `parameter_list`: JSON, paramsに含まれる重みパラメータのリスト
    - `parameters`: ZIP, 重みパラメータのバイナリファイル群
      - npyはNumPy配列のバイナリファイル
  - Tensorflow.js や Java にエクスポート可能
- 読み込み: env作って，モデルを呼び出すときに，`.load('name')`する

学習状況の監視の種類
- verbose: 標準出力として訓練情報 or TensorFlowデバッグを出力
- Monitor: `monitor.csv`に平均報酬とエピソード長を出力
- TensorBoard: TensorBoardで平均報酬などを監視
- コールバック: PPO2/ACERはnステップ毎，DQNなどは1ステップ毎にコールバック関数を呼び出す

TensorBoardによる監視
- modelを呼び出す際に，`tensorboard_log=log_dir`で出力先を指定
- `$ tensorboard --logdir=./logs/` を実行し，https://localhost:6006 で学習中，学習後に平均報酬を観察できる

コールバックによる監視
- コールバック関数を定義
  - `model.learn`の引数でcallbackに渡す
- コールバックのタイミング: PPO/ACERはnステップ毎，DQNは1ステップ毎
  - PPOの引数とかに`n_steps`がある
- `callback_watch.py`: 100ステップごとに100エピソードの平均報酬を計算し，最大の平均報酬を超えたらモデルを保存するコード
  - `def callback(_locals, _globals):`
  - `_locals`と`_globals`は辞書型で，それぞれ多くのキーを持つ
  - 戻り値はbool(学習を継続するかどうか)
  - `load_results()`により`monitor.csv`を読み込み
  - `ts2xy()`: DataFrameをx軸とy軸の配列に分割．今回はx軸がステップ数，y軸が報酬
  - `_locals['self']`によるモデルへのアクセス

マルチプロセッシング
- ベクトル化環境: 複数環境で同時に学習できる環境
  - 次の状態，報酬，エピソード完了，情報などをまとめた配列(ベクトル)
- ベクトル化環境の種類:
  - `DummyVecEnv`: 全環境を同じプロセスで実行
  - `SubprocVecEnv`: 各環境を別プロセスで実行
- CPUコアが2-8程度であれば，DummyVecEnvの方が高速
  - プロセス間通信の通信遅延がボトルネックに

===
正誤表
- p.104: オンポリシーにて「過去の経験を利用するため，サンプル効率は低い」となっている
- p.106: A3Cが「Advantage Actor-Critic」になっている
- p.107: GAILが「Imitaiton」の誤字
- p.118: 「model.learn()の引数「tensorboard_log」に，」
