---
layout: post
title: Character-Level Language Modeling with Deeper Self-Attention
category: NLP
tags: [nlp]
---

Paper review on "Character-Level Language Modeling with Deeper Self-Attention(2018-08-09)"

## Introduction
LSTM, RNN 의 성능은 long-term context를 기억하고 있다는데에 있다. 이번 논문에서는 fixed context deep(64-layer) transformer model에 character level language modeling을 적용해 문장예측에 높은 성능을 가짐을 보인다. deep layer을 이용하는만큼 speed convergence, additional regularizer기능을 제공해주는 auxiliary losses(Multiple Positions, Intermediate Layer Losses, Mulitple Targets)을 제안한다.  

## Character Transformer model
![image-1]({{site.baseurl}}/assets/images/character-level-language-modeling-with-deeper-self-attention-1.png)
language 모델은 다음과 같은 방식으로 예측 sequence probability을 예측하며, 64 transformer layers을 이용하며 각 포지션이 leftward하게 영향을 받기 위해 Figure 1 처럼 mask을 적용시킨다.
![image-2]({{site.baseurl}}/assets/images/character-level-language-modeling-with-deeper-self-attention-5.png)

## Auxiliary Losses
네트워크가 10 layers이상 깊어지면 slow convergence 와 poor accuracy문제가 발생하게 된다. 이에 본 논문에서는 다음과 같은 auxiliary losses을 제안한다. 이는 speed up convergence와 additional regularizer 역할을 한다.
각각의 auxiliary losses는 total loss of network에 더해지게 되며, 상대적으로 낮은 weight을 가지며. 각 losses마다 own schedule of decay 를 가진다.

### Mulitple Positions
![image-3]({{site.baseurl}}/assets/images/character-level-language-modeling-with-deeper-self-attention-2.png)
예측을 마지막 레이어 마지막 값에서 마지막 레이어 모든 값으로 바꾼다. 이는 때때로 하나 또는 두개의 단어로 다음 단어를 예측해야하는 상황을 만들지만, 실험적으로 이 loss정책은 트레이닝 속도를 올리고 더 좋은 결과를 가지게 해준다.

### Intermediate Layer Losses
![image-4]({{site.baseurl}}/assets/images/character-level-language-modeling-with-deeper-self-attention-3.png)
final layer을 제외하고 lower layers에도 predictions을 구하고 loss을 만드는 것을 말한다. lower layers는 상대적으로 낮은 contribute을 가지게 되며, l^th intermediate layer는 l/2n of training이후에 loss에 영향을 주지 않게 된다. 

### Multiple Target
![image-5]({{site.baseurl}}/assets/images/character-level-language-modeling-with-deeper-self-attention-4.png)
예측시에 하나의 값이 아닌 multiple한 값을 예측한다. extra targets 의 weight는 0.5라는 값을 곱해서 loss을 만들게 구현한다.

## Positional Embedding
64 layers는 매우 깊음으로 sinusodial timing singal을 input에서만 더하는 것으로는, propagation하면서 그 기능이 미미해 질 수 있다 판단했다.이에 본 논문에서는 learned positional embedding을 각 transformer layer input sequence에 더하게 된다. 이 때 layer 마다 positional embedding은 공유하지 않는다.

## Result
![image-6]({{site.baseurl}}/assets/images/character-level-language-modeling-with-deeper-self-attention-5.png)
character and accuracy of model is best on layer 64 tranformers. 그리고 적용시킨 auxiliary losses에 대해 ablation experiments을 한 결과. 모든 auxiliary losses을 고려했을 때 최상의 결과를 가져 왔다. 또한 같은 모델을 word모델에 적용시 PPL이 word level model이 더낮게 나왔는데 이는 추가 연구가 필요할 것 같다.

또한 분석결과 character level model이 prefer actual English words over non-existent words함을 확인했고, transformer self-attention구조가 copy sequences over long distances능력을 가져다줌을 예상한다.

## bpc란?
After all input has been read and encoded, the total length (in bits) of the result is measured and divided by the number of characters in the original, uncompressed input. If the model is good, it will have predicted the characters with high accuracy, so the bit sequence used for each character will have been short on average, hence the total bits per character will be low.

* [what-is-bpc] (https://stackoverflow.com/questions/17797922/how-to-calculate-bits-per-character-of-a-string-bpc)

## 참고문헌
* [Character-Level Language Modeling with Deeper Self-Attention](https://arxiv.org/abs/1803.02155)