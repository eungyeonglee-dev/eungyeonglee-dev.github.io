---
title: "11.chapter 7 Evaluating Recommender Systems"
excerpt: ""

categories:
    - recommendation-systems
tags:
    - recommendation-systems
use_math: true
---

# 7 추천 시스템 평가 방법
Evaluating Recommender Systems

협업필터링의 평가 방법은 분류의 평가 방법과 유사한 점이 많다. 그 유사성이 분류나 회귀 모델의 관점에서 볼 수 있기 때문이다. 그래도 추천시스템만의 평가 방법들이 있으며 이를 알아보자.

- 온라인 방식

    현재 나타나는 추천에 대한 사용자의 반응으로 추천시스템이 평가되어진다. 사용자 참여가 필수적이다.

    특징 : 뉴스 추천 시스템. 사용자가 시스템이 추천한 뉴스 기사를 클릭하는 것으로 conversion rate를 측정할 수 있다. A|B 테스팅이라고도 불린다. 이익있는 항목에 대한 conversion rate를 증가하는 것이 추천시스템의 가장 중요한 목표가 된다. 그리고 이는 그 시스템의 효율성의 측정 지표가 된다.

    단점 : benchmarking, rresearch분야에서 사용하기엔 적합하지 않다. 많은 사용자의 참여가 필요한 시스템이기 때문이다.

- 오프라인 방식

    과거 데이터를 사용한다. 추천시스템을 평가하는 흔한 방법으로 사용된다. 이 장에서도 오프라인 방식에 초점하여 설명한다. 다음은 추천시스템 평가의 설계 관점에서 중요한 문제들이다.

    - Evaluation goals : novelty, trust, coverage, serendipity
    - Experimental design issues: 모델 설계와 정확도 평가에 같은 sef of specified ratings을 사용하면 매우 과대적합될 것이다. (overffiting) 이러한 맥락에서 experimental design은 중요하다.
    - Accuracy metrics : 평가에서 가장 중요한 요소이다. 추천시스템은 정확도 예측값, 항목을 랭킹하는 것의 정확도 등의 관점에서 평가될 수 있다.

## 7.2 Evaluation Paradigms

user studies, online evaluation은 사용자를 포함하고 있지만, 살짝 다른 방식으로 작동된다.

### 7.2.1 User Studies

Feedback, interact

방식 : 사용자의 feedback을 활용한다.

예시 : 사용자에게 추천된 아이템에 대해 like, dislike와 같은 feedback을 받을 수 있다.

장점 : 추천시스템과 사용자간의 상호 작용에 대한 정보 수집을 가능하게 한다.

단점 : 추천시스템이 사용자의 피트백을 바탕으로 수행된다는 사실을 사용자가 알게되면, 사용자의 선택과 행동에에 편향을 줄 수 있다. → 사용자의 평가가 완벽히 믿을 수 없게 될 가능성이 있다.

### 7.2.2 Online Evaluation

A|B 테스팅

1. A와 B 그룹으로 나눈다.
2. A그룹에는 a 추천시스템을, B그룹에는 b 추천시스템을 적용한다. 다른 조건은 동일하다.
3. conversation rate를 비교한다.

장점 :

- 다양한 추천시스템의 비교가능한 성능을 평가할 수 있다.

단점 :

- 가입된 사용자 수가 많아야 한다. → startup 단계에선 사용하기 힘들다.
- 데이터 접근이 공개적이지 않아 owner의 권한을 갖고 있는 사람만 접근가능 할 수도 있다. 그러므로, 이러한 테스트가 상업적인 개체에 의해 수행되면 제한된 수의 시나리오만이 그 시스템에서 다뤄질 수 있다. 시스템 독립적인 벤치마킹에서 일반화되기 어렵다.
- 추천시스템은 다양한 도메인과 환경에서 테스트 되어야 좋은 추천이 가능해지는데, 그렇지 못하다.

### 7.2.3 Offline Evaluation

추천시스템을 테스트하는데 가장 인기있는 방법이다.

방식 : historical data를 이용한다.

예시 :

- Netflix Prize data set

장점 :

- 많은 사용자 데이터에 접근할 필요가 없다. 한번 데이터가 모아지면, 다양한 추천시스템을 비교하기 위해 사용되어질 수 있다.
- 다양한 도메인에서 다양한 데이터가 추천시스템을 일반화하기위해 테스트에 사용되어질 수 있다.

## 7.3 General Goals of Evaluation Design

### 7.3.1 Accuracy

추천시스템의 가장 기본적인 평가 방법.

정확도에는 예측된 ratings와 실제 ratings 간의 차이인 entry-specific error $e_{uj} = \hat{r_{uj}}-r_{uj}$ 를 가지고 정확도를 평가하는 방법과 top-k개의 추천 항목의 rankings의 정확도를 평가하는 방법이 있다. 추천시스템은 ratings matrix를 예측하는 것보다 추천 아이템을 제시하는 경우가 더 일반적이다.

1. Designing the accuracy evaluation

: 모든 관측된 ratings 행렬은 트레이닝시나 정확도 평가시에 사용하지 않는다. 과적합될 가능성이 있기 때문이다. 다른 데이터 셋을 사용하는것이 중요하다. 그래서 $S$ 행렬에서 일부분 떼어낸 $E$ 행렬을 평가를 위해 사용한다.

1. Accuracy metrics:
    - Accuracy of estimating ratings

        $e_{uj} = \hat{r_{uj}}-r_{uj}$ 을 활용해서 구한다.

        $MSE = \frac{\sum_{(u,j)\in{E}}{e^2_{uj}}}{\|E\|}$

        $RMSE = \sqrt{\frac{\sum_{(u,j)\in{E}}{e^2_{uj}}}{\|E\|}}$

    - Accuracy of estimating rankings

