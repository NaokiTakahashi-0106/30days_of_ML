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





