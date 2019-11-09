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
* Lexical Encoder 
BERT와 같은 구조를 가진다. word-piece embedding, segment embedding, position embedding 3가지 embedding을 이용하며 문장 맨앞엔 [CLS], 문장쌍을 이용할 땐 [SEP]토큰을 이용한다.

* Transformer Encoder
pre training을 하는 부분인데 모든 task에서 공유하는 부분이다. BERT모델은 이부분을 오로지 unsupervised pre-training하는 반면에, MT-DNN은 BERT의 pre-training에 이어서 multi-task objectives을 이용하여 학습시킨다. 이를 통해 보다 #enerative한 representation을 만들 수 있다.
* Single-Sentence Classification Output
classification by softmax on [CLS] representation
![image-6]({{site.baseurl}}/assets/images/2019-11-08-Multi-Task-Deep-Neural-Networks-For-Natural-Language-Understanding-6.png)
* Text Similarity Output
get regression data by sigmoid on [CLS] representation (0~1)
![image-7]({{site.baseurl}}/assets/images/2019-11-08-Multi-Task-Deep-Neural-Networks-For-Natural-Language-Understanding-7.png)
* Pairwise Text Classification Output
문장 쌍에서의 관계를 살펴볼때 한번 씩 문장을 봐서는 전체의 맥락을 파악하지 못할 수 있다에서 근거하여, GRU의 input에는 premise, hidden state에는 hypothesis 문장을 넣는다.
![image-8]({{site.baseurl}}/assets/images/2019-11-08-Multi-Task-Deep-Neural-Networks-For-Natural-Language-Understanding-8.png)
* Relevance Ranking Output
Question에 대한 답이 paragraph에 있는지 여부를 binary로 찾는 문제를, paragraph안에 모든 예상 answer에 대한 relevance score를 구해 문제를 해결한다.
![image-9]({{site.baseurl}}/assets/images/2019-11-08-Multi-Task-Deep-Neural-Networks-For-Natural-Language-Understanding-9.png)

## The Training Procedure
![image-1]({{site.baseurl}}/assets/images/2019-11-08-Multi-Task-Deep-Neural-Networks-For-Natural-Language-Understanding-1.png)
Pretrained 된 BERT model의 shared layers에다가 MTL을 SGD로 학습한다. 
shared layers, task specific layers를 동시에 학습한다.
데이터는 모두 합친 뒤 shuffle한 다음 batch만큼 가져오는 식으로.
3가지 objectives가 있다. 
- (6): cross entroy loss for single-sentence, pairwise text classification
- (7): mean squared error for text similarity tasks
- (8),(9): minimize negative log likelihood of positive example for relevance ranking tasks

![image-2]({{site.baseurl}}/assets/images/2019-11-08-Multi-Task-Deep-Neural-Networks-For-Natural-Language-Understanding-5.png)
![image-3]({{site.baseurl}}/assets/images/2019-11-08-Multi-Task-Deep-Neural-Networks-For-Natural-Language-Understanding-2.png)
![image-4]({{site.baseurl}}/assets/images/2019-11-08-Multi-Task-Deep-Neural-Networks-For-Natural-Language-Understanding-3.png)
![image-5]({{site.baseurl}}/assets/images/2019-11-08-Multi-Task-Deep-Neural-Networks-For-Natural-Language-Understanding-4.png)

## Experiments
1. GLUE
collection of nine NLU tasks이며, MLT에 주로 이용한 데이터
2. SNLI
sentence pair가 들어가있으며 주로 entailmnent데이터로 쓰인다, 해당 논문에서는 domain-adaptation으로만 쓰였다. (fine tuning하여 BERT와 성능비교시에)
3. SCiTail
과학 질문과 그에 대한 예상답변과 전제를 맞추는 데이터셋. 이 또한 domain-adaptation으로만 쓰였다.

