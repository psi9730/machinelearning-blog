---
layout: post
title: Self-Attention with Relative Position Representations
category: NLP
tags: [nlp]
---

Paper review on "Self-Attention with Relative Position Representations(2018-03-06)"

## Introduction
transformer은 recurrent, convolutional neural network와 달리 모델 자체에서 order of sequence을 표현하지 못하는 단점이 존재한다. 이에 sinusoidal singal를 이용해서 absolute position을 input에 더해주는 식의 position embedding을 활용하게 되었다. 이번 논문에서는 이러한 position을 relative하게 representation하는 방법을 제안한다. self-attention mechanism에 relative position 또는 distance between sequence elements을 추가했으며 이 방식이 machine translation에서 performance을 증가시킴을 확인했다. 또한 학습 중에 추가한 relative position을 구하며, 모든 heads에 동일한 relative position을 적용함으로써 space complexity을 줄였다.

## Proposed Architecture
### Relation-aware Self-Attention
positional information을 담기 위해 input elements간에 pairwise relationships을 이용한다. x_i, x_j 사이의 edge의 종류를 a_i_j^V, a_i_j^K 두가지 종류로 둔다. 
![image-1]({{site.baseurl}}/assets/images/self-attention-with-relative-position-representations-1.png)
![image-2]({{site.baseurl}}/assets/images/self-attention-with-relative-position-representations-2.png)
3번 식은 주어진 attention head에 선택된 값이 다음 encoder or decoder layers에게 유용한가를 구할 때 유용하다.
4번 식은 attention head값을 변화시켜주는 값이다. 두가지 pairwise information을 더해줌으로써 additional linear transformations 없이 relation을 나타낼 수 있다.

### Relative Position Representations
![image-3]({{site.baseurl}}/assets/images/self-attention-with-relative-position-representations-3.png)
![image-4]({{site.baseurl}}/assets/images/self-attention-with-relative-position-representations-4.png)

Input elements간의 거리가 일정이상 커지면 relation position information이 크게 유용하지 않다는 가정하에 다음과 같은 clip구조를 제안한다. 이에 edge label은 2k+1 unique가 있으며, 이를 통해 sequence length에 대한 고려를 포함시킬 수 있다. 해당 논문에서는 k=64에서 최대 BLEU을 가지는 것이 확인된다.

### Efficient Implementation
![image-5]({{site.baseurl}}/assets/images/self-attention-with-relative-position-representations-5.png)
By relative position representations sharing across attention heads, storing relative position representations space complexity = O(h*d_a*n^2) to O(d_a*n^2). By relative position representations sharing across sequences, all space complexity = O(bhnd) to O(bhnd + d_a*n^2)
그리고 식 4번을 5번 식으로 바꿈으로써 parallel multiplcation이 가능하도록 한다.

## 참고문헌
* [Self-Attention with Relative Position Representations](https://arxiv.org/abs/1706.03762)


