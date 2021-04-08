---
title: "2 Neighborhood-Based Collaborative Filtering"
excerpt: ""

categories:
    - recommendation-systems
tags:
    - recommendation-systems
use_math: true
---

# 2.1 Introduction
Neighborhood-based collaborative filtering algorithms은 memory-based algorithms으로 언급되기도 한다.
다음 두가지 방법이 있다.

1. User-based collaborative filtering :
추천 받고자 하는 대상 사용자 A와 유사한 (peer group) 사용자들이 각 항목에게 준 평가에 대한 가중치 평균값을 계산하여 평점을 예측한다.

2. Item-based collaborative filtering :
사용자 A가 평가를 준 항목들을 바탕으로 집합 S에서 추천할 대상 항목 B가 추천된다.
