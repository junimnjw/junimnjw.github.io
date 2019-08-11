---
layout: post
title: "Tensorflow Lite 모델 만들기(1)"
date: 2019-08-07
lastmod : 2019-08-11 09:16:00
sitemap :
changefreq : hourly
priority : 1.0
---

안녕하세요. 코딩벅스입니다. 

 **텐서플로우 라이트(Tensorflow Lite)**는 텐서플로우의 경량화 버전입니다. 스마트 폰 이나 태블릿 같은 임베디드 시스템은 하드웨어 성능이 비교적 열세하기 때문에, 서버나 PC같은 고성능 장치에서 학습을 진행하고, 여기서 생성한 모델을 텐서플로우 라이트 포맷(*.tflite)으로 변환 후, 이를 활용합니다. 

위 내용에 대한 설명은 다음 세 단계로 연재하려고 합니다. 

(1) 텐서플로우 모델(**SavedModel 또는 GraphDef**) 생성하기

(2) 텐서플로우 라이트 모델(**.tflite**)로 변환하기

(3) 안드로이드 앱에 적용

 첫번째 순서로 **(1) 텐서플로우 모델 생성하기**를 다루고자 합니다. 생성해야 할 텐서플로우 모델의 형태는 **SavedModel** 또는 **GraphDef**입니다. 텐서플로우 학습 모델은 **그래프**로 표현됩니다. 결국, 이 그래프의 노드와 에지 정보가 해당 모델을 일부 표현한다고 볼 수 있죠. **GraphDef**는 그래프에 대한 정보, 즉 노드와 노드사이의 엣지의 연결정보들을 가지고 있습니다. 다만, Weight와 Variable에 대한 정보는 별도의 파일로 가지게 됩니다. **SavedModel**은 그래프정보와 Variable정보를 망라한 모델 전체에 대한 정보를 가지고 있습니다. 

#### CheckPoint Vs. SavedModel

텐서플로우로 학습한 진행할 때 현재 진행중인 학습 모델에 대한 백업(back-up)은 보통 **Checkpoint(.ckpt)**형태로 이루어집니다. 그리고 이를 가지고 추후 복원(Restore)해서 다시 학습을 재개합니다. 다만, **Checkpoint(.ckpt)**는 저장 시점에서의 Variable 정보만을 가지고 있기 때문에, 학습을 재개하려면 기존 Checkpoint를 복원 하기 전, ***학습 모델의 그래프***를 다시 정의해주어야 합니다. 

 이와는 달리 **SavedModel**은 Variables 뿐 아니라 학습 모델의 그래프 정보를 같이 포함하고 있습니다. 사실상, 학습 모델에 대한 모든 정보를 가지고 있기 때문에, 모델 자체를 온전히 배포하는데 주로 활용됩니다. 텐서플로우에서 **SavedModel**에 대한 자세한 설명은 차후 별도로 다루겠습니다. **SavedModel**을 배포하는 간단한 예제를 다음 링크에서 공유합니다. 

[SavedModel 파이썬 생성예제 - main.py](/assets/main.py)

!["생성결과"](https://github.com/junimnjw/junimnjw.github.io/blob/master/assets/img/savedmodel_captured.JPG?raw=true)

