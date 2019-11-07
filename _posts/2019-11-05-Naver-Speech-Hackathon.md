---
layout: post
title: NAVER SPEECH HACKATHON 2019
category: STT
tags: [stt]
---

Experience attending "NAVER AI HACKATHON SPEECH(2019-09-17~ 2019-10-13)"

## 참가 후기
첫번째 AI해커톤인만큼 설레고 재밌는 경험이었다. 처음엔 STT에 분야에 대한 사전 지식이 없어서, 음성을 데이터화 시키는 mel spectogram 부터 이를 처리하는 다양한 모델들을 [listen and spell](https://arxiv.org/abs/1508.01211), [attention is all you need](https://arxiv.org/abs/1706.03762), [deep speech 2](http://proceedings.mlr.press/v48/amodei16.pdf), [RNN Transducer](https://arxiv.org/pdf/1211.3711.pdf)을 차근차근 공부했고, 그중 구조화가 쉬우며 간단한 음성 input에 대해 performance가 좋은 seq2seq 모델을 선택했다.

논문을 읽는 것과 실제로 코드를 구현하는것은 매우 재밌으면서 힘들다는 것을 느꼈고, 특히 성능를 올리는 방법을 고민할 때는 지식의 부족함을 크게 느꼈다. 오프라인 당일 날에는 24시간 시간이 주어졌지만 트레이닝 시간만 18시간 넘게 들어가는 상황에서 크게 성능을 향상 시킬수 없을 것이라 예상했다. 하지만 hackathon 당일날 페어코딩으로 상당한 집중력을 발휘 할 수 있었고, 튜터님들의 조언과 함께 성능을 대폭 상승시키는 행복한 경험을 할 수 있었다. 당일날에는 트레이닝 된 모델을 활용하는 beam search, ensemble에 초점을 맞춰서 코딩을 진행했다.

끝까지 최선을 다했고, 9등이라는 아쉽지만 뜻깊었던 결과를 맞이할 수 있었다. 이후 1,2,3위 팀의 질의응답시간에는 구현 방식에 대한 많은 질문을 했었다. 대회를 진행하면서 계속 성능의 차이가 모델의 구조에서 크게 올것이라 생각했는데 data preprocessing, weight initialization에서 놓친 부분이 많았었다는 사실이 가장 기억에 남는다. 어떤 작업이든 기본 실력을 쌓는 것이 가장 중요하다라는 것을 크게 느꼈다.

![HAPPY HACKATHON!]({{site.baseurl}}/assets/images/hackathon-image.jpeg)

## 참고자료
[행복코딩's HACKATHON GITHUB REPO](https://github.com/elzino/naver_ai_hackathon_speech)