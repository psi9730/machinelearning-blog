---
layout: post
title: spanBERT Improving Pre-training by Representing and Predicting Spans
category: NLP
tags: [nlp]
---

Paper review on "SpanBERT: Improving Pre-training by Representing and Predicting Spans(2019-7-24)"

## Abstract
* BERT를 pre-train할 때 하나의 토큰이 아닌 span을 예측해보자
* masking을 random tokens이 아닌 random spans으로
* masked span 끝의 두개 token + 예측 할려고하는 input token representaion 으로 span을 예측해보자
* span selection tasks(question answering and coreference resolution)과 같은 문제에서 상당한 성능향상을 이끌어냄

## introduction
* 기존 BERT모델은 token중심이기 때문에 정답이 "Korean pop music"일 때 "Korean"을 예측하는것보다 "Korean pop music"을 예측하는 것을 어려워한다.
* 이에 본논문에서는 span masking scheme과 span-boundary objective(SBO)를 제안한다
* 또한 two half-length에 NSP를 이용하는 것보다 single segment를 이용하는것이 좋다.
![image-1]({{site.baseurl}}/assets/images/2019-11-11-SpanBERT-Improving-Pre-Training-By-Representing-And-Predicting-Spans-1.png)
## Model

### Span Masking
Each iteration마다 geometric distribution에서 span length를 정한다. 15% tokens를 masking하며 이때 p=0.2, l_max = 10으로 설정한다. token이아닌 span단위로 마스킹한다는 것 외에 마스킹 방식은 기존 BERT와 같다.
![image-2]({{site.baseurl}}/assets/images/2019-11-11-SpanBERT-Improving-Pre-Training-By-Representing-And-Predicting-Spans-2.png)

### Span Boundary Objective(BSO)
(x_s,...,x_e)를 추론할때 Boundary tokens인 x_s-1, x_e+1 encodings를 이용한다. 2-layer 로 GELU activation과 layer normalization을 이용한다.
![image-3]({{site.baseurl}}/assets/images/2019-11-11-SpanBERT-Improving-Pre-Training-By-Representing-And-Predicting-Spans-3.png)

### Single-Sequence Training
two sequence of text and objective NSP is worse than single sequence train withoug NSP.
1. single sequence 은 상대적으로 긴 context를 담고 있다.
2. conditioning on context from another sentence 는 masked language model에서 노이즈를 발생시킬 수 있다.
![image-6]({{site.baseurl}}/assets/images/2019-11-11-SpanBERT-Improving-Pre-Training-By-Representing-And-Predicting-Spans-6.png)

## Tasks
span based task는 span-based pretraining으로 학습시킨 모델이 더 잘 동작할 것이다.

### Extractive Question Answering
질문과 paragraph가 주어졌을 때 paragraph에서 정답 문장 또는 정답 포함 여부를 찾는 문제
* SQuAD 1.1, 2.0, MRQA, NewsQA, SearchQA, TriviaQA, HotpotQA

### Coreference Resolution
동일 지시어 분석
* CoNLL-2012

### Relation Extraction
span쌍 사이의 관계를 분석, 42가지 관계와 no_relation으로 classification한다
* TACRED

### GLUE
9 sentence-level classifcation

## Implementation
기존 BERT-large모델에 BooksCorpus, English Wikipedia로 pre-training시켰다. masking을 매 epoch마다 다르게해주었고, 최대한 512 tokens에 맡게 input을 추출했다.

### Results
제안된 Tasks에 대하여 대부분 뛰어난 성능을 보였다.
![image-4]({{site.baseurl}}/assets/images/2019-11-11-SpanBERT-Improving-Pre-Training-By-Representing-And-Predicting-Spans-4.png)

## Ablation Studies

### Masking Schemes
1. Subword Tokens
sample random word piece tokens
2. Whole words
mask all ofthe subword tokens in those words
3. Named Entities
50% of the time sample from named entities in the text
4. Noun Phrases
50% of the time sample from noun phrases
5. Random Spans
![image-5]({{site.baseurl}}/assets/images/2019-11-11-SpanBERT-Improving-Pre-Training-By-Representing-And-Predicting-Spans-5.png)

### Auxiliary objectives
![image-6]({{site.baseurl}}/assets/images/2019-11-11-SpanBERT-Improving-Pre-Training-By-Representing-And-Predicting-Spans-6.png)