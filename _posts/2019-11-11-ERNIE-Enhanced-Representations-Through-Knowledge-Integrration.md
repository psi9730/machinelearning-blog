---
layout: post
title: ERNIE Enhanced Representaiton through Knowledge Integration
category: NLP
tags: [nlp]
---

Paper review on "ERNIE Enhanced Representaiton through Knowledge Integration(2019-4-19)"

## Abstract
* BERT의 마스킹 전략에 감명받아, entity-level masking, phrase-level masking을 통한 knowledge masking 전략을 제안한다.
* Entity-level masking은 특정 entities(names)들을 masking하는 것
* Phrase-level masking은 conceptual unit phrase을 masking하는 것
* 5가지 chinese natural language processing tasks(natural language inference, semantic similarity, named entity recognition, sentiment analysis, question answering)에서 state of the art result
* cloze test에도 높은 성능을 보인다

## Introduction
Language representaion pre-training은 natural language processing tasks에 많은 도움이됩니다.
Word2Vec, Glove에서 Elmo, GPT, BERT까지 word representation로 발전하고있다.
대다수의 모델들은 representations by predicting the missing word only through the contexts를 이용하는데, 본 논문에서는 context뿐만 아니라 prior knowledge 를 배워서 reliable language representation을 배우는 것을 목적으로 한다.

### contribution
1. Phrases, entity units masking을 통해 의미론적, 문법적 의미를 유추할 수 있다.
2. 5가지 Chinese natural language processing task에서 높은 성능을 보인다.
3. 완성된 ERNIE 코드를 공유하고있다.

## Related work

### Context-independent Representation
context와 상관없이 단어의 의미자체만을 담고 있는 Representation, 동음이의어는 같은 representation을 가진다.
* Word2Vec, Glove

### Context-aware Representations
context에 따라 의미, 느낌이 달라짐을 담고있는 representation
* Cove, ELMo, GPT, BERT, MT-DNN, GPT-2, XLM

### Heterogeneous Data
다양한 종류의 데이터를 이용했을 때 모델의 성능이 향상된다.

## Methods
### Transformer Encoder
* multi-layer Transformer
* using self attention, multi head, generating sequence of contextual embeddings
* CJK Unicode 양 옆에 space를 추가시켜줬다. WordPiece를 이용하여 tokenize시킴
* positional, segment embedding을 이용, [CLS]token이용

### Knowledge Integration
![image-1]({{site.baseurl}}/assets/images/2019-11-11-ERNIE-Enhanced-Representations-Through-Knowledge-Integrration-1.png)

#### Basic-Level Masking
BERT에서와 같은 random token masking 을 진행한다. high level semantic 정보를 얻을 수 없다

#### Phrase-Level Masking
words or characters 그룹을 하나의 unit으로 마스킹한다. 영어의 경우 lexical analysis, chunking tool을 이용하고, 다른 언어에 대해서는 그에 맞는 language dependent segmentation tool을 쓴다.
실제 적용할 떄는 Basic Level masking + Phrase Level masking을 동시에, 함께 쓴다.

#### Entity-Level Masking
Name Entity(사람, 위치, 단체, 상품)등을 masking한다.
![image-2]({{site.baseurl}}/assets/images/2019-11-11-ERNIE-Enhanced-Representations-Through-Knowledge-Integrration-2.png)

## Experiments
BERT와 모델사이즈가 같다. 12encoder layers, 768 hidden units and 12 attention heads.

### Heterogeneous Corpus Pre-training
![image-4]({{site.baseurl}}/assets/images/2019-11-11-ERNIE-Enhanced-Representations-Through-Knowledge-Integrration-4.png)

### DLM
Dialogue data를 이용하는데, 같은 replies는 question semantics가 비슷함에서 semantic representation에서 중요하다. 이에 segment embedding 대신에 Dialogue embedding(Q or R)을 이용하여 해당 Dialogue가 real or fake임을 예측하는 task을 목적으로 training시킨다. multi-turn conversations도 표현 가능하다.
![image-3]({{site.baseurl}}/assets/images/2019-11-11-ERNIE-Enhanced-Representations-Through-Knowledge-Integrration-3.png)

### Experiment Results
![image-5]({{site.baseurl}}/assets/images/2019-11-11-ERNIE-Enhanced-Representations-Through-Knowledge-Integrration-5.png)

### Ablation Studies
1. Effect of Knowledge masking strategies
phrase, entity level masking strategy 모두에서 성능이 향상됨을 보인다.
2. Effect of DLM
DLM task에서 0.7%/1.0% 정도의 성능향상을 보임.
3. Cloze Test
BERT와 ERNIE를 비교해놓았는데 BERT는 문장 내에 비슷한 단어를 복사하는 예측, 즉 entity type만을 예측 하는데 반면, ERNIE는 prior knowledge를 기반으로 정확한 entity 값을 예측하는 것을 볼 수 있다.
![image-6]({{site.baseurl}}/assets/images/2019-11-11-ERNIE-Enhanced-Representations-Through-Knowledge-Integrration-6.png)
