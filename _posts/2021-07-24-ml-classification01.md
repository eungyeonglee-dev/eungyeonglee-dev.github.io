---
title: "분류 - MNIST"
excerpt: ""

categories:
    - machine-learning
tags:
    - machine-learning
    - homl
use_math: true
---

# MNIST set

고등학생과 미국 인구조사국 직원들이 손으로 쓴 70,000개의 작은 숫자 이미지를 모은 데이터 세트이다. 이 데이터 셋은 학습용으로 아주 많이 사용되며 새로운 분류 알고리즘이 나올 때 마다 MNIST 데이터셋에 얼마나 잘 작동하는지 본다.

<img src="/assets/images/ml-classification01/1.png"/>

70,000개의 이미지가 있고 각 이미지에는 784($28\times28$)개의 특성이 있다.
- 1개의 원소는 0~255 사이의 값을 가지며 이는 픽셀 강도이다.
- DESCR : 데이터셋을 설명한다.
- data : 샘플이 하나의 행, 특성이 하나의 열로 구성된 배열
- target : 레이블 배열

# 분류 성능 측정

사이킷런에서 메소드가
- '_score'로 끝난다면 결괏값이 *클수록 모형 성능이 좋다.*
- '_error','_loss'로 끝난다면 결과값이 *작을수록 모형 성능이 좋다.*

## 교차 검증을 사용한 정확도 측정

사이킷런에서 제공하는 기능보다 교차 검증 과정을 더 많이 제어해야 할 필요가 있다. 그때는 교차 검증 기능을 직접 구현하면 된다.

*StratifiedKFold*는 클래스별 비율이 유지되도록 폴드를 만들기 위해 계층적 샘플링을 수행한다. 매 반복에서 분류기를 복제하여 훈련폴드(train_folds)로 훈련시키고 테스트 폴더로 예측을 만든다. 그런 다음 올바른 예측의 수를 세어 정확한 예측의 비율을 출력한다.  

## 정확도

정확도 : 전체 데이터 중 정답으로 분류되는 비율이다.

불균형한 데이터셋을 다룰 때, 정확도는 분류기의 성능 측성 지표로 선호하지 않는다.(이진 분류에서는 정확도보다는 다른 분류 성능 평가 지표가 더 중요시되는 경우가 많다)

## 오차 행렬

클래스 A의 샘플이 클래스 B로 분류된 횟수를 센다.    
오차 행렬을 만들려면 실제 타깃과 비교할 수 있도록 먼저 *예측값*을 만들어야 한다.

#### *sklearn.metrics*

- precision_score    
- recall_score    
- confusion_matrix    
- f1_score    
- classification_report

사용법 : 함수(데이터의 라벨, 예측한 값)

### cross_val_predict()

k-겹 교차 검증을 수행하지만 평가 점수를 반환하지 않고 각 테스트 fold 에서 얻은 예측을 반환한다.     
모델이 훈련하는 동안 보지 못했던 데이터에 대한 예측을 얻을 수 있다.

## 정밀도와 재현율

<table width=50% >
<tr align=center>
<td colspan=4><이진 분류표></td>
</tr>
<tr align=center>
  <td colspan=2> </td>
  <td colspan=2>예측</td>
</tr>
<tr align=center>
  <td colspan=2></td>
  <td>양성</td>
  <td>음성</td>
</tr>
<tr align=center>
  <td rowspan=2>실제</td>
  <td>양성</td>
  <td>*정답*<br> True <br>Positive(TP)</td>
  <td>오답<br> False <br>Negative(FP)</td>
</tr>
<tr align=center>
  <td>음성</td>
  <td>오답<br> False <br>Positive(FP)</td>
  <td>*정답*<br> True <br>Negative(TN)</td>
</tr>
</table>

### 정밀도<sup>precision</sup>

$정밀도 = \frac{TP}{TP+FP}$

