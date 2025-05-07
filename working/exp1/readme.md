# 鳥の鳴き声を分類するディープラーニングモデルの開発
## 仕様
### 学習データ
下記フォルダにある.oggファイル。.oggファイルをまとめるフォルダはクラス名を意味する。<br>
/home/data/train_audio
<br>
"/home/data/train.csv"のfilenameは"/home/data/train_audio"配下のファイルパスをさしている。
本実験ではプログラムの正常動作を確認するために"/home/data/train.csv"の上位5000行を学習対象とする。

### 機能
機能は大きく分けて下記2つとし、それぞれipynb形式でコーディングする。
<br>
プログラム保存先はworking/exp1とする。

#### 1. 音声をメルスペクトログラムに変換する機能
- ファイル名:audio2mel.ipynb
- 音声データの正規化:min-max scaling
- メルスペクトログラムの設定値
    - サンプリング周波数
        - 変数名:SR
        - 設定値:32000
    - FFTのウィンドウサイズ
        - 変数名:N_FFT
        - 設定値:1024
    - メルフィルタバンクの数
        - 変数名:N_MELS
        - 設定値:128
    - フレーム間のずれ
        - 変数名:HOP_LENGTH
        - 設定値:512
    - 最小周波数
        - 変数名:FMIN
        - 設定値:50
    - 最大周波数
        - 変数名:FMAX
        - 設定値:SR // 2

- 出力形式
メルスペクトログラムに変換したデータは1つのnpyファイルに出力する
    - 変数名:mel_data
    - データ格納形式:mel_data["/home/data/train.csv"のインデックス] = メルスペクトログラム
    - 保存先:"working/exp1/output/meldata.npy"

#### 2. メルスペクトログラムから鳥の鳴き声を分類するディープラーニングモデルを構築する機能
- ファイル名:modeling.ipynb
- 使用モデル:EfficientNet B0
- 学習データ
    - モデル出力(クラス名):
        "/home/data/train.csv"のprimary_label列
    - モデル入力:
        "working/exp1/output/meldata.npy"
    - バリデーション設計:
        層化抽出サンプリング primary_labelが均等になるように学習、テストを設計
    - 最適化指標:
        LogLoss
    - モデル評価指標:AUC
