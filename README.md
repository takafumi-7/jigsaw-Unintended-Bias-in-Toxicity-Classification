# jigsaw-Unintended-Bias-in-Toxicity-Classification
kaggleの「jigsaw Unintended Bias in Toxicity Classification」参加時に作成したコード（進行中）

### 内容
2019年04月〜kaggleで開催されている「jigsaw Unintended Bias in Toxicity Classification」のコンペに参加し、
作成したコードと今後の課題をまとめました。    

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
|:--------:|:-----------:|------------:|-------------------------:|-------:|  
| 50001 | 0.0003   | This is a pen      |  0.00000                |  …  |  
| 50002 | 0.9 | You are so foolish.       |  0.00001                    |  … |  
| 50003 | 0.01 |    This sentence is not toxic. Isn't this?    |  0.02  |  …  |  

データの詳細は以下リンクのData Description欄を参照。  
<https://www.kaggle.com/c/jigsaw-unintended-bias-in-toxicity-classification/data>    


### 作成したコード    
作成したコードは、同フォルダのtoxicity_classificationi.ipynbファイル内に記載しています。  
<https://github.com/takafumi-7/Microsoft-Malware-Prediction/blob/master/malware_prediction.ipynb>

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
c. 

### スコアアップに向けて  
現状大きく分けて以下の二通りの道が考えられる。
- ディープラーニングの理論・数式をより深く理解し、CNN, RNNのモデル構築を習得する。
  ⇨kernelsを見ると高スコア獲得者のほとんどがCNN又はLSTMを使っていたため、有効と考えられる。  
- 訓練データの前処理を研究・工夫する
  ⇨kernelsを見ると、まだ知らないテキスト処理用ライブラリを使っているものが散見されるため、  
   より良い処理法が見つかる可能性は高い。

### スコアアップに向けて  
これ以上テキストデータ前処理をの劇的な変化は考えにくいため、

### 課題
