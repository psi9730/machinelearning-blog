---
layout: post
title: Generating Wikipedia By Summarizing Long Sequences
category: NLP
tags: [nlp]
---

Paper review about "Generating Wikipedia By Summarizing Long Sequences(2018-01-30)"

## Introduction
Multi Documents을 summarize하는 wiki를 만드는 것을 목적으로한다. WikiSum Dataset을 제작하였으며, Transformer Decoder Only architecture을 제안하였으며, memory usage에 이점을 가지는 DMCA(Transformer Decoder with memnory-compressed attention), memory compressed한 구조를 제안했다. 또한 LSTM, RNN이 아닌 Transformer가 long sequence을 처리함에 있어서 이점을 가진 점이 본 task에 장점을 가짐을 강조함.

## Summarization
* Extractive Summary: 원문 단어만을 이용해 Summary
* Abstractive Summary: 원문 외의 단어를 활용해 Summary
* Multi-document Summary(본 논문에서 채택한 summarization방법): 여러 document를 한번에 summary 한다.

## WikiSum Dataset
* cited sources: C_i 로 해당 위키가 cited 한 소스 article을 말한다.
* web search results: reference가 부족한 경우 web crawling을 통해 관련 자료를 모은다.

## Training Method
input reference documents의 길이가 매우 길기 때문에 end-to-end가 아니라 extractive summarization을 통해 중요시되는 paragraphs을 뽑는 과정, 해당 paragraphs를 통해 wiki을 generate하는 abstractive stage로 나누었다.

### Extractive Summarization
1. First L tokens
    첫번째 L tokens만 뽑는다.
2. TF-IDF
    TF-IDF를 통해 query's words의 Tf-idf summation을 통해 ranking을 나누어 paragraphs를 구한다.
3. TextRank
    Google에서 제시한 page rank구조에서 node를 paragraph, edge을 similarity measure로 해서 paragraphs rank을 구한다.
4. SumBasic
    해당 article에서 나오는 words의 빈도를 다 더해서 가장 높은 sentence을 구하는데, words in the highest sentence choosen 의 빈도는 제곱을 통해 낮게 한뒤 다음 highest sentence를 구한다.
5. Cheating
    using bigram union with ground truth text

### Abstractive Stage
![image-1]({{site.baseurl}}/assets/images/generating-wikipedia-by-summarizing-long-sequences-1.png)
title과 ranked paragraphs를 concat시키고 난 뒤에 pretrained model의 input에 맞게 tokenize시킨다. 이후에 model에 집어넣어 generated article을 구한다.

## Transformer Decoder
transformer의 decoder구조만 이용하기 위해 input -> output 구조에서 input을 input + separator token + output을 decoder에 넣는 구조로 바꾼다. 이를 통해 parameters 반을 줄일 수 있었으며, 이후 결과값을 보면 encoder + decoder구조보다 높은 성능을 보인다.( decoder에 output만을 예측하는 것이 아닌 input+ output을 함께넣음으로서 regularization기능을 하게되는건 아닐까?)
(x_1, ...., x_n) -> (y_1, ...., y_m) to (x_1, ..., x_n, $, y_1, ...., y_m)

## Transformer Decoder With Memory-Compressed Attention(T-DMCA)
* local-attention
    input을 블락단위로 나누어서 각각 attention을 구한뒤 merge시키는 구조.
* memory-compressed attention
    key, value에 1-d convolution을 적용함으로써, divide the number of activations by a
    compression factor 효과를 얻는다.
![image-2]({{site.baseurl}}/assets/images/generating-wikipedia-by-summarizing-long-sequences-2.png)

## Experiments
seq2seq- attention < transformer-Encoder-Decoder < Transformer-D = Transformer-DMCA 순으로 perplexity performance을 보인다.

## 추가지식
1. Tf-idf ? 
Idf = log(전체 문서수 / 단어t가 포함된 문서의 수)
Tf = 해당 문서에 특정 단어의 빈도
Tf-idf = Tf * Idf
특정 문서 내에서 단어 빈도가 높고, 전체 문서에서 그 단어가 포함된 문서가 적다면 이 값은 높아진다.

2. Perplexity? 
단어를 예측할 때 헷갈리는 정도. Perplexity=50이면 해당 단어를 예측할 때 50개의 선택지에서 고민이 됨을 뜻한다.

## 참고문헌
* [Generating Wikipedia By Summarizing Long Sequences](https://arxiv.org/pdf/1801.10198.pdf)