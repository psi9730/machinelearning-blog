---
layout: post
title: RoBERTa A Robustly Optimized BERT Pretraining Approach.
category: NLP
tags: [nlp]
---

Paper review on "RoBERTa: A Robustly Optimized BERT Pretraining Approach(2019-7-26)"

## Abstract
* 하이퍼파라미터와 training data size 의 중요성을 보여주는 논문이다.
* GLUE, RACE, SQuAD에서 STOA를 달성

## Introduction

### modification on BERT
* 더 큰 batch size, epoch으로 학습한다
* NSP를 적용시키지 않는다
* 더 긴 sequences로 학습한다
* masking patter을 epoch마다 변화시켜준다

### contribution
* better design choices and training strategies
* CC-News which is new data
* better masked language model pretraining 

## Experimental Setup

### Implementation
1. Adam epsilon term
2. B_2 = 0.98
3. large batch size
4. full-length sequences

### Data
increasing data size 는 end task performance를 올려준다.
1. BOOKCORPUS plus ENGLISH WIKIPEDIA
2. CC-NEWS: collected from the English protion of the CommonCrawl News dataset
3. OPENWEBTEXT: text is web content extracted from URLS shared on Reddit
4. STORIES: story like style of Winograd Schemas

### Evaluation
GLUE, SQuAD, RACE

## Training Procedure Analysis

### Static vs Dynamic Masking
기존 BERT는 하나의 sentence에 masking이 하나
RoBERTa는 10개의 duplicated sentence를 만든다.
![image-2]({{site.baseurl}}/assets/images/2019-11-18-RoBERTa A Robustly Optimized BERT Pretraining Approach-2.png)

### Model Inut Format and Next Sentence Prediction
1. SEGMENT-PAIR+NSP
input has a pair of segments from natural **sentences**

2. SENTENCE-PAIR+NSP
input has a pair of natural sentences from **document**

3. FULL-SENTENCES
packed with full sentences sampled **contiguously** from one or more documents.

4. DOC-SENTENCES
packed with full sentences from documents which not cross document boundaries

특정 분야에 대해서 NSP는 장점을 보이기도 단점을 보이기도한다.
DOC-SENTENCES가 FULL-SENTENCES보다 성능이 좋지만 DOC-SENTENCES는 batch size를 조절해야한다는 구현상의 단점이 있어 본 논문에서는 FULL-SENTENCES를 채택한다.
본 논문에서는 NO NSP, FULL-SENTENCES를 사용한다.

![image-3]({{site.baseurl}}/assets/images/2019-11-18-RoBERTa A Robustly Optimized BERT Pretraining Approach-3.png)

### Training with large batches
![image-4]({{site.baseurl}}/assets/images/2019-11-18-RoBERTa A Robustly Optimized BERT Pretraining Approach-4.png)

### Text Encoding
unicode characters 는 큰 데이터셋을 encoding할 때 sizeable portion을 차지한다.
이에 bytes characters를 base subword units로 이용하여 많은 양의 data(over 50K units)를 표현할 수 있도록 한다.
실제 성능이 많이 좋아지진 않지만 universal encoding scheme이 가지는 장점이 있을 것이라 주장

## RoBERTa
1. dynamic masking
2. FULL-SENTENCES without NSP Loss
3. large mini-batches
4. large byte-level BPE

### Important factors
1. data used for pretraining
2. the number of training passes through the data
![image-5]({{site.baseurl}}/assets/images/2019-11-18-RoBERTa A Robustly Optimized BERT Pretraining Approach-5.png)

## Results
good performance on GLUE, SQuAD, RACE tasks
ephasize do not rely on data augmentation, ensemble and multi-task finetuning
![image-6]({{site.baseurl}}/assets/images/2019-11-18-RoBERTa A Robustly Optimized BERT Pretraining Approach-6.png)
![image-7]({{site.baseurl}}/assets/images/2019-11-18-RoBERTa A Robustly Optimized BERT Pretraining Approach-7.png)
![image-8]({{site.baseurl}}/assets/images/2019-11-18-RoBERTa A Robustly Optimized BERT Pretraining Approach-8.png)

## conclusion
* performance improved by training the model longer, with bigger batch or more data. 
* Removing NSP and applying dynamic masking pattern
* add CC-NEWS dataset