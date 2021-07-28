---
title: "11.chapter 6 Ensemble-Based and Hybrid Recommender Systems"
excerpt: ""

categories:
    - recommendation-systems
tags:
    - recommendation-systems
use_math: true
---

# 6장 앙상블 기반, 하이브리드 추천 시스템(6.6 ~ 6.10)

Ensemble-Based and Hybrid Recommender Systems

<img src="/assets/images/11chapter6/1.png" width="80%" height="80%" />

## 6.6 Feature Augmentation Hybrids

**방식** : staking 방식과 유사하다. 첫번째 level의 추천시스템이 특징들(a set of features)을 만들어서 두번째 level의 추천시스템의 입력값으로 사용한다. 한 추천시스템의 결과를 다음 추천시스템의 입력값으로 사용하는 방식이다.

예시 :

- Libra System, amazon 추천시스템과 베이즈 분류를 합친 시스템이다.
- Group Lens, Feature Augmentation 초기 예시. KB 방식으로 filterbots이 artificial rating을 만들고 그 데이터를 기반으로 협업필터링 시스템이 추천을 하는 시스템이다.

**특징** :

- Collaborative Recommender → Content-based

    : Libra를 예로 들면, Amazon 추천시스템이 협업필터링 시스템으로 항목을 표현할 수 있는 특징들(관련 작가, 관련 주제 등)을 사용해 추천을 만들어 콘텐츠 기반 추천 시스템의 입력값으로 넣는다. (앙상블 시스템으로 간주된다)

- Content-based → Collaborative Recommender

    : ratings 행렬에서 missing entries를 콘텐츠 기반 추천 시스템으로 채운다음 (새롭게 채워진 ratings들을 pseudo-ratings라고 한다.) 협업필터링 시스템이 dense ratings 행렬을 사용해 ratings을 예측한다. (이때 weight값들이 정해져야 하는데, 이 방식에서는 monolithic 시스템으로 간주된다.)

**비교** : cascade와 비교. cascade 방식은 boosting에 가까우며 추천시스템이 다른 추천시스템에 의해 추천된 아이템을 정제하는 방식이다. feature augmentation은 추천시스템에서 뽑은 특징들을 다음 추천시스템의 입력으로 다룬다. stacking과 유사하다.

**monolithic 방식**은 기존의 추천 시스템을 변형하거나 전체적으로 새로운 추천 방식으로 특징들을 합함으로써 생성되어 진다.

## 6.7 Meta-Level Hybrids

**방식** : 한 추천시스템에서 사용된 모델을 다른 추천시스템의 입력으로 넣는다. collaboration via content.

**예시** :

- Pazzani, 초기 collabortaion via content 연구를 했다.
- Labo Ur, instance-based 모델이 콘텐츠 기반의 사용자 profile을 학습하고, 그 profiles들이 협업 필터링을 이용해 비교된다.

**특징** : 현재 기존의 추천 시스템을 사용하지 않기 때문에 앙상블 시스템은 아니다.

<img src="/assets/images/11chapter6/2.png" width="80%" height="80%" />

첫 번째 단계, 콘텐츠 기반 시스템에서 각 사용자들마다 각 항목들이 갖고있는 대표적인 특성(discriminative feature)을 뽑는다. 그리고 관련 없는 단어들은 제거한다.(그 행렬이 위의 user-word 행렬)

두 번째 단계, 이 행렬은 더욱 밀도있어지고(denser) 사용자간 유사성을 강력하게 계산할 수 있다.

타겟 사용자와 유사한 그룹(peer group)을 찾기 위해선 user-word 행렬을 사용한다. 하지만 최종 추천은 rating 행렬을 사용한다. 이 부분이 두 단계에서 rating 행렬을 사용하는 협업필터링 방식과 다른점이다. 첫 단계에서 전처리 과정(특징을 선별하는)에서 기존의 콘텐츠 기반 모델만을 사용할 수 없다.

## 6.8 Feature Combination Hybrids

**방식** : 협업필터링 기반의 정보를 부가적인 특징으로 사용한 콘텐츠 기반 추천 시스템에 적용하기 전에, 다양한 sources들을 단일된 표현으로 통합한다. 협업 필터링 추천 시스템의 결과를 콘텐츠 기반 추천 시스템의 속성의 일부로 사용하는 방법이다.

**예시** : RIPPER 분류(repeated incremental pruning to produce error reduction)

- compressed matrix. 항목의 장르를 나타내는데 복잡한 분류(higher-level taxomony)가 되어있는 항목들이 있다고 하자. 사용자와 항목들이 장르를 기반으로 가지치듯이 증가할 수 있다. 이 관점으로 봤을 때, 아이템 수가 아니라 장르를 기준으로 구조화 될 수 있다. 항목의 수를 줄여야하고, 압축된 행렬이 되어야 더 효율적인 결과가 나오기 때문이다.
- augmented matrix. 항목과 관련된 keywords에 대해 columns들을 추가한 행렬이다. $m \times (n + d)$ 행렬이 되어 "키워드 항목"의 가중치는 사용자의 접근, 구매 또는 평가한 항목에 대한 표현의 가중치 합계에 기초한다. 그리고 다음과 같은 objective function이 나타난다. $\bar{\theta}$ vector값을 최적화하는 식이다.

  $J = CollaborativeObjective \left( \bar{\theta} \right) + \beta ContentObjective \left( \bar{\theta} \right) + Regularization $

**특징** :

- 아이템의 내재적인 정보를 이용함으로서 협업 필터링이 가진 평점을 매긴 사용자 수에 대한 민감성을 떨어트린다. 사용자 수가 많아도 합치면 되니까 괜찮다.

## 6.9 Mixed Hybrids

단일체(monolithic)나 앙상블 시스템으로 명확하게 분류되지 않는다.

**방식** : 추천된 아이템들을 구성하는 추천시스템이다. combination of presentation이 중요하다.

**예시** :

- TV 프로그램 구성 : 다른 추천시스템들로부터 추천된 아이템을 합해서 만든다.
- 여행 상품 구성 : 숙박, 레저스포츠, 항공권 등 각 카테고리마다 적합한 추천 방식이 있고 그에 따라 추천받은 항목들을 합한다. 각 카테고리끼리는 상호적으로 관련이 없다는 점이 중요하다. 여기서 제약 조건에 따라 사용자가 만족해야 하는데 이와 관련된 constraint satisfaction problem은 660,661 참조하면 된다.

## 6.10 Summary

Hybrids 시스템은 전반적으로 강력한 시스템을 만드는 장점을 가지고 있다. 앙상블 방법은 협업필터링 방식의 정확도를 개선하는 방법으로 사용된다. 여기서 제시된 다양한 방법들은 추천시스템(KB, CF, CB 등)의 편향을 줄이거나 다양성을 통합하기 위해 사용된다.
