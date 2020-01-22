---
layout: post
title: Population Based Training of Neural Networks(Hyperparameter Tuning)
category: DeepLearning
tags: [util]
---

Paper review on "Population Based Training of Neural Networks(2017-10-27)"

# Abstract
* PBT는 하이퍼파라미터를 찾는 동시에 모델을 학습시키는 방식으로 single fixed set을 찾는 방식과는 다르다.
* 기존 parallel search, sequential optimisation과 비교하여 성능, 속도에 우위를 가진다

# Introduction
* automatic selection of hyperparameters during training
* online model selection to maximise the use of computation spend on promising models
* the ability for online adaptation of hyperparameters to enable non-stationary training regimes and the discovery of complex hyperparameter schedules

# Related Work

## Sequential Optimisation
* hand tuning
* Bayesian optimisation(Gaussian Process)

## Parallel Search
* Grid Search
* random Search

## PBT
* 기존 Sequential Optimisation 방식에는 기존 데이터에서 prior을 얻고 거기서 이를 바탕으로 구한 posterior를 통해 새로운 parameter을 제안하는 형식으로 진행되어, 매 step마다 posterior를 update해야하는데오는 병목이 있다. 이 시간을 줄이기 위해 final performance를 다른 방식으로 예측하거나, 중간 값으로 예측하는 접근법이 제안된다.
* 이에 우리는 pretrained 된 파라미터를 가져오는 방식으로 need for sequential optimisation processes를 없애고자 한다.

![image-1]({{site.baseurl}}/assets/images/2019-11-18-Population Based Training of Neural Networks-1.png)
# Population Based Training
* PBT는 parameter theta와 hyperparameters h를 actual metric에 따라 optimise하는데 목적이있다.
![image-2]({{site.baseurl}}/assets/images/2019-11-18-Population Based Training of Neural Networks-2.png)
## ready
* 특정 조건이 만족된 이후부터 PBT를 실행하겠다는 함수. 예를 들어 최소 step 이후 또는 특정 loss, performance 이후에 exploit, explore를 하겠다는 것을 말한다.

## exploit
* 다른 population에서 특정 룰에 의해 현재 agent의 weight, hyper parameters를 대체할지 정하는 역할
* T-test Selection: 다른 population에서 uniform하게 값을 가져와 last 10 episodic rewards에 대해 welch't t-test를 진행, mean episodic rewards가 higher 하고 t-test를 통과할시 hyper parameter를 대체한다
* Truncation Selection: all agents의 episodic rewards를 순서대로 나열해서 만약 current agent가 하위 20%이면 상위 20%에서 랜덤으로 hyper parameter를 가져온다

## explore
* 대체 또는 기존 hyper parameters에 변형을 가하는 구간으로 기존 데이터를 통해서 resample을 하거나, 현재 hyper parameters에 0.8, 1.2와 같은 상수를 랜덤으로 곱하는 형식으로 진행된다.

## Question
* 그럼 처음 hyper parameter 초기화는 어떤식으로 진행하는것이 좋을까?