## DATASETs
1. CoLA (Corpus of Linguistic Acceptability)
영어 문장이 언어학적으로 맞는지 아닌지 예측
2. SST-2 (Stanford Sentiment Treebank)
Sentiment of Sentence을 결정. 영화 리뷰에서의 인간의 반응(긍부정)
3. STS-B (Semantic Textual Similarity Benchmark)
뉴스 헤드라인, 비디오, 이미지 캡션에 대해 얼마나 유사한지를 1~5점사이의 값으로 데이터가 제공되어 있음.
4. QNLI (Qeustion-Answering NLI)
원래 이 데이터세트는 SQuAD의 (변형?)한 버전으로 Question에 Answer을 할 수 있을지 없는지를 맞추는 데이터인데 여기서는 Relevance ranking task로 사용하였다.
Paragraph-Question이 쌍이고 이 때, 답을 할 수 있냐 없냐를 맞추는 문제
5. QQP (Quora Question Pairs)
커뮤니티의 question-answering website Quora에서 추출한 데이터
두 개의 질문이 같은 의미를 갖는지 0, 1로 표시
6. MRPC (Microsoft Research Paraphrase Corpus)
QQP와 유사하고 온라인 뉴스로 부터 문장을 뽑았음.
Accuract와 F1측정 방법이 evaluation metric임
7. MNLI (Multi-Genre Natural Language Inference)
Large-scale로 crowd-source entailment classificatino task임.
P가 주어질 때, H가 Entailment, conradiction, neutral인지를 맞추는 거임.
8. RTE (Recognizing Textual Entailment)
Series of annual challenges on textual entailment의 데이터 모음?
MNLI와 비슷하나 entailment / not entailment 두 가지로 구분
9. WNLI (Winograd NLI)
Winograd Schema dataset으로부터 추출된 NLI 데이터 세트
대명사(pronoun)가 주어진 문장에서 선택목록의 대명사 대상을 선택하는 것
10. SNLI (Stanford NLI)
NLI 데이터세트에서 엄청 보편적으로 사용되는 것
이 논문에서 Domain adaptation을 위한 데이터세트로 사용
SciTail (Science entailment)
Science question Answering(SciQ)에서부터 추출된 entailment dataset
P가 H를 entail 하는지 맞추는 문제.
과학적인 문장들이기 때문에 challenge 한 듯
이 논문에서 Domain adaptation을 위한 데이터세트로 사용  
(출처: https://ai-information.blogspot.com/2019/02/multi-task-deep-neural-networks-for.html by rungjoo)

## GLUE Results
MT-DNN_fine_tuning은 fine tuning없이 task를 유추하는데 쓰인 모델
ST-DNN은 MTL없이 single task fine tuning을 진행한 모델
ST-DNN과 MT-DNN을 비교함을 통해서 MT-DNN, 즉 MTL이 높은 성능향상 효과를 가져다줌을 알 수 있다.
![image-10]({{site.baseurl}}/assets/images/2019-11-08-Multi-Task-Deep-Neural-Networks-For-Natural-Language-Understanding-10.png)
![image-11]({{site.baseurl}}/assets/images/2019-11-08-Multi-Task-Deep-Neural-Networks-For-Natural-Language-Understanding-11.png)

## Domain Adaptation Results on SNLI and SciTail
BERT, MT-DNN각각에 Training Data percentage을 0.1, 1, 10, 100으로 두어서 각 모델에 fine-tuning data크기가 얼마나 영향을 주는지 확인해본다. 실험결과 MT-DNN은 매우적은 데이터로도 큰 성능을 내며, 데이터가 커질수록 그 차이가 매워지나, MT-DNN이 여전이 성능이 좋음을 확인할 수 있다.
![image-12]({{site.baseurl}}/assets/images/2019-11-08-Multi-Task-Deep-Neural-Networks-For-Natural-Language-Understanding-12.png)

## Conclusion
BERT에 MTL을 이용해서 unsupervised learning을 이용한 BERT보다 generative하고, 성능을 향상 시켜주는 모델을 만든다.
labeled data가 많이 없을 때, MT-DNN을 통해 representation을 뽑으면 매우 좋은 성능을 가질 수 있다.
두개의 문장을 비교할 때는 SAN을 써보자!