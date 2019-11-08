---
layout: post
title: Multi Task Deep Neural Networks for Natural Language Understanding
category: NLP
tags: [nlp]
no-post-nav: true
---

Paper review on "Multi Task Deep Neural Networks for Natural Language Understanding(2019-1-31)"

## Abstract
* Multi-task Deep Neural Network for learning representations accross multiple NLU tasks
* BERT에 multi task learning을 적용하여 regularization effect + 특정 task에 대한 fine-tuning에 필요한 데이터양을 줄여주며 + 성능향상까지
* representaion을 Language model by unsupervised data + multi-task learning by supervised data 으로한다는데 가장 큰 의의가 있다

## Introduction
* MTL을 이용하면 사람이 스키를 탈줄알면 스케이팅을 배우기 쉽듯이 다른 task에 대해 긍정적 영향을 준다.
* 특정 task에 대해 overfitting 되지 않아 regularization효과를 가진다.
* MT-DNN 이후에 task-specific labeled data가 적어도 fine-tuning이 잘된다.
* 두번의 fine-tuning과정 1. MTL 2. task specific fine tuning 하지만 논문에서는 MTL은 fine tuning이라 칭하지는 않는것같다??

## Tasks
MT-DNN 다음과 같은 4가지 objectives를 가진다.
* single-sentence classification
    - COLA: 문법적으로 맞는지 Binary classification
    - SST-2: 긍정 부정 Binary Classification
* pairwise text classification
    - RTE, MNLI: entailment, contradiction or neutral 예측하기
    - QQP, MRPC: 두개의 문장이 의미적으로 같은지
* text similarity scoring
    - STS-B: 두 문장 사이 유사도(0~1) 
* relevance ranking
    - QNLI: questions에 대한 정답이 paragraph에 있는가, 해당 논문에서는 pairwise-ranking task로 풀어 성능을 올렸다고 한다.

## The proposed MT-DNN Model

## The Training Procedure
![image-1]({{site.baseurl}}/assets/images/2019-11-08-Multi-Task-Deep-Neural-Networks-For-Natural-Language-Understanding-1.png)
Pretrained 된 BERT model의 shared layers에다가 MTL을 SGD로 학습한다. 
shared layers, task specific layers를 동시에 학습한다.
데이터를 
![image-1]({{site.baseurl}}/assets/images/2019-11-08-Multi-Task-Deep-Neural-Networks-For-Natural-Language-Understanding-1.png)


