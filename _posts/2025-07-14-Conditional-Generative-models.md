---
layout: single
title: "생성 모델 평가 지표 : 조선부 생성 모델 Conditional Generative Models"
categories: Generative-Ai
tag: [Generative-Ai,패스트캠퍼스,패스트캠퍼스AI부트캠프,업스테이지패스트캠퍼스,UpstageAILab,국비지원,패스트캠퍼스업스테이지에이아이랩,패스트캠퍼스업스테이지부트캠프]
use_math: true
---

## 조건부 생성 모델 (Conditional Generative Models)

### 일반 생성 모델과의 차이

* **일반 생성 모델**
![](/assets/images/2025-07-14/normal.png)
  * 데이터 전체의 분포 $p(X)$만 학습.
  * 생성 결과를 제어할 수 없으며, 무작위로 결과가 생성됨.

* **조건부 생성 모델**
![](/assets/images/2025-07-14/conditional.png)
  * 특정 조건 $Y$가 주어졌을 때의 분포 $p(X|Y)$를 학습.
  * 원하는 조건을 명시하여 특정 데이터를 생성 가능.
  * 예시: 특정 숫자(0\~9)를 조건으로, 해당 숫자 이미지를 생성.

---

## 조건부 생성 모델 평가 지표
![](/assets/images/2025-07-14/confitional_alleviate.png)
조건부 생성 모델은 기존의 생성 모델 평가 지표(IS, FID 등)로는 조건을 제대로 반영하는지 평가할 수 없기 때문에, 조건을 반영한 지표가 필요함.

### 1. **Intra FID**
![](/assets/images/2025-07-14/Intra_FID1.png)
* 클래스별로 FID를 계산한 후 평균.
* 특정 클래스 내의 실제 데이터와 생성된 데이터 간의 분포 차이를 평가.
* 예시로 '악기 하프' 클래스에서 생성 품질이 낮으면 매우 높은 FID 값으로 평가됨.

### 2. **사전 훈련된 분류기의 정확도**
![](/assets/images/2025-07-14/prior_train.png)
* 사전 훈련된 분류기를 활용하여 생성된 데이터가 조건(클래스)을 얼마나 잘 반영하는지 평가.
* 단점\
![](/assets/images/2025-07-14/prior_train_bad1.png)
  * 분류기 성능에 의존적.
  * 분류 경계만 만족하면 높은 점수를 얻을 수 있어, 다양성 부족 문제 발생 가능.

### 3. **CAS (Classification Accuracy Score)**
![](/assets/images/2025-07-14/CAS1.png)
* 생성 모델이 만든 데이터를 이용해 분류기를 학습하고, 실제 테스트셋에서 정확도를 측정.
* 생성 데이터의 품질과 다양성을 더 잘 평가할 수 있으나, 평가 시 분류기를 반복적으로 학습해야 하는 부담 존재.

### 4. **LPIPS (Learned Perceptual Image Patch Similarity)**
* 이미지 간 유사도를 사전 훈련된 분류기의 feature space에서 측정.
![](/assets/images/2025-07-14/LPIPS1.png)
* 원본과 생성 이미지 간 유사도가 낮으면, 다양성이 높은 것으로 평가됨.
![](/assets/images/2025-07-14/LPIPS_alleviate.png)
### 5. **CLIP Score**
![](/assets/images/2025-07-14/CLIP.png)
* 텍스트와 이미지의 관계를 평가하는 지표.
* 텍스트 인코더와 이미지 인코더를 이용하여 두 모달리티 간의 유사도를 측정하는 CLIP 모델을 활용.
* 생성된 이미지가 입력 텍스트 조건을 얼마나 잘 반영했는지를 평가.

---

## 정리 및 핵심 평가 지표 요약

* **Intra FID**: 클래스별 데이터 분포 일치도 평가.
* **Classification Accuracy / CAS**: 분류 정확도를 활용한 조건 충족 평가.
* **LPIPS**: 이미지 간 feature 기반 유사도로 다양성 평가.
* **CLIP Score**: 텍스트-이미지 조건 일치도 평가.

조건부 생성 모델 평가는 생성의 품질과 조건 충족도, 다양성 등을 함께 고려해야 하며 목적에 맞는 평가 지표를 선택적으로 활용 해야한다.
