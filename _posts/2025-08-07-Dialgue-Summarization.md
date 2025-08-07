---
layout: single
title: "Dialgue-Summarization 일상 대회 요약 경진대회"
categories: 경진대회
tag: [경진대회,패스트캠퍼스,패스트캠퍼스AI부트캠프,업스테이지패스트캠퍼스,UpstageAILab,국비지원,패스트캠퍼스업스테이지에이아이랩,패스트캠퍼스업스테이지부트캠프]
use_math: true
---



## Description
Dialogue Summarization 경진대회는 주어진 데이터를 활용하여 일상 대화에 대한 요약을 효과적으로 생성하는 모델을 개발하는 대회입니다. 

일상생활에서 대화는 항상 이루어지고 있습니다. 회의나 토의는 물론이고, 사소한 일상 대화 중에도 서로 다양한 주제와 입장들을 주고 받습니다. 나누는 대화를 녹음해두더라도 대화 전체를 항상 다시 들을 수는 없기 때문에 요약이 필요하고, 이를 위한 통화 비서와 같은 서비스들도 등장하고 있습니다.

그러나 하나의 대화에서도 관점, 주제별로 정리하면 수 많은 요약을 만들 수 있습니다. 대화를 하는 도중에 이를 요약하게 되면 대화에 집중할 수 없으며, 대화 이후에 기억에 의존해 요약하게 되면 오해나 누락이 추가되어 주관이 많이 개입되게 됩니다.

이를 돕기 위해, 우리는 이번 대회에서 일상 대화를 바탕으로 요약문을 생성하는 모델을 구축합니다!
참가자들은 대회에서 제공된 데이터셋을 기반으로 모델을 학습하고, 대화의 요약문을 생성하는데 중점을 둡니다. 이를 위해 다양한 구조의 자연어 모델을 구축할 수 있습니다.

제공되는 데이터셋은 오직 "대화문과 요약문"입니다. 회의, 일상 대화 등 다양한 주제를 가진 대화문과, 이에 대한 요약문을 포함하고 있습니다.

참가자들은 이러한 비정형 텍스트 데이터를 고려하여 모델을 훈련하고, 요약문의 생성 성능을 높이기 위한 최적의 방법을 찾아야 합니다.

경진대회의 목표는 정확하고 일반화된 모델을 개발하여 요약문을 생성하는 것입니다. 나누는 많은 대화에서 핵심적인 부분만 모델이 요약해주니, 업무 효율은 물론이고 관계도 개선될 수 있습니다. 또한, 참가자들은 모델의 성능을 평가하고 대화문과 요약문의 관계를 심층적으로 이해함으로써 자연어 딥러닝 모델링 분야에서의 실전 경험을 쌓을 수 있습니다.

본 대회는 결과물 csv 확장자 파일을 제출하게 됩니다.

input : 249개의 대화문

output : 249개의 대화 요약문

## 배경 
- 인턴쉽 취업 준비로 경진대회에 집중 하지 못하기 때문에 최대한 여러 실험과 <br>
인턴쉽 준비를동시에 진행 할 수 있게 처리 시간이 많은 방법 위주로 실험을 할것

## 점수
- Rouge1
- Rouge2
- RougeL
- Rougef1
## 전략
- Bart model 기준으로 다영한 모델 fin-training
- 다양한 파라미터 상호 작용
- 위의 결과가 미미 했을때 프롬프트로 데이터 생성

## Model 선택
- Frist baseline(경진대회에 제공된 베이스 코드) socre 46.5930
- baseline ? 47.6004
- hyunwoongko/kobart 영화리뷰 감성 학습 47.9140
- EbanLee/kobart-summary-v3 한국어 도서 요약 47.18
- gogamza/kobart-summarization 한국 뉴스 요약 47.2133
- ainize/kobart-news 한국 뉴스 요약 45.2095

## Fine tuning
- decoder max legth 512 last : 47.4883
- A output : topic + summary last : 46.7900
- B ouput : topic → test → topic(predict)
    - input : dialogue + topic bast : 47.6004
- weight_decay0.05 bast : 47.8631
- early_stopping20 last : <span style="color:violet">47.9140</span>
- decoder max legth 512 + early_stopping20 + weight_decay0.05 + B : 47.0271

## Model & Fine tuning 결과 해석
    다양한 실험으로 동일한 조건에도 Rougef1 점수가 일정하지 않고 46~47이내로 정체되었다
    경진대회 데이터에 val,train 데이터를 성명하지 못하는 것을로 판단하고 EDA 진행

## EDA
- topic 불균형 확인

![](/assets/images/2025-08-07/topic_count.png)

- topic 히스토그램

![](/assets/images/2025-08-07/topic_histogram.png)