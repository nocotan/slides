<!-- page_number: true -->

# Why ReLU Networks Yield High-Confidence Predictions Far Away From the Training Data and How to Mitigate the Problem
Ridge-i inc.
Masanari Kimura (mkimura@ridge-i.com)

---

# About

<style>
.column-left{
  float: left;
  width: 60%;
  text-align: left;
}
.column-right{
  float: right;
  width: 40%;
  text-align: left;
}
</style>


<div class="column-left">
  
### <span style="color: CornflowerBlue">Education & Career</span>
  
* 筑波大学卒 (2018)
* 株式会社Ridge-iエンジニア (2018 ~ )
* 産総研特専研究員 (2019 ~)

</div>

<div class="column-right">
Twitterやってます

<span style="color: CornflowerBlue">@machinery81</span>
  
<img src="./icon.jpg" style="width:40%">
</div>


### <span style="color: CornflowerBlue">Researches ater joining Ridge-i</span>

* <font size="4">Interpretation of Feature Space using Multi-Channel Attentional Sub-Networks (<span style="color: CornflowerBlue"><strong>CVPRW2019</strong></span>)</font>
* <font size="4">Intentional Attention Mask Transformation for Robust CNN Classification (MIRU2019)</font>
* <font size="4">PNUNet: Anomaly Detection using Positive-and-Negative Noise based on Self-Training Procedure (MIRU2019)</font>
* <font size="4">Progressive Data Increasing as the Neural Network Initializer (JSAI2019)</font>
* <font size="4">Anomaly Detection Using GANs for Visual Inspection in Noisy Training Data (<span style="color: CornflowerBlue"><strong>ACCVW2018</strong></span>)</font>
* <font size="4">Analyzing Centralities of Embedded Nodes (<span style="color: CornflowerBlue"><strong>ICDMW2018</strong></span>)</font>

---

今回紹介する論文

# Why ReLU Networks Yield High-Confidence Predictions Far Away From the Training Data and How to Mitigate the Problem

---

# Abstract
* CVPR2019採択論文 [1]
* DNNsの出力の信頼度に関する論文

---

# Confidence of DNNs outputs

* 一般的なsoftmaxを用いたDNNsの出力例

<img src="./fig_nn_softmax.png" />

---

# Problem of Overconfident Predictions
* 学習データと全く関係ないタスクのデータを入力
	*　<span style="color: #0E73B9">DNNsは非常に高い確信度で適当なクラスに分類してしまう</span>
	*　<span style="color: #E74832">本当はどのクラスの予測確率も低くあってほしい</span> 

<img src="./fig_over_confident.png" />

---

# Problem of Overconfident Predictions

<img src="./fig_over_confident_moon.png" width="90%" />

---

# Why ReLU Networks lead Overconfident ?

---

## <span style="color: #0E73B9">ReLU networks produce piecewise affine functions</span>

<img src="./fig_def1.png" />

### <span style="color: #E74832">→ NN with ReLU = piecewise affine function $\circ$ classifier</span>

---

## <span style="color: #0E73B9">ReLU networks produce piecewise affine functions</span>

ReLUを用いた，最終層が全結合層であるようなネットワークは，

  1. 入力を有限の超多面体に分割
  2. 全結合層で分類


と解釈できる[2]．

<img src="./fig_affine.png" width="90%"/>


---

## <span style="color: #0E73B9">Why ReLU Networks lead Overconfident ?</span>

<center>
<img src="./fig_theorem1.png" width="70%"/>
</center>

---

## <span style="color: #0E73B9">Why ReLU Networks lead Overconfident ?</span>

<img src="./fig_theorem2.png" width="40%"/>

### ReLUを使ったネットワークは$\alpha x$の予測確率の極限が1になる

* $\alpha$は定数
* $\alpha x$とは？

---

## Intuitive Understanding

* 以下のように入力を変換

<img src="./fig_convert.png" width="100%"/>

---

## Intuitive Understanding

* 学習データ＆想定した入力データは以下のエリアに変換

<img src="./fig_convert1.png" width="100%"/>

---

## Intuitive Understanding

* 想定していない入力データは以下のエリアに変換
* 定数$\alpha > 0$を掛ける→変換後の空間で右上に．．．
→　<span style="color: #E74832">$\alpha x$ = 想定していない入力</span>

<img src="./fig_convert2.png" width="100%"/>

---

（再活）

<img src="./fig_theorem2.png" width="50%"/>

つまり，<span style="color: #E74832">想定していない入力に対する予測確率の極限が1になる</span>

---

## <span style="color: #0E73B9">Adversarial Confidence Enhanced Training</span>

* 想定していない入力に対する予測確率を低くするような正則化

<img src="./fig_proposed1.png" width="80%"/>

---

## <span style="color: #0E73B9">Adversarial Confidence Enhanced Training</span>

* 想定していない入力に対する予測確率を低くするような正則化

<img src="./fig_proposed2.png" width="100%"/>


---

# Experimental Results

* 各データセットに対するOverconfidentの実験結果

<img src="./fig_overconfident.png" />

---

<img src="./fig_overconfident.png" />

<center>
<img src="./fig_overconfident2.png" width="85%"/>
</center>

---

# Conclusion & 疑問

* ReLUを用いたネットワークのoverconfident問題について理由づけ
* それを解決する正則化手法を提案

* そもそもsoftmaxの出力を信頼度と捉えてしまっていいのか
  * 不確実性を取り扱う手法を検討した方がいい気もする 
    * Bayesian NNs etc.

---


# References

* [1] Hein, et al. "Why ReLU Networks Yield High-Confidence Predictions Far Away From the Training Data and How to Mitigate the Problem" The IEEE Conference on Computer Vision and Pattern Recognition (CVPR). 2019.
* [2] R. Arora, A. Basuy, P. Mianjyz, and A. "Mukherjee.Understanding deep neural networks with rectified linear unit" International Conference on Learning Representations (ICLR).2018.