양성으로 예측했을 때, 실제 양성으로 나타나는 경우의 비율

### 재현율<sup>recall</sup>
$재현율 = \frac{TP}{TP+FN}$

민감율, 진짜 양성 비율(TPR)이라고도 한다.
실제로 양성에 해당하는 사람이 양성으로 예측되는 경우의 비율

### 특이도<sup>Specificity</sup>
$특이도 = \frac{TN}{FP+TN}$

실제로 음성에 해당하는 사람이 음성으로 예측되는 경우의 비율

### False Positive Rate(FPR)
$FPR = 1-특이도 = \frac{FP}{FP+TN}$

실제로 음성에 해당하는 사람이 양성으로 예측되는 경우의 비율

### F1 score
precision과 recall의 조화 평균값이다.
$F_1 = \frac{2}{ \frac{1}{recall} + \frac{1}{precision}} = 2 \times \frac{precision \times recall}{precision+recall}$
- 0~1 사이의 값을 가지며 1에 가까울수록 높은 성능을 나타낸다.

### 정밀도/재현율 트레이드오프

정밀도를 높이면 재현율이 낮아지고, 재현율을 높이면 정밀도가 낮아진다.  

## ROC 곡선<sup>Receiver operating characteristic</sup>

거짓 양성 비율에 대한 진짜 양성 비율이다.

- 재현율이 높을수록 분류기가 만드는 거짓 양성이 늘어난다.
- 곡선 아래의 면적(AUC)을 측정하면 분류기들을 비교할 수 있다.

# 다중 분류

둘 이상의 클래스를 구별한다.

[방법]
- OvR(One-versus-the-Rest) : 이진 분류기 사용       
    이진 분류기를 여러개 사용하여 이미지를 분류할 때 각 분류기의 결정 점수 중에서 가장 높은 것을 클래스로 선택한다.
- OvO(One-versus-One) : 각 숫자의 조합마다 이진 분류기를 훈련시킨다.       
    클래스가
  $ N $ 개라면 필요한 분류기의 개수는
  $ \frac{N\times\left(N+1\right)}{2} $
  개가 필요하다. 주요 장점은 각 분류기의 훈련에 전체 훈련 세트 중 구별할 두 클래스에 해당하는 샘플만 필요하다는 것이다.

(SVM 같은) 일부 알고리즘은 훈련 세트의 크기에 민감해서 큰 훈련 세트에서 몇개의 분류기를 훈련시키는 것 보다 작은 훈련 세트에서 많은 분류기를 훈련시키는 쪽이 빠르므로 OvO를 선호한다.

# 에러 분석

성능이 좋은 모델을 찾았다고 가정하고 이 모델의 성능을 향상 시킬 방법을 찾아보자.

- 에러의 종류 분석하기
    1. 오차행렬
        오차행렬을 분석하면 분류기의 성능 향상 방안에 대한 통찰을 얻을 수 있다.
    2. 그려보기
        시각적으로 한눈에 파악할 수 있다.

# 다중 레이블 분류

분류기 샘플마다 여러 개의 클래스를 출력해야 할 경우 생각해보자.

예시 ) 얼굴 인식 분류기. 같은 사진에 여러 사람이 등장하는 경우,(Alis, Bob) 여러 개의 이진 꼬리표를 출력 [1,0] (Alis 있음, Bob 없음) 하는 분류 시스템이 필요할 것.

## 평가 방법

적절한 지표는 프로젝트마다 다를 것이다.

각 레이블의 $F_1$ 점수를 구하고 간단하게 평균 점수를 계산한다. 그리고 각 레이블의 가중치도 생각해주어야 한다. 가중치를 포함하여 평가하는 간단한 방법은 클래스의 지지도<sup>support</sup>를 가중치로 주는 것이다.

# 다중 출력 분류

다중 레이블 분류에서 한 레이블이 다중 클래스가 될 수 있도록 일반화한 것이다.