### 7.3.2 Coverage

적용범위. 한 추천시스템이 매우 정확하더라도, 항목의 어떠한 부분에선 추천될 가능성이 없고, 특정한 사용자가 어떠한 부분에서 추천될 수 없는 경우가 생긴다. 시스템이 정확하면 추천 범위가 좁아지고, 추천 범위가 넓어지면 정확하지 않아지는 the trade-off between accuracy and coverage 는 항상 통합될 필요가 있다.

***예측할 수 없는 값에 default를 설정하여 100%의 정확도로 만들 수 있다.***
추천시스템이 한번이라도 추천한 아이템의 비율이다.

특징 :

- user-space coverage
- item-space coverage : 잘 발생하지 않는다.
    - 측정 방법: catalog coverage
        $CC = \frac{\|\cup_{u=1}^{m}T_u\|}{n}$
        아이템의 개수를 top-k개의 항목의 리스트로 나눈 값이다.

### 7.3.3 Confidence and Trust

- confidence(시스템의 신뢰도)

    신뢰구간이 같더라도 더 작은 confidence interval width를 가진 알고리즘이 정확하다. 같은 신뢰 구간을 가진 것들끼리만 비교할 수 있다.

- trust(사용자의 신뢰도)

    사용자가 평가한 평점의 신뢰도.추천 예측값이 정확할지라도 사용자가 올바르게 평가를 하지 않았다면 그 값은 유용하지 않다. 이는 usefulness와는 다르다. 예를 들어서 추천시스템이 사용자가 이미 알고 좋아하는 항목을 추천해준다면, 이의 usefulness는 적을지라도 사용자의 시스템에 대한 신뢰도는 높아질 수 있다.

    ***추천시스템에서 설명을 제공해줌으로써 trust 측정 가능. 왜 이거를 추천했는지. 이유를 제공해서 추천 시스템을 믿도록하는 방법.***

    측정 방법 : user surveys, online experiments(offline으로는 trust를 측정하기 어렵다)

### 7.3.4 Novelty

사용자에게 이전에 알지 못하고 보지 못한 항목을 추천해주는 것이다.

측정 방법 :

- online: online experimentation 사용자에게 추천항목에 대해 이전에 알고 있었는지 온라인으로 물어보는 것이다. 하지만 많은 online 사용자들을 기반하지 않는 시스템에서는 항상 좋은 방법은 아니다.
- offline: time-stamps novelty는 나중에도 사용자가 선택할만한 항목들을 추천한다. 따라서 time-stamps 방식으로 평가한다. (rating에 time-stamps가 존재한다면)이는 트레이닝 데이터에서 특정 시점$t_0$ 이후의 모든 rating을 지우는 방식이다.그리고 제거된 rating으로 트레이닝 시킨다. $t_0$이전의 평가되어 추천된 항목들에 대햇서 novelty 평가 점수를 낮추고 $t_0$이후의 항목에 대해선 점수를 높인다. 미래와 과거 사이에 다른 정확도를 측정한다.

### 7.3.5 Serendipity

의외성. lucky discoverry. conversation rate(전환율)에 장기적인 효과에 중요한 영양을 미친다.

비교 : novelty는 사용자가 이전에 알지 못한 아이템을 추천해주는 것이고, serendipity는 novelty보다 더 강한 조건이다. 모든 serendipitious한 항목은 novelty하지만 반대는 성립하지 않는다.

예시 :

인도 레스토랑에 간 사람 → 파시스칸 음식점 추천 (novelty)

인도 레스토랑에 간 사람 → 에티오피아 음식점 추천 (serendipity) : 뻔하지 않다!

측정 방법 :

- Online Methods: 사용자로부터 추천이 유용한지(usefulness), 뻔하지 않은지 (non-obvious)에 대해 feedback 받는다.
- Offline Methods: the fraction

### 7.3.6 Diversity

얼마나 다양하게 추천하는가. 만약 추천되는 3개의 영화가 특정한 장르에 비슷한 배우진들을 포함한다. 이 추천 영화리스트는 diversity가 적은 것이다.

특징 :

- diversity는 novelty와 serendipity와 관련 있다. novelty와 serendipity를 올리면 diversity도 함께 올라간다.
- novelty, serendipity, catalog → 양의 상관관계 accuracy → 음의 상관관계
- diversity가 높아지면 sales diversity와 시스템의 catalog 범위도 증가한다.

측정 방법 : content-centric similarity를 본다. 유사성이 낮아질수록 diversity가 높아진다.

### 7.3.7 Robustness and Stability

강력함과 안정성. 이는 attack model과 관련있으며 chapter 12에서 다룬다.

예시 : 저자나 출판사가 자신이 낸 책을 긍정적으로 fake rating하거나 라이벌 책을 부정적으로 fake rating하면 추천 시스템의 Robustness와 Stability가 낮아진다.

### 7.3.8 Scalability

big-data 패러다임이 나오면서 scalability의 중요성이 증가하고 있다. 최근에 많은 양의 rating 데이터 수집과 다양한 사용자로부터 feedback을 얻기가 쉬워지면서 데이터 셋의 크기도 증가하고 있다. 많은 양의 데이터를 이용해 효율적이고 효과적인 성능을 내는 추천시스템을 설계하는 것이 필수적이다. 따라서 데이터의 크기가 어느 정도인지를 측정하는 방법은 다음과 같다.

측정 방법 :

- Training Time
- Prediction Time
- Memory requirrements
