---
layout: post
title: ERNIE:2.0 A Continual pre-traiing framework for language understanding
category: NLP
tags: [nlp]
---

Paper review on "ERNIE:2.0 A Continual pre-traiing framework for language understanding(2019-7-29)"

## Abstract
* pre training을 하면서 co-occurence외에 lexical, syntactic, semantic 정보를 찾을 수 있다.
* 이를 위한 continual pre-traiing frame work와 7가지 pre-training task을 제안한다.

## introduction
* continual pre training framework ERNIE 2.0을 제안한다. customizined training tasks가 가능하며 multi-task pre-training을 incremental하게 적용한다
* unsupervised language processing tasks 7가지를 제안하고, BERT와 XLNet에 비해 크게 English, Chinese 에 대해 성능이 향상됨을 보인다.
* [ERNIE](https://github.com/PaddlePaddle/ERNIE.)에 코드를 올려놓았다

## Related work

### Unsupervised Transfer LEarning for Language Representation
기존 objective들은 context independent word embedding을 제안해왔지만, 최근 논문들에선 contextualized language representation을 제안한다.
* ELMO: context-sensitive features
* GPT: context-sensitive by transformer
* BERT: masked language, next sentence prediction
* XLM: unsupervised method on monolingual data, supervised method leverages parallel bilingual data
* MT-DNN: multi task supervised task in before fine-tuning
* XLNet: autoregressive pre-training method bidirectional contexts by maximizing the expected likelihood over all permutations of the factorization order

### continual Learning
여러개 task를 차례대로 학습한다. 인간이 삶의 지식을 축적하는것 처럼

## The ERNIE 2.0 Framework

### Continual Pre training
- unsupervised pre training continually with big data and prior knowledge
- incrementally  update the ERNIE model via multi task learning
- sequence-level loss + token-level loss
![image-1]({{site.baseurl}}/assets/images/2019-11-18-ERNIE-2-A-Continual-Pre-Training-Framework-For-Language-Understanding-1.png)
![image-2]({{site.baseurl}}/assets/images/2019-11-18-ERNIE-2-A-Continual-Pre-Training-Framework-For-Language-Understanding-2.png)

#### kind of pre-training task
1. word-aware task
2. structure-aware task
3. semantic-aware task

## ERNIE 2.0 Model

### Model structure
1. Transformer Encoder same with BERT
2. Task Embedding: embedding of pre-training task. random select in fine tuning
![image-3]({{site.baseurl}}/assets/images/2019-11-18-ERNIE-2-A-Continual-Pre-Training-Framework-For-Language-Understanding-3.png)

### Pre-training Task

#### Word-aware Pre-training Tasks
1. Knowledge Masking Task
**phrase masking** and **named entity masking** for catch local contexts and global contexts

2. Capitalization Prediction Task
대문자는 중요한 의미를 담고있는 경우가 많다. cased model은 named entity recognition과 같은 task에서 좋은 성능을 보인다. 해당 문자가 대문자인지 아닌지 예측하는 task

3. Token-Document Relation Prediction Task
token이 original document에 있을지 없을지에 대한 여부를 판단. 해당 문서의 key words를 찾을 수 있는 능력을 갖게 된다.

#### Structure-aware Pre-training Tasks
1. Sentence Reordering Task
하나의 문서를 m개의 segments로 나누어서, random permuted order shuffle한 다음 원래 순서를 예측한다.

2. Sentence Distance Task
3 class classification. 0 는 두개의 문장이 하나의 문서에 인접해있다. 1는 두개의 문장이 하나의 문서에 있지만 인접해있지는 않다. 2 두개의 문장이 관련이 없다.

#### Semantic-aware Pre-training Tasks
1. Discourse Relation Task
문장간 semantic or rhetorical 관계를 파악한다. [mining discourse markers for unsupervised sentence representation learning](https://arxiv.org/abs/1903.11850)

2. IR Relevance Task
3 class classification task which predicts the relationship between a query and a title. Baidu Search Enging을 이용했다.
0는 강한 상관관계(query를 입력했을 때 title이 클릭된다.), 1은 약한 상관관계(query를 입력했을때 title이 검색되지만 클릭되진않는다), 2는 상관이 아예없는

## Experiments

### Pre-training and Implementation
1. Pre-training Data
Wikipedia and BookCorpus, Reddit, Discovery data for English
encyclopedia, news, dialogue, Baidu Search Engine for Chinese
![image-4]({{site.baseurl}}/assets/images/2019-11-18-ERNIE-2-A-Continual-Pre-Training-Framework-For-Language-Understanding-4.png)
2. Pre-training settings
same model settings of BERT. 
- BASE: 12 layers, 12 self-attention heads and 768 dimensional of hidden size
- LARGE: 24 layers, 16 self-attention heads and 1024 dimensional of hidden size
- Adam optimizer parameter B_1 = 0.9, B_2 = 0.98 with batch size of 393216 tokens
- learning rate 5e-e for english, 1.28e-4 for chinese
- scheduled by decay scheme noam with warmup over the first 4000 steps

### Implementation Details
SOTA for every GLUE, MRC tasks both of English and Chinese
![image-5]({{site.baseurl}}/assets/images/2019-11-18-ERNIE-2-A-Continual-Pre-Training-Framework-For-Language-Understanding-5.png)
![image-6]({{site.baseurl}}/assets/images/2019-11-18-ERNIE-2-A-Continual-Pre-Training-Framework-For-Language-Understanding-6.png)

## Conclusion
pre-training tasks를 incrementally하게 만들며 continual way로 multi-task learning을 지원하는 Continual Pre-training framework를 제안한다. BERT 와 XLNet에 비해 english, chinese에 대해 높은 성능향상을 보임.

