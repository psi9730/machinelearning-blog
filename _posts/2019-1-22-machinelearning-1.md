---
layout: post
title: Univeral Sentence Encoder
category: machine-learning
tags: [statistic]
---

study on machine-learning

# 확률 이론에서 독립의 의미란?
두 사건 A와 B에서 한 사건의 결과가 다른 사건에 영향을 주지않을 때 A와 B를 독립이라고 한다
P(A|B)=P(A)

# 엔트로피란

## 정보량
-log(p)로써 하나의 사건이 일어날 때 이를 표현할 정보량이라 말할 수 있다. 허프만 코드 방식처럼 하나의 사건을 설명하는데 필요한 코드의 양이라고 생각하면 이해가 쉬운것 같다. 정보량은 해당 사건이 일어날 확률이 작을수록 커지게 된다.

## 엔트로피
정보량의 기댓값으로 E[-log(p)]이다. 어떤 사건을 기술하기 위해 필요한 코드 길이의 평균.

## cross entropy

## 이항분포(binomial distribution)
성공확률이 p인 베르누이 시행이 n번 반복되었을때, 성공횟수를 확률변수 X라고 할때, X를 이항확률변수라고한다.
X에 대한 확률질량함수는 다음과 같다.
![binomial distribution]({{site.baseurl}}/assets/images/2020-01-22-machinelearning.png)
E[X]=np, Var[X]=np(1-p)로 나타낼 수 있다.

## 포아송분포(poisson distribution)
정해진 시간 안에 어떤 사건이 일어날 횟수에 대한 기댓값 lambda라고 했을 때, 그 사건이 n회 일어날 확률은 다음과 같다.
![poisson distribution]({{site.baseurl}}/assets/images/2020-01-22-machinelearning-1.png)

## 지수분포(exponential distribution)
사건이 서로 독립적일 때, 일정 시간동안 발생하는 사건의 횟수가 푸아송 분포를 따른다면, 다음 사건이 일어날 때까지 대기 시간은 지수분포를 따른다
![exponential distribution]({{site.baseurl}}/assets/images/2020-01-22-machinelearning-2.png)
E(X)=1/lambda
Var(X)=1/lambda^2

## 지수족 분포
확률밀도함수가 
f(x|theta) = h(x)g(theta)exp(c(theta)T(x)-A(theta))를 만족하는함수
theta=모수. T(x)는 충분통계량의 역할을 한다

## 충분통계량(sufficient statistic)
P(X|Y)가 theta에 의존하지않을때, 모수를 표현하기에 충분하다.
“통계량 T를 아는 통계학자가 모든 랜덤 샘플을 가지고 있는 통계학자만큼 미지의 모수의 파라메터를 잘 추정할 수 있다면, 통계량 T는 충분 통계량이다.”
[sufficient statistic](http://contents.kocw.net/KOCW/document/2015/chungbuk/najonghwa/5.pdf)

## overfit, underfit
- overfit
모형이 크고, 데이터가 많이 없을때 잘일어남. 일반화 오류 커지고, 학습오류 작아짐

- underfit
모형이 작고, 데이터가 많을때 잘일어남. 학습오류가 큰 상황

## Logistic regression
확률 모델로서 독립 변수의 선형 결합을 이용하여 사건의 발생 가능성을 예측하는데 사용되는 통계 기법이다.

### complete seperation
separation occurs when the data set is too small to observe events with low probabilities.
solution
1. obtain more data
2. consider alternative model which contain more variable
3. manipulate data in group

## Mixture Densitiy Networks
하나의 입력이 주어졌을 때 여러개의 결과를 만들어낼 수 있다.
![Mixture Densitiy Networks]({{site.baseurl}}/assets/images/2020-01-22-machinelearning-3.png)

## kernel function
K(x,y)=M(x)*M(y)를 만족하는 맵핑함수가 존재하는 함수

### mapping function
특정 차원의 데이터를 특정차원의 데이터로 맵핑시키는 함수

### kernel trick
SVM에서 더 좋은 선형분류를 위해서 고차원 공간의 두벡터의 거리를 구해야하는데
고차원 공간으로 모든 데이터를 맵핑함수를 통해 맵핑한다음 거리를 구하는 것이 아닌 kernel함수를 이용하여 저차원공간에서 거리를 구할 수 있다.