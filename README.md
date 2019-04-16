# jigsaw-Unintended-Bias-in-Toxicity-Classification
kaggleの「jigsaw Unintended Bias in Toxicity Classification」参加時に作成したコード（進行中）

### 内容
2019年04月〜kaggleで開催されている「jigsaw Unintended Bias in Toxicity Classification」のコンペに参加し、
作成したコードと結果、スコアアップに向けた方針をまとめました。    

### コンペ概要
Jigsaw社の提供する200万近くの文章を元に、テストデータとして与えられる文章が  
「toxic（毒性の）」か否かを予測するコンペ（二値分類）。  
- **訓練データ件数**  
  1,804,874件  
- **テストデータ件数**  
  97,320件  
- **データセットのID**  
  id列  
- **データセットのラベル（目的変数）**  
  target列  
  toxicではない=0、toxicである=1  
- **評価指標**  
  AUC    


訓練データは以下のような構造（例）で、テストデータには「severe_toxicity」より右のカラムがない  
 ⇨comment_textのみを用いてモデルを構築する必要がある、と解釈しました。

| id    | target    | comment_text     | severe_toxicity | … |  
|:-----:|:-----------:|:------------|-------------------------:|-------:|  
| 50001 | 0.0003   | This is a pen      |  0.00000                |  …  |  
| 50002 | 0.9 | You are so foolish.       |  0.00001                    |  … |  
| 50003 | 0.01 |    This sentence is not toxic. Isn't this?    |  0.02  |  …  |  

データの詳細は以下リンクのData Description欄を参照。  
<https://www.kaggle.com/c/jigsaw-unintended-bias-in-toxicity-classification/data>    


### 作成したコード    
作成したコードは、同フォルダのtoxicity_classificationi.ipynbファイル内に記載しています。  
<https://github.com/takafumi-7/jigsaw-Unintended-Bias-in-Toxicity-Classification/blob/master/toxicity_classificatioin.ipynb>

大まかな流れとして、以下の順で処理するコードを作成しました。
1. 訓練データ・テストデータを前処理
2. モデルを訓練（バリデーションも行う）
3. テストデータの予測値を算出

以下にそれぞれの工程で試みたことを詳しく記載します。    


##### 1. 訓練データ・テストデータを前処理　　
以下3点を実施する。
a. 訓練データ・テストデータそれぞれについて、「comment_text」カラムに含まれる全単語とその出現回数を抽出  
b. aで抽出した単語から訓練データ・テストデータのいずれかに出現回数が偏っているもの、  
   出現回数が極端に少ないものを除去
c. 訓練データ・テストデータに対してbまでで抽出した単語の分だけカラムを作成し、  
   その単語が含まれている行の値を1、含まれていない行の値を0とする。その際、同時にsparse_matrixに変換する。  
（イメージ図）  

| id    |              comment_text               | this | foolish |  toxic  | … |  
|:-----:|:----------------------------------------|-----:|--------:|--------:|:-:|
| 50001 | This is a pen                           |   1  |    0    |    0    | … |  
| 50002 | You are so foolish.                     |   0  |    1    |    0    | … |  
| 50003 | This sentence is not toxic. Isn't this? |   1  |    0    |    1    | … |  

##### 2. モデルを訓練　　
訓練データでLightGBMを訓練する。訓練の際は、  
StratifiedKFoldを用いて5回に分けてバリデーションを行う。  

##### 3. テストデータの予測値を算出  
モデル訓練時の計5回のバリデーションの際、その一回一回の中で
訓練済みのモデルで予測値を出力する。  
バリデーション終了後、5回分の予測値の平均をとりsubmit用csvファイルを出力する。    

### 結果  
AUC  : 0.90003   
ランク: 752/893  
<https://www.kaggle.com/takafumif/competitions>    

### スコアアップに向けて  
現状大きく分けて以下の二通りの方法が考えられます。  
- ディープラーニングの理論・数式をより深く理解し、CNN, RNNのモデル構築を習得する。
  ⇨kernelsを見ると高スコア獲得者のほとんどがCNN又はLSTMを使っていたため、有効と考えられる。  
- 訓練データの前処理を研究・工夫する  
  ⇨kernelsを見ると、まだ知らないテキスト処理用ライブラリを使っているものが散見される。  
   より良い処理法が見つかる可能性は高い。  

データサイエンティストを目指す身としては、そろそろディープラーニングについての  
知見を深めてライブラリを用いた実装まではできるようになりたいため、ひとまず前者を採用します。  
このコンペの締め切りまでにはディープラーニングのモデルを使ってスコアを提出したい。
