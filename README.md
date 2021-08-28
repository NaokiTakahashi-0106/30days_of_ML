# 日記

## 2021/08/17

fkubotaさん（https://zenn.dev/fkubota/articles/3d8afb0e919b555ef068）のkaggle日記

https://github.com/fkubota/kaggle-Cornell-Birdcall-Identification

を参考に作成してみた。

今日は提出までの流れを確認するにとどめる。

結果は661位。
まあ、こんなものでしょう。

明日はDiscussionとNotebookをひたすら読み込むことにする。
早速Disscussionで日本語のnotebookをUPした方がいたので保存。

## 2021/08/18

Notebookを2つ読む。
0818_30dml-eda-xgboost.ipynbはEDAをかなり行っており、手法も含めて勉強になる。
### 0818_30dml-eda-xgboost
- 訓練データ、テストデータ共に欠損値はなし
- 訓練データのtargetにおいて5未満は外れ値
- 訓練データとテストデータの間にデータの不均衡はなし（もう一つの内容と矛盾する）
- targetと特徴量の間の相関関係は小さい

EDA後はOrdinalEncoderでobject変数を前処理した後、
交差検証でRNSEを確認している。
XGBで見られる特徴量の重みもヒートマップと変わらない。

### 0818_english-tps-feb-pseudo-labeling-11th-place
日本語で読みやすそうなので読んでみた。
擬似ラベリングはリークではないのか疑問。
専門書で確認したい。
勉強になったので次の箇所。
- targetが外れ値の行を除外(targetが4より小さい行を除外)  
- 変数"cat6"について"G"は学習データにしか存在しない(テストデータで値がGをとるデータが存在しない)ので、cat6の値がG 
  の行を除外(上のNoteBookと矛盾する) 
- cont列に対するRankGauss変換（手法は書いていないが、手元のkaggle本に手法は書いてあるので対応はできる） 
- LightBGMのパラメータ（optimizeする必要がなくなった）

Discussionも読んでみた。
https://www.kaggle.com/c/30-days-of-ml/discussion/265963
はEDAやFeature Engineeringの参考手法がかなり掲載されており勉強になりそう。
また、
https://www.kaggle.com/c/30-days-of-ml/discussion/266099
は精神衛生にはちょうど良い。

最後にDistordも覗いてみた。
上位が団子状態というのは変わらない感想の模様。
誰かPytorchのNotebookあげてくれないかなあ。

明日は
https://www.kaggle.com/c/30-days-of-ml/discussion/265963
の内容を一読しようと思います。

## 2021/08/19
notebookの方に書いたので概要だけ
- targetが外れ値の行を除外(targetが4より小さい行を除外) 
  →targetが多峰性分布になった。次の記事によればLidge回帰やLassoが有効？
  https://towardsdatascience.com/anchors-and-multi-bin-loss-for-multi-modal-target-regression-647ea1974617
- 一部のカテゴリ変数は列ごと削除することも考慮に入れる。
- また、cat6のGは学習・テスト両方ともあり
- カテゴリ変数をその他で表す方法を学んだ
- 数値変数同士の相関性は薄い

## 2021/08/20
とりあえずtarget_encodingと交差検証を試してみたかったので、<br>
0820_cats-on-a-hot-tin-roof-cats-encoding-methods<br>
を試してみる。<br>
0.71くらいのスコアが出たので提出してみたが、結果は0.75072という結果に。過剰適合かどうかもわからない。<br>
リッジ回帰も試してみたところ、0.73985と改善した。<br>

コピペだけではどうしようもないので、<br>
明日以降は自分の手でtarget_encoding→Lightbgmとリッジ回帰で交差検証<br>
という流れでやってみようと思う。<br>
順位よりも今の自分に不足している部分を学習するという観点で進めていきたい。

## 2021/08/21
- target_encodingと交差検証を両方試したが混乱した。<br>
target_encodingは一旦置いておいた方がいい。<br>
別の変換方法を試して、リッジ回帰・Lasso・LightGBMの3つでスコアを検証した方が勉強になるだろう。<br>
- ノートブックにも記入したが値は全く変わらなかった。<br>
この点についてはもっと検証したい。<br>
明日はnotebookを読んで上位のカテゴリ変換方法を参考にして、<br>
何か一つの推定方法を試して交差検証する方向で進めようと思う。

## 2021/08/24
- LightGBM×optunaでパラメータ設定したモデルに交差検証を行った。<br>
別のノートから持ってきた手法であるが、<br>
RMSEの値だけでなく、特徴量の重要性を測定することができ非常に使い勝手は良い。<br>
ただし、カテゴリ変数が軒並み低い値であったことが気になる。<br>
連続変数を標準化しても値は変化しなかった。<br>
- また、LightGBMではカテゴリ変数を扱えることを思い出したので<br>
pandasを用いてカテゴリ変数に変換して分析を行った。<br>
結果は次の図の通り順位更新はできたのだが、エラーが発生し、特徴量の重要性を見ることはできなかった。

<img width="963" alt="スクリーンショット 2021-08-24 14 09 59" src="https://user-images.githubusercontent.com/78991083/130561171-c5894991-667b-4f7d-9d37-8fcc4e6f2e89.png">

- LightGBMで特徴量エンジニアリングをしているnotebookを発見したので、<br>
明日はそこから始めることにする。

## 2021/08/25~27
- LightGBMの特徴量エンジニアリングは大した内容ではなかった。
- 上位陣がほとんどアンサンブルやスタッキングで推論していたが、
- 本当にその内容で上位に行けるのか確認したが、スコアはかなり良化した
- 初心者向けのコンペでこの内容はどうかと思うが仕方がない
- 最後はスタッキングしか良い手段が思い浮かばないので、
- GBRTとDNNとLinearModelを組み合わせて提出することにする。
- PytorchでDNNを実施したが、スコアが全くよくならない。

## 2021/08/28
- 前日のPytorchのモデルの予測値を提出してみた。
- 結果は0.73935で08/21のリッジ回帰と同程度の結果となった。
- tensorflowでやった人のnotebookもこのくらいだったので、
- 妥当かもしれない。
- 上位陣はstackingで埋まっている。
- 一層目でどのモデルを使用しているかでもわかればありがたいのだが…
- foldの分け方から全てをノートに示しているGrandMasterの方がいらっしゃったので、ありがたく参考とする。
