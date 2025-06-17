---
layout: single
title: "Forward & Back propagation"
categories: Deep-learning
tag: [Deep-learning,MLOps]
use_math: true
---


# ✨ 딥러닝 순전파와 역전파를 직접 구현해보며 이해한 과정


### 1. 개요

딥러닝이 어떻게 학습하며 손실(Loss)을 줄여가는지 알고 싶어, 직접 순전파(forward)와 역전파(backpropagation) 과정을 **코드로 구현**해 보았습니다. 단순한 신경망 구조를 예제로 사용하고, 각 가중치의 변화가 손실에 어떤 영향을 주는지 시각적으로 확인할 수 있었습니다.

---

### 2. 순전파 과정 (Forward Propagation)

아래 그림은 순전파를 통해 입력이 출력으로 전파되는 과정을 보여줍니다.

![순전파 구조](/assets/images/2025-06-17/미분1.png)
※ 첫 번째 그림: 입력 → 은닉층 → 출력 → 손실 계산 과정 시각화

#### 🔢 입력값

```python
x1 = np.array([0.5, 0.7, 0.2])
x2 = np.array([0.9, 0.3, 0.7])
y  = np.array([0.2, 0.9, 0.3])
```

#### 🧮 순전파 계산 코드

```python
h1 = ((x1 * w1) + (x2 * w3)) + b1
h2 = ((x1 * w2) + (x2 * w4)) + b1
h3 = ((h1 * w5) + (h2 * w6)) + b2
predictions = (h3 * w7) + b3
loss = (predictions - y) ** 2
loss_mean = np.mean(loss)
```

---

### 3. 역전파 과정 (Backpropagation)

역전파를 통해 각 가중치가 손실에 얼마나 영향을 주었는지 계산합니다. **체인 룰(연쇄 법칙)** 을 통해 최종 손실에 대한 각 가중치의 변화량(미분값)을 계산할 수 있습니다.

#### 🧮 역전파 미분 공식

```python
dL_dpred = (2 / n) * (predictions - y)
# 이후 체인 룰로 각 가중치에 대한 변화량 계산
```

#### 🧮 가중치 업데이트

```python
    w1 -= learning_rate * dL_dw1
    w2 -= learning_rate * dL_dw2
    w3 -= learning_rate * dL_dw3
    w4 -= learning_rate * dL_dw4
    w5 -= learning_rate * dL_dw5
    w6 -= learning_rate * dL_dw6
    w7 -= learning_rate * dL_dw7
    b1 -= learning_rate * dL_db1
    b2 -= learning_rate * dL_db2
    b3 -= learning_rate * dL_db3
```

---

### 4. 가중치 업데이트 이후 결과

아래 그림은 한 번의 역전파 후 가중치가 업데이트되고, 예측값이 실제값에 더 가까워졌음을 보여줍니다.

![가중치 업데이트 후 결과](/assets/images/2025-06-17/미분2.png)

---

### 5. 느낀점 및 정리

* 딥러닝의 학습 과정은 **결과(손실)와 입력 사이의 관계를 미분**하여, 각 가중치를 조절하는 과정이다.
* 순전파 → 손실 계산 → 역전파(체인 룰) → 가중치 업데이트의 반복을 통해 모델이 점점 더 나은 예측을 하게 된다.
* 이 과정을 직접 구현해보니, 딥러닝이 **수학적으로 얼마나 정교하고 직관적인 원리로 작동하는지** 체감할 수 있었다.

---

### 📎 전체 코드

> 아래는 전체 학습 코드를 첨부합니다.

<details>
<summary>코드 보기</summary>

```python
x1 = np.array([0.5, 0.7, 0.2])
x2 = np.array([0.9, 0.3, 0.7])
y = np.array([0.2, 0.9, 0.3])

w1,w2,w3 = 0.5,0.1,0.2
w4,w5,w6,w7 =0.7,0.6,0.5,0.4
b1,b2,b3 = 0.8,0.4,0.3

n = len(y)
learning_rate = 0.01

for epoch in range(10):
    h1 = ((x1 * w1)+(x2 * w3)) + b1
    h2 = ((x1 * w2)+(x2 * w4)) + b1
    h3 = ((h1 * w5)+(h2 * w6)) + b2
    predictions = (h3 * w7) + b3
    loss = (predictions - y) ** 2
    loss_mean = np.mean(loss)
    print(loss_mean)

    dL_dpred = (2 / n) * (predictions - y)
    dL_dw7 = np.sum(dL_dpred * h3)
    dL_db3 = np.sum(dL_dpred)
    dL_dh3 = dL_dpred * w7
    dL_dw5 = np.sum(dL_dh3 * h1)
    dL_dw6 = np.sum(dL_dh3 * h2)
    dL_db2 = np.sum(dL_dh3)
    dL_dh1 = dL_dh3 * w5
    dL_dh2 = dL_dh3 * w6
    dL_dw4 = np.sum(dL_dh2 * x2)
    dL_dw3 = np.sum(dL_dh1 * x2)
    dL_dw2 = np.sum(dL_dh2 * x1)
    dL_dw1 = np.sum(dL_dh1 * x1)
    dL_db1 = np.sum(dL_dh1 + dL_dh2)

    w1 -= learning_rate * dL_dw1
    w2 -= learning_rate * dL_dw2
    w3 -= learning_rate * dL_dw3
    w4 -= learning_rate * dL_dw4
    w5 -= learning_rate * dL_dw5
    w6 -= learning_rate * dL_dw6
    w7 -= learning_rate * dL_dw7
    b1 -= learning_rate * dL_db1
    b2 -= learning_rate * dL_db2
    b3 -= learning_rate * dL_db3
```

</details>

---

### ✍️ 마무리

처음에는 복잡하게만 느껴졌던 딥러닝의 학습 원리가, 수학과 코드를 통해 차근차근 따라가보니 구조가 명확히 보였습니다. 특히 **역전파는 손실이 얼마나 줄어들었는지를 수학적으로 추적하고 가중치를 조정하는 핵심 메커니즘**임을 몸소 느낄 수 있었습니다.

---