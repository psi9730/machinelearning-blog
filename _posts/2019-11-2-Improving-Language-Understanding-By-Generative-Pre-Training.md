---
layout: post
title: Improving Language Understanding By Generative Pre Training
category: NLP
tags: [nlp]
---

Paper review about "Improving-Language-Understanding-By-Generative-Pre-Training\(2018)"

## Introduction
2-stage framework: large unlabeled text corpus dataset을 이용하여 unsupervised pre-training 을 진행하고 labeled data을 이용하여 각 task에 맞게 fine tuning을 진행한다.
이에 본 논문에서는 해당 과정을 통해 minial changes to the model architecture을 하게 해주는 generative하게 사용가능한 pre training모델을 제시한다.

## Challenging for generalized unsupervised learning
1. 어떤 optimization objective을 써야 transfer하기에 적합한 pre train학습을 할 수 있을까?
    Deep autoencoder, use standard language modeling objective to maximize likelihood
    ![image-1]({{site.baseurl}}/assets/images/improving-language-understanding-by-generative-pre-training-1.png)
    ![image-2]({{site.baseurl}}/assets/images/improving-language-understanding-by-generative-pre-training-2.png)
2. How to Transfer learned representaions to the target task에 대한 정답이 아직 없다.
    Auxiliary objective function를 추가로 이용함과 동시에 구조를 각각의 task에 맞게 조금씩 변형시킨다. 밑에 식에서 L_2는 supervised training을 위한 objective, L_1은 auxiliary unsupervised learning에서 구한 objective이다.
    ![image-3]({{site.baseurl}}/assets/images/improving-language-understanding-by-generative-pre-training-3.png)
    ![image-4]({{site.baseurl}}/assets/images/improving-language-understanding-by-generative-pre-training-4.png)

## Experiments
1. pretrainig데이터
     BookCorpus dataset을 사용하였다.([https://github.com/soskek/bookcorpus](https://github.com/soskek/bookcorpus) 를 이용하자)

2. fine-tuning 데이터
![image-5]({{site.baseurl}}/assets/images/improving-language-understanding-by-generative-pre-training-5.png)

## Analysis
![image-6]({{site.baseurl}}/assets/images/improving-language-understanding-by-generative-pre-training-6.png)
layer을 크게 할수록 좋은 성능을 내며, LSTM보다 transformer을 이용했을 때 더 좋은 성능을 냄을 볼 수 있다.

## 추가지식
1. Zero shot behaviors
한번도 듣도보도 못한 클래스를 분류하도록 학습하는것. 데이터를 하나도 안보고 하는것??!!pretrained 이후 바로 결과를 찾는것.
2. BPE(byte pair encoding)
단어 분리(Subword segmenation) 작업은 하나의 단어는 의미있는 여러 단어들의 조합으로 구성된 경우가 많기 때문에, 단어를 여러 단어로 분리해보겠다는 전처리 작업입니다. 실제로, 언어의 특성에 따라 영어권 언어나 한국어는 단어 분리를 시도했을 때 어느정도 의미있는 단위로 나누는 것이 가능합니다. 삼성에서 논문을 작성했으며, 최고 빈도 token을 합치면서 단어를 분리해내는 기법.

## 참고문헌
* [Improving-Language-Understanding-By-Generative-Pre-Training](https://s3-us-west-2.amazonaws.com/openai-assets/research-covers/language-unsupervised/language_understanding_paper.pdf)