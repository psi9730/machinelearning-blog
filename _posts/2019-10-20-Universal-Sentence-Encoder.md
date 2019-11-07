---
layout: post
title: Univeral Sentence Encoder
category: NLP
tags: [nlp]
---

Paper review on "Universal Sentence Encoder(2018-3-29)"

## Introduction
NLP에선 워드 레벨의 pretrained Embedding을 활용한 성능향상 연구가 있어왔는데 본 논문에서는 transformer, DAN(Deep Averaging Network) 모델의 sentence Embedding을 제시함. 동시에 transfer learning에 활용하여 다양한 분야에 적은 수의 트레이닝 데이터셋으로 우수한 성능을 낼 수 있음을 보임.

## sentence Embedding(Encoder)
1. Transformer
transformer의 Encoder sub-graph는 단어들간의 순서와 문맥 정보를 포함시키기 위해 attention을 활용함 
2. DAN(Deep Averaging Network)
Averaging words/bi-grams embedding 이후 feedforward DNN에 태워 임베딩 벡터를 생산, Transformer에 비해 문장 길이에 대한 시간복잡도가 linear하게 증가한다는 장점이 있음. 정확도는 Transformer가 대체적으로 더 높게 나옴.

### encoders
* 인코더는 PTB(Penn Tree Bank) tokenized string을 인풋으로 받고 512-d 벡터(sentence Embedding)을 반환
* Multi-task learning을 위해 Skip-Thought like task, Conversational input-response task, Classifications등의 다운 스트림 task에 문장 임베딩을 얻는 학습을 시킴.

![universal-sentence-encoder]({{site.baseurl}}/assets/images/universal-sentence-encoder.png)

## Transfer Learning 
word level Transfer/ no transfer learning의 두 모델을 베이스라인으로 삼음.
sentence Embedding, word embedding, sentence embedding+word embedding(concat)을 이용하여 transfer learning성능을 비교함.
### Transfer Learning Result
Transformer > DAN
Sentence&word embedding > Sentence embedding > word embedding
데이터가 작을 수록 sentence level transfer learning성능이 word level 대비 매우 우수

* [Universal Sentence Encoder](https://arxiv.org/abs/1803.11175)