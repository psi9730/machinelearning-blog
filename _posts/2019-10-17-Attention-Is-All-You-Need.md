---
layout: post
title: Attention is all you need
category: NLP
tags: [nlp]
---

Paper review on "attention is all you need(2017-11-6)"

## 트랜스포머(transformer)란?
Attention mechanism을 기반으로 complex recurrent or convolutional neural network기반 sequence transduction model보다 parallelizable 하게 실행시키기 용의하며 트레이닝 시간을 명확히 줄일 수 있다. 기존 encoder-deocoder기반 sequence 모델은 다음 예측을 위해 그전 hidden_state과 input이 필요하며 이에 의해 input sequence가 늘어날 수록 시간, 메모리 활용면에서 문제가 난타난다. 또한 단어간 시간 차이가 클수록 영향력이 작아지는 문제가 있는데 이를 해결하기 위해 attention 개념이 등장했다. transformer는 self-attention을 이용하여 input, output간의 global dependency를 잡아냈으며, scaled dot-product, multi head attention을 이용하여 parallel processing에 장점을 가진다.

## Seq2Seq모델에서 attention이란?
디코더의 특정 time-step의 output이 인코더의 모든 time-step의 output 중 어떤 time-step과 가장 연관이 있는가

## self-attention이란?
1. Scaled Dot-Product Attention
Scaled Dot-Product는 Q와 K 을 내적하여 어텐션을 Softmax를 통해 구하고, 그 후에 V에 내적하여 중요한 부분(Attention)에 따른 V값을 구하는 것을 말한다. 이 때 Self Attention이란 Q, K, V에 같은 벡터를 넣어서 같은 문장 내에서 상호 중요도를 평가한다고 볼 수 있다.
(base attention function엔 additive와 dot product attention이 있다고 한다.)
2. Multi-Head attention
heads의 수만큼 다른 관점으로 attention을 구한 다음 이를 concatenate하여 최종 Attention을 구한다. 하나의 문장의 의미를 다양한 시각으로 해석할 수 있음을 적용한 개념.
{% highlight css %}
    MultiHead(Q, K, V) = Concat(head_1, ..., head_h)W^O
        where head_i = Attention(QW_i^Q, KW_i^K, VW_i^V)
{% endhighlight %}
3. position-wise feed-forward networks
Convolution layer을 이용해 concatenate된 multi-head attention을 균등하게 섞는 역할을 한다. 기존 attention은 head에따라 편향된 attention을 가지고 있음으로 이를 해결하기 위해 position-wise networks를 이용한다.

## 이외 주요개념
1. positional encoding
rnn의 장점중하나는 input의 순서를 기억하고 결과에 반영하는 것이 있다. 본 논문에서는 input에 이러한 order of sequence을 반영하기 위해 코사인, 사인 함수를 이용해서 input sequence embedding에 위치정보를 반영했다.

2. Layer Normalization
sub-layer가 residual connection으로 연결, 그 후에는 layer-normalization과정을 거침

3. Residual dropout

4. Label smoothing
## 참고문헌
* [Attnetion Is All You Need](https://arxiv.org/abs/1706.03762)


